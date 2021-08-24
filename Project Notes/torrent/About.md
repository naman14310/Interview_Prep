# P2P File Sharer (Mini-Torrent)

[Project Link](https://github.com/naman14310/Torrent_Peer2Peer_File_Sharing_System)


## Brief Introduction

This project is based on the concept of Torrent which is basically a peer to peer file sharing network. The user has functionalities like sharing a file, downloading a file, removing a file in a group they belongs to. Download will be parallel with multiple chunks from multiple peers. 

I've also implemented other features such as, creating, leaving, send joining request to some group. The person creating a group will be given admin roles. An admin can view and accept joining request of other members.

This was an abstract view of the project. Can i explain its architecture and working in detail ?


## Architecture

There are two major component in this project, i.e. Tracker and Peer.

**1. Tracker**

Tracker file stores the metadata and information about all groups, users and files, basically a mapping between groups, files and users. I've used only one tracker, but we can use more the one tracker to make it fault Tolerant. To make the system robust, we will maintain persistent data structures in a txt file or noSQL databases (as data will be stored in JSON like structures) so that whenever both trackers go down, we can restore the information when trackers are up again.

![img](https://github.com/naman14310/Interview_Prep/blob/main/Project%20Notes/torrent/tracker.png)


**2. Peer**

A client can either download or share a file with its peers. Whenever a client shares a file it informs the tracker about it which in turn updates its seeder list. After that the client starts acting like a server for its peers that want to download the file.

Whenever any peer wants to download a file, it asks the tracker for the list of all the peers that share that file. Once it gets the list of peers, it downloads unique parts of the file from different peers, keeping that in mind that the whole file is not downloaded from a single peer (Why? So that one peer does not have too much load). Here Peice Selection Algo came into picture. As soon as a piece of file gets downloaded it should be available for sharing. 

![img](https://github.com/naman14310/Interview_Prep/blob/main/Project%20Notes/torrent/Peer%20architecture.png)


## Piece Selection Algorithm 

Piece Selection Algorithm plays an important role in deciding which chunk should be downloaded from which seeder and hence makes parallel downloading more efficient by making use of multiple threads and balancing loads on every node.  

**Algo:**
1. We will sort the received 2D chunk vector on the basis of number of seeders.
2. Create a load vector and initialize it with 0. It'll store load for all seeders. (Load means count of ongoing uploads) 
3. Pick rarest chunk first i.e. the chunk with min number of seeders and increament its load in load vector. (Load vector changes after selecting seeder for every chunk)
4. If there are multiple seeders available then pick seeder with less load vector.  
5. Do this for all chunks and initiate parallel downloading through multithreading.

![img](https://github.com/naman14310/Interview_Prep/blob/main/Project%20Notes/torrent/piece%20selection.png)


## Optimizations that can be done

1. We are not updating seeder's list dynamically, while downloading is going on. 
Solution: Whenever any seeder leaves the room, is should update the seeder's list on tracker and tracker should push that seeder's list on the client peer so that chunk can be downloaded from some other seeder. 

2. Some peers may have more processing power or bandwidth compared to other peers. Can design a system such that those peers receive more piece requests.

3. To make system more robust, we will maintain persistent data structures in a txt file or noSQL databases (as data will be stored in JSON like structures) so that whenever both trackers go down, we can restore the information when trackers are up again.

4. We will work on UI part.

-------

## Interview Questions

#### **Q1. What is the advantage of P2P architecture?**

1. Peer can download multiple chunks from multiple peers simultaneously.
2. Peer can download chunks from another peer which is currently downloading the same file.
3. There is no dependency on a single server or set of servers.


#### **Q2. what are the challenges in sending and receiving files in this system?**

Client (Peer) should be able to download multiple files simultaneously. Seeder should be able to take download request concurrently. Each file had to be downloaded in a separate thread. Handling and Synchronising multiple threads created many challenges for me.


#### **Q3. How did you synchronise multiple threads**

While receiving the chunks from multiple seeders, I've used mutex locks to obtain synchronization.


#### **Q4. What are the problems you have faced while implementing project?**

1. Implementing Piece Selection algorithm was a real challenge to me. I've created nested data structure --> map of {groupId, map of {fileName, seederList for each chunk} } to efficiently represent the information.   
2. Synchronising multiple threads using locks and mutexes was also a tough task.


#### **Q5. Why is synchronization between trackers needed. How did you synchronize tracker?**

Synchronisation between tracker is needed to maintain the consistency of the system. We can achieve it by storing data into persistent data structures like text files or NoSql Databases (MongoDb) so that whenever both trackers go down, we can restore the information when trackers are up again.


#### **Q6. How do you verify that the chunk received is correct ?**

By computing SHA1 (Secure Hashing Algorithm) of every chunk at senders and receivers end. The SHA1 string will uniquely identify each chunk and each file.


#### **Q7. Which system calls are used in your code ?**
1. Pthread for multithreading 
2. Mutex for synchronising multiple threads


#### **Q8. How would you implement this system without tracker ?**

By using Distributed hash tables like pastry and chord. A distributed hash table (DHT) is a decentralized storage system that provides lookup and storage schemes similar to a hash table, storing key-value pairs. Each node in a DHT is responsible for keys along with the mapped values. Any node can efficiently retrieve the value associated with a given key.


#### **Q9. What happens when a download fails ?

The code was written inside the try-catch block, so if in case download fails and control flow reaches inside catch block, that thread will get terminated and this information will be sent to the tracker and seeder list will get updated accordingly. We will also notify user, and user need to again issue the download file command to get whole file in their machine. Since other chunks are downloaded successfully (and information gets updated in tracker), we only need to download the chunk which is not present.


#### **Q10. Does Bittorrent use TCP, or does it also use UDP ?**

Bittorrent is P2P sharing platform. It uses TCP as its transport protocol and uses UDP for control packets. 


#### **Q11. What protocol, sockets uses for communication ?**

TCP


#### **Q12. What is SHA, and What is SHA used for ?**

SHA stands for secure hashing algorithm. SHA is a modified version of MD5 and used for hashing data. A hashing algorithm shortens the input data into a smaller form that cannot be understood by using bitwise operations, modular additions, and compression functions. SHA works in such a way even if a single character of the message changed, then it will generate a different hash.


#### **Q13. What SHA is used for and Why ?**

Primary reason to use SHAs is the uniqueness of all the hash digests. If SHA-2 is used, there will likely be few to no collisions, meaning a simple change of one word in a message would completely change the hash digest. Since there are few or no collisions, a pattern cannot be found to make breaking the Secure Hashing Algorithm easier for the attacker.


#### **Q14. What is difference between hashing and encryption ?**

The only difference between hashing and encryption is that hashing is one-way, meaning once the data is hashed, the resulting hash digest cannot be cracked, unless a brute force attack is used.


#### **Q15. As a user what will I see in the user interface?**

As a user, you get a terminal based menu driven program which will give you options for different commands.


#### **Q16, Explain the working of actual bittorrent software.**

BitTorrent is a peer-to-peer protocol, which means that the computers in a BitTorrent “swarm” (a group of computers downloading and uploading the same torrent) transfer data between each other without the need for a central server. A computer joins a BitTorrent swarm by loading a .torrent file into a BitTorrent client.

Once connected, a BitTorrent client downloads bits of the files in the torrent in small pieces. Once the BitTorrent client has some data, it can then begin to upload that data to other BitTorrent clients in the swarm.

Users downloading from a BitTorrent swarm are commonly referred to as “leechers” or “peers”. Users that remain connected to a BitTorrent swarm even after they’ve downloaded the complete file, contributing more of their upload bandwidth so other people can continue to download the file, are referred to as “seeders”. For a torrent to be downloadable, one seeder – who has a complete copy of all the files in the torrent – must initially join the swarm so other users can download the data. If a torrent has no seeders, it won’t be possible to download.

In recent times, a decentralized “trackerless” torrent system allows BitTorrent clients to communicate among each other without the need for any central servers. BitTorrent clients use distributed hash table (DHT) technology for this, with each BitTorrent client functioning as a DHT node. When you add a torrent using a “magnet link”, the DHT node contacts nearby nodes and those other nodes contact other nodes until they locate the information about the torrent. BitTorrent becomes a fully decentralized peer-to-peer file transfer system. DHT can also work alongside traditional trackers. For example, a torrent can use both DHT and a traditional tracker, which will provide redundancy in case the tracker fails.
