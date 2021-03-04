---
layout: post
title:  "악성코드 Visualize - 01"
search: true
date: 2021-03-04
category: DataScience
---

### 1. 악성코드의 행동들
악성코드의 행동은 여러 형태들이 모여 악성행위가 되곤 한다.
최근들어 좀 잠잠해 졌지만 여전히 사용자에게 가장 큰 타격을 주는 악성코드인 랜섬웨어를 이용해 설명하면 다음과 같다.

1. FileA 는 브라우저에서 다운로드 된다. (사용자의 의도와 상관없이)
2. 사용자는 FileA 를 실행한다. (ProcessA 생성)
3. ProcessA 는 CommandCenter 로부터 악성행위를 수행할 FileB를 다운로드 한다.
4. ProcessA 는 svchost 에 인젝션을 수행한다.
5. svchost 에 인젝션된 FileB(DLL)은 내 컴퓨터 내에 있는 모든 문서파일을 암호화 한다.

위 패턴은 이제는 좀 지난 초창기 랜섬웨어의 동작방식이지만 설명하기 적절해보이기도 하고, 너무 요즘 트렌드의 악성코드를 논하면 업무상 비밀을 누설할 수도 있을 것 같아 이정도가 적당한 것 같다.

### 2. 행동의 표현
위에서 본 것 처럼 랜섬웨어는 악성 행위를 하기위해 여러가지 행동을 한다.
여기서 위의 예시처럼 1~5개로 분리한건 한번의 행동을 표현하기 위해서다. 이 행동들을 Graph 관점으로 풀이하면 다음과 같이 변경할 수 있다.<br><br>


|CurrentProcess|Action|Target|
|:--------|:----|:----|
|브라우저|Download (from WebSize)|FileA|
|Explorer|Execute Process|ProcessA (as FileA)|
|ProcessA|Download (from Command Center)|FileB|
|ProcessA|Inject|svchost (with FileB)|
|svchost|Encrypt|Documents|


<br><br>이처럼 한번의 행동은 CurrentProcess 가 Target에 어떠한 Action을 취했다로 표현할 수 있게 된다.

### 3. Visualize 의 필요성
이런 결과들을 수집하고 분석할 수 있다면 악성코드의 변종이 나와도 대응할 수 있다고 생각한다. 그런 의미에서 데이터 Visualize 꼭 필요한 기능이라고 생각한다. 프로세스의 행동은 한번에 하나씩 표현이 가능하고 위의 예시처럼 단순하게 나열한 것도 5번의 행동을 기반으로 악성행위를 완성하게 된다. 실제 행동은 더 복잡할 수 밖에 없다는건 말하지 않아도 이해할 것이라 생각한다.<br><br>

그렇다면 악성행동을 분석하기 위해 항상 행동 데이터를 로그형태로 받는다면 사막에서 실을 찾아 엮어나가는듯한 느낌으로 악성코드의 악성행동들을 찾아 행동들의 연관성을 이어나가야 한다. 만약 이런 행동들이 가시화된 그래프 혹은 더 좋은 방법으로 표시된다면, 악성행동을 분석하기에 더 좋은 환경이 되지 않을까 한다.


### 4. Visualize 를 위한 데이터
아래와 같은 로그성 데이터가 있다고 가정해보자.
```j
{
	"result" : [
		{
			"current_pid" : 1,
			"current_filename" : "chrome",
			"action_id" : 10,
			"action_desc" : "download",
			"target_type" : "file",
			"target_name" : "fileA"
		},
		{
			"current_pid" : 2,
			"current_filename" : "explorer",
			"action_id" : 20,
			"action_desc" : "execute process",
			"target_type" : "process",
			"target_pid" : 3,
			"target_name" : "fileA"
		},
		{
			"current_pid" : 3,
			"current_filename" : "fileA",
			"action_id" : 10,
			"action_desc" : "download",
			"target_type" : "file",
			"target_name" : "fileB"
		},
		{
			"current_pid" : 3,
			"current_filename" : "fileA",
			"action_id" : 30,
			"action_desc" : "inject",
			"target_type" : "file",
			"target_pid" : 4,
			"target_name" : "svchost"
		},
		{
			"current_pid" : 4,
			"current_filename" : "svchost",
			"action_id" : 40,
			"action_desc" : "encrypt",
			"target_type" : "file",
			"target_name" : "documents"
		}
	]
}
```

단순화한 로그지만 실제 행동들을 표현한 것이긴 하다. 그렇다 하더라고 이런 로그성 데이터를 보고 악성 행동을 유추해내고 어떤 파일이 문제인지 찾아내긴 여간 어렵지 않다. 이걸 가시화하면 좀 더 보기 편해진다.

### 5. Visualize 과정 예시

```python
import json
import pandas as pd

import networkx as nx
import matplotlib.pyplot as plt

# 샘플 데이터 로드
with open('data/sample.json', encoding='utf-8' ) as f :
    jsonData = json.load( f )

df = pd.DataFrame(jsonData["result"])
```

```python
# 그래프 선언
pid_gh = nx.DiGraph()
```


```python
nodeset_pid = set()
nodeset_file = set()

# 데이터에서 노드와 엣지 정보를 추출해 입력
for i, row in df.iterrows( ) :
    node_from = row.current_pid
    
    nodeset_pid.add( node_from )
    
    if 10 == row.action_id :
        node_to = row.target_name
        nodeset_file.add( node_to )
    elif 20 == row.action_id :
        node_to = int(row.target_pid)
        nodeset_pid.add( node_from )
    elif 30 == row.action_id :
        node_to = int(row.target_pid)
        nodeset_pid.add( node_from )
    elif 40 == row.action_id :
        node_to = row.target_name
        nodeset_file.add( node_to )
        
    print( 'from({}) action({}) to({})'.format( row.current_pid, row.action_id, node_to ) )
    pid_gh.add_edge( row.current_pid, node_to, weight=row.action_id )
```

    from(1) action(10) to(fileA)
    from(2) action(20) to(3)
    from(3) action(10) to(fileB)
    from(3) action(30) to(4)
    from(4) action(40) to(documents)
    {1, 2, 3, 4}
    {'fileB', 'documents', 'fileA'}
    


```python
pos = nx.shell_layout(pid_gh)

nx.draw( pid_gh, with_labels=True, pos=pos )

labels = nx.get_edge_attributes(pid_gh, 'weight')
nx.draw_networkx_edge_labels(pid_gh, pos, edge_labels=labels)

nx.draw_networkx_nodes(pid_gh, pos, nodelist=list(nodeset_pid), node_color='#FF1744', node_size=500)
nx.draw_networkx_nodes(pid_gh, pos, nodelist=list(nodeset_file), node_color='#1744FF', node_size=500)

# 그래프로 시각화
plt.show()
```
![png]({{site.url}}/public/img/visualize/visualize.png)

간단하게 그래프로 그려보면 PID 3인 프로세스가 FileB 를 다운로드받고 PID 4(svchost)에 Injection 한 후 악성행위(encrypt)를 한 주범 중 하나라는게 한눈에 보이게 된다.

### 6. 결론
이렇게 간단하게 표현해도 Visualize(시각화, 도식화)는 중요한 분석 자료로 쓰일 수 있다는 것을 알 수 있다.<br><br>

사용자의 데이터는 점점 더 많아지고(big data) 그 속에서 데이터를 분석해야 한다면(Data Analysis) Visualize은 피할 수 없는 하나의 도구가 되어야 할 것 같다.