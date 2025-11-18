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

char *rf_enc(const char *s,int r){
    int n=strlen(s); if(r<=1||n==0) return strdup(s);
    char *o=malloc(n+1),*p=o; int c=2*(r-1);
    for(int row=0;row<r;++row)
        for(int i=row;i<n;i+=c){
            *p++=s[i];
            int j=i+c-2*row; if(row>0&&row<r-1&&j<n)*p++=s[j];
        }
    *p=0; return o;
}

char *rf_dec(const char *c,int r){
    int n=strlen(c); if(r<=1||n==0) return strdup(c);
    int cycle=2*(r-1),*cnt=calloc(r,sizeof(int));
    for(int i=0;i<n;++i){ int m=i%cycle,row=m<r?m:cycle-m; cnt[row]++; }
    char **rows=malloc(r*sizeof(char*)),*o=malloc(n+1); int pos=0;
    for(int i=0;i<r;i++){ rows[i]=malloc(cnt[i]); for(int j=0;j<cnt[i];j++) rows[i][j]=c[pos++]; }
    int idx[r]; for(int i=0;i<r;i++) idx[i]=0;
    for(int i=0;i<n;i++){ int m=i%cycle,row=m<r?m:cycle-m; o[i]=rows[row][idx[row]++]; }
    o[n]=0; for(int i=0;i<r;i++) free(rows[i]); free(rows); free(cnt); return o;
}

int main(){
    char t[512],c[512]; int k;
    printf("Enter text: "); fgets(t,sizeof t,stdin); t[strcspn(t,"\n")]=0;
    printf("Enter number of rails: "); scanf("%d",&k);
    char *e=rf_enc(t,k); printf("Encrypted: %s\n",e);
    printf("Enter encrypted text to decrypt (no spaces): "); scanf("%s",c);
    char *d=rf_dec(c,k); printf("Decrypted: %s\n",d);
    free(e); free(d);
}


```
# OUTPUT

<img width="663" height="490" alt="Screenshot 2025-11-18 172315" src="https://github.com/user-attachments/assets/786dfe61-5e4e-487b-a05c-e17d4b34c202" />

# RESULT

Thus, the program executed successfully.
