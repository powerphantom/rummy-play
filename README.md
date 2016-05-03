#include "setup.h"

void Deal(stack<setup>& deck, vector<setup>& hand){ 
	for (int i = 0; i < 13; i++){
    	hand.push_back(deck.top());
    	deck.pop();
  }
}

void Com_Draw(stack<setup>& deck, vector<setup>& com_hand){
	com_hand.push_back(deck.top());
	deck.pop();
}

void Com_Discard(vector<setup>& discard, vector<setup>& com_hand){
	discard.push_back(com_hand[0]);
	com_hand.erase(com_hand.begin());
}

void Com_Sequence(vector<setup>& stock, vector<setup>& com_hand){
	int i=0;
	int j=0;
	int vcount=0;
	int scount=0;
	int hold=0;
	vector<setup> temp;

	for(i=0; i<com_hand.size(); i++){
		if(com_hand[i].val>com_hand[i+1].val){
			std::swap(com_hand[i], com_hand[i+1]);
			i=0;
			i--;
		}
	}

	for(i=0; i<com_hand.size(); i++){
		for(j=0; j<com_hand.size(); j++){
			if(com_hand[i].val==com_hand[j].val){
				temp.push_back(com_hand[j]);
				vcount++;}
		}
		if (temp.size()<=2){
			temp.erase(temp.begin(),temp.end());
			vcount=0;}
		else
			break;
	}
	

	
	if(vcount>2){
		for(i=0; i<temp.size(); i++){
			for(j=0; j<com_hand.size(); j++){
				if((temp[i].suit==com_hand[j].suit)&&(temp[i].val==com_hand[j].val)){
			com_hand.erase(com_hand.begin()+j);}}}
		for(i=0; i<temp.size(); i++){
		stock.push_back(temp[i]);}
	}

}

void Draw_Card (stack<setup>& deck,vector<setup>& hand,vector<setup>& discard){
  int p=0;
  int i=0;
  char resp; //user input
  setup newCard; //used for drawn card
  cout << "Do you wish to draw from the deck?" << endl;
  cin >> resp;
  if (resp == 'Y'||'y'){ //puts new card in hand, removes from stack
	newCard = deck.top(); 
	hand.push_back(newCard); //Did we get this working?
	deck.pop();
  }
  else if (resp == 'N'||'n'){
    cout << "Draw from discard pile?" << endl; //only draws first card, can modify
	cin >> resp;
    if (resp == 'Y'||'y'){
      for(i=0; i!= discard.size(); i++){
      cout << "In postion " << i << " " << discard[i].suit << discard[i].val << endl;
      }
      
      cout << "Please enter the position of the card you want: " << endl;
      cin>>p;
      
      for(p; p!=discard.size(); p++){
      hand.push_back(discard[p]);
      }
    }
    else
      cout << "Invalid Respose" << endl;
  }
  else
    cout << "Invalid Response" << endl;
}

void Discard(vector<setup>& hand, vector<setup>& discard){
	int pos;

for (unsigned int i = 0; i < hand.size(); i++) {
	cout << i << ": " << hand[i].suit << hand[i].val << ",  ";
}

	cout << "\n" << "Which card would you like to discard: " << endl;
	cin >> pos;

	discard.push_back(hand[pos]); //should add card from hand to last position of discard pile
	hand.erase(hand.begin() + pos); //should delete card in hand[pos] and decrease size of hand
}

void Ask_squence(vector<setup>& hand, vector<setup>& stock){
	char source;
	cout << "Is you sequence from the stock or your hand: (S/H/N)"<<endl;
	cin>>source;
	if (source=='S'){
		Sequence_Stock(hand, stock);
	}
	else if (source=='H'){
		Sequence_Hand(hand, stock);}
	else{
	cout<<"Player 2's turn"<<endl;}
}

