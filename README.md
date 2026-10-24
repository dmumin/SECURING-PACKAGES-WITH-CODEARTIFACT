# SECURING-PACKAGES-WITH-CODEARTIFACT

This repository contains my configuration and setup for secure, private package management using AWS CodeArtifact and Apache Maven. This project represents Part 3 of my ongoing DevOps roadmap to build an automated, secure CI/CD pipeline!

# Secure Packages with CodeArtifact

**Project Link:** [View Project](https://nextwork.ai/projects/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67)

**Author:** Dahir Mumin  
**Email:** dahirmumin7@gmail.com

---

![Image](https://nextwork.ai/vibrant_magenta_brave_angelfish/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_1d79e699)

## Introducing Today's Project!

In this project, I will demonstrate how to set up CodeArtifact as our CI/CD pipeline's artifact repository! We're doing this project to learn the importance and value of having an artifact repository. It's a huge time saver.

### Key tools and concepts

In this project, I went on an incredible learning journey, transforming from someone dealing with account blocks and permissions errors to securely managing and publishing software packages on AWS. Here are the key services and concepts I mastered along the way:

1. Key Services I Worked With

AWS CodeArtifact: I learned how to set up this fully managed service to act as my private package repository, securely storing and managing application dependencies.

AWS IAM (Identity and Access Management): I learned how to construct custom JSON policies and assign an IAM role to an EC2 instance so my server could fetch authorisation credentials securely.

Amazon EC2: I used an EC2 server as my build environment where I configured development tools and triggered builds.

AWS STS (Security Token Service): I leveraged STS to generate secure, temporary authorisation tokens that expire after 12 hours, which is a key security practice.

2. Key Core Concepts I learned.

Upstream repositories: I learned how to link my private CodeArtifact repository to a public source like Maven Central. When I ran my compilation, my repository acted as a secure middleman, caching packages locally.

Domains: I discovered how CodeArtifact domains act as logical containers, helping me group multiple repositories and centrally manage billing and security controls.

The Least Privilege Principle: I experienced firsthand why AWS resource connections fail by default and how attaching temporary roles to servers is far more secure than hardcoding long-term API keys.

Maven Build Configurations: I learned how to modify settings.xml to redirect default requests away from the public internet and directly authenticate with my private repository.

Package Publishing: In my extension mission, I learned how to go from being a package consumer to a publisher by creating and uploading my own custom package using the AWS CLI.

### Project reflection

This project took me approximately an hour and a half to complete, including the time spent troubleshooting my AWS account activation and resolving my initial connection errors. The most challenging part was understanding how my EC2 instance passes security details and figuring out why my initial token command failed with a credentials error before I attached the correct IAM role. It was most rewarding to see my Maven compile command finish with a giant green BUILD SUCCESS and then refresh my AWS CodeArtifact console to see all my cached packages populated inside my private repository for the first time!

This project is part three of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project as soon as I can! I am fully committed to completing my DevOps journey, and my next goal is to tackle the fourth project in this series, Continuous Integration with CodeBuild, to automate my application builds!

## CodeArtifact Repository

A fully managed artifact repository service provided by AWS that makes it easy for organizations to securely store, publish, and share software packages (such as packages used with npm, pip, Maven, or NuGet).

A domain is a logical container or "folder" that groups multiple repositories belonging to the same organisation or project, allowing you to centrally manage security policies, encryption keys, and package sharing in one single place

A CodeArtifact acts as a trusted external source or backup library that your primary repository can search when it doesn't have the package you need. If AWS CodeArtifact cannot find a package locally, it will automatically query its upstream repository to find, download, and cache a copy of that package so it is ready for all your future builds.

![Image](https://nextwork.ai/vibrant_magenta_brave_angelfish/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_n4o5p6q7)

## CodeArtifact Security

### Issue

To access CodeArtifact, we need a temporary authorization token (which acts like a secure, temporary password for our package manager client, Maven) to verify our identity before AWS lets us publish or retrieve software packages. I ran into an error when retrieving a token because our EC2 instance currently has no permissions to communicate with AWS CodeArtifact, which means AWS blocked the request with an 'Unable to locate credentials' security error.

### Resolution

To resolve the error with my security token, I created an IAM policy with specific permissions to access CodeArtifact, attached it to a new IAM role for EC2, and associated that role directly with my running EC2 instance. This resolved the error because attaching the role allowed AWS to automatically provide temporary security credentials to my server. When I re-ran the connection command, my instance was successfully authorised to retrieve the authorisation token instead of being blocked by the 'Unable to locate credentials' error.

It's security best practice to use IAM roles because they provide temporary, auto-rotating security credentials to our EC2 instance on demand, completely eliminating the need to store, manage, or hardcode long-term AWS access keys on the server.

## The JSON policy attached to my role

permissions to retrieve an authorisation token, locate repository endpoints, and read packages from our repository (using the codeartifact:GetAuthorizationToken, codeartifact:GetRepositoryEndpoint, and codeartifact:ReadFromRepository actions). It also allows calling sts:GetServiceBearerToken specifically for CodeArtifact to provide secure, temporary access tokens for our client.

![Image](https://nextwork.ai/vibrant_magenta_brave_angelfish/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_23rp7q8r9)

## Maven and CodeArtifact

### To test the connection between Maven and CodeArtifact, I compiled my web app using settings.xml

The settings.xml file configures Maven to locate our private CodeArtifact repository URLs, present our temporary authorisation token for authentication, and follow specific rules (using profiles and mirrors) on where to download and cache our project's dependencies.

Compiling means translating the human-readable source code that we write into binary machine code (or bytecode) that a computer's processor can actually read, understand, and execute as a working application.

![Image](https://nextwork.ai/vibrant_magenta_brave_angelfish/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_c17eace8)

## Verify Connection

After compiling, I checked my private AWS CodeArtifact repository. I noticed that it was newly populated with a list of Maven packages. These are the dependencies that my application needs, which CodeArtifact automatically fetched from my upstream repository (Maven Central) and cached locally for my future builds.

![Image](https://nextwork.ai/vibrant_magenta_brave_angelfish/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_1d79e699)

Contact
If you have any questions or comments about My Web Project, please contact: dmumin - [My Email](mailto:dahirmumin7@gmail.com)

[LinkedIn] (https://www.linkedin.com/in/dahir-mumin-661682335/)

Conclusion
Thank you for exploring my project! I'll continue to build this pipeline and apply my learnings to future projects.


---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67)*
