### Deploying a Django application to AWS Elastic Beanstalk with HTTPS

### Prerequisites
Before trying to deploy a Django application to AWS Elastic Beanstalk, make sure that you have a working knowledge of the following:
 - [Python](https://www.python.org/): General purpose programming language.
 - [Django](https://www.djangoproject.com/): Web framework for Python.
 - [AWS](https://aws.amazon.com/): Cloud computing platform. (We will assume you have an AWS account)

Finally, one article that may be helpful to review is [Building RESTful APIs using Django Rest Framework](./Django_Rest.md)
- This article will give you the basics of how to setup a Django application and create some useful routes to be deployed to AWS Elastic Beanstalk.

### Steps

#### 1. Create a Django application
- Create a Django application using the steps outlined in [Building RESTful APIs using Django Rest Framework](./Django_Rest.md)
- Make sure to create a virtual environment for your Django application and install all dependencies in the virtual environment.
- Make sure to create a requirements.txt file for your Django application. This file will be used by AWS Elastic Beanstalk to install all dependencies for your application.
  - A quick and easy way to do this is to run the following command in your virtual environment:
    - `pip freeze > requirements.txt`
    - This will create a requirements.txt file with all the dependencies installed in your virtual environment.
- Make sure to create a .gitignore file for your Django application. This file will be used to tell git which files to ignore when pushing to your repository.
  - A quick and easy way to do this is to run the following command in your virtual environment:
    - `echo "venv" >> .gitignore`
    - This will create a .gitignore file with the venv folder ignored.
- After doing the following your file structure should look something like this:
```
project-name/
│
├── app-name/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── serializers.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
│
├── project-name/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── manage.py
├── requirements.txt
└── .gitignore
```

#### 2. Login to AWS and create a Elastic Beanstalk application
- Login to your AWS account and navigate to the Elastic Beanstalk service.
- Click on the "Create Environment" button.
- Select "Web server environment" and click "Select".
- You can make the application name whatever you want. Same with the environment name. Make sure they are meaningful to you.
- Select "Python" for the platform and "Python 64bit Amazon Linux 2" for the platform branch.
- For Application Code we will select the sample application now
- Lastly make sure to set the preset to Single Instance (this is the only free tier eligible instance)
- On the next page we will have to create and use a new service role
  - Be sure to create a EC2 Instance role to attach, you can refer to this link for more specific instructions [Creating a EC2 Instance Profile](https://docs.matillion.com/metl/docs/2765606/#to-attach-an-iam-role-to-an-instance-using-the-aws-cli)
- Click "Create Environment" and wait for the environment to be created.

#### 3. Deploy our Django application onto the Elastic Beanstalk environment
- Now that we have our elastic beanstalk enviorment created we can deploy our Django application to it.
- We will need to create a zip file of our application first, but there are a couple modifications we need to make to the Django app before
  - First we need to create a .ebextensions folder in the root of our Django application. This folder will contain a config file that will tell AWS Elastic Beanstalk how to deploy our application.
    ```
    option_settings:
      aws:elasticbeanstalk:container:python:
      WSGIPath: test-app.wsgi:application
     ```
    - Next we will go into our settings.py and add our Elastic Beanstalk environment link to the allowed hosts
      - `ALLOWED_HOSTS = ['test-app-env.eba-xxxxxxx.us-east-2.elasticbeanstalk.com']`
    - Lastly, we will need to create a .ebignore file in the root of our Django application. This file will tell AWS Elastic Beanstalk which files to ignore when deploying our application.
      - A quick and easy way to do this is to run the following command in your virtual environment:
        - `echo "venv" >> .ebignore`
        - This will create a .ebignore file with the venv folder ignored.
- Then we need to create a zip file of our Django application. We can do this by running the following command in our virtual environment:
  - `zip -r your_project_name.zip . -x "*.git*" -x "*.ebignore" -x "*.zip" -x "env/*" -x "venv/*"`
  - This will create a zip file of our Django application in our virtual environment.
- We can now go back to our AWS Elastic Beanstalk environment and click on the "Upload and Deploy" button.
- We will then select the zip file we just created and click "Deploy".
- This will take a few minutes to deploy our application. Once it is done we can click on the link in the top right corner to view our application.

Your application should now successfully have been deployed to AWS Elastic Beanstalk. You can now make changes to your Django application and redeploy it to AWS Elastic Beanstalk by following the steps above.

#### 4. Adding HTTPS to our Elastic Beanstalk environment (optional)
- Now that we have our Django application deployed to AWS Elastic Beanstalk we can add HTTPS to our application.
- First we will need to create a SSL certificate for our application. We can do this by going to the AWS Certificate Manager and clicking on "Provision certificates".
- We will then need to add our domain name to the certificate. This will be the domain name of our Elastic Beanstalk environment.
- Once we have added our domain name we can click "Next" and then "Confirm and request".
- We will then need to confirm that we own the domain name by adding a CNAME record to our DNS configuration. This will be different depending on where you purchased your domain name.
- Once we have added the CNAME record we can click "Continue" and then "Request certificate".
- We can now wait for the certificate to be correctly issued. This may take some time.
- Once we obtain the certificate we can head back to the elatic beanstalk instance and click on configuration.
- We will then need to add a load balancer to our environment. We can do this by clicking on the "Modify" button next to "Load Balancer".
- We will then add an application Load Balancer
- Finally we can create a listener on port 443 and add our SSL certificate to the listener.

By following these steps we should now have HTTPS enabled on our Elastic Beanstalk environment.


### Common Errors
1. If you get an error that says "Your WSGIPath refers to a file that does not exist." then you need to make sure that your WSGIPath in your .ebextensions folder matches the name of your project.
   - For example, if your project name is "test-app" then your WSGIPath should be "test-app.wsgi:application"
2. If AWS did not automatically install the dependencies from the requirements.txt you may need to SSH into the instance and install them yourself using pip
   - You can follow this link for more information on how to SSH into your instance [SSH into EB Instance](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-ssh.html)

### Refrences
- [Deploying a Django app to Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-django.html)
- [Creating a EC2 Instance Profile](https://docs.matillion.com/metl/docs/2765606/#to-attach-an-iam-role-to-an-instance-using-the-aws-cli)
- [SSH into EB Instance](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-ssh.html)


