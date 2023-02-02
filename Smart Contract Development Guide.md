This essay outlines the phases to write Mainnet ready Solidity smart contracts from designing to final deployment. Your company and or team may have extra requirements (such as security and compliance policies etc.), you will be able to insert your specific requirements into each phase.

> Note: if you are new to Solidity, checkout the learning resources section to learn the basics and best practices.

In a nutshell, the process of smart contracts goes like the following.

***Design Specifications*** -> 
***Code Implementation*** -> 
***External Auditing*** -> 
***Testnet Deployment*** ->
***Mainnet Deployment*** -> 
***Contract Maintenance***

Read through each phase carefully. You may need to kick off later phases to speed up the process and reduce idle waiting time (like securing an audit slot early as you may not get an auditing slot right away).

## Phase 1 - Design Specification
First design your contract interfaces and identify special address access and future maintenance based on product needs.

Figure out answers to the following questions.
* __Do the contracts need to be all or partially upgradable?__ Will you need to update the code in the future? Upgradability provides flexibility, but also increases security concerns. There are resources available in the learning resources section for upgradable smart contracts.
* __What functions need to be public/external?__ The amount of external/public functions should be limited as needed. Any public function is a potential attacking entry.
* __Who should have access to those functions?__ Design auth carefully and mindfully. Don’t overcomplicate the contract, but consensus between multiple persons should be required for any powerful/user-impacting actions. 
  * If special access (like an admin role to upgrade a contract, operator role to update parameters) is required, who should own the addresses of those access, multi-sig on ETH, MPC wallet, EOA? Who should be signers for multi-sigs (they will be responsible for user-impactful actions on the contract)?
  * Once identified roles, document them.
* __Does it charge a fee?__ If so, who should receive the fee, treasury?
* __Are there public battle tested contracts that do what you need already?__ If so, it might be better to use them if there are no other concerns as they are battle tested.
* __Will you need an indexer?__ If so, write your event with that in mind.

The goal in this phase is to figure out what you are building, for who and why.

## Phase 2 - Code Implementation
At this phase, you know what you are building and you are ready to get your hands dirty: write the smart contract.

> Note: you may want to also start step 3 to secure an external auditing slot.

Checkout the appendix sections for recommended tools and interesting boilerplates to get you started.

Write your contracts and tests, 90% coverage is good and 100% is great.
* __Unit Tests.__ Test each public/external function and all possible scenarios for inputs to functions. For example, test a `swap` function to swap assets correctly.
* __Integration tests.__ Test a complete user flow (usually have more than one function). For example, in an ERC20 contract, you may want to test the scenario where a user can approve and allow a 3rd party contract/user to transfer assets out of their wallet.
* __Mainnet simulation.__ Especially if your contract needs to interact with a 3rd party contract. [Fork the mainnet](https://hardhat.org/hardhat-network/guides/mainnet-forking.html), and test your contract in the simulated environment against the current state of the mainnet.
* __Fuzzing.__ Run a fuzzer, like echidna (see recommended tools), against your contract to further identify potential bugs/vulnerabilities.
Your code should be reviewed by at least 1 fellow blockchain engineer. PR approved, Code merged. Time for the next steps.

## Phase 3 - External Auditing

Once your code is ready, time to have the code audited by a 3rd party. You can always choose a different option like [code4rena](https://code4rena.com/). The goal here is to get an independent party to review the code before millions of dollars go in it.

## Phase 4 - Testnet Deployment
There is no restriction on which testnet to use. It usually depends on which testnet has your dependencies. 
* __Deploy your contract to a testnet__ ([example local deploy script](https://github.com/trinity-0111/NFT-templates/blob/main/scripts/SimpleNFT-deploy.ts)).
* This is your staging environment. Frontend, backend should all be hooked up to the testnet and Product should be able to test your final product on testnet.
You technically can deploy to testnet anytime to test as long as you are comfortable with the fact the code will be in public on the testnet chain.

## Phase 5 - Mainnet Deployment
Deploy your contract to Mainnet ([example local deploy script](https://github.com/trinity-0111/NFT-templates/blob/main/scripts/SimpleNFT-deploy.ts)). Transfer ownership (if any) to a multi-sig or MPC wallet or some other wallet that is not controlled by one person.

## Phase 6 - Contract Maintenance
You may need to maintain the contracts, like upgrade to new versions if they are upgradable, adjust fee or other parameters.
However, at this point, only special roles (addresses usually not controlled by one person, should be designed in Phase 1) can make those changes. Based on the actually implementation of those wallet, you will have different maintenance process:
* __Multi-sigs__. Use [gnosis safes](https://gnosis.io/safe/) as an example, you should be able to use both their web app to start a transaction manually or their Safe transaction service to start a transaction through code. Only the required amount of signers signed the transaction, you can execute the maintenance transaction.

Congratulations! You have successfully written and deployed a production smart contract. 


## Appendix
### Learning Resources
* [Solidity Docs](https://docs.soliditylang.org/en/v0.8.13/)
* [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.13/style-guide.html)
* [Hardhat Tutorials](https://hardhat.org/tutorial/)
* [Solidity Leaning Zombie Game](https://cryptozombies.io/)
* [Opezeppelin Resources](https://docs.openzeppelin.com/contracts/4.x/)
* [Upgradable Smart Contracts](https://docs.google.com/presentation/d/1N8vnMXDfkqQv4x1Snp-GauKHT0eQ7dj3fG-qs8ryntg)

### Best Practices and Style Guides
* Solidity style: https://docs.soliditylang.org/en/v0.8.17/style-guide.html
* Foundry: https://book.getfoundry.sh/tutorials/best-practices
* Security: https://consensys.github.io/smart-contract-best-practices/

### Recommended Tools
* [Hardhat](https://hardhat.org/getting-started/)
* [Typescript](https://www.typescriptlang.org/) and [Type Chain](https://github.com/dethcrypto/TypeChain)
* [Yarn](https://yarnpkg.com/)
* Latest [Node](https://nodejs.org/en/)
* [Hardhat Plugins](https://hardhat.org/plugins/#community-plugins)
  * [Solidity Coverage Plugin](https://www.npmjs.com/package/solidity-coverage)
  * [Solidity Gas Reporter](https://www.npmjs.com/package/hardhat-gas-reporter)
  * [Hardhat Etherscan](https://hardhat.org/plugins/nomiclabs-hardhat-etherscan.html)
* Fuzzing tool [Echidna](https://github.com/crytic/echidna)
* [Slither](https://github.com/crytic/slither)
* [Sollint](https://protofire.github.io/solhint/)
* [Etherscan](https://etherscan.io/)

### Boilerplates/Templates
* [Open Source NFT](https://github.com/trinity-0111/NFT-templates)
* [Open Source Upgradable Contracts](https://github.com/trinity-0111/upgradable-contracts-template)
* [Open Source ERC20](https://github.com/CoinbaseStablecoin/contract-template-hardhat)
* [Open Source Greeter](https://github.com/paulrberg/solidity-template)

