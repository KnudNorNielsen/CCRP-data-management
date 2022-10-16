# Data transfer

This page is about how to navigate and move data between your computer or an HPC and The Electronic Research Data Archive (ERDA) 

## Content:
1.	Introduction
2.	SFTP setup
3.	Move files between systems using SFTP
4.	SCP
5.	Deleting files or folder in ERDA
6.	Compress files and folders
7.	Free computation!

## Introduction
SCP (Secure Copy Protocol) and SFTP (Secure/SSH File Transfer Protocol) are alternatives for FTP (File Transfer Protocol), which is useful for local, non-scheduled file transfers. All three can help moving files from one location to another over Ethernet. However, FTP sends data in plain text, while the other two use the Secure Shell (SSH) protocol for communication.
 
## Resources
[ucph-erda-user-guide.pdf](https://erda.ku.dk/public/ucph-erda-user-guide.pdf)

## SFTP
SFTP allows interaction with the remote directory, including directory listing, moving around within the directory and removal of single files (not folders). SCP only performs file transfer, but it does this faster than SFTP.
First I will wark through how to make a simple sftp connection to erda and transfer files. Below, in the section 'sftp password key' I will explain how you can setup and password key facilitating more smooth transfers.

Setting up the SFTP within ERDA enabling sftp connections from other systems.
Login to [erda.dk-(https://erda.dk/wsgi-bin/home.py), move to 'User' (i.e the person in the bottom left corner), then to 'setup', and then the 'sftp' tap in the top. 

Here you enter the PASSWORD you would like to use and press save - and you are ready to go!
From the command line, either on your personal computer or from a HPC as Computerome you connect to ERDA as follows:
```
sftp [username]@io.erda.dk

For me it will look like this:  
sftp knn@ign.ku.dk@io.erda.dk
```
After you have pressed enter, you will get promted for the password you entered in the lower part of the ERDA-SFTP webpage.
Now you have jumped into the root of the ERDA directory!


### Up and download using SFTP
Connecting to ERDA from either C2 or PC as described above

The sftp places you at the root of ERDA, from here you can navigate to the directory you wish to upload to or download from, or you can simply add the directory path to the file.
You have two modes of action: put and get, i.e up- and download. Both are structured similarly:
```
[action] [-r] [from] [to]
```
[-r] means recursive, and is needed in order to move folders. In practice it means, include everything inside folders.
Finaly you need to speficy what should be up- or downloaded and where it should go.

An download from ERDA could look like this:
The from is relative to your position on ERDA and the to, relative to the position you had on e.g. Computerome when you login to ERDA.
```
get -r 'folder to download' .
```
Here a folder at ERDA are downloaded to the directory from where I login to ERDA, i.e. [to]="."

An upload could look like this:
```
put -r [my_folder or file] CCRP/MATRIX/RawData/Microbiomics/Ref_Genomics/Amp_ITS/.
```
Here I upload to a folder without navigating to it, but just by giving the path to it.


### sftp password key
Rrepare connection key for connecting to ERDA form Computerome

```
cd ~/
ssh-keygen  # just press enter (if you do not want to specify a passphrase)
cat .ssh/id_rsa.pub
```
This is the public key that you need to copy to [ERDA setup page](https://erda.dk/wsgi-bin/setup.py?topic=sftp) under the sftp-tab.

Then create a local config, e.g. by typing nano ~/.ssh/config, and put the information as described by erda.dk under "How to proceed after enabling login above ..."

```
Host io.erda.dk
    Hostname io.erda.dk
    VerifyHostKeyDNS yes
    User [user]@plen.ku.dk
    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlMaster auto
    ControlPersist 10m
    Port 22
    # Assuming you have your private key in ~/.ssh/id_rsa
    IdentityFile ~/.ssh/id_rsa
```
Make sure that the file-permisson of the config-file is correct for openssh by typing:
```
chmod 600 ~/.ssh/config
```

#### Connect using sftp
Now you are good to connect - just type:
```
sftp -B 258048 io.erda.dk
```
[-B 258048], specifies the transfer bite size and this size is recommended by Computerome.

See help for the available commands.
```
sftp> help
Available commands:
bye                                Quit sftp
cd path                            Change remote directory to 'path'
chgrp grp path                     Change group of file 'path' to 'grp'
chmod mode path                    Change permissions of file 'path' to 'mode'
chown own path                     Change owner of file 'path' to 'own'
df [-hi] [path]                    Display statistics for current directory or
                                   filesystem containing 'path'
exit                               Quit sftp
get [-afPpRr] remote [local]       Download file
reget [-fPpRr] remote [local]      Resume download file
reput [-fPpRr] [local] remote      Resume upload file
help                               Display this help text
lcd path                           Change local directory to 'path'
lls [ls-options [path]]            Display local directory listing
lmkdir path                        Create local directory
ln [-s] oldpath newpath            Link remote file (-s for symlink)
lpwd                               Print local working directory
ls [-1afhlnrSt] [path]             Display remote directory listing
lumask umask                       Set local umask to 'umask'
mkdir path                         Create remote directory
progress                           Toggle display of progress meter
put [-afPpRr] local [remote]       Upload file
pwd                                Display remote working directory
quit                               Quit sftp
rename oldpath newpath             Rename remote file
rm path                            Delete remote file
rmdir path                         Remove remote directory
symlink oldpath newpath            Symlink remote file
version                            Show SFTP version
!command                           Execute 'command' in local shell
!                                  Escape to local shell
?                                  Synonym for help
```

#### sftp batch transfer
Assuming you have the following **sftp_commands.txt** file, you can run all commands using batch-mode execution for transferring files to erda.dk.
```
!put local/path/to/file1 remote/path/to/file1
!put -R local/folder  remote/folder
```
The ! exclamation mark ignores error in case you want to have files with partly transferred data (otherwise execution of the batch job stops). Remove in case you don’t need that behaviour.

If you set up access to your erda folder appropriatly you should be able to connect to erda <your-hostname>. I named it erda io.erda.dk (see above). If you can connect using this command, execute the sftp command in batch mode providing sftp_commands as an argument in order to store the files in a hela folder on your erda root directory.
```
sftp -B 258048 -b **sftp_commands.txt** io.erda.dk
```



## SCP
Downloading a file from Computerome (C2) to your own computer
```
scp [from] [to]
scp -r [user]@ssh.computerome.dk:~/matrix/projects/xxx/scripts/test.sh ./test/.
```

Uploading a file from your own computer to C2
```
scp [from] [to]
scp -r test.txt [user]@ssh.computerome.dk:~/matrix/projects/xxx/.
```


## Deleting files or folder in ERDA
Deleting files or folder in ERDA can cause some trouble - meaning it does not always actually delete the files when you ask it to. This is atleast true for the sftp connection.

To my knowledge the best solution is to open a terminal through a Data Analysis Gateway or DAG instance on ERDA. It sounds complicated, but it is not🙂
Press the Jupiter bottom at the ERDA start page > DAG > Start my server > Start (any of the notebook configs will work, just stick with the first one) > Terminal > cd work/CCRP/MATRIX/

Here you have the normal command line options of recursive deletion, i.e deleting folder with all its content
```
rm -r [folder name]
```


## Compress files and folders
Remember always to compress large files before transfer!

pigz offers multithreading:
```
pigz -p 20 something.r1.fq
```
This speeds things up by used 20 cores to compress the file to: *something.r1.fq.gz*


```
tar -czvf knndk1.consensus.ref.fasta.tgz knndk1.consensus.ref.fasta
tar -czvf archive.tar.gz stuff
```


## Decompress 
```
gzip -d sample.fastq.gz sample.fastq
or 
pigz -p 20 -d sample.fastq.gz sample.fastq
```
``` 
tar -xzvf archive.tar.gz
```

See: http://www.metagenomics.wiki/tools/ubuntu-linux/gzip-tar-gz

## Free computation!
Notice that ERDA provides free computation for smaller jobs!
With Jupiter you can start a command line interface, R notebooks Python and more, with direct access to the data on ERDA.

Press the Jupiter bottom at the ERDA start page > DAG > Start my server > Start (any of the notebook configs will work, just stick with the first one)
Depending on the notebook you choose, you will get different compilations of preloaded modules.

Then just choose the application you wish to use - and start your work on the data in your ERDA folders.
