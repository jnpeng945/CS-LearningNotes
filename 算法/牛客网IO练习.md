```c++
// case1:
#include <iostream>
using namespace std;

int main() {
    int a, b;
    while(cin >> a >> b) {
        cout << a + b << endl;
    }
    return 0;
}
```



```c++
// case2:
#include<iostream>
using namespace std;
int main() {
    int n, a, b;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> a >> b;
        cout << a + b << endl;
    }
    return 0;
}
```



```c++
//case3:
#include<iostream>
using namespace std;
int main() {
    int a, b;
    while (cin >> a >> b) {
        if (a != 0 && b != 0) {
            cout << a + b << endl;
        }
    }
}
```



```c++
// case4:
#include<iostream>
using namespace std;
int main() {
    int n, num;
    while (cin >> n && n != 0) {
        int sum = 0;
        for (int i = 0; i < n; ++i) {
            cin >> num;
            sum += num;
        }
        cout << sum << endl;
    }
}
```



```c++
//case5：
#include<iostream>
using namespace std;
int main() {
    int row;
    cin >> row;
    int n, num;
    while ( row-- && cin >> n) {
        int sum = 0;
        for (int j = 0; j < n; ++j) {
            cin >> num;
            sum += num;
        }
        cout << sum << endl;
    }
}
```



```c++
// case6:
#include<iostream>
using namespace std;
int main() {
    int n, num;
    while (cin >> n) {
        int sum = 0;
        for (int i = 0; i < n; ++i) {
            cin >> num;
            sum += num;
        }
        cout << sum << endl;
    }
}
```



```c++
// case7:
#include<bits/stdc++.h>
using namespace std;
int main() {
    int num = 0, sum = 0;
    while (cin >> num) {
        sum += num;
        if (cin.get() == '\n') {
            cout << sum << endl;
            sum = 0;
        }
    }
    return 0;
}
```



```c++
// case8:
#include<bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin >> n;
    string strs[n];
    for (int i = 0; i < n; ++i) {
        cin >> strs[i];
    }
    sort(strs, strs + n);
    for (auto item : strs) {
        cout << item << " ";
    }
    return 0;
}
```



```c++
//case9:
#include<iostream>
#include<vector>
#include<algorithm>
#include<string>
using namespace std;
int main() {
    vector<string> vec;
    string str;
    while(cin>>str) {
        vec.push_back(str);
        if(cin.get() == '\n') {
            sort(vec.begin(),vec.end());
            for(auto s:vec) {
                cout<<s<<" ";
            }
            cout<<endl;
            vec.clear();
        }
    }
}
```



```c++
#include "bits/stdc++.h"
using namespace std;
int main(){
    string sl;
    vector<string> vecs;
    while(getline(cin,sl)){
        vecs.clear();
        stringstream ss(sl);
        string s;
        while(getline(ss,s,',')){
            vecs.push_back(s);
        }
        sort(vecs.begin(),vecs.end());
        for(int i=0;i<vecs.size()-1;i++){
            cout << vecs[i] << ',';
        }
        cout << vecs.back() <<endl;
    }
    return 0;
}
```

