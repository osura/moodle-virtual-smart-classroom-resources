# moodle-virtual-smart-classroom-resources
* Project Repository : https://github.com/osura/moodle-virtual-smart-classroom-resources
# Setup
## Creating a BigBlueButton Server
The full step guide for making a BigBlueButton(BBB) server can be found in their official [Documentation](https://docs.bigbluebutton.org/2.2/install.html). But most of these steps can be automated using their official BBB install script ([bbb-install](https://github.com/bigbluebutton/bbb-install)). With only a few parameters, BBB-install.sh can have BBB server set up and ready for use in about 30 minutes (depending on your server's internet speed to download and install packages).

For the project, Amazon EC2(Ubuntu 16.04 64-bit server) was used to install the BBB Server. The server needs at least 4gb memory to install and also recommends 4 cores processor as well. Initially, as there was only an IP address for the EC2, the script was installed using the below command.

```shell script
wget -qO- https://ubuntu.bigbluebutton.org/bbb-install.sh | bash -s -- -w -v xenial-22 -a -w
```

Also below inbound rules for ports in AWS security group was added.

TCP/IP port 22 (for SSH)
TCP/IP ports 80/443 (for HTTP/HTTPS)
UDP ports in the range 16384 - 32768 (for FreeSWITCH/HTML5 client RTP streams)

Then assigned the ec2 instance an Elastic IP address to prevent it from getting a new IP address on reboot.

The script can be run using the below command if the server has a domain this command installs free automated certificates using certbot which enables to make https connection ( This method is recommended as most of the browsers nowadays prompt warnings when using microphone and camera with HTTPS connection.).
``` shell script
wget -qO- https://ubuntu.bigbluebutton.org/bbb-install.sh | bash -s -- -v xenial-22 -s bbb.example.com -e info@example.com -w
```

If the installation is successful it shows the demo web app URL. By visiting that url the server can be tested. To integrate the private BBB server to moodle a secret key and the url is needed. By using the “BBB-conf --secret” command, that information can be obtained from the BBB server.

```shell script
# bbb-conf --secret

       URL: http://xxx.xxx.xxx.xxx/bigbluebutton/
    Secret: yyy
```

This information can be added to the moodle by going through  Site administration => Plugins=>Plugins overview=>BigBlueButton Settings.

This will override their default testing BBB server and replace it with the private one. And all the BBB services will be served by this private server to the moodle.
