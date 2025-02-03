### This tutorial is taken from the FIU Genomics & Bioinformatics Analysis CURE BSC3990L ###

**HPCC Orientation**

We will be logging onto the HPCC and doing some preliminary navigation. You will use your panther credentials
(username and password) to connect.


Login at the FIU Panther cluster Web portal from a browser: https://hpcgui.fiu.edu
Note: Mozilla Firefox has not been working for me lately and I have to connect via Safari.

Once you are on, go to the top where it says 'Clusters' and select 'Panther Shell Access.' It will open a terminal and ask you 
for your panther credentials again.

Congratulations! You are officially on the server.

Once you are set up it’s time to learn a little about how to operate on the HPCC. First, note that the terminal looks just like 
the terminal you used on sandbox.bio. This means that the Unix commands you have used can be used on the HPCC but it also means 
that you can’t rely on old standbys like Microsoft Word or Excel. Type

    $ ls

and 

    $ pwd

to figure out where you are. 

Computers are organized into hierarchical directories, and navigating those directories is our first goal. Each directory has a **path**, and paths can be absolute or relative. 

Once you have your terminal open, you should see a prompt ($). For the rest of the class, the $ prompt means I am in the terminal. Type the text that follows to see what the UNIX command does. For example, to see what directory you are currently in type 

    $ pwd

Most UNIX commands have a meaningful name, and learning the meaning helps me to remember it. For example, pwd = "print working directory." I'm asking the computer to tell me which directory I'm working in. The first thing to know is that everything in the shell is a program. When I type "pwd" I'm calling a program that tells UNIX to tell me which directory I'm working in.

Why do we type "pwd" instead of "print working directory?" *It is a whole lot shorter.* When you spend your entire day working on the computer, you need to find efficiency. Also, **spaces do not work in the shell.** Memorize this, take it to heart, remember every time you do anything on the computer. Putting spaces in your filenames will only cause pain and suffering later.

The system will give me the **absolute** path. That means it is the entire path, from the base directory (which is specified by / on a Mac). The forward slashes separate directory levels. Another handy location specifier is ~, which means "home." Every user's home will be different on a given UNIX system.


In order to learn navigation we're going to use another command

    $ mkdir that_directory

mkdir = "make directory"

To see what is inside the directory type

    $ ls

Or "list." Now you should see your newly created directory.

The relative path is **relative** to the directory you are in, and it is specified as "." So if you want to type a path relative to your location, you would type 

    $ ./that_directory/

In order to navigate into it, type

    $ cd that_directory

cd = "change directory." Type 

    $ pwd

to see where you are. To navigate backwards, type

    $ cd ..

Try typing

    $ cd ./that_directory

and then

    $ cd /that_directory

instead. Then try "pwd" again. Where are you? Does the forward slash matter?

Navigate back to your home directory. Remember you can do this in a number of ways, including:

    $ cd ~

Now type

    $ rmdir ./that_directory

    $ ls

What happens?

In order to learn more about a command, we can use man = "manual". For example, type

    $ man ls

It should give you a number of options. You call these like so:

    $ ls -a

And now you can see "all" files, including hidden or "dot" files. These are called dot files because their name begins with a dot. They are not typically shown and are usually configuration files for different programs. 

It seems simple but you have mastered UNIX navigation!


Now let's navigate around the FIU HPCC itself. Remember “.” is a location reference that indicates our current 
working directory and “..” is a location reference that indicates one directory up. We can use this location reference to move out of our current directory and into an different directory. The command to change directory is ‘cd’ short for… change directory. Type

    $ cd ./

Now type 

    $ pwd

and 

    $ ls

What happens? You should be in the same place because you instructed the computer to cd to your current location. Now type

    $ cd ../

to go up one directory. Type

    $ pwd

and

    $ ls

Where are you now? What else is in this directory?

Go up another level

    $ cd ../

and type 

    $ ls

Where are you now? Try to look around a little bit (don’t worry, you can’t get anywhere you’re not supposed to be).

Now navigate back to your home directory like this:

    $ cd ~

To see which programs are available type

    $ module avail

It will pop out a list of all the software available to use on the HPCC. To scroll through the list type 'return' or hit 
your space bar, type 'q' to quit and go back to your prompt.

In order to use a program we type 

    $ module load ‘softwarenamehere’

For example, scroll through the list and find the minimap module (minimap is alignment software we will use to align our DNA sequence reads to a reference genome sequence).

    $ module load minimap

What happens?

The HPCC is very precise and we need to put the whole name of the module in because sometimes there are different versions of the same software. Type
    
    $ module load minimap2-2.24

This loads the minimap2 software and any packages or software it might need to run into your environment. Specifically, it adds the software location to your path.

