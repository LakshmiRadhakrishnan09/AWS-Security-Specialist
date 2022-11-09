

## Lambda@Edge

Lambda@Edge provides the ability to execute a Lambda function at an Amazon CloudFront Edge Location. 

Origin Response – This event is triggered after the origin returns a response to a request. It has access to the response from the origin.

What are security headers?

Security headers are a group of headers in the HTTP response from a server that tell your browser how to behave when handling your site’s content. For example, X-XSS-Protection is a header that Internet Explorer and Chrome respect to stop pages loading when they detect cross-site scripting (XSS) attacks. The following is a list of  header :

- Strict Transport Security
- Content-Security-Policy
- X-Content-Type-Options
- X-Frame-Options
- X-XSS-Protection
- Referrer-Policy

## Accessing Amazon CloudWatch logs for AWS Lambda

Lambda automatically integrates with CloudWatch Logs and pushes all logs from your code to a CloudWatch Logs group associated with a Lambda function, which is named /aws/lambda/<function name>.You can view logs for Lambda functions using the Lambda console, the CloudWatch console, the AWS Command Line Interface (AWS CLI), or the CloudWatch API. Your execution role needs permission to upload logs to CloudWatch Logs. You can add CloudWatch Logs permissions using the AWSLambdaBasicExecutionRole AWS managed policy provided by Lambda. 
  
Lambda automatically records a variety of standard metrics that are always published to CloudWatch metrics.   

### Lambda Accessing KMS

 Lambda doesnt use EnvelopEncryption. So doesnt need GenerateDataKey 

Default encryption key: No permissions are required for your user or the function's execution role. 
Customer managed key: need permission to use the key. Lambda uses your permissions to create a grant on the key. This allows Lambda to use it for encryption.
  
- kms:ListAliases – To view keys in the Lambda console. 
- kms:CreateGrant, kms:Encrypt – To configure a customer managed CMK on a function. 
- kms:Decrypt – To view and manage environment variables that are encrypted with a customer managed CMK. 
  
 ### Lambda Execution role and resource policies

Execution Role: a policy that defines the permissions that your lambda function needs to access other AWS services and resources. 

To give other accounts and AWS services permission to use your Lambda resources, use a resource-based policy.

When an AWS service such as Amazon Simple Storage Service (Amazon S3) calls your Lambda function, Lambda considers only the resource-based policy.

To restrict which lambda functions can access other resources, include the lambda:SourceFunctionArn condition key in a resource-based policy for the target resource. 

### Lambda Environment Variables

For securing your environment variables, you can use server-side encryption to protect your data at rest and client-side encryption to protect your data in transit.

Lambda always encrypts environment variables at rest. By default, Lambda uses an AWS KMS key that Lambda creates **in your account** to encrypt your environment variables. This AWS managed key is named aws/lambda.
On a per-function basis, you can optionally configure Lambda to use a customer managed key instead of the default AWS managed key to encrypt your environment variables. 

Lambda always encrypts files that you upload to Lambda, including deployment packages and layer archives.

Under Encryption in transit, choose Enable helpers for encryption in transit. Under AWS KMS key to encrypt in transit, choose a customer managed key that you created.
