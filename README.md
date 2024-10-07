# Decentralized Blockchain Implementation in C

## Project Overview

This project implements a decentralized blockchain infrastructure using the C programming language. The blockchain connects three local computers via an intranet and stores ledger data on each device, ensuring a decentralized ledger. The system allows for secure, tamper-proof financial transactions between devices. It features a Proof of Work consensus mechanism using SHA256 hashing, ensuring that only valid blocks are added to the chain.

### Key Features:
- **Peer-to-Peer Communication:** Nodes (computers) communicate directly over a local intranet using socket programming.
- **Decentralized Ledger:** Each node maintains an identical copy of the blockchain, storing transactions securely.
- **Cryptographic Transaction Validation:** Each transaction is signed using a public-private key pair for security.
- **Proof of Work:** Blocks are mined using a cryptographic proof-of-work mechanism, where nodes must find a nonce that generates a SHA256 hash with a set number of leading zeroes.
- **Block Verification:** Before adding a block to the chain, nodes verify the proof of work to ensure the integrity of the blockchain.
- **Fault Tolerance:** Each node stores the blockchain on disk, ensuring persistence across reboots.

## Applications

The project consists of three separate applications, each responsible for different tasks within the blockchain infrastructure:

### 1. **App 1: Transaction Manager**
   - **Functionality:**
     - Displays the current device's IP address.
     - Allows users to add details like the amount of money in disposal, IP address, and name.
     - Initiates transactions (e.g., "A pays B") with validation to ensure sufficient funds.
     - Signs transactions with the user's private key and broadcasts them for cryptographic computation.
     - Option to view transaction histories.
     - Tally the end result of transactions.
   - **Role:** Responsible for managing transactions and broadcasting them to the network for block creation.

### 2. **App 2: Cryptographic Miner**
   - **Functionality:**
     - Listens for transaction broadcasts and begins the cryptographic computation (Proof of Work).
     - Finds a nonce that, when combined with the block’s data, produces a SHA256 hash with 5 leading zeroes.
     - After finding the correct nonce, the miner broadcasts the complete block to the network for verification.
   - **Role:** Handles the mining process and contributes to securing the network.

### 3. **App 3: Blockchain Verifier**
   - **Functionality:**
     - Listens for new blocks broadcasted on the network.
     - Verifies that the block’s proof of work is valid (SHA256 hash starts with 5 zeroes).
     - Checks the validity of transactions in the block (e.g., signature verification, sufficient balance).
     - Adds the block to the blockchain if all checks pass.
   - **Role:** Responsible for maintaining the integrity of the blockchain by verifying and adding new blocks.

## Project Architecture

1. **Network Communication:**
   - Each node communicates via sockets over a local network, using peer-to-peer connections to broadcast transactions and blocks.
   
2. **Blockchain Structure:**
   - Blocks contain:
     - Index (position in the chain)
     - Timestamp
     - Transactions (signed data including sender, receiver, and amount)
     - Hash of the previous block
     - Nonce (Proof of Work)
     - SHA256 hash for the block.
   
3. **Proof of Work:**
   - Nodes compete to find a valid nonce that generates a SHA256 hash with 5 leading zeroes, ensuring computational difficulty.

## Installation and Setup

### Prerequisites:
- C Compiler (GCC recommended)
- OpenSSL (for SHA256 and cryptographic functions)
- Unix-like operating system (Linux, macOS) for socket programming

### Build Instructions:
1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/blockchain-c-project.git
   cd blockchain-c-project
   ```
   
2. Compile each app using the Makefile:
   ```bash
   make all
   ```

3. Run the executables on three separate machines or terminals:
   - **App 1:**
     ```bash
     ./transaction_manager
     ```
   - **App 2:**
     ```bash
     ./crypto_miner
     ```
   - **App 3:**
     ```bash
     ./blockchain_verifier
     ```

4. Set up IP addresses and network connections by either manually entering peer IPs or allowing auto-discovery (if implemented).

### Usage:
- **App 1 (Transaction Manager):** Follow the terminal prompts to input your device details and initiate transactions. Transactions will be signed and broadcasted for mining.
- **App 2 (Cryptographic Miner):** Once a transaction is received, the miner will attempt to find a valid nonce for the block.
- **App 3 (Blockchain Verifier):** When a block is mined and broadcasted, this app will verify and add the block to the blockchain.

### Files:
- **src/**: Contains the source code for all applications.
- **include/**: Header files defining the data structures and functions used in the project.
- **Makefile**: For compiling the executables.

## Future Enhancements
- **Automatic Peer Discovery:** Implement a multicast or broadcast mechanism for automatic detection of other nodes in the network.
- **Dynamic Proof of Work Difficulty:** Adjust the difficulty of mining based on the average time taken to mine a block.
- **Consensus Algorithm:** Implement a more robust consensus mechanism, such as majority voting on block acceptance.
- **GUI Interface:** Add a graphical user interface for easier interaction with the system.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements
The project was developed by [Mihir Shah](https://www.github.com/mihirpkshah), [Ved Mundhada]() and [Pratham Mundada]() as part of a DSA mini project in Semester 2, Second Year at COEP University.

This project is inspired by the concepts of decentralized finance and blockchain technology, with a focus on providing a simple, educational implementation in C.

Videos Watched for references:

[But how does bitcoin actually work? by 3Blue1Brown](https://www.youtube.com/watch?v=bBC-nXj3Ng4)

[How secure is 256 bit security? by 3Blue1Brown](https://www.youtube.com/watch?v=S9JGmA5_unY)

