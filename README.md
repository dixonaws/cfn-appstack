# cfn-appstack
Cloudformation template (JSON) for a simple application stack.

Included is a simple bash script, provisionCloudFormation, which is just a wrapper around the cloudformation CLI.
Example with your template in the current directory:
    ./provisionCloudformation.sh dixonaws@amazon.com cfn-appstack.template.json
    
You can tear down a stack using the cloudformation CLI:
    aws --profile dixonaws@amazon.com --region us-east-1 cloudformation delete-stack --stack name [foo]
    
Of course, you'll need to define your AWS credential in your profile, per http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html


