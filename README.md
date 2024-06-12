# `this space is so cold 🥶`


Linklayer VPN settings were explained below.

### Remote config directory

This directory contains an example of how to serve configurations remotely, pay attention that it is an example you can create your own backend of how to serve these files, also read the readme.txt within the subfolders


### General Settings 
The linklayerVPN configuration file is in json format.

**Auth** 

Authentication can currently be done in 3 ways:

> system

System mode is used to authenticate the user using the system username and password. You can authenticate without defining an expiration time or you can configure the user to expire.

> binary 

This mode requires that you put the path of the executable in the **executable** argument, when trying to authenticate linklayer it will pass the user as argument 1 and argument 2 as password, if the exit code is 0 the authentication was a success in otherwise failure

> static

To authenticate with static, you need the argument **file** with the path of the file where the authentication data will be, it will clarify that this data will be used in both username and password, example in this repo add auth.txt inside **demo** so, when you can put this credentials you use demo as username and password.


**limit_conn_single**


Adjusting this argument allows adjusting how many connections per user are valid in single tunnel mode, if it is **-1** it means that the user has no limit.
The operation happens when, for example, if you configure that a user can only connect once, if another user connects with the same credentials it will kill the previous connection, since it only allows 1 connection per user.

**limit_conn_request**

This argument limits the connections that can be made by request mode, be careful here it is different from single, an approximation of how many connections a user generates must be adjusted since request mode works very differently than single mode, here it is necessary to modify the value until it adjusts to contexts real users.
If left at **-1** it means there is no limit

This concludes the general configurations, for now if they are added in future updates this readme will be updated.

### Services,Layers(Protocols)

The services or layers are the intermediate protocols that help produce the connection. Below we describe how to configure each of these protocols.

**Layer `HTTP`**

The most common, this type of layer allows you to listen at the HTTP layer and process everything that is an HTTP request in order to continue the connection.

**Config HTTP**

> Listen 

The address where the example server will respond to requests: **0.0.0.0:80**

> Response 

 The custom HTTP response you want sent example: **HTTP/1.1 200 OK\r\n\r\n**
 
**Layer `TLS`**

This layer allows you to handle TLS/SSL connections sent by the client

**Config TLS**

> Cert

The path of Cert PEM format

> Key

The path of key PEM format

> Listen

The address where the example server will listen : **0.0.0.0:443**

#### Clarify an important option, the HTTP and TLS layer can work on the same port, for example, you can set HTTP to listen on port 80 and TLS on port 80 as well, since linklayer will be able to manage where to send the packets, whether to TLS or HTTP. so you can listen to both layers using the same address/port. only valid en Layer HTTP and Layer SSL/TLS

**Layer `HTTP/TLS` aka HTTP/TLS Mix**

HTTP TLS allows you to create a connection where an SSL/TLS connection is first established and once an HTTP packet is sent within said connection, the parameters, as you will understand, are the mix between the HTTP layer and also TLS, **important here is the port is unique, this means that it cannot be the same as the other layers.**

**Config HTTP/TLS**

> Response

 The custom HTTP response you want sent example: **HTTP/1.1 206 OK\r\n\r\n**
 
> Cert

The path of Cert PEM format

> Key

The path of key PEM format

> Listen

The address where the example server will listen : **0.0.0.0:443**

**Layer `DNSTT`**

This protocol is already known, a DNS tunnel from DNSTT is only implemented and adjusted to be better compatible with linklayer, The configurations below are valid within the link layer, however, additional configurations of the protocol itself are also required, which I will explain later.


**Config DNSTT**
 
> Domain

The domain that DNS will use is required. For more information, visit their page.

> Net 

This is mandatory, the network interface you use on your server is required, if you use more than 1, check which one has access to the Internet.


**OPTIONAL**
Optionally, if you wish, you can generate your own public and private keys, but to do so you need the main DNSTT binary. The command line should be as shown. The important thing is the names of the public and private keys.
To generate the public and private key, run this command.

**./dnstt-server -gen-key -privkey-file dnstt-priv.key -pubkey-file dnstt-public.pub**

Once you have generated these keys, you need to move them to the layers/cfgs/ folder and paste the files **dnstt-priv.key** and **dnstt-public.pub** there.

[Click here for more info DNSTT](https://www.bamsoftware.com/software/dnstt/)

Thanks to [@Rufu99](https://t.me/Rufu99) for the suggestion to start DNSTT and auto generate the public and private keys.



**Layer `UDP` aka UDP HyRequest**

UDP protocol called HyRequest

**Config UDP**

> listen
addr to listen UDP Server mandatory default 36718

> exclude

ports can be excluded from iptables, mandatory 53,5300

> net

your network interface to internet 

> cert 

path of certificate format PEM

> key 

path o key in format PEM

> obfs

the obfuscated password

These are the essential configurations for UDP to work.


**Layer `HTTPDUAL`**

The DUAL HTTP layer allows establishing a connection through separate HTTP connections, thus allowing better connection compatibility and stability.

**Config UDP**

> Listen

Only the address/port where the connection will be handled is required.


### Conclusion

It may seem somewhat difficult to understand but we have added a demo.json file to understand a little how to configure it. Please keep the server binary files and the layers folder together because if the layers folder does not appear and requires an external layer This could fail.

These are the protocols currently in Linklayer, one already known and others internal to Linklayer. You may wonder why there are protocols that already exist, so simply don't reinvent the wheel, improve it.

We hope that this mini guide can help you configure your server, if you have questions you can ask for help in the support group, we are working to add new layers and make it as easy as possible to use any feedback errors that happen to you, do not hesitate to ask and Thank you for using our application.


### You can run cfg demo ./server -cfg cfg/config.json
