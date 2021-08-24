# P2P File Sharer (Mini-Torrent)

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

Whenever any peer wants to download a file, it asks the tracker for the list of all the peers that share that file. Once it gets the list of peers, it downloads unique parts of the file from different peers, keeping that in mind that the whole file is not downloaded from a single peer (Why? So that one peer does not have too much load). Here Peice Selection Algo came into picture.

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


