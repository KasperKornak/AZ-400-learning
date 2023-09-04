# Provisioning and testing environments
## Deployment targets
When we focus on the deployment of the Infrastructure, we should first consider the differences between the target environments that we can deploy to:

- **On-Premises servers**.
- **Cloud servers or Infrastructure as a Service (IaaS)**. For example, Virtual machines or networks.
- **Platform as a Service (PaaS) and Functions as a Service (FaaS)**. For example, Azure SQL Database in both PaaS and serverless options.
- **Clusters**.
- **Service Connections**.

## Configuring automated integration and functional test automation

![Alt text](img/agile-testing-quadrants-1495c244.png)

We can make four quadrants where each side of the square defines our targets with our tests:

- **Business facing**: the tests are more functional and often executed by end users of the system or by specialized testers that know the problem domain well.
- **Supporting the Team**: it helps a development team get constant feedback on the product to find bugs quickly and deliver a quality build-in product.
- **Technology facing**: the tests are rather technical and non-meaningful to business people. They're typical tests written and executed by the developers in a development team.
- **Critique Product**: tests that validate a product's workings on its functional and non-functional requirements.

## Shift-left
The goal for shifting left is to move quality upstream by performing tests early in the pipeline. It represents the phrase "fail fast, fail often" combining test and process improvements reduces the time it takes for tests to be run and the impact of failures later on.

## Azure Load Testing
Azure Load Testing Preview is a fully managed load-testing service that enables you to generate a high-scale load.

- The service simulates your applications' traffic, helping you optimize application performance, scalability, or capacity.
- You can create a load test using existing test scripts based on Apache JMeter. Azure Load Testing abstracts the infrastructure to run your JMeter script and load test your application.
- Azure Load Testing collects detailed resource metrics for Azure-based applications to help you identify performance bottlenecks across your Azure application components.
- You can automate regression testing by running load tests as part of your continuous integration and continuous deployment (CI/CD) workflow.

## Lab addresses 
https://aka.ms/az-400-set-up-and-run-functional-tests

