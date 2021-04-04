# Global Application Configuration

In this chapter, we are going to focus on application settings and configuration files.

If you are building a large application or an application that performs the server side or RESTful APIs calls,
sooner or later you will come across the need to store global application settings somewhere.
For example the APIs URL string, you may hardcode it somewhere in the application or service,
but that will require rebuilding and redeploying your application every time the URL value needs changes.

It is much easier storing and loading configuration parameters in a separate file
that developers can maintain without changing the main application code.

Let's create an Application Configuration service that loads global settings from the external server-side file
before all other services get loaded, and provides access to configuration properties
for all other application components and services.
