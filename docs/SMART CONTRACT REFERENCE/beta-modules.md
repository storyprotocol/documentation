---
title: Modules
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Modules are internal logics encapsulated into individual contracts, responsible for atomic interactions in the protocol. For example, the royalty module handles all interactions involving royalty, while the licensing module handles all interactions involving licensing. 

Modules can interact with other modules as long as either (1) the IPAccount owner grants permission for a module-to-module function call; or (2) the global permission for a module-to-module function call has been granted by the protocol admin.