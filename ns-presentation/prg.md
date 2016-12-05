# CRC

```c
#include <stdio.h>
#include <string.h>
#define N strlen(g)

char t[128],cs[128],g[]="10001000000100001";
int a,e,c;

void xor()
{
  for(c=0;c<N;c++) cs[c]=((cs[c]==g[c])?'0':'1');
}

void crc()
{
  for(e=0;e<N;e++) cs[e]=t[e];
  do {
    if(cs[0]=='1') xor();
    for(c=0;c<N-1;c++) cs[c]=cs[c+1];
    cs[c] = t[e++];

  }
  while(e<=a+N-1);
}

int main()
{
  printf("Enter the polynomial ");
  scanf("%s",t);
  printf("Generating polynomial is: %s\n",g);
  a=strlen(t);
  for(e=a;e<a+N-1;e++) t[e]='0';
  printf("modified t[u] is: %s\n",t);
  crc();
  printf("checksum is: %s\n",cs);
  for(e=a;e<a+N-1;e++) t[e]=cs[e-a];
  printf("final codeword is: %s\n",t);
  printf("Error to be inserted 0:yes 1:no");
  scanf("%d",&e);
  if(e==0)
  {
    printf("Enter where error to be inserted ");
    scanf("%d",&e);
    t[e]=((t[e]=='0')?'1':'0');

  }
  crc();
  printf("Received polynomial is %s\n",t);
  for(e=0;(e<N+1)&&(cs[e]!='1');e++);
  if(e<N-1) printf("error detected");
   else printf("No error detected");
  return 0;
}
```

# Distvect

```cpp

#include <iostream>
#include <stdio.h>
#include <stdlib.h>

using namespace std;

struct node {
  int dist[20];
  int from[20];
} route[10];

int main()
{
  int dm[20][20],n;
  cout << "enter number of nodes" << endl;
  cin >> n;
  cout << "enter distance matrix" << endl;
  for(int i=0;i<n;i++)
    for(int j=0;j<n;j++) {
      cin >> dm[i][j];

      dm[i][i]=0;
      route[i].dist[j]=dm[i][j];
      route[i].from[j]=j;
    }
  int flag;
  do {
  for(int i=0;i<n;i++)
    for(int j=0;j<n;j++)
      for(int k=0;k<n;k++)
        if(route[i].dist[j]>route[i].dist[k]+route[k].dist[j]){
          route[i].dist[j]=route[i].dist[k]+route[k].dist[j];
          route[i].from[j]=k;
          flag=1;
        }
  }while(flag);

    for(int i=0;i<n;i++) {
      cout << "Router Information for " << i+1 <<endl;
      cout << "Dest\tNext Hop\tDistance\n";
      for(int j=0;j<n;j++)
        printf("%d\t%d\t%d\n",j+1,route[i].from[j]+1,route[i].dist[j]);

    }


}
```

# tcpserver

```c

#include <stdio.h>
#include <arpa/inet.h>
#include <fcntl.h>
#include <unistd.h>

int main()
{
  int welcome,new_soc,n,fd;
  char buffer[1024],fname[50];
  struct sockaddr_in addr;

  welcome = socket(PF_INET,SOCK_STREAM,0);

  addr.sin_family = AF_INET;
  addr.sin_port = htons(7891);
  addr.sin_addr.s_addr = inet_addr("127.0.0.1");

  bind(welcome,(struct sockaddr *)&addr,sizeof(addr));
  printf("Successfully started server");

  listen(welcome,5);
  new_soc = accept(welcome,NULL,NULL);

  recv(new_soc,fname,sizeof(fname),0);

  fd = open(fname,O_RDONLY);

  if(fd<0)
    printf("File not found\n");
  else
  {
    while((n=read(fd,buffer,sizeof(buffer))>0)){
      send(new_soc,buffer,n,0);
    }
  }

  printf("Sent data");
  close(fd);
  return 0;
}

```

# tcpclient

