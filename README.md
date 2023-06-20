# Project Bellflower : Step Function Callback Pattern Example (Amazon SQS, Amazon SNS, Lambda).

A State Machine demonstrating Lambda orchestration with a third party application using S3 and SQS and Lambda Callback functionality and notifying the users using SNS notification.

## Description

This is demonstration of a State Machine with Lambda, SNS, SQS and S3. The processing Lambda generates a payload and pushes to a SQS Queue. An integration Lambda processes the payload and send a file with task token to S3 outbound folder. An Business Application processes the information and sends a success trigger to the S3 inbound folder. The integration Lambda reads the success trigger and sends a success signal to the State Machine and resumes processing. Once processing completes, the State Machine sends a Success notification to SNS Topic. If the processing Lambda fails after 3 retries the State Machine fails and a Failure notification is sent to the SNS Topic..

![Project Bellflower - Design Diagram](https://subhamay-projects-repository-us-east-1.s3.amazonaws.com/0038-bellflower/bellflower-architecture-diagram.png?)

![Project Bellflower - Services Used](https://subhamay-projects-repository-us-east-1.s3.amazonaws.com/0038-bellflower/bellflower-services-used-cft.png?)

![Project Bellflower - State Machine](https://subhamay-projects-repository-us-east-1.s3.amazonaws.com/0038-bellflower/bellflower-state-machine.png?)


## Getting Started

### Dependencies

* Create a Customer Managed KMS Key in the region where you want to create the stack.
* Modify the KMS Key Policy to let the IAM user encrypt / decrypt using any resource using the created KMS Key. Following the kms-key-policy.json and use it after replacing the AWS Account Id.

### Installing

* Clone the repository.
* Create a S3 bucket to store the CloudFormation nested stack templates and the Lambda code zip files.
* Create the folders - bellflower/cft/nested-stacks, bellflower/cft/cross-stacks, bellflower/code/python, bellflower/cft/state-machine
* Upload the following YAML templates to bellflower/cft/nested-stacks
    * iam-role-stack.yaml
    * lambda-function-stack.yaml
    * s3-stack.yaml
    * sns-stack.yaml
    * sqs-stack
    * cloudwatch-stack.yaml
* Upload the following YAML templates to bellflower/cft/cross-stacks
    * custom-resource-lambda-stack.yaml
* Upload the following YAML templates to bellflower/cft/
    * bellflower-root-stack.yaml
* Zip and Upload the following Python files  to bellflower/code/python
    * processing_lambda.py (processing_lambda.zip)
    * integration_lambda.py (integration_lambda.zip)
* Upload the ASL file state-machine.asl.json to bellflower/cft-state-machine
* Create the cross-stack using the template custom-resource-lambda-stack.yaml by using the S3 url and pass the appropriate parameters.
* Create the entire using by using the root stack template custom-resource-lambda-stack.yaml by providing the required parameters and the s3 cross stack name created in the previous step.

### Executing program

* Execute the state machine with default payload. You can use the sample provided below:
```
{
    "Comment": "Bellflower - Callback Pattern Example (Amazon SQS, Amazon SNS, Lambda)"
}
```
![Project Bellflower - Start Execution ](https://subhamay-projects-repository-us-east-1.s3.amazonaws.com/0038-bellflower/bellflower-state-machine-start-execution.png?)


![Project Bellflower - Waiting for callback ](https://subhamay-projects-repository-us-east-1.s3.amazonaws.com/0038-bellflower/bellflower-state-machine-waiting-for-response.png?)

* Get the UUID of the execution from the log or from the S3 bucket outbound folder.
* Create a success.json file in the bucket s3://<s3 bucket uri>/inbound/<UUID>/
* Copy the success.json to S3 inbound folder
```
aws s3 cp success.json s3://<s3 bucket uri>/inbound/<UUID>/
```
![Project Bellflower - Upload the success trigger to S3 ](https://subhamay-projects-repository-us-east-1.s3.amazonaws.com/0038-bellflower/bellflower-state-machine-upload-success-trigger.png?)

![Project Bellflower - State Machine Succeeds ](https://subhamay-projects-repository-us-east-1.s3.amazonaws.com/0038-bellflower/bellflower-state-machine-succeeds.png?)

## Help

Post message in my blog (https://blog.subhamay.com)


## Authors

Contributors names and contact info

Subhamay Bhattacharyya  - [subhamay.aws@gmail.com](https://blog.subhamay.com)

## Version History

* 0.1
    * Initial Release

## License

This project is licensed under Subhamay Bhattacharyya. All Rights Reserved.

## Acknowledgments

AWS Documentation (https://docs.aws.amazon.com/step-functions/latest/dg/callback-task-sample-sqs.html)
