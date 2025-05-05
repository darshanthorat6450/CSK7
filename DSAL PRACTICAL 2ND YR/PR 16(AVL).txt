#include <iostream>
#include <string.h>

using namespace std;

// Structure for each node in the AVL tree
struct node {
    char word[20];
    char mean[50];
    node *left, *right;
    int ht;
};

// Class for dictionary operations using AVL Tree
class dict {
public:
    node* create(node *root);
    node* insert(node *root, char word[], char mean[]);
    void display(node *);
    int height(node *);
    int BF(node *);
    node* deletion(node *, char *);
    node* RR(node *);
    node* LL(node *);
    node* RL(node *);
    node* LR(node *);
};

// Function to create a dictionary with multiple entries
node* dict::create(node *root) {
    int n;
    char w[20], m[50];

    cout << "\n Enter total number of words: ";
    cin >> n;

    for (int i = 0; i < n; i++) {
        cout << "\n Enter word " << i + 1 << ": ";
        cin >> w;
        cout << " Enter meaning: ";
        cin >> m;
        root = insert(root, w, m);
    }

    return root;
}

// Function to insert a word and its meaning into the AVL tree
node* dict::insert(node *root, char w[], char m[]) {
    if (root == NULL) {
        root = new node;
        strcpy(root->word, w);
        strcpy(root->mean, m);
        root->left = root->right = NULL;
        root->ht = 1;
        return root;
    }

    if (strcmp(w, root->word) > 0) {
        root->right = insert(root->right, w, m);
        if (BF(root) == -2) {
            if (strcmp(w, root->right->word) > 0) {
                root = RR(root);
            } else {
                root = RL(root);
            }
        }
    } else {
        root->left = insert(root->left, w, m);
        if (BF(root) == 2) {
            if (strcmp(w, root->left->word) < 0) {
                root = LL(root);
            } else {
                root = LR(root);
            }
        }
    }

    root->ht = height(root);
    return root;
}

// In-order traversal display of AVL tree
void dict::display(node* root) {
    if (root != NULL) {
        display(root->left);
        cout << "\n " << root->word << " - " << root->mean;
        display(root->right);
    }
}

// Function to calculate the height of a node
int dict::height(node *root) {
    if (root == NULL)
        return 0;

    int lh = (root->left) ? root->left->ht : 0;
    int rh = (root->right) ? root->right->ht : 0;

    return (lh > rh ? lh : rh) + 1;
}

// Function to calculate Balance Factor of a node
int dict::BF(node *root) {
    if (root == NULL)
        return 0;

    int lh = (root->left) ? root->left->ht : 0;
    int rh = (root->right) ? root->right->ht : 0;

    return lh - rh;
}

// Left-Left Rotation
node* dict::LL(node *T) {
    node *y = T->left;
    T->left = y->right;
    y->right = T;

    T->ht = height(T);
    y->ht = height(y);

    return y;
}

// Right-Right Rotation
node* dict::RR(node *T) {
    node *y = T->right;
    T->right = y->left;
    y->left = T;

    T->ht = height(T);
    y->ht = height(y);

    return y;
}

// Left-Right Rotation
node* dict::LR(node *T) {
    T->left = RR(T->left);
    return LL(T);
}

// Right-Left Rotation
node* dict::RL(node *T) {
    T->right = LL(T->right);
    return RR(T);
}

// Function to delete a word from the AVL tree
node* dict::deletion(node *T, char *w) {
    if (T == NULL) {
        cout << "\n Word not found!";
        return NULL;
    }

    if (strcmp(w, T->word) > 0) {
        T->right = deletion(T->right, w);
        if (BF(T) == 2) {
            if (BF(T->left) >= 0)
                T = LL(T);
            else
                T = LR(T);
        }
    } else if (strcmp(w, T->word) < 0) {
        T->left = deletion(T->left, w);
        if (BF(T) == -2) {
            if (BF(T->right) <= 0)
                T = RR(T);
            else
                T = RL(T);
        }
    } else {
        // Node with only left child or no child
        if (T->right == NULL) {
            node *temp = T->left;
            delete T;
            return temp;
        } else {
            // Node with two children, get inorder successor
            node *p = T->right;
            while (p->left != NULL)
                p = p->left;

            strcpy(T->word, p->word);
            strcpy(T->mean, p->mean);
            T->right = deletion(T->right, p->word);

            if (BF(T) == 2) {
                if (BF(T->left) >= 0)
                    T = LL(T);
                else
                    T = LR(T);
            }
        }
    }

    T->ht = height(T);
    return T;
}

// Main function to drive the menu
int main() {
    int ch;
    dict d;
    node *root = NULL;
    char w[20], m[50];

    cout << "\n *** Dictionary: AVL Tree Implementation ***";

    do {
        cout << "\n\n MENU:";
        cout << "\n1. Create \n2. Insert \n3. Delete \n4. Display \n5. Exit";
        cout << "\n Enter your choice: ";
        cin >> ch;

        switch (ch) {
            case 1:
                root = d.create(root);
                break;
            case 2:
                cout << "\n Enter word: ";
                cin >> w;
                cout << " Enter meaning: ";
                cin >> m;
                root = d.insert(root, w, m);
                break;
            case 3:
                cout << "\n Enter word to delete: ";
                cin >> w;
                root = d.deletion(root, w);
                break;
            case 4:
                d.display(root);
                break;
            case 5:
                break;
            default:
                cout << "\n Invalid choice!";
        }

    } while (ch != 5);

    return 0;
}
