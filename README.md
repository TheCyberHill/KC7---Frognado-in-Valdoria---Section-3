# KC7---Frognado-in-Valdoria---Section-3
A write up of KC7 Cybers room - Frognado in Valdoria - Section 3

🐸 Section 3: Alright, It's Definitely an Angry Frog or Two

❓ Question 1<br>
"My name is Erik Bjorn, I’m a Chief Architect here."<br>
Erik explains he opened the mall construction plans and was greeted by… a frog meme. The frogs are furious. You suspect the threat actor went beyond Anita’s machine.<br>
🔍 What is the name of Erik Bjorn’s colleague?<br>
🛠️ Solution:<br>
Employees<br>
| where role == "Chief Architect"

✅ Answer: Sofia Lindgren

❓ Question 2<br>
You check if the Chief Architects received emails from the same internal address as Anita.<br>
📬 What is the subject of these emails?<br>
🛠️ Solution:<br>
Email<br>
| where sender == "alex_johnson@framtidxdevcorp.com"<br>
| where recipient == "sofia_lindgren@framtidxdevcorp.com"

✅ Answer: Important: Architectural Plan Changes

❓ Question 3<br>
The email link leads to a sign-in page.<br>
🌐 Which domain is the page hosted on?<br>
🛠️ Solution: Pull the link from the column.<br>

✅ Answer: greenprojectnews.net

❓ Question 4<br>
Same domain used to phish Anita. Subjects tailored to recipient roles.<br>
🎯 What type of phishing attack is this?<br>
🛠️ Solution: Targeted phishing type.<br>


✅ Answer: Spearphishing

❓ Question 5<br>
Threat actor did recon before attacking.<br>
🔎 How many distinct pages on the company’s website did they browse?<br>
🛠️ Solution:<br>
let acttack_ip =<br>
PassiveDns<br>
| where domain == "greenprojectnews.net"<br>
| distinct ip;<br>
InboundNetworkEvents<br>
| where src_ip in (acttack_ip)<br>
| distinct url<br>
| count<br>

✅ Answer: 78

❓ Question 6<br>
Let’s narrow it to job-related referrers.<br>
💼 Which job-related referrer returned the most results?<br>
🛠️ Solution: Compare LinkedIn and ValdorianJobs.<br>

✅ Answer: https://www.valdorianjobs.com

❓ Question 7<br>
Did Erik or Sofia log in to the phishing page?<br>
🔐 Who tried to log in to the actor-controlled page?<br>
🛠️ Solution:<br>
let chief_ip =<br>
Employees<br>
| where role == "Chief Architect"<br>
| distinct ip_addr;<br>
OutboundNetworkEvents<br>
| where url contains "greenprojectnews.net"<br>
| where src_ip in (chief_ip)<br>
| where url contains "password"<br>

✅ Answer: Both

❓ Question 8<br>
Threat actor used harvested credentials.<br>
⏰ What time did they log in to Sofia’s machine?<br>
🛠️ Solution:<br>
AuthenticationEvents<br>
| where username == "solindgren"<br>
| where src_ip in (acttack_ip)<br>

✅ Answer: 2024-06-27T10:41:38

❓ Question 9<br>
What did they do after gaining access?<br>
🧨 First PowerShell cmdlet used to delete something?<br>
🛠️ Solution:<br>
ProcessEvents<br>
| where process_name contains "powershell"<br>
| where username in (chief_username)<br>
| where process_commandline contains "mall"<br>

✅ Answer: Remove-Item

❓ Question 10<br>
🗑️ What file was deleted?<br>

✅ Answer: SuperImportantMallProjectArchitecturalPlans.docx<br>

❓ Question 11<br>
They must have replaced the file.<br>
📥 What file did they download?<br>

✅ Answer: fake_plans.docx<br>

❓ Question 12<br>
🌐 Which domain hosted the downloaded file?<br>

✅ Answer: newdevelopmentupdates.org<br>

❓ Question 13<br>
🔄 What PowerShell cmdlet was used to rename the file?<br>

✅ Answer: Rename-Item

❓ Question 14<br>
📁 What was the file renamed to?<br>

✅ Answer: SuperImportantMallProjectArchitecturalPlans.docx

❓ Question 15<br>
Sneaky replacement with a meme!<br>
🧠 According to MITRE, what kind of impact is this?<br>

✅ Answer: Data Manipulation
