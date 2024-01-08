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
