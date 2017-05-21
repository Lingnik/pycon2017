# Asynchronous Python for the Beginner
* presenter: Miguel Grinberg  @miguelgrinberg
* Flask developer
* Async project for socket.io
* "Async makes your code go fast"

## Definition
* "**One** style of concurrent programming" -- Doing many things at once

## How does Python do multiple things at once?
* Use multiple processes.
    * The OS doe sall the multi-tasking work
    * Only option for multi-core concurrency
* Use multiple threads.
    * A line of execution, like a process, but can have multiple threads in the context of the process.
    * The OS deoes all the multi-tasking work
    * In CPython, the GIL prevents multi-core concurrency. End up working in a single core no matter how many you have.
* Use asynchronous programming.
    * No OS intervention.
    * One process, one thread in that process.
    * What's the trick?

## Chess analogy
* Chess exhibition--player plays lots of games of chess against a lot of people.
* Synchronous- if each game takes 30 minutes, she'll be there for 12 hours.
* Asynchronous- walks to next board after each move, she'll be there for 1 hour.
* (He makes it sound like round-robin polling.)

## A practical definition
* The tasks that are running release the CPU during waiting periods, so that other tasks can use it.

## How?
* Have a Function that can Suspend and Resume
    * Async functions need the ability to suspend and resume
    * A function that enters a waiting period is suspended, and only resumed when the wait is over
    * Four ways to implement suspend/resume in Python without OS help
        1. Callback functions- pretty gross.
        2. Generator functions
        3. Async/await (Python 3.5+)
        4. Greenlets (3rd party, requires greenlet package)- c extensions package.
* Scheduling Asynchronous Tasks
    * See how the CPU is shared
    * Need a scheduler
    * Usually called "event loop"
    * Loop keeps track of all the running tasks
    * When function suspends, return controls to the loop, which then finds another function to start or resume
    * This is called "cooperative multi-tasking"

## Examples
* https://bit.ly/asyncpython

### Example: Standard (synchronous) Python
* def hello(): print; sleep(3); print;
* For loop on function: takes N * 3s.

### Example: Asyncio

    import asyncio
    loop = asyncio.get_event_loop()

    # Option 1: asyncio.coroutine/yield from asyncio.sleep()
    @asyncio.coroutine
    def hello1():
    print()
    yield from asyncio.sleep(3)
    print()

    # Option 2: async/await
    async def hello2():
        print()
        await asyncio.sleep(3)
        print()

    if __name__ == '__main__':
        loop.run_until_complete(hello1())
        loop.run_until_complete(hello2())

### Example: Greenlet Frameworks

gevent:

    from gevent import sleep

    def hello():
        print()
        sleep(3)
        print()

    if __name__ == '__main__':
        hello()

eventlet:

    from eventlet import sleep

    def hello():
        print()
        sleep(3)
        print()

    if __name__ == '__main__':
        hello()

* Loop is running underneath implicitly from x.sleep()

## Pitfalls
* CPU Heavy Tasks
    * Long CPU-intensive tasks must routinely release the CPU to avoid starving other tasks
    * This can be done by "sleeping" periodically, such as once per loop iteration
    * To tell the loop to return control back as soona s possible, sleep for 0 seconds
    * Example: `await asyncio.sleep(0)`
* Async and the Python Standard Library
    * Many functions in PSL are designed as blocking functions
    * These blocking library functions are incompatible with **every** async framework:
        * socket.*, select.*
        * subprocess.*, os.waitpid
        * threading.*, multiprocessing.*
        * time.sleep*
    * All async frameworks provide non-blocking replacements for these
    * Eventlet and Gevent can "monkey-patch" the standard library to make it async compatible
    * Can't do this with asyncio.

## Conclusion

### Processes vs. Threads vs. Async

* Not much is better than async.

| Category                           | Processes         | Therads           | Async                 |
| ---------------------------------- |:-----------------:|:-----------------:|:---------------------:|
| Optimize waiting periods           | Yes (preemptive)  | Yes (preemptive)  | Yes (cooperative)     |
| Use all CPU cores                  | Yes               | No                | No                    |
| Scalability                        | Low (ones/tens)   | Medium (hundreds) | **High (thousands+)** |
| Use blocking std library functions | Yes               | Yes               | No                    |
| GIL interference                   | No                | Some              | No                    |

### Reasons to use Async
* Massive scaling.
* Avoids going bankrupt in hosting.
