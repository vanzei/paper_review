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

