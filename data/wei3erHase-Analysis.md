About the Vault721 and SafeManager:
Merge them! There is no possibility of changing the SafeManager without breaking the afterTransfer, and there's no need of changing the SafeManager, unless new features are added, but then a new ERC721 can be released.

Now the deployment looks like this:
- deploy `Vault721(governor)` (leave 2 methods open to be initialized)
- deploy `SafeManager(safeManager, vault721)`
    - calls `vault721` to initialize itself
- deploy `NftRenderer(vault721, safeManager, oracleRelayer, ...)`
    - again calls `vault721` to initialize itself
    - only queries safeManager for SafeEngine

If Vault721 and SafeManager are merged into a SafeManager721, then the deployment could look like:
- deploy `NftRenderer(safeEngine, oracleRelayer, ...)`
- deploy `SafeManager721(nftRenderer, safeEngine, ...)`

Notice, the NftRenderer doesn't need at all to know what's the SafeManager, it doesn't even care about `ids`, since it can read the safe status directly from the safeEngine using `(address safeHandler, bytes32 cType)`.

Merging these 2 contracts seems like a logical design decision.

About the proxies:
Instead of having a mapping user to proxy, and the nft ownership is the user's, but the safe ownership is of the proxy... Why not making the whole permission based on the NFT owner, and ask the owner to provide his proxy the permission to act on his behalf. Is just 1 approve transaction away from not having to interconect so many contracts and concepts in the authorization flow. 

One argument against i can think would be "but the user then could authorize any proxy and get rugged", which is true, because you're asking him to set something up instead of making it seamless and by default (that's what i understand that the mechanics rationale is about). But it is also true, that despite the user has a proxy, he can also approve any random address and get rugged, so is risk neutral in a way, since is not adding more danger.

There is no reason why a EOA user shouldn't be calling straight to the SafeManager (ofc if he owns the safe), provided he can somehow interact via proxy with it: since is almost impossible, for example, to empty a SAFE without a proxy, since the debt to pay is calculated by the second and the parameters have to be previously signed to be broadcast.

### Time spent:
4 hours