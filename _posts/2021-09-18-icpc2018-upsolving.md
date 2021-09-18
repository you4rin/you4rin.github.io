---
title: "[업솔빙] ICPC 2018 풀이 Part 1"
categories:
 - boj
tags:
 - ICPC 2018
 - PS
 - CP
 - BOJ
---

　이번 주는 추석연휴라 모여서 팀연습을 진행하기 힘들 것 같아, 집에서 각자 ICPC 2018 서울 리저널 문제를 풀어보기로 했습니다. 9월 18일 현재 이 셋에서 가장 어려운 문제인 H와 I를 제외한 모든 문제를 풀었고 해당 문제들에 대한 풀이를 올려볼까 합니다.
<hr/>

### 　**D. [Go Latin](https://www.acmicpc.net/problem/16360)**
　티어: **<font color='#db6c00'>브론즈 I</font>**


　입력으로 주어진 문자열의 끝부분을 제공된 표에 따라 변형하면 되는 단순 **구현**문제입니다. 코드는 다음과 같습니다. ~~더럽습니다.~~

```c++
#include<iostream>
#include<string>
#define fastio() ios::sync_with_stdio(0);cin.tie(0);cout.tie(0)

using namespace std;

int main(){
    fastio();
    int n;
    string s;
    for(cin>>n;n--;){
        cin>>s;
        if(s[s.size()-1]=='a')s=s.substr(0,s.size()-1)+"as";
        else if(s[s.size()-1]=='i'||s[s.size()-1]=='y')
			s=s.substr(0,s.size()-1)+"ios";
        else if(s[s.size()-1]=='l')s=s.substr(0,s.size()-1)+"les";
        else if(s[s.size()-1]=='n')s=s.substr(0,s.size()-1)+"anes";
        else if(s.size()>1&&s[s.size()-2]=='n'&&s[s.size()-1]=='e')
			s=s.substr(0,s.size()-2)+"anes";
        else if(s[s.size()-1]=='o')s=s.substr(0,s.size()-1)+"os";
        else if(s[s.size()-1]=='r')s=s.substr(0,s.size()-1)+"res";
        else if(s[s.size()-1]=='t')s=s.substr(0,s.size()-1)+"tas";
        else if(s[s.size()-1]=='u')s=s.substr(0,s.size()-1)+"us";
        else if(s[s.size()-1]=='v')s=s.substr(0,s.size()-1)+"ves";
        else if(s[s.size()-1]=='w')s=s.substr(0,s.size()-1)+"was";
        else s+="us";
        cout<<s<<"\n";
    }
}

```

<hr/>

### 　**F. [Parentheses](https://www.acmicpc.net/problem/16362)**
　티어: **<font color='#db6c00'>골드 I</font>**

　공백은 해당 식이 valid/proper한지 여부에 아무런 영향을 미치지 않으므로, 먼저 제거하고 나면, 입력으로 들어오는 문자열을 크게 네 가지 유형으로 분류할 수 있습니다.

- 알파벳(a-z)
- 연산자(+,-,*,/,%)
- 여는 괄호('(')
- 닫는 괄호(')')

　그리고 각 유형의 문자열은, 바로 앞에 특정 유형의 문자열이 등장하면 error가 발생하는데, 그 목록은 다음과 같습니다.

- 알파벳: 바로 앞에 다른 알파벳이나, 닫는 괄호가 등장하는 경우
- 연산자: 바로 앞에 다른 연산자나, 여는 괄호가 있거나, 첫 번째 문자가 연산자인 경우
- 여는 괄호: 바로 앞에 알파벳이나, 닫는 괄호가 등장하는 경우
- 닫는 괄호: 바로 앞에 연산자나, 여는 괄호가 있거나, 첫 번째 문자가 닫는 괄호인 경우

　그리고 추가로, 짝이 맞지 않는 괄호 문자열이 있을 경우(여는 괄호와 짝이 맞는 닫는 괄호가 없거나, 닫는 괄호와 짝이 맞는 여는 괄호가 없는 경우) error 처리를 해야 하는데, 이는 후술하도록 하겠습니다.
　  
　  

　이제 주어진 문자열이 proper한지 여부를 판별해 봅시다.

- (a)
- (a+b)
- (a+b+c)
- (a+b+c+d)

　첫 번째 식은 괄호가 불필요하게 사용되었으므로 improper, 두 번째 식은 proper, 세 번째 식과 네 번째 식은 a+b와 b+c간의 우선순위가 모호하므로 improper입니다. 알파벳이 다섯 개 이상인 식도 같은 이유로 improper하다는 것도 쉽게 알 수 있습니다.

　즉, 괄호 문자열 안에 있는 식은 정확히 알파벳(또는 괄호 문자열 안의 식) 두 개와 연산자 하나로 이루어져 있어야 proper하다는 것을 알 수 있습니다.

