# The Google File System

[Original Paper Link](https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf)

### Authors: Sanjay Ghemawat, Howard Gobioff, and Shun-Tak Leung

## Assumptions:

1. A scalable implementation of distributed storage which certainly levels of scalability, reliability and availability.
   * Google Cloud Storage would run on hundreds / thousand of machines
   * Built on top of inexpensive hardware
     * Failures are considered the norm since baseline hardware could cause system fauld like : Disk Failures, Networking, Memory and CPU issues, human failure and etc. Due to this nature , continuous monitoring is also a requirement.
   * Small and large files are supported
   * Most common workloads are large streaming and small random reads
   * Sequential writes to append data are also expected, randem writes are supported but not optimized for efficency.

## Architecture

GFS decide to have a single master and mutiple chunkservers.
This desegn decision will benefit the system in the long run as explained in further details.

## Single Master

To avoid bottlenecks the read and write operations have to be minimized, which was benefitved of a fixed chunksize choice.
The clients never read from the Master, the master is just responsible for having global knowledge of the chucks and the transfer is made directly from the chunkservers.

## Chunk Size

The decision of using 64MB chunksize impose benefit and some challenges.
### Benefits
   * Reduces clients' need to communicate with the master. Since the chunck size is fixed, a single communication is need to share location of the chunks. Tha helps a lot since most of the read and writes are sequential, but even for randoma ccess the information can be cached by the custoemr.
   * Large chunck size also makes it more likely that the chuncks will be modified in a given chunk, reducing the network overhead.
   * Reduces the metadata size, allowing to store this informarion into memory.
  
### Drawbacks

   * Hot spots can occur since chunck size is quite big, so a big number of small files can be saved in a single place.
   * Hot spots did develop when GFS was first used by a batch-queue system: an executable was written to GFS as a single-chunk file and then started on hundreds of machines at the same time. The few chunkservers storing this executable were overloaded by hundreds of simultaneous requests.
     * A fix for this problem was created by storing such executables with a higher replication factor and by making the batch-queue system stagger application start times. A potential long-term solution is to allow clients to read data from other clients in such situations.

## Metadata

   * The master stores three major types of metadata: the file and chunk namespaces, the mapping from files to chunks, and the locations of each chunk’s replicas. All metadata is kept in the master’s memory

### In Memory Metadata, Chunk Locations and Operations Log

   * Metadata is saved in memory, so its acess is pretty fast and to keep it up to date the decision was to implement periodic scans. Periodic scans also are used to perform garbage collection and re-replciation in case of chunkserver failure.
  
   * Memory limitations are a not so critical concern since the just needs 64 bytes for each 64MB chunk, and some partial chunks + namespace data. Increasing the memory is a small price for this scenario.
  
   * The master does not keep a persistent record of which chunkservers have a replica of a given chunk. It gets the information at stay time and updates its location though the Heartbeat messages.