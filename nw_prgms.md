## CRC

```c
#include<stdio.h>
#include<string.h>
#define N strlen(g)

char t[128],cs[128],g[]="1000100000010001";
int a,e,c;

void xor()
{
  for(c=1;c<N;c++) cs[c]=((cs[c]==g[c])?'0':'1');
}

void crc()
{
  for(e=0;e<N;e++) cs[e]=t[e];
  do{
    if(cs[0]=='1') xor();
    for(c=0;c<N-1;c++) cs[c]=cs[c+1];
    cs[c]=t[e++];
  }
  while(e<=a+N-1);
}


int main() {
  printf("Enter poly ");
  scanf("%s",t);
  printf("Generating poly %s",g);
  a = strlen(t);
  for(e=a;e<a+N-1;e++) t[e]='0';
  printf("Modified t[u] %s\n",t);
  crc();
  printf("Checksum  %s\n",cs);
  for(e=a;e<a+N-1;e++) t[e]=cs[e-a];
  printf("Final Codeword  %s\n",t);
  printf("Eroor 0 or 1\n");
  scanf("%d",&e);
  if(e==0)
  {
    scanf("%d",&c);
    t[c]=(t[c]=='0')?'1':'0';
    printf("Erraneuos data: %s",t);
  }
  crc();
  for(e=0;(e<N-1)&&(cs[e]!='1');e++);
  if(e<N-1) printf("Error detected.");
  else printf("No Error Detected.");
  return 0;
}

```


## Leaky bucket


```c
#include<stdio.h>
#include<string.h>
#define bucket_size 500
void leaky_bucket(int a,int b)
{
  if(a>bucket_size)
   printf("Bucket overflow\n" );
   else{
     while(a>b)
     {printf("Output of %d bytes\n",b);
     a=a-b;
   }
     if(a>0)
       printf("Output of last %d bytes\n",a);
     printf("Successful");
   }

}
int main() {
  int op,size;
  printf("Enter out rate");
  scanf("%d",&op);
  printf("Enter packet size");
  scanf("%d",&size);
  leaky_bucket(size,op);
  return 0;
}
```


## RSA

```cpp
#include<iostream>
#include<stdlib.h>
#include<math.h>
#include<string.h>

using namespace std;

long int gcd(int a ,int b)
{
  if(a==0)
    return b;
  if(b==0)
    return a;
  return gcd(b,a%b);
}

int is_prime(int a)
{
  int i;
  for(i=2;i<a;i++)
    if(a%i==0)
      return 0;
  return 1;
}

long int encrypt(char ch,long int n,long int e)
{
  long int i;
  long int temp=ch;
  for(i=1;i<e;i++)
    temp = (temp *ch ) % n;
  return temp;
}

char decrypt(long int ch,long int n,long int d)
{
  long int i;
  long int temp=ch;
  for(i=1;i<d;i++)
    ch = (temp *ch ) % n;
  return ch;
}


int main() {
  long int p,q,n,phi,d,e,len,i,cipher[1000];
  char text[100];
  do {
    p = rand() % 30;
  }while(!is_prime(p));
  do {
    q = rand() % 30;
  }while(!is_prime(q));
  n = p*q;
  phi = (p-1)*(q-1);
  do {
   e = rand() % phi;
 }while(gcd(phi,e)!=1);
 do {
  d = rand() % phi;
}while((d*e)%phi!=1);
cout<<"p"<<p<<endl;
cout<<"q"<<q<<endl;
cout<<"(n,e)"<<n<<","<<e<<endl;
cout<<"(n,d)"<<n<<","<<d<<endl;
cout<<"phi "<<phi<<endl;

cin.getline(text,sizeof(text));
len = strlen(text);
cout<<"text: "<<text<<"\n len "<<len<<endl;

for(i=0;i<len;i++)
  cipher[i]=encrypt(text[i],n,e);

  for (i = 0; i < len; i++)
      cout << cipher[i];

for(i=0;i<len;i++)
  text[i]=decrypt(cipher[i],n,d);

  for (i = 0; i < len; i++)
      cout << text[i];

cout<<"text: "<<text<<endl;

  return 0;

}
```

## DSV

