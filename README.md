1.input the jump destiny

2.the codesize 0x34380356FDFD5B00FDFD is 10 (bytes), so the input value should fit for 10 - value = 6 (JUMPDESC)

3.input calldata with 4 bytes

4.codesize for 0x34381856FDFDFDFDFDFD5B00 is 12 (bytes), the value XOR 12 (1100 in binary) should be 0x0a (1010), so value should be (0110) 6

5.callvalue x callvalue = 0x100 (256), then the eq can result in 1 (true) and JUMPI will be triggered. callvalue should be 16

6.calldataload need one byte input for the offset, in the puzzle it is 0, so the calldata can be directly 0x000000000000000000000000000000000000000000000000000000000000000a (32 bytes) to make the jump input to be 0x0a (10)

7.the line 9 EXTCODESIZE need to give output 1, so need to generate a function opcode returns 1 byte, 
	  I use PUSH1 0x01
			PUSH1 0x00
			RETURN
	  which returns 1 byte, hex code is 0x60016000f3

8.the call need to return 0 (unsuccess) to pass the puzzle, so need to create a contract that creates an exception if calldata is 0 (or always fail)
	PUSH1 0x00
	CALLDATALOAD
	PUSH1 0x07
	JUMPI
	INVALID
	JUMPDEST
	generates hex code 0x600035600757fe5b
	PUSH8 0x600035600757FE5B
	PUSH1 0x00
	MSTORE
	PUSH1 0x08
	PUSH1 0x18
	RETURN
	returns the previous function and hex code is 0x67600035600757FE5B60005260086018f3, which is the calldata to solve the puzzle
   answer is 0x60FD60005360016000F3?

9.calldata size need to be more than 3 bytes to pass first jump condition, calldata size x call value should = 8 to pass second jump condition

10. call value should be < 27 (0x1b) to pass first condition, calldata size % 3 should be 0 to pass second condition, call value + 0x0a(10) should be 0x19(25) to pass third condition, call value = 15, calldata can be empty

11. callvalue ^ calldatasize = 64, 0x41 + calldatasize = 47, callvalue = 2, calldatasize = 6



# EVM puzzles

A collection of EVM puzzles. Each puzzle consists on sending a successful transaction to a contract. The bytecode of the contract is provided, and you need to fill the transaction data that won't revert the execution.

## How to play

Clone this repository and install its dependencies (`npm install` or `yarn`). Then run:

```
npx hardhat play
```

And the game will start.

In some puzzles you only need to provide the value that will be sent to the contract, in others the calldata, and in others both values.

You can use [`evm.codes`](https://www.evm.codes/)'s reference and playground to work through this.
