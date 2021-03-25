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

## AWS Design

* Cloudfront config, with correct TLS cert tied to a;
* S3 Origin for static assets
* Simple API Gateway secured to a single Lambda
* Lambda is secured to DynamoDB, Cloudwatch logs, SES only

## Code logic

* Form checks input
* If valid sends an email, saves order to db
* and sends HTML response to user

## Cloudformation

These have been split into two, `cloudformation-static.yaml` and `cloudformation-app.yaml`, to make them more managable.  They could be collapsed as both have some shared Parameters for example `RootDomainName` (not used yet, but expected to be added for API GW on a Custom Domain name).

## AWS Simple Email Service verififcaiton

You need to verify both the sending domain and the reciepient (if different) to do this.

Assuming an R43 hosted domain for a domain and any inbox you have access to (for individual email)

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

### Python
* https://stackoverflow.com/questions/42445237/looping-through-a-json-array-in-python
* UUID lib: https://docs.python.org/3/library/uuid.html
* https://nickolaskraus.org/articles/transforming-code-into-beautiful-idiomatic-python/
