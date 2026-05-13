# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls

```
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>

int main()
{
    int fd[2];
    pid_t pid;
    char write_msg[] = "Hello from Parent Process";
    char read_msg[100];

    // Create pipe
    if (pipe(fd) == -1)
    {
        printf("Pipe creation failed\n");
        return 1;
    }

    // Create child process
    pid = fork();

    if (pid < 0)
    {
        printf("Fork failed\n");
        return 1;
    }

    // Parent Process
    if (pid > 0)
    {
        close(fd[0]); // Close read end

        write(fd[1], write_msg, strlen(write_msg) + 1);

        printf("Parent Process Sent: %s\n", write_msg);

        close(fd[1]); // Close write end
    }

    // Child Process
    else
    {
        close(fd[1]); // Close write end

        read(fd[0], read_msg, sizeof(read_msg));

        printf("Child Process Received: %s\n", read_msg);

        close(fd[0]); // Close read end
    }

    return 0;
}
```



## OUTPUT
<img width="1321" height="1191" alt="WhatsApp Image 2026-05-13 at 7 39 21 PM" src="https://github.com/user-attachments/assets/f85c9795-7071-46a9-a802-bc28e3d14e95" />


## C Program that illustrate communication between two process using named pipes using Linux API system calls

```
Create Writer Program

#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main()
{
    int fd;
    char *message = "Hello from Writer Process";

    // Create named pipe
    mkfifo("mypipe", 0666);

    // Open pipe for writing
    fd = open("mypipe", O_WRONLY);

    // Write message into pipe
    write(fd, message, strlen(message) + 1);

    printf("Message Sent: %s\n", message);

    close(fd);

    return 0;
}

Create Reader Program

#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main()
{
    int fd;
    char buffer[100];

    // Open pipe for reading
    fd = open("mypipe", O_RDONLY);

    // Read message from pipe
    read(fd, buffer, sizeof(buffer));

    printf("Message Received: %s\n", buffer);

    close(fd);

    return 0;
}
```



## OUTPUT
<img width="1600" height="742" alt="WhatsApp Image 2026-05-13 at 7 39 21 PM (1)" src="https://github.com/user-attachments/assets/137395c0-57d3-4e34-92ae-17de90eb0c94" />


# RESULT:
The program is executed successfully.
