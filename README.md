## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

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

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char matrix[5][5];

// Generate 5x5 key matrix
void generateMatrix(char key[]) {
    int used[26] = {0};
    int k = 0;

    used['J' - 'A'] = 1;

    for (int i = 0; key[i] != '\0'; i++) {
        char ch = toupper((unsigned char)key[i]);

        if (ch == 'J')
            ch = 'I';

        if (ch >= 'A' && ch <= 'Z' && !used[ch - 'A']) {
            matrix[k / 5][k % 5] = ch;
            used[ch - 'A'] = 1;
            k++;
        }
    }

    for (int i = 0; i < 26; i++) {
        if (!used[i]) {
            matrix[k / 5][k % 5] = 'A' + i;
            k++;
        }
    }
}

// Find position of a character in matrix
void findPosition(char ch, int *row, int *col) {
    if (ch == 'J')
        ch = 'I';

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Playfair Encryption
void encrypt(char text[]) {
    char processed[1000];
    char cipher[1000];
    int n = 0;

    // Remove spaces and prepare plaintext
    for (int i = 0; text[i] != '\0'; i++) {
        char ch = toupper((unsigned char)text[i]);

        if (isalpha((unsigned char)ch)) {
            if (ch == 'J')
                ch = 'I';

            processed[n++] = ch;
        }
    }

    processed[n] = '\0';

    // Form pairs and insert X between repeated letters
    int i = 0, j = 0;

    while (i < n) {
        char a = processed[i];
        char b;

        if (i + 1 >= n || a == processed[i + 1]) {
            b = (a == 'X') ? 'Q' : 'X';
            i++;
        }
        else {
            b = processed[i + 1];
            i += 2;
        }

        int r1, c1, r2, c2;

        findPosition(a, &r1, &c1);
        findPosition(b, &r2, &c2);

        // Same row
        if (r1 == r2) {
            cipher[j++] = matrix[r1][(c1 + 1) % 5];
            cipher[j++] = matrix[r2][(c2 + 1) % 5];
        }

        // Same column
        else if (c1 == c2) {
            cipher[j++] = matrix[(r1 + 1) % 5][c1];
            cipher[j++] = matrix[(r2 + 1) % 5][c2];
        }

        // Rectangle rule
        else {
            cipher[j++] = matrix[r1][c2];
            cipher[j++] = matrix[r2][c1];
        }
    }

    cipher[j] = '\0';

    printf("\nKey Matrix:\n");
    for (int r = 0; r < 5; r++) {
        for (int c = 0; c < 5; c++) {
            printf("%c ", matrix[r][c]);
        }
        printf("\n");
    }

    printf("\nCiphertext: ");
    for (int k = 0; k < j; k++) {
        printf("%c", cipher[k]);

        if ((k + 1) % 2 == 0 && k != j - 1)
            printf(" ");
    }
    printf("\n");
    }

    int main() {
    char plaintext[500];
    char key[100];

    printf("Enter Plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0';

    printf("Enter Keyword: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';

    if (strlen(key) == 0) {
        printf("Invalid Key!\n");
        return 1;
    }

    generateMatrix(key);
    encrypt(plaintext);

    return 0;
    }
```



Output:

<img width="1157" height="420" alt="image" src="https://github.com/user-attachments/assets/d47dde69-05ed-4f06-ba47-8c3829d32756" />

Result :

Thus the implementation of Playfair Cipher Encryption and decryption using C is ran and executed successfully.

