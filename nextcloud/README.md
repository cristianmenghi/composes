On the details panel enter your domain name.

Scheme = http, Forward Hostname = whatever your machines local IP address is, (for example 192.168.1.35) and forward port = 80.

Cache Assets, Block Common Exploits, Websockets support all should be on.

Next on the Custom Locations tab we will enable caldav and carddav to allow remote access to your calendars and contacts.

Location 1:

    location = /.well-known/caldav
    scheme = html
    Forward Hostname = <local IP>/ remote.php/dav
    Forward Port 80

Location 2:

    location = /.well-known/carddav
    scheme = html
    Forward Hostname = <local IP>/ remote.php/dav
    Forward Port 80

Additional Configuration

Now if you are setting up Nextcloud to work with your custom domain you will need to open the config.php file and change trusted domains to whatever your domain is.

If you are trying to access your Nextcloud from your network it could be useful to add your Nextcloud’s local IP address.

'trusted_domains' => 
 array (
 0 => 'example.com',
 1 => '192.168.1.12:8081',
 ),
 'overwritehost' => 'example.com',
 'overwriteprotocol' => 'https',

Since Niginx Proxy Manager is set up, the following needs to be added to the config.php file:

'default_phone_region' => 'US',
 'trustedproxies' => 
 array (
 0 => 'NginxProxyManager',
 1 => '192.168.0.145',
 ),

To solve some of the warnings you need to do the following:

 ‘default_phone_region’ => ‘US’,

To set up mail alerts you will need to add the following to the config file. The values will need to be obtained from your email provider.

 'mail_from_address' => 'user', # insert your emails user
 'mail_smtpmode' => 'smtp',
 'mail_sendmailmode' => 'smtp',
 'mail_domain' => 'example.com', # Your email domain
 'mail_smtphost' => 'smtp.example.com', 
 'mail_smtpport' => '465',
 'mail_smtpauth' => 1,
 'mail_smtpsecure' => 'ssl',
 'mail_smtpname' => 'user@example.com',
 'mail_smtppassword' => 'secretpassword',

Run the container

docker-compose up -d

Congratulations! Nextcloud is setup using docker containers and docker compose! Let me know if you have any questions.
Potential Issues

    If you run into a 502 Gateway Error try clearing the cookies in your browser for the domain your server is hosted. It works most of the time for me.
    Make sure to update the docker images on a regular basis. Nextcloud in a Docker cannot whole number skip versions. For example if your version is 24 and the newest version is 26, DO NOT update straight to 26. I learned this the hard way. Update first to 25. So run “docker-compose pull” somewhat regularly.

https://chrisgrime.medium.com/deploy-nextcloud-with-docker-compose-935a76a5eb78