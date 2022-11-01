

## Lambda@Edge

Lambda@Edge provides the ability to execute a Lambda function at an Amazon CloudFront Edge Location. 

Origin Response – This event is triggered after the origin returns a response to a request. It has access to the response from the origin.

What are security headers?

Security headers are a group of headers in the HTTP response from a server that tell your browser how to behave when handling your site’s content. For example, X-XSS-Protection is a header that Internet Explorer and Chrome respect to stop pages loading when they detect cross-site scripting (XSS) attacks. The following is a list of each header we’ll be implementing with a link to more information.

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
  
- kms:ListAliases – To view keys in the Lambda console. 
- kms:CreateGrant, kms:Encrypt – To configure a customer managed CMK on a function. 
- kms:Decrypt – To view and manage environment variables that are encrypted with a customer managed CMK. 
  
 
