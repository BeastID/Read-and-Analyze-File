# Read-and-Analyze-File
Read and Analyze
#include <iostream>
#include <fstream>
#include <string>
#include <stdlib.h>

using namespace std;
struct ListNode
{
string name;
int pop1, pop2;
double value;
ListNode *next;
// Constructor
ListNode (string str, int value1, int value2, double double1, ListNode * next1 = nullptr) // Constructor function inside (inline in) the struct definition
{
name = str;
pop1 = value1;
pop2 = value2;
value = double1;
next = next1;
}
};

// Function prototypes

int size (ListNode *);
void displayList (ListNode *);
void writeData (ListNode *, string);
void writeNewFile (ListNode *, string);
void findStates (ListNode *, char);
void deleteList (ListNode *);

int main ()
{
ListNode *numberList = nullptr; // List of numbers

// Open the file
ifstream numberFile ("CensusData.csv");
if (!numberFile)
{
cout << "Error in opening the file of numbers.";
exit (1);
}
string line;
string name;
double number;
int pos1, pos2;
string validStateLetters = "";
cout << "1.Reading the file and storing in linked list.";
// Read the file into a linked list
while (getline (numberFile, line))
{
std::string delimiter = ",";
validStateLetters += string (1, line[0]) + ",";

size_t pos = 0;
string token;
int n = 1;
while ((pos = line.find (",")) != string::npos)
{
token = line.substr (0, pos);
line.erase (0, pos + delimiter.length ());
if (n == 1)
{
name = token;
n++;
}
else if (n == 2)
{
pos1 = atoi (token.c_str ());
n++;
}
else if (n == 3)
{
pos2 = atoi (token.c_str ());
n++;
}
else if (n == 4)
{
number = atof (token.c_str ());
n++;
}
}
// Create a node to hold this number.
numberList = new ListNode (name, pos1, pos2, number, numberList);
}

// Print the list
cout << endl << "The contents of the list are: " << endl;
//displayList(numberList);
// Print the size of the list
cout << endl << "The number of items in the list is: " << size (numberList);

cout << endl << "2. Writing data to CensusNodes.txt";
//writeData(numberList, "CensusNodes.txt");

cout << endl << "3. Listing the states on List based on first letter.";
cout << endl << "Enter the first letter: ";
string letter;
cin >> letter;
if (validStateLetters.find (letter) != string::npos)
{
cout << "The States with that letter are ";
findStates (numberList, letter[0]);
cout << endl;
}
else
{
cout << "There are no states with that letter. ";
}

cout << endl <<
"Writing answers in the text file: CensusNodes.txt";
writeNewFile (numberList, "CensusNodes.txt");

cout << endl << "Deleting the node:";
deleteList (numberList);

return 0;
}

//*****************************************
// length computes the number of nodes in *
// a linked list *
//*****************************************
int size (ListNode * ptr)
{
if (ptr == nullptr)
return 0;
else
return 1 + size (ptr->next);
}

//*******************************************
// finds and prints the states whose name
// starts with c
//*******************************************

void findStates (ListNode * ptr, char c)
{
if (ptr != nullptr)
{
if (ptr->name[0] == c)
cout << ptr->name << endl;
findStates (ptr->next, c);
}
}

//*******************************************
// displayList prints all the values stored *
// in the list *
//*******************************************

void displayList (ListNode * ptr)
{
if (ptr != nullptr)
{
cout << ptr->name << " , ";
displayList (ptr->next);
}
}


//*******************************************
// writes the name and memory address of each *
//pointers stored in the list *
//*******************************************

void writeData (ListNode * ptr, string filename)
{
ofstream MyFile (filename);
while (ptr != nullptr)
{
MyFile << ptr->name << ", " << &ptr << endl;
ptr = ptr->next;
}
}

//*******************************************
// writes the whole data of each *
//pointers stored in the list in new file*
//*******************************************

void writeNewFile (ListNode * ptr, string filename)
{
ofstream MyFile (filename);
while (ptr != nullptr)
{
MyFile << ptr->name << "," << ptr->pop1 << "," << ptr->
pop2 << "," << ptr->value << endl;
ptr = ptr->next;
}
MyFile << "End of Original Node List" << endl;
}

//*******************************************
// delete all the nodes stored *
// in the list *
//*******************************************

void deleteList (ListNode * ptr)
{
while (ptr != nullptr)
{
ListNode *temp = ptr;
ptr = ptr->next;
free (temp);
}
}
