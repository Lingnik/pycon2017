Dr. Microservices, Or How I Learned to Stop Worrying and Love the API
presenter: Ryan Anguiano

# Where to Start
* Don't start a new project with microservices
* Hard to pivot from business standpoint
* Core of project (core business logic) should be rigidly defined
* Switch to microservices because you need to, not because you want to

# The Monolith
* Very large Django app
* Old Python
* Dependency Hell (80+ lines requirements.txt)
* Deployment interrupts entire app

# Breaking up the Monolith
* Analyze your data flow
* Divide application into logical services
* Leave complicate business logical intact
* (missing)

# Analysis Diagram
(missing)
* Want core logic to be as small and contained as possible
* Break little bits off

# Building a Migration Roadmap
* Evaluate different tools- there are lots of them
* Use solutions that best meet your needs
* You can keep your existing project running alongside new services

* Pitched MANTL for folks who have no idea where to start
* Easy to get up and running, built by Cisco

# Services and API Design
* Standardize two methods of communication
    * Synchronous- need a response right away
    * Asynchronous- forget about responses
* You want all of your services to talk the same language
* Choose one for sync, one for async
* They chose HTTP REST and (missing)

# Twelve-Factor App
* https://l2factor.net
* Declarative configuration
* Environment portability
* Use as guide, not dogma

# Services and API Design
* Do not make breaking changes to API endpoints
* Increment endpoint version or add new endpoint
* Always test every older endpoint version

(missing)
# Separating Data Stores
* Every database should only be accessed by a single (missing)

# Diagram: Separating Data Stores
(missing)

# Microservices Toolset Creation
* Auth
* Messaging
* Log Settings
* Correlation IDs
* Middleware

# Example: Microservices Toolset Creation
* On Django, add a backend_api to request objects

    def my_view(request):
        user = request.backend_api.get(
            ('accounts', 'user')
        )

        return {'user_id': user['id']}

# Devops and Infrastructure Design
* Use Docker to develop in the same environment as production
* Use Terraform and Ansible for configuration-based infrastructure
* Use Continuous Integration to automate deployment (using mesos/marathon)
    * Testing & Automatic Deployment

# Centralized Data Pipeline
* Apache Kafka- distributed log system
* Confluent Open Source Platform
* All data has a schema
* Producers, Consumers, and Connectors

# Example: Centralized Data Pipeline
(missing)

* Things get complicated really fast.
* Business pushes you for more infrastructure.

# Example: Centralized Data Pipeline
(missing)

* Anything that wants to follow the database just follows the pipeline

# Logging and Analytics
* Send all logs into data pipeline
* Send all Docker logs into Kafka using logspout
* Send all syslog into Kafka using Kafka Connect
* Alternatives to Kafka: MQ, RabbitMQ... but Kafka can save history? can rewind?

# Logging and Analytics
* Generate Correlation IDs (CIDs) on all initial actions
* Use CIDs in logs and all communications between services
* CIDs can be used to trace issues across entire distributed system
    * Can dump them into ElasticSearch and search the CID


