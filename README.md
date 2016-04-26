#include "setup.h"
#include "setup.cpp"

char hand[13];

void Deal (stack<myvect>& deck){
  for (int i = 0; i < 13; i++){
    hand[i] = deck.top();
    deck.pop();
  }
}
