---
title: "git branch, pull request크기 나누기"
excerpt: "pull request, 어느 크기로 나누는게 적절한가"

categories:
 - TIL
tags:
 - TIL
 - Git
last_modified_at: 2020-02-25
---



* 협업에서의 Pull Request를 생각해보면 크게 두가지가 생각난다.
  1. 배포 버전과 테스트 버전 - master에 안정화된 배포버전을 두고 다른 브랜치에 그 외에 다른 테스트 버전을 두어 안정화를 진행 후 master 브랜치에 merge하는 방식
  2. 토픽 브랜치 - 어느 프로젝트에서 새로 생각나는 기능이나 주제, 작업을 브랜치를 만들어서 작업을 수행한다. 또 생각나는 기능이 있으면 또 브랜치를 만들어서 또 작업한다. 브랜치 사이를 옮겨다니는것은 매우 쉽다.

* 둘 다 공통된 점을 생각해보면, 프로젝트에서의 주제별로 브랜치를 만드는것이다. 배포버전에서 새로운 테스트를 해보기위해 브랜치를 만들거나, 기능 추가를 위해 브렌치를 만든다. 
* 브랜치를 만들어서 해당 주제에 맞는 작업을 수행 - Pull Request - Code Review나 conservation - 다시 PR - ... - Merge
* 즉, 주제에 맞는 크기로 Pull Request를 나누는것이 적절하다고 생각한다.

