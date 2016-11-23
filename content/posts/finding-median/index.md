+++
date = "2016-11-12T01:47:05+09:00"
title = "중앙값(Median) 찾기"
subtitle = "Heap을 이용한 중앙값 찾기"
tags = ["Algorithm"]
+++

# 중앙값(Median)
중앙값이란, 어떤 배열을 정렬했을 때 정 가운데에 위치하는 값을 말한다.  
예를들어, `[1, 2, 3]`의 배열에서는 `2`가 중앙값이다.

원소의 개수가 짝수인 경우엔, 가운데 두 원소의 평균이 중앙값이 된다.  
`[1, 2, 3, 4]`에서는 `2`와 `3`의 평균인 `2.5`가 중앙값인 것이다. 

## 중앙값 찾기
중앙값은 **정렬된 배열**에서 찾아야 한다.  
배열이 정렬되어 있다면 땡큐지만, 정렬되어 있지 않다면 정렬해야 하므로, 가장 먼저 정렬을 수행해야 한다.

정렬 후엔, 단순히 배열 크기의 절반에 해당하는 인덱스에 접근해 반환하면 된다.
``` c++
float median(vector<int> &arr){
    sort(arr.begin(), arr.end());

    if(arr.size() % 2 == 1){
        return (float)arr[arr.size() / 2];
    }
    else{
        return ((float)arr[arr.size() / 2] + (float)arr[arr.size() / 2 + 1]) / 2.0f; 
    }
}
```
int형 자료를 갖는 배열임에도 float형을 반환하는 이유는, 짝수개에 대한 중앙값의 정의 때문이다. 물론 소수점 이하가 필요 없다면 그대로 반환해도 되겠다. 반올림이 아닌 절삭인 것도 감안하고.

## 실시간으로 중앙값 찾기
정렬된 배열에서 중앙값을 찾아야 하기 때문에 정렬이 선행되어야 함을 알았다. 그렇다면, 데이터가 지속적으로 추가/삭제되고 있는 상황에서 중앙값을 찾으려면 어떻게 해야 할까? 위에서 구현한 방법을 사용한다면, 중앙값을 찾을 때마다 배열 전체에 대한 정렬을 시도하므로 엄청난 자원 낭비가 아닐 수 없다.

그러므로 값을 찾을 때마다 정렬하기 보다는, 자료구조 자체를 바꿔야 하겠다. 수행 속도가 훨씬 빠른,추가/삭제가 정렬되어 이루어지는 자료 구조를 이용하는 것이다. 추가/삭제가 정렬되어 이루어지는 자료 구조는 여러가지가 있지만, 중앙값을 찾는 데에는 Heap을 이용할 것이다.

Heap의 성질인 최대 또는 최소값에 접근이 용이함을 이용해, 중앙값보다 작은 값들은 Max heap에, 중앙값보다 큰 값들은 Min heap에 저장해 손쉽게 중앙값을 찾을 수 있다.

int main(){
    int n;
    cin >> n;
    vector<int> a(n);
    
    priority_queue<int> smaller;
    priority_queue<int, vector<int>, greater<int> > bigger;
    
    cin >> a[0];
    smaller.push(a[0]);
    printf("%.1lf\n", (double)a[0]);

    for(int a_i = 1;a_i < n;a_i++){
       cin >> a[a_i];
        
        if(a[a_i] < smaller.top()){
            smaller.push(a[a_i]);
        }
        else{
            bigger.push(a[a_i]);
        }
        
        int diff = bigger.size() - smaller.size();
        if(diff > 0){
            smaller.push(bigger.top());
            bigger.pop();
        }
        else if(diff < -1){
            bigger.push(smaller.top());
            smaller.pop();
        }
        
        if(smaller.size() == bigger.size()){
            double m = 0.0;
            m += smaller.top();
            m += bigger.top();
            printf("%.1lf\n", m * 0.5);
        }
        else{
            printf("%.1lf\n", static_cast<double>(smaller.top()));
        }
    }
    return 0;
}

## 세 수(Triple)의 중앙값 찾기
퀵정렬을 설계할 때 Pivot을 선택하는 방법에는 여러가지가 있지만, 대표적으로는 임의의 한 원소를 선택하는 방법과 **처음, 중간, 끝의 원소 중 중앙값인 원소를 선택하는 방법**의 두 가지가 있다.  

가장 쉽게 생각할 수 있는 방법은, 다량의 조건문을 이용해 찾는 것이다.
``` c++
int median(int a, int b, int c){
    if (a > b){
        if (b > c)          return b;
        else if (a > c)     return c;
        else                return a;
    }
    else{
        if (a > c)          return a;
        else if (b > c)     return c;
        else                return b;
    }
}
```
조건부 연산자(`exp?exp:exp`)를 이용하면 코드가 좀 더 간단해지겠지만, 해석하기가 어려워 진다.

그러나, 교환 법칙이 성립되는 XOR의 성질을 이용해 분기 없이 중앙값을 찾을 수도 있다.
``` c++
int median(int a, int b, int c){
    int maximum = max(max(a, b), c);
    int minimum = min(min(a, b), c);
    return a ^ b ^ c ^ maximium ^ minimum;
}
```
세 수 모두를 XOR로 결합하고, 다시 최대 최소를 XOR로 제외함으로써 중앙값을 구하고 있다.