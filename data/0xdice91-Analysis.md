# Analysis - Open Dollar Contest


## Approach
During my audit, I focused on understanding the mechanisms of Open dollar, especially its ERC 721 Vault, the interaction of the different proxies, vault, and the safe manager., and how it they work together to tie CDP ownership to a specific NFT.
- Read the  Open Dollar docs.
- Understanding the different proxies, the safe manager and how they work together.
- Read the contracts in scope line by line and understand every function and how they interact with each other
- Track the flow of the transaction execution process of each vault and how the NFT transferred
- Write questions down (on how things can go wrong, and how a malicious user can go about causing harm to the protocol).
- Write down my issues found.
- Read the docs again, and research similar protocols and issues they usually face.
- Write my report.

I started by reading the documentation thoroughly to get a full grasp of the Open Dollar protocol, what Open Dollar is and how it works. I spent time a considerable amount of time understanding how transactions and safe ownership are executed as well as running different scenarios on how things can go wrong or be manipulated. In general, I believe the architecture of the protocol is foolproof especially the mechanism used in transaction execution, safe transfer and the use of a proxy to represent each user is a well-thought-out and great idea for security.
As a whole, I consider the code execution to be excellent and its docs did well to express the intention and security consciousness of the developers, something worth noting would be the simplicity of the protocol and how the implementation of its functions is easy to understand thereby ensuring a better audit, I really had a good time auditing this protocol, below is my review of the different aspects of the codebase.
| Codebase Quality Categories  | Comments |
| --- | --- |
| **Unit Testing**  | The Codebase was actually well-tested for the most part, and its use of foundry enabled a better audit experience.|
| **Code Comments**  | Comments and Natspecs in general were easy to understand and straight to the point. Although in some cases more information would have made auditing easier overall on a scale of 1-10 I'll give them an 8 |
| **Documentation** | The docs explained how users interact with the protocol and clear description of the job of each contract in scope making it easier to digest as an auditor, The docs tackled all the major contracts and their functionalities and also provided a great deal of help in understanding the implementation of their mechanisms, |
| **Organization** | The Codebase was actually so easy and simple removing complexities making it well organized and ensuring clear distinctions between the contracts, and how they interact with each other to help make for a smoother audit|
## Centralization Risks
The protocol offers comfort to the user and removes any chance of a centralization risk as all transactions and approved accounts are all user-chosen addresses. The use of Governance Minimization is an excellent idea to serve as a safeguard against any long-term centralization of protocol control.
## Mechanism Review
The mechanism used for Open Dollar is well thought out as it brings comfort to the user while ensuring full authority over the vault, The implementations are very easy to grasp and work very well as far as I can tell, and due to the nature of the protocol I think it should go by easily in terms of user adoption.
## Systematic Risks
There is little to no systematic risk that poses any kind of threat whatsoever to the growth and longevity of the protocol, although the implementation of upgradable contracts would have served as an easy way to solve any unforeseen risks on deployment or that surface because of other external factors later on along the life cycle of the protocol.
## Conclusion
I applaud the Open Dollar team for the creation of Open Dollar as it is a brilliant, well-executed idea to offer an alternative way in which collateralized debt position can be owned.


### Time spent:
11 hours