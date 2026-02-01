# CILabProject (DevOps)

A simple Maven + JUnit project for CI lab submission.

## Build
```bash
mvn clean test
mvn clean package
```

## Installation Guide (Jenkins)
1. Install a supported Java JDK (Jenkins requires a modern Java runtime).
2. Install Jenkins (LTS) using your OS installer or by running the `jenkins.war`.
3. Start Jenkins and complete the initial setup wizard.
4. Install/verify these plugins: Git, Pipeline, Maven Integration, JUnit.
5. Configure global tools in **Manage Jenkins > Tools**:
   - JDK (point to your JDK install)
   - Maven (or use a system Maven already on PATH)
6. Ensure at least one agent is available that can run Windows `bat` steps (this Jenkinsfile uses `bat`).
7. Create a **Multibranch Pipeline** or **Pipeline** job that points to this repository so Jenkins can read `Jenkinsfile`.

## User Manual (Jenkins Jobs)
- **What the pipeline does**
  - **Checkout**: pulls source and prints the branch name.
  - **Test (all branches)**: runs `mvn clean test` and publishes JUnit reports.
  - **Package + Archive (main only)**: runs `mvn clean package` and archives `target/*.jar`.
  - **Security Scan (release/* only)**: placeholder stage for a future scanner.
- **How to run**
  1. In Jenkins, open the job and click **Build Now** (or trigger via SCM webhook).
  2. Watch **Console Output** for progress and errors.
  3. After a successful build:
     - Test reports appear under **Test Result**.
     - Artifacts are available under **Artifacts** (main branch only).
- **Branch behavior**
  - Any branch runs tests.
  - `main` additionally produces and archives the JAR.
  - `release/*` additionally runs the security scan placeholder.

## Troubleshooting Guide
- **`mvn` not found / Maven version missing**: Install Maven or configure it in **Manage Jenkins > Tools**. Ensure the agent PATH includes Maven.
- **Tests not published**: Confirm tests ran and `target/surefire-reports/*.xml` exists. Check Maven Surefire output.
- **No artifacts archived**: Packaging only runs on the `main` branch and expects `target/*.jar` to exist.
- **Pipeline fails on Linux agents**: This Jenkinsfile uses Windows `bat` steps; switch to `sh` or use a Windows agent.
- **Security Scan stage not running**: It only runs on branches named `release/*`.
- **SCM checkout failures**: Verify repository URL and credentials in the Jenkins job configuration.
