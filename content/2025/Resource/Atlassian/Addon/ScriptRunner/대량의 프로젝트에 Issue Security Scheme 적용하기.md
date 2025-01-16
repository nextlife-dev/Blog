---
title: 대량의 프로젝트에 Issue Security Scheme 적용하기
description: 
aliases:
  - Seungwoo Ham
tags:
  - atlassian
  - addon
  - script
date: 2025-01-01
---
기존 프로젝트에 Issue Security Scheme 적용할 일이 생겼습니다. 일일이 적용하기에는 반복적인 일을 싫어했습니다.

저는 Jira에 ScriptRunner가 있고, 이를 활용하여 대량의 프로젝트에 손 쉽게 적용해볼 수 있을 거라는 생각이 퍼뜩였죠.

## 구현

```groovy
for (project : specialProjects) {
    issueSecuritySchemeManager.setSchemeForProject(project, subDefectIssueSecurityScheme.getId())
}
```

issue security scheme manager 관련 클래스가 있습니다. -- https://docs.atlassian.com/software/jira/docs/api/7.0.5/com/atlassian/jira/issue/security/IssueSecuritySchemeManager.html

위의 사이트에서 `setSchemeForProject` 메소드를 활용해 해당 프로젝트에 Issue 보안 적용시킬 수 있다는 점을 알았습니다.
