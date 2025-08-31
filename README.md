# 🐉 SHIHULUD 🪱

Here you will find the instructions to participate in the Shihulud incentivized testnet! Users who will help us in testing the network may earn community allocations of the future WORM token!

In order to participate in the testnet you'll need:

- [x] An x86-64 machine or VPS or dedicated-server with at least 16GB of RAM (***WARN:*** ARM / Apple Silicon devices are not supported!)
- [x] At least 1.0 Sepolia ETH which you may get from Sepolia faucets

Test on Debian/Ubuntu systems:

## 1. Install requirements:
   ```
   sudo apt install -y build-essential cmake libgmp-dev libsodium-dev nasm curl m4 git wget unzip nlohmann-json3-dev
   ```
## 2. Install Rust toolchain:
   ```
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```
## 3. Clone the repo:
   ```
   git clone https://github.com/worm-privacy/miner && cd miner
   ```
## 4. Download parameters files:
   ```
   make download_params
   ```
## 5. Install `worm-miner`:
   ```
   cargo install --path .
   ```
## 6. Burn ETH and Immediately Mint BETH & submit a proof

   Burn 1 ETH, and use 0.999 of it in the same transaction (i.e., full spend = 0.999, fee = 0.001).
   This means no remaining amount is left for later spending: 

   ```
   worm-miner burn --network anvil --private-key 0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d  --amount 1 --spend 0.999 --fee 0.001
   ```
   This will mint 0.999 BETH to your address
   
## 7. Congrats! 0.999 BETH has been minted for `0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1`! 
   To verify the minted balance:

   ```
   worm-miner info --network anvil --private-key 0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d
   ```

## 8. List your available coins:

   After burning ETH, not all of it may be consumed immediately. The unspent portions are stored as coins.
   You can list all coins linked to your account with:
   ```
   worm-miner ls --network sepolia
   ```
   example out put : 
   ```
   Found 1 entrie(s) for network: "sepolia" :
  1: {
  "amount": "85000000000000000",
  "burnKey": "5687092680620625472546954797008075032170996993219203510495914288796328250267",
  "id": "1",
  "network": "sepolia"
}
   ```
   Each coin in the output will have a unique ID and associated balance.
## 9. Burn ETH and Spend Only Partially
   If you don’t want to lock the entire burn amount, you can spend part of a coin’s balance.
   For example, burn 1 ETH, but only spend 0.5 now:
   ```
   worm-miner burn \
   --network anvil \
   --private-key 0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d \
   --amount 1 \
   --spend 0.5 \
   --fee 0.001 
   ```
   That leaves 1 - 0.5 - 0.001 = 0.499 ETH for future use.
   
   Use the coin ID from the step #8 and specify how much to spend, the fee, and the receiver address: 
   ```
   worm-miner spend --id 1 --amount 0.3 --fee 0.1 --private-key 0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d2171b23b1d --receiver 0x1dF62f291b2E969fB0849d99D9Ce41e2F137006e --network anvil
   ```
   This command creates a spend transaction from that coin, leaving the unused balance available for future spends.

   Where: 
   - `--id` → The coin ID (from worm-miner ls) that you want to spend from.
   - `--amount` → The amount of ETH (converted to BETH) to spend in this transaction.
   - `--fee` → The transaction fee (in ETH) deducted from the coin’s balance.
   - `--private-key` → Your Ethereum account private key (used to sign the spend transaction).
   - `--receiver` → The Ethereum address that will receive the spent amount.
   - `--network` → The network to run against (e.g., anvil, sepolia).

## 10. Participating and Claiming WORM Rewards
   The **mining process** is split into two commands:  
- `participate` → Register to mine WORM in future epochs.  
- `claim` → Collect WORM rewards for completed epochs.  

## ⏱️ Epochs and Reward Distribution

- **Epoch Duration:** Each epoch lasts **30 minutes**.  
- **Fixed Rewards:** A fixed total amount of **WORM** is generated in every epoch (e.g., `X WORM per epoch`).  

Your personal reward in an epoch depends on how much **BETH** you commit compared to other participants.  
The distribution is **proportional**, meaning your share is:

