A GitHub app is an app that takes action via the API using their own identity and can be installed on an organization or user accounts. Once installed, the app takes action on behalf of the user or organization.
In order to follow through this tutorial, we need to be familiar with the following:
GitHub Apps
Webhooks
The Ruby programming language
REST APIs
Sinatra
We will start by cloning this repository.
git clone https://github.com/github-developer/using-the-github-api-in-your-app.git
Visit https://smee.io/ and create a new channel.
Starting a new Smee channel creates a unique domain where GitHub can send webhook payloads. You’ll need to know this domain for the next step. Here is an example of a unique domain at https://smee.io/xpab5ZPgnKLBq9:
Now we can go back to the terminal and follow use these commands to run Smee
1. Install the client:
npm install --global smee-client
2. Run the client (replacing https://smee.io/xpab5ZPgnKLBq9 with your own domain):
smee --url https://smee.io/xpab5ZPgnKLBq9--path /event_handler --port 3000
You should be able to see the following output:
Forwarding https://smee.io/xpab5ZPgnKLBq9 to http://127.0.0.1:3000/event_handler
Connected https://smee.io/xpab5ZPgnKLBq9
This basically tells Smee to forward all the webhook events received by Smee channel to Smee client installed on your computer.
I recommend to leave Smee client running though out the tutorial.

The next thing we need to do is to register a GitHub app. To register the app, visit app setting page in your GitHub profile, and click New GitHub App. Follow the following steps to configure the app:
For the “Homepage URL”, use the domain issued by Smee(Mine was https://smee.io/xpab5ZPgnKLBq9):
For the “Webhook URL”, again use the domain issued by Smee.
For the “Webhook secret”, create a password to secure your webhook endpoints. You can run
ruby -rsecurerandom -e 'puts SecureRandom.hex(20)'
to generate a new webhook secret. Please Note it somewhere since we need it in order for our template code to work. We use this secrete to verify the webhook sender. 
4. On the Permissions & Webhooks page, you can specify a set of permissions for your app. For our sample application, we will need read and write access to the issues.
5. In the “Subscribe to events” section, select Issues to subscribe to the event.
6. Click Create GitHub App to create your app!
After creating the app, we are taken back to app setting page where we need to do two more things:
Generate a private key. Find this at the bottom of the page.
Note the App ID. 
We are going to use dotenv gem for managing environment variables. Credentials such as “Web Secrete” and “Private Key” should never be exposed. To use them, we add them as environment variables or use a package that like dontev that was designed to do this on our behalf.
Now let’s open the code template we had cloned earlier, create a .env file in the root directory and add all our environment variables. The following are some of the variables we need to get started.
1. GITHUB_PRIVATE_KEY . This is the private_key we had earlier generated. After generating the key, pem file was downloaded containing the key. To copy the key, we’ll use `cat path/to/your/private-key.pem` command and copy the entire contents of the file as the value for GITHUB_PRIVATE_KEY environment variable.
2. GITHUB_APP_IDENTIFIER: Use the app ID you noted in the previous section.
3. GITHUB_WEBHOOK_SECRET: Add your webhook secret.

We can now run bundle install to install all the gems in the Gemfile.
That’s all we need for the project set up. In case of any issue, feel free to refer to GitHub documentation. 
Read this article where we go ahead and complete our Issue-Responder GitHub app.


