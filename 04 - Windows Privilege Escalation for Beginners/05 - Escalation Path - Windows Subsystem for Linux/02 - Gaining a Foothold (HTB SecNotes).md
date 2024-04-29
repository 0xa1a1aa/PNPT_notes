1) Navigate to webpage on port 80
2) Sign up
3) Login

The site *contact.php* displays an email: tyler@secnotes.htb
=> we now got a username + email

TLDR: SQL injection in signup site

Login as **'OR 1 OR'**
There is a note with tylers password

Connect to SMB with the creds:
```bash
smbclient \\<target-ip>\new-site -U tyler
```

Uploaded aspx files get deleted quickly => Upload a php file
Access the uploaded file at the website under port 8808