Your WORM reward = (Your BETH committed / Total BETH committed in the epoch) × Total WORM for the epoch

### 🔢 Example Calculation

Suppose each epoch generates **100 WORM**:

- Alice commits **0.5 BETH**  
- Bob commits **1.0 BETH**  
- Carol commits **1.5 BETH**

➡️ Total committed = **3.0 BETH**  

Rewards distribution:  
- Alice: `(0.5 / 3.0) × 100 = 16.67 WORM`  
- Bob: `(1.0 / 3.0) × 100 = 33.33 WORM`  
- Carol: `(1.5 / 3.0) × 100 = 50.00 WORM`  

At the end of the epoch, when each participant calls `claim`, they will receive their portion of WORM.

---

### ⚖️ Important Notes
- Committing **more BETH** increases your share, but the reward depends on the **relative contribution of all participants**.  
- If you are the **only participant**, you receive **100%** of the WORM generated for that epoch.  
- Unclaimed rewards remain available until you explicitly run the `claim` command. 
### Putting It Into Action:
### 1. Get Epoch Information
Before participating, you can check the current epoch and related details:
```bash
worm-miner info --network sepolia
```
example output:
```
Current epoch: 0
BETH balance: 0.595000000000000000
WORM balance: 0.000000000000000000
Claimable WORM (10 last epochs): 0.000000000000000000
Epoch #0 => 0.002000000000000000 / 0.002000000000000000 (Expecting 50.000000000000000000 WORM)
Epoch #1 => 0.002000000000000000 / 0.002000000000000000 (Expecting 50.000000000000000000 WORM)
Epoch #2 => 0.002000000000000000 / 0.002000000000000000 (Expecting 50.000000000000000000 WORM)
Epoch #3 => 0.000000000000000000 / 0.000000000000000000 (Expecting 0.000000000000000000 WORM)
Epoch #4 => 0.000000000000000000 / 0.000000000000000000 (Expecting 0.000000000000000000 WORM)
Epoch #5 => 0.000000000000000000 / 0.000000000000000000 (Expecting 0.000000000000000000 WORM)
Epoch #6 => 0.000000000000000000 / 0.000000000000000000 (Expecting 0.000000000000000000 WORM)
Epoch #7 => 0.000000000000000000 / 0.000000000000000000 (Expecting 0.000000000000000000 WORM)
Epoch #8 => 0.000000000000000000 / 0.000000000000000000 (Expecting 0.000000000000000000 WORM)
Epoch #9 => 0.000000000000000000 / 0.000000000000000000 (Expecting 0.000000000000000000 WORM)
```
### 2. Participate in Future Epochs
To register for mining, use the participate command:

```
worm-miner participate \
--amount-per-epoch 0.002 \
--num-epochs 3 \
--private-key 0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d \
--network sepolia
```

Where:

- `--amount-per-epoch` → The amount of BETH you commit per epoch.
- `--num-epochs` → Number of future epochs you want to participate in, starting from the current available epoch.
- `--private-key` → Your Ethereum private key (used to sign the transaction).
- `--network` → The target network (anvil, sepolia, etc.).
### 3. Claim Rewards

At the end of an epoch, participants can claim their WORM rewards based on their share of committed BETH.
```
worm-miner claim \
--from-epoch 0 \
--network sepolia \
--num-epochs 1 \
--private-key 0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d

```
Where:

- `--from-epoch` → The starting epoch index from which you want to claim rewards.
- `--num-epochs` → The number of epochs to claim rewards for (starting from from-epoch).
- `--private-key` → Your Ethereum private key (used to sign the claim transaction).
- `--network` → The target network (anvil, sepolia, etc.).

➡️ Example: With --from-epoch 0 and --num-epochs 1, you will claim rewards for epoch 0 only.
➡️ Example: If you set --amount-per-epoch 0.002 and --num-epochs 3, you will commit 0.002 BETH for each of the next 3 epochs.

Participation Flow:

- Use worm-miner info to check the current epoch.

- Use worm-miner participate to commit BETH for upcoming epochs.

- After epochs are completed, use worm-miner claim to collect your WORM rewards.