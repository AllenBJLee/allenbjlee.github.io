---
layout: post
title:  "Hackerrank - Search"
search: true
date: 2021-03-21
category: Algorithms
---

### 1. Hash Tables: Ice Cream Parlor
난이도 : <span style="color:orange">Medium</span>

링크 : [Strings: Hash Tables: Ice Cream Parlor](https://www.hackerrank.com/challenges/ctci-ice-cream-parlor/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search){:target="_blank"}

> 문제
 
 - 두명의 친구가 아이스크림을 사고 싶은데 보유한 돈을 다 써서 살 수 있는 최적의 가격을 구해보자.
 - 단, 조건에 만족하는 케이스는 유니크함을 보장한다.

> 풀이

 - 제목에 Hash Tables 라고 써있드시 Hash Table 을 만들어 해결했다.
 - Price 를 기준으로 Hash 를 만들고, 중복된 가격이 있으면 vector 로 연결해 사용했다. (list 를 써도 될 것 같다.)
 - 그 후 보유한 돈을 이용해 최적값을 찾기 위한 검색을 시작한다.
   - hash tables 을 순회하며 계산한다.
   - 주어진 돈(m) - 현재 가격(c_key) = 다른 키(other_key) 가 된다.
   - other_key == c_key 이고 c_key 의 item 이 2개면 성공
   - other_key 가 존재하면 성공
 	
```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

// Complete the whatFlavors function below.
void whatFlavors(vector<int> cost, int money) {
    
    // first - price, second - index array
    map< int, vector<int> > mapData;
    
    map< int, vector<int> >::iterator it;
    
    int nSize = cost.size();
    
    for( int nIndex = 0; nIndex < nSize; ++nIndex ) {
        it = mapData.find( cost[nIndex] );
        
        if( it == mapData.end() ) {
            // 새로운 아이템
            mapData.insert( pair< int, vector<int> >( cost[nIndex], vector<int>( 1, nIndex ) ) );
        } else {
            // 이미 존재하는 아이템
            it->second.push_back( nIndex );
        }
    }
    
    int nRemainCost = 0;
    
    map< int, vector<int> >::iterator itFind;
    
    vector<int> vecResult( 2, 0 );
    
    for( it = mapData.begin(); it != mapData.end(); ++it ) {
        nRemainCost = money - it->first;
        
        itFind = mapData.find( nRemainCost );
        
        if( itFind == mapData.end() ) {
            // 0 로 일치하는 아이템이 없다.
        } else if( itFind == it && 2 == itFind->second.size() ) {
            // 일치하는 값을 찾았으나 중복 아이템이다
            // second 개수가 두개라면 반환 가능
            vecResult[0] = itFind->second[0]+1;
            vecResult[1] = itFind->second[1]+1;
            break;
        } else {
            vecResult[0] = it->second[0]+1;
            vecResult[1] = itFind->second[0]+1;
            break;
        }
    }
    
    sort( vecResult.begin(), vecResult.end() );
    
    printf( "%d %d\n", vecResult[0], vecResult[1] );
}

int main()
{
    int t;
    cin >> t;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    for (int t_itr = 0; t_itr < t; t_itr++) {
        int money;
        cin >> money;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        int n;
        cin >> n;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        string cost_temp_temp;
        getline(cin, cost_temp_temp);

        vector<string> cost_temp = split_string(cost_temp_temp);

        vector<int> cost(n);

        for (int i = 0; i < n; i++) {
            int cost_item = stoi(cost_temp[i]);

            cost[i] = cost_item;
        }

        whatFlavors(cost, money);
    }

    return 0;
}

vector<string> split_string(string input_string) {
    string::iterator new_end = unique(input_string.begin(), input_string.end(), [] (const char &x, const char &y) {
        return x == y and x == ' ';
    });

    input_string.erase(new_end, input_string.end());

    while (input_string[input_string.length() - 1] == ' ') {
        input_string.pop_back();
    }

    vector<string> splits;
    char delimiter = ' ';

    size_t i = 0;
    size_t pos = input_string.find(delimiter);

    while (pos != string::npos) {
        splits.push_back(input_string.substr(i, pos - i));

        i = pos + 1;
        pos = input_string.find(delimiter, i);
    }

    splits.push_back(input_string.substr(i, min(pos, input_string.length()) - i + 1));

    return splits;
}

```

### 2. Swap Nodes [Algo]
난이도 : <span style="color:orange">Medium</span>

링크 : [Swap Nodes [Algo]](https://www.hackerrank.com/challenges/swap-nodes-algo/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search){:target="_blank"}

> 문제

 - 트리가 주어지고, K 값이 주어진다.
 - K 값과 Depth 가 일치하거나 multiple of K 를 만족하면 같은 depth 에 있는 노드들끼리 swap 한다.

> 풀이

 - 처음엔 트리를 vector 로 구축해서 풀어보려 했는데 swap 때문에 vector 로 구축하기엔 무리가 있다고 판단했다.
   - child 들 까지 따라가면서 swap 을 해주기엔 vector 로 구현된 tree 는 무리가 있다.
 - tree 를 구축하고, 조건(K 및 multiple of K)에 해당하는 node 들을 swap 해준다.
 - 사실 문제를 이해하는데 너무 많은 시간(약 30분 이상)이 들어갔다. 정작 이해하고 해결하는건 오래걸리지 않았다. (K 가 multiple of K 도 포함한다는 걸 찾는데 시간이 오래걸렸다. 영알못 ㅠㅠ)
 
```cpp
#include <bits/stdc++.h>

using namespace std;

struct Node {
    int nValue { 0 };
    Node* pLeft = nullptr;
    Node* pRight = nullptr;
};

void searchTree( Node* pNode, vector<int>& vResult ) {
    if( nullptr == pNode ) {
        return ;
    }
    
    searchTree( pNode->pLeft, vResult );
    vResult.push_back( pNode->nValue );
    searchTree( pNode->pRight, vResult );
}

void swapNode( Node* pNode, int nDepth, int k ) {
    if( nullptr == pNode ) {
        return ;
    }
    
    if( nDepth >= k && 0 == nDepth % k ) {
        Node* pTemp = pNode->pLeft;
        
        pNode->pLeft = pNode->pRight;
        pNode->pRight = pTemp;
    }
    
    swapNode( pNode->pLeft, nDepth + 1, k );
    swapNode( pNode->pRight, nDepth + 1, k );
}

void eraseTree( Node* pNode ) {
    if( nullptr == pNode ) {
        return ;
    }
    
    eraseTree( pNode->pLeft );
    eraseTree( pNode->pRight );
    
    delete pNode;
    pNode = nullptr;
}

/*
 * Complete the swapNodes function below.
 */
vector<vector<int>> swapNodes(vector<vector<int>> indexes, vector<int> queries) {
    /*
     * Write your code here.
     */
    
    Node* pRoot = new Node;
    Node* pCurrent = pRoot;
    
    pRoot->nValue = 1;
    
    queue< Node* > qTree;
    
    qTree.push( pRoot );
    
    // tree 를 구성한다.    
    for( auto const& items : indexes ) {
        pCurrent = qTree.front();
        
        qTree.pop();
        
        if( -1 != items[0] ) {
            pCurrent->pLeft = new Node;
            pCurrent->pLeft->nValue = items[0];
            
            qTree.push( pCurrent->pLeft );
        }
        
        if( -1 != items[1] ) {
            pCurrent->pRight = new Node;
            pCurrent->pRight->nValue = items[1];
            
            qTree.push( pCurrent->pRight );
        }
    }
    
    vector< vector<int> > vResult;
    
    // 쿼리에서 k 를 꺼내 swap 하고 결과를 저장한다.
    for( const auto& k : queries ) {
        swapNode(pRoot, 1, k);
        
        vector<int> vSubResult;
        searchTree(pRoot, vSubResult);
        
        vResult.push_back( vSubResult );
    }
    
    // erase all tree
    eraseTree( pRoot );
    
    return vResult;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    int n;
    cin >> n;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    vector<vector<int>> indexes(n);
    for (int indexes_row_itr = 0; indexes_row_itr < n; indexes_row_itr++) {
        indexes[indexes_row_itr].resize(2);

        for (int indexes_column_itr = 0; indexes_column_itr < 2; indexes_column_itr++) {
            cin >> indexes[indexes_row_itr][indexes_column_itr];
        }

        cin.ignore(numeric_limits<streamsize>::max(), '\n');
    }

    int queries_count;
    cin >> queries_count;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    vector<int> queries(queries_count);

    for (int queries_itr = 0; queries_itr < queries_count; queries_itr++) {
        int queries_item;
        cin >> queries_item;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        queries[queries_itr] = queries_item;
    }

    vector<vector<int>> result = swapNodes(indexes, queries);

    for (int result_row_itr = 0; result_row_itr < result.size(); result_row_itr++) {
        for (int result_column_itr = 0; result_column_itr < result[result_row_itr].size(); result_column_itr++) {
            fout << result[result_row_itr][result_column_itr];

            if (result_column_itr != result[result_row_itr].size() - 1) {
                fout << " ";
            }
        }

        if (result_row_itr != result.size() - 1) {
            fout << "\n";
        }
    }

    fout << "\n";

    fout.close();

    return 0;
}
```

### 3. Pairs
난이도 : <span style="color:orange">Medium</span>

링크 : [Pairs](https://www.hackerrank.com/challenges/pairs/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search){:target="_blank"}

> 문제

 - K 와 Array 가 주어지고 Array 의 요소들을 Pair 로 묶어 두 수의 차이가 K 가 되는 케이스를 조사해 보자.

> 풀이

- 조건에 배열 안의 모든 값은 유니크함을 보장한다고 써있다.
- 배열 요소의 최대값이 2^31 - 1 (signed int 최대값) 이므로 각 요소들을 카운팅할 수 있는 bool vector 를 만들어 존재 유무를 체크한다.
  - 주어진 아이템을 미리 인덱싱 해준다.
- bool vector[k + arr::itrator] 값이 true 라면 pair 에 만족한 조건이 된다.
 
```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

// Complete the pairs function below.
int pairs(int k, vector<int> arr) {
    // 조건
    //  - arr 안에 있어있는 값을 t 로 정의
    //    - t 는 모두 다른 값이다.
    //    - arr 안에 들어있는 타겟의 값은 0 < t < 2^31 - 1 이다. (signed int 범위 안에 있다)
    
    vector< bool > vecSorted( pow( 2, 31 ), false );
    
    // arr 를 돌면서 있는 값을 true 로 체크해준다.
    for( const auto& t : arr ) {
        vecSorted[t] = true;
    }
    
    int nResult = 0;
    
    // k 값과 t 값을 더한 값이 배열에 존재한다면 성공
    for( const auto& t : arr ) {
        if( true == vecSorted[k + t] ) {
            ++nResult;
        }
    }
    
    return nResult;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    string nk_temp;
    getline(cin, nk_temp);

    vector<string> nk = split_string(nk_temp);

    int n = stoi(nk[0]);

    int k = stoi(nk[1]);

    string arr_temp_temp;
    getline(cin, arr_temp_temp);

    vector<string> arr_temp = split_string(arr_temp_temp);

    vector<int> arr(n);

    for (int i = 0; i < n; i++) {
        int arr_item = stoi(arr_temp[i]);

        arr[i] = arr_item;
    }

    int result = pairs(k, arr);

    fout << result << "\n";

    fout.close();

    return 0;
}

vector<string> split_string(string input_string) {
    string::iterator new_end = unique(input_string.begin(), input_string.end(), [] (const char &x, const char &y) {
        return x == y and x == ' ';
    });

    input_string.erase(new_end, input_string.end());

    while (input_string[input_string.length() - 1] == ' ') {
        input_string.pop_back();
    }

    vector<string> splits;
    char delimiter = ' ';

    size_t i = 0;
    size_t pos = input_string.find(delimiter);

    while (pos != string::npos) {
        splits.push_back(input_string.substr(i, pos - i));

        i = pos + 1;
        pos = input_string.find(delimiter, i);
    }

    splits.push_back(input_string.substr(i, min(pos, input_string.length()) - i + 1));

    return splits;
}
```

### 4. Triple sum
난이도 : <span style="color:orange">Medium</span>

링크 : [Triple sum](https://www.hackerrank.com/challenges/triple-sum/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search){:target="_blank"}

> 문제

 - 배열 a,b,c 가 주어진다.
 - 각각의 배열에서 p in a, q in b, r in b 라고 정의한다.
 - p <= q and r <= q 를 만족하는 합집합의 개수를 찾아보자.

> 풀이

 - 각 배열은 정렬, 중복제거가 필요하다.
   - 중복된 값을 포함할 땐 중복값을 케이스로 인정하지 않는다.
 - 정리가 끝나면 조건에 맞는 합집합의 개수를 파악한다.
   - 이 때 정렬을 했기 때문에 이전에 조건이 만족한다면, q 값이 증가해도 만족하는 케이스가 되기 때문에 index 를 계속 0부터 돌릴 필요가 없다.
 
```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

// Complete the triplets function below.
long triplets(vector<int> a, vector<int> b, vector<int> c) {
    // a b c 는 정렬되지 않았고 중복값도 포함하고 있다.
    // test case 들 중간 중간에 중복값과 정렬때문에 실패하는 경우 발생
    sort( a.begin(), a.end() );
    sort( b.begin(), b.end() );
    sort( c.begin(), c.end() );
    
    a.erase( unique( a.begin(), a.end() ), a.end() );
    b.erase( unique( b.begin(), b.end() ), b.end() );
    c.erase( unique( c.begin(), c.end() ), c.end() );

    long lResult = 0;   
    
    int nASize = a.size();
    int nCSize = c.size();
    
    int nIdxA = 0;
    int nIdxC = 0;
    
    for( const auto& q : b ) {
        // 정렬과 중복제거를 진행한 상태이기 때문에
        // 한번 찾은 p < q 관계는 q가 증가했을 때 동일하게 포함관계를 가지게 된다.
        // p 를 다시 idx:0 부터 돌면서 찾지 않아도 된다.
        while( nIdxA < nASize && a[nIdxA] <= q ) {
            ++nIdxA;
        }
        while( nIdxC < nCSize && c[nIdxC] <= q ) {
            ++nIdxC;
        }
        
        lResult += ( (long)nIdxA * (long)nIdxC );
    }
    
    return lResult;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    string lenaLenbLenc_temp;
    getline(cin, lenaLenbLenc_temp);

    vector<string> lenaLenbLenc = split_string(lenaLenbLenc_temp);

    int lena = stoi(lenaLenbLenc[0]);

    int lenb = stoi(lenaLenbLenc[1]);

    int lenc = stoi(lenaLenbLenc[2]);

    string arra_temp_temp;
    getline(cin, arra_temp_temp);

    vector<string> arra_temp = split_string(arra_temp_temp);

    vector<int> arra(lena);

    for (int i = 0; i < lena; i++) {
        int arra_item = stoi(arra_temp[i]);

        arra[i] = arra_item;
    }

    string arrb_temp_temp;
    getline(cin, arrb_temp_temp);

    vector<string> arrb_temp = split_string(arrb_temp_temp);

    vector<int> arrb(lenb);

    for (int i = 0; i < lenb; i++) {
        int arrb_item = stoi(arrb_temp[i]);

        arrb[i] = arrb_item;
    }

    string arrc_temp_temp;
    getline(cin, arrc_temp_temp);

    vector<string> arrc_temp = split_string(arrc_temp_temp);

    vector<int> arrc(lenc);

    for (int i = 0; i < lenc; i++) {
        int arrc_item = stoi(arrc_temp[i]);

        arrc[i] = arrc_item;
    }

    long ans = triplets(arra, arrb, arrc);

    fout << ans << "\n";

    fout.close();

    return 0;
}

vector<string> split_string(string input_string) {
    string::iterator new_end = unique(input_string.begin(), input_string.end(), [] (const char &x, const char &y) {
        return x == y and x == ' ';
    });

    input_string.erase(new_end, input_string.end());

    while (input_string[input_string.length() - 1] == ' ') {
        input_string.pop_back();
    }

    vector<string> splits;
    char delimiter = ' ';

    size_t i = 0;
    size_t pos = input_string.find(delimiter);

    while (pos != string::npos) {
        splits.push_back(input_string.substr(i, pos - i));

        i = pos + 1;
        pos = input_string.find(delimiter, i);
    }

    splits.push_back(input_string.substr(i, min(pos, input_string.length()) - i + 1));

    return splits;
}
```

### 5. Minimum Time Required
난이도 : <span style="color:orange">Medium</span>

링크 : [Minimum Time Required](https://www.hackerrank.com/challenges/minimum-time-required/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search){:target="_blank"}

> 문제

 - 생산 머신의 배열(machines)과 목표 생산 개수(goal) 이 주어진다.
 - machines은 제품을 1개 생상할 때 소요되는 day 가 요소로 들어있다.
 - 목표 생산을 맞출 수 있는 최소 시간을 계산해보자.
 
> 풀이

 - 목표 생산 개수를 파악하는 search 알고리즘을 구현하면 된다.
 - Brute Force 로 풀 수도 있고, Binary Search 로 풀 수도 있고 풀이 방법은 다양하다.
 - 조건이 최적의 시간을 찾는 것이기 때문에 딱히 고민없이 Binary Search 로 구현해보기로 한다.

```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

// Complete the minTime function below.
long minTime(vector<long> machines, long goal) {
    sort( machines.begin(), machines.end() );
    
    long lSum = 0;
    
    int nMachineSize = machines.size();

    // binary search 를 위해 시작값과 최대값을 설정한다.
    long lStartDay = 0;
    long lEndDay = goal * machines[nMachineSize-1];
    long lDay = 0;
    
    long lResult = 0;
    
    // 시작값이 최대값보다 커지면 탐색을 중지한다.
    while( lStartDay < lEndDay ) {
        lSum = 0;
        
        // 시작값과 최대값의 중앙값을 기준으로 검색한다.
        lDay = (lStartDay + lEndDay) / 2;
        
        for( long i = 0; i < nMachineSize; ++i ) {
            lSum += ( lDay / machines[i] );
            if( lSum > goal )
                break;
        }
        
        // 합이 goal 보다 작으면 시작값 위치를 중앙값 +1로 이동한다.
        if( lSum < goal ) {
            // match day 를 찾으면 lStartDay 와 lEndDay 가 같은값이 되므로
            // +1 해서 찾지 않으면 무한루프를 돌게 된다.
            lStartDay = lDay + 1;
        } else {
            lResult = lDay;
            lEndDay = lDay;
        }
    };
    
    return lResult;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    string nGoal_temp;
    getline(cin, nGoal_temp);

    vector<string> nGoal = split_string(nGoal_temp);

    int n = stoi(nGoal[0]);

    long goal = stol(nGoal[1]);

    string machines_temp_temp;
    getline(cin, machines_temp_temp);

    vector<string> machines_temp = split_string(machines_temp_temp);

    vector<long> machines(n);

    for (int i = 0; i < n; i++) {
        long machines_item = stol(machines_temp[i]);

        machines[i] = machines_item;
    }

    long ans = minTime(machines, goal);

    fout << ans << "\n";

    fout.close();

    return 0;
}

vector<string> split_string(string input_string) {
    string::iterator new_end = unique(input_string.begin(), input_string.end(), [] (const char &x, const char &y) {
        return x == y and x == ' ';
    });

    input_string.erase(new_end, input_string.end());

    while (input_string[input_string.length() - 1] == ' ') {
        input_string.pop_back();
    }

    vector<string> splits;
    char delimiter = ' ';

    size_t i = 0;
    size_t pos = input_string.find(delimiter);

    while (pos != string::npos) {
        splits.push_back(input_string.substr(i, pos - i));

        i = pos + 1;
        pos = input_string.find(delimiter, i);
    }

    splits.push_back(input_string.substr(i, min(pos, input_string.length()) - i + 1));

    return splits;
}
```
