---
title: Installing M-Pin Core
taxonomy:
    category: docs
---
This content is relevant only to the **direct download distribution** of M-Pin Core. For information on the AWS distribution, see AWS Getting Started Guide.

The following links may provide useful information for you to help you understand the installation requirements as well as the process.

The initial installation on a Linux machine is as simple as running a command in the command prompt:

>>>>The actual link for the installation script could be subject to change   

`python -c "$(curl -fsSL https://mpin.miracl.com/v4/install)"`

>>>If you wish to install v3.5 of Core, use this command    

`python -c "$(curl -fsSL https://mpin.certivox.net/v3/install)"`   

The following is a sample output:

```
MIRACL M-Pin v4.0 server installation.

Enter the installation path [/opt/mpin]
No previous installation found, proceed with installation? [yes/no] y
Proceeding with M-Pin Core installation...
Downloading components [1/6] (mpin4.0_mobile.tar.gz)
Downloading components [2/6] (mpin4.0_mpincommand.tar.gz)
Downloading components [3/6] (mpin4.0_servers.tar.gz)
Downloading components [4/6] (mpin4.0_pythonlibs.tar.gz)
Downloading components [5/6] (mpin4.0_mpinscripts.tar.gz)
Downloading components [6/6] (mpin4.0_python2.7_x86_64bit_linux.tar.gz)
MIRACL reserve the right to deactivate this installation after six months. If you would like to be contacted for alternatives before this happens, please ensure that you enter youremail address or Twitter name when requested to below.

You can enter an email address or a Twitter account to be contacted on by our support team for full info on out to get the most value from M-Pin Core.To skip this step just hit enter, you can still contact us on support@miracl.com or on Twitter on @miraclhq
Your email or Twitter account: john@certivox.com

Getting Community D-TA keys...
Do you want to enable native apps: [yes/no] y
Please enter the public address on which the RPA is served:http://192.168.15.20:8005
Please enter a name for used service:Core Demo

M-Pin Server has been installed into the following folder /opt/mpin.
For a list of helpful commands, navigate to the install directory and type: ./mpin --help.
To quickly test out the built-in demo, run: ./mpin start all.
Then open a browser on the same machine and visit: http://127.0.0.1:8005.
Select Sign in from here and follow the on-screen instructions.
For more details on testing the demo site, please visit: http://docs.miracl.com/docs/m-pin-core/quick-install-guide
For additional next steps, please visit: http://docs.miracl.com/getting-started
The M-Pin Core documentation home page can be found here: http://docs.miracl.com/m-pin-core

IMPORTANT: This installation uses a shared Distributed Trust Authority and therefore should only be used for development and testing. For more information, please see: http://docs.miracl.com/docs/m-pin-core/m-pin-understanding-dta
```

>>>>>To find out more about enabling native apps, click here.

Overview of the Default Installation:
+ Each M-Pin Platform utilizes two Distributed Trust Authority (D-TA) Services.   
+ Dual D-TA Services insure no single point of compromise.   
+ Each D-TA Service has its own root key and operates independently.   
+ Each D-TA Service issues a 'share' of an M-Pin Key to clients and servers.   
+ Clients and servers assemble whole M-Pin Keys from these shares.   
+ Only M-Pin Clients and Servers ever possess a whole, complete M-Pin Key.   
+ The first D-TA Service exists as part of this M-Pin Server installation.   
+ This is your D-TA Service, exclusively under your control.   
+ The second D-TA Service is the Community D-TA operated by MIRACL.   
+ The Community D-TA Service at MIRACL is a free of charge, fair-use service.   
+ Use it for testing and development, not for production.   
+ For production deployments, purchase an M-Pin Service Plan at MIRACL.com.   
+ An M-Pin Service Plan includes:   
+ Dedicated D-TA Service instance at MIRACL, replacing the Community D-TA   
+ Individual root key secured with FIPS 140-2 Level 3 HSMs   
+ Support, 100% uptime service level, monitoring, and unlimited use   

>>>>>For security reasons, it is not possible to migrate M-Pin Keys with shares issued from a Community D-TA to a dedicated D-TA Service.
See MIRACL.com for more information.
