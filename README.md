# Ex-5 Rail-Fence-Program

# IMPLEMENTATION OF RAIL FENCE – ROW & COLUMN TRANSFORMATION TECHNIQUE

# AIM:

To write a C program to implement the rail fence transposition technique.

# DESCRIPTION:

In the rail fence cipher, the plain text is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

# ALGORITHM:

STEP-1: Read the Plain text.

STEP-2: Arrange the plain text in row columnar matrix format.

STEP-3: Now read the keyword depending on the number of columns of the plain text.

STEP-4: Arrange the characters of the keyword in sorted order and the corresponding columns of the plain text.

STEP-5: Read the characters row wise or column wise in the former order to get the cipher text.

# PROGRAM

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char *rf_encrypt(const char *s, int r){
    int n=strlen(s);
    if(r<=1||n==0) return strdup(s);
    char *out=malloc(n+1), *p=out; int cycle=2*(r-1);
    for(int row=0; row<r; ++row)
      for(int i=row; i<n; i+=cycle){
        *p++=s[i];
        int j=i+cycle-2*row;
        if(row>0 && row<r-1 && j<n) *p++=s[j];
      }
    *p=0; return out;
}

char *rf_decrypt(const char *c, int r){
    int n=strlen(c);
    if(r<=1||n==0) return strdup(c);
    int cycle=2*(r-1);
    int *cnt=calloc(r,sizeof(int));
    // count chars per row
    for(int i=0;i<n;++i){
        int mod=i%cycle;
        int row = mod<r ? mod : cycle-mod;
        cnt[row]++;
    }
 
    char **rows = malloc(r*sizeof(char*));
    int pos=0;
    for(int row=0; row<r; ++row){
        rows[row]=malloc(cnt[row]+1);
        for(int k=0;k<cnt[row];++k) rows[row][k]=c[pos++];
        rows[row][cnt[row]]=0;
    }

    char *out=malloc(n+1);
    int idxs[r]; for(int i=0;i<r;++i) idxs[i]=0;
    for(int i=0;i<n;++i){
        int mod=i%cycle;
        int row = mod<r ? mod : cycle-mod;
        out[i]=rows[row][ idxs[row]++ ];
    }
    out[n]=0;
    for(int i=0;i<r;++i) free(rows[i]);
    free(rows); free(cnt);
    return out;
}

int main(void){
    char text[512], cipher[512];
    int key;
    printf("Text: "); fgets(text,sizeof text,stdin); text[strcspn(text,"\n")]=0;
    printf("Rails: "); if(scanf("%d",&key)!=1) return 0;
    char *enc = rf_encrypt(text,key);
    printf("Encrypted: %s\n", enc);
    printf("Enter encrypted (no spaces) to decrypt: ");
    if(scanf("%s",cipher)!=1){ free(enc); return 0; }
    char *dec = rf_decrypt(cipher,key);
    printf("Decrypted: %s\n", dec);
    free(enc); free(dec);
    return 0;
}

```
# OUTPUT

<img width="663" height="490" alt="Screenshot 2025-11-18 172315" src="https://github.com/user-attachments/assets/786dfe61-5e4e-487b-a05c-e17d4b34c202" />

# RESULT

Thus, the program executed successfully.
