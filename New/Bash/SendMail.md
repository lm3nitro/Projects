
![Pasted image 20241017154807](https://github.com/user-attachments/assets/d778703c-e783-4692-b240-0aadd2ae876e)




SendEmail is a lightweight, command line SMTP email client. If you have the need to send email from a command line, this free program is perfect: simple to use and feature rich. It was designed to be used in bash scripts, batch files, Perl programs and web sites, but is quite adaptable and will likely meet your requirements.

## Installing  Sendmail:



```
 sudo apt install sendmail mailutils sendmail-bin
```
![Pasted image 20241017155004](https://github.com/user-attachments/assets/3562f6cf-8d7b-4d17-ae63-d4d9c880742e)

# Create Gmail authentication file:

### NOTE:

 Elevate to the root user, as most of these commands will require root access – even when changing directories where needed.
 
```
sudo su
```

### Making a new directory where we will store the Gmail configuration file, then change into it:



```
mkdir -m 700 /etc/mail/authinfo/
cd /etc/mail/authinfo/
```

It should look like this at the end:

![Pasted image 20241017142221](https://github.com/user-attachments/assets/c37ee863-13b7-4415-884d-a86d67f952d3)



### Creating a new file with nano or your preferred text editor that will contain our authentication info. To keep it simple, we’ll call ours `gmail-auth`:


```
nano gmail-auth
```


![Pasted image 20241017142512](https://github.com/user-attachments/assets/723b3910-cb71-4f99-8177-72db80877375)


Note: The gmail-auth.db file show on the screenshot above is a file that is created only after the configuration parameter has been passed to the file /etc/mail/sendmail.mc under MAILER_DEFINITIONS and then executing the make -C /etc/mail witch will rebuild the configuration using the parameter specify. 

#### Inside this file, paste the following template and then edit it with your own information. Specifically, enter your Gmail address and password:



Syntax:

```
AuthInfo: "U:root" "I:YOUR GMAIL EMAIL ADDRESS" "P:YOUR PASSWORD"
```


#### Creating a hash map for the above authentication file:


```
makemap hash gmail-auth < gmail-auth
```

 The Gmail authentication is setup, moving on to configuring Sendmail.

# Configure Sendmail: 

### Next, edit the file in `/etc/mail/sendmail.mc`:

```
 nano /etc/mail/sendmail.mc
```


### Pasting the following lines  down "MAILER_DEFINITIONS" line:


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


Save the changes to the file.

### Re-build sendmail’s configuration

```
make -C /etc/mail
```

### Reload the Sendmail service for all of our changes to take effect:

```
systemctl restart sendmail
```

OR

```
/etc/init.d/sendmail reload
```


Note that the service will try to resolve your fully qualified domain name. If it’s not configured, the process may hang for a minute, but it will eventually start. Check the status of the Sendmail service to get a report on any errors it encounters.


#### ## Configuration test:

Syntax:

```
$ echo "Just testing my sendmail gmail relay" | mail -s "Sendmail gmail Relay" my-email@my-domain.com
```


![Pasted image 20241017144757](https://github.com/user-attachments/assets/6b29f79b-6f2c-4842-9ead-07148d43df66)


```
cat /var/log/mail.log | tail
```

![Pasted image 20241017144748](https://github.com/user-attachments/assets/1426a2b2-df86-43c3-98a6-e04bd510b0c7)


### Verify Gmail inbox:
![Pasted image 20241017145023](https://github.com/user-attachments/assets/4ed244fc-89bd-4878-abdb-d1e10080ddc0)



# Troubleshooting:


The logs are stored in /var/log/

![Pasted image 20241017145744](https://github.com/user-attachments/assets/9eafae60-e27d-4f21-90f5-64285de5aa24)


```
cat /var/log/mail.log 

```

Or real time logs:

```
tail -f /var/log/mail.log 
```

![Pasted image 20241017144927](https://github.com/user-attachments/assets/d81665de-71bf-402e-b798-fffd84142a23)


This error output actually makes the problem quite clear. The `unqualified host name` text means exactly what it says. It means that Sendmail is **not able to resolve your fully qualified domain name**.


 Checking `hostname` command:
 
![Pasted image 20241017145350](https://github.com/user-attachments/assets/ff514c52-ef7c-47c4-a59e-718dc83872dd)



To begin troubleshooting, check the contents of your `/etc/hosts` file. In our case the host name is “debian” is not a FQDN. To resolve this problem change `/etc/hosts`:

# FROM:


```
127.0.0.1       localhost
127.0.1.1       lm3nitro-vm
```

# TO:
Adding the following lines to the /etc/hosts:

```
127.0.0.1       localhost.mydomainname localhost lm3nitro-vm
127.0.0.1       lm3nitro-vm.mydomainname

```

NOTE: Substitute `lm3nitro-vm` with the actual name of your FQDN.

Example:

![Pasted image 20241017141633](https://github.com/user-attachments/assets/c93d9cc0-b5d9-4d1a-b01a-eb4ae240fbf7)


### Restarting the sendmail service and try sending your email again:

```
sudo systemctl restart sendmail
```
