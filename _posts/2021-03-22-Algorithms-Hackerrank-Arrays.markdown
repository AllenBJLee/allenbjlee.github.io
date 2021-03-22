---
layout: post
title:  "Hackerrank - Arrays"
search: true
date: 2021-03-22
category: Algorithms
---

### 1. 2D Array - DS
난이도 : <span style="color:green">Easy</span>

링크 : [2D Array - DS](https://www.hackerrank.com/challenges/2d-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays){:target="_blank"}

> 문제
 
 - 6x6 배열이 주어지고
 - 배열 내 모든 요소를 이용해 다음 조건에 만족하는 합 중 가장 큰 값을 찾는 문제
   - 배열 내 요소들을 모래시계 모양으로 더해 결과를 낸다.

> 풀이

 - 6x6 배열 내 모든 값을 조건에 맞게 더해서 가장 큰 값을 찾는다. (Brute Force 탐색)
 	
```cpp
#include <bits/stdc++.h>

using namespace std;

// Complete the hourglassSum function below.
int hourglassSum(vector<vector<int>> arr) {
    int nResult = -9 * 7;
    
    int nSum = 0;
    for( int nIndex = 0; nIndex < 4; ++nIndex ) {
        
        for( int j = 0; j < 4; ++j ) {
            nSum = arr[nIndex+1][j+1];

            for( int k = 0; k < 3; ++k ) {
                nSum += arr[nIndex][j+k];
                nSum += arr[nIndex+2][j+k];
            }
            
            if( nResult < nSum ) {
                nResult = nSum;
            }
        }
    }

    return nResult;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    vector<vector<int>> arr(6);
    for (int i = 0; i < 6; i++) {
        arr[i].resize(6);

        for (int j = 0; j < 6; j++) {
            cin >> arr[i][j];
        }

        cin.ignore(numeric_limits<streamsize>::max(), '\n');
    }

    int result = hourglassSum(arr);

    fout << result << "\n";

    fout.close();

    return 0;
}
```

### 2. Arrays: Left Rotation
난이도 : <span style="color:green">Easy</span>

링크 : [Arrays: Left Rotation](https://www.hackerrank.com/challenges/ctci-array-left-rotation/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays){:target="_blank"}

> 문제

 - 배열 a와 로테이션 회수 d가 주어진다.
 - d 회수 만큼 맨 앞 요소를 꺼내 맨 뒤로 보낸 후 배열을 반환한다.

> 풀이

 - 설명대로 코드를 구현한다.
 
```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

// Complete the rotLeft function below.
vector<int> rotLeft(vector<int> a, int d) {
    for( int i = 0; i < d; ++i ) {
        a.push_back(a[0]);
        a.erase( a.begin() );
    }
    
    return a;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    string nd_temp;
    getline(cin, nd_temp);

    vector<string> nd = split_string(nd_temp);

    int n = stoi(nd[0]);

    int d = stoi(nd[1]);

    string a_temp_temp;
    getline(cin, a_temp_temp);

    vector<string> a_temp = split_string(a_temp_temp);

    vector<int> a(n);

    for (int i = 0; i < n; i++) {
        int a_item = stoi(a_temp[i]);

        a[i] = a_item;
    }

    vector<int> result = rotLeft(a, d);

    for (int i = 0; i < result.size(); i++) {
        fout << result[i];

        if (i != result.size() - 1) {
            fout << " ";
        }
    }

    fout << "\n";

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

### 3. New Year Chaos
난이도 : <span style="color:orange">Medium</span>

링크 : [New Year Chaos](https://www.hackerrank.com/challenges/new-year-chaos/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays){:target="_blank"}

> 문제

 - 배열 q 가 주어진다.
 - 배열 내 숫자들은 티켓 순서이다.
 - 순서대로 롤러코스터를 탑승할 수 있으나 'bribe'(뇌물) 을 주고 내 순서를 앞으로 땡길 수 있다.
   - 단, 땡길 수 있는 최대 회수는 2회 한정이다.
 - 2회를 초과해 뇌물을 제공하면 'Too chaotic' 을 출력한다.
 - 순서가 변경되도 최초로 받은 티켓 번호는 유지된다.

> 풀이

- 우선 루프를 돌며 index 값이 최대 회수2 회를 초과하는 값일 경우 Too chaotic 을 출력하면서 종료한다.
- Too chaotic 에 해당하지 않을 경우 내 순서를 기준으로 나보다 뒤에 있는 사람들 중 티켓 번호가 낮은 사람을 체크하면 된다. (고 생각했다)
  - 이렇게 풀었더니 Time Limited 가 발생했다.
- disscusion 을 좀 살펴보니 앞에서 부터 찾지 않고 뒤에서 부터 찾는 방식이 있었다.
  - 결국 배열의 요소는 앞으로는 갈 수 있으나(뇌물을 써서) 뒤로는 밀려가는 방법 외엔 없으므로 q[index] - 2 부터 index 까지만 검사하면 내 앞 사람들을 모두 검사할 수 있게 된다. (나는 무슨 수를 써도 2칸 앞 까지만 접근 가능하므로)
- 이 아이디어를 이용해 뒤에서부터 찾으면 Time Limited 도 해결된다.
 
```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

// Complete the minimumBribes function below.
void minimumBribes(vector<int> q) {
    int nSize = q.size();
    int nResult = 0;
    int nSum = 0;
    
    for( int i = nSize - 1; i >= 0; --i ) {
        if( q[i] - i > 3 ) {
            // Too chaotic
            printf( "Too chaotic\n" );
            return ;
        }
        
        for( int j = max( 0, q[i] - 2 ); j < i; ++j ) {
            if( q[j] > q[i] ) {
                ++nResult;
            }
        }
    }

    printf( "%d\n", nResult );
}

int main()
{
    int t;
    cin >> t;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    for (int t_itr = 0; t_itr < t; t_itr++) {
        int n;
        cin >> n;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        string q_temp_temp;
        getline(cin, q_temp_temp);

        vector<string> q_temp = split_string(q_temp_temp);

        vector<int> q(n);

        for (int i = 0; i < n; i++) {
            int q_item = stoi(q_temp[i]);

            q[i] = q_item;
        }

        minimumBribes(q);
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

### 4. Minimum Swaps 2
난이도 : <span style="color:orange">Medium</span>

링크 : [Minimum Swaps 2](https://www.hackerrank.com/challenges/minimum-swaps-2/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays){:target="_blank"}

> 문제

 - 배열이 주어지고 배열을 정렬하기 위해 가장 최소 교환 회수를 구해보자.
 - 조건
   - 배열은 1부터 증가하는 자연수이며, 중간에 빠진 숫자나 중복된 숫자는 없다.

> 풀이

 - 처음엔 가장 빠른 교환 알고리즘을 구현해야 하나 생각했는데 '최소 교환 회수'를 구하는 문제이다.
 - Selection Sort 처럼 구하면 제시된 조건 기준 가장 작은 교환 회수가 된다.
 
```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

// Complete the minimumSwaps function below.
int minimumSwaps(vector<int> arr) {
    int nSize = arr.size();
    
    int nCnt = 0;
    int nTemp = 0;
    
    for( int i = 0; i < nSize; ++i ) {
        if( i == arr[i] - 1 ) {
            continue;
        }
        
        nTemp = arr[arr[i]-1];
        arr[arr[i]-1] = arr[i];
        arr[i] = nTemp;
        
        ++nCnt;
        --i;
    }
    
    return nCnt;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    int n;
    cin >> n;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    string arr_temp_temp;
    getline(cin, arr_temp_temp);

    vector<string> arr_temp = split_string(arr_temp_temp);

    vector<int> arr(n);

    for (int i = 0; i < n; i++) {
        int arr_item = stoi(arr_temp[i]);

        arr[i] = arr_item;
    }

    int res = minimumSwaps(arr);

    fout << res << "\n";

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
