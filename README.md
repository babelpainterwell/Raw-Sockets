Zhongwei Zhang & Alexander Wilcox
03/21/2023
CS 5700
Project #4

#*High-Level Approach*
For this project we used Python. We used several libraries, including: os, random, socket,
struct, sys, time, and urllib.parse. It made intuitive sense to encapuslate the TCP and
IPv4 headers into their own classes; those two classes were built with the appropriate
attributes (metadata), the ability to pack themselves, and with static methods to unpack
a raw packet into these objects and to verify a raw packet's checksum value. The TcpPacket
class also has functionality to calculate its flags when being packed or unpacked. The two 
raw sockets were also encapsulated into a class with methods to send and receive from these
raw sockets.  

We therefore decided to break the program down into the following components:

`rawhttpget`: The class containing main and which executes the program at a high level.
`utils.py`: A file containing various helper functions used in the other files, but which
	were general enough that they did not logically belong in the other files.
`RawSockets.py`: A class that contained the send and receive raw sockets, as well as the
	methods needed to send and receive the right packets from these raw sockets.
`TcpPacket.py`: A class that encapsulates a TCP packet, containing the approrpiate attributes
	and methods to pack itself, unpack a raw packet into a TcpPacket object, etc.
`Ipv4Packet.py`: A class that encapsulates an IPv4 packet, containing the appropriate attributes
	and methods to pack itself, unpack a raw packet into an Ipv4Packet object, etc.
`TimeoutException.py`: A very simple Exception that is thrown when no packet destined for this
	program is received within 60 seconds (see `RawSockets.py`). 

#*Who Worked On This Project*
We created this program from scratch, primarily leaning on Piazza posts and TA office hours.
Early in the project, we also found the articles on BinaryTides.com written by "Silver Moon" 
("Code a network Packet Sniffer in Python for Linux" and "Raw Socket Programming in Python 
on Linux" in particular) to be useful, even if they did contain some inaccuracies. 

This project was done in very close collaboration between the two of us, so it is hard to
give a perfect breakdown of who worked on what, but the work could generally be attributed
as follows:
- *Zhongwei*
  - Worked on all of the congestion control and flow control logic and functionality.
  - Worked on ensuring sequence and acknowledgement numbers were updated correctly.
  - Worked on tearing down the connection.
  - Debugged a tear-down issue we were experiencing. 
  - Created the functionality to sort, parse, and save the packet data to a local file.
  - Worked on receiveing packets with payloads in a loop until a "FIN" is received. 
  - Worked on parsing out HTTP headers from the received data.
- *Alexander*
  - Worked on logically decomposing the program into files, classes, functions/methods, etc.
  - Worked on setting up the promiscuous packet listener and basic filtering. 
  - Debugged a issue where we were inadvertently dropping the first packet with a payload.
  - Debugged a persistent issue with our checksum validation functions.
  - Worked on the functionality that allowed for re-transmission requests after 60 seconds.
  - Worked on logic/functions associated with resetting the connection when an "RST" is received.
  - Wrote the Python function documentation.

#*Challenges*
There were many challenges that we ran into over the course of this project. Some include:
- Figuring out the correct way to pack IPv4 and TCP packets.
- Figuring out the correct way to unpack IPv4 and TCP packets.
- Figuring out that we could not re-use the same port number unless we had previously torn down
	the connection successfully (or waited several minutes).
- Not initially realizing that we had to use `ethtool` to disable the Generic Receive Offload 
	(GRO) feature of the network interface for our VMs.
- Debugging an issue where our connection tear down would always fail.
- Figuring out why our saved files were consistently smaller than those returned by `wget`, and then
	debugging the issue (we were inadvertently dropping the first packet with a payload).

#*Testing*
Per Project 4's specification page, we tested our program against the files returned by `wget`, and
then using the `diff` program to see if the files differed. We tested against all of the log files, 
as well as a few other HTTP websites. We also made sure that our program ran in a timely fashion.

