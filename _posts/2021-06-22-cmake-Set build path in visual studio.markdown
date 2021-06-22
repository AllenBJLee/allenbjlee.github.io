---
layout: post
title:  "Visual Studio cmake - 빌드 경로 지정"
search: true
date: 2021-06-22
category: cmake
---

### 빌드 경로 지정법
1. CMakeSettings.json
 - buildRoot 에 설정할 수 있다.
```json
"buildRoot": "${workspaceRoot}\\build\\${name}"
```