　이 부분에 대한 판별은 **스택**을 사용하면 됩니다. 닫는 괄호가 등장할 때까지 모든 문자를 스택에 넣다가, 닫는 괄호가 등장하면, 여는 괄호가 등장할 때까지 스택에서 문자를 하나씩 꺼냅니다.(여는 괄호가 없다면, error입니다.) 이때, 스택에서 꺼낸 문자 중 알파벳이 정확히 2개, 연산자가 정확히 1개가 아니라면, 주어진 식은 improper하다고 볼 수 있습니다. 그리고 스택에서 꺼내는 작업이 끝난 후에는 스택에 알파벳 하나를 넣어주면 됩니다.

　괄호 문자열 처리가 끝난 후에도 여는 괄호가 남아있거나, 연산자로 끝나는 경우에는 error, 이전에 improper하다고 판명되었거나, 알파벳이 3개 이상 남아있는 경우에는 improper, 나머지 경우는 proper를 출력하면 됩니다. 다만 여기서 주의해야 할 것이, 괄호 문자열의 경우와 달리, 알파벳이 정확히 1개 있는 경우에도 proper를 출력해야 한다는 점입니다. (a)와 달리 a는 proper한 문자열이니까요.

　제 풀이를 코드로 나타내면 다음과 같습니다. 코드의 간결함을 위해, 모든 알파벳을 알파벳 a로, 모든 연산자를 연산자 +로 치환하여 풀었으니, 이 부분 참고 바랍니다.

```c++
#include<iostream>
#include<string>
#include<stack>
#include<algorithm>
#define fastio() ios::sync_with_stdio(0);cin.tie(0);cout.tie(0)

using namespace std;

stack<char> stk;

int main(){
    fastio();
    string s,t="";
    bool proper=true;
    int lcnt=0,rcnt=0;
    getline(cin,s);
    for(auto i:s){
        if(i==' ')continue;
        if(i=='('||i==')')t+=i;
        else if(i>='a'&&i<='z')t+="a";
        else t+="+";
    }
    s=t;
    for(auto i:s){
        if(i=='('){
            if(stk.size()&&(stk.top()=='a'||stk.top()==')')){
                cout<<"error";
                return 0;
            }
            stk.push(i);
            ++lcnt;
            continue;
        }
        if(i=='a'){
            if(stk.size()&&(stk.top()=='a'||stk.top()==')')){
                cout<<"error";
                return 0;
            }
            stk.push(i);
            continue;
        }
        if(i=='+'){
            if((stk.size()&&(stk.top()=='+'||stk.top()=='('))||stk.empty()){
                cout<<"error";
                return 0;
            }
            stk.push(i);
            continue;
        }
		if(stk.size()&&(stk.top()=='+'||stk.top()=='(')){
            cout<<"error";
            return 0;
        }
        int cnt=0;
        ++rcnt;
        while(1){
            if(stk.empty()){
                cout<<"error";
                return 0;
            }
            char c=stk.top();
            stk.pop();
            ++cnt;
            if(c=='(')break;
        }
        if(cnt!=4)proper=false;
        stk.push('a');
	}
    if(lcnt!=rcnt||(stk.size()&&stk.top()=='+')){
        cout<<"error";
        return 0;
	}
    if(stk.size()!=3&&t.size()!=1)proper=false;
    cout<<(proper?"proper":"improper");
}

```

<hr/>

### 　**L. [Working Plan](https://www.acmicpc.net/problem/16368)**
　티어: **<font color='#ffb800'>골드 II</font>**

　이 문제는 다음과 같은 절차로 **그리디**하게 풀 수 있습니다.

1. 휴식이 끝난 사람들을 '일할 수 있는 사람 목록'에 추가한다.
2. 일할 수 있는 사람 중 \\( w_i \\)값이 가장 큰 사람을 선택한다. 일 할 수 있는 사람이 없다면 즉시 -1을 출력한다.
3. 그 사람의 \\( w_i \\)값을 w만큼 줄이고, j일부터 j+w-1일까지, \\( d_j \\)값을 1 줄인다. \\( w_i \\)와 \\( d_j, d_{j+1}, ..., d_{j+w-1} \\) 중 하나라도 음수가 된다면, 즉시 -1을 출력한다.
4. \\( d_j \\)가 0이 될 때까지 2와 3을 반복한다.

　1일부터 n일까지에 대해 1~4 과정을 반복한 후에 \\( w_i \\)와 \\( d_j \\) 중 0이 아닌 값이 하나라도 있다면 -1을, 아니라면 각 사람이 일하는 날짜를 출력하면 \\( O(n \log m ) \\)에 문제를 해결할 수 있습니다. 코드는 다음과 같습니다.

