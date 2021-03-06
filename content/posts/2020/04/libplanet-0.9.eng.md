---
title: Libplanet 0.9 Released
date: 2020-04-28
authors: [hong.minhee]
translators: [kidon.seo]
---

Hello Everyone! We have released the ninth minor version of [Libplanet], [Version 0.9][0.9.0].

Libplanet is a common library that solves game implementation problems such as P2P communication and data synchronization when creating online multiplayer games that run on distributed P2P.

From this version, Libplanet will be distributed in several NuGet packages. This is because Libplanet's externalities have expanded, making it too heavy to include some of the dependent libraries across all applications.

This piece introduces the new NuGet packages and addresses key major changes in Version 0.9.


[Libplanet]: https://libplanet.io/
[0.9.0]: https://github.com/planetarium/libplanet/releases/tag/0.9.0


[Libplanet.RocksDBStore]
------------------------

A newly added Nuget package, Libplanet.RocksDBStore includes `RocksDBStore` class, which is Lipblanet's [`IStore` interface][IStore] implemented as [RocksDB] backend. Through internal testing, `RocksDBStore` has shown to be about 10 times faster to write, 2 times faster to read, and thanks to compression, takes 15% less storage space than [`DefaultStore`][DefaultStore].

Despite the advantages mentioned above, it might be difficult to use `RocksDBStore` on some platforms because the RocksDB native binary, written in C++, needs to be distributed together with an application.
Therefore, `RocksDBStore` class is not distributed as a Libplanet package, but as a separate NuGet package called Libplanet.RocksDBStore. `DefaultStore` is still available in the Libplanet package. And so when developing, you can first use `DefaultStore` that's easy to install and use `RocksDBStore` only for testing and actual deployment. Or, for platforms that are difficult to provide RocksDB binaries, you can just go with the `DefaultStore`.

For more information, see [Applying RocksDB to Libplanet][1] written by Seunghun Lee.

[Libplanet.RocksDBStore]: https://www.nuget.org/packages/Libplanet.RocksDBStore/
[IStore]: https://docs.libplanet.io/0.9.0/api/Libplanet.Store.IStore.html
[RocksDB]: https://rocksdb.org/
[DefaultStore]: https://docs.libplanet.io/0.9.0/api/Libplanet.Store.DefaultStore.html
[1]: {{< ref "rocksdb/index.eng.md" >}}


Receiving Blocks from Multiple Peers
------------------------------------

Up to this version, [`PreloadAsync()`][Swarm.PreloadAsync] and [`StartAsync()` method][Swarm.StartAsync] of [`Swarm<T>` class][Swarm] requested and received all blocks from a single peer to catch up with blocks piled up on the network. But unfortunately, if there were many piles of blocks, it would take a long time to get them from just one peer.
And if you were unlucky, you might request blocks from a peer with a very slow connection which would take extra longer time. It also put a lot of pressure on the peer that’s sending the blocks and it was especially hard to ignore the burden for seed nodes that were set by default for deployed applications.

So starting with this version, block downloads are improved to be evenly distributed across multiple peers and even when there’s a slow peer among the senders, the downloading speed has been significantly reduced. 


[Swarm]: https://docs.libplanet.io/0.9.0/api/Libplanet.Net.Swarm-1.html
[Swarm.PreloadAsync]: https://docs.libplanet.io/0.9.0/api/Libplanet.Net.Swarm-1.html#Libplanet_Net_Swarm_1_PreloadAsync_System_Nullable_TimeSpan__IProgress_Libplanet_Net_PreloadState__IImmutableSet_Libplanet_Address__EventHandler_Libplanet_Net_PreloadBlockDownloadFailEventArgs__CancellationToken_
[Swarm.StartAsync]: https://docs.libplanet.io/0.9.0/api/Libplanet.Net.Swarm-1.html#Libplanet_Net_Swarm_1_StartAsync_TimeSpan_TimeSpan_CancellationToken_


Signed App Protocol Version
---------------------------

Last year, [Libplanet 0.3 added `appProtocolVersion` parameter in the `Swarm<T`() constructor.][2] This allowed nodes with compatible protocols to communicate with each other and adequately handled nodes without compatible protocols according to the application.

Our team also took advantage of this feature and used it for software updates when encountering a higher version of the node. But with this use, we realized that a modulated software could be used to maliciously display high-version numbers (which have never been reported) and attack other nodes to attempt false software updates.

