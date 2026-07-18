# Chapter 01: CI/CD Fundamentals

This manual introduces CI/CD, the differences between Continuous Integration, Continuous Delivery, and Continuous Deployment, and the problems resolved by automation.

---

## 1. What is CI/CD?

**CI/CD** stands for Continuous Integration and Continuous Delivery (or Continuous Deployment). It represents a set of operating principles and practices that enable software development teams to deliver code changes more frequently, reliably, and securely.

```
                  [ DEVELOPER commits code to Git ]
                                 |
                                 v
   +-----------------------------------------------------------+
   |                  CONTINUOUS INTEGRATION (CI)              |
   |                                                           |
   |   [ Automatically Build App ] ===> [ Run Unit Tests ]    |
   +-----------------------------------------------------------+
                                 |
                                 v
   +-----------------------------------------------------------+
   |                  CONTINUOUS DELIVERY (CD)                 |
   |                                                           |
   |   [ Package Artifact / Docker Image ] ===> [ Push to Repo]|
   +-----------------------------------------------------------+
                                 |
                                 v
   +-----------------------------------------------------------+
   |                  CONTINUOUS DEPLOYMENT (CD)               |
   |                                                           |
   |             [ Deploy to Production Cluster ]              |
   +-----------------------------------------------------------+
```

### 1.1 Continuous Integration (CI)
Developers merge their code changes back to the main branch as frequently as possible. 
* **The Process**: Each merge triggers an automated build and test sequence.
* **Goal**: Detect integration bugs early, before they accumulate and become difficult to resolve.

### 1.2 Continuous Delivery (CD)
Extends Continuous Integration by ensuring that every build that passes the testing phase is packaged and ready to be deployed to production.
* **The Process**: Code is built, packaged into an artifact (like a `.war` file or Docker image), and pushed to a repository.
* **Goal**: Deployment to production remains a simple, single-click decision.

### 1.3 Continuous Deployment (CD)
Takes Continuous Delivery a step further. Every code change that passes all stages of the production pipeline is deployed directly to customers automatically, with no human intervention.

---

## 2. Problems Resolved by CI/CD

Before CI/CD, teams utilized manual deployment workflows:
* **The "Integration Nightmare"**: Developers worked on isolated features for weeks. When they finally tried to merge their code, hundreds of conflicts occurred, halting progress for days.
* **Manual Deployment Errors**: Operators followed manual checklist documents to install packages and configure servers. A single typo could cause a production outage.
* **Slow Feedback Loops**: Bugs introduced in development were often not discovered until weeks later during manual QA testing, making them expensive to fix.
