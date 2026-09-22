# Jenkins-Nexus-Integration-RBAC-Custom-Workspace
This project documents my hands-on learning with Jenkins and three important DevOps concepts:

Jenkins Integration with Nexus Repository
Role Based Access Control (RBAC)
Custom Workspace Configuration
1. Jenkins Integration with Nexus
Architecture
Developer
    |
    v
 GitHub
    |
    v
 Jenkins
    |
    +--> Compile
    |
    +--> Test
    |
    +--> Package
    |
    v
 Artifact
    |
    v
 Nexus Repository
Pipeline Flow
GitHub
   ↓
Jenkins
   ↓
Checkout
   ↓
Compile
   ↓
Test
   ↓
Maven Package
   ↓
WAR Artifact
   ↓
Nexus Repository
Nexus Setup

For practice, I configured a Nexus server using an Amazon Linux 2 instance.

The setup included:

Amazon Linux 2
30 GB storage
t2.medium instance
Java 17
Nexus Repository Manager

After installing Nexus, I configured a Maven hosted repository.

Example repository:

Repository Type: maven2 (hosted)
Repository Name: hotstar
Version Policy: Snapshot
Deployment Policy: Allow redeploy
Jenkins Configuration

The Nexus Artifact Uploader plugin was installed from:

Manage Jenkins
→ Plugins
→ Available Plugins
→ Nexus Artifact Uploader

Nexus credentials were then configured in Jenkins.

Example:

Credentials ID: nexuscreds
Username: admin
Password: <configured Nexus password>
Example Nexus Upload Step
nexusArtifactUploader(
    artifacts: [[
        artifactId: 'myapp',
        classifier: '',
        file: 'target/myapp.war',
        type: '.war'
    ]],
    credentialsId: 'nexuscreds',
    groupId: 'in.reyaz',
    nexusUrl: 'NEXUS_IP:8081',
    nexusVersion: 'nexus3',
    protocol: 'http',
    repository: 'hotstar',
    version: '8.3.3-SNAPSHOT'
)

Replace the Nexus IP, repository name, credentials ID, group ID, artifact ID, and version with the values used in your own environment.

2. RBAC – Role Based Access Control

RBAC allows Jenkins administrators to control what different users are allowed to access and perform.

What I Practiced
Creating Jenkins users
Installing Role-Based Authorization Strategy
Creating roles
Assigning permissions
Assigning users to roles
Testing user permissions
Configuring project-level permissions
Example
Jenkins Administrator
        |
        +----------------+
        |                |
     Senior            Junior
        |                |
   Admin Access       Read Access
Authentication vs Authorization

Authentication

Determines whether a user is allowed to sign in.

Authorization

Determines what an authenticated user is allowed to access or perform.

Example:

Authentication
      ↓
Can the user enter Jenkins?

Authorization
      ↓
What can the user do inside Jenkins?

I also practiced Project-Based Matrix Authorization to provide permissions at the individual job level.

3. Custom Workspace

Jenkins normally creates workspaces automatically for jobs.

A custom workspace allows us to specify a particular directory where the job should execute.

Example:

/home/ec2-user/reyaz
Configuration

In a Jenkins job:

Job
 → Configure
 → General
 → Advanced
 → Use custom workspace

Then specify:

/home/ec2-user/reyaz
Linux Permissions

When Jenkins uses a custom directory, the Jenkins user needs appropriate permissions to access that directory.

I practiced troubleshooting permission issues using Linux ownership and permission commands.

For example:

ls -al

and checking ownership of the required directories.

The key learning was:

Jenkins Job
     |
     v
Custom Workspace
     |
     v
Linux File Permissions
     |
     v
Build Success
Key Learnings

Through this practice, I learned that Jenkins is not only about creating pipelines.

I also need to understand:

CI/CD
Artifact Management
Nexus Repository
Maven
Jenkins Plugins
RBAC
Authentication
Authorization
Linux Users
Linux File Permissions
Custom Workspaces
Jenkins Security
Technologies Used
Jenkins
Nexus Repository Manager
Maven
Git
GitHub
Linux
Amazon Linux 2
Java 17
Jenkins Pipeline
Conclusion

This hands-on practice helped me understand how Jenkins can integrate with an artifact repository, manage user permissions, and work with custom workspace locations.

I am continuing to build my DevOps skills through hands-on Jenkins and AWS projects.
