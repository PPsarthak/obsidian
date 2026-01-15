#placement-preparation  #ci/cd

CI/CD is a DevOps practice that automates the process of building, integrating, testing, and releasing software.

---
#### 💡Continuous Integration
Continuous Integration is the practice of <mark style="background: #B266FF;color:#FFF;">automatically building and testing your application every time code is committed </mark>to a shared repository.
> The goal is to detect bugs early, integrate code frequently in feature branch itself, and keep the main codebase stable.

###### 🧰 What CI Does
1. ✅ The CI pipeline is triggered
2. 🛠️ Code is compiled/built using a tool (say Maven)
3. 🧪 Unit tests are run automatically (e.g., JUnit)
4. 🔍 Linting/static analysis checks are done (e.g., SonarQube)
5. 📦 A build artifact is generated (`.jar/.war`, Docker image)
6. 📬 Feedback is sent to the developer — pass/fail, logs, etc.
---
#### Continuous Deployment
Continuous Deployment is the practice of <mark style="background: #ADCCFFA6;">automating the release of your software, starting from a successful build in CI</mark>.

###### 🔄 How CD Works (After CI)
1. ✅ CI completes — code is tested and built successfully.
2. 📦 Artifact (JAR, Docker image, etc.) is stored (e.g., in Nexus, Artifactory, ECR).
3. 🧪 Optional integration, smoke, and acceptance tests run in a staging environment.
4. 🔁 Depending on setup:
    - <span style="color:rgb(0, 112, 192)">Delivery</span>: Waits for a manual approval to deploy to prod.
    - <span style="color:rgb(0, 112, 192)">Deployment</span>: Automatically deploys to production.
---
#### Continuous Delivery vs Continuous Deployment
> The key difference between Continuous Delivery and Continuous Deployment is how far automation goes after CI

| Aspect             | Continuous Delivery                               | Continuous Deployment                             |
| ------------------ | ------------------------------------------------- | ------------------------------------------------- |
| Automation Level   | Automates everything except production release    | Automates everything, including prod deployment   |
| Human Intervention | Manual approval required to deploy to prod        | No manual steps — goes straight to production     |
| Use Case           | Suitable when manual QA or approvals are required | Ideal when you trust automated testing completely |
| Which Comes First? | Comes before Continuous Deployment                | Comes after Continuous Delivery                   |
> [!hint]+ 🧠 Mnemonic
> Delivery = The code is "ready to be delivered"
> Deployment = The code is actually "deployed to production" 

---
##### ⚙️ What CI/CD Does — Step-by-Step
1. ✅ <span style="color:rgb(0, 112, 192)">Code Push</span>
    - Developer pushes code to a shared repository (e.g., GitHub, Bitbucket).
2. 🔄 <span style="color:rgb(0, 112, 192)">CI Kicks In</span>
    - Code is automatically built and tested using tools like Maven/Gradle + JUnit.
    - Linting, static analysis, and unit tests are run.
    - Feedback is provided immediately (pass/fail).
3. 📦 <span style="color:rgb(0, 112, 192)">Artifact Creation</span>
    - A deployable build artifact (e.g., `.jar`, `.war`, Docker image) is generated.
4. 📤 <span style="color:rgb(0, 112, 192)">CD Takes Over</span>
    - Artifact is deployed to staging/test environments.
    - Further tests (integration, load, smoke) are run.
    - On success, it may automatically or manually go to production.
5. 🚀 <span style="color:rgb(0, 112, 192)">Production Deployment</span>
    - Final approved build is deployed to the live environment — safely and consistently.
---
#### Flow of CI/CD in IDeaS
![[CI-CD at IDeaS Example.png]]
###### 🧪If Continuous Deployment was implemented - 
- Production deployment would automatically occur if all tests pass from TechOps team