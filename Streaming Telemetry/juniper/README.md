Note: This install requires the openconfig and network agent packages from Juniper. Support for Juniper platforms and code trains varies. Please check the Juniper documentation if you are unsure whether your device is supported.  The network agent is included with the Junos OS image starting in 18.3R1 for supported platforms.

JTI Overview: https://www.juniper.net/documentation/en_US/junos/topics/concept/junos-telemetry-interface-oveview.html

Network Agent Overview: https://www.juniper.net/documentation/en_US/junos/topics/task/installation/network-agent-installing.html

### JTI Install


### Disable ISSU (The OpenConfig and Network Agent packages do not support ISSU for install. You can re-enable these options once the install is complete)

delete chassis redundancy graceful-switchover

delete protocols layer2-control nonstop-bridging

delete routing-options nonstop-routing



### Install the OpenConfig package 

request system software add /var/tmp/$openconfigPackage



### Install the network agent

request system software add /var/tmp/$networkAgent


### JTI w/o SSL

set system services extension-service request-response grpc clear-text port 50051
set system services extension-service request-response grpc skip-authentication
set system services extension-service notification allow-clients address 141.142.141.140/32



### JTI w/ SSL. Still in progress

set system services extension-service request-response grpc ssl

set system services extension service request-response grpc ssl local-certificate $certificate

set system services extension service request-response grpc ssl ip-address $telegraf_host

set system services extension service request-response grpc ssl port 32767



########################################################################################################


Upgrading JUNOS:  Steps required if your performing a Junos update on an existing JTI install.


Make sure that the OpenConfig and NetworkAgent are copied into /var/tmp.

Disable Configuration:  
delete chassis redundancy graceful-switchover

delete protocols layer2-control nonstop-bridging

delete routing-options nonstop-routing

delete system services extension-service request-response grpc clear-text port 50051
delete system services extension-service request-response grpc skip-authentication
delete system services extension-service notification allow-clients address $telegraf_host



Add packages
request system software add /var/tmp/$openconfigPackage

request system software add /var/tmp/$networkAgent



Re-Enable configuration:
set chassis redundancy graceful-switchover

set protocols layer2-control nonstop-bridging

set routing-options nonstop-routing

set system services extension-service request-response grpc clear-text port 50051
set system services extension-service request-response grpc skip-authentication
set system services extension-service notification allow-clients address $telegraf_host
