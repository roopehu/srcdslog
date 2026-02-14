# Parse Source Dedicated Server logs

Listen for UDP packets on multiple sockets. Receiver sorts them into a hashmap by address and pushes the (__time, log string__) tuple to a bounded single-producer-single-consumer queue, a.k.a a ring buffer. When the buffer starts nearing full capacity, the receiver issues a task to the thread pool. In case of congestion in the pool, the buffer will start overwriting existing data(?). A worker thread will be dispatched to lock and start parsing tuples from the ring buffer. Important datapoints are stored in a SQL database while the original log data is stored in a custom (lossy) format. Finally, all temporary data is written to a binary log file on the disk to be possibly retrieved later, until eventually getting stored in a semi-permanent archive (S3, .tar.gz etc).  

## Diagram of the UDP log parsing code flow

![Diagram of the threaded UDP listener.](/doc/listenudp_chart.png)