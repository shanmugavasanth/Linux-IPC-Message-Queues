# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them

```
// C Program for Message Queue (Writer Process)

#include <stdio.h> 
#include <sys/ipc.h> 
#include <sys/msg.h> 

struct mesg_buffer
{ 
	long mesg_type; 
	char mesg_text[100]; 
} message;

int main() 
{
  key_t key; 
	int msgid;
	key = ftok("progfile", 65);
	msgid = msgget(key, 0666 | IPC_CREAT); 
	message.mesg_type = 1; 
	printf("Write Data : "); 
	gets(message.mesg_text); 
	msgsnd(msgid, &message, sizeof(message), 0); 
	printf("Data send is : %s \n", message.mesg_text); 
	return 0; 
}

// C Program for Message Queue (Reader Process)

#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

struct mesg_buffer
{
	long mesg_type;
	char mesg_text[100];
} message;

int main()
{
	key_t key;
	int msgid;
	key = ftok("progfile", 65);
	msgid = msgget(key, 0666 | IPC_CREAT);
	msgrcv(msgid, &message, sizeof(message), 1, 0);
	printf("Data Received is : %s \n", message.mesg_text);
	msgctl(msgid, IPC_RMID, NULL);
	return 0;
}
```

## OUTPUT

```
Write Data : Helloworld
Data send is : Helloworld
```

```
Data Received is : Helloworld 
```

```
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages    
0xffffffff 720896     admin    666        560          5           

------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status      
0x00000000 262144     admin       600        33554432   2          dest         
0x00000000 360449     admin       600        524288     2          dest         
0x00000000 688130     admin       600        524288     2          dest         
0x00000000 622595     admin       600        524288     2          dest         
0x00000000 786436     admin       600        524288     2          dest         
0x00000000 655365     admin       600        524288     2          dest         
0x00000000 983046     admin       600        524288     2          dest         

------ Semaphore Arrays --------
key        semid      owner      perms      nsems    
```

# RESULT:
The programs are executed successfully.
