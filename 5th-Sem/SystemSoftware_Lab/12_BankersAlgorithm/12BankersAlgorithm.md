## Aim:
Design, develop and run a program to implement the Bankers Algorithm. Demonstrate its working with different data values.

## Description:
The Banker's algorithm is a resource allocation and deadlock avoidance algorithm developed by Edsger Dijkstra that tests for safety by simulating the allocation of predetermined maximum possible amounts of all resources, and then makes an "s-state" check to test for possible deadlock conditions for all other pending activities, before deciding whether allocation should be allowed to continue. The Banker's algorithm is run by the operating system whenever a process requests resources. The algorithm avoids deadlock by denying or postponing the request if it determines that accepting the request could put the system in an unsafe state. When a new process enters a system, it must declare the maximum number of instances of each resource type that it may ever claim; clearly, that number may not exceed the total number of resources in the system. Also, when a process gets all its requested resources it must return them in a finite amount of time.

## Code
```
#include <stdio.h>
#include <stdlib.h>

#define true 1
#define false 0

int process,count,m,i,sequence[15];
int n,alloc[10][10],max[10][10],avail[10],work[10],finish[10];
int j,need[10][10],a;

void init();
void readmatrix(int T[10][10]);
void readvector(int V[10]);
void findneed();
void printtable(int T[10][10]);
void selectprocess();
void executeprocess(int p);
void releaseresource();

void main() {
	init();
	findneed();
	do {
		selectprocess();
		executeprocess(process);
		releaseresource();
		finish[process]=true;
	} while(count < m);

	printf("Safe Sequence is:\n");
	for(i = 0; i < m; i++)
		printf("%d\t",sequence[i]);
	exit(0);
}

void init() {
	printf("Enter the no. of processes:\n");
	scanf("%d",&m);
	printf("Enter the no. of resources:\n");
	scanf("%d",&n);
	printf("Enter the Allocation Matrix:\n");
	readmatrix(alloc);
	printf("Enter the Maximum Matrix:\n");
	readmatrix(max);
	printf("Enter the Available Vector:\n");
	readvector(avail);

	for(i=0;i<n;i++)
		work[i]=avail[i];

	for(i=0;i<m;i++)
		finish[i]=false;
}

void readmatrix(int T[10][10]) {
	for(i=0;i<m;i++)
		for(j=0;j<n;j++)
			scanf("%d",&T[i][j]);
}

void readvector(int V[10]) {
	for(i=0;i<n;i++)
		scanf("%d",&V[i]);
}

void findneed() {
	for(i=0;i<m;i++)
		for(j=0;j<n;j++)
			need[i][j]=max[i][j]-alloc[i][j];
	printf("Need Matrix is:\n");
	printtable(need);
}

void printtable(int T[10][10]) {
	for(i=0;i<m;i++) {
		for(j=0;j<n;j++)
			printf("%d\t",T[i][j]);
		printf("\n");
	}
}

void selectprocess() {
	int flag;
	for(i=0;i<m;i++) {
		for(j=0;j<n;j++) {
			if(need[i][j]<=work[j])
				flag=1;
			else {
				flag=0;
				break;
			}
		}

		if(flag==1 && finish[i]==false) {
			process=i;
			count++;
			break;
		}
	}

	if(flag==0)	{
		printf("System is Unsafe!\n");
		exit(0);
	}
}

void executeprocess(int p) {
	printf("%d executed!\n", p);
	sequence[a++]=p;
}

void releaseresource() {
	for(j=0;j<n;j++)
		work[j]=work[j]+alloc[process][j];
}
```

## Output
```
Enter the no. of processes:
5
Enter the no. of resources:
3
Enter the Allocation Matrix:
0 1 0
2 0 0
3 0 2
2 1 1
0 0 2
Enter the Maximum Matrix:
7 5 3
3 2 2
9 0 2
2 2 2
4 3 3
Enter the Available Vector:
3 3 2
Need Matrix is:
7       4       3
1       2       2
6       0       0
0       1       1
4       3       1
1 executed!
3 executed!
0 executed!
2 executed!
4 executed!
Safe Sequence is:
1       3       0       2       4
```
