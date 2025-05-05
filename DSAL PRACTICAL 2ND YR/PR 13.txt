#include <iostream>
#include <string.h>
#define max 10
using namespace std;

struct node {
    char name[15];
    long int mobno;
    int chain;

    node() {
        strcpy(name, "-");
        mobno = 0;
        chain = -1;
    }
};

class hasht {
    node ht[max];
public:
    int hashfun(long int);
    void insert();
    void display();
    void search();
    void del();
    
};

int hasht::hashfun(long int num) {
    return (num % max);
}

void hasht::insert() {
    int ind, prev;
    node S;
    cout << "Enter name and mobile number of a person:" << endl;
    cin >> S.name >> S.mobno;

    ind = hashfun(S.mobno);
    if (ht[ind].mobno == 0) {
        ht[ind] = S;
    } else {
        prev = ind;
        while (ht[ind].mobno != 0) {
            ind = (ind + 1) % max;  
        }
        ht[ind] = S;
        ht[prev].chain = ind;    
    }
}

void hasht::display() {
    cout << "Index\tName\t\tMobile Number\tChain" << endl;
    for (int i = 0; i < max; i++) {
        cout << i << "\t" << ht[i].name << "\t\t" << ht[i].mobno << "\t\t" << ht[i].chain << endl;
    }
}

void hasht::search(){
    long int num;
    int ind;
    cout<<"enter the no. which you want to be search:";
    cin>>num;
    ind=hashfun(num);
    while(ind != -1){
        if(num==ht[ind].mobno){
            cout<<"mobile no. is present at index: "<<num<<endl;
            return ;
        }
        ind=ht[ind].chain;
    }
}

void hasht::del(){
    long int num;
    int ind,prev;
    cout<<"which no. you want to delete :";
    cin>>num;
    ind=hashfun(num);

    while(ind != -1){
        if(num==ht[ind].mobno){
            if(ht[ind].chain==-1)
            {
                strcpy(ht[ind].name,"-");
                       ht[ind].mobno=0;
                cout<<"record is deleted";
            }
            else{
                int next=ht[ind].chain;
                ht[ind]=ht[next];
                strcpy(ht[next].name,"-");
                ht[next].mobno=0;
                ht[next].chain= -1;
            }


            {
                if(prev != -1)
                   ht[prev].chain= -1;
            }
            
        }
        prev=ind;
        ind=ht[ind].chain;
    }
}


int main(){
    int cho;
    hasht h;
    char a,y,Y;
    do{
        cout<<"1.insert \n2.display\n3.serach\n4.deleted\n5.exit\nenter the choice: ";
        cin>>cho;
        switch(cho){
            case 1:
              h.insert();
              break;
            case 2:
               h.display();
               break;

            case 3:
               h.search();
               break;

            case 4:
               h.del();
               break;


            case 5:
              cout<<"exit"<<endl;
              break;

            default :
               cout<<"invalid  choice"<<endl;
                break;
        }
        cout<<"you will display other case 1.yes , 2.no =";
        cin>>a;

    }while(a ='y');
     return 0 ;
}
