# IN-260 - Migrate Your SAP Process Orchestration Scenarios to SAP Integration Suite

## Description

This repository contains the material for the SAP TechEd 2026 session called IN-260 - Migrate Your SAP Process Orchestration Scenarios to SAP Integration Suite.

## Overview

This session introduces attendees to the migration assessment and migration tooling supporting you in the transformation from SAP Process Orchestration to SAP Integration Suite.

The [**Migration Assessment**](https://help.sap.com/docs/integration-suite/sap-integration-suite/migration-assessment) capability of SAP Integration Suite lets you evaluate how various integration scenarios might be migrated and allows you to estimate the technical efforts involved in the migration process.

After having finished the assessment of the current SAP Process Orchestration landscape, the next step is the actual migration. The goal of the [**Migration Tooling**](https://help.sap.com/docs/integration-suite/sap-integration-suite/migration-tooling) is to provide a semi-automated migration where interfaces in the Cloud Integration capability of SAP Integration Suite will be automatically created based on SAP Process Orchestration information, so that ideally 60-70% of technical migration efforts are automated. The migration of scenarios is based on a pattern approach, which means that integration flow templates following a specific pattern are used as skeleton to migrate the content and create the final integration flows.

In this session, we use the [**Integration Content Migration Agent**](https://help.sap.com/docs/integration-suite/isuite-integrations-and-apis/java-mapping) that helps you migrate custom Java-based mapping artifacts used in SAP Process Orchestration integration scenarios to groovy scripts in Cloud Integration capability of SAP Integration Suite. While migrating integration scenarios via the migration tooling, this agent generates Groovy script equivalents for supported java mappings, helping accelerate modernization efforts and reduce manual redevelopment activities.

Furthermore, we leverage the [**Pipeline for Cloud Integration**](https://help.sap.com/docs/migration-guide-po/migration-guide-for-sap-process-orchestration/pipeline-concept) which is supported within the migration tooling. The Pipeline for Cloud Integration allows you to set up your integration scenarios in Cloud Integration similarly to how messages are processed in SAP Process Orchestration — in pipelines. It serves as a framework for handling asynchronous messages with out-of-the-box support for exactly-once delivery, error handling, and sophisticated restart capabilities. For an introduction to the Pipeline for Cloud Integration, check out this [blog post](https://community.sap.com/t5/technology-blog-posts-by-sap/introducing-the-new-pipeline-concept-in-cloud-integration/ba-p/13639651).

## Requirements

To follow the exercises in this repository you need access to an SAP Integration Suite tenant. The instructors will provide you with the tenant URL, your user number, and the password during the session.

We provide the following SAP Integration Suite tenants. The instructors will let you know which tenant to use.

- Option 1: Workshop tenant [**workshop01a**](https://workshop01a.integrationsuite-cpi049.cfapps.eu10-003.hana.ondemand.com/shell/home)
- Option 2: Workshop tenant [**workshop01b**](https://workshop01b.integrationsuite-cpi050.cfapps.eu20-002.hana.ondemand.com/shell/home)

Access:
- Use the user **userXX** with **XX** from 01 to 45.

**Note**: Please replace **XX** with the ID assigned to you. The ID and the password will be provided by the instructors during the session.

```yaml
userXX
```

## Prerequisites

As a prerequisite for the migration tooling exercises, you first need to run through the following preparation steps.
- [Exercise 0 - Prepare the Migration Exercises](exercises/ex0/)

## Exercises

The following list covers all exercise steps. Use this section as an index or table of contents. Use the breadcrumb navigation on top of the pages to go back to the Table of Contents. You can run through all the exercises independently.

If you are interested in how the **Migration Assessment** works, start with the first exercise.
- [Optional Exercise 1 - Migration Assessment](exercises/ex1/)

For running through the **Migration Tooling** with a Point-to-Point scenario containing a **mapping with function library**, continue with Exercise 2.
- [Exercise 2 - Migrate a Point-to-Point Scenario](exercises/ex2/)

For running through the **Migration Tooling** with a Content-Based Routing scenario leveraging the **Pipeline for Cloud Integration**, continue with Exercise 3.
- [Exercise 3 - Migrate a Content-Based Routing Scenario](exercises/ex3/)

For running through the **Migration Tooling** with a Java Mapping scenario leveraging the **migration agent**, continue with Exercise 4.
- [Exercise 4 - Migrate a Java Mapping Scenario](exercises/ex4/)

## Contributing

Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct

Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to Obtain Support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License

Copyright (c) 2026 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
