In this post, we will show you how to use <a href="https://zast.ai" target="_blank">zast.ai</a> to assess vulnerability step by step. Simply visit <a href="https://zast.ai" target="_blank">zast.ai</a> and register for an account to get started. Currently, new account holders need to apply to join a waitlist for activation. Please ensure you provide thorough information on the waitlist application page to expedite your early access.

There are five steps to finish an assessment on <a href="https://zast.ai" target="_blank">zast.ai</a>. 
- Upload the Java archive file
- Check connectivity
- Verify ownership
- Upload source code
- Verify test accounts

## Step 1: 
Give the project a name, choose and upload the Java archive file, then proceed to the next step for connectivity check.

![]({{'/assets/img/ZastxCodespace/upload-jar.png' | relative_url }})

## Step 2: 
Simply enter the URL of the target service url.

![]({{'/assets/img/ZastxCodespace/connectivity.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/connectivity-check.jpg' | relative_url }})

## Step 3: 
In the ownership verifying phase, follow the detailed instructions to finish adding HTTP challenge source. You can also refer to documentation for more detailed information.

![]({{'/assets/img/ZastxCodespace/hash.jpg' | relative_url }})

## Step 4: 
Then we can proceed to upload the source code.  <a href="https://zast.ai" target="_blank">Zast.ai</a> does not mandate that the actual source code be uploaded. However, the availability of source code will improve the precision of the scan result, e.g., the line numbers for each frame of the vulnerability call flows.

![]({{'/assets/img/ZastxCodespace/source-code.jpg' | relative_url }})

## Step 5: 
Adding and verifying test accounts, enter the login URL for the test accounts first. 

![]({{'/assets/img/ZastxCodespace/login-url.jpg' | relative_url }})

The system will connect to it and provide a snapshot of the page for quick verification. Confirm it and click "Next" to start adding test accounts.

![]({{'/assets/img/ZastxCodespace/snapshot.jpg' | relative_url }})

In this step, please remember to download the Google extension zast.ai Helper and enable it in incognito mode if it's your first time using <a href="https://zast.ai" target="_blank">zast.ai</a>.

![]({{'/assets/img/ZastxCodespace/verify-account.jpg' | relative_url }})

Once the verification begins, an incognito window will pop up displaying the previously configured page. We will need to log in to the corresponding role account.

![]({{'/assets/img/ZastxCodespace/incognito.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/login-account.png' | relative_url }})

Next, open the zast.ai AI Helper extension, click "Save Session," and start verifying the account.

![]({{'/assets/img/ZastxCodespace/session.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/session-check.jpg' | relative_url }})

![]({{'/assets/img/ZastxCodespace/session-confirm.jpg' | relative_url }})

When the account verification is complete, return to the zast page, where we will see that the administrator has been successfully added. We can now add new role accounts and repeat the previous steps until all test accounts are added.

![]({{'/assets/img/ZastxCodespace/test-account.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/add-accounts.jpg' | relative_url }})

After that, the system will ask if there are any other login URLs that need to be added. If we have multiple login URLs to scan, just repeat the above procedure; since we only have one url this time, select "No" and move to the next step.

![]({{'/assets/img/ZastxCodespace/multiple-url.jpg' | relative_url }})

## Process Completed

The system will then present an overview of this scan, including the content we submitted. After confirming everything is correct, tick the box for the service terms and privacy policy, and then we can start the scan!

![]({{'/assets/img/ZastxCodespace/overview.jpg' | relative_url }})

Scanning time will depend on the size of the submitted project, typically taking a few hours. The system will notify we via email once the scan is complete, so keep an eye on inbox.

![]({{'/assets/img/ZastxCodespace/report.png' | relative_url }})

That’s all for the detailed steps on using <a href="https://zast.ai" target="_blank">zast.ai</a> for vulnerability scanning. We hope this will be helpful for you.