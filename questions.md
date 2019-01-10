# Questions

## HPC good citizenship

1. On the UCI cluster, the resource request "-pe openmp 64" refers to the number of processors requested.  Does that
   request mean that your commands will use multiple processors?

answer: no some commands do not use multiple processors. To note: some commands/programs allow you to choose the number of processors that are used.  So the program would use the number of processors chosen by the user in the command line.  So if you request 64 processors "-pe openmp 64", but only declare 20 in the command line script the command will use 20 processors.  

2. In general, how do you know how many processors, how much RAM, how many files would be required/needed/written by the
   jobs you are running on the cluster?

answer: Before running a program you can determine the ballpark RAM, # of files, etc. by reading the a program manual (if there is one) or googling the info online for a particular command.  However, this info many times is not available online.  Before running a command or program for the first time using your own data you should use a smaller test data set and run a prefix command before the program/command you want to run. You will most likely have to scale up the amount of RAM needed to match the size of your dataset. The prefix command is:

```bash
/usr/bin/time -v
```  

3. In order to be a "good citizen", you need to have some idea of much RAM your job requires.  In particular, you need
   to know the "peak" (i.e., maximum) RAM required at any point during execution.  Show an example of the shell command
   that you would use on a Linux machine to measure run time and peak ram usage of an arbitrary command, where the time/peak RAM values are written to a file.
answer:
The shell command I would use would be:
```bash
/usr/bin/time -o stats.txt my_program
```

4. What are the units of your answer for number 3? 
answer: kbytes
5. What are the bash commands for the following operations:

    * Checking that a file exists: 
answer:
```bash
[ -f  test.txt ] && echo "yes" || echo "no"
```

   * Checking if a file exists and is not empty
answer:
```bash
[ -s test.txt ] && echo "exists and not empty" || echo "empty or doesn't exist"
```

6. How would you use the commands from your answer to 5 to write a work flow for HPC that only runs a job if the
   expected output file is **not** present.
```bash
 [ -f test.txt ] || sh test_script.sh
```

## Trickier questions

7. Would your answer to number 3 work on Apple's OS X operating system?  If no, do you have any idea why not? 

answer: no, /usr/bin/time is linux specific. There is another program called time on OS X that is a different program and gives a different output.

8. Most of the HPC nodes have 512Gb (gigabytes) of RAM. Let's say you have a job that will require **no more** than 24Gb
   of RAM.  How would you request resources so that you can run more than one job on a node **and** not cause nodes to
   crash?  Show an example of a skeleton HPC script as part of your answer.  **Knowing how to do this is super important
   and will save you loads of frustration and prevent you from taking out your colleagues' jobs on the cluster,
   preventing you from getting nasty emails from Harry!!!!!!!!!!!**

```bash
#!/bin/bash

#$ -N log
#$ -q bio
#$ -l node=name_of_node
#$ -l h_vmem=24GB

echo "Write your commands here"
```

after submitting this script you can submit another shell script using #$ -l node=name_of_node by specifying the same node using #$ -l h_vmem=RAM# as long as the h_vmem does not add up to over the amount of RAM the node has.
