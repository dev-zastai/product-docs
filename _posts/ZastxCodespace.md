<a href="https://github.com/features/codespaces" target="_blank">GitHub Codespaces</a> is a tool highly praised by developers, offering a cloud-driven development environment suitable for any activity, whether it’s a long-term project or a short-term task like reviewing pull requests. Operators can use these environments from either Visual Studio Code or a web-based editor. With it, we can quickly start a project and set up an environment, saving significant time and costs associated with preparing machines, setting up environments, configuring domains, etc.

![]({{'/assets/img/ZastxCodespace/codespace.png' | relative_url }})

Today, we are going to illustrate how to use GitHub Codespaces to build projects online, compiling Java archive files and packaging into a Java archive file (war/jar), and submit the archive file to zast.ai for vulnerability scanning.

## Step 1 
Log in to GitHub account and locate the corresponding project. Currently, since zast.ai focuses security vulnerabilities in Java-based web applications for now, we have prepared a Java web project as an example.

![]({{'/assets/img/ZastxCodespace/playground.png' | relative_url }})

Click the green "Code" button, and the pop-up window will display two tabs: Local and Codespaces. Switch to the Codespaces tab. If you have never created a Codespace before, select "Create Codespace on main."

![]({{'/assets/img/ZastxCodespace/playground-code.png' | relative_url }})


GitHub will then start creating the Codespace, which is a quick process, taking only a few seconds.

![]({{'/assets/img/ZastxCodespace/codespace-setup.png' | relative_url }})

Once created, we’ll see the project directory on the left side. Confirm that the project contains a pom.xml file; if it doesn't, the compilation will fail.

## Step 2
Now we can start building the project in the cloud on Codespace. From the "menu," select "Terminal," then open a "new terminal," or directly type in the lower terminal area:

*mvn clean package -DskipTests*

![]({{'/assets/img/ZastxCodespace/mvn-clean-package.png' | relative_url }})

After hitting Enter, the program will start running automatically. Once it finishes, you will see a "Build Success" prompt in the terminal window, and a new target folder will be created in the file directory. Open this folder to find the built Java files.

![]({{'/assets/img/ZastxCodespace/build-success.png' | relative_url }})

## Step 3 
Next, we will compile and package the project into an archive filethe project into a Java archive file so that we can upload it to zast.ai for vulnerability scanning. In the terminal, type the following command:

*java -jar target/simple-login-app-0.0.2-SNAPSHOT.jar*

After typing the first letter, press the Tab key, and Codespace will auto-fill it for you.

![]({{'/assets/img/ZastxCodespace/target-jar.png' | relative_url }})

Once the run is completed, we will usually see a prompt in the bottom right corner of Codespace asking whether to set the port to public. We should confirm this setting.

![]({{'/assets/img/ZastxCodespace/public.png' | relative_url }})

In addition, on the right of the "Terminal" tab, there’s a "Ports" tab with a notification bubble saying "1." Click it, and we will see a network icon next to the address displaying “Open in Browser.” Click it to open.

![]({{'/assets/img/ZastxCodespace/open-in-browser.png' | relative_url }})

If everything goes smoothly, we will see the following content. Clicking "Continue" will redirect us to this address. Completing this step means the Java project has been successfully created and is visible on the internet. We copy this site URL in advance, as it will be used for submitting the scanning task later.

![]({{'/assets/img/ZastxCodespace/copy-link.png' | relative_url }})

Next, go back to Codespaces, find the target folder, and download the archive file so that we can submit it to zast.ai for vulnerability scanning.

![]({{'/assets/img/ZastxCodespace/download-jar.png' | relative_url }})

## Step 4
Now, let’s explain how to submit and scan this Java project on zast.ai. First, visit zast.ai and register for an account. Currently, new account holders need to apply to join a waitlist for activation. Please ensure you provide thorough information on the waitlist application page to expedite your early access.
Before submitting the project for a scan, there are five steps: uploading the Java archive file, performing connectivity checks, ownership verification, uploading source code, and adding test accounts. Here’s a detailed description of each step.
* First, give the project a name, choose the Java archive file downloaded from Codespace, then proceed to the next step for connectivity check.

![]({{'/assets/img/ZastxCodespace/upload-jar.png' | relative_url }})

