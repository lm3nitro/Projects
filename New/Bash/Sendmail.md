# Sendmail

![Pasted image 20241017154807](https://github.com/user-attachments/assets/d778703c-e783-4692-b240-0aadd2ae876e)

Sendmail is a lightweight, command line SMTP email client. If you have the need to send emails from a command line, this free program is perfect: simple to use and feature rich. It can also be used in bash scripts, batch files, Perl programs, and so much more. 

## Introduction

## Installing SendMail

To get started, I will be installing Sendmail:

```
 sudo apt install sendmail mailutils sendmail-bin
```

![Pasted image 20241017155004](https://github.com/user-attachments/assets/3562f6cf-8d7b-4d17-ae63-d4d9c880742e)

Once that is installed, I created a Gmail authentication file:

> [!NOTE]  
> Elevate to the root user, as most of these commands will require root access – even when changing directories where needed.
 
```
sudo su
```

After, I made a new directory where I will store the Gmail configuration file. I then navigated to it:

```
mkdir -m 700 /etc/mail/authinfo/
cd /etc/mail/authinfo/
```

This is what it looks like at the end:

![Pasted image 20241017142221](https://github.com/user-attachments/assets/c37ee863-13b7-4415-884d-a86d67f952d3)

I then created a new file that will contain the authentication info. To keep it simple, I called mine `gmail-auth`:

```
nano gmail-auth
```

![Pasted image 20241017142512](https://github.com/user-attachments/assets/723b3910-cb71-4f99-8177-72db80877375)

> [!NOTE]  
> The gmail-auth.db file show on the screenshot above is a file that is created only after the configuration parameter has been passed to the file /etc/mail/sendmail.mc under MAILER_DEFINITIONS and then executing the make -C /etc/mail witch will rebuild the configuration using the parameter specify. 

Inside this file, I pasted the following template and then edited with my own information. Specifically, entering my Gmail address and app password. 

> [!IMPORTANT]  
> I already had an app password configured in my gmail account. If you plan to do this for yourself, this will be needed. I provided information on this process under **SSMTP.md**

This is the syntax I used:

```
AuthInfo: "U:root" "I:YOUR GMAIL EMAIL ADDRESS" "P:YOUR PASSWORD"
```

Next, I needed to create a hash map for the above authentication file:

```
makemap hash gmail-auth < gmail-auth
```

Now that the Gmail authentication is setup, I needed to configure Sendmail.

## Configure SendMail: 

To configure Sendmail, I edited the file in `/etc/mail/sendmail.mc`:

```
nano /etc/mail/sendmail.mc
```

I pasted the following lines under "MAILER_DEFINITIONS":

![Pasted image 20241017143546](https://github.com/user-attachments/assets/6f07dd8e-914a-437b-b97c-a44adaf65dfd)

```
define(`SMART_HOST',`[smtp.gmail.com]')dnl
define(`RELAY_MAILER_ARGS', `TCP $h 587')dnl
define(`ESMTP_MAILER_ARGS', `TCP $h 587')dnl
define(`confAUTH_OPTIONS', `A p')dnl
TRUST_AUTH_MECH(`EXTERNAL DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
define(`confAUTH_MECHANISMS', `EXTERNAL GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
FEATURE(`authinfo',`hash -o /etc/mail/authinfo/gmail-auth.db')dnl
```

Once I added the lines, I saved the files. After saving the file, I needed to re-build Sendmail’s configuration:

```
make -C /etc/mail
```

I reloaded the Sendmail service so that all the changes can take effect. Either option below works:

```
systemctl restart sendmail
```

OR

```
/etc/init.d/sendmail reload
```

> [!NOTE]  
> The service will try to resolve your fully qualified domain name. If it’s not configured, the process may hang for a minute, but it will eventually start. Check the status of the Sendmail service to get a report on any errors it encounters.

## Configuration Test:

To test the configuration, below is the syntax and command that I used:

```
$ echo "Just testing my sendmail gmail relay" | mail -s "Sendmail gmail Relay" my-email@my-domain.com
```

![Pasted image 20241017144757](https://github.com/user-attachments/assets/6b29f79b-6f2c-4842-9ead-07148d43df66)

```
cat /var/log/mail.log | tail
```

![Pasted image 20241017144748](https://github.com/user-attachments/assets/1426a2b2-df86-43c3-98a6-e04bd510b0c7)

## Verify Gmail Inbox:

I can see that I have received email:

![Pasted image 20241017145023](https://github.com/user-attachments/assets/4ed244fc-89bd-4878-abdb-d1e10080ddc0)

>### Troubleshooting:
>The logs for Sendmail are stored in /var/log/
>![Pasted image 20241017145744](https://github.com/user-attachments/assets/9eafae60-e27d-4f21-90f5-64285de5aa24)
>```
>cat /var/log/mail.log
>```
>
>Or real time logs:
>
>```
>tail -f /var/log/mail.log
>```
>
>![Pasted image 20241017144927](https://github.com/user-attachments/assets/d81665de-71bf-402e-b798-fffd84142a23)
>
>This error output actually makes the problem quite clear. The `unqualified host name` text means exactly what it says. It means that Sendmail is **not able to resolve your fully qualified domain name**.
>
>Checking `hostname` command:
>
>![Pasted image 20241017145350](https://github.com/user-attachments/assets/ff514c52-ef7c-47c4-a59e-718dc83872dd)
>
>To begin troubleshooting, check the contents of your `/etc/hosts` file. In our case the host name is “debian” is not a FQDN. To resolve this problem change `/etc/hosts`:
>
>FROM:
>```
>127.0.0.1       localhost
>127.0.1.1       lm3nitro-vm
>```
>
>TO:
>Adding the following lines to the /etc/hosts:
>
>```
>127.0.0.1       localhost.mydomainname localhost lm3nitro-vm
>127.0.0.1       lm3nitro-vm.mydomainname
>```
>
>NOTE: Substitute `lm3nitro-vm` with the actual name of your FQDN.
>
>Example:
>
>![Pasted image 20241017141633](https://github.com/user-attachments/assets/c93d9cc0-b5d9-4d1a-b01a-eb4ae240fbf7)
>
>Restart the Sendmail service and try sending your email again:
>
>```
>sudo systemctl restart sendmail
>```

## Summary:

I included the troubleshooting section because when I originally configured Sendmail, I was getting an error and wanted to share what I was able to do to resolve the error and issue I encountered. Overall, Sendmail has a wide variety of features that make it user friendly. In installing and configuring Sendmail, I was able to send an automated email directly from the command line. Sending emails from the CLI enhances efficiency, and allows you to manage email tasks effectively within a workflow.
