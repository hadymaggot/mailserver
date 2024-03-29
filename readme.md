# [Docker Mailserver](https://github.com/docker-mailserver/docker-mailserver)

### Clone this repo
```bash
git clone [repo_name] mailserver
```

### Enter the directory
```bash
cd mailserver
```

### Create data_dir for mailserver
```bash
mkdir docker-data
```

### Change data_dir ownership
```bash
sudo chown "$USER":"$USER" docker-data/ -R
```

---
### __Don't forget__ to make a change:

[compose.yaml](compose.yaml)
- Hostname
- Domain Name
- SSL cert path (I'm using cert generated by web server, then mounting in)

[mailserver.env](mailserver.env)
- make changes according to your needs

---
### Create user
```bash
./setup.sh email add postmaster@domain.tld your-password
```
#### or this
```bash
docker run --rm -e MAIL_USER=postmaster@domain.tld -e MAIL_PASS=your-password -it mailserver/docker-mailserver /bin/sh -c 'echo "$MAIL_USER|$(doveadm pw -s SHA512-CRYPT -u $MAIL_USER -p $MAIL_PASS)"' >> ./docker-data/dms/config/postfix-accounts.cf
```

### Generate a DKIM key
```bash
./setup.sh config dkim
```

### DKIM record path
```bash
/your/path/mailserver/docker-data/dms/config/opendkim/keys/domain.tld/mail.txt
```

### Fill out mail._domainkey in DNS
```bash
v=DKIM1; h=sha256; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsg3kc3TfVpnmoBjJzB+kp1MgD6fVkj8k6ITDAnM1FtJF4j+ibbjhQoF4aQ6pCjOK+L2lWohFoSe/i58U6ecvmlm5f4syFnHq/yQuvcAppAGB4uGUnN+JVY/3TXEZcmUKSj85rGErH+igFDL84nhByS4hioJEwL0CIt3FU6wQZ6Oo0dbfHiimlLEf4peFGZiFxyolHUHaPioWYj
```

### Bring up the Container
```bash
docker-compose up
```

---
### Email Client Setup Guides

**INCOMING** mail server: <br>
(IMAP/Pop3)
| setting     | your_input  |
| ----------- | ----------- |
| Host Name | `mail.domain.tld` |
| User Name | `postmaster@domain.tld` |
| Password | `your-password` |


**OUTGOING** mail server: <br>
(SMTP, Outlook/Thunderbird/Mail/etc for example)
| setting     | your_input  |
| ----------- | ----------- |
| Host Name | `mail.domain.tld` |
| User Name | `postmaster@domain.tld` |
| Password | `your-password` |
| Use SSL | `on` |
| Authentication | `Password` |
| Server Port | `465` |

---
__Good Luck__ *_v
