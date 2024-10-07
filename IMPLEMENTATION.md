## Mihir - Requirement Identification

Legders + Digital Signatures

Everyone maintains thier own ledger

New Entry will be broadcasted

Allow ledger to add → If computation is completed, cryptic hash function!

Proof of work is required to complete the verification

DSA → Singly Linked-List → But instead of “Links” it will be computed hashes

Program End points:

- Listen to all the block events online
    - Verify authenticity
    - Broadcast authenticity
    - Add data to blockchain
- Actions
    - Add New Entry
    - Broadcast new Block to online computers

Two types of Broadcasts:

- Broadcast Start Mining (Finding hash for a block)
- Broadcast hashed-verified (proof of work) to add to all the ledgers!

## CHATGPT - Requirement Identification

### 1. **Node Communication via Intranet**

- **Socket Programming:** Use socket programming to connect the 3 computers over a local intranet. Each node should act as both a client and a server, receiving and sending messages.
- **Peer Discovery:** Implement a discovery mechanism to ensure each computer knows the other nodes in the network.

### 2. **Blockchain Data Structure**

- **Block Structure:**
    - **Block Header:** Include the previous block hash, timestamp, nonce, and merkle root.
    - **Transaction Data:** Store details of transactions like sender, receiver, amount, and digital signatures.
- **Blockchain:** Maintain a list of blocks, where each new block contains the hash of the previous block, creating a chain.

### 3. **Transaction Mechanism**

- **Ledger Implementation:**
    - Each node should have a decentralized copy of the ledger, storing details of all transactions.
    - The terminal interface will allow sending "money" by adding a transaction to the pool.
- **Digital Signatures:**
    - Use RSA or ECC for signing transactions, ensuring that only valid users can initiate transactions.

### 4. **Proof of Work (PoW)**

- **SHA256 Implementation:** Use a cryptographic library in C to compute SHA256 hashes.
- **Mining Process:**
    - Nodes solve a Proof of Work challenge (finding a SHA256 hash with 10 leading zeroes).
    - After a successful hash, the block is created and broadcasted to all nodes for verification.

### 5. **Block Verification and Consensus**

- **Broadcast Protocol:**
    - Use a broadcasting system to share the newly mined block to all connected nodes.
    - All nodes must verify the legitimacy of the block, checking the hash, nonce, and previous block reference.
- **Consensus:**
    - Implement a consensus mechanism like the longest chain rule to resolve conflicts if two nodes mine blocks simultaneously.

### 6. **Terminal Interface**

- **User Interface:** Design a simple text-based terminal interface for users to:
    - View connected devices/nodes.
    - Send money to other nodes (create a transaction).
    - View the blockchain (view transaction history and block details).
- **Broadcast Transactions:** The terminal should broadcast the transaction to all nodes, where it is added to the transaction pool waiting to be mined.

### 7. **Persistence and Fault Tolerance**

- **Data Storage:** Store the blockchain ledger on disk for each node. If a node goes down and restarts, it should reload its local copy of the blockchain and sync any new blocks from other nodes.
- **Rebroadcast Mechanism:** When a node reconnects to the network, it can request recent blocks from peers to stay updated.

---

### Example Architecture:

1. **Node Setup:**
    - Node 1 (192.168.1.1), Node 2 (192.168.1.2), Node 3 (192.168.1.3)
    - Each node runs a C program with server and client capabilities (socket communication).
2. **Block Example:**
    
    ```c
    struct Block {
        int index;
        time_t timestamp;
        char previousHash[64];
        char hash[64];
        int nonce;
        Transaction transactions[NUM_TX];
    };
    
    ```
    
3. **SHA256 Hashing for Proof of Work:**
    - Use the `libcrypto` library for hashing (SHA256).
    - Run a loop, incrementing the nonce until a valid hash with 10 leading zeroes is found.
