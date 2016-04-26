#include "setup.h"

char hand[13];

void Deal (){
  for (int i = 0; i < 13; i++){
    hand[i] = myvect.top();
    myvect.pop();
  }
}
