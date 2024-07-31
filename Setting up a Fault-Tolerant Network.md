In this scenario, there are two subnets connected by two redundant
routers. However, I am going to set them up in a way so that they
cooperate and work together as one gateway using the Hot Standby Router
Protocol (HSRP).

### Before Setting Anything Up

Before I mess with any router settings, I tested connectivity between
the two end users and all packets timed out.

![](output\Setting up a Fault-Tolerant Network/media/image10.png){width="10.0in"
height="2.3194444444444446in"}

I am now going to set both Router 1 (R1) and Router 2 (R2) to have the
same **virtual HSRP** ip address for both respective subnet interfaces,
however I will set R1 to be the default router used. Only when R1 goes
down, will R2 start to act.

![](output\Setting up a Fault-Tolerant Network/media/image8.png){width="10.0in"
height="4.111111111111111in"}

### Router 1

For R1, I will set the Gig0/0 interface, facing the blue subnet, to be
192.168.10.1. Even though I will do the same thing for R2's Gig0/0, I am
going to set R1's interface to have a higher priority so that it will be
used by default.

![](output\Setting up a Fault-Tolerant Network/media/image4.png){width="5.953125546806649in"
height="5.370627734033246in"}

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

![](output\Setting up a Fault-Tolerant Network/media/image12.png){width="5.869792213473316in"
height="3.376829615048119in"}

### Router 2

Now, I will repeat similar steps for R2. However, I won't change
anything regarding priority as it is already a lower priority than R1 by
default. I will simply configure the ip addresses of the two interfaces
to match R1's interfaces.

![](output\Setting up a Fault-Tolerant Network/media/image1.png){width="7.046875546806649in"
height="5.2058792650918635in"}

I notice here that the "state" of R2 is set to **Standby**, while R1
shows that it went to **Standby** and then to **Active**. I take this as
confirmation that things are going in the right direction.

I can also type 'show standby' to see these statuses again. I only took
a screenshot of the first interface for clarity.

For R1

![](output\Setting up a Fault-Tolerant Network/media/image9.png){width="5.041666666666667in"
height="1.0520833333333333in"}

For R2

![](output\Setting up a Fault-Tolerant Network/media/image11.png){width="5.052083333333333in"
height="1.1354166666666667in"}

### Testing Everything

Now to finally test all the settings, I should be able to send a ping
from a computer in one subnet, to a computer in the other subnet by
following the router priority rules set. The 'tracert' command is
perfect for this.

PC1's ip address is 192.168.10.101

Laptop1's ip address is 192.168.20.101

From PC1 to Laptop1 - connectivity is finally established

![](output\Setting up a Fault-Tolerant Network/media/image2.png){width="6.864583333333333in"
height="2.5104166666666665in"}

From Laptop1 to PC1

![](output\Setting up a Fault-Tolerant Network/media/image7.png){width="6.854166666666667in"
height="2.46875in"}

Finally the two computers can talk to each other, and they both used
R1's IP address, which is what we want. It seems different than the IP
address set up (182.168.20.1), because those are HSRP's virtual IP
addresses used to coordinate the two routers.

If I completely disconnect R1 from the network, this is the tracert
output from PC1 to Laptop1

![](output\Setting up a Fault-Tolerant Network/media/image6.png){width="6.625in"
height="1.9166666666666667in"}

It is now headed to R2's IP address, because R1 is no longer connected
on the network.

### HOWEVER, One scenario I can't figure out

The above tracert output only happens when I completely disconnect R1
from the network from both interfaces.

If I only disconnect Gig0/0 and leave Gig0/1 connected on R1 (as seen in
the picture below), then the packet from PC1 to Laptop1 times out. If I
send a packet from Laptop1 to PC1, then it just recursively sends
packets to R1 and never stops.

![](output\Setting up a Fault-Tolerant Network/media/image5.png){width="8.036458880139982in"
height="3.022042869641295in"}

When I configure R2 and type 'show standby' I see that the first Gig0/0
is 'Active', which is what we want, but Gig0/1 is on 'Standby' which is
likely why the packets are timing out. I'm not sure how to resolve this
currently.

![](output\Setting up a Fault-Tolerant Network/media/image3.png){width="5.682292213473316in"
height="4.3716568241469815in"}
