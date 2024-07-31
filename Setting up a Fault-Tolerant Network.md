## Objective
In this scenario, there are two subnets connected by two redundant
routers. However, I am going to set them up in a way so that they
cooperate and work together as one gateway using the Hot Standby Router
Protocol (HSRP).

## Skills Learned
Understanding and implementing fault tolerance in networking<br>
Configuring HSRP on routers and setting router priorities<br>
Verifying router states<br>
Troubleshooting network connectivity issues<br>
Testing network configurations with 'tracert' and 'ping'<br>

## Before Setting Anything Up

Before I mess with any router settings, I tested connectivity between
the two end users and all packets timed out.

![image10](https://github.com/user-attachments/assets/d1c8c423-eba5-46d4-8592-d48a44d4233c)


I am now going to set both Router 1 (R1) and Router 2 (R2) to have the
same **virtual HSRP** ip address for both respective subnet interfaces,
however I will set R1 to be the default router used. Only when R1 goes
down, will R2 start to act.

![image8](https://github.com/user-attachments/assets/82cfe20b-1bcf-4a52-8a0f-9727951607ff)


## Router 1

For R1, I will set the Gig0/0 interface, facing the blue subnet, to be
192.168.10.1. Even though I will do the same thing for R2's Gig0/0, I am
going to set R1's interface to have a higher priority so that it will be
used by default.

![image4](https://github.com/user-attachments/assets/53933ac3-6c56-4e25-bf95-d14929301636)


Here is the summary of commands I entered. I used the "?" switch to find
more information about the commands as well which was very helpful.

**#standby 1 ip 192.168.10.1** - In group 1, set the IP address of that
interface to 192.168.10.1

**#standby 1 priority 150** - In group 1, change the priority of that
interface to 150. The default priority value is 100, with higher numbers
having higher priority.

**#standby 1 preempt** - This makes it "overthrow" other active routers
that have lower priority values which is the whole point of this

I will now repeat the same steps for the other interface on R1, facing
the other subnet.

![image12](https://github.com/user-attachments/assets/74149e2a-dc3f-4415-97de-9206e24061cb)


## Router 2

Now, I will repeat similar steps for R2. However, I won't change
anything regarding priority as it is already a lower priority than R1 by
default. I will simply configure the ip addresses of the two interfaces
to match R1's interfaces.

![image1](https://github.com/user-attachments/assets/da434d1e-9bc8-4897-9dbd-db0cd4290c7c)


I notice here that the "state" of R2 is set to **Standby**, while R1
shows that it went to **Standby** and then to **Active**. I take this as
confirmation that things are going in the right direction.

I can also type 'show standby' to see these statuses again. I only took
a screenshot of the first interface for clarity.

For R1

![image9](https://github.com/user-attachments/assets/a1e1e5b8-9816-4e10-98c2-778dff99209d)


For R2

![image11](https://github.com/user-attachments/assets/9bf8837a-8a45-4c8b-9901-226186244ae1)


## Testing Everything

Now to finally test all the settings, I should be able to send a ping
from a computer in one subnet, to a computer in the other subnet by
following the router priority rules set. The 'tracert' command is
perfect for this.

PC1's ip address is 192.168.10.101

Laptop1's ip address is 192.168.20.101

From PC1 to Laptop1 - connectivity is finally established

![image2](https://github.com/user-attachments/assets/0ef243ab-798d-44e0-8d0a-a5f3c666a55a)


From Laptop1 to PC1

![image7](https://github.com/user-attachments/assets/090e3932-e053-42ba-99b3-87631caf5b23)


Finally the two computers can talk to each other, and they both used
R1's IP address, which is what we want. It seems different than the IP
address set up (182.168.20.1), because those are HSRP's virtual IP
addresses used to coordinate the two routers.

If I completely disconnect R1 from the network, this is the tracert
output from PC1 to Laptop1

![image6](https://github.com/user-attachments/assets/de5af68b-180e-4a25-9275-6d7f3f154a6c)


It is now headed to R2's IP address, because R1 is no longer connected
on the network.

## HOWEVER, One scenario I can't figure out

The above tracert output only happens when I completely disconnect R1
from the network from both interfaces.

If I only disconnect Gig0/0 and leave Gig0/1 connected on R1 (as seen in
the picture below), then the packet from PC1 to Laptop1 times out. If I
send a packet from Laptop1 to PC1, then it just recursively sends
packets to R1 and never stops.

![image5](https://github.com/user-attachments/assets/fbf38f4a-11b1-48d1-9df5-3427c62bff6b)


When I configure R2 and type 'show standby' I see that the first Gig0/0
is 'Active', which is what we want, but Gig0/1 is on 'Standby' which is
likely why the packets are timing out. I'm not sure how to resolve this
currently.

![image3](https://github.com/user-attachments/assets/aad8ddd4-5bfe-4b74-87ef-ccf120be7426)

