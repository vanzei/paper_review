# Objective
## The purpose of this GitHub page is to offer a concise summary of noteworthy technical papers that, in certain instances spanning the last decades, elucidate remarkable use cases and projects.


# Papers

<<<<<<< HEAD
![Figure 1](/Images/GFS-Fig1.png)

Process steps:

First, using the fixed chunksize, the client translates the file name and byte offset specified by the application into a chunk index within the file. Then, it sendsthe master a request containing the file name and chunk index. The master replies with the corresponding chunk handle and locations of the replicas. The client caches this information using the file name and chunkindex as the key. The client then sends a request to one of the replicas, most likely the closest one. The request specifies the chunk handle and a byte range within that chunk. Further reads of the same chunk require no more client-master interaction until the cached information expires or the file is reopened. In fact, the client typically asks for multiple chunks in the same request and the master can also include the information for chunks immediately following those requested. This extra information sidesteps several future client-master interactions at practically no extra cost.
=======
[Original Paper Link](https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf)
>>>>>>> 4fba038 (Feb-3)
