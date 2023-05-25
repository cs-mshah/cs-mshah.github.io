# Getting started with competitive programming

<!--more-->
## What this post is about
This post assumes that 
- you are familiar with [Codeforces](https://codeforces.com/blog/entry/99660) and competitive programming
- you know the basics of a particular programming language ([C++](https://codeforces.com/blog/entry/74684))

This post is
- more about getting a good setup (according to me)
	- setting up [CP Editor](https://cpeditor.org/), [cf-tool](https://github.com/xalanq/cf-tool) and competitive companion
	- some optimisations like pre-compiling headers
- about setting and using a debug template
- some resources to practice CP.

## Selecting the right editor

There are a few good choices for selecting an editor for CP. Here are some:
- CP editor
- Sublime Text
- VS Code

I'll go over the best setup according to me, which is the CP editor with a few other extensions. 
1. First, go and download [CP editor](https://cpeditor.org/download/). If using linux, an AppImage would be fine. Just download the AppImage and give execute permissions.
2. Download [cf tool](https://github.com/xalanq/cf-tool/releases/tag/v1.0.0). Extract the executable and add it to a location which is in your PATH. As this is a command line tool, it should be in your PATH.
    - If on a linux machine, you could `cd` into the folder where you extracted the cf tool and on the terminal type: `sudo cp cf /usr/bin/`. This would copy the cf tool to `/usr/bin/`. Note: You can use any other location which is in your `$PATH`.
    - If using windows, have a look at: [Add to the PATH on Windows 10](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/).  

Now, we need to configure the cf tool, which is where most of the trouble comes. So open the terminal and type `cf`. If you are unable to see the usage instructions, then it means that your `cf` isn't in the PATH. Go above and and do the steps correctly.  
- Now type `cf config`. Option `0` for login (as we are using it for the first time).
- After successfully logging in, we will configure our template, so that the tool gets ready for submitting to codeforces. So go ahead and type `cf config` option `1`. Now select the language code.
- Now enter the absolute path to your template (Look at the template section and come back, If you don't know what this is). 
- `The suffix of template above will be added by default.` - leave empty
- `Template's alias (e.g. "cpp" "py"):` - In my case I used cpp
- `Before script` - leave empty
- `Script` - just add the path to your default template as empty isn't allowed.
- `After script` - leave empty
- `Make it default (y/n)?` - `y`  

3. Add [competitive companion](https://github.com/jmerle/competitive-companion) to your browser.
    - Go to the options of the extension and add `10045` to `Custom ports`. This is the port which `CP editor` will listen to for requests.
    - This extension is able to parse the testcases and send them to a port for a large variety of competitive programming websites.

With the setup almost done, just open `CP editor` and check if the `Messages` section is clean. If there is some error, it means that some tool isn't configured correctly. If there is an issue with the compiler, then make sure that `g++` is in your `$PATH`. Open the `preferences` tab and edit your preferences. (preference help can be found on their site too)

To test if everything works well, go to any problem on codeforces, and click on the green + icon of your competitive companion extension. Now after opening the `CP editor`, you should be able to see a new file loaded with the default template and the sample testcases of the problem. Write the code and press `compile and run` to see the automatic checking of testcases. You can even edit/add testcases and change the type of checker. After you feel that all cases work out, just hit the submit button, to directly submit the problem from within the editor without leaving it!! If you get any error in `messages`, that means that `cf tool` hasn't been configured correctly.

{{< admonition type=success title="Hurray!" open=true >}}
Hurray !! You have configured your editor with a great setup and can easily submit/check any problem on codeforces
{{< /admonition >}}

## Making your default template  

To speed up the coding, a default template would be nice to have. Here is my default `c++` template (as of now. I do make changes to it):  

<details>
<summary>Template</summary>

```cpp
#pragma GCC target ("avx2")
#pragma GCC optimization ("O3")
#pragma GCC optimization ("unroll-loops")

#include "bits/stdc++.h"
/*#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>*/

using namespace std;
//using namespace __gnu_pbds;

typedef long long ll;
typedef long double ld;
typedef pair<ll,ll> pll;
typedef vector<bool> vb;
typedef vector<int> vi;
typedef vector<ll> vll;
typedef vector<vi> vvi;
typedef vector<vb> vvb;
typedef vector<vll> vvll;
typedef vector<pll> vpll;
typedef vector<string> vs;
typedef unordered_map<ll,ll> umll;
template<class T> using pq = priority_queue<T, vector<T>, greater<T>>;
/*template <typename num_t>
using ordered_set = tree<num_t, null_type, less<num_t>, rb_tree_tag, tree_order_statistics_node_update>;*/
// find_by_order(k): iterator to the kth largest(0 indexed). order_of_key(k): no. of items < k

#define GET_MACRO(_1,_2,_3,_4,NAME,...) NAME
#define FOR3(i,a,b) for(long long i=a;i<b;i++)
#define FOR4(i,a,b,step) for(long long i=a;i<b;i=i+step)
#define REV3(i,a,b) for(long long i=a;i>=b;i--)
#define REV4(i,a,b,step) for(long long i=a;i>=b;i=i-step)
#define FOR(...) GET_MACRO(__VA_ARGS__, FOR4, FOR3)(__VA_ARGS__)
#define REV(...) GET_MACRO(__VA_ARGS__, REV4, REV3)(__VA_ARGS__)
#define F first
#define S second
#define pb push_back
#define ub upper_bound
#define lb lower_bound
#define all(v) v.begin(),v.end()
#define rall(v) v.rbegin(),v.rend()
#define tc ll tests;cin>>tests;while(tests--)
#define io ios_base::sync_with_stdio(false);cin.tie(nullptr);
#define coutv(v) for(auto it: (v))cout<<it<<" ";newl;
#define cout2d(v) for(auto it: (v)) {for(auto j:it) cout<<j<<" ";newl;}
#define cinv(v,n) vll (v)(n);FOR(i,0,(n)){cin>>v[i];}
#define cinvg(v,n) (v).resize(n);FOR(i,0,(n)){cin>>v[i];}
#define cin2d(v,n,m) vvll (v)(n,vll(m,0));FOR(i,0,n){FOR(j,0,m){cin>>v[i][j];}}
#define cin2dg(v,n,m) (v).resize(n,vll(m));FOR(i,0,n){FOR(j,0,m){cin>>v[i][j];}}
#define pyes(CONDITION) cout << (CONDITION ? "YES" : "NO") << '\n';
#define newl cout<<"\n"
#define MOD 1000000007
#define INF LLONG_MAX/2
#define m1(x) template<class T, class... U> void x(T&& a, U&&... b)
#define m2(x) (int[]){(x forward<U>(b),0)...}
m1(pr) { cout << forward<T>(a);  m2(cout << " " <<); cout << "\n"; } 
m1(re) { cin >> forward<T>(a); m2(cin >>); }
template <class ...Args>
auto &readd(Args &...args) { return (cin >> ... >> args); }
#define re(...) __VA_ARGS__; readd(__VA_ARGS__)

const ll dx[4] = {0,1,0,-1}, dy[4] = {1,0,-1,0};
// const ll dx[8] = {-2,-1,1,2,2,1,-1,-2}, dy[8] = {1,2,2,1,-1,-2,-2,-1}; //knight moves

// ************************DEBUG START********************************
#ifndef ONLINE_JUDGE
//#define cerr cout
#include "myprettyprint.hpp"
#else
#define dbg(...)
#endif
// ************************DEBUG END**********************************

constexpr ll pct(ll x) { return __builtin_popcountll(x); } // # of bits set
constexpr ll bits(ll x) {return x == 0LL ? 0LL : 63LL-__builtin_clzll(x); } // floor(log2(x)) 
constexpr ll p2(ll x) { return 1LL<<x; }
constexpr ll msk2(ll x) { return p2(x)-1LL; }
mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());

void test()
{
    
}

int main()
{
    io;
    ll tests=1;
    cin>>tests;
    while(tests--)
    {
    	test();
    }
    return 0;
}
```

</details>  
<br>

Lets have a look at how to use the template and some features:  
- `typedefs` are used to define a new type from existing data types. Have a look at this [GFG article](https://www.geeksforgeeks.org/typedef-versus-define-c/) for a better understanding.
- `#define` defines aliases to certain things.
- `io`: `ios_base::sync_with_stdio(false);cin.tie(nullptr);` for fast input output.
- `GCC pragmas`: these are compiler specific, and may optimise certain solutions. There is no guarantee, but it may even work.
-   
    ```cpp
    #ifndef ONLINE_JUDGE
    //#define cerr cout
    #include "myprettyprint.hpp"
    #else
    #define dbg(...)
    #endif
    ```
    
    By default the ONLINE_JUDGE constant is defined when submitting code in most online judges ex: codeforces or codechef. It helps the code to determine whether the code is being run on an online judge or on a local system machine.  
    So how can we take advantage of this?  
    With a `#ifndef` we can have various things like custom header files and other definitions locally. `#ifndef` means 'if not defined'.  
    Now an `else` would mean when it is defined (so on the judge). In this case, we can simply set our custom definitions to "" (empty string), so that they are ignored in the code which the online judge compiles.
    The `#ifndef` directive ends with `endif`.  
    
    Now lets have a look at **how to use the template :**
    
- <details>
  <summary>Loops</summary>
  
  ```cpp
  // normal code:                 // using template:
  for(long long i=0;i<n;i++) {}   FOR(i,0,n) {}
  for(long long i=0;i<n;i+=5) {}  FOR(i,0,n,5) {} // we can use different parameters!!
  
  for(long long i=n;i>=0;i--) {}  REV(i,n,0) {}
  for(long long i=n-2;i>=1;i-=2) {}  REV(i,n-2,1,2) {}
  
  ```
  
  </details>
  
- <details>
  <summary>Inputs</summary>
  
  ```cpp
  // normal code:                 // using template:
  ll i, j, k;                     ll re(i,j,k);
  cin>>i>>j>>k;                   // accepts multiple parameters and takes input of the type specified, declaring the variables locally.
  
  // If you have different datatypes/have globally declared variables, use: 
  ll n;
  string s;
  main()
  {
      re(n,s);
  }
  
  // input vectors:
  cinv(a, n) // note: semi-colon not needed
  cinvg(a, n) // a is already globally declared
  cin2d(g, n, m) // 2d vector of size n*m
  cin2dg(g, n, m) // 2d global vector of size n*m
  
  newl; // newline
  pyes(boolean) // print YES if true, else NO
  
  coutv(a, n) // print vector contents space separated and endline
  cout2d(g, n, m) // print 2d vector
  ```
  
  </details>

- <details>
  <summary>STL</summary>
  
  ```cpp
  // normal code:                         // using template:
  p.first                                 p.F
  p.second                                p.S
  sort(a.begin(), a.end());               sort(all(a));
  v.push_back(5);                         v.pb(5);
  auto it=upper_bound(a.begin(),a.end(),7) auto it=ub(all(a),5)
  v.rbegin(),v.rend()                     rall(v)
  priority_queue<ll,vector<ll>,greater<ll>> pq<ll>
  
  ```
  
  </details>
  
- <details>
   <summary>myprettyprint.hpp</summary>
  
   > myprettyprint.hpp: Read the debugging section to know more!!
  
   </details>
  
  There are several other possibilites that you can add too.
  
## Debugging  

This is a very important part in programming. Everyone faces bugs and we should know how to find them quickly.  
Here are a few ways:  
- Writing print statements to print the value of variables.
- Using a debugger and debugging using breakpoints.

Both of the above methods have shortcomings.The first one is very slow and while submitting any code, we need to remove the print statement. The second one is not easy for a beginner and requires some other setup and knowledge. So, we need to find something better and smarter. We use our own debugging library.  

Firstly, one should use good compiler flags to compile the code, so that several things like - `Integer Overflow`, `Array out of bounds` and several other errors can be caught and pointed out by the compiler.  
Here is the compile command which catches several errors and is really useful:  

 `g++ -std=c++1z -D_GLIBCXX_DEBUG -D_GLIBCXX_DEBUG_PEDANTIC -g -fsanitize=undefined`
(if you want to know more: [link](https://codeforces.com/blog/entry/15547))
You can add this to your c++ compile command in the preferences of CP editor.

Now, coming to the main part. **The debugging template:**

<details>
<summary>myprettyprint.hpp</summary>

```cpp
template <class T1, class T2>
ostream &operator<<(ostream &os, const pair<T1, T2> &p) {
  return os << '{' << p.first << ", " << p.second << '}';
}

template <class T, class = decay_t<decltype(*begin(declval<T>()))>,
          class = enable_if_t<!is_same<T, string>::value>>
ostream &operator<<(ostream &os, const T &c) {
  os << '[';
  for (auto it = c.begin(); it != c.end(); ++it)
    os << &", "[2 * (it == c.begin())] << *it;
  return os << ']';
}
//support up to 5 args
#define _NTH_ARG(_1, _2, _3, _4, _5, _6, N, ...) N
#define _FE_0(_CALL, ...)
#define _FE_1(_CALL, x) _CALL(x)
#define _FE_2(_CALL, x, ...) _CALL(x) _FE_1(_CALL, __VA_ARGS__)
#define _FE_3(_CALL, x, ...) _CALL(x) _FE_2(_CALL, __VA_ARGS__)
#define _FE_4(_CALL, x, ...) _CALL(x) _FE_3(_CALL, __VA_ARGS__)
#define _FE_5(_CALL, x, ...) _CALL(x) _FE_4(_CALL, __VA_ARGS__)
#define FOR_EACH_MACRO(MACRO, ...)                                             \
  _NTH_ARG(dummy, ##__VA_ARGS__, _FE_5, _FE_4, _FE_3, _FE_2, _FE_1, _FE_0)     \
  (MACRO, ##__VA_ARGS__)
//Change output format here
#define out(x) #x " = " << x << "; "
#define dbg(...)                                                              \
  cerr << "Line " << __LINE__ << ": " FOR_EACH_MACRO(out, __VA_ARGS__) << "\n"
```

</details>
<br>

This might seem scary to look at, but there's no need to get scared as we only need to know how to use it. In fact, we can smartly put this in a header `myprettyprint.hpp` and simply include it using `ifndef ONLINE_JUDGE`, as I have done in my template.
For the include to work, `myprettyprint.hpp` should be placed in a directory so that it is in the search path when compiling.
on linux, you can place it here: `/usr/include/c++/9/myprettyprint.hpp`.
If you don't know where the compiler looks for header files, then see: [gcc Search Path](https://gcc.gnu.org/onlinedocs/cpp/Search-Path.html).

**How to use:**

<details>
<summary>debug examples</summary>

```cpp
ll my_func(ll a, ll b)
{
    return a*b;
}

void test()
{
    int x=5, y=4;
    string s="Hello";
    vector<int> v={1,2,3,4,5};
    set<int> st={34,45};
    map<int, vector<int> > m;

    for(int i=0;i<3;i++)
    {
        for(int j=0;j<4;j++)
        {
            m[i].push_back(i+j);
        }
    }

    dbg(x, y, s, v, st);
    dbg(m, my_func(3,7));
}
```

</details>

<details>
<summary>output</summary>

```cpp
Line 43: x = 5; y = 4; s = Hello; v = [1, 2, 3, 4, 5]; st = [34, 45]; 
Line 44: m = [{0, [0, 1, 2, 3]}, {1, [1, 2, 3, 4]}, {2, [2, 3, 4, 5]}]; my_func(3,7) = 21;
```

</details>

So this debug template is able to print out a variety of `stl containers` including the ones in `pbds` (policy based data structures) too.
So while submitting the problem, we need not even remove the `dbg()` statements, as they get automatically converted to empty strings. (`else dbg(...)`)

If you are printing your output to a terminal which supports `ANSI based colours`, you could even use this debug template:
<details>
<summary>colour debug template</summary>

```cpp
// colout print
template <class T1, class T2>
ostream &operator<<(ostream &os, const pair<T1, T2> &p) {
  return os << '{' << p.first << ", " << p.second << '}';
}

template <class T, class = decltype(begin(declval<T>())),
          class = enable_if_t<!is_same<T, string>::value>>
ostream &operator<<(ostream &os, const T &c) {
  os << '[';
  for (auto it = begin(c); it != end(c); ++it)
    os << (it == begin(c) ? "" : ", ") << *it;
  return os << ']';
}

#define _NTH_ARG(_1, _2, _3, _4, _5, _6, N, ...) N
#define _FE_1(_CALL, x) _CALL(x)
#define _FE_2(_CALL, x, ...) _CALL(x) _FE_1(_CALL, __VA_ARGS__)
#define _FE_3(_CALL, x, ...) _CALL(x) _FE_2(_CALL, __VA_ARGS__)
#define _FE_4(_CALL, x, ...) _CALL(x) _FE_3(_CALL, __VA_ARGS__)
#define _FE_5(_CALL, x, ...) _CALL(x) _FE_4(_CALL, __VA_ARGS__)
#define _FE_6(_CALL, x, ...) _CALL(x) _FE_5(_CALL, __VA_ARGS__)
#define FOR_EACH_MACRO(MACRO, ...)                                             \
  _NTH_ARG(__VA_ARGS__, _FE_6, _FE_5, _FE_4, _FE_3, _FE_2, _FE_1)              \
  (MACRO, __VA_ARGS__)
#define watch(x) cerr << "\033[1;32m" #x " = \033[1;34m" << (x) << "\033[0m; ";
#define dbg(...)                                                             \
  cerr << "\033[2;31mLine " << __LINE__ << ": \033[0;m";                       \
  FOR_EACH_MACRO(watch, __VA_ARGS__)                                           \
  cerr << "\n"
```

</details>

<details>
<summary>output</summary>
<img src="https://i.imgur.com/K8iRhFh.png"></img>
</details>

With all this, debgging should become very easy and quick now. Just print out whatever you want.

## Precompiling header files

This section is mostly for those who face long compile times `(>2s-3s)`.

One way to reduce compile time is to precompile the header files using the same compile command that you use to compile your code.

- navigate to the header file `bits/stdc++.h` on your terminal (`/usr/include/x86_64-linux-gnu/c++/9/bits` on ubuntu) and type:
`sudo g++ -std=c++1z -D_GLIBCXX_DEBUG -D_GLIBCXX_DEBUG_PEDANTIC -g -fsanitize=undefined stdc++.h`
- You could also copy the above file and place it in the directory having your code. Precompiling here will also generate the `.gch` file. All you need to change in your code is `#include <bits/stdc++.h>` to `#include "<bits/stdc++.h>"`. This will tell the compiler to search for a local header in the `bits` subdirectory and name `stdc++.h` first, and then the default search path. When the compiler finds a `.gch` file, it uses it instead of the `.h` file.
```
CP_files
│ 
│   probA.cpp    
│
└───bits
    │   stdc++.h
    |   stdc++.gch 

```

This should reduce your compile time a little.

## Resources

**PRACTICE PRACTICE PRACTICE**. The only thing that can help you get better at CP.  

Here are some resources which are best for practice and learning. Note that these are just some good ones according to me. There are several others which you can easily find, but the point to remember is to *focus on actual practice* and *not to get overwhelmed* by the mammoth content out there.

###  Practice and Learning

- [USACO guide](https://usaco.guide/) : This is one of the finest resources written by topcoders. It has everything. Right from guidance to practice problems.
- [hackerearth](https://www.hackerearth.com/practice/algorithms/searching/linear-search/tutorial/) : this site contains good articles and practice problems.
- [cp-algorithms](https://cp-algorithms.web.app/) : one of the best references for theory and implementation of a large variety of algorithms in c++.
- [codeforces](https://codeforces.com/problemset?tags=-1300) : One of the best sites to practice and improve. It contains severals problems and contests. Just sort the problems based on a particular rating (typically +200-300 of your current) and start practicing. You can also host mashups and practice.
- [codechef](https://www.codechef.com/LEARNDSA/?itm_medium=navmenu&itm_campaign=learndsa) : This site has good problems and contest too. The DSA learning series is also amazing to learn a particular topic.
- [atcoder](https://atcoder.jp/) : This site hosts good quality problems. the `ABC` beginner contests are resourceful to a beginner. 
- [a2oj](https://a2oj.com/) : This has currently been discontinued, but [with this extension](https://github.com/pratikdaigavane/A2OJ-Enhancer), you can pick a ladder and start solving.
- Blogs
	- [Catalog - Codeforces](https://codeforces.com/catalog)
	- [The Ultimate Topic List (with Resources, Problems and Templates)s](https://codeforces.com/blog/entry/95106)

### Youtube channels

- [Code N Code](https://www.youtube.com/channel/UC0zvY3yIBQTrSutsV-4yscQ): A good channel which explains several topics.
- [Kartik Arora](https://www.youtube.com/user/MrHulasingh25) : He is a CM himself and explains several topics.
- [Priyansh Agarwal](https://www.youtube.com/c/PriyanshAgarwal) : He is a CM himself and explains several topics and does screencasts.
- [Utkarsh Gupta](https://www.youtube.com/channel/UCGS5ZzcSAymQbWZvNoKOFhQ): A GM himself, has explained and done several screen casts.
- [Colin Galen](https://www.youtube.com/c/ColinGalen): A GM who regularly does screen casts of contests.
- [Neal Wu](https://www.youtube.com/channel/UC6MXTfIi_pSMxwPQDKT2VGg): A legend's channel!

### Browser extensions

- [codeforces customizer](https://codeforces.com/blog/entry/93536): An extension made by me!! Check the blog to see what it does. Use the `all submissions` tab to see others' submissions and improve! (also do not forget to upvote the blog and rate a 5 star 🌟 on the chrome web store :grin:)
- [keep problems](https://chrome.google.com/webstore/detail/keep-problems/bpcgbgiipbblkoajepkmlcdgafnhiamp): use this to mark favourite problems/add notes/export etc.

