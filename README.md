Project Instructions:

AWS Lambda Exercise
Due Monday by 11:59pm Points 2 Submitting a file upload File Types png and jpg
Complete the exercise in Typescript.

In this exercise you will:

Write an AWS Lambda function that sends email messages using the AWS Simple Email Service (SES).
Transpile the Typescript lambda code into Javascript.
Zip the lambda dependencies and create a lambda layer for the lambda.
Create an AWS IAM role that provides your Lambda function the security permissions it requires.
Deploy your Lambda function using your JAR file and IAM role.
Test the Lambda function using a test event.
Assumptions
The AWS credentials are configured on the computer.  This should have been done when installing the AWS CLI on the computer.

Steps
By default, to send emails using the Simple Email Service (SES), the to and from emails must be verified.
Login to the AWS console
Navigate to the Simple Email Service
Under Configuration/Verified identities on the right side of the SES screen, Select "Create Identity"
Select the "Email Address" option and enter your email address
Do this for each of the email addresses you want to send SES emails to and from.
When deploying an AWS Lambda, assign it an IAM role that specifies what security permissions the Lambda should have when it runs.  For this purpose, create an IAM role that defines the security permissions your Lambda function should have when it executes.
Login to the AWS console
Navigate to the IAM service
On the left of the IAM service screen, select “Roles”
Click the “Create role” button
When asked to “Select type of trusted entity”, select “AWS Services”, and under "Choose the service that will use this role", select "Lambda" and click the Next button
On the “Attach permissions policies” screen, attach the AmazonSESFullAccess policy to your role.  This will allow your Lambda function to send emails using the SES service.  You can find the AmazonSESFullAccess role by entering “SES” in the search field. 
Also attach the CloudWatchLogsFullAccess policy to your role.  This will allow your Lambda function to log messages using the AWS CloudWatch service.  After doing that, click the Next button.
Skip the “Add tags” screen by clicking the Next button
On the “Review” screen, enter a name for your role (e.g., cs340lambda or whatever you like), and click the “Create role” button
The code is already fully written and just needs to be downloaded: lambda-starter-code.zipDownload lambda-starter-code.zip
There is a Main class that can be changed to test the code from the local computer, but it does not get used from within lambda.
Transpile the code using tsc.
Run npm install to install the dependencies referenced in package.json
We already created a build script in package.json so you can transpile by running npm run build
Zip the transpiled code.
(Optional): To exclude the parent directory, zip the contents of the dist folder rather than the dist folder
Next, deploy the Lambda function.
Login to the AWS console
Navigate to the Lambda service
Select “Functions” on the left side of the Lambda screen
Click the “Create Function” button in the top-right corner
Give your function a name (e.g., send_email)
As your function’s Runtime select “Node.js 20.x” if it is not selected by default.
Under Permissions, click “Change default execution role”
Under “Execution role”, select “Use an existing role”
In the “Existing role” drop-down, select the name of the IAM role that you created in a previous step (e.g., cs340lambda)
Click the “Create function” button.  This will actually create the function, and take you to a screen that lets you further configure the function.
Scroll down to the “Runtime settings” section of the function configuration screen.
In the “Handler” field, specify the name of the function that contains your Lambda function handler (e.g., lambda/EmailSender.handler, or dist/lambda/EmailSender.handler)
Scroll up to the "Code Source" section.
Click the "Upload from" dropdown.
Select ".zip or .jar file"
Click the "Upload button and select the zip file created in the previous step and click "Save".
Create a lambda layer and connect it to the lambda.
Since tsc does not transpile the code dependencies located in the node_modules folder, the lambda will not recognize the code's import statements. You will create a lambda layer that contains the dependencies, and connect the lambda layer to the lambda. The structure of the lambda must match the following, nodejs -> node_modules -> [dependencies].
Create a folder called nodejs in your VS Code project.
Copy the node_modules folder into nodejs. Make sure to copy the node_modules folder itself, and not just its contents.
Zip the nodejs folder. Make sure to zip the nodejs folder itself and not just its contents.
Create the lambda layer in the aws console.
In the aws console, open up the lambda service, and select layers on the left panel.
Click Create Layer.
Give your layer a name (e.g. sesExerciseDependencies)
Upload the zipped nodejs file.
Select Node.js 20.x as a Compatible Runtime.
Click Create.
Connect the lambda layer to the lambda.
Within the lambda that you created previously, scroll down to the layers section.
Click Add a Layer.
Select Custom Layers.
Choose the lambda layer created earlier.
If the lambda layer does not show up, its runtime configuration might not have been set to the same as the lambda.
Select a version from the "Version" drop down (there will probably only be one to choose from)
Click Add.
Now, test the Lambda function.  This can be done as follows:
Below the Function Overview, select the "Test" tab
An editor for Test Events should appear.  A test event is simply a JSON object that you will send to your Lambda function to see if it works.
Specify a name for the test event (e.g., SendEmail)
Fill in the values for the following test JSON object.  Adjust the field names if they do not match the EmailRequest class created earlier. The JSON object’s attributes should start with lower-case letters.  (AWS will automatically deserialize the JSON object into an EmailRequest javascript object)
{
"to": "TODO: Add email.",
"from": "TODO: Add email.",
"subject": "TODO: Add Subject.",
"textBody": "TODO: ADD text.",
"htmlBody": "TODO: ADD text."
}
Click the “Save” button at the top. 
Execute the test event by clicking the “Test” button.  This will execute the Lambda function with the test event JSON object.
The output of the function will be displayed in the console. You should also receive the test email at the 'to' address. For more details, click the "monitor" tab and click "View CloudWatch logs" to view the complete CloudWatch log for the Lambda function.
