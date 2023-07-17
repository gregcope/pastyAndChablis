# pastyAndChablis

AWS Static website for a bit of fun ordering takeaway pizza!

1. A static website with menu
1. User submits a form of their Pizza order
1. Order is emailed and saved in a DB
1. User gets a response

## Aim

In no real order

* Set up a website without servers
* Have a proper cert/domain name
* CDN and origin config, secured to only answer CDN
* Configured as Code
* Learn all the deep details
* Be secure, low cost and good observability
* [NEW] Resilient to region or serice (in one region failure)

## AWS Design

* Cloudfront config, with correct TLS cert tied to a;
* S3 Origin for static assets
* Simple API Gateway secured to a single Lambda
* AWS WAF V2 WebACL to protect the above APIGW with managed rules (e.g. XSS)
* Lambda is secured to DynamoDB, Cloudwatch logs, SES only
* [NEW] Deployed in more than one region
* [NEW] Dynamo DB global tables replication
* [NEW] R53 failover

## Code logic

* Form checks input
* If valid sends an email, saves order to db
* and sends HTML response to user

## Cloudformation

These have been split into, `cloudformation-static.yaml`, `cloudformation-app.yaml` and `cloudformation-waf.yaml`, to make them more managable.  They could be collapsed as both have some shared Parameters for example `RootDomainName` (not used yet, but expected to be added for API GW on a Custom Domain name).

## AWS Simple Email Service verififcaiton

You need to verify both the sending domain and the reciepient (if different) to do this.

Assuming an R53 hosted domain for a domain and any inbox you have access to (for individual email)

1. Login into the AWS console and goto your prefered region
1. Go to the `SES Home`
1. Under `Identity Management`, Choose to;
1. Under `Domains` click `Verify a New Domain` and enter the Domain name, click `Generate DKIM Settings` and click `Verify This Domain`.  Down the bottom click `Use Route 53` Click the `Domain Verificaiton Record` and `DKIM Settings` and then `Create Record Sets`.  Return and refresh and it should show as `Verified`
1. Under the `Email Addresses` click `Verify a New Email Address` and enter the recpient (or sender) and click go.  Access the inbox and look for the email from AWS, and click the link.  Return and refresh and it should show as `Verified`

## Notes

https://www.cs.oberlin.edu/~kuperman/help/vim/indenting.html
* `:set expandtab`
* `:set autoindent`

## Resources

### Static Websites with CF, CF and S3
* https://nickolaskraus.org/articles/creating-a-static-website-using-cloudformation/
* https://hackernoon.com/how-to-configure-cloudfront-using-cloudformation-template-2c263u56
* Region mappings: https://liamraven.com/blog/aws-cloudformation-for-a-static-website
* Serverless contact form with reCaptcha; https://www.storyblok.com/tp/serverless-contact-form-setup

### API GW, Lamdba, SES
* https://acloudguru.com/blog/engineering/how-to-build-a-serverless-contact-form-on-aws
* https://github.com/guardian/ses-send-email-lambda/blob/master/conf/cloudformation.yml
* Best one: https://nickolaskraus.org/articles/creating-an-amazon-api-gateway-with-a-lambda-integration-using-cloudformation/

### AWS WAF and WebACL
* https://github.com/aws-samples/wafv2-json-yaml-samples/blob/master/YAML/snippet-001.yaml
* https://github.com/aws-samples/wafv2-json-yaml-samples/blob/master/YAML/webacl-create-001.yaml
* https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-control-access-aws-waf.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-wafv2-webacl-rule.html

### Python
* https://stackoverflow.com/questions/42445237/looping-through-a-json-array-in-python
* UUID lib: https://docs.python.org/3/library/uuid.html
* https://nickolaskraus.org/articles/transforming-code-into-beautiful-idiomatic-python/

### Resilience / tweaks
* https://repost.aws/knowledge-center/api-gateway-cloudfront-distribution
* https://aws.amazon.com/blogs/compute/building-a-multi-region-serverless-application-with-amazon-api-gateway-and-aws-lambda/
* https://github.com/aws-samples/blog-multi-region-serverless-service/blob/master/helloworld-api/get_healthcheck.py
* https://www.pluralsight.com/resources/blog/cloud/building-a-serverless-multi-region-active-active-backend
* https://aws.amazon.com/blogs/networking-and-content-delivery/improve-web-application-availability-with-cloudfront-and-route53-hybrid-origin-failover/#:~:text=In%20summary%2C%20CloudFront%20Origin%20Failover,detect%20failure%20from%20the%20origin.
* https://aws.amazon.com/fr/blogs/networking-and-content-delivery/three-advanced-design-patterns-for-high-available-applications-using-amazon-cloudfront/
* https://aws.amazon.com/blogs/networking-and-content-delivery/improve-web-application-availability-with-cloudfront-and-route53-hybrid-origin-failover/
