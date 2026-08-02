Base is an optimistic rollup built on the OP Stack that aims to make onchain apps more accessible, cheaper and developer-friendly. This repo kicks off my journey contributing to the Base ecosystem.
docs: add project overview and Base network details

Base is an OP Stack L2 secured by Ethereum. This update documents the chain ID (8453), RPC endpoints and the main goals of this repository: learning and shipping onchain apps.
feat: add SimpleStorage contract for Base

Created a basic SimpleStorage.sol contract with set and get functions. First smart contract ready to deploy on Base Sepolia.
feat: add owner-only modifier to SimpleStorage

Introduced an onlyOwner modifier and restricted the set function. Improves security for contracts deployed on Base.
feat: add getOwner function to SimpleStorage

Exposed a public getOwner view function. Makes it easier to verify ownership after deploying on Base.
feat: add pause functionality to SimpleStorage

Implemented a pause mechanism so the owner can temporarily disable the set function. Useful safety feature for contracts on Base.
