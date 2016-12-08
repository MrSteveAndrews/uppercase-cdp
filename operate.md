#Operating BlueCDP
BlueCDP has several moving parts, each of which must be understood in order for it to be properly demonstrated.  

## Sample Application Code Base
BlueCDP is demonstrated by means of building and testing a simple microservice and web application.  The application under development is intentionally *very* simple so as not to distract the audience from the intent of the demo, which is how the application is built, deployed, and tested.

The Uppercase application consist of two runtime components:

* **uppercase-service**	 : A microservice that converts an input string of characters to upper case using the REST protocol
* **uppercase-web** : A web application that provides a simple user interface for interacting with uppercase-service

The code for these components is managed in GitHub in the following repositories:

* [uppercase-service](https://github.com/MrSteveAndrews/uppercase-service) : The uppercase-service application code
* [uppercase-service-at](https://github.com/MrSteveAndrews/uppercase-service-at) : The uppercase-service acceptance tests
* [uppercase-web](https://github.com/MrSteveAndrews/uppercase-web) :  The uppercase-web application code
* [uppercase-web-at](https://github.com/MrSteveAndrews/uppercase-web-at) : The uppercase-web acceptance tests

### Application Code
The application code is written in Java using the [Spring](https://spring.io/) framework and built with [Maven](https://maven.apache.org/).  Both the service and web components are run as [Spring Boot](https://projects.spring.io/spring-boot/) applications.

Once compiled, the uppercase-service and uppercase-web components are packaged into [Docker](https://www.docker.com/) images, which allows them to be deployed and run on any host that has the Docker runtime installed.

### Acceptance Tests
The uppercase-service and uppercase-web acceptance test specifications are defined using [Gherkin](https://cucumber.io/docs/reference#gherkin) and made executable using [Cucumber-JVM](https://cucumber.io/docs/reference/jvm).  uppercase-web acceptance test steps use [Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/) with the [PageObject](https://github.com/SeleniumHQ/selenium/wiki/PageObjects) pattern.  

If you want to understand the expected behavior of uppercase-service, simply refer to the uppercase-service-at feature files. 

~~~gherkin
Feature: Transform string values to upper case using the REST service API

  Scenario: Transform lower case strings to upper case
    Given the upper case service is running
    When the input string "abc" is passed into the upper case service
    Then the resulting output string is "ABC"
  Scenario: Transform upper case strings to upper case
    Given the upper case service is running
    When the input string "GHI" is passed into the upper case service
    Then the resulting output string is "GHI"
~~~


## Cloud Computing Environment
All of the BlueCDP components are deployed to [AWS EC2](https://aws.amazon.com/ec2/) virtual instances under the Blue Agility AWS account.  

### Security
The Blue Agility AWS account may be accessed at [https://blueagility.signin.aws.amazon.com/console](https://blueagility.signin.aws.amazon.com/console).  The login credentials may be found [here](https://intranet.blue-agility.com/bluejazz/wiki/bluecdp/).

All of the BlueCDP computing resources are secured with the **devops** key pair: 

![devops-key-pair](images/devops-key-pair.png)

In order to SSH to any EC2 instance secured with this key pair, you must have the associated private key on your local host.  Instructions for creating the private key file are located [here](https://intranet.blue-agility.com/bluejazz/wiki/bluecdp/).

### Networking
All BlueCDP computing resources are accessible over the public Internet via the public subnet of the devops_vpc virtual private cloud (VPC).  The purpose for this is to attain a level of network isolation between BlueCDP and other Blue Agility computing resources.

![devops-vpc](images/devops-vpc.png)

![devops-vpc-subnet](images/devops-vpc-subnet.png)

### EC2

BlueCDP EC2 instances may be managed and monitored from the EC2 console.  Note that you can filter the list to only show those instances with key pair = devops:

![devops-ec2-console](images/devops-ec2-console.png)

#### CDP Server
BlueCDP uses [ThoughtWorks Go CD](https://www.go.cd/) to orchestrate the steps in the build, deploy, test value stream.  GoCD is free and open source and does a great job of visualizing the value stream.

GoCD leverages a distributed architecture wherein a central *server* orchestrates the activities of *agents* that perform the work in the various pipeline jobs.

The Go Server and a single Go Agent are running on the **devops-gocd-server** and **devops-gocd-agent** EC2 instances respectively.

![devops-ec2-gocd](images/devops-ec2-gocd.png)





