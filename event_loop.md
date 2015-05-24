
**** Priliminary thoughts ****
- I/O cost is expensive.

   L1-cache               3 cycles
   L2-cache              14 cycles
   RAM                  250 cycles
   Disk          41 000 000 cycles
   Network      240 000 000 cycles

- Apache is multithreaded: it spawns a thread per request (or process). eat up memory.

- Nginx and Nodej.js are not multithreaded, because threads and processes carry a heavy memory cost. They are single threaded, but event-based.

**** Node.js keeps a single thread for your code ... ****

you can't do parallel code execution.

doing a "sleep" for example will block the server for one second.

If you would have some CUP-intensive code, say, for resizing images, that would still block all other requests.


**** However, everything runs in parallel except your code ****

All I/O is evented and asynchronous.


Having asynchronous I/O is good, because I/O is more expensive than most code and we should be doing something better than just waiting for I/O.


An event loop is "an entity that handles and processes external events and converts them into callback invocations"


CPU-intensive work should be split off to another process with which you can interact as with events.

Answer from Github: 
    + What you need is a task quest! Moving your long running tasks out of the webserver is a GOOD thing.
    + Web server should only be used to "talk" with clients. Heavy load tasks should be delegated to standlone programs (that of course can be also written in JS).
    + Don't spawn a child process for every request (that defats the purpose of node.js). Spawn workers from inside your heavy requests only. Or route your heavy background work to something other than node.js.
    + If you need to scale larger you just make the main server a node cluster and reverse proxy the CPU intensive tasks to other servers built for multi-threaded processing.
    + Node.js should not be used for CPU intensive jobs. Its built for scalability and scalability can be achieved only if you don't blick the single thread of Nojs. As Node.js is single threaded, so blocking it with CPU intensive task will kill the benefits of it.1
 
Solution:

   + node-resque: https://github.com/taskrabbit/node-resque
