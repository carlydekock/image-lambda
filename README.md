# Lab 17: AWS S3 and Lambda

AWS Lambda allows writing code that is triggered in the cloud, without thinking about maintaining servers. We’ll use it today to automatically run some processing on image files after they’re uploaded to an S3 Bucket. User should be able to upload an image at any size, and have both the original size and a thumbnail size. When an image is uploaded to the S3 bucket, it triggers a Lambda function which resizes the image and saves it to another S3 bucket, doing so with a predictable naming convention.

## Author: Carly Dekock

## Link to GitHub [repo](https://github.com/carlydekock/image-lambda.git)
## Link to PR [here](https://github.com/carlydekock/image-lambda/pull/1)

## Documentation

### How to use Lambda

- AWS Tutorial [here](https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example.html)
- Create two buckets via the Amazon S3 console:
  - First will be your source bucket: mybucketname
  - Second will be the resized bucket: mybucketname-resized
- Upload an image to the source bucket
- Create the IAM Policy for the Lambda function
  - Open the policies page in the IAM console
  - Create policy
  - Under JSON tab, input the policy
  - Choose review policy
  - Specify the policy name: AWSLambdaS3Policy
- Create the execution role that gives your function permission
  - Open the roles page in the IAM console
  - Choose create role
  - Create a role with the following properties: trusted entity (AWS Lambda), permission (AWSLambdaS3Policy) and role name (lambda-s3-role)
- Create the function (demo code in the AWS tutorial)
- Save the function code in an index.js file within a folder named lambda-s3
- In the terminal run the command: ```npm install --arch=x64 --platform=linux --target=12.13.0  sharp```
- Navigate into your lambda-s3 folder
- Run the command: ```zip -r function.zip .```
- From this point, you can either continue on in the browser interface (upload the zip file), or through the command line
- From the command line, next run: ```aws lambda create-function --function-name CreateThumbnail \ --zip-file fileb://function.zip --handler index.handler --runtime nodejs12.x \ --timeout 10 --memory-size 1024 \ --role arn:aws:iam::123456789012:role/lambda-s3-role --cli-binary-format raw-in-base64-out```
- Save the sample event data in a inputFile.txt, and update the JSON with the name of your bucket and image (reference: demo code in the AWS tutorial)
- Run the invoke command: ```aws lambda invoke --function-name CreateThumbnail --invocation-type Event \ --payload file://inputFile.txt outputfile.txt --cli-binary-format raw-in-base64-out```
- You should now have a thumbnail in your -resized bucket.

### Issues encountered

- Permissions issues
- Timeout when trying to connect in the CLI, had to increase the timelimit
- Zip file placement and process, needed to have function.zip within the lambda-s3 folder not at root level

### Images

- Original image [here](/assets/turtle.jpg)
- Resized image [here](/assets/resized-turtle.jpg)
![image](/assets/bucket.png)
![image2](/assets/bucket-resized.png)

### Credits and Collaborators

- Connection issues troubleshooting via GitHub [here](https://github.com/aws/aws-cli/issues/3842)
- Stack overflow article [here](https://stackoverflow.com/questions/37498124/accessdeniedexception-user-is-not-authorized-to-perform-lambdainvokefunction)
- Worked at lab table with Jason D, Jason Q, Nick M, and Seid
