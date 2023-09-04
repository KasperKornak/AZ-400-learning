# Introdcution to deployment patterns
## Microservices architecture
A **microservice** is an autonomous, independently deployable, and scalable software component. They're small, focused on doing one thing well, and can run autonomously. If one microservice changes, it shouldn't impact any other microservices within your landscape.

Each microservice has its lifecycle and Continuous Delivery pipeline. If you built them correctly, you could deploy new microservice versions without impacting other system parts. Microservice architecture is undoubtedly not a prerequisite for Continuous Delivery, but smaller software components help implement a fully automated pipeline.

## Classical deployment patterns

![Alt text](img/classic-deployment-pattern-8d085442.png)

## Modern deployment patterns

- Blue-green deployments.
- Canary releases.
- Dark launching.
- A/B testing.
- Progressive exposure or ring-based deployment.
- Feature toggles.