```cpp
#include<iostream>
#include<stdio.h>

using namespace std;

struct node {
  int dist[20];
  int from[20];
}route[10];

int main(){
  int i,j,k,no,dm[20][20],flag;
  cout<<"Enter no of nodes ";
  cin>>no;
  cout<<"Enter distance matrix";

  for(i=0;i<no;i++){
    for(j=0;j<no;j++){
      cin>>dm[i][j];

      dm[i][i] = 0;
      route[i].dist[j] = dm[i][j];
      route[i].from[j] = j;
    }
  }

  do {
    flag = 0;
    for(i=0;i<no;i++){
      for(j=0;j<no;j++){
        for(k=0;k<no;k++){
          if(route[i].dist[j] > route[i].dist[k]+ route[k].dist[j]) {
           route[i].dist[j] > route[i].dist[k]+ route[k].dist[j];
           route[i].from[j]=k;
           flag=1;
         }
        }
      }
    }
  }while(flag);

  for(i=0;i<no;i++){
    cout << "For node number "<<i<<endl;
    cout <<" Dest\tNext hop\tDistance"<<endl;
    for (j = 0; j < no; j++) {
      cout<<j+1<<"\t"<<route[i].from[j]+1<<"\t\t"<<route[i].dist[j]<<endl;
    }
  }
  return 0;
}
```

## FIFO

### FIFO Client

```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/stat.h>

int main() {
  int res,req,n;
  char buffer[1024],fname[50];

  req = open("req.fifo",O_WRONLY);
  res = open("res.fifo",O_RDONLY);
  if(res<0 || req<0)
    printf("Start server\n" );

  scanf("%s",fname);
  write(req,fname,sizeof(fname));


    while ((n=read(res,buffer,sizeof(buffer)))>0) {
      write(1,buffer,n);
    }



  return  0;
}
```

### FIFO Server

```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/stat.h>

int main() {
  int res,req,file,n;
  char buffer[1024],fname[50];

  mkfifo("req.fifo",0777);
  mkfifo("res.fifo",0777);

  req = open("req.fifo",O_RDONLY);
  res = open("res.fifo",O_WRONLY);

  printf("Waiting for req\n");
  read(req,fname,sizeof(fname));

  file = open(fname,O_RDONLY);
  if(file<0)
    write(res,"file not found",15);

  else
    while ((n=read(file,buffer,sizeof(buffer)))>0) {
      write(res,buffer,n);
    }
  close(res);
  close(req);
  unlink("res.fifo");
  unlink("res.fifo");


  return  0;
}
```

## TCP Client server

### TCP Client

```c
#include<stdio.h>
#include<arpa/inet.h>
#include<fcntl.h>
#include<unistd.h>

int main(int argc, char const *argv[]) {
  int soc,n;
  char fname[50],buffer[1024];
  struct sockaddr_in addr;

  soc = socket(PF_INET,SOCK_STREAM,0);

  addr.sin_family = AF_INET;
  addr.sin_port = htons(7891);
  addr.sin_addr.s_addr = inet_addr("127.0.0.1");

  while(connect(soc,(struct sockaddr *) &addr, sizeof(addr)));
  printf("Client connected\n Enter filename ");
  scanf("%s",fname);

  send(soc,fname,sizeof(fname),0);

  while((n=recv(soc,buffer,1024,0))>0)
    printf("%s",buffer);

  return 0;
}
```

### TCP server

```c
#include<stdio.h>
#include<arpa/inet.h>
#include<fcntl.h>
#include<unistd.h>

int main() {
  int fd,n,welcome,new_soc;
  char fname[50],buffer[1024];
  struct sockaddr_in addr;

  welcome = socket(PF_INET,SOCK_STREAM,0);

  addr.sin_family = AF_INET;
  addr.sin_port = htons(7891);
  addr.sin_addr.s_addr = inet_addr("127.0.0.1");

  bind(welcome, (struct sockaddr *)&addr, sizeof(addr));
  listen(welcome,5);
  printf("Server is online");
  new_soc = accept(welcome,NULL,NULL);

  recv(new_soc,fname,50,0);
  fd = open(fname,O_RDONLY);

  if(fd<0)
    send(new_soc,"File not found\n",15,0);
  else
    while((n=read(fd,buffer,sizeof(buffer)))>0)
      send(new_soc,buffer,sizeof(buffer),0);
  printf("Req sent");

  return 0;
}
```
