Listen for UDP packets on multiple sockets, sort them by address and eventually when the buffer is full, store them on disk in a binary log format

![Diagram of the threaded UDP listener.](/doc/listenudp_chart.png)