```c++
#include<cstdio>
#include<queue>
#include<vector>
#include<algorithm>

using namespace std;
using pii=pair<int,int>;

int arr[2010],cnt[2010];
vector<int> in[6010];
vector<int> ans[2010];
priority_queue<pii> pq;

int main(){
    int m,n,w,h;
    scanf("%d %d %d %d",&m,&n,&w,&h);
    for(int i=0;i<m;++i)scanf("%d",arr+i);
    for(int i=0;i<n;++i)scanf("%d",cnt+i);
    for(int i=0;i<m;++i)in[0].push_back(i);
    for(int i=0;i<n;++i){
        for(auto j:in[i])pq.push({arr[j],j});
        while(cnt[i]){
            if(pq.empty()){
                printf("-1");
                return 0;
            }
            int idx=pq.top().second;
            pq.pop();
            arr[idx]-=w;
            if(arr[idx]<0){
                printf("-1");
                return 0;
            }
            for(int j=i;j<i+w;++j){
                --cnt[j];
                if(cnt[j]<0){
                    printf("-1");
                    return 0;
                }
            }
            ans[idx].push_back(i);
            in[i+w+h].push_back(idx);
        }
	}
    for(int i=0;i<m;++i){
        if(arr[i]){
            printf("-1");
            return 0;
        }
    }
    printf("1\n");
    for(int i=0;i<m;++i){
        for(auto j:ans[i])printf("%d ",j+1);
        printf("\n");
    }
}

```

<hr/>

### 　**A. [Circuits](https://www.acmicpc.net/problem/16357)**
　티어: **<font color='#2cffbc'>플래티넘 I</font>**

　우선 x축 정보는 필요없는 정보이니 무시하고, 직사각형들을 y축에 평행한 선분으로 취급하고, y좌표에 대해 좌표압축을 합니다. 이제 각 직선의 왼쪽 끝은 \\( v_y \\), 오른쪽 끝은 \\( u_y\\)라 합시다. 이때 [\\( v_y \\), \\( u_y\\)] 구간의 선분을 추가하는 것은, **레이지 세그먼트 트리**에서 [\\( v_y \\), \\( u_y\\)] 구간에 1을 더하는 것으로 생각해 볼 수 있습니다. 이런 식으로 모든 선분을 추가해 두고, 다음과 같은 작업을 수행합니다.

　우선 0 이상 2n-1 이하의 좌표 i에 대해 순회하면서, 두 직선 하나를 좌표 i에 고정해놓고, 이때 해당 직선과 교차하는 선분 개수를 cnt라고 둡시다. 그리고 다음과 같은 작업을 반복합니다.

1. \\( u_y\\)가 i-1인 모든 선분을 세그먼트 트리에 다시 추가하고, cnt값을 1 감소시킵니다. ([\\( v_y \\), \\( u_y\\)] 구간에 1을 더합니다.)
2. \\( v_y\\)가 i인 모든 선분을 세그먼트 트리에서 제거하고, cnt값을 1 증가시킵니다. ([\\( v_y \\), \\( u_y\\)] 구간에 1을 뺍니다.)
3. 세그먼트 트리의 전체 구간에서 최댓값을 MAX라 할 때, 답을 기존 답과 cnt+MAX중 더 큰 값으로 갱신시킵니다.

　이렇게 되면 업데이트 n번, 쿼리 2n번을 수행하기 때문에, \\( O(n \log n ) \\)에 문제를 해결할 수 있습니다. 코드는 다음과 같습니다.

```c++
#include<cstdio>
#include<queue>
#include<vector>
#include<algorithm>
#define N (1<<18)
#define cur seg[pos]
#define left seg[pos*2]
#define right seg[pos*2+1]
#define x first
#define y second

using namespace std;
using pii=pair<int,int>;
using info=pair<int,pii>;

struct Node{
    int s,e,lazy,M;
};

pii arr[100010];
vector<int> v;
priority_queue<info,vector<info>,greater<>> pq;
Node seg[N<<1];

void lazy(int pos){
    if(cur.s!=cur.e){
        for(auto i:{pos*2,pos*2+1}){
            seg[i].lazy+=cur.lazy;
            eg[i].M+=cur.lazy;
        }
        cur.lazy=0;
    }
}

void update(int pos,int s,int e,int val){
    lazy(pos);
    if(cur.s>e||s>cur.e)return;
    if(cur.s>=s&&e>=cur.e){
        cur.lazy+=val;
        cur.M+=val;
        lazy(pos);
        return;
    }
    update(pos*2,s,e,val);
    update(pos*2+1,s,e,val);
    cur.M=max(left.M,right.M);
}

int main(){
    int n,ans=0,cnt=0;
    scanf("%d",&n);
    for(int i=0;i<n;++i){
        scanf("%*d %d %*d %d",&arr[i].y,&arr[i].x);
        v.push_back(arr[i].x);
        v.push_back(arr[i].y);
	}
    sort(v.begin(),v.end());
    v.erase(unique(v.begin(),v.end()),v.end());
    for(int pos=N;pos<2*N;++pos)cur.s=cur.e=pos-N;
    for(int pos=N-1;pos;--pos)cur.s=left.s,cur.e=right.e;
    for(int i=0;i<n;++i){
        arr[i].x=lower_bound(v.begin(),v.end(),arr[i].x)-v.begin();
        arr[i].y=lower_bound(v.begin(),v.end(),arr[i].y)-v.begin();
        pq.push({arr[i].x,{i,-1}});
        pq.push({arr[i].y+1,{i,1}});
        update(1,arr[i].x,arr[i].y,1);
    }
    for(int i=0;i<200000;++i){
        if(pq.empty())break;
        while(pq.size()&&pq.top().x==i){
            int idx=pq.top().y.x,val=pq.top().y.y;
            pq.pop();
            val==1?--cnt:++cnt;
            update(1,arr[idx].x,arr[idx].y,val);
        }
        ans=max(ans,cnt+seg[1].M);
    }
    printf("%d",ans);
}

```

