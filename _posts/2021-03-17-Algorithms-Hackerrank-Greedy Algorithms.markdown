---
layout: post
title:  "Hackerrank - Greedy Algorithms"
search: true
date: 2021-03-17
category: Algorithms
---

### 1. Minimum Absolute Difference in an Array
난이도 : <span style="color:green">easy</span>

링크 : [Minimum Absolute Difference in an Array](https://www.hackerrank.com/challenges/minimum-absolute-difference-in-an-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms){:target="_blank"}

> 문제

주어진 int형 배열의 요소를 두개씩 묶어 두 숫자의 차이값중 가장 작은 값을 찾는다.<br>
ex)<br>
```
input
 { 3, -7, 0 } 
```
```
output
 3
```
{ 3, -7, 0 } 은 (3,-7), (3,0), (-7,0) 으로 묶이며
 - |3 - -7| = 10
 - |3 - 0| = 3
 - |-7 - 0| = 7
이므로 3이 정답이다.

> 풀이

 - 두 숫자 차이가 가장 작은 경우를 찾는 것이기 때문에 배열을 정렬하고
 - 인덱스를 증가시키며 숫자를 뺐을 때 가장 작은값을 찾으면 해결된다.

```cpp
int minimumAbsoluteDifference(vector<int> arr) {
    sort( arr.begin(), arr.end() );
    
    int nResult = 0;
    
    int nSize = arr.size();
    
    nResult = abs(arr[0] - arr[1]);
    
    for( int i = 1; i < nSize - 1; ++i ) {
        if( nResult > abs( arr[i] - arr[i+1] ) ) {
            nResult = abs( arr[i] - arr[i+1] );
        }
    }
    
    return nResult;
}
```


### 2. Luck Balance
난이도 : <span style="color:green">easy</span>

