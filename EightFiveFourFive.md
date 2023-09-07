We are given a solidity source code on creating an instance.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract EightFiveFourFive {
    string private use_this;
    bool public you_solved_it = false;

    constructor(string memory some_string) {
        use_this = some_string;
    }

    function readTheStringHere() external view returns (string memory) {
        return use_this;
    }

    function solve_the_challenge(string memory answer) external {
        you_solved_it = keccak256(bytes(answer)) == keccak256(bytes(use_this));
    }

    function isSolved() external view returns (bool) {
        return you_solved_it;
    }
}
```
we can see that our obvious goal here is to set the `you_solved_it` variable to `True`. I solved this challenge using foundry CLI tool `cast`.

see : official documentation [cast](https://book.getfoundry.sh/cast/)

first we need to find the actual value of the `use_this` string value,it is set in the constructor and can be read with `readTheStringHere()` . 

>cast call 0xf22cB0Ca047e88AC996c17683Cee290518093574 "readTheStringHere()(string)" --rpc-url https://blockchain-eightfivefourfive-88d67f3487ecf2be-eth.2023.ductf.dev:8545`

after we run the command it gives us the string `I can connect to the blockchain!`
we can now pass it as argument to the `solve_the_challenge()` function which will set `isSolved()` to True.

>cast send 0xab9A67BDA6C35E84B64F48A12c668978A450c7B0 "solve_the_challenge(string)" "I can connect to the blockchain!" --rpc-url https://blockchain-eightfivefourfive-88d67f3487ecf2be-eth.2023.ductf.dev:8545 --private-key 0x54f6736d945070c644c48b3c5f89ad0306eaa5740a5854ef005fb1fe9367267a https://blockchain-eightfivefourfive-88d67f3487ecf2be-eth.2023.ductf.dev:8545 --legacy

and that's that we can now read the flag from the instance provided since `isSolved()` will now return true.

`DUCTF{I_can_connect_to_8545_pretty_epic:)}`
