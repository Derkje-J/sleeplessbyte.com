---
layout: post
title: "Retroshare Network Configurations"
date: 2013-07-31 15:01
comments: false
categories: [retroshare]
---
![](/images/headers/network.jpg)
Over the past few days there has been a lot of questions around the network configurations of [Retroshare](http://retroshare.sourceforge.net/), an application that provides secure [Friend-To-Friend (F2F)](https://en.wikipedia.org/wiki/Friend-to-friend) networks. There is not a lot of humanly readable documentation on the matter. I've been a Retroshare user for over 14 months at the time of writing and wrote this article as a part of a series on retroshare. In this article I'll explain each of the four network configurations and their consequences.

On january 6, 2013 the Retroshare team wrote [an article](https://retroshareteam.wordpress.com/2013/01/06/privacy-on-the-retroshare-network/) on privacy on the Retroshare Network. It explains some basics and isn't a bad read at all. 

<!-- more -->

I ought to cover a few basics in this article. As a retroshare user you want to **be able to establish connections** and **remain as private as possible**. However the former, the level of connectivity, decreases when you increase the latter and visa-versa. So let's see how much you want to exposes and chose your settings accordingly.

# Network Configuration
![](/images/inline/configuration.jpg)
When you use the Setup wizard or press Options > Server you can choose from four network configurations. These determine the basic connectivity and privacy levels of your RetroShare client. They (see 1 in the image) are ordered from public: best connectivity, least private to darknet: no connectivity, most private. Each option is a permutation of two options: DHT and Discovery.

## DHT: Distributed Hash Table / The Phonebook on Facebook
[DHT](http://en.wikipedia.org/wiki/Distributed_hash_table) is a technology that is used to find IP addresses. When DHT is enabled your IP is shared with all your connections. It is linked to your **Location ID**. Your Location ID is an unique ID for each location (each computer you run RetroShare on). However, you can not link a Location ID to a Identity ID, which is an unique ID for each certificate and links to you, unless you are friends with someone. As long as someone doesn't have your Location ID, nothing 'vital' is shared.

__Why does this increase connectivity?__ Well, if you change your IP or are firewalled, your new address will be in the DHT. All your friends, some who may be distant, know your Location ID. since the IP is linked to a location ID and the table of Location ID -> IP is distributed across the network, eventually your friends will receive the new IP and be able to connect to you. Yay for distribution.

__Why does this decrease privacy?__ If someone has your Location ID, he or she automagically has your IP address and the fact that you are using RetroShare can be tracked. Your IP address is also known by the public. **Note:** How you are using RetroShare is not visible from the outside, merely the fact that you are using RetroShare. 

- On a sidenote: any outgoing connection to any service exposes your IP address, so consider how important this is to you. 
- For the nerds: Yes you can spoof or proxy your IP. In the former you can't maintain a connection and with the latter the proxy will still know your IP address. RetroShare uses [Mainline DHT](http://en.wikipedia.org/wiki/Mainline_DHT).

## Discovery: Friends of Friends on Facebook
With this option enabled RetroShare will share your friends list with your friends. By sharing the list I mean the Certificates and Known IP's. This means that your friends will receive your connections, their Location ID's and their IP's. However this still doesn't mean they can see what you are doing. They also can't 'just connect' to your friends. Certificates still need to be mutually accepted. 

__Why does this increase connectivity?__ Your IP will be known by your extended network (friends of friends). If you change IP or are firewalled you only need one mutual friend to be connected for all other mutual friends to be able to find you. Great! This works particulary good between people with a lot of mutual friends and will overcome the problems when one of the two has DHT disabled.

__Why does this decrease privacy?__ Obviously the friends of your friends will know your IP address and certificate. It is less intrusive than DHT option as it is only shared with the extended network, but it is more intrusive than the DHT option as it shares the certificate instead of merely a hash. Why shouldn't you care? Well you are supposed to __trust__ your friends. Apart from that, the certificate is supposed to be public. 

# Choosing an option
Consider at least enabling one of the two options. By default RetroShare runs in the Public configuration to optimise connectivity. If you have a interconnected network consider running on Private. Your friends will supply the IP updates and you don't expose your information. 

**The following is __important__:**
- Once you enable DHT and connect to 1 person, your IP is shared. Period. It is exposed. 
- You need at least one friend to get contents into your DHT. Duh, there are no servers whatsoever.
- When not running in Darknet, your IPs are added to your Certificate. Don't forget this when sharing your certificate. 

## Darknet: Hello Privacy, Goodbye connections
If you are a real paranoid or if you simply don't want to share ANYTHING you can select the Darknet option. Please note that you can not be found by your friends. This means that the only way you can connect to a friend is if they share their information or you are already connected. 

__Wait what?__

Okay. So to connect to a friend one of you needs to initiate a handshake. You are running a DarkNet so your details are never (!) shared. The only way you can connect to a friend is if you initiate the handshake. This will get you into connectivity problems if your friend moves or is behind a firewall. Additionally, even if you manage to connect, you will still run into problems when you move or are behind a firewall. 

If you run a local F2F network this option provides total security but don't come crying when your connections are failing. 

## Dynamic DNS (3): A compromise
So you don't want to be listed in the phonebook (DHT) and you don't trust the friends of your friends. You can still get some connectivity by enabling Dynamic DNS. See [this](http://dnslookup.me/dynamic-dns/) extensive list of providers. Instead of globally sharing your IP through DHT or sharing it to your extended network through discovery you only add a Dynamic DNS entry to your Certificate. This is simply another lookup that is done by your friends. It works like this:

- Your __dynamic__ IP is send to a DNS service
- The IP is bound to a __static__ hostname
- The hostname is exposed

__Why does this increase connectivity?__ Although your friends won't be able to find you through DHT or Discovery, your hostname's lookup will update when your IP changes. Everyone that has your certificate will eventually get the updated IP and be able to connect. This way you don't need to distribute to your extended network and don't need to be added to the DHT.

__Why does this decrease privacy?__ The people that have your DNS lookup name (read: your certificate) will be able to find your IP. However only your friends are supposed to have that certificate anyway, so no problems here. Additionally the service you use obviously knows your IP address. You ought to trust at least someone! **Note:** Run in Darknet or your details are shared regardless of this setup. Setting up DynamicDNS isn't particulary easy, so consider this when choosing for this option.

## Side Note (2): External IP finder
In order to find your external IP, becuase you are behind a router (NAT) or firewall, an external site is used. These sites are all generally known for IP lookups so should be save. There is no way the site could figure out that RetroShare is asking for the IP, but if you don't trust these sites or are particulary paranoid you can turn off this service.

__Why does this increase connectivity?__ Your Local IP can only be used by your Local Network (e.g. your home network, your work network). Your friends usually need your External IP to be able to connect to you. See your Local IP as the phone number you dial internally and the External IP as the phone number you dial after a 0 (external number). 

__Why does this decease privacy?__ It doesn't, except that one of the sites in this list will know thay you exist. That's it. But hey, you can go all dark and turn this off. 

# What is shared?

The [article](https://retroshareteam.wordpress.com/2013/01/06/privacy-on-the-retroshare-network/) mentioned above has a good section on what is shared and what is associated to you. Take this in consideration when sharing. This has nothing to do with DHT or Discovery.  I could repeat what is said there, but I will merely show the all comprehensive image

![](http://retroshareteam.files.wordpress.com/2013/01/public_grid1.png)

# Conclusions
Retroshare provides a lot of functions to increase or decrease connectivity. Its up to you to choose how comfortable you are sharing certain amounts of information. My take on this is use public and keep in mind everyone has your IP when you're sharing. Remember having someone's IP address doesn't mean knowing what he or she is using RetroShare for.

# Further reading

- I wrote [this article](/downloads/article-darknet.pdf) on Darknet and it was published in the faculty magazine called MaChazine of the EEMCS faculty on Delft, University of Technology
- Read the [article](https://retroshareteam.wordpress.com/2013/01/06/privacy-on-the-retroshare-network/) I keep referencing from the RetroShare team blog
- Read more about [DHT](http://en.wikipedia.org/wiki/Distributed_hash_table) and [Mainline DHT](http://en.wikipedia.org/wiki/Mainline_DHT) on WikiPedia
- Read about [Friend-To-Friend (F2F)](https://en.wikipedia.org/wiki/Friend-to-friend) networks in general

If you have questions, please connect to me at [@DerkJan_Delft](https://twitter.com/DerkJan_Delft).
