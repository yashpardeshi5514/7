#include <iostream>
#include <string>

using namespace std;

class Node {
public:
    int prn;
    string name;
    Node* link;
    Node(int pr, string nm) : prn(pr), name(nm), link(nullptr) {}
};

class LinkedList {
    Node* start;

public:
    LinkedList() : start(nullptr) {}

    void insertPresident() {
        int pr;
        string nm;
        cout << "Enter PRN and Name:\n";
        cin >> pr >> nm;
        Node* tmp = new Node(pr, nm);
        tmp->link = start;
        start = tmp;
        display();
    }

    void insertSecretary() {
        int pr;
        string nm;
        cout << "Enter PRN and Name:\n";
        cin >> pr >> nm;
        Node* tmp = new Node(pr, nm);
        if (!start) {
            start = tmp;
        } else {
            Node* q = start;
            while (q->link) q = q->link;
            q->link = tmp;
        }
        display();
    }

    void insertMember() {
        int pr, index;
        string nm;
        cout << "Enter PRN and Name:\n";
        cin >> pr >> nm;
        Node* tmp = new Node(pr, nm);
        if (!start) {
            start = tmp;
        } else {
            cout << "\nEnter index after which element to be inserted:\n";
            cin >> index;
            Node* q = start;
            for (int i = 0; i < index; i++) {
                if (!q) {
                    cout << "There are fewer elements\n";
                    delete tmp;
                    return;
                }
                q = q->link;
            }
            tmp->link = q->link;
            q->link = tmp;
        }
        display();
    }

    void deletePresident() {
        if (!start) {
            cout << "List is empty!\n";
            return;
        }
        Node* tmp = start;
        start = start->link;
        delete tmp;
        display();
    }

    void deleteSecretary() {
        if (!start) {
            cout << "List is empty!\n";
            return;
        }
        Node* q = start;
        while (q->link && q->link->link) q = q->link;
        delete q->link;
        q->link = nullptr;
        display();
    }

    void deleteMember() {
        int pr;
        cout << "Enter PRN to be deleted: ";
        cin >> pr;
        Node* q = start, * tmp = start;
        if (start->prn == pr) {
            deletePresident();
            return;
        }
        while (tmp && tmp->prn != pr) {
            q = tmp;
            tmp = tmp->link;
        }
        if (tmp) {
            q->link = tmp->link;
            delete tmp;
        } else {
            cout << "Member with PRN " << pr << " not found.\n";
        }
        display();
    }

    void display() const {
        if (!start) {
            cout << "Club is empty!!\n";
            return;
        }
        cout << "**** Members in Club ****\n";
        Node* q = start;
        while (q) {
            cout << q->prn << "  " << q->name << endl;
            q = q->link;
        }
    }

    void count() const {
        int i = 0;
        Node* q = start;
        while (q) {
            i++;
            q = q->link;
        }
        cout << "Total no. of members in club: " << i << endl;
    }

    void concat(LinkedList& l2) {
        if (!start) {
            start = l2.start;
        } else {
            Node* ptr = start;
            while (ptr->link) ptr = ptr->link;
            ptr->link = l2.start;
        }
        l2.start = nullptr;
        display();
    }
};

int main() {
    LinkedList l1, l2;
    int ch;

    cout << "\n**** Linked List *****\n";
    cout << "1. Insert President\n2. Insert Secretary\n3. Insert Member\n4. Delete President\n";
    cout << "5. Delete Secretary\n6. Delete Member\n7. Display\n8. Count\n9. Reverse\n";
    cout << "10. Concat\n11. Exit\n";

    while (true) {
        cout << "\nEnter Your Choice: ";
        cin >> ch;
        switch (ch) {
            case 1: l1.insertPresident(); break;
            case 2: l1.insertSecretary(); break;
            case 3: l1.insertMember(); break;
            case 4: l1.deletePresident(); break;
            case 5: l1.deleteSecretary(); break;
            case 6: l1.deleteMember(); break;
            case 7: l1.display(); break;
            case 8: l1.count(); break;
            case 9: l1.reverse(); break;
            case 10:
                l2.insertPresident();
                l2.insertMember();
                l2.insertSecretary();
                l1.concat(l2);
                break;
            case 11: exit(0);
            default: cout << "\nWrong Choice!\n";
        }
    }
    return 0;
}
