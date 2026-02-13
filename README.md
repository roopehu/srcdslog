# UDP Log Parser

Listen for UDP packets on multiple sockets, sort them by address and push the log string to a bounded single-producer-single-consumer queue, a.k.a a ring buffer. When the buffer starts nearing a full capacity, the receiver issues a task to the thread pool. In case of congestion in the pool, the buffer will start overwriting existing data(?). A worker thread will be dispatched to lock and start parsing log strings from the ring buffer. Important analytical results are stored in a SQL database while the original log strings are stored in a more efficient format and finally written to a binary log file on the disk.  

## Diagram of the threaded UDP listener

![Diagram of the threaded UDP listener.](/doc/listenudp_chart.png)