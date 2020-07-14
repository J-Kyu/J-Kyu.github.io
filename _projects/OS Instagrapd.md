---
short_name: Instagrapd
name: OS Instagrapd
date: 2020-07-08
summary: Operating System Programming Assignment  Instagrapd
youtubeID: 1QceiWtrsD4
thumbnail: ../images/thumbnail_images/thumbnail_instragrapd.png
---

# OS: Instagrapd

> * Handong Global Unversity: ITP30002
>
> * Operating System Programming Assignment 
> * Team 
>   * YeJin Yang 21700431
>   * SangKyu Jeon 21500629


1. Problem Ananlysis
2. Work Flow
3. Funtionalities
4. Conclusion
   

## 1. Problem Analysis

1. We didn't really understand what it meant to connect to the instagram properly. So we assumed that a single Socket would require multiple send and receives until the proper answer is sent.

   ​	-> Revision : Submitter recreates the sockets several times and repeatedly connects with a instagrapd.

2. We thought that global variables are shared between child process and parent process, then id and pw would have accumulated data with global variables. 

   ​	-> Revision : Whenever a child process is created, the parent process creates a thread for storing user information to share information about users between two processes.

3. We also thought that 10 test cases were tested in worker with multithread. 

   ​	-> Revision : Worker builds a test file and tests only one test case at a time.

## 2. WorkFlow

<center>
	<img src="../images/OS_ Instagrapd_1.png" alt="OS_ Instagrapd_1" style="zoom:100%;" />
</center>

- Submitter - > instagramd

  1. Submitter continues to communicate with a instagrapd through socket until the result arrives.
  2. The first time Submitter sends a request, it sends id, password, and file contents. 

- Instagrapd 

  1. If the request sent by the submitter is received, the submitter will communicate with the child process of instagrapd.

  2. The parent process of Instagrapd creates a thread that stores the id and pw of the submitter and a thread that stores the result of the build.

  3. Child process examines that id and password received from the user are on the table.

     3-1) If the table does not contain the id and password,
     As the user requests for the first time, Instagrapd stores id and password to the user table. Instagrapd notifies the submitter that the result has not yet arrived, and executes the connectWorker function. Save the results of the 10 test cases from connectWorker function to the table

     ​	3-2) If id and password are registered on the table

     ​		3-2-1)and ‘build result’ is not NULL, 

     ​		 Send the ‘build result’ to submillimeter. 

     ​		3-2-2) and ‘build result’ is NULL,
      Instagrapd notifies the submitter by sending the message "none" that the result has not yet arrived from the worker.

  4. The connectWorker function communicates with the worker through a socket.
     Make 10 sockets in total and send test case and testfile to each connection by pair. 

     ​	 4-1) Compare results from the worker and n.out to count the number of correct answers.
     ​	 4-2) If the result from the worker is “time out” or “build fail”, returns the result.

- Worker -> instagramd

  1. Build the Testfile to the given test case, and send the result to instagrapd.

## 3. Functionaliy



### submitter

> In submitter code section, its main duty is to submit a code to instagrapd. In oderto to send code to instagrapd server, we have to connect through TCP/IP, which is socket programming.

* TCP

​	TCP(Transmission Control Protocol) is a protocol where Internet Protocol defines how computer send packet of data to each other.  In TCP, there is no sustaining connection. One send and one receive. Not only submitter also in Instagrapd and worker, all the connection between isolated programs are linked with TCP/IP. 

```c
sock_fd=socket(AF_INET /*IPv4*/, SOCK_STREAM /*TCP*/, 0 /*IP*/) ;
```

​	When we declare a socket variable, we use socket function to generate new socket. For the  parameter, second parameter, we give “SOCK_STREAM” value in order to set TCP socket. Once we created sock_fd, we are ready to connect with instagrapd. 



​	The next step is to call connect function to connect with server socket. Before we call connect function, we have to create certain values for finding server socket.

~~~c
struct sockaddr_in serv_addr;
memset(&serv_addr, '0', sizeof(serv_addr)); 
serv_addr.sin_family = AF_INET; 
serv_addr.sin_port = htons(8080); 
~~~

​	In this code, we created serv_add variable where its type is sockaddr_in. We use memset to clear all the existing unnecessary values with 0 from serv_addr, and give certain conditions such as address type and ports number. 

