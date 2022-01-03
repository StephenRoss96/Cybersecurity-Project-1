## Activity File: Kibana Continued

### Activity Tasks

#### SSH Barrage

Task: Generate a high amount of failed SSH login attempts and verify that Kibana is picking up this activity.

1. Start by logging into your jump-box. 

	- Run: `ssh username@ip.of.web.vm`

	- You should receive an error:

		```bash
		sysadmin@Jump-Box-Provisioner:~$ ssh sysadmin@10.0.0.5
		sysadmin@10.0.0.5: Permission denied (publickey).
		```

	- This error was also logged and sent to Kibana. 

2.  Run the failed SSH command in a loop to generate failed login log entries.

	 - You can use a bash `for` or `while` loop, directly on the command line, to repeatedly run the SSH command.

3. Search through the logs in Kibana to locate your generated failed login attempts.
	$ for i in {1..10}; do echo $1; ssh azureuser@10.1.0.9; done
**Bonus**: Create a nested loop that generates SSH login attempts across all three of your VM's.
	$ for i in {1..2}; do for j in {1..2}; do echo $i $j; ssh azureuser@10.1.0.9; ssh azureuser@10.1.0.10; done; done


#### Linux Stress

1. From your jump box, start up your Ansible container and attach to it.

2. SSH from your Ansible container to one of your WebVM's.

3. Run `sudo apt install stress` to install the stress program.

4. Run `sudo stress --cpu 1` and allow `stress` to run for a few minutes. 

5. View the Metrics page for that VM in Kibana.  What indicates that CPU usage increased? The graph(?)

6. Run the `stress` program on all three of your VMs and take screenshots of the data generated on the Metrics page of Kibana.

  	- **Note:** The stress program will run until you quit with Ctrl+C.


#### wget-DoS

The Metrics section for a single VM will show Load and Network Traffic data. 

We can generate abnormal data to view by creating a DoS web attack. The command-line program `wget` can do this easily.

`wget` will download a file from any web server. Use man pages for more info on `wget`.

1. Log into your jump box.

2. Run `wget ip.of.web.vm`.

	```bash
	sysadmin@Jump-Box-Provisioner:~$ wget 10.0.0.5
	--2020-05-08 15:44:00--  http://10.0.0.5/
	Connecting to 10.0.0.5:80... connected.
	HTTP request sent, awaiting response... 302 Found
	Location: login.php [following]
	--2020-05-08 15:44:00--  http://10.0.0.5/login.php
	Reusing existing connection to 10.0.0.5:80.
	HTTP request sent, awaiting response... 200 OK
	Length: 1523 (1.5K) [text/html]
	Saving to: ‘index.html’

	index.html            100%[=======================>]   1.49K  --.-KB/s    in 0s      

	2020-05-08 15:44:00 (179 MB/s) - ‘index.html’ saved [1523/1523]
	```

3. Run `ls` to view the file you downloaded from your web VM to your jump box. 

	```bash
	sysadmin@Jump-Box-Provisioner:~$ ls
	index.html
	```

4. Run the `wget` command in a loop to generate many web requests.

	- You can use a bash `for` or `while` loop, directly on the command line, just as you did with the SSH command.

	$ x=1; while [ $x -le 10 ]; do echo $x; wget 10.1.0.10; x=$(( $x + 1)); done

5. Open the Metrics page for the web machine you attacked and answer the following questions:
	
	- Which of the VM metrics were affected the most from this traffic?

	Network traffic increased from 46.7kbit/s to 58.6kbit/s (outbound) and from 8.1kbit/s to 24.1kbit/s (inbound)
	The other metrics were mostly unchanged.

**Bonus**: Notice that your `wget` loop creates a lot of duplicate files on your jump box.

-  Write a command to delete _all_ of these files at once.
	rm ~/index.html*

-  Find a way to run the `wget` command without generating these extra files.
		--delete-after option
		
	- Look up the flag options for `wget` and find the flag that lets you choose a location to save the file it downloads. 
		wget -O or --output-document=FILE
		
	- Save that file to the Linux directory known as the "void" or the directory that doesn't save anything.
		wget -O /tmp/ 10.1.0.10

**Bonus**: Write a nested loop that sends your `wget` command to all three of your web VMs over and over.
		$ x=1; while [ $x -le 10 ]; do echo $x; wget --delete-after 10.1.0.10; wget --delete-after 10.1.0.9; x=$(( $x + 1)); done

---

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.  
