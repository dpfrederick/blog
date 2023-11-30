---
layout: post
title: Scanning Dependencies for Vulnerabilities
date: 2023-11-30 10:00:00 -0500
tags: security dependencies vulnerabilities
---

No matter what you're building, your project most likely relies on third party libraries of software to function (dependencies.) When you build the project the code from these libraries will be built into your executable or bundled into your web application. These libraries most likely contain their own dependencies, which are called transitive dependencies. These can also contain vulnerabilities that may be explotable in the context of your project, or they may not be.

When scanning project dependencies for vulnerabilities, it is important to consider the exploitability of the vulnerability in your environment, which is also known as the [attack surface](https://en.wikipedia.org/wiki/Attack_surface). If you and / or the security team have determined that a vulnerability is not exploitable in your environment, it may be preferable to suppress the vulnerability rather than fix it. This is a decision that should be made by the security team.

These third party libraries can contain vulnerabilities that can be exploited by attackers that may not be obvious in the context of your project. It is important to make sure that your dependencies are up to date so that you're taking advantage of the latest security patches.

The easiest way to limit the number of security vulnerabilites that you need to resolve for your project is to diligently keep your dependencies up to date.

## What you can do locally

### Node.js

Yarn is a package manager for Node.js that supports the `yarn audit` command. This command will scan your dependencies for known vulnerabilities and generate a report. You can also use the `yarn why` command to see why a particular dependency is installed which can aid in the process of resolving vulnerabilites in transitive dependencies.

The `yarn outdated` command will list all of the dependencies that are out of date which can also come in handy here.

### .NET

The .NET CLI has a command called `dotnet list package` that will list all of the packages that your project depends on. You can use this command to see if any of your dependencies have known vulnerabilities with `dotnet list package --vulnerable`. You can also use the `dotnet outdated` command to see if any of your dependencies are out of date.

## Checking for Vulnerabilities in CICD

### OWASP Dependency Check

We can use a tool called [OWASP Dependency Check](https://owasp.org/www-project-dependency-check/) to scan our dependencies for known vulnerabilities. This tool is free and open source and can be run locally or integrated into your CI/CD pipeline.

This tool works by building a list of all of the dependencies in your project and then comparing them to a database of known vulnerabilities. It will then generate a report of any vulnerabilities that it finds. The most well known database of vulnerabilities is the [National Vulnerability Database](https://nvd.nist.gov/), but this tool also checks [the Github Advisory Database](https://github.com/advisories) and the [Sonatype OSS Index](https://ossindex.sonatype.org/).

It's important to note here that the security scan identifies vulnerabilities within your project by looking at the version numbers of the dependencies that you're using. It does not actually scan the code of your dependencies to see if they contain any malicious code. This is a different type of security scan called a [Software Composition Analysis](https://en.wikipedia.org/wiki/Software_composition_analysis) (SCA) scan.

The results of this scan can be used to cause a pipeline to fail if any vulnerabilities are found. This is a good way to prevent vulnerabilities from being introduced into your project.  

If you want to explore this tool further you can run it locally with this powershell script:

```powershell
$date = Get-Date -Format "yyyy-MM-dd-HH-mm"

docker run `
--rm `
-v ${PWD}/src:/src `
-v ${PWD}/report:/report `
-u root `
owasp/dependency-check:8.0.1 `
--scan /src/**/yarn.lock `
--scan /src/**/package.json `
-o /report/$date `
-f JUNIT `
-f HTML
# use the `-n` flag to skip the update of the NVD database for speed.
# You can't use this flag the first time you run the image.
```

This script will scan the `src` directory for `yarn.lock` and `package.json` files and generate a report in the `report` directory. You can then open the `report.html` file in your browser to view the report.

## CycloneDX Generator

[The cdxgen node package](https://github.com/CycloneDX/cdxgen) can be used to generate a bill of materials (BOM) for your project that meets [the CycloneDX standard](https://cyclonedx.org/). This report can be generated in XML or JSON format and contains all of the dependencies for your project (including transitive dependencies.)

This `bom.json` file can be generated in your CI when the project is built and uploaded somewhere and associated with your build artifact. When the artifact is deployed the `bom.json` file can be uploaded to a tool like [Dependency Track](https://dependencytrack.org/) which can be used to track the dependencies of your project and alert you when vulnerabilities are found. This tool will regularly poll the NVD database and alert you when new vulnerabilities are found for the dependencies that are being used in the version of your project that is currently deployed.

