# pastyAndChablis

AWS Static website for a bit of fun ordering takeaway pizza!

1. A static website with menu
1. User submits a form of their Pizza order
1. Simple recording in S3 access logs

## Aim

In no real order

* Set up a website without servers
* Have a proper cert/domain name
* CDN and origin config, secured to only answer CDN
* Configured as Code
* Learn all the deep details of AWS Cloudformation/config

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
* `:set autointend`

## Resources

Static Websites with CF, CF and S3
* https://nickolaskraus.org/articles/creating-a-static-website-using-cloudformation/
* https://hackernoon.com/how-to-configure-cloudfront-using-cloudformation-template-2c263u56
* Region mappings: https://liamraven.com/blog/aws-cloudformation-for-a-static-website

API GW, Lamdba, SES
* https://acloudguru.com/blog/engineering/how-to-build-a-serverless-contact-form-on-aws
* https://github.com/guardian/ses-send-email-lambda/blob/master/conf/cloudformation.yml
* Best one: https://nickolaskraus.org/articles/creating-an-amazon-api-gateway-with-a-lambda-integration-using-cloudformation/
