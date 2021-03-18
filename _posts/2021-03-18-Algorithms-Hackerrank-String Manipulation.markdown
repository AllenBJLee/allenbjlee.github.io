---
layout: post
title:  "Hackerrank - String Manipulation"
search: true
date: 2021-03-18
category: Algorithms
---

### 1. Strings: Making Anagrams
난이도 : <span style="color:green">easy</span>

링크 : [Strings: Making Anagrams](https://www.hackerrank.com/challenges/ctci-making-anagrams/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings){:target="_blank"}

> 문제
 
 - 문자열 a,b 에서 겹치는 char 외 다른 char 를 제거한다.
 - 제거된 char 의 개수를 구해보자.
ex)<br>
```
input
 cde
 abc
```
```
output
 4
```
cde 에서 de 를 제거하고, abc 에서 ab 를 제거하면 c 만 남는다. 총 제거한 문자는 4이다.

> 풀이

 - 문자열 a,b 에 들어있는 모든 문자 개수를 구하고
 - 각각의 문자 개수에서 차이나는 숫자만큼 더하면 해결된다.
 	
```cpp
int makeAnagram(string a, string b) {
    vector<int> vecA( 26, 0 ), vecB( 26, 0 );
    
    for( const auto& c : a ) {
        ++vecA[c-'a'];
    }
    
    for( const auto& c : b ) {
        ++vecB[c-'a'];
    }
    
    int nDeleted = 0;
    
    for( int i = 0; i < 26; ++i ) {
        if( vecA[i] != vecB[i] ) {
            nDeleted += abs( vecA[i] - vecB[i] );
        }
    }
    
    return nDeleted;
}
```

### 2. Alternating Characters
난이도 : <span style="color:green">easy</span>

