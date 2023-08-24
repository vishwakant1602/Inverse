# Inverse
#include <stdlib.h>
#include <stdio.h>

void railfence_encipher(int key, const char *plaintext, char *ciphertext);
void railfence_decipher(int key, const char *ciphertext, char *plaintext);

int main(int argc, char *argv[]){
    /* the string to encipher / decipher */
    char *plaintext = "defendtheeastwallofthecastle";
    /* allocate some space for results of enciphering/deciphering */
    char *ciphertext = malloc(strlen(plaintext)+1);
    char *result = malloc(strlen(plaintext)+1);
    
    /* the following lines do all the enciphering and deciphering */
    railfence_encipher(3, plaintext,ciphertext);
    railfence_decipher(3, ciphertext,result);
    /* print our results */
    printf("-->original:   %s\n-->ciphertext: %s\n-->plaintext:  %s\n",
            plaintext,ciphertext,result);
    
    free(ciphertext);
    free(result);
    
    system("PAUSE");
    return 0;
}

/*******************************************************************
void railfence_encipher(int key, const char *plaintext, char *ciphertext)
- Uses railfence transposition cipher to encipher some text.
 - takes a key, string of plaintext, result returned in ciphertext.
 - ciphertext should be an array the same size as plaintext.
- The key is the number of rails to use
*******************************************************************/
void railfence_encipher(int key, const char *plaintext, char *ciphertext){
    int line, i, skip, length = strlen(plaintext), j=0,k=0;    
    for(line = 0; line < key-1; line++){
        skip = 2*(key - line - 1); 
        k=0;
        for(i = line; i < length;){
            ciphertext[j] = plaintext[i];
            if((line==0) || (k%2 == 0)) i+=skip;
            else i+=2*(key-1) - skip;  
            j++;   k++;
        }
    }
    for(i=line; i<length; i+=2*(key-1)) ciphertext[j++] = plaintext[i];
    ciphertext[j] = '\0'; /* Null terminate */  
}

/*******************************************************************
void railfence_decipher(int key, const char *ciphertext, char *plaintext)
- Uses railfence transposition cipher to decipher some text.
- takes a key, string of ciphertext, result returned in plaintext.
- plaintext should be an array the same size as plaintext.
- The key is the number of rails to use
*******************************************************************/
void railfence_decipher(int key, const char *ciphertext, char *plaintext){
    int i, length = strlen(ciphertext), skip, line, j, k=0;
    for(line=0; line<key-1; line++){
        skip=2*(key-line-1);	  
        j=0;
        for(i=line; i<length;){
            plaintext[i] = ciphertext[k++];
            if((line==0) || (j%2 == 0)) i+=skip;
            else i+=2*(key-1) - skip;  
            j++;        
        }
    }
    for(i=line; i<length; i+=2*(key-1)) plaintext[i] = ciphertext[k++];
    plaintext[length] = '\0'; /* Null terminate */  
  
Example
#include<bits/stdc++.h>
using namespace std;
#define N 5
void getCfactor(int M[N][N], int t[N][N], int p, int q, int n) {
   int i = 0, j = 0;
   for (int r= 0; r< n; r++) {
      for (int c = 0; c< n; c++) //Copy only those elements which are not in given row r and column c: {
         if (r != p && c != q) { t[i][j++] = M[r][c]; //If row is filled increase r index and reset c index
            if (j == n - 1) {
               j = 0; i++;
            }
         }
      }
   }
}
int DET(int M[N][N], int n) //to find determinant {
   int D = 0;
   if (n == 1)
      return M[0][0];
   int t[N][N]; //store cofactors
   int s = 1; //store sign multiplier //
   To Iterate each element of first row
   for (int f = 0; f < n; f++) {
      //For Getting Cofactor of M[0][f] do getCfactor(M, t, 0, f, n); D += s * M[0][f] * DET(t, n - 1);
      s = -s;
   }
   return D;
}
void ADJ(int M[N][N],int adj[N][N])
//to find adjoint matrix {
   if (N == 1) {
      adj[0][0] = 1; return;
   }
   int s = 1,
   t[N][N];
   for (int i=0; i<N; i++) {
      for (int j=0; j<N; j++) {
         //To get cofactor of M[i][j]
         getCfactor(M, t, i, j, N);
         s = ((i+j)%2==0)? 1: -1; //sign of adj[j][i] positive if sum of row and column indexes is even.
         adj[j][i] = (s)*(DET(t, N-1)); //Interchange rows and columns to get the transpose of the cofactor matrix
      }
   }
}
bool INV(int M[N][N], float inv[N][N]) {
   int det = DET(M, N);
   if (det == 0) {
      cout << "can't find its inverse";
      return false;
   }
   int adj[N][N]; ADJ(M, adj);
   for (int i=0; i<N; i++) for (int j=0; j<N; j++) inv[i][j] = adj[i][j]/float(det);
   return true;
}
template<class T> void print(T A[N][N]) //print the matrix. {
   for (int i=0; i<N; i++) { for (int j=0; j<N; j++) cout << A[i][j] << " "; cout << endl; }
}
int main() {
   int M[N][N] = {
      {1, 2, 3, 4,-2}, {-5, 6, 7, 8, 4}, {9, 10, -11, 12, 1}, {13, -14, -15, 0, 9}, {20 , -26 , 16 , -17 , 25}
   };
   float inv[N][N];
   cout << "Input matrix is :\n"; print(M);
   cout << "\nThe Inverse is :\n"; if (INV(M, inv)) print(inv);
   return 0;
}
