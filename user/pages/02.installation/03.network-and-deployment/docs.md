---
title: Network and Deployment
taxonomy:
    category: docs
---
####Network Setup
There are a few network setup considerations that should take place. By default, all the Customer-hosted services are installed on the same machine for several reasons:   

+ Simplicity   
+ Ease of deployment   
+ Communication between the different services that could be done on the loopback network interface without going on the wire.   

The RPS and the RPA are basically two parts of the same - the Relying Party, as the RPS encapsulates and abstracts some more specific functionality related to the M-Pin authentication scheme and protocol. Therefore, the RPS and the RPA need to exchange some private messages, as both expose some API that should not be publicly accessible.   

The RPS API consists of two types of requests:   

1.	**Public API:** the URL's for the public API always start with the `/rps prefix` (customizable).   

    Examples:   
    `GET /rps/clientSettings`   
    `PUT /rps/user`   

2.	**Private API:** the API that should be exposed only to another M-Pin Services as the RPA or the M-Pin Server. The URL's for those requests do not start with the /rps prefix.   

    Examples:   
    `POST /user/<mpin-id>`   
    `PUT /token`     

Since the RPS should not expose its Private API, it should be placed behind a proxy.   

####Proxy Considerations

#####Proxy

>>>>>Do not confuse this proxy with the D-TA Proxy. The D-TA Proxy is part of the MIRACL-hosted services and does not exist on the Customer side.

#####RPA serving as a proxy   

This is the default and the simpler option, but not necessarily the best one for a real deployment. The RPA is the only service that is "exposed" to the world. It forwards to the RPS only the requests starting with the /rps prefix and handles all the rest by itself. If the RPS and the RPA run on the same machine (the default), the RPS could listen only to the loopback network interface, which limits the possible attack vectors.   

#####Dedicated proxy   
This option involves deploying a dedicated proxy (as Nginx), which should be configured to forward all the requests with prefix /rps to the RPS, and all the rest, to the RPA. Both the RPS and the RPA should not listen to "public" network interfaces. If they are deployed on the same machine with a dedicated proxy and the rest of the customer-hosted services (M-Pin Server, D-TA) , then they might listen to the loopback network interface only.   

Please take into account the proxy considerations when considering the deployment options below.   

####Deployment Types   

#####Single-Tier Deployment   

Deploying M-Pin Core on a single machine is the default type. Since the RPS needs to communicate "privately" with the rest of the services (RPA, D-TA, M-Pin Server), this option enables this communication without being sent over a wire, which makes it more secure. The RPS and the D-TA might listen to loopback network interface only. The M-Pin Server has to have its authentication endpoints "exposed", but the M-Pin Protocol that takes place on those endpoint, is safe without being secured by additional means.   

#####Distributed Deployment   

You deploy one or more M-Pin Services on separate machines, where you set up a VPN and all the machines that host the M-Pin Services should communicate securely inside this VPN. You may have to deploy a public facing dedicated proxy (https://MIRACL.org/display/ENG/M-Pin+v0.3+-+Installation%2C+Configuration+and+Maintenance#M-Pinv0.3-Installation,ConfigurationandMaintenance-Dedicatedproxy) to propagate the requests to the services inside the VPN and configure the services to listen only on the relevant network interface(s).   

As the RPS, the M-Pin Server and the D-TA communicate with each other, they should communicate over the private network if you deploy them on separate machines. You may want to keep the communication between HTTPS/SSL secure (recommended).  Since our Python services do not support HTTPS requests, you should deploy an additional proxy with each service, so the proxy "terminates" the HTTPS connection and communicates with the corresponding M-Pin service over HTTP on the machine's loopback interface.   

A possible scenario is that the RPS, M-Pin Server and D-TA will run on one machine, as originally installed, but the actual RPA (which might be a heavy and complex web application) will run on one or more machines. In such cases, a VPN should be set up around those machines and a proper proxy should be deployed in front to propagate the requests to the RPA, RPS and M-Pin Server.   

####Default Network Configuration
By default, all the services, including the demo RPA, are installed on a single machine. The installation process itself is detailed further in this document. The services are configured as follows:
•	Demo RPA - listens on loopback (127.0.0.1) network interface, on port 8005. Communicates with the RPS over *http://127.0.0.1:8011. The RPA is proxying requests starting with the rps prefix to the RPS.
•	RPS - listens on loopback (127.0.0.1) network interface, on port 8011. Communicates with the RPA over *http://127.0.0.1:8005.
•	M-Pin Server - listens on loopback (127.0.0.1) network interface, on port 8002. Communicates with the RPS over *http://127.0.0.1:8011 and with the D-TA over http://127.0.0.1:8001.
•	Customer D-TA - listens on loopback (127.0.0.1) network interface, on port 8001
