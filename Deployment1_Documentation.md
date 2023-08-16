**GitHub**
Create a new repository

Under the tab `Add file`, select `Upload files`.

Upload files from your local server to your GitHub repository.

**Jenkins**
Log into your Jenkins server. 

Go through the process of creating and running a Jenkins build for the application. This will allow us to test out our application for any bugs, syntax errors, etc., prior to us deploying it.

Go through the process of entering a name for your Jenkins build pipeline.

On the `Configure` page, scroll down to `Pipeline`, and select `Pipeline script from SCM`.

To automate the build process, in your GitHub repo, the `Jenkinsfile` will have steps that will do that, using a language called Groovy, that sits on top of Java, which is a language Jenkins understands.

Under `SCM`, click on `Git`.

Enter your GitHub repository link. 

Add credentials. 

For credentials, click on `Add`, then `Jenkins`.

Enter your GitHub username; for the password, you will generate a token:

- In GitHub, go to Settings, Developer Settings, and Personal Access Tokens.

- Click on `Token Classic`. Generate a new token, take that code you're given, and then give it `Repo, private public access, admin repo hook, etc. permissions as you'll require.

- Go back to Jenkins and enter it in the password section on Jenkins.

For ID, put GitHub.

Once added, go back to Credentials to find your username.

Change `Branch to Build` to main.

Scroll down to click `Apply` and then `Save`.

Then, start your build under `BuildNow`.

You can look into your build and test logs, to see what happened during that run.

If the pipeline is successful, download the application files from your repository and proceed to deploy to AWS ElasticBeanstalk

**AWS Elastic BeanStalk:**

We'll now create and deploy a Python URL shortener on AWS Elastic BeanStalk.

A URL shortener takes a long, messy-looking website address, like "HTTP://www.verylongwebsiteaddress.com/page/subpage/example?query=true",

and shortens it into something much simpler and shorter like "bit.ly/abc123"

For a faster deployment, and to avoid a file-size limit issue, zip up the file components of your application.

Please note: On your local server, make sure you are in the file directory where your .zip file is, and remove files you don't want to be included. It's best practice to create a separate folder, just as you'd create a separate repository for a project.

Next, create your IAM permissions.

IAM permissions are important because this is what will allow ElasticBeanStalk to "act as us" and assume our role; The IAM Role will allow ElasticBeanstalk to create EC2 instances, autoscaling groups, VPCs, and any other configurations we'll require

in order to deploy our specific application. It's able to manage the infrastructure so we don't have to.

Navigate to your AWS Console, and head to your IAM Dashboard. On the left-hand side, select `roles`. 

Then, select, `Create Role`.

Click the "AWS Service" that allows AWS services like EC2, Lambda, or others to perform actions in this account." field and then Elastic Beanstalk - Customizable

After clicking next, click the "Role name" field and enter "aws-elasticbeanstalk-service-role"

Click "Create role"

"Create role" to create a second Role; Go through the same process, but this time, click the "EC2Allows EC2 instances to call AWS services on your behalf." field.

Click `Next`

Filter policies by property or policy name and press enter." field and find "AWSElasticBeanstalkWebTier", "AWSElasticBeanstalkWorkerTier", and "AWSElasticBeanstalkMulticontainerDocker"

On the next page, click the "Role name" field and enter "Elastic-EC2", then create the role.

Navigate to the AWS Elastic BeanStalk console to create a new application. 

Rather than provisioning and managing the platform, we can just upload our code, and move on to the next one, which is a benefit

of platform-as-a-service; No need to manage virtual machines or install server software yourself.

Give your application a name, and under the `Platform` Tab, select Python. We are telling ElasticBeanstalk that the application was developed and runs in Python.

Specify the `Platform-Branch` to be even more detailed. Select `Python 3.9 running on 64bit Amazon Linux 2023`.

Upload your application code from your local computer and select `v1` for the version label.

Upload your .zip file.

Click `Next`.

For the `EC2 instance profile`, click on the IAM role we previously created, *ElasticEC2*. 

The role we created will allow our EC2 instance to perform operations such as autoscaling as needed.

Click `Next`.

Under `VPC (virtual private cloud)`, select your default VPC. The VPC allows you to have your own slice of the private cloud that no one without permissions can access.

Click `Next`.

Configure your root volume to General Purpose (SSD), GB of 10, and an instance type of T.2 MICRO.

Continue to click `Next` until you see the page with `Submit`.

Click `Submit`.

If configured properly, the health status should say "OK". If not, navigate to the Elastic BeanStalk log tab and request the last 100 logs. Download and view the logs, under `/var/log/web.stdout.log`.

The health status could degrade for a number of reasons, such as availability zone issues. 

Elastic Beanstalk deploys EC2 instances across multiple AZs automatically for high availability. If some AZs are down, it may not be able to deploy instances in the ideal configuration across AZs.

Once you see the issue in the logs, you can take action such as restarting the environment, re-creating it, trying a different AZ/Region, etc. 

Once the environment's health status says "OK", you can select your Domain Name.
