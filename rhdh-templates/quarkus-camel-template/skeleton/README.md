= REST with Jackson: A Camel Quarkus example
:cq-example-description: An example that demonstrates how to create a REST service using the Camel REST DSL and Jackson.

{cq-description}

This example is a port of Quarkus' quickstart https://github.com/quarkusio/quarkus-quickstarts/blob/main/rest-json[rest-json] to Camel.

TIP: Check the https://camel.apache.org/camel-quarkus/latest/first-steps.html[Camel Quarkus User guide] for prerequisites
and other general information.

== Start in the Development mode

[source,shell]
----
$ mvn clean compile quarkus:dev
----

The above command compiles the project, starts the application and lets the Quarkus tooling watch for changes in your
workspace. Any modifications in your project will automatically take effect in the running application.

TIP: Please refer to the Development mode section of
https://camel.apache.org/camel-quarkus/latest/first-steps.html#_development_mode[Camel Quarkus User guide] for more details.

Then point your browser to one of the endpoints:

* http://localhost:8080/fruits.html
* http://localhost:8080/legumes.html

=== Package and run the application

Once you are done with developing you may want to package and run the application.

TIP: Find more details about the JVM mode and Native mode in the Package and run section of
https://camel.apache.org/camel-quarkus/latest/first-steps.html#_package_and_run_the_application[Camel Quarkus User guide]

==== JVM mode

[source,shell]
----
$ mvn clean package
$ java -jar target/quarkus-app/quarkus-run.jar
...
[io.quarkus] (main) camel-quarkus-examples-... started in 1.163s. Listening on: http://0.0.0.0:8080
----

==== Native mode

IMPORTANT: Native mode requires having GraalVM and other tools installed. Please check the Prerequisites section
of https://camel.apache.org/camel-quarkus/latest/first-steps.html#_prerequisites[Camel Quarkus User guide].

To prepare a native executable using GraalVM, run the following command:

[source,shell]
----
$ mvn clean package -Pnative
$ ./target/*-runner
...
[io.quarkus] (main) camel-quarkus-examples-... started in 0.013s. Listening on: http://0.0.0.0:8080
...
----

== Feedback

Please report bugs and propose improvements via https://github.com/apache/camel-quarkus/issues[GitHub issues of Camel Quarkus] project.