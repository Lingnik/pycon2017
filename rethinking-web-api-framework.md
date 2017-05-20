# Level up! Rethinking the Web API framework.
* presenter: Tom Christie
* Author of Django REST Framework
* New API framework- how can we take a slightly newer approach to building APIs in Python?

## Manifesto
* How we can build powerfully expressive APIs by using function annotations for dependency injection... (missing)
* Building meaning into our codebase.

## Example: Kitten API
* Add kittens to a favorites list, by name.
* Show the kittens in our favorites list, optionally filtering to only include kittens of a given color.

## (Code) View Function for favorite_kittens(request)
(missing)

* First change: let's always think about writing our views in a way that we always split out individual HTTP methods.
* In a regular Django function-based view, we'd typically have a URL root point at the view, any HTTP method would get rooted to that regardless of the method.

## (Code) Change #1: Always split out views by different HTTP requests
(missing)

### (Code) Route all methods to a single view
(missing)

### (Code) Route methods to individual views
(missing)

### (Code) Automatic API documentation, before splitting our view
(missing)
(missing)

* The URL gives us two underlying operations that you can do.

## Rethinking the view interface
* Look at view abstraction that represents the interface that we're exposing

### (Code) Change the input parameters
(missing)

* Put the operation in the function signature. (user, name)
* Rather than return a JSON response, return the actual data (dict instead of JSONResponse)

## Dependency Injection

### (Code) Change the output type
(missing)
* How does the framework know about those dependencies for inputs and outputs?

### (Code) Dependency injection in the pytest framework.
(missing)

* Arguments that you provide to the test function automatically get provided by the framework.

### (Code) pytest fixtures

* Have a decorator and a function that returns the argument.
* The name of the argument is used to pick out the fixture that you want.

### (Code) Adding type annotation to our views
(missing)

    def view(request: Request) -> Response:
        ...

### (code) Dependency injection using type annotation
(missing)

    def view(user: User) -> Response:
        ...

    def view(color: QueryParam) -> Response:
        ...

### (code) Dynamic return types
(missing)

    def view(request: Request) -> ResponseData:
        ...

## (code) Method to describe how components are created
(missing)

    class QueryParam(str):
        pass

    @component_builder
    def build_query_param(environ: WSGIEnviron, arg: ArgName) -> QueryParam:
        ...

## (code) Multiple Dependency-Injected Arguments
(missing)

## (code) Working at different abstraction layers
(missing)

* Could write a view that digs down into the WSGI layer directly.
* Framework is able to dynamically build up what it needs to satisfy the interface of the view.

## (code)
(missing)

    def list_favorite_kittens(user: User,
                              color: QueryParam) --> ...

## More testable, reusable interface
* Makes for more testable interfaces- you don't have to mock up an HTTP Request. Just test the interface directly.

### (code) Old Way
(missing)

### (code) New Way
(missing)

## Schemas as a type system
* Describe the data structures as inputs and outputs of the API.
* JSON Schema is a widely used representation for expressing the data structure and the constraints of a JSON data structure.
* What's nice is that the format allows you to express those constraints in a way that can be reused by many other tools,
(missing- 4 reasons)
(missing- primitives, container types, composites)
(missing- examples)
(missing- composite types)
(missing- selections)

## How can we apply JSON Schema to our view interface?

### Create a type system that maps onto JSON Schema
(missing)

### (code) Use our schema types as regular data primitives
(missing)

### (code) Schema types enforce data validation
(missing)

### (code) Can build up more complex schemas
(missing)
* e.g. objs with complex data properties

### (code) Use schemas as type annotations
(missing)

* Give rich semantic information about what the inputs and outputs look like.

## Powerful Properties
* Translate parameters and response types in JSON Schema.
* Easily map our API into an API schema such as Swagger.
* Create HTML forms corresponding based on our input parameters.

## Refactoring Kitten API to rich schema annotations
(missing)
(missing)

    , name: KittenName) -> Kitten


# Why?
1. API mocking-
    * Gives your frontend devs access to a mock API before you've written the logic. if we build our APIs in this sort of way, once we have declared our views and schemas even before we've implemented anything else at all, we'll be able to build a mock API based on that information
    * Promotes an API design-first approach
2. Web-Browsable APIs
3. API Documentation
    * Gives us everything we need to automatically generate documentation for the API as a whole.
    * Guarantees our documentation is always in-sync with our codebase.
4. Dynamic Client Libraries
    * A client that uses the generated schemas to present the interface to the user.
    * Already have JavaScript, Python, Command-Line clients for Django REST Framework.
    * Example of `coreapi` (missing)
5. Command Line Interfaces
    * Function signatures are now a plain data interface
    * CLI and HTTP can both route to the same view.
6. Realtime APIs
    * Route a websocket route onto the same view used by HTTP.
    * Framework would be responsible for handling the protocol.

# Where are we now?
(missing)

## Summary of Benefits

## apistar
* https://github.com/tomchristie/apistar

### Efficiency, when needed
* Framework should be able to get out of your way when possible to bypass high-level abstractions.

### Explicit dependencies
(missing)
* Make dependencies explicit in the interface
* e.g. DatabaseSessions for views that need access
* e.g. Network requests for views that need requests
* Can make sure when testing the API we can pass mocks instead of the real thing.
* e.g. Datetimes
* e.g. Randomization

## ASGI
* Came about in Django to support a deployment mode where Django can be used with things like websockets and not just HTTP requests w/ responses.
