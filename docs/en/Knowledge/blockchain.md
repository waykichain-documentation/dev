## BlockChain Knowledge


#### 1. What is Blockchain?
A blockchain is a distributed database system in which nodes participate. It’s unchangeable, unforgeable, and can be understood as a ledger system. It is an important concept of Bitcoin. It uses an integrated copy of Bitcoin blockchain records with every transaction. With this information, we can find the value of each address, at any point in history. A blockchain is composed of a series of data blocks generated with cryptographic methods. Each block contains the hash of the previous block. Connect form the genesis block to the current block, forming the block chain. Each block is guaranteed to be generated in chronological order after the previous block, otherwise the hash value of the previous block is unknown. By applying a random hash to a set of data existing in the form of block, with a time stamp added, and broadcasted random hash. Obviously, the time stamp can prove that certain data existed at a certain time, because only with it existing can the corresponding random hash value be obtained. Each time stamp should include the previous time stamp in its random hash value. Each subsequent time stamp is reinforcing the previous time stamp, thus forming a Chain.

#### 2. What is Bitcoin?
Bitcoin is a cryptocurrency, a form of digital currency. It is a decentralized digital currency without a central bank or single administrator. In fact, it can be sent from user-to-user on the peer-to-peer bitcoin network without the need for intermediaries.
Transactions are verified by network nodes through cryptography and recorded in a public distributed ledger called a blockchain. Bitcoin was invented by an unknown person or group of people using the name Satoshi Nakamoto and released as open-source software in 2009. Bitcoins are created as a reward for a process known as mining. They can be exchanged for other currencies, products, and services. Research produced by the University of Cambridge estimates that in 2017, there were 2.9 to 5.8 million unique users using a cryptocurrency wallet, most of them using bitcoin.
Bitcoin has been criticized for its use in illegal transactions, it’s high electricity consumption, price volatility, thefts from exchanges, and the possibility that bitcoin is an economic bubble. Bitcoin has also been used as a means of investment, although several regulatory agencies have issued investor alerts about bitcoin.

#### 3. What are the characteristics of blockchain?
Blockchain is a shared distributed database technology. Although there are different wordings of blockchain, the following four technical characteristics are consensual. 1. In a decentralized financial system, there is no intermediary organizations and every node has the same rights and obligations. If any one the nodes stop working it will not have an affect on the operation of the whole system; 2. Trustless: All nodes in the system can conduct transactions without trust because databases and the operations of the entire system are transparent and open. Within the systems rules and time, nodes cannot deceive each other. 3. Collective maintaining: The system is maintained by all nodes that have maintenance functions. All people in the system participate in the maintenance work. 4. Information is irreversible: once the information is verified and added to the blockchain, it will be recorded permanently in chronological order. It will produce irreversible, trustworthy database that prohibits tempering. Therefore, blockchain data’s stability and reliability are extremely high.

#### 4. How does blockchain work?
Just as in the Bitcoin network, miners are tasked with solving a complex mathematical problem in order to successfully “mine” a block. This is known as  “Proof of Work”. Any computational problem that requires orders of magnitude more resources to solve algorithmically than it takes to verify the solution is a good candidate for proof of work. In order to discourage centralization due to the use of specialized hardware (e.g. ASICs), as has occurred in the Bitcoin network, Ethereum chose a memory-hard computational problem. If the problem requires memory as well as CPU, the ideal hardware is in fact the general computer. This makes Ethereum’s Proof of Work ASIC-resistant, allowing a more decentralized distribution of security than blockchains whose mining is dominated by specialized hardware, like Bitcoin.

#### 5. Consensus mechanism
The consensus mechanism is the rule of node competition accounting in the blockchain system. The blockchain is a decentralized ledger. The resulting transactions requires a node to be billed and broadcasted to the entire network. How to choose one of these nodes requires a rule to be selected according to the rules. This rule is the consensus mechanism, and each participating node must follow the consensus mechanism. Different consensus mechanisms are running in different blockchain systems. Some common consensus mechanisms as follows : POW, POS, DPOS.

#### 6. What is decentralization?
In a system with many nodes distributed, each node has a highly autonomous feature. Nodes can be freely connected to each other to form of a new connection unit. Any node can become a phased center, but does not have mandatory central control functions, and no single party controls data or information. The influence between nodes and nodes creates a nonlinear causal relationship through the network. This open, flat, and equal system phenomenon or structure is called decentralization. Each party on the blockchain has access to the entire database and its complete history. People can directly verify the records of their trading partners without the need for an intermediary.

#### 7. Why can‘t data be tampered with?
Modifications to the database by a single or even multiple nodes cannot affect the database of other nodes unless it is possible to control more than 51% of the nodes in the entire network to modify at the same time. This is almost impossible when the node base is large enough. Each transaction in the blockchain is cryptographically linked to two adjacent blocks, so it can be traced back to the past and present of any transaction.

