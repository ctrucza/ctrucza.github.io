---
layout: post
title:  "My way to DAL"
date:   2014-08-19 12:15:41
categories: 
---

Repositories
---
- decoupling
- lots of interfaces
- wrong place for decoupling

Clean Architecture
---
- database
- DAL
- business
- isolation between the DAL and database

DAL
---
- one or more repositories
- data access operations

Storage
---
- this is what you want to isolate
- so you can mock it
- one interface, two implementations
- MockStorage
- EFStorage

Dependency Inversion
---
- Injection a.k.a. passing parameters
- proliferation of parameters
- Service Locator

Poor Man's Service Locator
---
- DalLocator
- threading 
- lifetime
- unit of work