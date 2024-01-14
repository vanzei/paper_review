# Objective
## The purpose of this GitHub page is to offer a concise summary of noteworthy technical papers that, in certain instances spanning the last decades, elucidate remarkable use cases and projects.


### First Paper

**The Google File System**

[Original Paper Link](https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf)

**Authors:** Sanjay Ghemawat, Howard Gobioff, and Shun-Tak Leung

Assumptions:

1. A scalable implementation of distributed storage which certainly levels of scalability, reliability and availability.
   * Google Cloud Storage would run on hundreds / thousand of machines
   * Built on top of inexpensive hardware
     * Failures are considered the norm since baseline hardware could cause system fauld like : Disk Failures, Networking, Memory and CPU issues, human failure and etc. Due to this nature , continuous monitoring is also a requirement.
   * Small and large files are supported
   * Most common workloads are large streaming and small random reads
   * Sequential writes to append data are also expected, randem writes are supported but not optimized for efficency.

**Architecture**

GFS decide to have a single master and mutiple chunkservers.
This desegn decision will benefit the system in the long run as explained in further details.

**Single Master**

To avoid bottlenecks the read and write operations have to be minimized, which was benefited of a fixed chunksize choice.
The clients never read from the Master, the master is just responsible for having global knowledge of the chucks and the transfer is made directly from the chunkservers.

Process steps:

First, using the fixed chunksize, the client translates the file name and byte offset specified by the application into a chunk index within the file. Then, it sendsthe master a request containing the file name and chunk index. The master replies with the corresponding chunk handle and locations of the replicas. The client caches this information using the file name and chunkindex as the key. The client then sends a request to one of the replicas, most likely the closest one. The request specifies the chunk handle and a byte range within that chunk. Further reads of the same chunk require no more client-master interaction until the cached information expires or the file is reopened. In fact, the client typically asks for multiple chunks in the same request and the master can also include the information for chunks immediately following those requested. This extra information sidesteps several future client-master interactions at practically no extra cost.
