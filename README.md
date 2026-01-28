## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 -----
 #### Date: 28/01/26
 #### Reg No: 212224230211
 #### Name: Priyan V
 -----

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:
```

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.

```


## Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5
char matrix[SIZE][SIZE];

void generateMatrix(const char key[]) {
    int used[26] = {0};
    int row = 0, col = 0;

    for (int i = 0; key[i] && row < SIZE; i++) {
        if (!isalpha(key[i])) continue;
        char ch = toupper(key[i]);
        if (ch == 'J') ch = 'I';

        if (!used[ch - 'A']) {
            matrix[row][col++] = ch;
            used[ch - 'A'] = 1;
            if (col == SIZE) { col = 0; row++; }
        }
    }

    for (char ch = 'A'; ch <= 'Z' && row < SIZE; ch++) {
        if (ch == 'J' || used[ch - 'A']) continue;
        matrix[row][col++] = ch;
        if (col == SIZE) { col = 0; row++; }
    }
}

void findPosition(char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I';
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
}

void encryptDigraph(char a, char b, char out[3]) {
    int r1, c1, r2, c2;
    findPosition(a, &r1, &c1);
    findPosition(b, &r2, &c2);

    if (r1 == r2) {
        out[0] = matrix[r1][(c1 + 1) % SIZE];
        out[1] = matrix[r2][(c2 + 1) % SIZE];
    } 
    else if (c1 == c2) {
        out[0] = matrix[(r1 + 1) % SIZE][c1];
        out[1] = matrix[(r2 + 1) % SIZE][c2];
    } 
    else {
        out[0] = matrix[r1][c2];
        out[1] = matrix[r2][c1];
    }
    out[2] = '\0';
}

int prepareText(const char *input, char digraphs[][2], int max) {
    char clean[200];
    int n = 0;

    for (int i = 0; input[i]; i++) {
        if (!isalpha(input[i])) continue;
        char ch = toupper(input[i]);
        if (ch == 'J') ch = 'I';
        clean[n++] = ch;
    }

    int d = 0;
    for (int i = 0; i < n && d < max; ) {
        char a = clean[i];
        char b = (i + 1 < n) ? clean[i + 1] : 'X';

        if (a == b) {
            digraphs[d][0] = a;
            digraphs[d][1] = 'X';
            i++;
        } else {
            digraphs[d][0] = a;
            digraphs[d][1] = b;
            i += 2;
        }
        d++;
    }
    return d;
}

void displayMatrix() {
    printf("Playfair Matrix:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++)
            printf("%c ", matrix[i][j]);
        printf("\n");
    }
}

int main() {
    char key[] = "KEYWORD";
    char text[] = "Playfair";

    generateMatrix(key);
    displayMatrix();

    char digraphs[50][2];
    int count = prepareText(text, digraphs, 50);

    printf("\nPlaintext Digraphs:\n");
    for (int i = 0; i < count; i++)
        printf("%c%c ", digraphs[i][0], digraphs[i][1]);

    printf("\n\nEncrypted Text: ");
    char pair[3], final[200] = "";

    for (int i = 0; i < count; i++) {
        encryptDigraph(digraphs[i][0], digraphs[i][1], pair);
        printf("%s ", pair);
        strcat(final, pair);
    }

    printf("\n\nFinal Encrypted Output: %s\n", final);
    return 0;
}
```




## Output:
<img width="1738" height="967" alt="Screenshot 2026-01-28 063557" src="https://github.com/user-attachments/assets/84625c92-a8f8-4e3b-a65a-4dc291c83d4f" />





## Result:
The program is executed successfully.
