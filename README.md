#include "setup.h"
#include "setup.cpp"

char hand[13];

void Deal (stack<myvect>& deck, char hand){
  for (int i = 0; i < 13; i++){
    hand[i] = deck.top();
    deck.pop();
  }
}

void Draw_Card (deck, hand, discard){
  char resp; //user input
  card newCard; //used for drawn card
  cout << "Do you wish to draw from the deck?" << endl;
  cin >> resp;
  if (resp == Y||y){ //puts new card in hand, removes from stack
    newCard = deck.top(); 
    hand.push_back(newCard); //Did we get this working?
    deck.pop();
  }
  else if (resp == N||n){
    cout << "Draw from discard pile?" << endl; //only draws first card, can modify
    cin >> resp;
    if (resp == Y||y){
      newCard = discard.top();
      hand.push_back(newCard);
      discard.pop();
    }
    else
      cout << "Invalid Respose" << endl;
  }
  else
    cout << "Invalid Response" << ednl;
}

void Discard(hand, discard){  //Kyle Probert
  //Show the player the cards they have in their hand and what position they're in. Ask the player what is the position of the card they want to get rid of. Use cin to get the positon hand[i].suit hand[i].value to tell their card push_back to put card in the discard pile and delet that card from their hand and decrease the size of their hand.
}

void Sqeunce_Checker(stock/*The squneces already made*/, hand){
  //Use cin to ask how many cards they want to sequnce. Then ask for the postions of the cards, then use if else statements to chexck if the cards in in a sequnce. Also show them the stack and ask if they wnat to put a card there.
}