~~~c
if (inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr) <= 0) {   //127.0.0.1
        perror("inet_pton failed : ") ; 
        exit(EXIT_FAILURE) ;
    } 
~~~



​	Then we use inet_pton function to convert  IP address and serv_addr variable which are human-readable text in to binary term.

~~~c
connect(sock_fd, (struct sockaddr *) &serv_addr, sizeof(serv_addr)
~~~

​	Finally we call connect function to connect with the server that we added IP number and socket number. At this point, connection is done, now we can send and receive to and from the server socket which is instagrapd. 

​	Some interesting point in submitter is that, even though we use TCP/IP connection, we cannot disconnect from instagrapd just after the end of connection from server, if submitter did not receive promising return values. Therefore, we designed to connect n times of connection with Instagrapd. 

​	In the first connection to Instragrapd, without any chances, submitter will receive “none” string from Instagrapd, because in Instagrapd user table, there is no information of the submitter because it is the first connection. Therefore Instargrapd will return “none”, and register the information to the table, then in Instargrapd, it will connect to Worker for compiling and to get actual return value of the given code from submitter. 

​	After the registration on tables, submitter will infinitely nock to Instragrapd door until Instagrapd return a value which are not “none” string. Once worker return all the outputs of given inputs, Instagrapd will compare with given outputs with output from Instagrapd and will return the result to submitter. 

​	Finally, once submitter successfully received the return value from Instagrapd, submitter will break from while loop and print out the return value.

### Worker

* Build and Pipe


<center>
	<img src="../images/OS_ Instagrapd_2.png" alt="OS_ Instagrapd_2" style="zoom:100%;" />
</center>

​	Store the test case in the variable and the file array in target.c. Create a child process A using fork() that performs the request from the instagrpd. In the child process A, creates a child process B using fork() for executing execution file. A process communicates with the B process through the pipe.B process changes file descriptor of an executable file and builds the executable file. 

​	First of all, replace the Pipes [0] with stdin. Since scanf in C file is received data through stdin, scanf can received a data by writing a data using pipes[1]. In order to print out the results of Target.c in stdout, open the target.out file in write and create mode and replace the file descriptor with stdout.

​	 Likewise, to obtain the Build fail message, replace the file descriptor with stderr.Using the Execl() function, execute the executable file of target.c. The process B is replaced by the process in which the executable file is executed.

​	The process A gives the input to the process B and sends the results to the instagrapd.If the input value is passed through Pipes[1] using a Write() function, then it is transferred to stdin. Parent process waits for the child process to finish.When the Child process is finished, read target.out files and sends the result to instagrapd.

* Singal Handling	

​	Stop the execution if the target program takes more than 3 second. Set the signal handler for SIGALRM to the time_handler. Set the interval of the Itimerval to 3 seconds. Set the timer with the Setimer just before the excutable file is executed, and it counts down the time.  After 3 seconds, the time_handler is invoked and the processB ends.If process A verifies that nothing is written in target.out, it  sends "Time Out" message to instagram.



### Intsagrapd

> In Instagrapd code section main functionality was TCP socket programming with submitter and worker, forking of child process for submitter, and Multi threading for register information on user table and updating user table when worker return actual values. However, in this report section I will only explain about fork and thread because TCP socket programming is already handled in submitter part. 

* Fork

```c
t_pid = fork();	//Forking new process

if(t_pid == 0){    					child_proc(new_socket,workerPort,ip,regPipes[userRecord],worPipes[userRecord]);	//child process duty
            close(new_socket); //once chold_proc function is done, close socket and exit the process
            exit(0);
        }
```



​	Forking is for creating new process. What is very interesting is that, once I call fork(), there are another process running on the computer where the cloned process starts from next line of fork function. Thus, we handle new process by branch statement. 

​	In Instagrapd code, we fork a new process for dealing with different submitter from different socket. Let's say there are A and B clients are trying to submit sample.c for testing and compiling. B has submitted just after A's  submission. For Instagrapd point of view, it is kind of burdensome for dealing two client in concurrently. Therefore, instagrapd clone its child process to deal with A and B clients. What if there are more than 2 clients? Well, same procedures can be done. Instagrapd keep generating new process for each clients. 

​	In here what we should not forget is that, submitter does not wait until the worker return its value. Instagrapd intentionally disconnect, or I say close the socket when worker has not returned its value. In other word, for dealing with one client, instagrapd may have to create several fork process for going obtaining promising return value from worker. 

​	There are some points we may easily miss while we are forking. As I mentioned, forking is cloning from its parent process, and it is usually handle with branch statements. In other other word, once branch for child process is done, it will leave the branch statement and execute next line of codes which we did not intended. In order to prevent from such crisis, we have exit() once child process is done in the branch bracket.

​	One more thing that we have to keep in mind when we do fork is that, although child process copy all the information from parent process, they do not share same memory section. Which means they are isolated process but can be connected with pipes. These pipe concept, where child process temper parents process memory (in our example modifying user table that exists in instagrapd) is done by multi threading with pipe.

* Multi-threading and pipe 

~~~c
//this branch bracket is where parent process creates threads 
        else {
            if(thread_reg == NULL){
                thread_reg = (pthread_t *)malloc(sizeof(pthread_t));
				thread_upd = (pthread_t *)malloc(sizeof(pthread_t));
            }
            else{
                thread_reg = (pthread_t*)realloc(thread_reg (threadCount+2)*sizeof(pthread_t));
				thread_upd = (pthread_t*)realloc(thread_upd,(threadCount+2)*sizeof(pthread_t));
            }
            threadCount++;

			pthread_create(&thread_reg[threadCount-1],NULL,pipe_subRegister,(void *)&regPipes[userRecord]);
			pthread_create(&thread_upd[threadCount-1],NULL,pipe_worUpload,(void *)&worPipes[userRecord]);
          
			pthread_join(thread_upd[threadCount-1] ,NULL);
			pthread_join(thread_reg[threadCount-1] ,NULL);
        }
'''
~~~



​	When submitter is connected to instagrapd, and when instagrapd is ready to connect with worker through socket programming, instragrapd creates 2 threading for each procedure. These threads are created from parent process from instagrapd. The purposes of each threads are one, to register new submitter's information to user table and two, to update the user table once worker has returned its output value to instagrapd. 

​	Two threads are both linked with child process when submitter has connected to instargrapd. They communicate with pipes. The flows of these procedure goes like this. Once submitter has connected to instagrapd, parent process instragrapd will fork new process for submitter and then creates two threads for communicating with child process. 

 	Through socket programming, the child process receive datas from submitter then they check the user table (this procedure is not done through pipe, because once child process is fork from parent process the child process will hold the most recent data, so checking the use table is valid). If none of ID and password exist on the user table, they write on file descriptor where it's been linked through pipes. Then register thread will receive the data, parse it and register on table with return value "NULL" because worker has not been executed yet. Once its been registered on table, the register threads finish its jobs, and the child process will return string "none" to submitter and connect to worker. 

​	After worker finish its job, it will send back the return value through socket to the child process. When the child process receive the return value, they write the return value with submitter's id and password on the different file descriptor which are already determined from parent process. Then, the other thread. the update thread will receive the data from the pipe and parse the id, password and return value to search on the table and update the return value. 

​	In instragrapd code, threads are dynamically allocated. Therefore, for every submitter there are at least two threads are alway with the child process. If there are more submitters, then there will be more threads which we call multi threads.

## Conclusion

​	After all, what has left on us is knowledge of socket programming, fork for new processes, pipe, concept of file descriptor, siganl handling and more and more things. However, there are so many spots on code that we want to fix. Since this programming assignment were based on socket programming transferring datum occurs very reqeuntly. Allocating and free the variable of char pointer was sort of diffucilities in PA2. We have encountered so many segments fault and back trace error messages. From this view, we learned handling data size should be done wisely.

​	Moreover, our debuging skill has improved so much. When we see segments fault error, it made us easily lethargic. Since segments faults does not show us which lines are error codes. Therefore,  we have to find step by step. We learned that segments fault are usually occured with lack of memory and when accessing to none permitted memory. According to these experience we could overcome segments fault and back trace error messages. 

​	However, out of all the experience, socket programming and making new process and creating multi threads was very new and gave us new view points on programming. In the past, when we program with only one thread and one process it feels more like only vertical or top down flow. However, after PA2, programming came to us more like vertical and horizontal view. By creating multiple processes, certain procedures become simpler and faster. Threads can be added to support process and they could be communicated with pipeps. In conclusion, such concept gave us impression of programming can be tunned better and should be desinged more specific in order to make program meaningful and bigger. To sum up, through this programming assignment 2,  when programming becomes  bigger and wider communication and details becomes also crucial for its completion.
