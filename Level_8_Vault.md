# Level 8 : Vault

**Ethernaut Level 8 : Vault : security of variable on storage**

Each smart contract has its own storage to reflect the state of the contract. The values in storage persist across different function calls. And each storage is tethered to the smart contract’s address.

![Untitled](images/Level_8.png)

`web3.eth.getStorageAt(contractAddress, slotNumber)` 

Here password is in the slot number 1, easy peazy !