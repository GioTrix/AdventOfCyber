# One man's false positive is another man's potpourri.
## The story
It’s the most wonderful time of the year again, and it’s also the most stressful day for Wareville’s Security Operations Center (SOC) team. Despite the overwhelming alerts generated by the new and noisy rules deployed, Wareville’s SOC analysts have been processing them nonstop to ensure the safety of the town.

However, the SOC analysts are now burning out of all the workload needed before Christmas. Numerous open cases are still pending, and similar alerts are still firing repeatedly, making them think of the possibility of false positives out of all this mess.

Now, help the awesome Wareville’s SOC team analyse the alerts to determine whether the rumour is true—that Mayor Malware is instigating chaos within the town.


## Connecting to the Elastic SIEM
Once the machine is up and running, we can connect to the Elastic SIEM by visiting https://MACHINE-IP.p.thmlabs.com in your browser using the following credentials:
- Username:	`elastic`
- Password:	`elastic`

After logging in, click on the menu in the top-left corner and navigate to the Discover tab to view the events.

## Setting the Time Window
The alert from the Mayor's office indicates that the activity occurred between November 29th, 2024, 00:00:00.00 and December 1st, 2024, 23:30:00.00.

To set this timeframe:

1. Click on the timeframe settings in the upper-right corner.
2. Select the Absolute tab.
3. Set the time range from November 29th, 2024, 00:00:00.00 to December 1st, 2024, 23:30:00.00.
4. Click Update to apply the changes.

## Making Events Readable
In their current form, the events may not be easily readable. To make the data more comprehensible, you can use the fields in the left pane to add columns to the results. Hovering over a field name will allow you to add that field as a column.

**Recommended Columns:**

- host.hostname: The hostname where the command was run.
- user.name: The user who performed the activity.
- event.category: To ensure you are looking at the correct event category.
- process.command_line: To see the actual commands run using PowerShell.
- event.outcome: To check if the activity succeeded.
- source.ip: To find out who ran the PowerShell commands.

## Questions and Answers

1. **What is the name of the account causing all the failed login attempts?**
   - **Filter:**
     - user.name: service_admin
     - event.category: authentication

  <details>
      <summary>Click to reveal the answer</summary> service_admin
  </details>

2. **How many failed login attempts were observed?**
   - **Filter:**
     - user.name: service_admin
     - event.category: authentication
     - event.outcome: failure

    <details>
      <summary>Click to reveal the answer</summary> 6791
    </details>

3. **What is the IP address of Glitch?**
   - **Action**: Check the "source.ip" column in the results to identify the IP address of Glitch.

    <details>
      <summary>Click to reveal the answer</summary> 10.0.255.1
    </details>

4. **When did Glitch successfully logon to ADM-01?**
   - **Filter:**
     - user.name: service_admin
     - event.category: authentication
     - source.ip: 10.0.255.1
     - NOT event.outcome: failure

    <details>
      <summary>Click to reveal the answer</summary> Dec 1, 2024 @ 08:54:39.000
    </details>

5. **What is the decoded command executed by Glitch to fix the systems of Wareville?**
   - **Filter:**
     - event.category: process

   **Action**: Look for the PowerShell command in the `process.command_line` field.

   Example of encoded command:
    ```bash
    C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -EncodedCommand SQBuAHMAdABhAGwAbAAtAFcAaQBuAGQAbwB3AHMAVQBwAGQAYQB0AGUAIAAtAEEAYwBjAGUAcAB0AEEAbABsACAALQBBAHUAdABvAFIAZQBiAG8AbwB0AA==
    ```
    McSkidy knows that encoded PowerShell commands are typically Base64 encoded and can be decoded using tools such as CyberChef. To ensure confidentiality, as the command might contain sensitive information, McSkidy chose not to use a public portal. Instead, she spun up her own instance of CyberChef hosted locally.

    **To decode the command:**
    Paste the encoded portion of the command into the Input pane in [CyberChef](https://gchq.github.io/CyberChef/).

    Use the From `Base64` and `Decode Text` recipes from the left pane to process the command.

    McSkidy configured the Decode Text recipe to use the `UTF-16LE (1200)` encoding, as this is the standard used by PowerShell for `Base64` commands.

    <details>
      <summary>Click to reveal the answer</summary> Install-WindowsUpdate -AcceptAll -AutoReboot
    </details>
    
## Conclusion
In this investigation, we assisted Wareville’s SOC team by analyzing the alerts related to failed logins, tracking the IP address, and decoding the malicious PowerShell commands. 
The steps involved reviewing authentication logs, filtering based on specific user activities, and using decoding techniques to reveal the true nature of the command executed by Glitch.

The investigation reaffirmed the importance of understanding PowerShell command encoding and having the tools to analyze suspicious activities within a SIEM system. 
Through our efforts, we confirmed the malicious nature of the actions, with the "service_admin" account attempting numerous failed logins, and Glitch using a Base64-encoded command to run a system update in an attempt to fix the systems of Wareville, likely to cover up other malicious activities.
