**GitHub**

- Create a new repository

<img width="362" alt="Screen Shot 2023-08-15 at 6 32 58 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/3d810d81-62ed-42db-97f7-59e42d5586b1">

- Under the tab `Add file`, select `Upload files`.

- Upload files from your local server to your GitHub repository.

**Jenkins**

- Log into your Jenkins server.

<img width="1428" alt="Screen Shot 2023-08-15 at 6 44 07 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/bf485f3d-f180-46f6-b5cd-1d1889c09bc7">

- Go through the process of creating and running a Jenkins build for the application. This will allow us to test out our application for any bugs, syntax errors, etc., prior to us deploying it.

- Go through the process of entering a name for your Jenkins build pipeline.

<img width="1366" alt="Screen Shot 2023-08-15 at 6 47 32 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/930041ce-2a16-47e2-9b3d-644e6fe39271">

- On the `Configure` page, scroll down to `Pipeline`, and select `Pipeline script from SCM`.

- To automate the build process, in your GitHub repo, the `Jenkinsfile` will have steps that will do that, using a language called Groovy, that sits on top of Java, which is a language Jenkins understands.

- Under `SCM`, click on `Git`.

- Enter your GitHub repository link. 

- Add credentials. 

- For credentials, click on `Add`, then `Jenkins`.

- Enter your GitHub username; for the password, you will generate a token:

- In GitHub, go to Settings, Developer Settings, and Personal Access Tokens.

- Click on `Token Classic`. Generate a new token, take that code you're given, and then give it `Repo, private public access, admin repo hook, etc. permissions as you'll require.

- Go back to Jenkins and enter it in the password section on Jenkins.

- Under `ID`, put GitHub.

- Once added, go back to `Credentials` to find and select your username.

- Change `Branch to Build` to main.

- Scroll down to click `Apply` and then `Save`.

- Start your build under `BuildNow`.

- You can look into your build and test logs, to see what happened during that run.

<img width="1366" alt="Screen Shot 2023-08-15 at 7 12 16 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/de2c77dc-08bf-463c-99b0-89ba16d3ae63">


- If the pipeline is successful, download the application files from your repository and proceed to deploy to AWS ElasticBeanstalk.

<img width="1366" alt="Screen Shot 2023-08-15 at 7 49 31 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/ab1b2ba7-97f4-481c-bd97-6062a88f88e0">

**AWS Elastic BeanStalk:**

We'll now create and deploy a Python URL shortener on AWS Elastic BeanStalk.

A URL shortener takes a long, messy-looking website address, like `HTTP://www.verylongwebsiteaddress.com/page/subpage/example?query=true`, and shortens it into something much simpler and shorter like `bit.ly/abc123`.

For faster deployment, and to avoid a file-size limit issue, zip up the files that make up your application.

**NOTE: On your local server, ensure you are in the file directory where your .zip is, and remove files you don't want to be included. It's best practice to create a separate folder, just as you'd create a separate repository for a project**

- Next, create your IAM permissions.

- IAM permissions are important because this is what will allow ElasticBeanStalk to "act as us" and assume our role; The IAM Role will allow ElasticBeanstalk to create EC2 instances, autoscaling groups, VPCs, and any other configurations we'll require in order to deploy our specific application. It's able to manage the infrastructure so we don't have to.

- Navigate to your AWS Console, and head to your IAM Dashboard. On the left-hand side, select `roles`. 

- Select, `Create Role`.

- Click the AWS Service that allows `AWS services like EC2, Lambda, or others to perform actions in this account.` field and then Elastic Beanstalk - Customizable

- After clicking next, click the `Role name` field and enter *aws-elasticbeanstalk-service-role*.

- Click `Create role`.

*`Create role`* to create a second Role; Go through the same process, but this time, click the `EC2Allows EC2 instances to call AWS services on your behalf.` field.

- Click `Next`

- Filter the policies by property or policy name and press enter. Find and select `AWSElasticBeanstalkWebTier`, `AWSElasticBeanstalkWorkerTier`, and `AWSElasticBeanstalkMulticontainerDocker`.

- On the next page, click the `Role name` field and enter `Elastic-EC2`, then create the role.

<img width="855" alt="Screen Shot 2023-08-15 at 7 53 59 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/c889b69f-e3c6-468e-b31f-3a8a325f7d84">

- Navigate to the AWS Elastic BeanStalk console to create a new application. 

- Rather than provisioning and managing the platform, we can just upload our code, and move on to the next one, which is a benefit of platform-as-a-service; no need to manage virtual machines or install server software yourself.

- Give your application a name, and under the `Platform` Tab, select Python. We are telling ElasticBeanstalk that the application was developed and runs in Python.

<img width="863" alt="Screen Shot 2023-08-15 at 7 57 55 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/e48f99ad-ad10-4c73-8a82-079b07b742b6">

- Specify the `Platform-Branch` to be even more detailed. Select `Python 3.9 running on 64bit Amazon Linux 2023`.

<img width="863" alt="Screen Shot 2023-08-15 at 7 57 59 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/0607b112-90fe-40a8-9329-d4899763e2ef">

- Upload your application code from your local computer and select `v1` for the version label.

- Upload your .zip file.

- Click `Next`.

- For the `EC2 instance profile`, click on the IAM role we previously created, *ElasticEC2*. 

- The role we created will allow our EC2 instance to perform operations such as autoscaling as needed.

- Click `Next`.

- Under `VPC`, select your default VPC. The VPC allows you to have your own slice of the private cloud that no one without permission can access.

- Click `Next`.

- Configure your root volume to `General Purpose (SSD)`, 10 `GB`, and an instance type of `T2.MICRO`.

- Continue to click `Next` until you see the page with `Submit`.

- Click `Submit`.

- If configured properly, the health status should say `OK`. If not, navigate to your terminal, and request the last 100 logs. Download and view the logs, under `/var/log/web.stdout.log`.

<img width="841" alt="Screen Shot 2023-08-15 at 8 16 29 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/eae82ec5-a934-4cfe-8369-40aa1107ddca">

- The health status could degrade for a number of reasons, such as availability zone issues. 

- Elastic Beanstalk deploys EC2 instances across multiple AZs automatically for high availability. If some AZs are down, it may not be able to deploy instances in the ideal configuration across AZs.

- Once you see the issue in the logs, you can take action such as restarting the environment, re-creating it, trying a different AZ/Region, etc.

<img width="1159" alt="Screen Shot 2023-08-15 at 8 18 29 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/f89eecde-82cb-489d-81cc-e4cc1000f755">


- Once the environment's health status says `OK`, you can select your Domain Name.

<img width="1159" alt="Screen Shot 2023-08-15 at 8 20 20 PM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/7b677a16-bd40-457d-ac16-967a7e0f1565">

<img width="1420" alt="Screen Shot 2023-08-17 at 8 54 11 AM" src="https://github.com/belindadunu/Deployment_1/assets/139175163/349674a0-fd26-451a-a30b-f7c143bf8e9f">

