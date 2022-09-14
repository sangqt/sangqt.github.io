# How to deploy GitHub pages with AWS Route 53 registered custom domain and force HTTPS
In this guide I will explain how to deploy a website to GitHub pages forcing HTTPS over a custom domain that is registered with AWS Route 53. We will set up our domain so that the www subdomain will redirect to the apex domain.

**Summary**
- Set up the GitHub repo
- Commit and push an index.html or use Jekyll
- Configure AWS Route 53
### Step 1: Create GitHub repo and turn on GitHub Pages
If it does not exist yet, create a repository using the naming pattern your-github-username.github.io. You can follow this tutorial to create your GitHub Page.[GitHub Page quickstart](https://docs.github.com/en/pages/quickstart)
### Step 2: Push source code to GitHub

1. Clone the repo to your local machine

    `git clone git@github.com:your-github-username/your-github-username.github.io.git && cd your-github-username.github.io`
2. Create an index.html file with some content

    `echo "Hello GitHub Pages!" > index.html`
3. Looking forward, we will need to have a file named CNAME that contains a single row: your custom domain. For example: example.com

4. Push the files to GitHub

    `git add . && git commit -m 'Create content and CNAME record' && git push`

Visit http://your-github-username.github.io and https://your-github-username.github.io. You should see the contents of your index.html file at both the unsecured and secured addresses.

### Step 3: Configure AWS Route 53 to use your custom vanity domain
#### Configure main domain
1. Log into the AWS console and go to the [Route 53 dashboard](https://us-east-1.console.aws.amazon.com/route53/v2/home#Dashboard).
2. Click Hosted zones
3. Click the domain you would like to use
4. Click *Manage DNS*
5. Click the hosted zone that has the same name with your domain
6. Click *Create record*
7. Do not enter anything into the Record Name (*subdomain*) field
8. Under the Type dropdown, select A — IPv4 addresses
9. No to Alias 
10. Enter the following four IP addresses into the value text area. Then click Save Record Set.
	```
    185.199.108.153
    185.199.109.153
    185.199.110.153
    185.199.111.153
    ```
11. Click Create Records

#### Configure sub domain
1. Click Create Record, again
2. Into the Recod Name field, enter www
3. Under the Type dropdown, select CNAME
4. The Alias toggle should be set to Yes
5. Set to your github page
6. Click Create Records

### Step 4: Configure GitHub to serve over your custom domain
1. Return to your GitHub repository’s settings tab
2. Scroll down to the GitHub Pages section
3. In the Custom domain field enter your custom domain: your-custom-domain.com
4. Click Save
5. Check DNS
6. Check Enforce HTTPS

### Step 6: Confirm that your page is accessible at your custom domain
Visit https://your-custom-domain.com. You should see the contents of your GitHub Page.
