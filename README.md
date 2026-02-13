# UDP Log Parser

Listen for UDP packets on multiple sockets. Receiver sorts them into a hashmap by address and pushes the (__time, log string__) tuple to a bounded single-producer-single-consumer queue, a.k.a a ring buffer. When the buffer starts nearing a full capacity, the receiver issues a task to the thread pool. In case of congestion in the pool, the buffer will start overwriting existing data(?). A worker thread will be dispatched to lock and start parsing tuples from the ring buffer. Important analytical results are stored in a SQL database while the original log data is stored in a more efficient format and finally written to a binary log file on the disk.  

## Diagram of the threaded UDP listener

![Diagram of the threaded UDP listener.](/doc/listenudp_chart.png)