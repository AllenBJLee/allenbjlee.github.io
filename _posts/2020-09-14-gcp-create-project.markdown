---
layout: post
title:  "Google Cloud Platform 시작하기 (프로젝트 생성 - Ubuntu 16)"
search: true
category: GCP
last_modified_at: 2020-09-14
---

새로운 프로젝트를 생성하고 Ubuntu 16.04 를 설치해 보도록 한다.

우선 GCP Console 화면 상단 중앙즘 My First Project 를 선택해 새로운 프로젝트를 생성한다.

![프로젝트 메뉴 선택]({{site.url}}/public/img/gcp/create_project/gcp_create_project_01.png)

새 프로젝트 메뉴 선택

![새 프로젝트]({{site.url}}/public/img/gcp/create_project/gcp_create_project_02.png)

프로젝트 이름을 입력하고 '만들기' 버튼을 클릭한다. (여기엔 Ubuntu 16.04 와 ELK 를 설치해볼 예정이기 때문에 이름을 ubuntu-16-LTS-ELK 로 지었다)

![프로젝트 이름]({{site.url}}/public/img/gcp/create_project/gcp_create_project_03.png)

프로젝트가 만들어지면 GCP Console 에서 'My First Project' 메뉴를 선택해 방금 생성한 프로젝트를 선택해 현재 프로젝트를 변경한다.

![프로젝트 선택]({{site.url}}/public/img/gcp/create_project/gcp_create_project_04.png)

다음으로 Ubuntu 16 을 설치하기 위해 'Compute Engine' 메뉴에서 'VM 인스턴스' 메뉴를 선택하면 최초 인스턴스 구성을 위해 약 1분정도 대기하게 된다.

![VM 인스턴스 설치]({{site.url}}/public/img/gcp/create_project/gcp_create_project_05.png)

VM 인스턴스 준비가 완료되면 '만들기' 버튼을 이용해 인스턴스 생성 준비한다.

![프로젝트 메뉴 선택]({{site.url}}/public/img/gcp/create_project/gcp_create_project_06.png)

우리는 Ubuntu 16.04 를 설치할 예정이기 때문에 이름은 'ubuntu-16-04' 로 설정했고 시간대 설정은 '서울'로 지정했다.

![인스턴스 이름 설정]({{site.url}}/public/img/gcp/create_project/gcp_create_project_07.png)

스크롤을 아래로 내려보면 '부팅 디스크'가 debian 으로 되어있는 것을 확인할 수 있다. '변경'버튼을 이용해 ubuntu 16 으로 OS 를 변경해준다.

![부팅 디스크 변경]({{site.url}}/public/img/gcp/create_project/gcp_create_project_08.png)

버튼을 누르면 이렇게 운영체제를 선택할 수 있다. (CentOS 가 편한 사람은 CentOS 를 선택해도 된다.)

![프로젝트 메뉴 선택]({{site.url}}/public/img/gcp/create_project/gcp_create_project_09.png)

부팅 디스크 변경이 완료됐으면 '만들기'버튼을 클릭해 인스턴스가 생성되는 것을 확인할 수 있다.

![인스턴스 생성]({{site.url}}/public/img/gcp/create_project/gcp_create_project_10.png)

인스턴스 생성이 완료되면 '연결'탭에서 원하는 방식으로 연결을 시도해 설치된 OS 에 접근할 수 있게 된다.

![SSH 접속 시도]({{site.url}}/public/img/gcp/create_project/gcp_create_project_11.png)

SSH 로 접속한 모습

![프로젝트 메뉴 선택]({{site.url}}/public/img/gcp/create_project/gcp_create_project_12.png)