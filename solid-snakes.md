Solid Snakes or: Hot to Take 5 Weeks of Vacation
presenter: Hynek Schlawack

"My last year" -- Vacationed in the U.S. for 5 weeks. People kept asking: how'd you do it?

1. Attitude
You will have to spend a significant amount of time on non-features.
It all comes down to incentives.
Your manager will call everything the "first" priority.

Reliable systems do take time, and if your only performance metric is "shipping new features"... there's a solid business case for reliabile systems.
The fewer urgent interruptions you have, the more you can work on important stuff.

Sir C.A.R. Hoare: The price of reliability is the pursuit of the utmost simplicity.
Do not conflate simple and easy. Coming up with simple solutions is everything but easy.

What is complexity? The # of concepts you have to keep in mind if you're trying to reason about the behavior of a system.
People have a finite number of things they can juggle.

When a system is tightly coupled, misbehavior in one place may break the whole system.


Two types of complexity.

**Essential Complexity**

**Accidental Complexity**
* Something you do to yourself, e.g.
 * Complex employment behaviors.
 * Wrong abstractions.
 * Inadequate tools.

**What is simple software?**
Normal accidents come from tightly coupled systems. Ravioli is not tightly coupled at all. They know nothing about other members of the system.

"Dependencies will kill you." - The dependencies between objects. Bidirectional dependencies are bad.

**Bad Design**
"God objects" - an object that knows too much and does too much. You need a bunch of other objects to do something. If you need 10 fakes to test something, you probably aren't really testing anything.

God objects are pretty common in Python. Subclassing, for example. It was meant for specialization, not code sharing.

Adding subclassing to architecture makes it more complex, it's easier to write, but you may end up with namespace confusion and scattered logic. Don't start your project by drawing hierarchies because that's what you've been taught.

Meta classes are also powerful but overuse.


Writing classes is tedious. "I wrote attrs"

'namedtuples are not made as a class substitution' -- they're tuples with names. if a function returns a tuple, it's easier to used if it's named. people started using them for classes.


**Operational Complexity**

"Distributed systems are hard."
(diagram)
Lots of single points of failure. (red boxes and red arrows)

If you run at scale, rare events are common.

**Microservices**

Lots more red boxes and red arrows.
Message busses? Tracing?

What you want/need are boundaries. Interfaces.

Kubernetes, etc, are pretty easy to set up. But do you understand it? Do you understand all the pieces under it?

You can't run it on the side. You can start it on the side, but it's not sustainable.

**Plan for Stupidity**
People act stupid sometimes, e.g. sometimes they're sleep deprived.

John Allspaw: "I don't believe in human error." If a human causes an outage, it's usually because the system failed them, not malice or ignorance.

S3 outage was caused by a human using a tool the wrong way, but the postmortem focused on improving the system.

If it takes one API call to delete all their data, they will eventually use it.

**Illegal State**
An intersection of sharp edges and complexity: Class instances that end up in an illegal state.

Solution? Make illegal state unrepresentable.

Do two things: objects have to be valid after initialization, secondly, prevent them from mutating into something illegal.

NO PARTIAL INITIALIZATION:

    conn = Connection()
    conn.tls = True
    conn.connect("host.name")

Alternative: classmethod factories:

    conn = Connection.connect(
        "host.name",
        tls=True
    )

Treat __init__ as private APIs and provide factories to create and return instances.

Alternative: builder pattern:

    conn = ConnectionBuilder() \
        .for_hostname("host.name") \
        .with_tls(True) \
        .connect()

**Prevent Mutation**
Properties and state machines are fiddly. Don't give your users an API to modify objects, but give them a way to create a new object based on this object.

**Data Validation**
Be conservative both at sending and receiving.

Invalid data wanders all through your system.

* Data Validation at Edges- validate as soon as possible
* Data Normalization at Edges- give it a simple, canonical form

Don't pass strings around. Parsers are evil. Use first class objects.


## Failure is Inevitable
Failure is part of your life whether you like it or not.

Minimize risk, prepare for it, then deal with it.

Based entirely on: Containment and recovery.


**Expect Failure**
* Check for outages
* Instrument your systems
* Have solid reporting

Silent failures are the worst.

Prometheus & Sentry are great for reporting & error reporting. Open source.

Local errors are easy. (Exception is raised and you deal with it.)

Remotely? You're lucky if it fails right away. What's worse is nothing happens--so put a timeout on it.

Noone is immune from timeouts.

What do you do when you run into timeouts? Do you carry on (and slow down)?

**Circuit breaker pattern**
* Call the circuit breaker, it gives a result.
* Circuit breaker calls the API, times out.
* If the circuit breaker is open, allow requests.
* If the circuit breaker is closed, deny requests immediately and don't hit the API.
    * Periodically probe the remote API.

**Redundancy**
* "2 is 1, 1 is none" --> you should have 3.
* This principle made the internet almost unbreakable, except too many people are using the same DNS provider or the same storage in the same region.

* Multiple routers
* Server heartbeats
* Datacenter/AZ distribution
* Backups
    * If a fire is enough to take you down, you won't have backups.
    * Test your backups.

**Docs**
* If you want to be indispensible, be a knowledge silo.
* If someone asks you something, write it down.
* If you have a standard task or procedure, write it down.
* Don't rely on remembering things. WRITE IT DOWN.

**Deal with Failure**
What do you do when your database is down? Your web service is returning invalid responses?
* Don't make it any worse. Avoid cascading failure unrelated to your original failure.
* RETRIES. Retries are essential in a distributed system. But they can DDoS yourself. So you have to back off.
    * How long? A second? A minute? Hours?
    * All of them. Good: Start with a short period, raise exponentially.
    * Better: Exponential backoff with jitter. Space them off so the backoff period expiration doesn't melt your system.
    * Simple solution: Only retry at the "top".
    * Alternative: Per-request retry budgets.
    * Also: Add a back-pressure signal to indicate you're overloaded.
    * Avoid unbounded queues.

**Don't Swallow Errors**
* Add error reporting.
* Don't do this:

    try:
        do_something()
        return True
    except:
        return False

* Do this:

    try:
        do_something()
    except Exception as e:
        raise AppException() from e
        AppException().__cause__ == e

* If you write a library, just let exceptions happen. Your users will know what to do with it.

**Don't Try Too Hard**
* Too much complexity is a bad thing.

    sys.exit(1)

* **Crash-Only Software**

** Fail Fast, Fail Loudly**
* A crash is much better than a hang. (e.g. a quick 500)
* 500 after 30s vs browser timeout after 3m
* Show an informative stacktrace
* Redis takes it to a higher level: gives you a stack trace **and** runs a quick memory test.

**Focus on Recovery**
* MTTR reigns supreme.
* If you accept the failure happened and write crash-only software, it becomes much less important whether it goes down, much more important that it comes up again fast.
* No humans can be allowed when restoring your service.
* You have to have zero expectations about what is already present and why you are starting up.
* e.g. your container should just try its db connections, then retry w/ backoff.


##
* Built Fault Tolerant Systems that recover automatically and switch off your phone.
