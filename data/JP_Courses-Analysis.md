I really like the idea of Non-fungible Vaults, it opens up possibilities that did not exist before in the DeFi/Web3 ecosystem.

Only spend a couple hours on this one.

Although I didnt yet find a way to see if someone else can own the valid user's NFT, I did find a QA + GAS finding where the `||` logic considers the case where user is linked to proxy via the mapping, but user is not the owner. This should never be possible, as far as my understanding goes, and therefore would violate an invariant. Therefore this case can be removed from the `||` logic operator check, as well as another one, probably a good saving on some gas, and improving overall QA.

I will definitely deep dive this protocol next time, whether via contest or bounty.

So far the codebase seems tight.

### Time spent:
3 hours