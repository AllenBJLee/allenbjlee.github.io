---
layout: post
title:  "내가 경험한 애자일(2) - 발견되는 문제점들"
search: true
categories: Agile
last_modified_at: 2020-09-25
---

**그래서 애자일을 시작하게 되었다**<br>
앞서 얘기했던 여러 문제점들 때문에 애자일 도입을 시도하게 되었다.<br>
하지만 팀내에 애자일 프로세스를 경험해본 사람이 없었기 때문에 여러 시행착오를 격게 된다.
<br>
<br>

**1. 길어지는 데일리 미팅**<br>
데일리미팅을 얼마나 잘 활용하느냐에 따라서 스프린트의 성공 여부가 갈릴 만큼 애자일에 있어서 데일리 미팅은 상당히 중요한 요소중 하나이다. 그만큼 중요하지만 제대로 활용하지 못한다면 발생할 수 있는 문제점은 다수가 된다.<br>

1. 미팅 시간이 길어진다.
* 데일리 미팅의 기본 원칙은 짧게 한일, 할일, 장애요소만 얘기하고 지나가는 것을 원칙으로 하고 있다. (번다운 차트를 기록하기 위한 Story Point 감점도 포함된다.)
* 하지만 처음 접해본 데일리 미팅에서 모두가 '한일, 할일, 장애요소'를 장황하게 설명한다면 데일리 미팅이 자칫 장시간 회의로 번질 수 있다.
* 그리고 우리는 매일같이 '장시간 회의'를 한동안 진행해야 했다.
2. 미숙한 스크럼 마스터
* 데일리 미팅에선 최대한 짧게 얘기하는게 원칙이지만 세세하게 진행상황을 체크하길 원하는 스크럼 마스터의 경우 데일리 미팅을 자칫 보고의 자리로 활용하는 경우가 생긴다.
3. 누적되는 피로
* 결국 위와 같은 상황 때문에 데일리 미팅은 피로도만 가중시키고 정작 개발을 위한 개발 시간은 단축되어 야근으로 이어지는 악순환이 반복될 수 있다.
<br>

**2. 업무 분배 실패**<br>
스프린트 시작 전 '계획회의'를 통해 각자 스프린트 내에서 진행할 업무를 할당받게 된다. 하지만 개발자는 모두 개발 역량이 다르기 때문에 각자 진행할 수 있는 업무량은 정해져 있다.<br>
새로운 기능을 개발하기 위해선 얼마의 시간이 필요한지 누구도 예측할 수 없기 때문에 스토리 포인트를 적용하기란 쉽지 않은 일이다.<br>
더 나아가 기능 정의가 명확하지 않다면 '기능 정의'부터 스토리에 포함시켜 진행해야 하는데 그걸 무시한다면 결국 야근으로 이루어지는 패턴이 된다.<br>

1. 결국 새로운 기능을 개발할 땐 짧게 짧게 '기능 정의' 및 '파일럿' 은 필수가 되어야 한다고 생각한다.
* 기능을 어떻게 개발할지 정의하지 않고 바로 개발한다면 결국 애자일 이전의 개발 방법론에서 하던 문제를 답습하게 된다. (야근...)
<br>

**3. CI/CD 의 문제**<br>
애자일 방법론 적용을 위한 가장 기본이라고 생각하는 CI/CD 가 구축되어 있지 않다면 결국 모든걸 제대로 지켜도 야근을 할 수 밖에 없다고 생각한다.<br>
기능을 아무리 열심히 잘 만들어도 QA에서 통과되지 않는다면? 그 기능은 다시 개발해야 하고 스프린트를 넘겨 진행하거나 야근을 해야 한다.<br>
앞서 말했듯 마일스톤 단위 개발을 진행할 경우 QA는 항상 개발이 완료된 후에 진행되게 된다.<br>
우리는 이 문제점을 개선하기 위해 스프린트 중에도 개발이 완료된 기능이 있다면 QA를 진행하도록 했으나 생각처럼 잘 진행되진 않았다.<br>

1. QA 가 늦을 수 밖에 없는 시스템 한계
* 결국 스프린트 일정은 1~3주로 정의되어 있고 개발자들은 열심히 개발하지만 스프린트 마지막에 완료되는 기능들은 QA에겐 Bottle Neck이 된다.
2. 스프린트 막바지에 완료되는 기능들은 QA 실패시 결국 다음 스프린트로 넘겨야 한다.
<br>

**결국 야근의 반복**<br>
이런 문제점들이 지속되면서 굳이 애자일을 할 필요가 있는지? 혹은 우리가 개발하는 엔진은 애자일 방법론으론 진행할 수 없는지에 대한 의문점만 남기면서 애자일은 쓸데 없다는 생각만 남기게 되었다.<br>

**연결된 포스트**<br>
[내가 경험한 애자일(1) - 왜 애자일을 시작했을까?]({% post_url 2020-09-20-Agile-Start %}) <br>
[내가 경험한 애자일(3) - 문제 해결하기]({% post_url 2020-09-26-Agile-Solution %})