<hr/>

### 　**K. [TV Show Game](https://www.acmicpc.net/problem/16367)**
　티어: **<font color='#28edac'>플래티넘 III</font>**

　3개의 예측 중 2개 이상을 맞힌다는 것은, 다음과 같은, 6개의 조건으로 나타낼 수 있습니다.

- 첫 번째 예측이 틀렸다면, 두 번째 예측은 맞힌다.
- 첫 번째 예측이 틀렸다면, 세 번째 예측은 맞힌다.
- 두 번째 예측이 틀렸다면, 첫 번째 예측은 맞힌다.
- 두 번째 예측이 틀렸다면, 세 번째 예측은 맞힌다.
- 세 번째 예측이 틀렸다면, 첫 번째 예측은 맞힌다.
- 세 번째 예측이 틀렸다면, 두 번째 예측은 맞힌다.

　n명의 참가자에 대해 위의 6가지 조건을 모두 추가하고, **2-SAT**을 돌리면 \\( O(n) \\)에 문제를 해결할 수 있습니다. 코드는 다음과 같습니다.

```c++
#include<cstdio>
#include<stack>
#include<vector>
#include<algorithm>

using namespace std;

struct Node{
    int visit,comp,val;
    vector<int> v;
};

int k,n,cnt;
Node arr[10010];
vector<vector<int>> scc;
stack<int> stk;

int dfs(int idx){
    arr[idx].visit=++cnt;
    int ret=arr[idx].visit;
    stk.push(idx);
    for(auto i:arr[idx].v){
        if(!arr[i].visit)ret=min(ret,dfs(i));
        else if(!arr[i].comp)ret=min(ret,arr[i].visit);
    }
    if(arr[idx].visit==ret){
        vector<int> tmp;
        while(1){
            int t=stk.top();
            stk.pop();
            arr[t].comp=scc.size()+1;
            tmp.push_back(t);
            if(t==idx)break;
        }
        scc.push_back(tmp);
    }
    return ret;
}

void find_scc(int n){
    for(int i=1;i<=n;++i)if(!arr[i].visit)dfs(i);
}

int op(int x){return x>n?x-n:x+n;}

void add_edge(int x,int y){
    if(x<0)x=-x+n;
    if(y<0)y=-y+n;
    arr[x].v.push_back(y);
    arr[op(y)].v.push_back(op(x));
}

bool solve(){
    find_scc(n<<1);
    for(int i=1;i<=n;++i)if(arr[i].comp==arr[op(i)].comp)return 0;
    for(int i=scc.size()-1;i>=0;--i){
        bool flag=false;
        for(auto j:scc[i]){
            if(arr[j].val){
                flag=true;
                break;
            }
        }
        for(auto j:scc[i]){
            arr[j].val=flag;
            arr[op(j)].val=!flag;
        }
    }
    return 1;
}

int main(){
    int a,b,c;
    char ch;
    scanf("%d %d",&n,&k);
    for(int i=0;i<k;++i){
        scanf("%d %c",&a,&ch);
        if(ch=='B')a=-a;
        scanf("%d %c",&b,&ch);
        if(ch=='B')b=-b;
        scanf("%d %c",&c,&ch);
        if(ch=='B')c=-c;
        add_edge(-a,b);
        add_edge(-a,c);
        add_edge(-b,c);
    }
    if(!solve()){
        printf("-1");
        return 0;
    }
    for(int i=1;i<=n;++i)printf(arr[i].val?"R":"B");
}

```

<hr/>

　나머지 5문제의 풀이는 Part 2에서 뵙겠습니다. 감사합니다 :)