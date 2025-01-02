---
title: KMP
katex: true
date: 2024-12-30 21:27:33
excerpt: KMP算法
tag: ds
---

[\[编程题\]字符串匹配](https://www.nowcoder.com/questionTerminal/f7a070bc72e644d68d28fdacc9cc6792)，可以用正则（有时间看看评论区的字典树）。regex 可以看 [c++11的regex使用](https://blog.csdn.net/A315776/article/details/107774791?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167041515216800192217563%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167041515216800192217563&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-107774791-null-null.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=regex_constants::ECMAScript%20%7C%20&spm=1018.2226.3001.4187)，[ios::sync_with_stdio(false)](https://blog.csdn.net/qq_33248299/article/details/52144485?ops_request_misc=&request_id=&biz_id=102&utm_term=ios::sync_with_stdio%28false%29&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-52144485.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&spm=1018.2226.3001.4187) 和 [cin.tie(0)](https://blog.csdn.net/qq_45475271/article/details/107675845?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167041550016800215013271%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167041550016800215013271&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-107675845-null-null.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=cin.tie%280%29&spm=1018.2226.3001.4187) 可以加速IO
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<string> strings;
    ios::sync_with_stdio(false); cin.tie(0);
    for (int n; cin >> n;) {
        strings.resize(n + 1);
        for (int i = 0; i <= n; ++i) cin >> strings[i]; //最后一个字符是pattern
        regex patt(strings.back(), regex_constants::ECMAScript | regex_constants::icase);
        for (int i = 0; i < n; ++i) {
            if (regex_match(strings[i], patt)) cout << i + 1 << ' ' << strings[i] << '\n';
        }
    }
}
```
[\[编程题\]String Matching](https://www.nowcoder.com/questionTerminal/00438b0bc9384ceeb65613346b42e88a)，用 find 即可解决，find 的第二个字符是给定字符串起始查找的位置
```cpp
#include <bits/stdc++.h>

using namespace std;
int main() {
		string s,t;
		cin>>s>>t;
		int count=0,i=s.find(t);
		while(i!=-1){
			count++;
			i=s.find(t,i+1);
		}
		cout<<count;
		return 0;
}
```
```cpp
#include <bits/stdc++.h>
using namespace std;

void BuildMatch(const string& patt,int *match){
    int i,j;
    match[0]=-1;
    int m=patt.size();
    for (j=1;j<m;j++){  //O(m)
        i=match[j-1];
        while ((i>=0) && (patt[i+1]!=patt[j])) i=match[i];  //找到之前已经匹配的前缀
        if (patt[i+1]==patt[j]) match[j]=i+1;   //i回退的次数不会超过增加的总次数
        else match[j]=-1;
        //printf("j=%d,i=%d\n",j,i);
    }
    //for (int i=0;i<m;i++) printf("match[%d]=%d\n",i,match[0]);
}

int KMP(const string& s,const string& patt){
    //设置两个指针sIndex和pIndex分别指向正在比较的两个字符
    int n=s.size();         //O(n)
    int m=patt.size();      //O(m)
    int sIndex=0,pIndex=0;
    int* match=new int[m];
    BuildMatch(patt,match);
    //for (int i=0;i<m;i++) printf("match[%d]=%d\n",i,match[0]);
    while (sIndex<n && pIndex<m){   //O(n)
        //printf("sIndex=%d,pIndex=%d\n",sIndex,pIndex);
        if (s[sIndex]==patt[pIndex]){
            sIndex++;
            pIndex++;
        }
        else if (pIndex>0) pIndex=match[pIndex-1]+1; //直接位移到匹配的前缀那里
        else sIndex++;
    }
    //printf("pIndex=%d\n",pIndex);
    return (pIndex==m)?(sIndex-m):-1;
}
int main(){
    string s="This is a simple example.";
    string patt="simple";
    int i=KMP(s,patt);
    //printf("i=%d\n",i);
    if (i==-1) printf("Not found.\n");
    else printf("%c",s[i]);
}

```

