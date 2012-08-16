# Zeromq majordomo broker for iServe
## Differences from the official majordomo protocol
This protocol has been slightly modified to enable streaming.
The interaction between a client and a worker in the official protocol is
a simple request-reply one:  
  
```
    client  ---------- request -------> worker
    client <--- reply [msg1, msg2, ...] --- worker
```
  
However, when the reply consists of several hundren kB,
and is sent over a slow 3G wireless network, the whole system becomes
vulnerable to timeouts in both the client and worker side. This modified
protocol attempts to solve this problem by streaming the large payload of
the reply:  
  
```
    client  -------------- request -----------> worker
    client <---- reply [a1, a2, ... 'more'] --- worker
    client <---- reply [b1, b2, ... 'more'] --- worker
                             ...
    client <---- reply [n1, n2, ... nn] ------- worker
```
  
Note that trailing 4 byte 'more' string is the signal for more coming bytes.
## Reference
http://rfc.zeromq.org/spec:7
