Data transfer
=============

ERDA - Data transfer
This page is about how to navigate and move data between your computer or an HPC and The Electronic Research Data Archive (ERDA) 

Content:
1.	Introduction
2.	SFTP setup
3.	Move files between systems using SFTP
4.	SCP to Computerome
5.	Mounting Computerome or ERDA to your computer
6.	Deleting files or folder in ERDA
7.	Compress files and folders
8.	Free computation!

Introduction
SCP (Secure Copy Protocol) and SFTP (Secure/SSH File Transfer Protocol) are alternatives for FTP (File Transfer Protocol), which is useful for local, non-scheduled file transfers. All three can help moving files from one location to another over Ethernet. However, FTP sends data in plain text, while the other two use the Secure Shell (SSH) protocol for communication.
 
Resources
`ucph-erda-user-guide.pdf (ku.dk) <https://erda.ku.dk/public/ucph-erda-user-guide.pdf>`

SFTP
----
SFTP allows interaction with the remote directory, including directory listing, moving around within the directory and removal of single files (not folders). SCP only performs file transfer, but it does this faster than SFTP.

Setting up the SFTP within ERDA enabling sftp connections from other systems.
Visit ERDA.dk > User (i.e the person in the bottom left corner) > setup > sftp (in the top)

Here you enter the PASSWORD you would like to use and press save - and you are ready to go!

My own page looks like this: 

From the command line, either on your personal computer or from a HPC as Computerome you connect to ERDA as follows:
sftp Username@io.erda.dk            (For me it will look like this:   sftp knn@plen.ku.dk@io.erda.dk)

After you have pressed enter, you will get promted for the password you entered in the lower part of the ERDA-SFTP webpage.
Now you have jumped into the root of the ERDA directory!



Move files between systems using SFTP
Connecting to ERDA from either C2 or PC
Go into folder that you which to upload from
.. code-block:: console
    sftp [user_email]@io.erda.dk
    password

The sftp places you at the root of ERDA, from here you can navigate to the directory you wish to upload to or download from, or you can simply add the directory path to the file.
.. code-block:: console
    # [action] [-r] [from] [to]
    put -r 'folder to upload' .
    get -r 'folder to download' .

[-r] means recursive, and is needed in order to move folders. In practice it means, include everything inside folders.
An upload could look like this:
.. code-block:: console
    put -r [my_folder or file] CCRP/MATRIX/RawData/Microbiomics/Ref_Genomics/Amp_ITS/.



Transferring files from your computer – back and forth to Computerome - scp
Replace username by our computerome2 username, and specify either a file or folder and the path/to/folder.
You are not logged in to computerome.
From your machine to computerome2
scp file username@ssh.computerome.dk:/path/to/folder
scp -prC folder username@ssh.computerome.dk:/path/to/folder
From computerome to your machine
scp username@ssh.computerome.dk:/path/to/folder/file  ~/local/path/
Downloading a file from Computerome (C2) to your own computer
.. code-block:: console
    #scp [from] [to]
    scp -r [user]@ssh.computerome.dk:~/matrix/projects/xxx/scripts/test.sh ./test/.

Uploading a file from your own computer to C2
.. code-block:: console
    # scp [from] [to]
    scp -r test.txt [user]@ssh.computerome.dk:~/matrix/projects/xxx/.

Mounting Computerome or ERDA to your computer
It is possible to “mount” Computerome to your computer. This means that Computerome appears as a regular drive on your computer, so you can interface with files on Computerome as if they were placed on your own computer. Do note that files are streamed to and from Computerome - meaning that IO operations become network operations. As such when mounted, the speed of reading/writing Computerome files will be set by your network to Computerome.
How to mount: Linux
•	Install sshfd.
•	Run sudo modprobe fuse to add the FUSE module to your kernel
•	Create a new directory on your computer where Computerome will be “placed”: mkdir ~/computerome
•	Now you can mount. The following command will mount Computerome’s /home/projects/group_name to your new directory: sshfs USER@ssh.computerome.dk:/home/projects/group_name ~/computerome.
How to mount: Other file systems
For MacOS, check out https://osxfuse.github.io/



Deleting files or folder in ERDA
--------------------------------
Deleting files or folder in ERDA can cause some trouble - meaning it does not always actually delete the files when you ask it to. This is atleast true for the sftp connection.

To my knowledge the best solution is to open a terminal through a Data Analysis Gateway or DAG instance on ERDA. It sounds complicated, but it is not🙂
Press the Jupiter bottom at the ERDA start page > DAG > Start my server > Start (any of the notebook configs will work, just stick with the first one) > Terminal > cd work/CCRP/MATRIX/

Here you have the normal command line options of recursive deletion, i.e deleting folder with all its content
.. code-block:: console
    rm -r [folder name]



Compress files and folders
--------------------------
Remember always to compress large files before transfer!

**Compress files**:

.. code-block:: console
    tar zcvf knndk1.consensus.ref.fasta.tgz knndk1.consensus.ref.fasta
    tar -zcvf pop.tgz pop/


**Decompress** .gz file (compressed .gz file will be removed after decompression)  http://www.metagenomics.wiki/tools/ubuntu-linux/gzip-tar-gz

gzip -d sample.fastq.gz sample.fastq
tar zxvf knn*a.tgz
 


Free computation!
-----------------
Notice that ERDA provides free computation for smaller jobs!
With Jupiter you can start a command line interface, R notebooks Python and more, with direct access to the data on ERDA.

Press the Jupiter bottom at the ERDA start page > DAG > Start my server > Start (any of the notebook configs will work, just stick with the first one)
Depending on the notebook you choose, you will get different compilations of preloaded modules.

Then just choose the application you wish to use - and start your work on the data in your ERDA folders.








