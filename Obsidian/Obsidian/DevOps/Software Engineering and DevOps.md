 
Software Development Lifecycle (SDLC)
- Analysing and defining software requirements
- Creating software architecture and design
- Software construction
- Software testing and QA
- Software deployment
- Software maintenance


UML Class diagrams 
▪ UML == Unified Modelling Language 
▪ Visualize classes and their relationships

### Deployment Environments

<span style="color:rgb(107, 255, 174)">Dev environment</span> 
▪ Used by developers 
▪ No client data

<span style="color:rgb(107, 255, 174)">Test environment</span> 
▪ Used by QA engineers 
▪ No client data

<span style="color:rgb(107, 255, 174)">Staging environment</span> 
▪ Used by QA engineers and/or clients for User Acceptance Testing 
▪ Limited production data

<span style="color:rgb(107, 255, 174)">Production environment</span> 
▪ Used by clients 
▪ Full Production data

### Software maintenance
▪ The process of changing a system after it has been released
Reasons for maintenance changes 
▪ Fixing bugs and patching security vulnerabilities 
▪ Changing business needs: new features and requirements 
▪ Adapting to new environments: hardware / platforms / software

## DevOps
#### <span style="color:rgb(255, 0, 0)">development (Dev) and operations (Ops)</span>

Declarative 
▪ Defines the desired state of the system, i.e. resources you need and their properties
Imperative 
▪ Defines the specific commands for the desired configuration

## DevSecOps

#### <span style="color:rgb(255, 0, 0)">development + security + operations</span>

### Serverless Computing
Serverless computing refers to outsourcing back-end cloud infrastructure and operations tasks to a cloud provider 
▪ Developers focus on writing code 
▪ Cloud provider manages the infrastructure, ensuring agility and scalability
Serverless computing == Function-as-a-Service (Faas) 
▪ Based on event-driven execution ▪ Allows functions to be triggered in response to specific events (changes in data or user requests, etc.) 
▪ Stateless nature 
▪ Serverless functions are designed to be stateless 
▪ Wide range of tools: Frameworks, SDKs, CLIs

### Microservices 
architectural approach to development that breaks the application into different loosely coupled services

Each service focuses on a specific business capability
Can be independently developed, deployed and scaled
Makes the development process more flexible

Communication between services is typically achieved through lightweight protocols, e.g., HTTP/REST 
Each microservice can have its own technology stack, programming language and database

### Common Git Branching Strategies

##### Trunk-Based Development
"No branches" strategy
Make smaller changes more frequently and commit directly into the trunk
This strategy is suited to more senior developers

##### GitHub Flow
Each feature is implemented in its own branch 
Keep the master code in a constant deployable state
Before merging, each branch comes through a Pull Request and code review, then the changes are tested and integrated
	
##### GitFlow Branches
This strategy contains separate and straightforward branches for specific purposes 
Ideal to handle multiple versions of the product
- Master (main)
- Release
- Hotfix
- Develop
- Feature

##### GitLab Flow
Combines feature-driven development and feature branching with issue tracking 
Each feature is implemented in its own branch 
Developers work with the main branch right away 
The main branch is deployed to production (manually or auto) GitLab Flow 

### **Regression testing** 
ensures that recent code changes, such as bug fixes or new features, do not negatively impact the existing functionality of the software. It involves retesting affected areas and related components to maintain stability, often using automation for efficiency.

npm install -D @playwright/test
-D - dev dependency

### YAML 
(YAML Ain't Markup Language)
Originally – Yet Another Markup Language
Human-readable data serialization standard

### Development Methodology
A set of practices and procedures for organizing the development process
A set of principles, conventions, processes and procedures the organization decides to follow 
A set of rules that developers must follow