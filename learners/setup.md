---
title: Setup
---


## Data Sets


Example data include in analyses include:

- 3 bacteria genomes including 2 Staphylococcus aureus genomes (GCF_000013425, GCF_003264815) and 1 Xanthomonas genome GCF_001719145.

- a subset of 1000 short DNA reads from rat (bioproject PRJNA574594 SRR10233452).

- chromosome 20 from the rat genome.

- The *C. elegans* proteome

- Human proteins transcribed and translated from chromosome 1.

### OS systems for attendees


### Distribute IP address and passwords


### Accessing the system through ssh


```ssh student@<yourIP>```

### Accessing artifacts like images through a new terminal

- open a new terminal 

- edit then issue the following command  
```scp student@<IPorHost>:<PathToFile>  <LocalFileLocation>```

**Example SCP local machine terminal:**

Issue the scp command with your IP and where you want to put the file. The command below puts it in current working directory with the dot at the end of the command " . ".  

```scp student@34.73.218.236:/home/student/test_scp.txt  . ```

Enter the password.  
View the file on your local machine.
```cat test_scp.txt```

- use your local system to view the downloaded file. 

