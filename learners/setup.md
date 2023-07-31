---
title: Setup
---

FIXME: Setup instructions live in this document. Please specify the tools and
the data sets the Learner needs to have installed.

## Data Sets

<!--
FIXME: place any data you want learners to use in `episodes/data` and then use
       a relative link ( [data zip file](data/lesson-data.zip) ) to provide a
       link to it, replacing the example.com link.
-->

Example data include chromosome 20 from the rat genome, a subset of 1000 short reads from Bioproject PRJNA574594 SRR10233452, DeepWeeds data set from AlexOlsen.

The bacteria genome X .

Links

https://github.com/AlexOlsen/DeepWeeds

### Discussion OS systems for attendees



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