* On the connectivity check page, we need to enter the URL of the target service. This URL is the one we mentioned in step 3, so paste it directly and then perform the connectivity check. Once finished, click the next step to enter the ownership verification. 

![]({{'/assets/img/ZastxCodespace/connectivity.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/connectivity-check.png' | relative_url }})

* In the ownership verifying step, the page provides detailed instructions. First, we need to manually add the HTTP challenge source and then copy the hash value provided on the page. Go back to Codespace, find the zast.txt document, and paste the hash value there. (If you haven't created one before, you need to create a new one.) 

![]({{'/assets/img/ZastxCodespace/hash.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/zast-txt.png' | relative_url }})

* After completing the ownership verification, proceed to upload the source code.  zast.ai does not mandate that the actual source code be uploaded. However, the availability of source code will improve the precision of the scan result, e.g., the line numbers for each frame of the vulnerability call flows.

![]({{'/assets/img/ZastxCodespace/source-code.jpg' | relative_url }})

* After uploading the source code, move on to the step of adding test accounts. Before this, enter the login URL, which is the same as the one filled in the second step for the connectivity check.

![]({{'/assets/img/ZastxCodespace/login-url.jpg' | relative_url }})

Click the next step, the system will connect to the login URL and provide a snapshot of the page for quick verification. Confirm it and click the next step to start adding test accounts.

![]({{'/assets/img/ZastxCodespace/snapshot.jpg' | relative_url }})

Before starting to add test accounts, remember to download the Google extension zast.ai AI Helper and enable it in incognito mode.

![]({{'/assets/img/ZastxCodespace/verify-account.jpg' | relative_url }})

Once the verification begins, an incognito window will pop up displaying the previously configured Codespace page. After clicking "Continue," you will need to log in to the corresponding role account.

![]({{'/assets/img/ZastxCodespace/incognito.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/login-account.png' | relative_url }})

Next, open the zast.ai AI Helper extension, click "Save Session," and start verifying the account.

![]({{'/assets/img/ZastxCodespace/session.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/session-check.jpg' | relative_url }})

![]({{'/assets/img/ZastxCodespace/session-confirm.jpg' | relative_url }})

When the account verification is complete, return to the zast page, where we will see that the administrator has been successfully added. We can now continue to add new role accounts and repeat the previous steps until all test accounts are added.

![]({{'/assets/img/ZastxCodespace/test-account.png' | relative_url }})

![]({{'/assets/img/ZastxCodespace/add-accounts.jpg' | relative_url }})

After all accounts are added, click the next step. The system will ask if there are any other login URLs that need to be added. If you have multiple login URLs to scan, just repeat the above procedure; since we only have this one url this time, select "No" and move to the next step.

![]({{'/assets/img/ZastxCodespace/multiple-url.jpg' | relative_url }})

Before officially starting the scan, the system will present an overview of this scan, including the content we submitted. After confirming everything is correct, tick the box for the service terms and privacy policy, and then we can start the scan!

![]({{'/assets/img/ZastxCodespace/overview.jpg' | relative_url }})

Scanning time will depend on the size of the submitted project, typically taking a few hours. The system will notify we via email once the scan is complete, so keep an eye on inbox.

![]({{'/assets/img/ZastxCodespace/report.png' | relative_url }})

## Step 5
To ensure that the scanning task completes smoothly, we need to keep the Codespace service running during the scan. Therefore, please change the Codespace idle time to 4 hours; the path for modification is: GitHub -> Settings -> Code, Planning, and Automation -> Codespaces -> Default Idle Timeout. After making the time change, remember to save it before exiting.

![]({{'/assets/img/ZastxCodespace/codespace-idle.png' | relative_url }})

That’s all for the detailed steps on using GitHub Codespace to create projects and compile Java, as well as submitting Java archive files to the zast.ai platform for vulnerability scanning. We hope this is helpful for everyone.
Lastly, here’s a little tip: once we receive the vulnerability report from zast.ai, the Codespace service can be wrapped up. Remember to return to the GitHub project page and manually stop and delete this Codespace project in the “Code” section; otherwise, it will keep running, consuming our allotted time.

![]({{'/assets/img/ZastxCodespace/delete-codespace.png' | relative_url }})
GitHub provides users with 120 hours of free time each month, you can view the specific consumption of Codespace in your Profile -> Settings -> Billing & Plan.

![]({{'/assets/img/ZastxCodespace/usage-check.png' | relative_url }})
