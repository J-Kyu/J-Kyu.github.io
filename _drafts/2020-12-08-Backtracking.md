---
layout: post
title: Backtracking Algorithm
tags: [Algorithm, Backtracking,CSP,Constraint Satisfaction Problem]
---
# Backtracking Algorithm

> * [정의](##정의)
> * [개념](##개념)
> * [예제](##예제)

## 정의

> Tree에서 상태 공간을 방문하며 expanding하며 Search 할 때(CSP), 조건에 맞지 않은 경우, Backtracking을 통해 Computation 중복 방지하며 searching하는데 도움을 준다.
>
> CSP(Constraint Satisfactory Problem)에 사용되기 좋다.

* ``모든 경우 수를 전부 고려하는 알고리즘으로, 상태 공간을  트리로 나타낼 수 있는 적합한 방식이다``라고 [나무위키](https://namu.wiki/w/%EB%B0%B1%ED%8A%B8%EB%9E%98%ED%82%B9)에 나와있다. 
* 핵심단어는 다음고 같다.
  * **모든 경우 수**
  * **트리**
* 위의 두 단어를 통해서 ``Backtracking Algorithm``은 **모든 경우의 수**를 **트리**로 접근할 수 있다는 것을 알 수있다. (BFS or DFS 처럼 )
* 하지만, BFS와 DFS는 Brute-Force 방식이다.  Tree의 크기가 엄청난 경우,  **모든 경우의 수**에 대하여 조사하는 것은 엄청난 낭비가 될 수 있다. 
* **이런 경우 우리는 Backtracking 방법을 통하여 중복되는 계산을 막아 효과적인 방법으로 solution을 찾을 수 있다.**
  * 이러한 문제를 ``CSP``(Constraint Satisfaction Problem)라고 하며, Backtracking을 통해서 중복을 줄일 수 있다.
  * 여기서 말하는 ``CSP``는  조건이 존재하는 Search 문제라고 생각하자.

## 개념

### Description

* ``CSP``는 다음과 같이 표현 될 수 있다.
  * Variable
  * Domain
  * Constraint
* 문제로 부터 **Variable$$_{\text{변수}}$$**와 변수가 갖을 수 있는 **Domain$$_{\text{정의역}}$$**을 정의한다. 이때, 변수간의 관계를 **Constraint$$_{\text{존건}}$$**으로 정의 할 수 있다.
* 이렇게 3가지에 대하여 정의가 된 경우, ``CSP``를 풀기 위한 준비는 완료된다.

### Flow



### Code

```c++
#include <iostream>
#include <cmath>


using namespace std;

bool ConstraintCheck(int* column, int index){
	/*
	if the variable satisfy Constraints -> return True
	else return False
	*/
	return true;
}


void Search(int* variables, int index, int N){
    //if all the variables have values, we backtrack to previous variables
	if(index >= N){
		return ;
	}
	//set Value for variable....lets say 0~N is domain for variable
	for (int i=0; i < N; i++){
		variables[index] = i;

		if(ConstraintCheck(variable,index)){
			//Go to next variable(index+1), if statisfies....
			result += SetColumnValue(column,index+1,N);
		}
        //else..change the value
	}
    //return for backtracking
	return ;
}


int main(){
	int N = 0;

	cin >> N;
	int * variables = new int[N];
	cout << Search(variables,0,N) << endl;
}
```



## [예제](https://www.acmicpc.net/problem/9663)

> * N-Queen문제

### 개념

* **Variable**
  * Each Column
* **Domain**
  * 0~N (Row Position)
* **Constraint**
  * Queens cannot attack Each other

```c++
#include <iostream>
#include <cmath>


using namespace std;


bool CheckMakeSense(int* column, int index){
	
	for(int i = 0; i < index; i++){
        //check constraint
		if(column[i] == column[index] || abs(i-index) == abs(column[i]-column[index])) {
			return false;	
		}
	}
	return true;
}


int SetColumnValue(int* column, int index, int N){

    int result = 0;	//Number of position that queens can locate on chess board

	if(index >= N){
		return  1; // Satisfy
	}

	//set Value for each column
	for (int i=0; i < N; i++){
		column[index] = i;
		if(CheckMakeSense(column,index)){
			//Go to next column, if statisfies....
			result += SetColumnValue(column,index+1,N);
		}
	}
	return result;
}


int main(){


	int N = 0; //Length of chess board and Number of Queens
	
	cin >> N;
	int * column = new int[N];
	cout << SetColumnValue(column,0,N) << endl;



}
```

