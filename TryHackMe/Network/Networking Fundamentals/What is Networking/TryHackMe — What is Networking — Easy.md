**Difficult** -> *easy*
**Date** -> 16-jul-26
**Type** -> Training
**Weblink** -> [https://www.tryhackme.com/room/whatisnetworking](What is networking?)
## Introduction
```plain text
Begin learning the fundamentals of computer networking in this bite-sized and interactive module. (THM definition)
```
In this WriteUp I will documentate the process for the "**What is networking?**" room, what I learned and what mistakes I did.
## Solution
### Task 1 — What is networking?
Is only a little explication about what is a networking and what is a network itself. The networks are things connected and networks are part of our lives everyday
*Question 1: What is the key term for devices that are connected together?*
Answer: **Network**
![](<Pasted image 20260716191317.png>)
In the image of above, are a simple example of a network of friends Alice, Bob and Jim
### Task 2 — What is the internet?
The internet is a massive one network consisting in a many small networks.}
![](<Pasted image 20260716192206.png>)
Now in the image of above, Alice introduce Zayn and Toby to Bob and Jim but Alice only can speak the language of Zayn and Toby. Now, Alice made a new network because she is the only one can speak with Zayn and Toby and Bob and Jim in their respective languages.
The first network in history was the **ARPANET** project builded by the United States Defense Departament in the late 1960. The "modern" network was created in 1989 by **Tim Berners-Lee** with his World Wide Web (**WWW**).
![](<Pasted image 20260716192859.png>)
The image of above is a network representation of Alice and her friends. Alice is the Internet, while Bob and Jim are a 1nd network and Zayn and Toby are the 2nd network
*Question 1: Who invented the World Wide Web?*
**Answer: Tim Berners-Lee**
### Task 3 — Identifying devices on a Network
In a network, all the devices must be identifying and identifiable. The devices has two ways to identifying itself
1. IP Address: can change
2. MAC Address: can't change
#### IP address
An IP address is an identifier for a device. The IP can change with the time and being assignated to another device without the IP can change. But the IP cannot be assigned to 2 devices at the same time.
IP is divided in 4 groups separated with points (`.`), this groups are called octets
![](<Pasted image 20260716201023.png>)
The devices can be on both a private and public network and depending on the type of network, the IP can be public or private. The IP in the image of above are an IPv4.
#### MAC address
The devices has an network physical interface in the motherboard. This interface is assigned at the factory. This is called Media Access Control (**MAC**). Consists in a twelve-character hex number split in two separated with a colon (:)
![](<Pasted image 20260716202955.png>)
The MAC address can be faked with a method called **spoofing**. This is when a device pretends be another device using its MAC address.
*Question 1: What does the term "IP" stand for?*
**Answer: Internet Protocol**
*Question 2: What is each section of an IP address called?*
**Answer: Octet**
*Question 3: How many sections (in digits) does an IPv4 address have?*
**Answer: 4**
*Question 4: What does the term "MAC" stand for?*
**Answer: Media Access Control**
*Question 5: Deploy the interactive lab using the "View Site" button and spoof your MAC address to access the site.  What is the flag?*
![](<Pasted image 20260716204116.png>)
The first image is the lab without change. We need spoofing the Alice MAC address into Bob's machine.
![](<Pasted image 20260716204300.png>)
Only copy the grey MAC in the text box down in the Alice machine
![](<Pasted image 20260716204434.png>)
When press the "Request button we got the flag"
**Answer: THM{YOU_GOT_ON_TRYHACKME}**
### Task 4 — Ping (ICMP)
**ICMP** is an acronym for Internet Control Message Protocol. Its a useful tool to help verifying if the connection with other device exists. The time taken for ICMP packets between devices is called *ping*.
*Question 1: What protocol does ping use?*
**Answer: ICMP**
*Question 2: What is the syntax to ping 10.10.10.10?*
**Answer: ping 10.10.10.10**
*Question 3: What flag do you get when you ping 8.8.8.8?*

![](<Pasted image 20260716231659.png>)
Open the Site attached and put the `8.8.8.8` in the textbox and press the button "Send Ping Request"
![](<Pasted image 20260716231758.png>)
We got the flag
**Answer: THM{I_PINGED_THE_SERVER}**___
