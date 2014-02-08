This is an example Tornado app I set up for my post about running [Tornado](http://www.tornadoweb.org/) on [Linode](http://www.linode.com/), which you can read [here](http://chase.io/post/17197274701).



A while back we switched environments and frameworks at Fetchnotes, and I thought it might help to provide a simple tutorial to deploying an app with Tornado, an open-source web server from Facebook, on Linode, the best hosting on the planet. Here it is and I hope it helps.

Follow the instructions here to get started on Linode.

*Ubuntu 10.04+ is recommended. If you’re not running as root, you’ll have to prepend ‘sudo’ to just about every command below.

Open the terminal.

Log into your Linode:

ssh root@yourpublicip 

*Your Public IP can be found in the “Remote Access” tab of the Linode Manager

Installing tools and dependencies:

apt-get install python-setuptools 

easy_install pip 

pip install tornado 

apt-get install git 

apt-get install nginx 

pip install supervisor 

Make directories for your app:

mkdir /srv/www 

mkdir /srv/www/domain.com 

Pull in source code:

cd /srv/www/domain.com 

git clone git@github.com:chaselee/tornado-linode.git 

Create symbolic links to conf files:

cd /etc/nginx 

rm nginx.conf 

ln -s /srv/www/domain.com/tornado-linode/conf/nginx.conf nginx.conf 

cd 

ln -s /srv/www/domain.com/tornado-linode/conf/supervisord.conf supervisord.conf 

Create nginx user:

adduser --system --no-create-home --disabled-login --disabled-password --group nginx 

Create a logs directory:

mkdir ~/logs 

Start Supervisor and Nginx:

supervisord 

/etc/init.d/nginx start 

Visit your public IP address

Done. You now have a Tornado app running on Linode, load-balanced with Nginx and monitored with Supervisor.

When you want to update the hello linode app:

cd /srv/www/domain.com/tornado-linode 

git pull 

Tip: 
If you ever see this error when running supervisord: 
Error: Another program is already listening on a port that one of our HTTP servers is configured to use. Shut this program down first before starting supervisord.
then:
cd 
unlink supervisord.sock

Let me know if you have questions and enjoy!
