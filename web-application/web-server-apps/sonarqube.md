---
description: Pentesting SonarQube
---

# SonarQube

> SonarQube is an open-source platform developed by SonarSource for continuous inspection of code to detect bugs. Unauthorized access might lead to source code access, potentially obtaining usernames, passwords, internal URLs...&#x20;

#### No authentication needed

Sometimes, you found that SonarQube does not require authentication to access. It simply allows you to freely browse information through the application and obtain sensitive information such as users, passwords, shared network paths, business logic, among others from the source code of the applications.

#### Juicy Endpoints

If the application requires authentication, try to check this endpoints as unauthenticated user:

```
/projects/
/issues/
/api/users/search/
```
