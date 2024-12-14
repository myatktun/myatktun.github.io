===
CTF
===

1. `Basics`_
2. `PWN College`_
3. `Additional Resources`_

`back to top <#ctf>`_

Basics
======

*

`back to top <#ctf>`_

PWN College
===========

* `Program Interaction`_

Program Interaction
-------------------
    * from Optional Refreshers Dojo
    * **Level 99**
        - run the python script and enter the calculation results
        - other method would be to write a script with threads that read from stdout,
          calculate the operations, and write to stdin

        .. code-block:: python

           import subprocess
   
           subprocess.run("/challenge/run")


    * **Level 100**
        - run the python script and enter the calculation results
        - other method would be to write a script with threads that read from stdout,
          calculate the operations, and write to stdin

        .. code-block:: python

           import subprocess
   
           subprocess.run("/challenge/run")


    * **Level 101**
        - make a link to ``/challenge/run`` with required file name and run the python script
        - calling to the linked file instead of the challenge file will set the ``argv[0]`` value

        .. code-block:: python

           import subprocess
   
           subprocess.run("linked_file_name")


    * **Level 103**
        - make a fifo, create a thread to write to the fifo and open it to redirect it to the
          process stdin

        .. code-block:: python

           def write_to_fifo(fifo_path):
               with open(fifo_path, 'w') as f:
                   f.write('required_text')
                   f.flush()
   
   
           def main():
               FIFO_PATH = '/tmp/fifo'
   
               if not os.path.exists(FIFO_PATH):
                   os.mkfifo(FIFO_PATH)
   
               writer_thread = threading.Thread(target=write_to_fifo, args=(FIFO_PATH,))
               writer_thread.start()
   
               with open(FIFO_PATH, 'r') as f:
                   subprocess.run('/challenge/run', stdin=f)
   
               writer_thread.join()
               os.remove(FIFO_PATH)


    * **Level 104**
        - similar to Level 103
        - make a fifo, create a thread to write to the fifo and open it to redirect the
          process stdout

        .. code-block:: python

           def main():
               # modified part
               with open(FIFO_PATH, 'r') as f:
                   subprocess.run('/challenge/run', stdout=f)


    * **Level 105**
        - similar to Level 103
        - make a fifo, create a thread to write to the fifo and open it to redirect the
          process stdout and stdin

        .. code-block:: python

           def main():
               # modified part
               with open(FIFO_PATH, 'r') as f:
                   subprocess.run('/challenge/run', stdin=f, stdout=f)


    * **Level 113**
        - fork a child and use ``execve()``, do not forget for ``waitpid()``

        .. code-block:: c

           void pwncollege()
           {
                   pid_t pid = fork();
   
                   if (pid == -1) {
                           perror("child: fork");
                           exit(1);
                   }
   
                   if (pid == 0) {
                           char* args[] = {"/challenge/run", NULL};
                           if (execve(args[0], args, NULL) == -1) {
                                   perror("child: execve");
                                   exit(1);
                           }
                   }
                   else {
                           int status;
                           waitpid(pid, &status, 0);
                   }
           }


    * **Level 114**
        - similar to Level 113, but cannot set challenge file as ``args[0]``
        - use ``fork()``, ``execve()``, ``waitpid()``, and pass the required name as argument to ``execve()``

        .. code-block:: c

           void pwncollege()
           {
                   if (pid == 0) {
                           // modified part
                           char* args[] = {"required_text"};
                           if (execve("/challenge/run", args, NULL) == -1) {
                                   perror("child: execve");
                                   exit(1);
                           }
                   }
           }


    * **Level 115**
        - same as Level 114, use ``fork()``, ``execve()``, ``waitpid()``, and pass the required name
          as argument to ``execve()``
    * **Level 116**
        - create a fifo with necessary permissions, use ``mkfifo()``, ``fork()``, ``execve()``, and
          ``waitpid()``
        - open the fifo in child and use ``dup2()`` to redirect stdin
        - open fifo in child with ``O_RDONLY``, and ``O_WRONLY`` in parent
        - make sure to close and unlink fifo

        .. code-block:: c

           void pwncollege()
           {
                   char* fifo_path = "/tmp/my_fifo";
                   pid_t pid;
                   int   fd;
   
                   if (mkfifo(fifo_path, 0666) == -1) {
                           perror("child: mkfifo");
                           exit(1);
                   }
   
                   pid = fork();
   
                   if (pid == -1) {
                           perror("child: fork");
                           exit(1);
                   }
   
                   if (pid == 0) {
                           fd = open(fifo_path, O_RDONLY);
   
                           if (fd == -1) {
                                   perror("child: open fifo");
                                   exit(1);
                           }
   
                           if (dup2(fd, STDIN_FILENO) == -1) {
                                   perror("child: dup2");
                                   exit(1);
                           }
   
                           close(fd);
   
                           if (execve("/challenge/run", NULL, NULL) == -1) {
                                   perror("child: execve");
                                   exit(1);
                           }
                   }
                   else {
                           int status;
                           const char* msg = "required_text";
                           fd = open(fifo_path, O_WRONLY);
   
                           if (fd == -1) {
                                   perror("parent: open fifo");
                                   exit(1);
                           }
   
                           write(fd, msg, strlen(msg));
                           close(fd);
   
                           waitpid(pid, &status, 0);
                           unlink(fifo_path);
                   }
           }


    * **Level 117**
        - similar to Level 116, use ``mkfifo()``, ``fork()``, ``execve()``, ``waitpid()``, ``dup2()`` and
          redirect stdout
        - open fifo in child with ``O_WRONLY``, and ``O_RDONLY`` in parent

        .. code-block:: c

           void pwncollege()
           {
                   if (pid == 0) {
                           // modified part
                           fd = open(fifo_path, O_WRONLY);
   
                           // modified part
                           if (dup2(fd, STDOUT_FILENO) == -1) {
                                   perror("child: dup2");
                                   exit(1);
                           }
                   }
                   else {
                           // modified part
                           char buffer[1024];
                           fd = open(fifo_path, O_RDONLY);
   
   
                           // modified part
                           ssize_t num_bytes = read(fd, buffer, sizeof(buffer) - 1);
                           if (num_bytes == -1) {
                                   perror("parent: read fifo");
                           }
                           else {
                                   buffer[num_bytes] = '\0';
                                   printf("output: %s\n", buffer);
                           }
                   }
           }


    * **Level 118**
        - similar to combination of Level 116 and 117, but only need to write to fifo from
          parent, like Level 116
        - use ``mkfifo()``, ``fork()``, ``execve()``, ``waitpid()``, ``dup2()`` and redirect stdin and
          stdout of child process, but open the fifo as read-only, otherwise it is blocked
        - in the parent process, open the fifo as write-only and write the text to it
        - parent or child opening the fifo with ``O_RDWR`` will get blocked, as the fifo never
          reaches EOF condition
        - can also use semaphores, shared memory, message queues, file locks (``flock()``,
          ``fcntl()``), or signals (``signal()``, ``kill()``)

        .. code-block:: c

           void pwncollege()
           {
                   if (pid == 0) {
                           // modified part
                           if (dup2(fd, STDIN_FILENO) == -1) {
                                   perror("child: dup2 stdin");
                                   exit(1);
                           }
   
                           // modified part
                           if (dup2(fd, STDOUT_FILENO) == -1) {
                                   perror("child: dup2 stdout");
                                   exit(1);
                           }
                   }
           }


    * **Level 119**
        - similar to Level 118
        - in the parent process, open the fifo as write-only, get input from using ``fgets()``,
          and write the input to the fifo

        .. code-block:: c

           void pwncollege()
           {
                   if (pid == 0) {
                           // child process things
                   }
                   else {
                           // modified part
                           char input[128];
   
                           fd = open(fifo_path, O_WRONLY);
   
                           if (fd == -1) {
                                   perror("parent: open fifo");
                                   exit(1);
                           }
                           fgets(input, sizeof(input), stdin);
   
                           write(fd, input, strlen(input));
                   }
           }


    * **Level 120**
        - create the file first and write necessary text in it, can use ``touch`` & ``echo``
        - open the file in child and use ``dup2()`` to required target file descriptor

        .. code-block:: c

           void pwncollege()
           {
                   char* file_path = "/tmp/my_file";
                   int   fd;
                   pid_t pid;
   
                   pid = fork();
   
                   if (pid == -1) {
                           perror("child: fork");
                           exit(1);
                   }
   
                   if (pid == 0) {
                           fd            = open(file_path, O_RDONLY);
                           int target_fd = 159;
   
                           if (fd == -1) {
                                   perror("child: open");
                                   close(fd);
                                   exit(1);
                           }
   
                           if (dup2(fd, target_fd) == -1) {
                                   perror("child: dup2");
                                   close(fd);
                                   exit(1);
                           }
                           close(fd);
   
                           if (execve("/challenge/run", NULL, NULL) == -1) {
                                   perror("child: execve");
                                   exit(1);
                           }
                           close(target_fd);
                   }
                   else {
                           int status;
                           waitpid(pid, &status, 0);
                   }
           }


    * **Level 121**
        - same as Level 113, just using ``execve()`` seems to be working
        - does not check for any file descriptors
    * **Level 122**
        - same as Level 120, use ``touch`` & ``echo`` to create file with necessary data first
    * **Level 123**
        - create a child process like Level 123
        - get signal number input and send it to the child, use ``fgets()``, ``strtol()`` and ``kill()``

        .. code-block:: c

           void pwncollege()
           {
                   // child process code
                   else {
                           int   status;
                           char  line[4];
                           char* endptr;
                           if (fgets(line, sizeof(line), stdin) != NULL) {
                                   int sig = strtol(line, &endptr, 10);
                                   kill(pid, sig);
                           }
                           waitpid(pid, &status, 0);
                   }
           }


    * **Level 124**
        - same as Level 123, but get input and send signal specific times

        .. code-block:: c

           void pwncollege()
           {
                   // child process code
                   else {
                           int   status;
                           char  line[4];
                           char* endptr;
                           int   count = 0;
                           while (count < 5) {
                                   memset(&line, 0, sizeof(line));
                                   if (fgets(line, sizeof(line), stdin) != NULL) {
                                           int sig = strtol(line, &endptr, 10);
                                           kill(pid, sig);
                                           ++count;
                                   }
                           }
                           waitpid(pid, &status, 0);
                   }
           }


    * **Level 130**
        - same as the [code](#read-stdout-write-stdin) from [Additional Resources](#additional-resources)
    * **Level 131**
        - same as the [code](#read-stdout-write-stdin) from [Additional Resources](#additional-resources)
    * **Level 135**
        - create two global shared pipes, and a thread to read and write to child process
        - use ``pipe()``, ``dup2()``, ``pthread_create()``, and ``pthread_join()``

        .. code-block:: c

           #define MAXBUFLEN 1024
   
           int pipe_stdout[2];
           int pipe_stdin[2];
   
           void pwncollege()
           {
                   pid_t     pid;
                   pthread_t process_child_thread;
   
                   if (pipe(pipe_stdout) == -1 || pipe(pipe_stdin) == -1) {
                           perror("pipe");
                           exit(1);
                   }
   
                   pid = fork();
   
                   if (pid == -1) {
                           perror("child: fork");
                           exit(1);
                   }
   
                   if (pid == 0) {
                           dup2(pipe_stdout[1], STDERR_FILENO);
                           close(pipe_stdout[0]);
                           close(pipe_stdout[1]);
   
                           dup2(pipe_stdin[0], STDIN_FILENO);
                           close(pipe_stdin[0]);
                           close(pipe_stdin[1]);
   
                           char* args[] = {"/challenge/run", NULL};
                           if (execve(args[0], args, NULL) == -1) {
                                   perror("child: execve");
                                   exit(1);
                           }
                   }
                   else {
                           close(pipe_stdout[1]);
                           close(pipe_stdin[0]);
   
                           int status;
   
                           pthread_create(&process_child_thread, NULL,
                                          process_child_output, NULL);
   
                           pthread_join(process_child_thread, NULL);
   
                           close(pipe_stdout[0]);
   
                           waitpid(pid, &status, 0);
                   }
           }
   
           void* process_child_output(void* arg)
           {
                   char    buffer[MAXBUFLEN];
                   ssize_t n;
                   char*   pfound;
                   char*   search_string = "CHALLENGE! Please send the solution for: ";
   
                   while ((n = read(pipe_stdout[0], buffer, sizeof(buffer) - 1)) > 0) {
                           buffer[n] = '\0';
                           if ((pfound = strstr(buffer, search_string)) != NULL) {
                                   pfound += strlen(search_string);
                                   printf("!!!!To Calculate: %s\n", pfound);
                                   snprintf(buffer, sizeof(buffer),
                                            "python -c 'print(%s)'", pfound);
   
                                   FILE* python_output = popen(buffer, "r");
                                   if (python_output == NULL) {
                                           perror("popen");
                                           exit(1);
                                   }
   
                                   memset(buffer, 0, sizeof(buffer));
                                   if (fgets(buffer, sizeof(buffer), python_output) !=
                                       NULL) {
   
                                           write(pipe_stdin[1], buffer, strlen(buffer));
                                   }
                                   pclose(python_output);
                           }
                           else {
                                   printf("!!!!Child Output: %s\n", buffer);
                           }
                   }
   
                   close(pipe_stdin[1]);
                   return NULL;
           }


    * **Level 136**
        - same as Level 135, but use at least 1024\*7 bytes for ``MAXBUFLEN``
        - use ``pipe()``, ``dup2()``, ``pthread_create()``, and ``pthread_join()``
    * **Level 140**
        - use ``exec 3<>`` to use file descriptor 3 to open bidirectional connection to the
          server
        - can also use other available file descriptors, but must be hardcoded
        - use ``eval "exec $FD<>`` to use with a variable
        - use ``<&3`` to read and ``>&3`` to write
        - use ``exec 3>&-`` to close the file descriptor and the connection

        .. code-block:: sh

           #!/bin/bash
   
           command="/challenge/run"
   
           $command &
           sleep 5s
   
           HOST="localhost"
           PORT=1737
   
           exec 3<>/dev/tcp/$HOST/$PORT
   
           read_response() {
               while read -r line <&3; do
                   echo "Server response: $line"
               done
           }
   
           read_response &
   
           while true; do
               echo -n "Enter message: "
               read message
               echo "$message" >&3
   
               if [[ "$message" == "exit" ]]; then
                   break
               fi
           done
   
           exec 3>&-


    * **Level 141**
        - run the challenge in the background
        - create a Python TCP client using socket module
        - can directly calculate the challenge received from the server
        - make sure to add newline at the end before sending to the server

        .. code-block:: python

           import socket
   
   
           def main():
               HOST = "localhost"
               PORT = 1865
               # address family: a pair of (host, port) for AF_INET
               addr = (HOST, PORT)
   
               client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   
               client_socket.connect(addr)
               print(f"Connected to {HOST} on port {PORT}")
   
               while True:
                   recv_bytes = client_socket.recv(1024)
                   if len(recv_bytes) > 0:
                       data = recv_bytes.decode('utf-8')
                       print(f"Server send: {data}")
                       if "CHALLENGE" in data:
                           values = data.strip().split(':')[-1]
                           print(f"calculate: {values}")
                           answer = str(eval(values)) + '\n'
                           print(f"Sending: {answer}")
                           sent_data = client_socket.send(answer.encode("utf-8"))
                           print(f"Total send: {sent_data}")
                   else:
                       print("Server close connection")
                       break


`back to top <#ctf>`_

Additional Resources
====================

* `Read stdout & Write stdin`_

Read stdout & Write stdin
-------------------------
    * in level 99 and 100 of pwn.college Program Interaction, formulas from stdout can be read,
    calculated, and written to stdin
    * below is a python script that automate calculations, for simplicity it uses ``eval()``

    .. code-block:: python

       import subprocess
       import threading
   
   
       def main():
           # Command to execute an interactive Python shell
           command = ['/challenge/run']
   
           # Condition variable for synchronizing threads
           condition = threading.Condition()
           # Shared variable to store if the condition for writing input is met
           write_input_flag = {'should_write': False}
           # Shared variable to store the last line of output
           shared_output = {'line': ''}
           # Shared variable to indicate if the process is running
           process_running = {'running': True}
   
           # Open subprocess in a pipe for stdin, stdout, and stderr
           process = subprocess.Popen(command,
                                      stdin=subprocess.PIPE,
                                      stdout=subprocess.PIPE,
                                      stderr=subprocess.PIPE,
                                      text=True,  # Ensure text mode for Python 3
                                      bufsize=1,  # Line-buffered
                                      universal_newlines=True)  # Cross-platform newline handling
   
           try:
               # Start threads to read stdout and stderr
               stdout_thread = threading.Thread(target=read_output, args=(
                   process.stdout, condition, write_input_flag, shared_output, process_running))
               stderr_thread = threading.Thread(target=read_output, args=(
                   process.stderr, condition, write_input_flag, shared_output, process_running))
               stdout_thread.start()
               stderr_thread.start()
   
               # Start thread to write input
               input_thread = threading.Thread(target=write_input, args=(
                   process.stdin, condition, write_input_flag, shared_output, process_running))
               input_thread.start()
   
               # Wait for the subprocess to complete
               process.wait()
               process_running['running'] = False
   
               # Notify all threads to exit
               with condition:
                   condition.notify_all()
   
               # Wait for threads to complete
               stdout_thread.join()
               stderr_thread.join()
               input_thread.join()
   
           except KeyboardInterrupt:
               print("\nProcess terminated by user.")
           finally:
               # Close stdin to ensure the subprocess can terminate
               process.stdin.close()
   
   
       def read_output(stream, condition, write_input_flag, shared_output, process_running):
           target_value = "CHALLENGE! Please send the solution"  # Set your target value here
           while process_running['running']:
               line = stream.readline()
               if not line:
                   break
               print(f"Received: {line.strip()}")
               if target_value in line:
                   with condition:
                       shared_output['line'] = line.strip()  # Store the output line
                       write_input_flag['should_write'] = True
                       condition.notify()
           # Ensure all remaining lines are processed after process ends
           while True:
               line = stream.readline()
               if not line:
                   break
               print(f"Received: {line.strip()}")
               if target_value in line:
                   with condition:
                       shared_output['line'] = line.strip()  # Store the output line
                       write_input_flag['should_write'] = True
                       condition.notify()
   
   
       def write_input(stream, condition, write_input_flag, shared_output, process_running):
           try:
               while process_running['running']:
                   with condition:
                       condition.wait_for(
                           lambda: write_input_flag['should_write'] or not process_running['running'])
                       if not process_running['running']:
                           break
                       # Process the last line of output
                       processed_input = process_output(shared_output['line'])
                       # Write processed input to the subprocess
                       stream.write(processed_input + '\n')
                       stream.flush()
                       # Reset the flag
                       write_input_flag['should_write'] = False
           except EOFError:
               pass
   
   
       def process_output(output_line):
           values = output_line.strip().split(':')[-1]
           print(f"calculate: {values}")
           return str(eval(values))
   
   
       if __name__ == "__main__":
           main()


`back to top <#ctf>`_