링크 : [Luck Balance](https://www.hackerrank.com/challenges/luck-balance/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms){:target="_blank"}

> 문제

중요한 컨테스트중 주어진 K 만큼 이기고 나머지 중요한 컨테스트에선 이기지 못한다고 가정했을 때 가장 높은 luck balance 는 얼마인지 계산해보자.

> 풀이

 - 우선 모든 luck 포인트를 합치고
 - (중요한 경기수 - 무조건 이겨야 하는 대회) 만큼 가장 작은값을 뺀다.
 
```cpp
int luckBalance(int k, vector<vector<int>> contests) {
    int nLuckResult = 0;
    int nImportCnt = 0;
    
    vector<int> nVecImportant;
    
    nVecImportant.reserve( contests.size() );
    
    for( auto cont : contests ) {
        if( 1 == cont[1] ) {
            // 중요한 컨테스트
            nVecImportant.push_back( cont[0] );
            ++nImportCnt;
        }
        
        nLuckResult += cont[0];
    }
    
    sort( nVecImportant.begin(), nVecImportant.end() );
    
    for( int i = 0; i < nImportCnt - k; ++i ) {
        // luck 전체를 더할 때 한번 구했으므로 2배를 뺀다.
        nLuckResult -= ( nVecImportant[i] * 2 );
    }
    
    return nLuckResult;
}
```

### 3. Greedy Florist
난이도 : <span style="color:orange">Medium</span>

링크 : [Greedy Florist](https://www.hackerrank.com/challenges/greedy-florist/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms){:target="_blank"}

> 문제

 - 꽃의 가격 배열 C 가 주어지고 친구 K 명이 주어진다.
 - 배열로 주어진 꽃 C 는 친구들이 모두 사야한다. 단 다음에 제시된 조건을 만족하며 가장 작은 금액으로 꽃을 사야한다.
 -- 조건 : 각자 꽃을 구매할 때 다음과 같은 요금을 받는다.
 --- 꽃 요금 * ( 이전 꽃 구매 회수 + 1 )

ex)
```
input
c={2, 5, 6}
```
```
output
15
```
꽃은 3종류이고 사람은 2명이기 때문에 한명은 꽃을 2종류를 사고 한명은 한종류를 사야한다. 가장 비싼꽃 6과 5는 각각 6 * ( 0 + 1 ), 5 * ( 0 + 1 ) 로 구매하고, 두번째 꽃을 구매하게 될 땐 2 * ( 1 + 1) 이 되므로 6 + 5 + 4 = 15 이다.

> 풀이

 - 꽃 가격을 내림차순으로 정렬하고
 - Index 를 증가시키며 K 로 나눠 구매회수를 구해 계산하고 합산해주면 된다.
 
```cpp
int getMinimumCost(int k, vector<int> c) {
    // 가격을 내림차순으로 정렬한다.
    sort( c.begin(), c.end() );
    reverse( c.begin(), c.end() );
    
    int nTotal = 0;
    
    int nSize = c.size();
    
    // 가장 비싼 꽃 부터 선택하며 금액을 계산한다.
    for( int i = 0; i < nSize; ++i ) {
        nTotal += ( i / k + 1 ) * c[i];
    }
    
    return nTotal;
}
```

### 4. Max Min
난이도 : <span style="color:orange">Medium</span>

링크 : [Max Min](https://www.hackerrank.com/challenges/angry-children/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms){:target="_blank"}

> 문제

 - arr 의 요소를 k 만큼 선택해 최대값과 최소값을 구한 후
 - 최대값 - 최소값이 가장 작은 수를 구해보자
 
> 풀이

 - 숫자를 정렬하면 k 만큼 선택할 때 index 를 움직이며 값차이를 계산하기 편해진다.
 - 정렬 후 index 를 이동하며 값의 차이가 최소일때를 구하면 된다.
 
```cpp
int maxMin(int k, vector<int> arr) {
    sort( arr.begin(), arr.end() );
    
    int nSize = arr.size();
    int nMinResult = arr[k-1] - arr[0];
    int nCalc = 0;
    
    for( int i = k; i < nSize; ++i ) {
        nCalc = arr[i] - arr[i-k+1];
        if( nMinResult > nCalc ) {
            nMinResult = nCalc;
        }
    }
    
    return nMinResult;
}
```

### 5. Reverse Shuffle Merge
난이도 : <span style="color:red">Advanced</span>

링크 : [Reverse Shuffle Merge](https://www.hackerrank.com/challenges/reverse-shuffle-merge/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms){:target="_blank"}

> 문제

Greedy Algorithms 문제들 중 가장 애먹은 문제이다.

 - 문자열 A 가 주어지고 A1 = reverse(A), A2 = shuffle(A) 인 상황
 - S = merge( A1, A2 ) 이며, 조건을 만족하는 S 의 사전적 순서 최소값을 찾는 문제
 
여기서 예제로 제시해준 것들은 A1(reverse(A)) 사이에 A2(shuffle(A)) 한 값들이 껴들지 않았다. 그래서 A1 은 온전히 긴 통 문자열로 보존된다고 생각했다.

> 풀이 1 (오류)

 - 문자열은 모두 소문자이므로
 - 26개 배열을 만들고 char 를 찾아 0-a, 1-b, ... 25-z 에 맞춰 개수를 확인한다.
 - 문자열 A 가 두개이기 때문에 모든 char 는 A 요소의 *2 개 만큼 존재하게 된다.

 - A 를 구성하는 char 모두를 만족할 경우 원하는 문자열이 된다.
 - 뒤에서부터 순차적으로 Elements Vector 의 조건을 만족할때만 string 에 추가해
 - 모든 조건에 만족하는지 확인해본다.

```cpp
string reverseShuffleMerge(string s) {
    string strResult;
    
    vector<int> nVecElements( 26, 0 );
    vector<int> nVecComp( 26, 0 );

    // 요소를 찾는다.
    for( const auto& c : s ) {
        ++nVecElements[c-'a'];
    }
    
    // 모든 요소는 1/2 개 만큼 존재해야 A 를 만족하므로 개수를 1/2로 모두 나눈다.
    for( auto& i : nVecElements ) {
        i = i / 2;
    }
    
    int nStrALength = s.size() / 2;
    
    string strCmp;
        
    int arrComp[26];
    int nIdx = 0;
    char chNow = '\0';
    
    bool bMatched = false;
    
    // reverse 된 문자열을 찾아야 하기 때문에
    // 마지막 문자에서부터 비교해본다.
    for( int i = s.size() - 1; i  >= nStrALength - 1; --i ) {
        nVecComp = nVecElements;
        bMatched = true;
        
        strCmp.clear();
        strCmp.reserve( nStrALength );
        
        // 마지막 문자열에서 하나씩 선택
        for( nIdx = 0; nIdx < nStrALength; ++nIdx ) {
            chNow = s[i - nIdx];
            strCmp.push_back( chNow );
            if( 0 > --nVecComp[chNow-'a'] ) {
                break;
            }
        }
        
        // 만들어진 문자열이 조건에 일치하는지 파악한다.
        for( const auto& i : nVecComp ) {
            if( 0 != i ) {
                bMatched = false;
                break;
            }
        }
        
        if( false == bMatched ) {
            // 찾아낸 문자열이 조건에 일치하지 않으므로 다시 찾는다.
            continue;
        }
        else {
            if( 0 == strResult.length() || strResult > strCmp ) {
                strResult = strCmp;
                
                //printf( "%s\n", strResult.c_str() );
            }
        }
    }

    return strResult;
}
```
샘플 코드들은 당연히 다 통과했으나 hidden 테스트에서 실패해 몇개 unlock 해서 확인해보니 merge( A1, A2 ) 할 때 A1 사이 사이에 A2 가 끼어있다는 것을 발견했다.

> 풀이 2

결국 reverse 된 문자열 사이 사이에 shuffle 이 일어난 문자열이 끼어들기 때문에

그때 그때 조건에 맞는 최상의 결과를 조합해 내야 한다.

1. element 조건에 만족하는지 비교하며 데이터를 추가/삭제해볼 stack 이 필요하다.
2. element 를 추가할 수 있는지 비교하기 위한 Remain/Skip 이 필요하다.
 - remain 은 Reverse(A), Skip은 Shuffle(A) 에 각각 매칭된다고 판단.
3. 문자열을 뒤에서부터 체크하며 다음 조건으로 비교해본다.
 - 이미 선택조건에 의해 Remain 에 해당하는 문자열이 존재하는가?
 - Remain 에 해당하는 문자열이 현재 선택된 조건의 문자열보다 큰가?
 - Remain 에 더 추가할 수 있는가?
 - 위 세개를 다 만족하며 Skip 할 수 있는가?
4. 3번에 해당하면 stack 에서 pop 해 skip 한다.
 - 결국 그때 그때 가장 최적의 조건에 해당하는 문자열을 찾아 stack 에 넣어둔다. 

```cpp
string reverseShuffleMerge(string s) {
    vector<int> nVecRemains( 26, 0 );
    vector<int> nVecSkip( 26, 0 );

    // 요소를 찾는다.
    for( const auto& c : s ) {
        ++nVecRemains[c-'a'];
    }
    
    // 모든 요소는 1/2 개 만큼 존재해야 A 를 만족하므로 개수를 1/2로 모두 나눈다.
    for( auto& i : nVecRemains ) {
        i = i / 2;
    }
    
    // 문자열 Remain 과 Skip 은 같은 Char를 보유해야 한다.
    nVecSkip = nVecRemains;
    
    // 문자를 넣고 뺄 스택 하나를 준비한다.
    stack<char> stChar;
    
    for( int nIdx = s.size() - 1; nIdx >= 0; --nIdx ) {
        
        // stack 이 비어있지 않다면 아래 조건을 만족할 때 까지 현재 선택한 문자와 비교해
        // skip 할지 아닐지 결정한다.
        while( false == stChar.empty() &&  // 스택에 이미 선택된 문자가 존재하고
            stChar.top() > s[nIdx] &&   // 현재 선택한 문자가 기존에 선택한 문자보다 앞서며
            nVecRemains[s[nIdx]-'a'] > 0 && // 아직 remain 할 문자열이 남아있고
            nVecSkip[stChar.top() - 'a'] > 0 // 기존에 선택한 문자를 아직 스킵할 수 있다면
        ) {
            // stack 에 기록했던 문자는 skip 조건에 해당한다.
            ++nVecRemains[stChar.top() - 'a'];
            --nVecSkip[stChar.top() - 'a'];
            stChar.pop();
        }
        
        // 위 while 문을 통과한 현재 문자열을 stack 에 추가할지 skip 할지 결정한다.
        if( nVecRemains[ s[nIdx] -'a' ] > 0 ) {
            stChar.push(s[nIdx]);
            --nVecRemains[s[nIdx] - 'a'];
        } else {
            --nVecSkip[s[nIdx] - 'a'];
        }
    }
    
    int nResultLength = s.size()/2;
    string strResult( nResultLength, '\0' );
    
    for( int nIdx = nResultLength - 1; nIdx >= 0; --nIdx ) {
        strResult[nIdx] = stChar.top();
        stChar.pop();
    }
    
    return strResult;
}
```