링크 : [Alternating Characters](https://www.hackerrank.com/challenges/alternating-characters/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings){:target="_blank"}

> 문제

 - A, B 로 이루어진 문자열이 주어지며, A B 가 반복되게 만들기 위해서 중간에 중복된 문자를 삭제해야 한다.
 - 삭제한 문자 개수를 반환해보자.

> 풀이

 - 선택된 문자를 기준으로 다음 문자가 같으면 삭제, 아니면 문자를 교체해가며 비교한다.
 - 기본적인 압축 알고리즘
 
```cpp
int alternatingCharacters(string s) {
    char chCurrent = '\0';
    int nDeleted = 0;
    
    for( const auto& ch : s ) {
        if( '\0' == chCurrent || chCurrent != ch ) {
            chCurrent = ch;
        } else {
            ++nDeleted;
        }
    }
    
    return nDeleted;
}
```

### 3. Sherlock and the Valid String
난이도 : <span style="color:orange">Medium</span>

링크 : [Sherlock and the Valid String](https://www.hackerrank.com/challenges/sherlock-and-valid-string/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings){:target="_blank"}

> 문제

 - 주어진 문자열 S 에서 단 하나의 문자만 지웠을 때
 - 모든 문자의 개수가 동일해지는지 파악해보자

> 풀이

- 주어진 문자를 모두 카운팅하고
- 문자열 개수별로 정렬한 다음
  - 문자 개수의 종류가 2개를 초과하면 무조건 실패
  - 문자 개수의 종류가 1개면 성공
  - 문자 개수가 2개인데
    - 문자 개수가 1개인 종류가 딱 1케이스만 존재하면 성공
    - 문자 개수가 연달아 이어지는 케이스면서 더 높은 개수를 소유한 케이스가 1개일때 성공
- 나머지는 실패로 본다.
 
```cpp
string isValid(string s) {  
    vector<int> nVec(26, 0);
    
    int nMax = 0;
    
    for( const auto& ch : s ) {
        ++nVec[ch-'a'];
    }
    
    map< int, int > mapCount;
    map< int, int >::iterator it;
    
    for( const auto& i : nVec ) {
        if( 0 < i ) {
            it = mapCount.find( i );
            
            if( it != mapCount.end() ) {
                ++(it->second);
            } else {
                mapCount.insert( pair< int, int >(i, 1) );
            }
        }
    }
    
    // 모든 문자 개수가 2개를 초과하면 NO
    if( 2 < mapCount.size() ) {
        return "NO";
    } else if( 1 == mapCount.size() ) { // 1개의 종류만 있을 경우 YES
        return "YES";
    }
    
    map< int, int >::iterator itNext;
    
    it = mapCount.begin();
    
    itNext = it;
    ++itNext;
    
    // 1개의 출현 빈도를 가진 케이스가 1개면 얘만 지워주면 되므로 성공
    if( ( 1 == it->first && 1 == it->second ) ) {
        return "YES";
    }
    else if( 1 == itNext->first - it->first &&
            ( 1 == itNext->second ) ) {
        // 출현 빈도가 낮은 케이스와 높은 케이스의 차이가 1이고
        // 출현 빈도가 높은 케이스가 딱 1건일 경우만 성공
        return "YES";
    }
    
    // 나머지는 다 실패
    return "NO";
}
```

### 4. Special String Again
난이도 : <span style="color:orange">Medium</span>

링크 : [Special String Again](https://www.hackerrank.com/challenges/special-palindrome-again/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings){:target="_blank"}

> 문제

 - 문자열이 주어지면 다음 조건을 만족하는 SubString 들을 찾아 개수를 반환해보자.
 - 조건
   - 반복된 문자일 경우 카운팅
     - ex) "aabcaa" 일 경우 0, 1, 4, 5 번지 문자를 이용해 {"a", "aa", "a", "aa" } 를 획득
   - 반복 문자열 중앙값만 다를 경우 카운팅
     - ex) "aabaaa" 일 경우 0, 1, 2, 3, 4, 5 를 이용해 {"aabaa", "aba"} 를 획득 가능
 - 위 두 조건을 모두 만족하는 SubString 개수를 수집한다.
 
> 풀이 1

 - 주어진 조건을 그대로 Implementation 하게 구현해 모든 결과를 통과한다.
 - 모든 테스트 케이스 (Hidden Case 포함)에 통과하지만 시간복잡도가 너무 클 것 같아 다른 사람들이 어떻게 풀었는지 Discussion 을 참고해 다시 풀어보도록 결정.
 
```cpp
enum CompResult {
    CompSucceed = 0,
    CompFailedButNeedMore,
    CompDoNotContinue
};

CompResult comp( const string& s ) {
    int nLen = s.size();
    
    if( 1 == nLen ) {
        return CompSucceed;
    }
    
    map< char, int > mapComp;
    map< char, int >::iterator it;
    
    for( int nIndex = 0; nIndex < s.size(); ++nIndex ) {
        it = mapComp.find( s[nIndex] );
        
        if( it == mapComp.end() ) {
            // first find
            if( 2 == mapComp.size() ) {
                return CompDoNotContinue;
            }
            
            mapComp.insert( pair< char, int >( s[nIndex], 1 ) );
        }
        else {
            ++(it->second);
        }
    }
    
    // 조사 결과 1개의 문자로만 이루어지면 성공
    if( 1 == mapComp.size() ) {
        return CompSucceed;
    }
    
    it = ++(mapComp.begin());    
    if( 1 < mapComp.begin()->second && 1 < it->second ) {
        // 두개 문자 모두 2개 이상일 경우 더이상 비교하지 않는다.
        return CompDoNotContinue;
    }
    
    if( 0 != nLen % 2 ) {
        // 문자열이 홀수개인 경우
        
        if( 1 == mapComp.find( s[0] )->second ) {
            // 시작 문자가 1개고 반복된 문자가 2개면 더이상 비교할 필요가 없다.
            return CompDoNotContinue;
        }
        
        // 미들에 다른 문자열이 존재하면 우선 성공        
        if( s[0] != s[nLen/2] ) {
            return CompSucceed;
        }
    }
    
    // 모든 조건에 만족하지 않으면 계속 비교해본다.
    return CompFailedButNeedMore;
}


// Complete the substrCount function below.
long substrCount(int n, string s) {
    int nSize = s.size();
    
    long lResult = 0;
    
    CompResult enCompResult = CompFailedButNeedMore;
    
    string strComp;
    
    strComp.reserve( s.size() + 1 );
    
    for( int nIndex = 0; nIndex < nSize; ++nIndex ) {
        for( int j = 0; j < nSize - nIndex; ++j ) {
            strComp = s.substr( nIndex, j + 1 );
            
            enCompResult = comp( strComp );
            
            if( CompSucceed == enCompResult ) {
                ++lResult;
            } else if( CompDoNotContinue == enCompResult ) {
                break;
            }
        }
    }
    
    return lResult;
}
```

> 풀이 2

 - Discussion 을 좀 읽어보니 조합론을 이용해 중복된 문자의 개수를 파악하고 n * (n+1) / 2 를 공식으로 만들어낸 사람이 있었다.
   - 사실 이거 보고 좀 충격먹었는데 문제를 보자마자 저런 알고리즘을 떠올렸다는게 신기하다.
 - 이 알고리즘을 이용해 풀어보면 다음과 같다.
 
```cpp
struct CHAR_DATA {
    char ch;
    int nRepeated;
};

// Complete the substrCount function below.
long substrCount(int n, string s) {
    // 알고리즘
    // 1. 연속된 문자열이 반복될 경우 이를 special substring 으로 계산하는 공식
    // 원문
    // For a string with n characters, we can make a total of n*(n+1)/2 substrings.
    // Note substrings keep the same order and don't skip characters.
    // For instance, aaaa will make a, aa, aa, aa, aaa, aaa and aaaa
    // This is because, for a string of size n we can make n - 1 substrings of size 2 and n - 2 substrings of size 3 and so on.
    // This can be generalized as n-(k-1) where k is the length of the substring.
    // So total number of substrings (of all lenghts) is then n-1 + n-2 + ... + n - (k-1) + ... + n - (n-1).
    // This when written in reverse is same as 1 + 2 + ... + n-2 + n-1 + n.
    // The sum of an arithmetic sequence is = n*(first+last)/2. Therefore, the sum of above sequence is n*(1+n)/2

    // 번역기
    // n 개의 문자가있는 문자열의 경우 총 n * (n + 1) / 2 개의 하위 문자열을 만들 수 있습니다.
    // 하위 문자열은 동일한 순서를 유지하고 문자를 건너 뛰지 않습니다.
    // 예를 들어, aaaa는 a, aa, aa, aa, aaa, aaa 및 aaaa를 만듭니다.
    // 이것은 크기 n의 문자열에 대해 크기 2의 n-1 개의 하위 문자열과 크기 3의 n-2 개의 하위 문자열을 만들 수 있기 때문입니다.
    // 이것은 n- (k-1)로 일반화 할 수 있습니다.
    // 여기서 k는 부분 문자열의 길이입니다. 따라서 (모든 길이의) 총 부분 문자열 수는 n-1 + n-2 + ... + n-(k-1) + ... + n-(n-1)입니다.
    // 반대로 쓸 때 이것은 1 + 2 + ... + n-2 + n-1 + n과 같습니다.
    // 산술 시퀀스의 합은 = n * (첫 번째 + 마지막) / 2입니다. 따라서 위 시퀀스의 합은 n * (1 + n) / 2

    // 2. 문자열 중앙값만 다른 연속되는 문자열을 구하는 방법
    // 문자열을 순서대로 반복되는 회수를 기록해 나열한 후 Current 와 Current + 2가 같은 문자고 Current + 1 이 다른 문자이며,
    // Current + 1 이 단 1개의 문자만 소유할 경우를 체크한다.
    // 그리고 Current 와 Current + 2 중 문자열 개수가 작은 쪽의 개수만 카운팅하면 된다.
    // ex) "aabaaa" 가 있을 때
    //  [ {"a", 2}, {"b", 1}, {"a", 3} ] 으로 정리된다.
    //  이때 찾을 수 있는 결과는 "aabaa", "aba" 가 되므로 Current( {"a", 2} )의 2개를 추가해주면 된다.
    
    vector<CHAR_DATA> vecData;

    char chCurrent = s[0];
    int nSize = s.size();

    vecData.push_back( { chCurrent, 1 } );

    // 문자열을 순회하면서 반복되는 문자열을 순서대로 정리한다.
    for( int nIndex = 1; nIndex < nSize; ++nIndex ) {
        if( chCurrent == s[nIndex] ) {
            ++(vecData[vecData.size()-1].nRepeated);
        } else {
            chCurrent = s[nIndex];
            vecData.push_back( { chCurrent, 1 } );
        }
    }

    long lResult = 0;
    
    nSize = vecData.size();

    for( int nIndex = 0; nIndex < nSize; ++nIndex ) {
        // 알고리즘 1에 해당하는 값을 더해준다
        lResult += ( vecData[nIndex].nRepeated * ( 1 + vecData[nIndex].nRepeated ) / 2 );

        if( nIndex + 2 < nSize ) {
            // 알고리즘 2에 해당하는 값을 추가로 찾아 더해준다
            if( vecData[nIndex].ch == vecData[nIndex+2].ch &&       // Current 와 Current + 2 의 문자가 같고
                1 == vecData[nIndex+1].nRepeated ) {    // 중간에 낀 문자의 개수가 1개일 때
                lResult += ( vecData[nIndex].nRepeated < vecData[nIndex+2].nRepeated ? vecData[nIndex].nRepeated : vecData[nIndex+2].nRepeated );
            }
        }
    }

    return lResult;    
}
```

### 5. Common Child
난이도 : <span style="color:orange">Medium</span>

링크 : [Common Child](https://www.hackerrank.com/challenges/common-child/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings){:target="_blank"}

> 문제

전형적인 LCS(Long Common SubSequence) 알고리즘 문제이다.

 - 문자열 A, B 가 주어지면 A, B 에서 순서대로 일치하는 가장 긴 문자열의 개수를 파악하는 문제

ex)
```
input
 A="abecd"
 B="adbcf"
```
```
output
 2
```
A의 bc 와 B의 bc 가 순서대로 일치하는 가장 긴 문자열이다. SubSequence 이기 때문에 문자열이 인접해있지 않아도 된다.
 
> 풀이

 - LCS 알고리즘을 이용해 풀어낸다.

```cpp
int commonChild(string s1, string s2) {
    int nLength = s1.length();
    
    // lcs 를 구할 수 있는 2차원 배열 준비
    // lcs 알고리즘은 같은 최종 0 를 만났을 때 탐색이 종료되므로
    // 0 행 0열을 0으로 채워야 한다. (종료 조건을 맞추기 위해)
    vector< vector< int > > vecData( nLength + 1, vector<int>( nLength + 1, 0 ) );
    
    // s1 을 기준으로 s2 에 일치하는 문자열이 있는지 체크한다.
    for( int i = 1; i < nLength + 1; ++i ) {
        for( int j = 1; j < nLength + 1; ++j ) {
            if( s1[i-1] == s2[j-1] ) {
                // 일치하면 배열의 i-1, j-1 값 + 1을 해준다.
                vecData[i][j] = vecData[i-1][j-1] + 1;
            } else {
                // 일치하지 않으면 [i][j-1] 과 [i-1][j] 중 큰 값을 배치한다.
                vecData[i][j] = vecData[i][j-1] > vecData[i-1][j] ? vecData[i][j-1] : vecData[i-1][j];
            }
        }
    }
    
    stack< char > sResult;
    
    // 배열의 마지막 행,열 부터 찾아가며 문자열을 찾는다.
    int nRow = vecData.size() - 1;
    int nCol = nRow;
    
    int nFoundResult = vecData[nRow][nCol];
    
    while( nCol > 0 && nRow > 0 ) {
        // 현재 선택된 요소의 값과 일치하는지 비교한다.
        // 비교 대상은 선택된 요소의 윗 Row(nRow-1) 와 좌측 Column(nCol-1) 이다
        if( nFoundResult == vecData[nRow-1][nCol] ) {
            // 윗 row 가 현재값과 일치하면 윗 row 로 이동
            --nRow;
        } else if( nFoundResult == vecData[nRow][nCol-1]) {
            // 좌측 column이 현재값과 일치하면 좌측 column으로 이동
            --nCol;
        } else {
            // 윗 row 와 좌측 column이 현재값과 일치하지 않는다면
            // 현재 문자열을 result에 insert 한다.
            sResult.push( s2[nCol-1] );
            
            // 일치하는 값을 찾지 못했으므로 row 와 col 을 -1 씩해 좌상으로 이동한다.
            nFoundResult = vecData[--nRow][--nCol];
        }
    }
    
    // 이 스택을 pop 해서 문자열에 채우면 lcs 문자열이 된다.   
    return sResult.size();
}
```
