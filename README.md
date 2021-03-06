# Zeromq majordomo broker for iServe
## Differences from the official majordomo protocol
This protocol has been slightly modified to enable streaming.
The interaction between a client and a worker in the official protocol is
a simple request-reply one:  
  
```
    client  ---------- request -----------> worker
    client <--- reply [msg1, msg2, ...] --- worker
```
  
However, when the reply consists of several hundren thousand bytes,
for example, a 1024x768 image, and is sent over a slow 3G wireless network,
the whole system becomes vulnerable to timeouts in both
the client and worker side. This modified protocol attempts to solve
this problem by streaming the large payload of the reply:  
  
```
    client  -------------- request -----------> worker
    client <---- reply [a1, a2, ... 'more'] --- worker
    client <---- reply [b1, b2, ... 'more'] --- worker
                             ...
    client <---- reply [n1, n2, ... nn] ------- worker
```
  
Note that trailing 4 byte 'more' string is the signal for more coming bytes.
## References
* official zeromq majordomo documentation http://rfc.zeromq.org/spec:7
* example worker https://github.com/fumin/rubymotion-zeromq
* example client https://github.com/fumin/world
