
# UFO Assignment 3

## Links

- [Assignment definition](https://datsoftlyngby.github.io/soft2019fall/UFO/03-Prototyping_and_Evaluation.html#exercise-for-hand-in)


## Problem

There have been complaints about from clients from US and Asia about the software client being non-responsive. This has not been observed by clients located geographically closer to the server, which is located in Frankfurt, Germany. Since there are no other known similarities between the two clients that reported issues, we expect the distance to be the most important factor. However, in terms of computer networks, the distance is not as significant as the number of intermediary points between the two systems. These points, often called routers, are responsible for where to send the data next. **We believe that the number of hops the data packet has to go through and the performance of the individual routers impacts overall latency the most. Specifically, the more hops there are, the more likely it is to take longer**.

## Experiment

### Description

End devices, such as our software server and client are unable to control the route that their messages will take through the internet. This is the task done by routers, which may take into account factors such as priority of the packet, current load(for example, if a lot of TCP packets are waiting for handling, UDP packets may be dropped/delayed because of it). We have no control over these factors, which may skew the results. Even so, it should be possible to observe how latency changes between different number of hops. 
To get the number of hops between two computer systems we use a common utility program called [`traceroute`](https://linux.die.net/man/8/traceroute). Specifically in this experimenet we will use version X of this program runnin on Ubuntu 18.04.3(LTS) which all our servers will be running. This program will list all the routers in between two systems and the time it took to get to each one. As mentioned before, this route may be different every time due to how the internet works, but overall should give a rough estimate.

### Execution

We used [DigitalOcean](digitalocean.com) to set up four droplets(servers in DigitalOcean terms) with the following configurations

| Server number | Location | Address during the experiment | Linux Distribution | Traceroute version | DigitalOcean Resources |
|--------|----------|-------------------------------|--------------------|--------------------|------------------------|
| 0      | Frankfurt, Germany | 167.172.175.54 | 18.04.3 (LTS) x64 | NA | 1GB RAM, 1 CPU, 25GB SSD, 1000GB transfer |
| 1      | New York, USA | 167.99.0.175 | 18.04.3 (LTS) x64  | 2.1.0 | 1GB RAM, 1 CPU, 25GB SSD, 1000GB transfer |
| 2      | Singapore, Malaysia | 165.22.97.123 | 18.04.3 (LTS) x64 | 2.1.0 | 1GB RAM, 1 CPU, 25GB SSD, 1000GB transfer |
| 3      | Amsterdam, Netherlands | 192.81.220.164 | 18.04.3 (LTS) x64 | 2.1.0 | 1GB RAM, 1 CPU, 25GB SSD, 1000GB transfer |

Servers 1, 2 and 3 had `traceroute` installed using the following commands:

```
apt-get update
apt-get install traceroute
```

### Results

As seen in the table below, the number of hops is very similar, no matter how far the server is. What we can observe however, is that the duration of the 2-4 hops is significantly longer for servers 1 and 2.

| Server number  | Number of hops | Traceroute output | Overall average duration* |
|----------------|----------------|-------------------|---------------------------|
| 1              | 5              <td><pre>traceroute to 167.172.175.54 (167.172.175.54), 30 hops max, 60 byte packets <br> 1  167.99.0.253 (167.99.0.253)  7.022 ms 167.99.0.254 (167.99.0.254)  1.559 ms  1.536 ms <br> 2  138.197.251.80 (138.197.251.80)  1.252 ms 138.197.251.68 (138.197.251.68)  1.498 ms 138.197.251.64 (138.197.251.64)  1.465 ms <br> 3  138.197.248.71 (138.197.248.71)  84.804 ms 138.197.244.135 (138.197.244.135)  84.738 ms 138.197.248.71 (138.197.248.71)  84.776 ms <br> 4  138.197.244.135 (138.197.244.135)  84.726 ms 138.197.250.175 (138.197.250.175)  84.702 ms  84.622 ms <br> 5  167.172.175.54 (167.172.175.54)  85.463 ms 138.197.250.175 (138.197.250.175)  84.663 ms 167.172.175.54 (167.172.175.54)  85.537 ms </pre></td> | 257 MS
| 2              | 6              <td><pre>traceroute to 167.172.175.54 (167.172.175.54), 30 hops max, 60 byte packets <br> 1  165.22.96.254 (165.22.96.254)  2.696 ms  2.688 ms  2.657 ms <br> 2  138.197.250.252 (138.197.250.252)  2.620 ms 138.197.250.250 (138.197.250.250)  2.608 ms 138.197.250.254 (138.197.250.254)  2.573 ms <br> 3  138.197.245.4 (138.197.245.4)  159.132 ms  159.115 ms 138.197.245.0 (138.197.245.0)  159.178 ms <br> 4  138.197.244.161 (138.197.244.161)  159.024 ms  159.048 ms  159.028 ms <br> 5  138.197.250.169 (138.197.250.169)  159.432 ms  159.419 ms  159.398 ms <br> 6  167.172.175.54 (167.172.175.54)  159.829 ms  157.471 ms  157.412 ms</pre></td> | 640 MS
|  3             | 6             <td><pre>traceroute to 167.172.175.54 (167.172.175.54), 30 hops max, 60 byte packets <br> 1  146.185.156.253 (146.185.156.253)  1.021 ms  0.961 ms  0.927 ms <br> 2  138.197.250.18 (138.197.250.18)  0.849 ms  0.787 ms 138.197.250.14 (138.197.250.14)  6.425 ms <br> 3  138.197.244.75 (138.197.244.75)  7.075 ms  7.404 ms 138.197.244.79 (138.197.244.79)  13.364 ms <br> 4  138.197.244.68 (138.197.244.68)  6.869 ms  6.972 ms  6.993 ms <br> 5  138.197.250.175 (138.197.250.175)  12.275 ms  12.260 ms  12.578 ms <br> 6  167.172.175.54 (167.172.175.54)  7.140 ms  6.652 ms  6.550 ms</pre></td> | 46 MS 


## Conclusion

As seen from our _primitive_ experiment we can see that the number of hops a data packet takes, has little meaning. On the other hand, the quality of the connection to the router and its own performance seem to suggest that they are causing the major issue.