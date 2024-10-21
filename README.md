# entropy-devnet

### Prerequisites:
- **Rust** installed
- **Solana CLI** installed
- A working knowledge of **Solana** and **Rust**
- **Anchor** framework for building Solana programs

---

## Step 1: Install Required Dependencies

### 1.1 Install Rust
Ensure you have Rust installed. If not, install it with the following:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

After installation, verify by running:

```bash
rustc --version
```

### 1.2 Install Solana CLI
Next, install the Solana CLI:

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.9.28/install)"
```

Then, add Solana to your PATH and verify:

```bash
solana --version
```

### 1.3 Install Anchor
Anchor is a framework that simplifies building Solana programs. Install it via npm:

```bash
cargo install --git https://github.com/project-serum/anchor --tag v0.24.2 anchor-cli --locked
```

Verify installation:

```bash
anchor --version
```

---

## Step 2: Set Up Solana Devnet

Before interacting with Entropy on Devnet, you need to configure Solana for Devnet.

```bash
solana config set --url devnet
```

Generate a keypair if you don't have one:

```bash
solana-keygen new
```

A public/private keypair will be saved locally, and you can view your public key by running:

```bash
solana address
```

Airdrop some Devnet SOL to this wallet for transactions:

```bash
solana airdrop 1
```

---

## Step 3: Cloning and Setting Up Entropy Repo

Clone the Entropy repository:

```bash
git clone https://github.com/EntropyXYZ/entropy.git
```

Navigate to the directory:

```bash
cd entropy
```

Install dependencies:

```bash
anchor build
```

Once built, deploy the smart contract to Devnet:

```bash
anchor deploy
```

---

## Step 4: Interact with the Entropy Program

Entropy provides a number of tools and commands to interact with the system. You can start by running a local test environment to ensure everything works.

### 4.1 Launch Local Validator

If you want to test everything locally, you can start the Solana validator:

```bash
solana-test-validator
```

### 4.2 Run Tests

You can use Anchor's built-in test system to ensure everything is working:

```bash
anchor test
```

This will run all tests included in the Entropy repository and simulate interactions on Devnet.

---

## Step 5: Customize the Devnet

Once the basic setup is working, you can modify your Entropy deployment to suit your needs. You might:
- Add custom markets.
- Modify order book parameters.
- Test additional features.

### Example Code to Create a Market:

```rust
use anchor_lang::prelude::*;
use entropy::state::{Market, initialize_market};

#[program]
pub mod entropy_market {
    use super::*;

    pub fn initialize_market(ctx: Context<InitializeMarket>, price: u64) -> ProgramResult {
        let market = &mut ctx.accounts.market;
        market.price = price;
        Ok(())
    }
}

#[derive(Accounts)]
pub struct InitializeMarket<'info> {
    #[account(init, payer = user, space = Market::LEN)]
    pub market: ProgramAccount<'info, Market>,
    #[account(mut)]
    pub user: Signer<'info>,
    pub system_program: Program<'info, System>,
}
```

Compile and deploy:

```bash
anchor build
anchor deploy
```

---

## Step 6: Monitoring the Devnet

To monitor your transactions, you can use tools like **Solana Explorer**:

- Visit [Solana Explorer](https://explorer.solana.com/)
- Switch to Devnet and search for your transactions or programs.
