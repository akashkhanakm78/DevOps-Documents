# Chapter 01: DevOps Culture & Mindset

This manual explores the definition of DevOps, its history, Agile alignment, the CALMS framework, and the Continuous Integration / Continuous Deployment lifecycle.

---

## 1. What is DevOps?

### 1.1 The Definition
**DevOps** (a portmanteau of "Development" and "Operations") is a combination of cultural philosophies, practices, and tools that increases an organization's ability to deliver applications and services at high velocity. 

* It breaks down the traditional silos between **Software Development teams (Dev)** and **IT Operations teams (Ops)**.
* Historically, Dev wanted to push changes *fast* (new features), while Ops wanted to keep the system *stable* (no changes). This created a conflict of interest, slow release cycles, and manual errors.

```
       +-------------------+                     +------------------+
       | DEVELOPMENT TEAM  |                     | OPERATIONS TEAM  |
       |                   |   "Over the wall"   |                  |
       | Wants Velocity    | ==================> | Wants Stability  |
       | (Pushes Code)     |                     | (Keeps Server Up)|
       +-------------------+                     +------------------+
                                  VS.
                      +-------------------------+
                      |      DEVOPS UNION       |
                      |                         |
                      |   Shared Responsibility |
                      |    Automated Pipeline   |
                      +-------------------------+
```

---

## 2. Agile vs. DevOps

* **Agile** is a software development methodology focused on iterative development, collaboration, customer feedback, and small, incremental releases of working software (addressing development lifecycle).
* **DevOps** extends the Agile methodology into the operations domain. It ensures that the small, incremental releases produced by the Agile Dev team can be built, tested, packaged, deployed, and monitored automatically in production.

---

## 3. The CALMS Framework

DevOps is not a single tool; it is evaluated using the **C.A.L.M.S.** framework:

1. **Culture**: Emphasizes shared responsibility, mutual trust, and a blameless post-mortem environment when failures occur.
2. **Automation**: Standardizes code integration, unit testing, server provisioning, and deployment using automated pipelines to eliminate manual error.
3. **Lean**: Focuses on minimizing waste, reducing batch sizes of deployments, and getting feedback from production quickly.
4. **Measurement**: Tracks key metrics (such as deployment frequency, lead time for changes, mean time to recover (MTTR), and change failure rates).
5. **Sharing**: Promotes open communication and sharing experiences, tools, and lessons learned across the organization.

---

## 4. The DevOps Lifecycle

The DevOps lifecycle is modeled as an infinite loop (the infinity symbol), demonstrating that software delivery is a continuous, circular feedback loop:

```
          [ Plan ]                     [ Operate ]
         /        \                   /           \
    [ Code ]       [ Build ]     [ Deploy ]       [ Monitor ]
        \            /               \            /
         [  Test  ]                     [ Release ]
```

### The Phases:
1. **Plan**: Define business requirements, design features, and track tasks (Jira, Trello).
2. **Code**: Write application source code and manage changes using version control (Git).
3. **Build**: Compile source code, resolve dependencies, and package binaries (Maven, npm).
4. **Test**: Run automated unit and integration tests to verify code quality (JUnit, Selenium).
5. **Release**: Approve build artifacts and prepare them for deployment (Jenkins, GitHub Actions).
6. **Deploy**: Automatically provision servers and release the code to production hosts (Docker, Ansible, Terraform).
7. **Operate**: Keep systems running, scale resources, and manage traffic (Kubernetes).
8. **Monitor**: Track application performance, error rates, and server resources (Prometheus, Grafana, ELK).