void Sequence_Hand(vector<setup>& hand, vector<setup>& stock){
	int cards=0;
	int vcount=0;
	int scount=0;
	int i=0;
	int j=0;
	vector<setup> temp_stock;

	cout << "Please enter how many cards you want to use:"<< endl;
	cin>>cards;

	for(i=0; i<hand.size(); i++){
		cout << "In position "<< i << " you have the " << hand[i].val<<" of " << hand[i].suit<<endl;
}

	cout << "Please enter the postions of the cards you want to use IN ORDER:" << endl;

	int p=0;
	for(i=0; i<cards; i++){
		cin>>p;
		temp_stock.push_back(hand[p]);}

	for (i=0; i<cards; i++){

		if(temp_stock[i].val==temp_stock[i-1].val&&temp_stock[i].val==temp_stock[i+1].val){
			vcount++;}

		if(temp_stock[i].suit==temp_stock[i-1].suit&&temp_stock[i].suit==temp_stock[i+1].suit){
			scount++;}
	}


	if (vcount==cards-2){
		for(i=0; i<temp_stock.size(); i++){
			for(j=0; j<hand.size(); j++){
				if((temp_stock[i].suit==hand[j].suit)&&(temp_stock[i].val==hand[j].val)){
			hand.erase(hand.begin()+j);}}}
		for(i=0; i<cards; i++){
		stock.push_back(temp_stock[i]);
		}
	}

	else if(scount==cards-2){
	int hold;
		scount=0;
		for(i=0; i<cards-1; i++){
			if((hold+1)==temp_stock[i+1].val){
			scount++;}
			else{
			break;}}

	if(scount==cards-1){
		for(i=0; i<temp_stock.size(); i++){
			for(j=0; j<hand.size(); j++){
				if((temp_stock[i].suit==hand[j].suit)&&(temp_stock[i].val==hand[j].val)){
			hand.erase(hand.begin()+j);}}}
		for(i=0; i<cards; i++){
		stock.push_back(temp_stock[i]);}
	}
}
	else{
		cout<<"Invalid Squence"<<endl;}


}


void Sequence_Stock(vector<setup>& hand, vector<setup>& stock){
	int i=0;
	int j=0;
	int ipos=0;
	int fpos=0;
	int vcount=0;
	int scount=0;
	int cards=0;
	vector<setup> temp;

	for(i=0; i!=stock.size(); i++){
		cout << i << stock[i].suit << stock[i].val<<endl;
	}

	cout<<"Please enter the starting position of the squence you want to add to:"<<endl;
	cin>>ipos;

		cout<<"Please enter the final position of the squence tou want to add to:"<<endl;
	cin>>fpos;
	i=ipos;

	for(ipos; ipos<=fpos; ipos++){
		temp.push_back(stock[ipos]);
	}

	cout<<"Please enter the number of cards you want to use: "<<endl;
	cin>>cards;

	cout<<"In your hand: "<<endl;

		for(i=0; i<hand.size(); i++){
		cout << "In position "<< i << " you have the " << hand[i].val<<" of " << hand[i].suit<<endl;
}

	cout<< "Please enter the position of the cards you want to use IN ORDER: "<<endl;
	int p=0;
	for(i=0; i<cards; i++){
		cin>>p;
		temp.push_back(hand[p]);
}


	for (i=0; i<temp.size(); i++){

		if(temp[i].val==temp[i-1].val&&temp[i].val==temp[i+1].val){
			vcount++;}

		if(temp[i].suit==temp[i-1].suit&&temp[i].suit==temp[i+1].suit){
			scount++;}
	}


	if (vcount==temp.size()){
		for(i=0; i<temp.size(); i++){
			for(j=0; j<stock.size(); j++){
				if((temp[i].suit==stock[j].suit)&&(temp[i].val==stock[j].val)){
			stock.erase(stock.begin()+j);}}}
		for(i=0; i<temp.size(); i++){
		stock.push_back(temp[i]);
		}
	}
	else if(scount==temp.size()){
		for(i=0; i<temp.size(); i++){
		if(temp[i].val>temp[i+1].val){
			std::swap(temp[i], temp[i+1]);
			}
		}
		scount=0;
		int hold;
		for(i=0; i<temp.size()-1; i++){
			hold=temp[i].val;
			if(temp[i+1].val == (hold+1)){
			scount++;}
			else{
			break;}}

	if(scount==temp.size()-1){
		for(i=0; i<temp.size(); i++){
			for(j=0; j<stock.size(); j++){
				if((temp[i].suit==stock[j].suit)&&(temp[i].val==stock[j].val)){
			stock.erase(stock.begin()+j);}}}
		for(i=0; i<temp.size(); i++){
		stock.push_back(temp[i]);}
	}
}
	else{
		cout<<"Invalid Squence"<<endl;}

}
