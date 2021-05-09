# Mail Server Configuration

This is a example to how integrate a mailing service with a development environment of Consul.

In this example we used [Mailgun](https://www.mailgun.com/).

## Create an account in Mailgun

![Creating an account in Mailgun](../../../.gitbook/assets/mailgun-create-account%20%281%29.png)

* Skip the credit card form
* And activate your account with the link sent by email

## Domain configuration

* Go to the Domains section: ![Mailgun domain section](../../../.gitbook/assets/mailgun-domains%20%281%29.png)
* Since you don't have a domain yet, you should click in the sandbox that is already created;
* Remember the next credentials:

  ![Mailgun sandbox](../../../.gitbook/assets/mailgun-sandbox%20%281%29.png)

## Consul mailing configuration

* Go to `config/secrets.yml` file;
* Add the lines on the file to configure the mail server under the section `staging`, `preproduction` or `production`, according to your deploy:

```yml
  mailer_delivery_method: :smtp
  smtp_settings:
     :address: "<smpt address>"
     :port: 587
     :domain: "<domain>"
     :user_name: "<user_name>"
     :password: "xxxx"
     :authentication: "plain"
     :enable_starttls_auto: true
```

* Fill, `address`, `domain`, `user_name`, `password` with your information. 

You can also use Mailgun to production, adding your custom domain. Mailgun’s logs sent and delivered emails.

