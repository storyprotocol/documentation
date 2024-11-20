---
title: Open Data Access
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Introduction

Think in “**Programmable IP”**: In the realm of IP, the concept of Programmable IP represents a paradigm shift, enabling users/developers to write their own programs to operate and manipulate IP data. This approach hinges on two fundamental elements: **data** and **functions**. By enabling users to program customized functions for data operation via a unified standardized approach, Story Protocol opens up a new avenue for IP data utilization. 

1. "Greater openness in IP data access is preferable to restricted IP data access."
2. "More equitable access is superior to privileged data access control."

This approach simplifies and accelerates the development of a robust Programmable IP ecosystem by third-party and community developers.

### Core Principles

The success of Programmable IP rests on two key principles:

1. **Open Data Access**: Instead of restricting data access, Story Protocol advocates for open data access. This means no super modules; all modules should have open access to data under the same rules. 
2. **Equal Access**: Every participant in the system should have equal opportunity to access and manipulate data. There should be no privileged access or modules with disproportionate control over IP data. This equality fosters a fairer environment for IP program development.

## Implementing Open Data Access

To achieve true openness and equality in data access, Story Protocol proposes the following structure:

- **Uniform Access Rules**: All modules access data following the same set of rules, ensuring fairness and transparency in data manipulation.
  - **Any module can write any data into any IPAccount, but can only write its own data.**
  - **Any module can read any data from any IPAccount**
- **Data Ownership Protocols**: Only the owner of the data can write/modify it.  However, anyone can read _any_ data.

## (Post-Beta, Future Implementation) IPAccount Centric Data Model

_NOTE THAT THE NAMESPACE STORAGE FEATURE WILL BE INTRODUCED IN A FUTURE RELEASE. **FOR BETA, WE DO NOT SUPPORT THE NAMESPACE STORAGE** DESCRIBED BELOW._

### NameSpace Storage and Ownership

As a structured storage, an IPAccount:

- Enforces data ownership access rules.
- Standardizes data access and sharing between modules (from IPAsset to module namespace to data).
- Identifies clear data ownership.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1f2395e-Screenshot_2024-01-29_at_22.26.05.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


### Permissionless Access

- **IPAccount NameSpace Storage**: Story Protocol uses the module address as a namespace and ensures that any data access passes through its respective module in IPAccount. Any module can write any data into IPAccount under its own namespace.
- **Autonomy Data Access Control**: Each module has the autonomy to decide how to open its data write access. Each module can read data of other modules in the IPAccount.
- **User/App Selecting Data Source**: Users or applications have the discretion to trust data from specific modules based on their own criteria.