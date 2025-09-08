## NAME: S.KUSHMA
## REG NO: 212224040168

## EXP : 2 IMPLEMENTATION OF PLAY FAIR CIPHER
  
 ## AIM :
 To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.
 
 ## ALGORITHM:
 1.	To perform Encryption :
 •	Preprocess the key:
 a.	Convert the key to lowercase.
 b.	Remove spaces and duplicate letters.
 c.	Replace 'j' with 'i'.
 2.	Generate the 5×5 key matrix (key table):
 a.	Fill in the unique characters of the key.
 b.	Fill the remaining cells with unused letters of the alphabet (except 'j').
 3.	Preprocess the plaintext:
 a.	Convert to lowercase.
 b.	Remove spaces.
 c.	Replace 'j' with 'i'.
 d.	Break into digraphs (pairs of two letters).
 If a pair has two same letters, insert 'x' between them.
 If there's a single letter left at the end, add a padding letter (like 'z').
 4.	Encrypt each digraph using these rules:
 a.	Same row: Replace each letter with the letter to its right (wrap to start if needed).
 b.	Same column: Replace each letter with the letter below it (wrap to top if needed).
 c.	Different row and column: Swap the column positions of the two letters.
 5.	Display the encrypted ciphertext.
 6.	To perform Decryption:
 •	Preprocess the ciphertext like the encryption step.
 •	Decrypt each digraph using these rules:
 a.	Same row: Replace each letter with the letter to its left (wrap to end if needed).
 
 b.	Same column: Replace each letter with the letter above it (wrap to bottom if needed).
 
 c.	Different row and column: Swap the column positions of the two letters.
 
 7.	Display the decrypted plaintext.
 
 ## PROGRAM :
 ```
#include <string.h>
#include <ctype.h>
#define SIZE 5
 char keyTable[SIZE][SIZE];
 int pos[26][2]; 
 void generateKeyTable(char key[]) {
 int used[26] = {0};
 int row = 0, col = 0;
   
    used['J' - 'A'] = 1;
   
    for (int i = 0; i < strlen(key); i++) {
        char c = toupper(key[i]);
        if (c < 'A' || c > 'Z') continue;
        if (c == 'J') c = 'I';
        if (!used[c - 'A']) {
            keyTable[row][col] = c;
            pos[c - 'A'][0] = row;
            pos[c - 'A'][1] = col;
            used[c - 'A'] = 1;
            col++;
            if (col == SIZE) { col = 0; row++; }
        }
    }
   
    for (char c = 'A'; c <= 'Z'; c++) {
        if (!used[c - 'A']) {
            keyTable[row][col] = c;
            pos[c - 'A'][0] = row;
            pos[c - 'A'][1] = col;
            used[c - 'A'] = 1;
            col++;
            if (col == SIZE) { col = 0; row++; }
        }
    }
 }

 int prepareText(char text[], char pairs[][2]) {
    int n = strlen(text), k = 0;
    for (int i = 0; i < n; i++) {
        char c = toupper(text[i]);
        if (c < 'A' || c > 'Z') continue;
        if (c == 'J') c = 'I';
        if (k > 0 && pairs[k-1][0] == c && pairs[k-1][1] == '\0') {
            pairs[k-1][1] = 'X';
            i--;
            k++;
        } else {
            if (pairs[k][0] == '\0')
                pairs[k][0] = c;
            else {
                pairs[k][1] = c;
                k++;
            }
        }
    }
   
    if (pairs[k][0] != '\0' && pairs[k][1] == '\0') {
        pairs[k][1] = 'X';
        k++;
    }
    return k;
 }
 
 void encryptPair(char a, char b, char *x, char *y) {
    int r1 = pos[a - 'A'][0], c1 = pos[a - 'A'][1];
    int r2 = pos[b - 'A'][0], c2 = pos[b - 'A'][1];
    if (r1 == r2) { 
        *x = keyTable[r1][(c1 + 1) % SIZE];
        *y = keyTable[r2][(c2 + 1) % SIZE];
    } else if (c1 == c2) { 
        *x = keyTable[(r1 + 1) % SIZE][c1];
        *y = keyTable[(r2 + 1) % SIZE][c2];
    } else { 
        *x = keyTable[r1][c2];
        *y = keyTable[r2][c1];
    }
 }
 int main() {
    char key[] = "KUSHMA";
    char text[] = "KUSHMA";
    char pairs[50][2] = {{0}};
    char encrypted[100];
    int pairCount, k = 0;
   
    generateKeyTable(key);
   
    
    pairCount = prepareText(text, pairs);
    printf("\nPairs:\n");
    for (int i = 0; i < pairCount; i++) {
        printf("%c%c ", pairs[i][0], pairs[i][1]);
    }
    
    printf("\n\nEncrypted Text: ");
    for (int i = 0; i < pairCount; i++) {
        char x, y;
        encryptPair(pairs[i][0], pairs[i][1], &x, &y);
        encrypted[k++] = x;
        encrypted[k++] = y;
    }
encrypted[k] = '\0';
 printf("%s\n", encrypted);
 return 0;
 }
```
 ## OUTPUT :
 <img width="1900" height="1026" alt="Screenshot 2025-09-08 133025" src="https://github.com/user-attachments/assets/fcb1bdaa-2b55-4724-ae9a-ef3c7f21c561" />

 ## RESULT:
Hence a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique is executed successfully.