To avoid this, the application protocol version that used to be described as [`System.Int32`][System.Int32] is now changed to [`AppProtocolVersion` type][AppProtocolVersion], which includes multiple metadata such as signatures and signers. The app protocol version must be signed, and each node will *individually* determine which signer's version of the app protocol it will trust using the `trustedAppProtocolVersionSigners` parameter in the `Swarm<T>()` constructor.

This approach protects each node from unintended (modified) software updates, while also giving each node the freedom to choose a different application roadmap that is freely forked if desired.


[2]: {{< ref "../../2019/05/libplanet-0.3.eng.md" >}}#responsive-apis-for-nodes-with-different-versions
[System.Int32]: https://docs.microsoft.com/en-us/dotnet/api/system.int32
[AppProtocolVersion]: https://docs.libplanet.io/0.9.0/api/Libplanet.Net.AppProtocolVersion.html


Key Storage
-----------

Last year, [Libplanet added a `ProtectedPrivateKey` class to safely store `PrivateKey` in Version 0.7.][3] However, because [`ProtectedPrivateKey`][ProtectedPrivateKey] deals with only one key, handling multiple keys required a separate implementation for the application to create a directory, set a file name, and write the file.

So from this version, [`Web3KeyStore`][Web3KeyStore] class, which physically preserves and manages keys, are now available, eliminating the need to implement these features separately. In addition to the `Web3KeyStore` that preserves keys in [Web3 Secret Storage Definition] format, we have also introduced [`IKeyStore` Interface][IKeyStore], which abstracts specific preservation methods (implementation details).


[3]: {{< ref "../../2019/11/libplanet-0.7/index.eng.md" >}}#key-store-implementation-to-safely-store-personal-keys
[ProtectedPrivateKey]: https://docs.libplanet.io/0.9.0/api/Libplanet.KeyStore.ProtectedPrivateKey.html
[Web3KeyStore]: https://docs.libplanet.io/0.9.0/api/Libplanet.KeyStore.Web3KeyStore.html
[Web3 Secret Storage Definition]: https://github.com/ethereum/wiki/wiki/Web3-Secret-Storage-Definition
[IKeyStore]: https://docs.libplanet.io/0.9.0/api/Libplanet.KeyStore.IKeyStore.html


`planet`: CLI Utility
---------------------

[`Swarm<T>()` constructor][Swarm()] has [assumed a specific genesis block since last version][4] and [from this version, it signs the app protocol version.][5] Because these values include the results of cryptographic algorithm, it is impossible for a human to randomly generate the value. However, since the development process does require a lot of filling up random values, it was definitely a hassle to call up Libplanet APIs from C# interactive shells or PowerShell to calculate the desired values.

To make this easier, we are deploying a <abbr title="command-line interface">CLI</abbr> utility called `planet` from this version. The `planet` command contains multiple subcommands, and currently provides key storage management and app protocol version signing feature. In the future, features such as creating a random genesis block will be added. Please check the `planet --help` command for detailed instructions.

The `planet` command is distributed in a NuGet package called [Libplanet.Tools], which can be installed on systems with .NET Core SDK using the following command:


~~~~ bash
dotnet tool install -g Libplanet.Tools
~~~~


If the .NET Core SDK is not installed, you can also download and install the official binary uploaded on the [Release Page][0.9.0]. The official binary is available in three versions: Linux (x64), macOS (x64), and Windows (x64).


[4]: {{< ref "../../2020/02/libplanet-0.8.eng.md" >}}#genesis-block-anticipation
[5]: #signed-app-protocol-version
[Swarm()]: https://docs.libplanet.io/0.9.0/api/Libplanet.Net.Swarm-1.html#Libplanet_Net_Swarm_1__ctor_Libplanet_Blockchain_BlockChain__0__Libplanet_Crypto_PrivateKey_Libplanet_Net_AppProtocolVersion_System_Int32_System_String_System_Nullable_System_Int32__IEnumerable_Libplanet_Net_IceServer__Libplanet_Net_DifferentAppProtocolVersionEncountered_IEnumerable_Libplanet_Crypto_PublicKey__
[Libplanet.Tools]: https://www.nuget.org/packages/Libplanet.Tools/


Furthermore
-----------

You can learn more about additional changes in our [release notes][0.9.0].

If you’re interested, install and try it out! And as always, if you have any questions about the new release or Libplanet in general, please visit our [Discord chatroom][Discord] and let’s chat!

[Nine Chronicles]: https://nine-chronicles.com/
[Discord]: https://discord.gg/planetarium
