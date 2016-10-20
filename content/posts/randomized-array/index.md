+++
date = "2016-10-20T18:26:57+09:00"
title = "랜덤 뽑기"
subtitle = "배열에서 임의의 요소를 중복되지 않도록 뽑는 방법"
tags = ["Algorithm"]
+++

# 임의의 접근
'임의(랜덤)'라는 말을 들으면 `rand()`를 이용해 무작위로 접근하는 방법이 가장 먼저 떠오를 것이다. 그러나 당연하게도 무작위 접근은 중복 접근을 방지하지 못 한다.  
그렇다면, 중복을 피해서  

``` c++
class ShuffledIndices{
private:
    std::vector<int> indices;
    int range;
public:
    ShuffledIndices(int size): indices(size), range(size) {
        for(int i=0; i<size; ++i){
            indices[i] = i;
        }
    }

    int getIndex() {
        int randomIndex = rand()%range; // [0, range)

        // Data swap.
        int temp = indices[randomIndex];
        indices[randomIndex] = indices[range - 1];
        indices[range - 1] = temp;

        // Reduce picking range.
        if(--range < 1){
            range = indices.size();
        }

        return temp;
    }
};
```
이는 다음과 같이 이용하면 된다.
``` c++
// arr는 std::vector를 이용한 임의의 배열.

ShuffledIndices si(arr.size());
for(int i=0; i<arr.size(); ++i){
    arr[si.getIndex()];
    ...
}
```

그렇다면, 인덱스를 섞어서 뽑는 방법 대신, 직접 원본 배열을 섞어서 뽑는 방법은 어떨까?  
원본 배열이 변형되어도 되고, 배열에 저장된 데이터가 레지스트리를 이용하는 기본 자료형(`int`, `char`, ...)이면 전혀 문제될 것이 없지만, 객체와 같이 데이터 스왑에 필요한 비용이 큰 자료형의 경우엔 성능에 큰 영향을 끼칠 것이다. 그러므로, 직접 원본 배열을 이용하는 것 보다는 위와 같이 따로 섞인 인덱스를 이용하는 편이 낫다고 할 수 있겠다.  
물론, 섞인 인덱스가 저장된 배열이 차지하는 메모리도 무시할 수 없지만, 일반적으로는 기본 자료형으로 인한 공간 복잡도보다 객체 복사로 인한 시간 복잡도가 더 큰 문제가 될 것이다.

# 배열 섞기
목적이 배열을 섞는 것이라면 위의 방법를 이용하는 것도 좋다.
``` c++
template <typename type_t>
void shuffle(std::vector<type_t> &arr) {
    int range = arr.size();

    for(int i=0; i<arr.size(); ++i){
        int randomIndex = rand()%range; // [0, range)

        // Data swap.
        type_t temp = arr[randomIndex];
        arr[randomIndex] = arr[range - 1];
        arr[range - 1] = temp;

        --range; // Reduce picking range.
    }
}
```
달라진 점은 원본 배열을 직접 이용한다는 것과 여러 자료형에 적용할 수 있도록 `template`을 사용하는 것 뿐이다.