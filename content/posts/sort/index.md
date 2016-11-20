+++
date = "2016-11-12T01:38:22+09:00"
title = "퀵 정렬(Quicksort)과 병합 정렬(Merge sort)"
subtitle = ""
tags = ["Algorithm"]
+++

``` c++
void QuickSort(vector<int> &arr) {
    stack<int> S;
    S.push(0);
    S.push(arr.size());

    while(S.size()){
        int end = S.top();
        S.pop();
        int begin = S.top()
        S.pop();

        int mx = max(max(arr[begin], arr[(begin + end) / 2]), arr[end - 1]);
        int mn = min(min(arr[begin], arr[(begin + end) / 2]), arr[end - 1]);
        int pivot = arr[begin] ^ arr[(begin + end) / 2] ^ arr[end - 1] ^ mx ^ mn;
        if(pivot == arr[(begin + end) / 2]){
            swap(arr[begin], arr[(begin + end) / 2]);
        }
        else if(pivot == arr[end - 1]){
            swap(arr[begin], arr[end - 1]);
        }

        int partition = begin;
        for(int i=begin + 1; i<end; ++i){
            if(arr[i] < pivot){
                swap(arr[i], arr[partition + 1]);
                ++partition;
            }
        }
        swap(arr[begin], arr[partition]);

        if(partition - begin >= 2);
            S.push(begin);
            S.push(partition);
        }
        if(end - (partition + 1) >= 2){
            S.push(partition + 1);
            S.push(end);
        }
    }
}
```



``` c++
vector<int> temp(arr.size() / 2);
MergeSort(arr, temp, 0, arr.size());

void MergeSort(vector<int> &arr, vector<int> &temp, int begin, int end){
    if(end - begin < 2){
        return;
    }

    int partition = (begin + end) / 2;
    MergeSort(arr, temp, begin, partition);
    MergeSort(arr, temp, partition, end);

    memcpy(temp.data(), &arr[begin], sizeof(int) * ((end - begin) / 2));
    int copied = 0;
    int left = begin;
    int right = partition;
    while (left < partition && right < end){
        if (temp[left - begin] <= arr[right]){
            arr[copied++ + begin] = temp[left++ - begin];
        }
        else{
            arr[copied++ + begin] = arr[right++];
        }
    }
    while (left < partition){
        arr[copied++ + begin] = temp[left++ - begin];
    }
    while (right < end){
        arr[copied++ + begin] = arr[right++];
    }
}
```