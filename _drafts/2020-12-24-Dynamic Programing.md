---
layout: post
title: Dynamic Programming
tags: [Algorithm,DP,Dynamic Programming]
---
# Dynamic Programming

> * [정의](##정의)
> * [개념](##개념)
> * [예제](##예제)

## 정의

> Dynamic Programming은 memoization을 통해서 주어진 조건에 따라 optimal value를 구할 때, subproblems(점화식 형태)로 나누어 풀 때 사용된다.
>

* ``복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법을 말한다``라고 [나무위키](https://namu.wiki/w/%EB%B0%B1%ED%8A%B8%EB%9E%98%ED%82%B9)에 나와있다. 

* 핵심단어는 다음과 같다.
  * **나누어 푼다**
  
* 주어진 값들에 대하여 각각의 값을 하나의 문제라고 가정하자. 이렇게 나눠진 문제(값)를 풀어나아가며 memoization에 해당 값을 누적시킨다면, optimal 값을 얻을 수 있다.

  > :exclamation: Subproblem이라고 해서 recursive하게 들어가야 한다는 생각은 하지 말자:exclamation:		

* 결국, DP의 핵심은 subproblem이며 이것은 **점화식(Recurrence Relation)**을 code로 옮기는 것을 의미한다.

## 개념

### Description

* 구성은 다음과 같다
  * Values
  * Memoization
  * Subproblem
  
* DP의 문제는 보통 주어진 **Values$$_{\text{값들}}$$**들로 부터 조건에 의한 optimal value를 구하는 것이 목적이다. 그렇기 때문에 **조건으로 부터 점화식**을 이끌어 내는 것이 핵심이다.

  

### Subproblems

> 보통 점화식을 생각하면 recusrive하게 들어간다는 생각을 많이 한다. 하지만, DP문제에서 정의되는 점화식은 tree 형태의 subproblem이 아닌, **이미 누적된 값들과** 비교한다고 관점에 보는 것이 옳다.

* 여기서 이미 누적된 값들이라는 말은, 이미 subproblem을 통해 optimal value가 memoization에 누적된 값들을 의미한다. 쉽게 말해 현재 subproblem의 값을 지정하기 위해서는 이미 풀은 subproblem들의 값을 참조하여 현재 점화식 값과 비교하여 값을를 선정한다는 것이다.
* 다음은 점화식을 구할 때 도움이될 수 있는 개념이 있다.
  * **Memoization$$_{\text{누적된 optimal Values}}$$** 는 보통 ``dp[i]`` 혹은 ``dp[i][j]``와 같이 array형식으로 구성된다. 이 때, ``dp[i]``의 의미는 보통 **i번 째 value에 대한 subproblem을 풀었을 때의 값이 ``dp[i]`` 값이 된다** 이다.
  * 이 때, 선정되는 ``dp[i]``값은 이미 구해진 dp 값을 참조 비교하여 선정된다.

### Flow



### Code

```c++
#include <iostream>

using namespace std;

int values[10001]; //Values
int dp[10001]; //Memoization

int main(){
	int sizeOfValues = 0;
    
    cin >> sizeOfValues ;
    for(int i = 0; i < sizeOfValues; i++){
        cin >> values[i];
    }

	//design enter each value (ready for subproblem)
    for(int i = 0; i < sizeOfValues; i++){
        //subproblems
    }
    
    //find optimal Value
    int optIdx = 0;
    for(int i = 1; i < sizeOfValues; i++){
        optIdx = dp[i] < dp[optIdx] ? optIdx : i;
    }
    cout << dp[optIdx] << endl;
}
```



## [예제](https://www.acmicpc.net/problem/9663)

> * Longest Increasing Sequence문제

### 개념

* **Values**
  * 임의 수로 나열된 값

* **Memoization**
  * ``dp[i]``: i번 째 value가 최종 높이 일 때, 계단의 수
* **Subproblem**
  * $$dp[i] = \max_{\substack{j < i,\\ val[j] < val[i]}} dp[j] + 1$$

```c++
#include <iostream>

using namespace std;

int values[10001]; //Values
int dp[10001]; //Memoization

int main(){
	int sizeOfValues = 0;
    
    cin >> sizeOfValues ;
	for(int i = 0; i < sizeOfValues; i++){
		cin >> values[i];
	}


	//design enter each value (ready for subproblem)
    for(int i = 0; i < sizeOfValues; i++){
        int opt = 0;
		for(int j = i-1; j >= 0; j--){
			if(values[i] > values[j]){
                opt = max(opt,dp[j]);
			}
		}
		dp[i] = opt+1;     
    }
    
    //find optimal Value
    int optVal = 0;
    for(int i = 0; i < sizeOfValues; i++){
        optVal = max(optVal,dp[i]);
    }
    cout << optVal << endl;
}
```

