<font size="+12"><center>
Verifying Zast.ai Website Ownership Verification

</center></font>

# Introduction

Zast.ai is a cutting-edge SaaS solution that uses artificial intelligence to find zero-day vulnerabilities in web applications. By leveraging advanced AI capabilities, Zast.ai helps web application owners identify security flaws before malicious attackers can exploit them. However, due to the powerful nature of this tool, it's essential to ensure that only the genuine owners of websites can initiate scanning their properties. Therefore, Zast.ai requires customers to prove their ownership of target websites by hosting a verification file (zast.txt) at a specific URL.

While this process is straightforward for some, it can be tricky for those less familiar with server configurations. This blog aims to guide users through the process of proving website ownership for various server setups, with a particular focus on using reverse proxies such as Nginx.

## The Verification Process with Zast.ai

Before diving into technical solutions, let's briefly outline the verification process Zast.ai employs:

1. **Generate a Verification String** : When you attempt to scan a website using Zast.ai, a unique, randomly generated string is provided, as illustrated in the screenshot below.

![]({{'/assets/img/OwnerVerification/Fig-1-VerificationString.png' | relative_url }})

2. **Create** **`zast.txt`** : Place this string in a file named `zast.txt`.
3. **Upload** **`zast.txt`** : This file should be accessible at `[your-domain]/.well-known/zast.txt`. For example, if your website is `https://example.com`, the file should be available at `https://example.com/.well-known/zast.txt`.
4. **Verify** : Zast.ai checks this URL to confirm ownership. Once ownership verification succeeds, the screenshot illustrated below appears.

![]({{'/assets/img/OwnerVerification/Fig-2-VerificationSuccess.png' | relative_url }})

Let's discuss how you can achieve this on different server setups, starting with the commonly used Nginx reverse proxy.

## Using Nginx to Place the `zast.txt` File

Nginx is a popular web server and reverse proxy solution that many websites use to handle traffic and manage connections to backend services. If you use Nginx, adding the `zast.txt` file to the required location is relatively straightforward.

### Example Nginx Configuration

Suppose your website is hosted behind an Nginx reverse proxy with the following configuration:

```Nginx
server {
    server_name  example.com;
    location / {
       proxy_pass http://crmeb-admin:20700;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    location = /404.html {
        internal;
    }
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        internal;
    }
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/admin.example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/admin.example.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
```

### Adding the `zast.txt` Location

To add the `zast.txt` file for Zast.ai verification, modify the Nginx configuration as follows:

1. **Add a New Location Block** : Insert a new location block in the server configuration to serve the `zast.txt` file:

```Nginx
location /.well-known/zast.txt {
    root         /usr/share/nginx/html/dist;
}
```

2. **Create the File** : Ensure that the file `/usr/share/nginx/html/dist/.well-known/zast.txt` exists and contains the verification string provided by Zast.ai.
3. **Restart Nginx** : After making these changes, restart Nginx to apply the new configuration:

```Bash
sudo systemctl restart nginx
```

### Explanation

- The `location` directive in Nginx specifies how to respond to requests for the URL path `/.well-known/zast.txt`.
- The `root` directive tells Nginx where to find the file to serve. In this example, it's `/usr/share/nginx/html/dist`.
- Make sure that the file exists at the specified location and contains the exact string provided by Zast.ai.

## Other Web Server Architectures

### Apache HTTP Server

If you are using Apache instead of Nginx, the process is somewhat similar:

1. **Create the `zast.txt` File** : Place the file with the required content in your document root. For example, when the document root is the default /var/www/html:

```Bash
/var/www/html/.well-known/zast.txt
```

2. **Configure Apache** : Ensure Apache allows access to this directory. You might need to add something like this to your Apache configuration:

```Apache
<Location "/.well-known/">
    Options FollowSymLinks
    AllowOverride None
    Require all granted
</Location>
```

3. **Restart Apache** :

```Bash
sudo systemctl restart apache2
```

### Cloud Hosting Services

If your website is hosted on a cloud service like AWS, Google Cloud, or Azure, you might need to handle file placement differently:

1. **AWS S3 + CloudFront** :
   - If using AWS S3 and CloudFront for a static website, upload `zast.txt` to the S3 bucket in a `.well-known` directory.
   - Ensure CloudFront is configured to serve files from this path.
2. **Google Cloud Storage** :
   - Similar to AWS, upload the `zast.txt` file to the bucket in a `.well-known` directory and make it publicly accessible.
3. **Azure Storage** :
   - Similar to AWS, upload the `zast.txt` file to a `.well-known` directory and make it publicly accessible.
   - Step 1: Create the `.well-known` directory structure
     - **Log in to Azure Portal** and navigate to your **Storage Account** .
     - Find the Blob Containers section in the Storage Account and open the container where your website is hosted (usually named `\$web` for static websites).
     - Inside the blob container, create a directory (folder) named `.well-known`. Azure Blob Storage supports "virtual directories," so creating a folder is essentially a part of the blob name.
       - In Azure Storage Explorer, you can create the folder `.well-known` directly by appending it to the file path.
       - If using the Azure portal, you can create it by uploading a file to the desired path with the name, including the directory (e.g., `/.well-known/zast.txt`).
   - Step 2: Upload the `zast.txt` file
     - Once the `.well-known` directory is created, upload the `zast.txt` file inside it.
       - In the Azure Portal, navigate to the `.well-known` directory and click the **Upload** button.
       - Select your `zast.txt` file and ensure it's placed inside the `.well-known` directory.
   - Step 3: Set Blob Properties (Optional)
     - After uploading the file, ensure the **Blob's access level** allows it to be publicly accessible. This can be done in the Azure Portal:
     - Go to the **Blob properties** of `zast.txt`.
     - Set the **Blob's access level** to **Public** (if not already set).

## Troubleshooting Tips

- **Permission Errors** : Ensure the `zast.txt` file and its directory have the correct permissions to be served by the web server.
- **Caching Issues** : If using a caching layer like Varnish or Cloudflare, purge the cache to ensure the new file is served correctly.
- **File Not Found** : Double-check the file path and URL to match the required format (`[your-domain]/.well-known/zast.txt`).

## Conclusion

Proving website ownership before using Zast.ai's powerful AI scanning capabilities is essential to ensure ethical usage and protect websites from unauthorized scanning. While placing the `zast.txt` file can vary depending on your website's architecture, this guide provides the necessary steps for typical setups like Nginx, Apache, and cloud hosting services.

Verifying ownership enables you to harness Zast.ai’s full potential and aligns with best security practices, ensuring that only you can authorize website scans.

## SEO Optimized Keywords

#zastai #websiteownership #websecurity #zeroday #nginxsetup #apacheconfiguration #websiteverification #reverseproxy #cloudhosting #contentmanagementsystems #securityscanning #aiagent
