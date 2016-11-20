+++
date = "2016-11-12T02:29:34+09:00"
title = "리스트의 순환 여부 검사하기"
subtitle = "토끼와 거북이 알고리즘"
tags = ["Algorithm"]
+++

struct Node {
    int data;
    Node *next;
}

bool has_cycle(Node* head) {
    if(head == nullptr){
        return false;
    }
    
    Node *turtle = head;
    Node *rabbit = head->next;
    while(rabbit != turtle){
        if(rabbit == nullptr || rabbit->next == nullptr){
            return false;
        }
        
        turtle = turtle->next;
        rabbit = rabbit->next->next;
    }
    
    return true;
}