4. **Consensus Mechanism (Longest Chain Rule):**
    - If two nodes generate competing blocks, the node that extends its chain with the next block first wins.

### Libraries and Tools:

- **Socket Programming:** `sys/socket.h`
- **SHA256 Hashing:** `libcrypto` (OpenSSL) or custom implementation.
- **Threading:** `pthread.h` to handle concurrent mining and network communication.
- **Digital Signatures:** Use `libcrypto` for signing transactions.

This should give you a strong starting point to build a decentralised blockchain infrastructure in C. What part of the project would you like to dive into further?

## Mihir - Implementation Plan

---

APP 1:

1. View Current Device IP addr
    1. And add all other IP address to the network (???)
2. Add Current Details - Amount of money in disposal, IP address, Name etc.
3. Ability to ‘A pays B’ add transactions like that
    1. Pre check validation - If enough money is present
    2. If valid then add a signature and send this block for Cryptographic Computation
    3. this Broadcast is send
4. Option to View Transaction Histories
5. End Result Tally Option

APP 2:

1. Receive Broadcast for doing ‘Cryptographic Computation’
2. Start finding a ‘Proof of Work’ [and integer] such that The whole blocks SHA256 starts with 5 zeroes
3. After finding a number broadcast this complete block for adding it to Block-chain

APP 3:

1. Listen for ‘Adding transaction` in block chain
2. Verify the Proof of Work → That its SHA265 starts with 5 zeroes
3. If successful then add it to blockchain

## ChatGPT Suggestion of Implementation Plan

Your implementation plan is clear and well-structured. Splitting the tasks across three separate executables makes it modular and easier to manage. Here are some suggestions to fine-tune or improve specific areas:

### Improvements & Suggestions:

### **APP 1: Managing Network Addresses (1.1)**

- Instead of manually adding IP addresses, you could implement a **discovery protocol** using broadcasting/multicasting, where each node announces itself to the network. This way, nodes can automatically find others, making the system more dynamic and less dependent on manual configuration.

### **APP 1: Validating Transactions (3.1 & 3.2)**

- For **pre-check validation** on available money, ensure that you maintain a local ledger (or a balance state) for each participant.
- When **adding a signature**, use a strong public-private key cryptography scheme (like RSA or ECC) to ensure transaction authenticity. You can use **OpenSSL** for this.

### **APP 2: Proof of Work Optimization (2.2)**

- When finding the proof of work, you could add **multi-threading** using `pthreads` to parallelize the computation and speed up the search for the correct nonce.
- Consider implementing a **difficulty adjustment mechanism** if you're planning to extend the system further, where the target number of leading zeroes can dynamically change based on block generation time.

### **APP 3: Consensus Mechanism (3.3)**

- To strengthen consensus, you could implement a **validation quorum** where a block must be verified by a majority of nodes before being added to the blockchain. This way, a rogue node can't easily manipulate the chain.
- Additionally, instead of just verifying the proof of work, ensure that the **transaction validity** is also checked (i.e., verifying signatures and that the sender has enough balance) before adding the block to the chain.

### **General Enhancements:**

1. **Persistent Storage:**
    - For robustness, ensure that each node stores a **persistent copy** of the blockchain to disk, so that on reboot or failure, the node can reload its state and sync up with the network.
2. **Broadcasting Mechanism:**
    - When broadcasting transactions or blocks, use **message queues** or **UDP broadcasting** to efficiently handle communication between nodes. Implement retries or acknowledgments to ensure all nodes receive the messages.
3. **Error Handling:**
    - Add proper error handling and timeouts in cases where one node fails to respond, ensuring that the system remains fault-tolerant.

### Optional Enhancements:

- **App Monitoring and Logging:**
    - Add a basic logging mechanism to track key actions (transactions, proof of work attempts, block additions) in real-time for debugging and monitoring the blockchain's state.

With these suggestions, your architecture should scale well and be more robust for real-world testing. Would you like to discuss any specific part of the implementation in more depth?
