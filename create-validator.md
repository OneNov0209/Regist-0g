🧾 Register as Validator on 0G Network

This guide walks you through the complete process of registering as a validator in the 0G Network, from setting up the environment to submitting the validator registration transaction using Remix IDE.


## 1. 📨 Create Deposit Message

Run:
```
0gchaind deposit create-validator \
<validator_address> \
200000000 \
$HOME/.0gchaind/0g-home/0gchaind-home/config/genesis.json \
--home $HOME/.0gchaind/0g-home/0gchaind-home \
--chaincfg.chain-spec=devnet
```

You will get output like:

pubkey: 0x...
credentials: 0x...
amount: 200000000
signature: 0x...

Save pubkey and signature.


---

## 2. 🧮 Compute Validator Address

Install Foundry:
```
curl -L https://foundry.paradigm.xyz | bash
```
```
source ~/.bashrc
```
```
foundryup
```

Compute address:
```
cast call 0xea224dBB52F57752044c0C86aD50930091F561B9 \
"computeValidatorAddress(bytes)(address)" \
<pubkey_bytes> \
--rpc-url https://evmrpc-testnet.0g.ai
```


---

## 3. 🛠 Deploy Contract in Remix

📄 File: StakingInterface.sol
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IStakingContract {
    struct Description {
        string moniker;
        string identity;
        string website;
        string securityContact;
        string details;
    }

    function createAndInitializeValidatorIfNecessary(
        Description calldata description,
        uint32 commissionRate,
        uint96 withdrawalFeeInGwei,
        bytes calldata pubkey,
        bytes calldata signature
    ) external payable;
}
```

🚀 Execute on Remix:

1. Go to Remix


2. Paste StakingInterface.sol


3. Compile with Solidity ^0.8.0


4. Go to Deploy & Run, select Injected Web3


5. In At Address, paste validator contract address


6. Click At Address


7. Expand function createAndInitializeValidatorIfNecessary


8. Fill in:
```

[
  "YourMoniker",
  "YourIdentity",
  "https://yourwebsite.xyz",
  "you@example.com",
  "Stake with us!"
]
```
Commission Rate: 50000

Withdrawal Fee in Gwei: 1

Pubkey: 0x... (from step 2)

Signature: 0x... (from step 2)


9. Click Transact and confirm in MetaMask

---

## 4. ✅ Verify Transaction
```
curl -s -X POST https://evmrpc-testnet.0g.ai \
-H "Content-Type: application/json" \
--data '{
  "jsonrpc":"2.0",
  "method":"eth_getTransactionReceipt",
  "params":["<tx_hash>"],
  "id":1
}' | jq
```

If status is 0x1, transaction succeeded.


---

🧭 Final Notes

Your validator will appear in the dashboard after delegation by the dev team.

You do not need to redeposit.

Make sure your node is running and synced.



---

🌐 Useful Links

RPC: https://rpc-0gchaind.onenov.xyz/

API: https://api-0gchaind.onenov.xyz/

EVM RPC: https://evm-0gchaind.onenov.xyz/

Official Site: https://0g.ai



---

📩 Need help? Join the 0G community or ask your validator friends!


---