#### 8. What is a smart contract?
From the users’ point of view, a smart contract is usually considered as an automatic guaranteed settlement. For example, when certain conditions are met, the program will automatically execute and release or transfer funds.

#### 9. What is a private key? How to decrypt the signature to complete the transaction?
The private key is the most important part of ensuring the security of your WICC. When you generate a normal private key, it will be saved on your computer or mobile phone. When you use the address corresponding to your private key to trade, you will be required to enter the wallet password, which is actually the process of decrypting the private key. Once you sign in with your private key, the transaction will be broadcasted and the transaction will be completed.

#### 10. What is a wallet?
The wallet is a management tool used to manage tokens’ and their private key. The wallet contains pairs of private and public keys. The user signs the transaction with the private key, thereby demonstrating that the user has the right to export the transaction. The exported transaction information is stored in the blockchain. When the user is using the wallet, both his wallet file information and plain text private key are the wallets. The wallet file is a “locked” wallet , and the plain text private key is the wallet that is completely exposed, and there is no security at all. So when using a plain text private key, be sure to keep it secret.

#### 11.What is address?
Generated by public key, WaykiChain address is a 34-bit string consisting of English letters and numbers that may look like digital gibberish. My WaykiChain address WdPY9EEo1GWYuvyB6qZApdLknGmeXXXXXX, as an example, looks like this. All transfer records for each WaykiChain address can be found through the blockchain explorer. The address is a personal WaykiChain account like your bank account number. Anyone can transfer WICC to you via your WaykiChain address. How do I get my own WaykiChain address then? You can download a WaykiChain Wallet on WaykiChain official website, or register one on trading platforms. Each user’s WaykiChain address is unique. It should be noted that each WaykiChain wallet can only create one address, therefore the wallet mnemonics must be kept carefully.

#### 12.What is the usefulness of block browser?
The Block browser can make it possible for every user to view all information of the block network, including block information, address information, transaction detail information and application-generated record information, thereby providing evidence for blockchain’s openness and transparency.

#### 13.Encryption and digital signature
Asymmetrical encryption is also known as public key cryptography, which is a relatively new method, compared to symmetric encryption. Asymmetric encryption uses two keys to encrypt a plain text. Secret keys are exchanged over the Internet or a large network. It ensures that malicious persons do not misuse the keys. It is important to note that anyone with a secret key can decrypt the message and this is why asymmetrical encryption uses two related keys to boost security. A public key is made freely available to anyone who might want to send you a message. The second private key is kept a secret so that you can only know.

A message that is encrypted using a public key can only be decrypted using a private key, while also, a message encrypted using a private key can be decrypted using a public key. Security of the public key is not required because it is publicly available and can be passed over the internet. The asymmetric key has far better power in ensuring the security of information transmitted during communication.

Asymmetric encryption is mostly used in day-to-day communication channels, especially over the Internet. Popular asymmetric key encryption algorithms includes EIGamal, RSA, DSA, Elliptic curve techniques, PKCS.

#### 14.Development of Block Chain

* Block Chain 1.0

Block Chain 1.0 is composed of digital currency and payment behavior. Its main function is to record transaction information. In Bitcoin. The main features are chain data block mode, all-node sharing and asymmetric encryption.

* Block Chain 2.0

Block Chain 2.0 is Turing-perfect and can develop intelligent contracts and DAPPs, such as CryptoKitties. By replacing manual operation with smart contracts, the process is more transparent and efficient.

* Block Chain 3.0

Block Chain 3.0 is designed to solve the biggest problems of 2.0. If poor performance and low TPS can be solved, blockchain technology will be widely used, for example EOS and WICC.

#### 15.Types of blockchains
Currently, there are three types of blockchain networks - public blockchains, private blockchains and consortium blockchains.

###### Public blockchains
A public blockchain has absolutely no access restrictions. Anyone with an internet connection can send transactions to it as well as become a validator (i.e., participate in the execution of a consensus protocol). Usually, such networks offer economic incentives for those who secure them and utilize some type of a Proof of Stake or Proof of Work algorithm.
Some of the largest, most known public blockchains are Bitcoin and Ethereum.

##### Private blockchains
A private blockchain is exclusive. One cannot join it unless they are invited by the network administrators. Participants and validators access are restricted.
This type of blockchain can be considered a middle-ground for companies that are interested in the blockchain technology in general but are not comfortable with a level of control offered by public networks. Typically, they seek to incorporate blockchain into their accounting and record-keeping procedures without sacrificing autonomy and running the risk of exposing sensitive data to the public internet.

##### Consortium blockchains
A consortium blockchain is often said to be semi-decentralized. It, too, is exclusive but instead of a single organization controlling it, a number of companies might each operate a node on such a network. The administrators of a consortium chain restrict users' reading rights as they see fit and only allow a limited set of trusted nodes to execute a consensus protocol.