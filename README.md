# End-to-End CI/CD Pipeline with Java, Maven and DevSecOps Checks

A Jenkins pipeline that builds a Java web application with Maven, runs unit tests and code-coverage reports, checks code quality with SonarQube, scans for vulnerabilities with Trivy, publishes the build to Nexus, and deploys the WAR file to Apache Tomcat on AWS EC2. Slack receives a notification for each run.

## Pipeline Overview

```
GitHub push -> Jenkins -> Maven build -> Unit tests + JaCoCo -> SonarQube quality gate
            -> Trivy scan -> Publish to Nexus -> Deploy WAR to Tomcat on EC2 -> Slack notification
```

## Pipeline Stages

1. **Checkout:** pull the source code from GitHub.
2. **Build:** compile and package the application with Maven.
3. **Test:** run unit tests and generate JaCoCo code-coverage reports.
4. **Code quality:** run SonarQube analysis and enforce the quality gate.
5. **Security scan:** scan the application with Trivy.
6. **Publish:** upload the versioned WAR file to Nexus Repository Manager.
7. **Deploy:** copy the WAR file to Tomcat on EC2 and restart it.

A manual approval step runs before deployment, and Slack receives the pipeline status.

> Edit this list so the stage names match the Jenkinsfile in this repository exactly.

## Tools Used

| Tool | Purpose |
|------|---------|
| Jenkins | Runs the pipeline |
| Maven | Builds and packages the Java application |
| JaCoCo | Code-coverage reports |
| SonarQube | Static analysis and quality gate |
| Trivy | Vulnerability scanning |
| Nexus Repository Manager | Stores versioned build artifacts |
| Apache Tomcat | Runs the deployed WAR file |
| AWS EC2 | Hosts Jenkins, SonarQube, Nexus, and Tomcat |
| Slack | Pipeline notifications |

## Infrastructure

The project runs on four EC2 instances:

| Instance | Role | Instance type | Ports |
|----------|------|---------------|-------|
| `<name>` | Jenkins | `<type>` | `<8080>` |
| `<name>` | SonarQube | `<type>` | `<9000>` |
| `<name>` | Nexus | `<type>` | `<8081>` |
| `<name>` | Tomcat | `<type>` | `<8080>` |

## Prerequisites

- An AWS account with permission to launch EC2 instances
- A GitHub repository for the application source code
- A Slack workspace with an incoming webhook or app token

## Setup

1. Launch the four EC2 instances and open only the ports listed above in the security groups.
2. Install and configure Jenkins, SonarQube, Nexus, and Tomcat. `<add the install steps or a link to your notes>`
3. Add these credentials in Jenkins: GitHub access, a SonarQube token, Nexus login, Tomcat deployment user, and the Slack integration.
4. Create a Jenkins Pipeline job that uses the `Jenkinsfile` in this repository.
5. Run the job and check each stage in the Jenkins console output.

## Quality and Security Gates

- **SonarQube quality gate:** the pipeline fails when the code does not meet the gate conditions, so problems are caught before deployment.
- **Trivy scan:** reports known vulnerabilities in the application before it is published.
- **JaCoCo:** reports code coverage for each build.

## Testing the Gates

To confirm the gates work, I intentionally introduced code issues and vulnerabilities, then checked that SonarQube and Trivy flagged them. After confirming the detections, I fixed the issues.

## Repository Structure

```
.
├── Jenkinsfile          # <confirm file name>
├── pom.xml              # Maven build configuration
├── src/                 # Java application source and tests
└── README.md
```

## Future Improvements

- `<optional: for example, add Docker image builds or a Kubernetes deployment>`
