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
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 30

void toLowerCase(char plain[], int ps) {
  for (int i = 0; i < ps; i++) {
      if (plain[i] >= 'A' && plain[i] <= 'Z')
          plain[i] += 32;
  }
}

int removeSpaces(char *plain, int ps) {
  int i, count = 0;
  for (i = 0; i < ps; i++) {
      if (plain[i] != ' ')
          plain[count++] = plain[i];
  }
  plain[count] = '\0';
  return count;
}

void generateKeyTable(char key[], int ks, char keyT[5][5]) {
  int i, j, k;
  int dicty[26] = {0};
  for (i = 0; i < ks; i++) {
      if (key[i] != 'j')
          dicty[key[i] - 'a'] = 2;
  }
  dicty['j' - 'a'] = 1;
  i = 0; j = 0;
  for (k = 0; k < ks; k++) {
      if (dicty[key[k] - 'a'] == 2) {
          dicty[key[k] - 'a'] -= 1;
          keyT[i][j] = key[k];
          j++;
          if (j == 5) {
              i++;
              j = 0;
          }
      }
  }
  for (k = 0; k < 26; k++) {
      if (dicty[k] == 0 && (char)(k + 'a') != 'j') {
          keyT[i][j] = (char)(k + 'a');
          j++;
          if (j == 5) {
              i++;
              j = 0;
          }
      }
  }
}

void search(char keyT[5][5], char a, char b, int arr[]) {
  int i, j;
  if (a == 'j') a = 'i';
  if (b == 'j') b = 'i';
  for (i = 0; i < 5; i++) {
      for (j = 0; j < 5; j++) {
          if (keyT[i][j] == a) {
              arr[0] = i;
              arr[1] = j;
          } else if (keyT[i][j] == b) {
              arr[2] = i;
              arr[3] = j;
          }
      }
  }
}

int mod5(int a) {
  return (a % 5 + 5) % 5;
}

int prepare(char str[], int ptrs) {
  if (ptrs % 2 != 0) {
      str[ptrs++] = 'z';
      str[ptrs] = '\0';
  }
  return ptrs;
}

void encrypt(char str[], char keyT[5][5], int ps) {
  int i, a[4];
  for (i = 0; i < ps; i += 2) {
      search(keyT, str[i], str[i + 1], a);
      if (a[0] == a[2]) {
          str[i] = keyT[a[0]][mod5(a[1] + 1)];
          str[i + 1] = keyT[a[2]][mod5(a[3] + 1)];
      } else if (a[1] == a[3]) {
          str[i] = keyT[mod5(a[0] + 1)][a[1]];
          str[i + 1] = keyT[mod5(a[2] + 1)][a[3]];
      } else {
          str[i] = keyT[a[0]][a[3]];
          str[i + 1] = keyT[a[2]][a[1]];
      }
  }
}

void decrypt(char str[], char keyT[5][5], int ps) {
  int i, a[4];
  for (i = 0; i < ps; i += 2) {
      search(keyT, str[i], str[i + 1], a);
      if (a[0] == a[2]) {
          str[i] = keyT[a[0]][mod5(a[1] - 1)];
          str[i + 1] = keyT[a[2]][mod5(a[3] - 1)];
      } else if (a[1] == a[3]) {
          str[i] = keyT[mod5(a[0] - 1)][a[1]];
          str[i + 1] = keyT[mod5(a[2] - 1)][a[3]];
      } else {
          str[i] = keyT[a[0]][a[3]];
          str[i + 1] = keyT[a[2]][a[1]];
      }
  }
}

void encryptByPlayfairCipher(char str[], char key[]) {
  int ps, ks;
  char keyT[5][5];
  ks = strlen(key);
  ks = removeSpaces(key, ks);
  toLowerCase(key, ks);
  ps = strlen(str);
  toLowerCase(str, ps);
  ps = removeSpaces(str, ps);
  ps = prepare(str, ps);
  generateKeyTable(key, ks, keyT);
  encrypt(str, keyT, ps);
}

void decryptByPlayfairCipher(char str[], char key[]) {
  int ps, ks;
  char keyT[5][5];
  ks = strlen(key);
  ks = removeSpaces(key, ks);
  toLowerCase(key, ks);
  ps = strlen(str);
  toLowerCase(str, ps);
  ps = removeSpaces(str, ps);
  generateKeyTable(key, ks, keyT);
  decrypt(str, keyT, ps);
}

int main() {
  char str[SIZE], key[SIZE];
  printf("Simulating Playfair Cipher\n");
  strcpy(key, "NAME");
  printf("Key text: %s\n", key);
  strcpy(str, "KUSHMA");
  printf("Plain text: %s\n", str);
  encryptByPlayfairCipher(str, key);
  printf("Cipher text: %s\n", str);
  decryptByPlayfairCipher(str, key);
  printf("Decrypted text: %s\n", str);
  return 0;
}

```
 ## OUTPUT :
 <img width="961" height="412" alt="Screenshot 2025-09-22 023401" src="https://github.com/user-attachments/assets/b0e00b66-cf34-47f4-bbf4-7991682fb049" />


 ## RESULT:
Hence a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique is executed successfully.
