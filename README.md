# cfn-appstack
Cloudformation template (JSON) for a simple application stack.

Included is a simple bash script, provisionCloudFormation, which is just a wrapper around the cloudformation CLI.
Example with your template in the current directory:
    ./provisionCloudformation.sh dixonaws@amazon.com cfn-appstack.template.json

You can tear down a stack using the cloudformation CLI:
    aws --profile dixonaws@amazon.com --region us-east-1 cloudformation delete-stack --stack name [foo]

Of course, you'll need to define your AWS credential in your profile, per http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html

The template creates the following resources:
<ol type="1">
<li>Web servers in an autoscaling group (WebServerASG)</li>
  <ol type="a">
  <li>3 web servers in us-east-1 across AZs a, c, and d</li>
  <li>Stubs for policy adjustments based on CPU metrics - CPUAlarmLow and CPUAlarmHigh (min and max is 3 in this example template)</li>
  </ol>
<li>A launch configuration that references an Amazon Linux AMI (WebServerLC)</li>
  <ol type="a">
    <li>Instance size m1.small, 16GB root partition</li>
    <li>httpd and lynx installed from Amazon yum repos</li>
    <li>Simple index.html installed to /var/www/html/index.html</li>
    </ol>
<li>A security group for the web servers (WebInstanceSecurityGroup)</li>
<ol type="a">
    <li>TCP 80, 443, 22 are allowed inbound access</li>
    </ol>
<li>A public-facing ELB attached to the autoscaling group (WebELB)</li>
<ol type="a">
    <li>TCP 80 is open to the world</li>
    <li>Instances listen on TCP 80</li>
    <li>Healthcheck pings TCP 80 on each instance</li>
    <li>The ASG itself and the instances are tagged with Name and Application_ID keys</li>
    </ol>
<li>A ForgeRock OpenAM server installed in a random AZ in us-east-1 (AZ a or c) (OpenAMASG)</li>
<ol type="a">
    <li>based on ami-84311293</li>
    <li>Access at http://serverurl:8080/openam</li>
    </ol>
<li>1 application server installed in a random AZ in us-east-1 (AZ a or c) (AppServerASG)</li>
<ol type="a">
<li>Apache Tomcat 7 installed in an Amazon Linux OS</li>
<li>TCP 8080 open to the world</li>
<li>Access at http://serverurl:8080/</li>
    </ol>

The only parameter supported is KeyName: the name of the private key that will be used to access the instances created by the template. This key must exist before running the template.
