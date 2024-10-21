### Cloud Computing - Lab 1 Solutions 

### Introduction

This lab focuses on understanding and using the Linux command-line terminal. The exercises are performed in the Google Cloud Shell, where you can execute commands directly in a cloud-based environment.

### How to Access the Google Cloud Shell

To access the Google Cloud Shell, ensure you have a Gmail account and log in at [Google Cloud Shell](https://shell.cloud.google.com). Your shell session will persist as long as it’s active.

### Setup and First-Time Configuration

- **Remove the default project ID from the shell prompt**:

    ```bash
    gcloud config unset project
    ```

- **Restore man pages**:
  
    If the `man` command is not working, you can run the following script to restore manual pages in the shell:

    ```bash
    sudo sed -i '/path-exclude/s/^/#/' /etc/dpkg/dpkg.cfg.d/excludes
    sudo apt-get --reinstall install man-db manpages manpages-dev vim -y
    sudo apt-get install manpages-posix manpages-posix-dev -y
    sudo mv /usr/bin/man.REAL /usr/bin/man
    sudo mandb -c
    ```

---

### File Permission Exercises

1. **Create a new file called `test.txt` using the `touch` command.**

   **Answer:**
   ```bash
   $ touch test.txt
   ```

2. **What are the permissions of the file `test.txt`?**

   **Answer:**
   ```bash
   -rw-r--r--
   ```

#### Changing File Permissions Using Opcode Mode

3. **Allow the owner read access to the file and deny everyone else access.**

   **Answer:**
   ```bash
   $ chmod u+r,go-rw test.txt
   OR
   $ chmod u=r,go= test.txt
   ```

4. **Allow the owner and world to read the file but deny all access to the group.**

   **Answer:**
   ```bash
   $ chmod uo=r,g= test.txt
   ```

5. **Deny access to the file for the owner, group, and world.**

   **Answer:**
   ```bash
   $ chmod ugo= test.txt
   ```

6. **Grant the owner read and write access, and grant the group and world read-only access.**

   **Answer:**
   ```bash
   $ chmod u=rw,go=r test.txt
   ```

#### Changing File Permissions Using Octal Mode

7. **Allow the owner read access to the file and deny everyone else access.**

   **Answer:**
   ```bash
   $ chmod 400 test.txt
   ```

8. **Allow the owner and world to read the file but deny all access to the group.**

   **Answer:**
   ```bash
   $ chmod 404 test.txt
   ```

9. **Deny access to the file for the owner, group, and world.**

   **Answer:**
   ```bash
   $ chmod 000 test.txt
   ```

10. **Grant the owner read and write access, and grant the group and world read-only access.**

   **Answer:**
   ```bash
   $ chmod 644 test.txt
   ```

---

### File and Text Exercises

1. **Create a file called `capital.txt` with the specified content.**
   
   **Command:**
   ```bash
   $ echo -e "Dublin is the capital city of Ireland.\nIreland is viewed as the best country in the world by many people.\nDublin has a population greater than 1 million people.\nThere are many places to see and visit in Dublin.\nThe largest university in Ireland is TU Dublin." > capital.txt
   ```

2. **Create a file called `cities.txt` with the following content:**

   **Command:**
   ```bash
   $ echo -e "Dublin\nCork\nBelfast\nGalway\nWaterford\nKilkenny" > cities.txt
   ```

3. **Display the contents of the file `capital.txt`.**

   **Answer:**
   ```bash
   $ cat capital.txt
   ```

4. **Copy the contents of `capital.txt` and `cities.txt` into a new file `join.txt`.**

   **Answer:**
   ```bash
   $ cat capital.txt cities.txt > join.txt
   ```

5. **Display the count of the number of lines in `capital.txt`.**

   **Answer:**
   ```bash
   $ wc -l < capital.txt
   ```

6. **Display the count of the number of words in `capital.txt`.**

   **Answer:**
   ```bash
   $ wc -w < capital.txt
   ```

7. **Count and display the number of bytes in `capital.txt`.**

   **Answer:**
   ```bash
   $ wc -c < capital.txt
   ```

8. **Display a listing of folders in your home folder.**

   **Answer:**
   ```bash
   $ ls -l ~ | grep ^d
   ```

9. **Display a listing of files, including hidden files, in your home folder.**

   **Answer:**
   ```bash
   $ ls -al ~ | grep ^-
   ```

10. **Display the cities in `cities.txt` in sorted order.**

    **Answer:**
    ```bash
    $ sort cities.txt
    ```

11. **Create a new file called `sortedcities.txt` with the sorted cities.**

    **Answer:**
    ```bash
    $ sort cities.txt > sortedcities.txt
    ```

12. **Display the last two lines in `capital.txt`.**

    **Answer:**
    ```bash
    $ tail -2 capital.txt
    ```

13. **Display the first three lines in `capital.txt`.**

    **Answer:**
    ```bash
    $ head -3 capital.txt
    ```

14. **Display the lines in `capital.txt` containing the word "Dublin".**

    **Answer:**
    ```bash
    $ grep Dublin capital.txt
    ```

15. **Display the lines in `capital.txt` containing "dublin" (case-insensitive).**

    **Answer:**
    ```bash
    $ grep -i dublin capital.txt
    ```

16. **Display the lines in `capital.txt` that do not contain "dublin" (case-insensitive).**

    **Answer:**
    ```bash
    $ grep -iv dublin capital.txt
    ```

17. **Count the lines in `capital.txt` that do not contain "dublin".**

    **Answer:**
    ```bash
    $ grep -ivc dublin capital.txt
    ```

18. **Display the line number(s) in `cities.txt` containing "cork" (case-insensitive).**

    **Answer:**
    ```bash
    $ grep -in cork cities.txt | cut -d':' -f1
    ```

19. **Display just the first character of each line in `cities.txt`.**

    **Answer:**
    ```bash
    $ cut -c1 cities.txt
    ```

20. **Display the second-to-last line of `capital.txt`.**

    **Answer:**
    ```bash
    $ tail -2 capital.txt | head -1
    ```

21. **Display the output of `ps -aux` and redirect it to `processes.txt`.**

    **Answer:**
    ```bash
    $ ps -aux | tee processes.txt
    ```

22. **Create a file called `numbers.txt` with numbers from 1 to 4.**

    **Answer:**
    ```bash
    $ seq 4 > numbers.txt
    ```

23. **Append numbers 1 to 6 to `numbers.txt`.**

    **Answer:**
    ```bash
    $ seq 6 >> numbers.txt
    ```

24. **Display the contents of `numbers.txt` in ascending order.**

    **Answer:**
    ```bash
    $ sort -n numbers.txt
    ```

25. **Display the contents of `numbers.txt` in ascending order with duplicates removed.**

    **Answer:**
    ```bash
    $ sort -n numbers.txt | uniq
    ```

26. **Display how many times each line is duplicated in `numbers.txt`.**

    **Answer:**
    ```bash
    $ sort -n numbers.txt | uniq -c
    ```

27. **Display only the unique (non-duplicate) lines in `numbers.txt`.**

    **Answer:**
    ```bash
    $ sort -n numbers.txt | uniq -u
    ```

---

### Exercises on Processes

1. **Terminate process P1 without using the `kill` command.**

   **Answer:**
   ```bash
   $ jobs
   $ fg %jobnumber
   Press Ctrl+C
   ```

2. **Identify the PID of process P2.**

   **Answer:**
   ```bash
   $ ps -x | grep "sleep 2000"
   ```

3. **Terminate process P2 using the `kill` command.**

   **Answer:**
   ```bash
   $ kill PID
   ```

4. **Suspend process P3.**

   **Answer:**
   ```bash
   $ fg %jobnumber
   Press Ctrl+Z
   ```

5. **Bring process P4 to the foreground and then terminate it.**

   **Answer:**
   ```bash
   $ ps -x | grep "sleep 4000"
   $ kill PID
   ```

6. **Terminate process P5 by force using the `kill` command.**

   **Answer:**
   ```bash
   $ ps -x | grep "sleep 5000"
   $ kill -9 PID
   ```

7. **Terminate process P3 without using the `kill` command.**

   **Answer:**
   ```bash
   $ fg %jobnumber
   Press Ctrl+C
   ```