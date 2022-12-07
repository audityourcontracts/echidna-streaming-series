# List of Invariants 

## Tested 
- [Deposit increases margin balances](https://github.com/crytic/echidna-streaming-series/blob/25a6367ea13343b95fc66e608ab6c9504780d801/part5/README.md#L4) – any call to `Margin.deposit` should increase the respective Margins.balances object 
- [Withdrawals decrease margin balances](https://github.com/crytic/echidna-streaming-series/blob/1525cf004e6060d9a342e2946e8c3550b2e4bace/part5/contracts/echidna/LibraryMathEchidna.sol#L44-L52)
- [Scale up and scale down are exact inverses](https://github.com/crytic/echidna-streaming-series/blob/1525cf004e6060d9a342e2946e8c3550b2e4bace/part5/contracts/echidna/LibraryMathEchidna.sol#L55-L62)
- [Scale to X64 and from X64 are exact inverses](https://github.com/crytic/echidna-streaming-series/blob/1525cf004e6060d9a342e2946e8c3550b2e4bace/part5/contracts/echidna/LibraryMathEchidna.sol#L63-L73)

## "In-English" Invariants of the System
Note: Implementations for these will be implemented in Part 6. 

**System Deployment**
- PRECISION() constant = 10**18
- MIN_LIQUIDITY() should be > 0
- One deployed, engine's tokens should map to the correct tokens 
- Last timestamp on engine should never exceed timestamp's maturity 
- Gamma should never exceed 10000

**Engine Creation** 
- Correct preconditions should never revert 
  - Strike > 0 
  - Sigma is between [1, 1e7]
  - Gamma is between [9000,10000]
  - Maturity is in the future
- Invalid engine creation states 
  - Gamma > 10000 should always revert 
  - Maturity in the past should always revert 
  - Strike = 0 should always revert 
  - Sigma < 1 should always revert

## Deposits and Withdrawals 

**Engine Deposit**
- Users can never deposit 0 tokens into the system 
- Under correct preconditions, deposit should always increase a recipient's balance 
- Engine's risky and stable balance should always increase

**Engine Withdrawals** 
- Users can never withdraw 0 tokens 
- A withdrawal should reduce msg.sender's margin balance 
- Engine's risky and stable balances should always decrease

**Inter-related Operations** 
- A deposit and withdrawal should result in the same pre- and post-margin balances 
## Remove and Allocate
**Engine Allocate** 
- Liquidity chooses the minimum of the liquidity factors 
- An allocation to a pool should result in an increase to pool reserves 

**Engine Remove** 
- Removing from a pool should result in a decrease to pool reserves 

## Running Code (for Part 5)

```
npx hardhat clean && npx hardhat compile && echidna-test . --contract LibraryMathEchidna --corpus-dir  corpus --test-mode assertion
```