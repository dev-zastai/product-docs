A few days ago, a client submitted a vulnerability assessment, and the results were quite interesting. With their permission, we are sharing this case with you. In this case study, we will show how zast is able to detect command injection vulnerability even sophisticated variants. 
## 1. Vulnerability Assessed by zast.ai - Command Injection
Let's see the 1st vulnerability report:

![]({{'/assets/img/Bandaid/1ts.png' | relative_url }})

![]({{'/assets/img/Bandaid/report-1-taint-source.png' | relative_url }})

![]({{'/assets/img/Bandaid/report-1-POC.png' | relative_url }})

<u>The taint source and taint sink are identified, along with a POC that shows a malicious payload executing a shell command</u>. 

Our client discussed the report after reviewing it:

"Zast.ai reported a critical vulnerability, we must address it immediately."

"What if we add Base64 encoding to the input? That might help obscure it and prevent exploitation."

"Let’s try that and resubmit it to zast.ai for testing."

After implementing the first patch, the team submitted for reassessment. Now, let's see what happens next.

## 2. 1st Patch - Base64 Encoding
See below for the second report:

![]({{'/assets/img/Bandaid/2ts.png' | relative_url }})

![]({{'/assets/img/Bandaid/report-2-taint-source.png' | relative_url }})

![]({{'/assets/img/Bandaid/report-2-POC.png' | relative_url }})

<u>The taint source remained the same in both reports, indicating that the vulnerability was unchanged. In the taint sink, it shows that Base64 encoding is applied.</u>

<u>The POC indicates a request with an encoded shell command that bypasses superficial checks. Despite the encoding, the application processes the input without proper validation, allowing the command to execute.</u>

When receiving the report, the security team leader told our consultant: "I'm impressed that zast.ai cracked the Base64 encoding. But, I’m confident I can write code that zast won't be able to exploit. Let me try something different." And they reassessed the application without revealing any details about the second patch.

## 3. 2nd Patch - Prefix Matching
Let's see how zast.ai works this time:

![]({{'/assets/img/Bandaid/3ts.png' | relative_url }})

![]({{'/assets/img/Bandaid/report-3-taint-source.png' | relative_url }})

![]({{'/assets/img/Bandaid/report-3-POC.png' | relative_url }})

<u>The taint source stays the same, so the vulnerability wasn't fixed. In the taint sink, the code implemented a prefix validation for the Base64-decoded command, allowing execution only if the command starts with "secret." It reduces command injection risks by validating the input and using substring(6) to remove the prefix, ensuring only specific commands execute.</u>

<u>Again, the POC validate the command injection vulnerability by sending a Base64-encoded shell command with a prefix match to a specified URL. By combining the prefix "secret" with the command, it tests whether the server is susceptible to remote code execution. Additionally, it uses HTTP headers to mimic legitimate requests, revealing potential security weaknesses in the application's input handling.</u>

The client came to us and shared their feedback.

"I’m genuinely impressed by zast.ai’s analytical capabilities. It uncovered vulnerability with such precision!"

"Absolutely! Even with our attempts to obscure the flaw through base64 encoding and prefix matching, it saw right through it."

"What makes zast.ai unique is its different approach compared to traditional black-box and white-box testing tools."

"That’s the game changer! This method allows it to find vulnerabilities in ways that traditional methods can't."

"It really sets a new standard for vulnerability detection. I can’t help but wonder how we ever managed without it!"

We're pleased to receive this feedback from our client. It alighs perfectly with how we expected <a href="https://zast.ai" target="_blank">zast.ai</a> to perform. Afterwards, we also provided some suggestions for fixing the vulnerability.

## 4. Zast.ai's Edge in Vulnerability Assessment
From this case, we can see that after two patches, the vulnerability became much harder to detect (or identify). However, <a href="https://zast.ai" target="_blank">zast.ai</a> was able to uncover and confirm it thanks to its advanced large language model. As our client mentioned, <a href="https://zast.ai" target="_blank">zast.ai</a> truly stands out compared to traditional methods like black-box and white-box testing. 


Visit zasta.ai now to try it and keep your systems secure! We also expect to hear your examples of vulnerability cases and remediation strategies. Join us in the conversation and let's collaborate on advancing research and best practices for vulnerability identification and prevention together!