To see the software in your path type

    $ module list

And to remove software from your path type (for example)

    $ module unload minimap2-2.24

Now type 

    $ module list

again. What happens? 

To see precisely what is in your path and where it lives type

    $ echo $PATH


In order to create files we need to use a text editor on the HPCC. My favorite is vim. It has a lot of functionality but we will use it in the most simple way to create our scripts. Type

    $ vi myfile.txt
    
Your window should now have a lot of blue tildes in a single column. You are inside your file and you can edit it. Type 'i' for 'insertion mode' and you should see 

    --INSERT--

 at the bottom of your screen. We will now write our first script, a truly momentous day! Our script will start with a header or 'shebang' line that declares our desired shell, bash. We will then write our first program!
 
    #!/bin/bash
    
    echo "Hello World!"
 
In order to run our script we need to save our file and exit vi. We do this by hitting the escape key to move from insertion mode to command mode, typing the save command and typing the exit or quit command. Type

    <escape>
    :wq
  
You should now be back at the prompt. In order to make our script executable we type

    $ chmod +x myfile.txt
 
and we run the script by typing
 
    $ bash myfile.txt
    
What happens? 

A momentous occasion! You have run your first script!


Next up we will learn how to submit jobs on the HPCC. As Shawn explained the HPCC has an architecture composed of a head or login node and compute nodes. We submit job scripts on the head node specifying the computations to be done on the compute nodes. 

A script is a set of commands that we give the system to execute. We tell it which shell to execute under with the first line (called the ‘shebang’, for example #!/bin/bash for the bash shell) and any lines that we don’t want executed (comments) are ‘commented out’ with a # in the beginning. We will go through creating and submitting a job script.
Linux/Unix systems don’t support programs like Microsoft Word and in order to create a script on HPCC we need to use a text editor. We will use one called vi (this is my favorite) but feel free to use another one. 

Make sure you are in your home directory

    $ cd ~

and use vi to create a new file

    $ vi my_script.sh

You should see the vi window with the ~ symbols. Type ‘i’ to get into input or insertion mode and then type the following into your file:

    #!/bin/bash
    
    #SBATCH --account acc_jfierst_classroom
    #SBATCH -n 8
    #SBATCH --output=out_miniTry.log
    #SBATCH --mail-user=YOURFIUIDHERE@fiu.edu
    #SBATCH --mail-type=ALL

    module load minimap2-2.24
   
    # place commands here
    module list
    echo "Hello World"
    echo "Hello World" > out.txt

Hit the escape button (‘esc’) to get out of insertion mode and type ‘:wq’ (without the single quotes) to save the file and exit vi. You have just written your first HPCC job script!

What’s going on here? We are telling the system

    #!/bin/bash (1)

    #SBATCH --account acc_jfierst_classroom (2)
    #SBATCH -n 8
    #SBATCH --output=output_miniTry.log
    #SBATCH --mail-user=YOURFIUIDHERE@fiu.edu
    #SBATCH --mail-type=ALL
    
    module load minimap2-2.24 (3)

    #place commands here (4)
    module list (5)
    echo "Hello World" (6)
    echo "Hello World" > out.txt (7)

(1)	Which shell to use (bash)

(2)	A number of configuration commands, here which accounts to use and what resources we need. Also, an output file and an email to update us.

(3)	Which module to load

(4)	Comments for ourself (not read by the machine)

(5)	To list the loaded modules for us

(6)	to print “Hello World” to stdout

(7)	to redirect “Hello World” to a file called out.txt

The last thing is that we have to make the script executable. Type

    $ chmod +x my_script.sh

Now we can submit this script to the HPCC queue system and see what happens. Type

    $ sbatch < my_script.sh

If you need to check your job you can type

    $ squeue -u YOURUSERNAMEHERE
    
to check your jobs in the 'queue.' You should see something like this:
     
        JOBID               NAME        USER           TIME     ST        QOS
        52067   myscriptshSCRIPT        jfierst        0:05     R         class

where JOBID is your numerical job identifier, NAME is your script identifier, USER is your username, TIME is the time it has been running, ST is the state of the job (R is for running) and QOS is the ‘quality of service’ or queue it is running in (here, the class queue).

When it is finished type

        $ ls
and now you should see your script (my\_script.sh) along with a job output file (output_miniTry.log) and your output file out.txt. Look inside your output file

        $ more out.txt

What does it say? 

Now look in the job output file

        $ more output_miniTry.log

**In-class Assignment**

Submit your answer to this question on Canvas

1. What are the contents of your output_miniTry.log file?

2. Write a full HPCC job script that creates a file called out.txt with these contents:

Hello Class!
