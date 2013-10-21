##The Linux Cheat Sheet.

Commands that seem to catch you out:

Remember some of these may need sudo power. Its not a good idea to use sudo all the time because it allows you to make some devastating mistakes.

<a name="contents"></a>
#Contents#

- [Dealing With Services](#services) 
- [Directory Navigation](#navigation)
- [Copying & Moving Files](#copymove)
- [Deleting Files](#deleting)
- [Mounting Devices](#mountingDevices)
- [Mounting Network Drives](#mountingDrives)
- [SSH](#ssh)
- [SSH Copying](#sshcopy)
- [Viewing & Editing Files](#editing)

<a name="services"></a>
####Dealing with Services
[Contents](#contents)

To see the status of a service running on a server:

	sudo service xxxxx status/start/restart/stop

This will do to the service xxxxx what you ask of it. E.g

	sudo service plexmediaserver status

Will tell you whether or not the plexmediaserver is running. If it is start won't do anything - restart will though...

	sudo!!

Will put sudo infront of the last command you ran, i.e if you try and do something without sudo, and it needs sudo, instead of typing it all out again you can just use this command to automatically do that

<a name="navigation"></a>
####Directory Navigation
[Contents](#contents)

Assuming your current location is:

	/home/username:~$

To look inside the current directory use ls:

	/home/username:~$ ls
	aFolder aTempFolder documents myStuff

we can see there are four folders

To go inside one

	/home/username:~$ cd documents

You can cd as many times as you wish in one go:

	/home/username:~$ cd documents/mywork

Will move you into directory mywork, inside documents. I.e move up two directories in one

We can chain commands like

	/home/username:~$ cd documents; ls

Will go inside a directory and then list the contents of that directory

<a name="copymove"></a>
####Copying & Moving Files
[Contents](#contents)

To copy a file use:

	cp source/location destination/location

again, if the source file is owned by root you would need to sudo this command:

	cp /home/username/textfile.txt /home/username/documents

Will move the textfile found in your home directory to a directory called documents inside that directory.
The command mv does the same except moves the file rather than copies it.

Copy a whole directory:
	
	cp -r /home/username/folder /home/username/documents

will copy the directory folder to documents. Again you can do the same with mv if you want to move it

<a name="deleting"></a>
####Deleting
[Contents](#contents)

Delete a file with rm.

	rm textfile.txt

Delete a directory with

	rm -rf /home/username/folder

The "f" forces it to delete everything even if it should ask you first as to whether it should be deleted. Be careful!

<a name="mountingdevices"></a>
####Mounting
[Contents](#contents)

If you plug a usb stick in it should be automounted. You will find it in the directory /media. Some directories can be accessed from anywhere. /media is one of them:

	/home/username:~$ cd /media ; ls
If it has already been mounted it should be listed in there.

If it hasn't, give me a call....

<a name="mountingDrives"></a>
######To mount a network drive
[Contents](#contents)

	mount [ip of network drive]:/location/of/folder/to/mount /media/where/to/mount/to nfs rw 0 0
	
The above command will mount a network drive at a certain ip address and folder location into a folder in /media. Convention says all mounted drives should be in media. The nfs is the filesystem type. It maybe fat32 for instance. the rw says you have read/write capability and the 0 0 gives everyone rights to it. If you want to limit users controls over network locations you should look up more detailed commands.

You can put the same command in the /etc/fstab file, just remove the "mount" part of the command. /etc is another directory that can be accessed from anywhere/
To edit the fstab file use nano.

	/home/username:~$ nano /etc/fstab

Follow nano's instructions to save and exit.

To remount (refresh the contents of fstab) do:

	mount -a

<a name="ssh"></a>
####SSH
[Contents](#contents)

SSH is how you connect to the server from another location. I.e you dont have to be at home...

If you are using a Mac or Linux:

	ssh username@[internet ip] -p [port number]

If you want to pipe all your internet useage through your home network, set your internet browsers proxy to 127.0.0.1 and port 9050 then change the SSH command to:

	ssh -D 127.0.0.1:9050 username@[internet ip] -p [port number]

Login, and now try browsing the web. You are browsing as though you are at home. You can then connect to your home router etc by going to its ip address as though you were at home.

If you are using putty then !yuck! I can't remember how you do that, but it is possible.....

<a name="sshcopy"></a>
####SSH Copying
[Contents](#contents)

You can copy files between your local computer and a remote computer over ssh using scp

	scp username@[remote ip address]:/home/username/textfile.txt /home/localuser/documents

Will copy the file textfile.txt from your remote computer to your local documents folder. It can be done the other way around aswell

	scp /home/localuser/documents/textfile.txt username@[remote ip address]:/home/username/

To copy a whole directory use the "r" switch:

	scp -r /home/localuser/documents/folder username@[remote ip address]:/home/username/

If the data isn't hugely important, you can increase speed of copying by changing the encryption to the blowfish algorithm rather than the default triples DES algorithm.

	scp -c blowfish /home/localuser/documents/textfile.txt username@[remote ip address]:/home/username/

<a name="editing"></a>
####Viewing & Editing Files
[Contents](#contents)

If you want to view a text file, or actually any file for that matter, you can just use cat. cat will print the contents of a file to the screen for you to look at. You cant edit it, but you can look inside it. For instance:

	/home/username:~$ cat /etc/fstab

Will show you the contents of your fstab file - the file that holds the details of drives to mount on start up - i.e where to mount them from, and where to mount them to.

If you wanted to edit it, you could use nano:

	/home/username:~$ nano /etc/fstab

Nano is a very basic text editor. Once you have made your changes, CTRL-X will ask you if you want to save your changes before quitting, Y to save, N to delete changes.