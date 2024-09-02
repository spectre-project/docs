# Rusty-Spectre: Full Node Installation Guide
[![GitHub release](https://img.shields.io/github/v/release/spectre-project/rusty-spectre.svg)](https://github.com/spectre-project/rusty-spectre/releases)
[![Join the Spectre Discord Server](https://img.shields.io/discord/1233113243741061240.svg?label=&logo=discord&logoColor=ffffff&color=5865F2)](https://discord.com/invite/FZPYpwszcF)

**The Rust node is the recommended software** providing enhanced performance, scalability, and security compared to its [Golang node](https://github.com/spectre-project/spectred).
THE GO VERSION HAS BEEN SUPERSEDED BY THE STABLE RUST VERSION OF SPECTRE.

We encourage developers and blockchain enthusiasts to collaborate, test, and optimize this Rust-based implementation. Your contributions will help shape a platform focused on scalability and speed without compromising decentralization.

## Compilation Requirements

To compile the binaries yourself (if you're unsure whether you need to, you probably don't), you'll need to install [Rust](https://www.rust-lang.org/tools/install).

## Setup

Mining Spectre requires three components:

1. **Node (spectred):** Listens for new blocks.
2. **Miner:** Searches for blocks to report to the node.
3. **Wallet:** For creating and managing wallets.

All three components are standalone executables that require no installation.

You can either download the precompiled binaries or compile the codebase yourself. For most users, downloading the binaries is recommended.

> **Note:** The spectred node and miner must run concurrently, each from a separate console. Do not interrupt them while mining is in progress.

### Download Binaries

The simplest way to get started with Rusty-Spectre is by downloading the binaries from [here](https://github.com/spectre-project/rusty-spectre/releases/latest). After downloading, extract them to a folder.

> **Important:** The rest of this guide assumes installation from source. Before running any command, navigate to the folder where you extracted the binaries:
```bash
$ cd <THE_EXTRACTED_BINARIES_FOLDER>
```

Linux and Mac users may need to prepend commands with `./` to execute the binary. For example:
```bash
./spectred --utxoindex
```

### Build from Source

<details>
<summary>Building on Linux</summary>

1. Install general prerequisites:
    ```bash
    sudo apt install curl git build-essential libssl-dev pkg-config 
    ```

2. Install Protobuf (required for gRPC):
    ```bash
    sudo apt install protobuf-compiler libprotobuf-dev
    ```

3. Install the Clang toolchain (required for RocksDB and WASM secp256k1 builds):
    ```bash
    sudo apt-get install clang-format clang-tidy clang-tools clang clangd libc++-dev libc++1 libc++abi-dev libc++abi1 libclang-dev libclang1 liblldb-dev libllvm-ocaml-dev libomp-dev libomp5 lld lldb llvm-dev llvm-runtime llvm python3-clang
    ```

4. Install the [Rust toolchain](https://rustup.rs/).
   If Rust is already installed, update it with:
    ```bash
    rustup update
    ```

5. Clone the repository:
    ```bash
    git clone https://github.com/spectre-project/rusty-spectre
    cd rusty-spectre
    ```

6. Build the binaries:
    ```bash
    cargo build --release --bin spectred --bin spectre-wallet
    ```
</details>

<details>
<summary>Building on Windows</summary>

1. [Install Git for Windows](https://gitforwindows.org/) or an alternative Git distribution.

2. Install [Protocol Buffers](https://github.com/protocolbuffers/protobuf/releases/download/v21.10/protoc-21.10-win64.zip) and add the `bin` directory to your `Path`.

3. Install [LLVM](https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.6/LLVM-15.0.6-win64.exe) and add the `bin` directory of the LLVM installation (`C:\Program Files\LLVM\bin`) to `PATH`.

   - Set the `LIBCLANG_PATH` environment variable to point to the `bin` directory as well.
   - Rename `LLVM_AR.exe` to `AR.exe` in the LLVM installation directory to address potential C++ dependency issues.

4. Install the [Rust toolchain](https://rustup.rs/).
   If Rust is already installed, update it with:
    ```bash
    rustup update
    ```

5. Clone the repository:
    ```bash
    git clone https://github.com/spectre-project/rusty-spectre
    cd rusty-spectre
    ```

6. Build the binaries:
    ```bash
    cargo build --release --bin spectred --bin spectre-wallet
    ```
</details>

<details>
<summary>Building on macOS</summary>

1. Install Protobuf (required for gRPC):
    ```bash
    brew install protobuf
    ```

2. Install LLVM:
   - The default Xcode installation does not support WASM build targets. Install LLVM from Homebrew:
    ```bash
    brew install llvm
    ```

   - Add the following to your `~/.zshrc` file:
    ```bash
    export PATH="/opt/homebrew/opt/llvm/bin:$PATH"
    export LDFLAGS="-L/opt/homebrew/opt/llvm/lib"
    export CPPFLAGS="-I/opt/homebrew/opt/llvm/include"
    export AR=/opt/homebrew/opt/llvm/bin/llvm-ar
    ```

   - Reload the `~/.zshrc` file:
    ```bash
    source ~/.zshrc
    ```

3. Install the [Rust toolchain](https://rustup.rs/).
   If Rust is already installed, update it with:
    ```bash
    rustup update
    ```

4. Clone the repository:
    ```bash
    git clone https://github.com/spectre-project/rusty-spectre
    cd rusty-spectre
    ```

5. Build the binaries:
    ```bash
    cargo build --release --bin spectred --bin spectre-wallet
    ```
</details>

## Getting Started

Spectred offers various configuration options, but most basic operations require no configuration except for the `--utxoindex` flag (which can be omitted if you're not using the wallet):

```bash
$ spectred --utxoindex
```

Use `spectred --help` to view more running options.

### Creating a Wallet (Optional)

To mine, you'll need a keypair. Start by launching the wallet:

```bash
$ ./spectre-wallet
```

Use the "help" command to explore available options. For instance, to create a wallet:

1. Select a Spectre network:
    ```bash
    $ network mainnet
    ```

2. Create a wallet:
    ```bash
    $ wallet create
    ```

3. Request a new address:
    ```bash
    $ address new
    ```

Your screen will show you something like this:
```
Your default account deposit address:
spectre:0123456789abcdef0123456789abcdef0123456789
```

**Note:** Each time you request an address, a new one will be generated. This is normal, as each secret key can correspond to multiple public addresses.

### Opening Ports

To make your node publicly accessible, forward port 18111 
(or the configured port) to the machine running spectred. Public nodes help the overall health of the network by allowing others to sync with your node.

### Running a Miner (Optional)

**Note:** The Golang-based built-in CPU miner has been significantly outperformed by newer CPU miners. For optimal performance, we recommend using our official [Spectre Rust-Based CPU Miner](https://github.com/spectre-project/spectre-miner) or one of the trusted third-party CPU miners listed below.

Available miners include:
- [Tnn-miner](https://github.com/Tritonn204/tnn-miner/releases)
- [BinaryExpr](https://github.com/BinaryExpr/spectre-miner/releases)
- [TT-Miner](https://github.com/TrailingStop/TT-Miner-release/releases)

To start mining, copy the address generated by your wallet and run the miner with it:
```bash
$ spectreminer --miningaddr spectre:<YOUR_CREATED_ADDRESS>
```

**Note:** Mining will only start after the network is fully synchronized.


Find your node's IP address using `ifconfig` on Linux/Mac or `ipconfig` on Windows.

### Spectre Rust-Based CPU Miner

To enhance your mining performance, consider using the Spectre Rust-based CPU miner, available on [GitHub](https://github.com/spectre-project/spectre-miner).

#### Miner Configuration Options:

- **Mining Address** (`-a`, `--mining-address <MINING_ADDRESS>`): The Spectre address where the miner rewards will be sent.
- **Spectred Address** (`-s`, `--spectred-address <SPECTRED_ADDRESS>`): The IP address of the spectred instance. Default is `127.0.0.1`.
- **Port** (`-p`, `--port <PORT>`): The port used by spectred. Defaults are `18110` for Mainnet and `18210` for Testnet.
- **Threads** (`-t`, `--threads <NUM_THREADS>`): The number of miner threads to launch. By default, this is set to the number of logical CPUs available.

#### Running the Miner

To start mining, use the following command:

```bash
$ spectre-miner --spectred-address <node IP address> --mining-address <wallet address> --threads <NUM_THREADS>
```

If you are running the miner on the same machine as spectred, you can omit the `--spectred-address` flag since it defaults to `localhost`.

**Windows Users:** The CPU miner binary is unsigned, which might cause it to be blocked by Windows Defender. You may need to manually add an exclusion.

**Linux/Mac Users:** If you encounter a "permission denied" error, you can fix it by making the file executable:

```bash
chmod +x <file name>
```

### Development Fund

**Note:** This feature is disabled by default.

The development fund (devfund) is a community-managed fund used to support the ongoing development of Spectre. If you wish to contribute a percentage of your mining rewards to the devfund, you can do so using the following flags:

```bash
./spectre-miner --mining-address=<spectre:XXXXX> --devfund=spectre:qrxf48dgrdkjxllxczek3uweuldtan9nanzjsavk0ak9ynwn0zsayjjh7upez
```

By default, the contribution is set to 1%. To adjust the percentage, use the `--devfund-percent=XX.YY` flag to specify the desired amount.

Here's a more compact version of your guide:

---

### Begin Mining Spectre Using High-Performance Third-Party Miners

#### 1. **Download and Set Up the Stratum Bridge:**
   - Download the [Stratum Bridge](https://github.com/spectre-project/spectre-stratum-bridge/releases).
   - Launch the Bridge using `./spr_bridge` (or `spr_bridge.exe` on Windows). If the bridge and node are on the same machine, the default `config.yaml` will work. Otherwise, update `localhost` in `config.yaml` to the node's IP address.

#### 2. **Install and Start the Miner:**
   - Download the miner for your desired software:
     - [Tnn-Miner](https://github.com/Tritonn204/tnn-miner/releases)
     - [BinaryExpr Miner](https://github.com/BinaryExpr/spectre-miner/releases)
     - [TT Miner](https://github.com/TrailingStop/TT-Miner-release/releases)

#### 3. **Start Mining:**
   - Use the appropriate command for your miner:

     **Tnn-Miner:**
     ```bash
     ./tnn-miner --spectre --daemon-address <ip> --port 5555 --wallet <wallet_address> --threads <threads>
     ```

     **BinaryExpr Miner:**
     ```bash
     ./spectre-miner -d <ip:port> -w <wallet_address> -t <threads>
     ```

     **TT Miner:**
     ```bash
     ./TT-Miner -a spectrex -P <wallet_address>@<ip:port> -cpu <threads>
     ```

   - Replace `<ip>` with the bridge's IP if on a different machine, `<wallet_address>` with your wallet, and `<threads>` with the number of threads you wish to use.
   - Ensure the node is set to allow external connections by running:
     ```bash
     spectred --utxoindex --rpclisten=0.0.0.0:18110
     ```

### Pool Mining Guide

To start pool mining, first select a mining pool from [MiningPoolStats](https://miningpoolstats.stream/spectre). Below are example configurations for two popular pools.

#### Example 1 with Tnn-miner: [tw-pool](https://tw-pool.com/)

```bash
tnn-miner --spectre --daemon-address spr.tw-pool.com --port 14001 --wallet spectre:<YourWalletAddress> --threads <NumberOfThreads> --no-lock --worker <WorkerName>
```

#### Example 2 with TT-miner: [acc-pool](https://spectre.acc-pool.pw/)

```bash
./TT-Miner -a spectrex -P spectre:<YourWalletAddress>.<WorkerName>@acc-pool.pw:18061
```

### Configuration Instructions:

- **Pool Address:** Replace the pool's address and port in the configuration with the one provided by your chosen pool.
- **Wallet Address (`<YourWalletAddress>`):** Replace `spectre:X` with your actual Spectre wallet address.
- **Threads (`<NumberOfThreads>`):** Specify the number of threads you want to dedicate to mining.
- **Worker Name (`<WorkerName>`):** Assign a name to your worker for easy identification in the pool's dashboard.

With these settings configured, you should be ready to start mining in the pool of your choice.


### rusty-spectre Hardware Requirements

**Minimum:**
- 100 GB disk space
- 7th generation i7 4-core processor or AMD equivalent
- 8 GB memory
- 10 Mbit internet connection

**Recommended:**
- 9th generation i7 8-core processor or AMD equivalent
- 16 GB memory
- 40 Mbit internet connection
