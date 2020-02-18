---
layout: post
title: AWS Ligthsail Wordpress setup
image: 
categories: [Blog]
tags: [aws]
---
Instructions to setup [AWS Lightsail](https://aws.amazon.com/lightsail/) with [Wordpress](https://wordpress.org/)

# Setting up the wordpress instance using AWS Lightsail and Bitnami
Follow the simple instructions here:  
<https://www.gritsolutions.ph/how-to-create-wordpress-website-amazon-lightsail/>

[Remove the Bitnami banner](https://docs.bitnami.com/general/how-to/bitnami-remove-banner/):
1. SSH into the AWS Lightsail instance
2. Run `sudo /opt/bitnami/apps/wordpress/bnconfig --disable_banner 1`
3. Run `sudo  /opt/bitnami/ctlscript.sh restart apache` to reload the webserver

# Setting up the domain name
To use a custom domain name, go to `Networking` in the AWS Lightsail page.
1. Click on `Create static IP` and complete the instructions for your Wordpress site
2. Click on `Create DNS Zone`, which will hold the Domain Name System (DNS) records.
3. Create an `A record` with subdomain `@.example.com` that maps to your `Static IP`
4. Create an `CNAME record` with subdomain `www.example.com` that maps to `example.com`
5. Optionally create an `MX` and `TXT` record if needed for forwarding email to an email server, 
see your domain provider for info. For example, see the details for the domain registrar 
[Porkbun](https://kb.porkbun.com/article/47-how-to-use-porkbun-email-when-your-dns-is-hosted-elsewhere).

The last step means that www.example.com gets mapped to the apex/top domain example.com, 
so that both work.

At the bottom of the `DNS Zone` page you'll find a list of `Name Servers`. 
These need to be copied to the `Domain Name Servers` section of your domain registrar. 
This will mean that DNS servers will ask AWS Lightsail DNS servers which IP the Wordpress website is hosted on.

Note: it can take about 48 hours for the DNS changes at a domain registrar to propagate 
to the DNS servers around the world.

To follow the progress of the propagation of the DNS changes: <https://www.whatsmydns.net/>

# Use AWS Simple Email Service (SES) to send WordPress Emails
It's not easy to setup a mail server on AWS Lightsail. A simpler method to send emails from wordpress, 
for forms and such, is to setup a SMTP using [AWS SES](https://aws.amazon.com/ses/).

Note:
* This will only allow sending to/from verified emails, which is fine for feedback forms.
* The free version is limited to 200 emails/day

## Setup Wordpress to use the AWS SES SMTP
1. Install the `WP Mail SMTP` plugin, see Plugins -> Add New on the Wordpress admin page
2. Set the `From Mail` to the AWS SES verified email
3. Set `Mailer` to `Other SMTP`
4. Set `SMTP Host` to the SES provided SMTP: `email-smtp.PROVIDED_REGION.amazonaws.com`, 
where `PROVIDED_REGION` needs to be changed
5. Set `Encryption` to `TLS`
6. Set `SMTP Port` to `587`
7. Set `SMTP Username` to the AWS SES provided Username
8. Go to the AWS Lightsail website -> Instances -> Your Wordpress instance -> Connect using SSH. Modify `apps/wordpress/htdocs/wp-config.php` by adding
```php
define( 'WPMS_ON', true );  
define( 'WPMS_SMTP_PASS', 'YOUR_PASSWORD' );  
```
where `YOUR_PASSWORD` needs to be replaced by the AWS SES provided password.
9. Click on `Save Setttings`

You can test sending an email by clicking on the `Email Test` tab and 
sending an email to one of the verified emails.
In case it doesn't work, you might need to assign one of the verified emails to 
one of the Wordpress Users, see Users on the Wordpress admin page.

# References:
* <https://techinscribed.com/wordpress-on-aws-lightsail-a-complete-guide/>
* [How to setup/use AWS SES](https://docs.bitnami.com/aws/how-to/use-ses/)