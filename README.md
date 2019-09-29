# ICPC Preparation Notebook

***Prepared (Ongoing) by*** 
***Mohammad Sheikh Ghazanfar***
***Machine Learning R&D, Nascenia***

* [Dynamic Programming](#dp)
   * [Knapsack](#knapsack)
* [Math](#math)
      * [Counting](#counting)
        - De-arrangement 
        - Binomial Coeffecient
        - Catalan Number
      * [Number Therory](#number_theory)
        - Primes (Sieve Method)
        - Euler Totient
* [Data Structure](#ds)
   * [Segment Tree](#segment_tree)
   * [Binary Indexed Tree](#bit)
   * [Policy-Based Data Structure](#pbds)
* [Graph](#graph)
* [My template](#template)
____
## <b name="dp">Dynamic Programming</b>

#### <a name="knapsack">Knapsack</a>

## <b name="coding">Data Structures</b>

#### <a name="segment">Segment Tree</a>

- Do point and range update in an array (adding k to each number between *l-r*  etc.) in *O(logn)*
- Get range query (Sum of all numbers between *l-r*) in *O(logn)*
- Building the tree is not always required (Note that building the whole tree at first requires *O(nlogn)*)

```c++

#define MAX_SIZE 1000002

int arr[MAX_SIZE];
int tree[MAX_SIZE*3];
int lazy[MAX_SIZE*3];

void build(int node,int b,int e)
{
    if(b==e){
        tree[node]=arr[b];
        return;
    }
    int mid=(b+e)/2;
    build(node*2,b,mid);
    build(node*2+1,mid+1,e);
    tree[node]=tree[node*2]+tree[node*2+1];
}

void update(int node,int b,int e,int l,int r,int val)
{
    if(lazy[node]!=0){
        tree[node]+=(e-b+1)*lazy[node];
        if(b!=e){
            lazy[node*2]+=lazy[node];
            lazy[node*2+1]+=lazy[node];
        }
        lazy[node]=0;
    }
    if(b>e || b>r || e<l){
        return;
    }
    if(b>=l && e<=r){
        tree[node]+=(e-b+1)*val;
        if(b!=e){
            lazy[node*2]+=val;
            lazy[node*2+1]+=val;
        }
        return;
    }
    int mid=(b+e)/2;
    update(node*2,b,mid,l,r,val);
    update(node*2+1,mid+1,e,l,r,val);
    tree[node]=tree[node*2]+tree[node*2+1];
}

ll query(int node,int b,int e,int l,int r)
{
    if(b>e || b>r || e<l){
        return 0;
    }
    if(lazy[node]!=0){
        tree[node]+=(e-b+1)*lazy[node];
        if(b!=e){
            lazy[node*2]+=lazy[node];
            lazy[node*2+1]+=lazy[node];
        }
        lazy[node]=0;
    }
    if(b>=l && e<=r){
        return tree[node];
    }
    int mid=(b+e)/2;
    int p1=query(node*2,b,mid,l,r);
    int p2=query(node*2+1,mid+1,e,l,r);
    return (p1+p2);
}

```



#### <a name="pbds">Policy-Based Data Structure</a>

Following modules need to be included to use PBDS.

```c++
#include <ext/pb_ds/assoc_container.hpp> // Common file
#include <ext/pb_ds/tree_policy.hpp> // Including tree_order_statistics_node_update

#include <ext/pb_ds/detail/standard_policies.hpp>

using namespace __gnu_pbds;
typedef tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> pbds;
```

It works as a set but elements at the i-th position can be fetch using **find_by_operation(index)** and numbers strictly less than a given number k can be found by **order_of_key(k)**.

```c++
    pbds X;
    X.insert(1);
    X.insert(2);
    X.insert(4);
    X.insert(8);
    X.insert(16);

    cout<<*X.find_by_order(1)<<endl; // 2
    cout<<*X.find_by_order(2)<<endl; // 4
    cout<<*X.find_by_order(4)<<endl; // 16

    cout<<X.order_of_key(-5)<<endl;  // 0
    cout<<X.order_of_key(1)<<endl;   // 0
    cout<<X.order_of_key(3)<<endl;   // 2
    cout<<X.order_of_key(4)<<endl;   // 2
    cout<<X.order_of_key(400)<<endl; // 5
```



## <b name="template">My Template</b>

```c++
/*
****Mohammad Sheikh Ghazanfar****
	Competitive Programmer,
	Machine Learning Team, Nascenia,
	Former Programmer, Chief ERP,
	Bachelor of  Engineering in Computer Science and Engineering,
	Department of Computer Science and Engineering,
	International Islamic University Chittagong
	Email : skghazanfar@gmail.com
*/

 /*Header file starts here*/

//#include <bits/stdc++.h>
#include <vector>
#include <list>
#include <map>
#include <set>
#include <queue>
#include <stack>
#include <bitset>
#include <algorithm>
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <cmath>
#include <cstdlib>
#include <cmath>
#include <iterator>
#include <cstdio>
#include <cstdlib>
#include <cctype>
#include <climits>
#include <string>
#include <sstream>
#include <cstring>
#include <ctime>

/*Header File ends here*/

/*Macro starts here*/

#define f(i,j,n) for(int i=j; i<n; i++)
#define fu(i,j,n) for(int i=j; i>n; i--)
#define c(x, n) x n; cin>>n
#define in(name, n) for(int ___i=0;___i<n;___i++) { cin >> name[___i];}
#define vin(type, name, n) for(int ___i=0;___i<n;___i++) { type x; cin >> x; name.push_back(x); }
#define ft int t; scanf("%d", &t); for(int c=1; c<=t; c++)
#define w(n) while(n--)
#define visited 1
#define unvisited -1
#define debug printf("\n<<CameHere<<\n")
#define mem(x,y) memset(x, y, sizeof(x))
#define pal(temp) pali(temp, 0, temp.size()-1)
#define nl printf("\n")
#define space printf(" ");
#define eps 1e-9
#define inf 1<<28
#define cases printf("Case %d:", c);
#define all(v) (v).begin(), (v).end()
#define pb push_back
#define mii make_pair
#define min3(a,b,c) min(min(a,b),c)
#define min4(a,b,c,d) min(min(a,b),min(c,d))
#define max3(a,b,c) max(max(a,b),c)
#define max4(a,b,c,d) max(max(a,b),max(c,d))
#define fin freopen("in.txt", "r", stdin)
#define fout freopen("out.txt", "w", stdout)
#define fast ios_base::sync_with_stdio(false); cin.tie(NULL)
#define MOD (ll)1e9+7
#define gamma 0.57721566490153286060651209008240243
#define PI acos(-1)
#define pi 2*acos(0)
#define cos(a) cos(a*pi/180)
#define sin(a) sin(a*pi/180)
#define tan(a) tan(a*pi/180)
#define cosi(a) acos(a)/(pi/180)
#define sini(a) asin(a)/(pi/180)
#define tani(a) atan(a)/(pi/180)
#define tanii(a,b) atan2(a,b)/(pi/180)//tan(90) = Infinity or a/0
// log(a)=ln(a) base 2.718281828459044979,, log10(a) base 10;
#define powe(a) exp(a)//e^a
//Extended Geometry
#define dis(x1,y1,x2,y2) ((x1-x2)*(x1-x2))+((y1-y2)*(y1-y2))
#define t_area(a,b,c,s) sqrt(s*(s-a)*(s-b)*(s-c))
#define t_angle(a,b,c) cosi((b*b+c*c-a*a)/(2*b*c))

#define deb(x) cerr<< #x <<"="<<x<<endl;

#define si(a) int a; scanf("%d", &a);
#define sll(a) ll a; scanf("%lld", &a);
#define sllu(a) unsigned ll a; scanf("%llu", &a);
#define sd(a) double a; scanf("%lf", &a);
#define sc(a) char a; scanf("%c", &a);

//#define p printf
/*Macro Ends here*/

using namespace std;

/*typedef start here*/

typedef string st;
typedef long long ll;
typedef vector <int> vi;
typedef vector <ll> vll;
typedef vector <string> vs;
typedef pair <int, int> ii;
typedef vector < pair <int, int> > vii;
typedef priority_queue<int,vector<int>,greater<int> > min_heap;

/*typedef ends here*/

/*Important functions starts here */

template <class T> T abs (T a, T b=0)
{
    if(b==0) return (a<0) ? -a : a;
    return (a>b) ? (a-b) : (b-a);
}

ll xp(ll b, ll n, ll m) {
	ll r = 1;
	while(n > 0) {
		if(n & 1) {
			r = r * b;
			if(r >= m) r %= m;
		}
		n >>= 1;
		b = b * b;
		if(b >= m) b %= m;
	}
	return r % m;
}

ll inv(ll a, ll m){
    return xp(a, m-2, m);
}

/*Important functions ends here*/

/*Debug Extension starts here*/

template <typename F, typename S>
ostream& operator << (ostream& os, const pair< F, S>& p)
{
    return os<<"(" <<p.first<<", "<<p.second<<")";
}

template<typename T> ostream &operator << (ostream &os, const vector<T> &v)
{
    os<<"{";
    typename vector<T> :: const_iterator it;
    for(it= v.begin(); it!=v.end(); it++)
    {
        if(it!=v.begin()) os<<",";
        os<<*it;
    }
    return os<<"}";
}

template <typename T> ostream &operator << (ostream & os, const set<T>&v)
{
    os<<"[";
    typename set<T> :: const_iterator it;
    for(it=v.begin(); it!=v.end(); it++)
    {
        if(it!=v.begin()) os<<",";
        os<<*it;
    }
    return os<<"]";
}

template <typename F, typename S> ostream &operator << (ostream & os, const map<F,S>&v)
{
    os<<"[";
    typename map<F,S>::const_iterator it;

    for(it=v.begin(); it!=v.end(); it++)
    {
        if(it != v.begin()) os<<",";
        os<< it ->first<<"="<<it->second;
    }
    return os<<"]";
}

/*Debug Extension ends here*/

void precompute(){

}

void clear(){

}

int main(){

	precompute();

	ft{
		clear();
	}

return 0;}
```
