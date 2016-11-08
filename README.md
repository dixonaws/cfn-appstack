# cfn-appstack
Cloudformation template (JSON) for a simple application stack.

Included is a simple bash script, provisionCloudFormation, which is just a wrapper around the cloudformation CLI.
Example with your template in the current directory:
    ./provisionCloudformation.sh dixonaws@amazon.com cfn-appstack.template.json
    
You can tear down a stack using the cloudformation CLI:
    aws --profile dixonaws@amazon.com --region us-east-1 cloudformation delete-stack --stack name [foo]
    
Of course, you'll need to define your AWS credential in your profile, per http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html

The template creates the following resources:<ol>
<li>1. Web servers in an autoscaling group (WebServerASG)</li>
<li>a. 3 web servers in us-east-1 across AZs a, c, and d</li>
    b. Stubs for policy adjustments based on CPU metrics - CPUAlarmLow and CPUAlarmHigh (min and max is 3 in this example template)
2. A launch configuration that references an Amazon Linux AMI (WebServerLC)
    a. Instance size m1.small, 16GB root partition
    b. httpd and lynx installed from Amazon yum repos
    c. Simple index.html installed to /var/www/html/index.html
3. A security group for the web servers (WebInstanceSecurityGroup)
    a. TCP 80, 443, 22 are allowed inbound access
4. A public-facing ELB attached to the autoscaling group (WebELB)
    a. TCP 80 is open to the world
    b. Instances listen on TCP 80
    c. Healthcheck pings TCP 80 on each instance
    d. The ASG itself and the instances are tagged with Name and Application_ID keys
5. A ForgeRock OpenAM server installed in a random AZ in us-east-1 (AZ a or b) (OpenAM-ASG)
    a. based on ami-84311293
    b. Access at http://serverurl:8080/openam
    </ol>
    
The only parameter supported is KeyName: the name of the private key that will be used to access the instances created by the template. This key must exist before running the template.
