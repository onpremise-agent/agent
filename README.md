# Fillout.com on-premise agent
*Instructions for deploying fillout.com with an on-premise agent.*

**Who is it for?**

The Fillout on-premise agent is best suited for companies with highly sensitive data requirements, like companies that wish to remain compliant with the Health Insurance Portability and Accountability Act (HIPAA) and companies that handle personal identifiable information (PII).

# Getting started

1. [Create a Fillout account](https://app.fillout.com/invite/signup). 
2. Navigate to the "Agent" tab and add an agent. You can leave the host and port blank if you don't know them yet.
&nbsp;
&nbsp;
![Create agent](/images/createAgent.png)
&nbsp;
3. Choose a deployment option and follow the necessary steps [below](#deployment-options). You'll need the Fillout agent key obtained on the screen from step #2.
&nbsp;
4. Once you've deployed the agent, press "Check health" to confirm that the agent was connected successfully.
&nbsp;
&nbsp;



# Deployment options
- [EC2](#ec2)
- [Aptible](#aptible)
- Looking for another option? [Contact us](https://app.fillout.com/flow/iN7kf2ZcRr/basicinfo). The Fillout agent can be deployed on most infrastructure providers.


## EC2 (docker compose)

1. Add the following secrets to your docker.env

```
FILLOUT_AGENT_KEY=<obtained from your fillout.com account, e.g. agent_key_xyzabc>
DB_NAME=<postgres-db-name>
DB_HOST=<postgres-host>
DB_USER=<postgres-username>
DB_PASSWORD=<postgres-user-password>
ENCRYPTION_SECRET=<some-random-string>
DB_PORT=<optional-postgres-port>

```

2. Run `docker-compose up`



## Aptible

1. Install the Aptible CLI and authenticate into your account with `aptible login`.

2. Clone this Github repository: `git clone https://github.com/fillout/onpremise-agent`

3. `cd onpremise-agent`

4. Create a new app on Aptible `aptible apps:create <app-name>`

5. Create a postgres database `aptible db:create <db-name> --type postgresql --version 14`

6. Set the necessary environment variables with the command below. *You'll find the database connection details in the Aptible dashboard and the `FILLOUT_AGENT_KEY` from the [Getting started](#getting-started) steps.*

```
aptible config:set --app <app-name> \
    FILLOUT_AGENT_KEY=<fillout-key-from-fillout-agent-dashboard> \
    DB_NAME=<postgres-db-name> \
    DB_HOST=<postgres-host> \
    DB_USER=<postgres-username> \
    DB_PORT=<optional-postgres-port> \
    DB_PASSWORD=<postgres-user-password> \
    ENCRYPTION_SECRET=$(cat /dev/urandom | base64 | head -c 64) 

```


&nbsp;

7. Set your git remote: `git remote add aptible <your-git-url>` You can find the git url in the Aptible dashboard.

8. If you're not on the `master` branch, switch to the master branch or run `git checkout -b master` to create it. Aptible deploys based on changes to the master branch.

9.  If you haven't already, add a public SSH key to your Aptible account. Then run `git push aptible`.

10. Open the Aptible dashboard and create an Aptible endpoint for your app. Add the endpoint host to the Fillout agent dashboard, so that Fillout knows which host to send requests to.