```c

#include <stdio.h>
#include <arpa/inet.h>
#include <fcntl.h>
#include <unistd.h>

int main()
{
  int soc,n;
  char buffer[1024],fname[50];
  struct sockaddr_in addr;

  soc = socket(PF_INET,SOCK_STREAM,0);

  addr.sin_family = AF_INET;
  addr.sin_port = htons(7891);
  addr.sin_addr.s_addr = inet_addr("127.0.0.1");

  while(connect(soc,(struct sockaddr *)&addr,sizeof(addr)));
  printf("Successfully connected to server");

  printf("Enter filename :");
  scanf("%s",fname);

  send(soc,fname,sizeof(fname),0);

  while((n=recv(soc,buffer,sizeof(buffer),0)>0)){
    printf("%s",buffer);
  }

  return 0;
}

```

# fifoserver

```c


#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<sys/types.h>

int main()
{
  int fd,res,req,n;
  char buffer[1024],fname[50];
  mkfifo("res.fifo",0777);
  mkfifo("req.fifo",0777);
  printf("Server started\n");
  req = open("req.fifo",O_RDONLY);
  res = open("res.fifo",O_WRONLY);

  read(req,fname,sizeof(fname));

  fd = open(fname,O_RDONLY);

  if(fd<0)
    write(res,"file not found",15);
  else {
    while((n=read(fd,buffer,sizeof(buffer)))>0){
      write(res,buffer,sizeof(buffer));
    }
  }
  close(res);
  close(req);
  unlink("res.fifo");
  unlink("req.fifo");
  return 0;

}

```

#fifoclient

```c

#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<sys/types.h>

int main()
{
  int fd,res,req,n;
  char buffer[1024],fname[50];

  req = open("req.fifo",O_WRONLY);
  res = open("res.fifo",O_RDONLY);

  if(req<0 || res<0)
  {
      printf("Start server first");

  }

  printf("Enter filename");
  scanf("%s",fname);

  write(req,fname,sizeof(fname));


    while((n=read(res,buffer,sizeof(buffer)))>0){
      printf("Buffer: %s",buffer);
    }

  close(res);
  close(req);
  return 0;
}

```

# RSA

```cpp

#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<sys/types.h>

int main()
{
  int fd,res,req,n;
  char buffer[1024],fname[50];

  req = open("req.fifo",O_WRONLY);
  res = open("res.fifo",O_RDONLY);

  if(req<0 || res<0)
  {
      printf("Start server first");

  }

  printf("Enter filename");
  scanf("%s",fname);

  write(req,fname,sizeof(fname));


    while((n=read(res,buffer,sizeof(buffer)))>0){
      printf("Buffer: %s",buffer);
    }

  close(res);
  close(req);
  return 0;
}

```

# leaky bucket

```c

#include<stdio.h>
#include<stdlib.h>
#include<math.h>

int main()
{
  int incoming_packet,bucket_size, store, n,out_rate;
  printf("Enter number of packets");
  scanf("%d",&n);
  printf("Enter bucket size");
  scanf("%d",&bucket_size);
  printf("Enter output rate");
  scanf("%d",&out_rate);

  while(n!=0){
    printf("Enter incoming packet size");
    scanf("%d",&incoming_packet);

    if(incoming_packet <= (bucket_size-store))
    {
      store+=incoming_packet;
      printf("The bucket contains %d/%d bytes\n",store,bucket_size);
    }
    else
    {
      int dropped_packets = incoming_packet - (bucket_size-store);
      store=bucket_size;
      printf("The bucket contains %d/%d bytes (full)\n",store,bucket_size);
      printf("%d bytes of the packet dropped\n",dropped_packets);
    }

    if(store >= out_rate) {
      printf("%d bytes of data transmitted\n",out_rate);
      store-=out_rate;
    }
    else {
      printf("%d bytes of data transmitted\n",store);
      store=0;
    }
    printf("The bucket contains %d/%d bytes after transmission\n\n",store,bucket_size);
    n--;
  }
  while(store!=0){
    if(store >= out_rate) {
      printf("%d bytes of data transmitted\n",out_rate);
      store-=out_rate;
      printf("The bucket contains %d/%d bytes after transmission\n",store,bucket_size);
    }
    else {
      printf("%d bytes of data transmitted\n",store);
      store=0;
      printf("The bucket contains %d/%d bytes after transmission\n",store,bucket_size);
    }
  }
}

```
