# Distributive-peer-to-peer-file-transfer-application
There are two python files in this project. One is for the “Registration Server” named
Registration_Server.py and the other one named Peer.py for the peers.
Registration server should ideally be executed on a separate machine which would act as the
registration server.
Registration Server:
The modules needed for Registration_Server.py are time, datetime, socket, threading, os,
platform, sys, random and pickle. To execute this script, just run the file from a command line
once the module dependency is satisfied.
The user will be requested to provide the port number which will be used by the registration
server. For the purpose, it has been requested to provide port numbers in the range of 65400
and 65500 because of VCL restrictions. But there is no limitation in the code and any free port
can be provided to be used by the registration server. It would then run and wait for connections
and accordingly provide the messages on the terminal where it is running, whenever a
transaction happens.
Peers:
The modules needed for Peer.py are socket, threading,os, platform, time, pickle and sys. To
execute this script, it should be executed from a folder location such that the RFC files are
copied in a folder name RFCs in the same location. RFC files are also attached with the script
for ease of verification but it might be needed to remove some RFC files from the folder to
actually demonstrate transfer of files between peers. The peer client and peer and instantiating
in a sequence. This script would search for the existing RFC file in the local folder first and then
only connect to the remote peer. It is needed that the port numbers used by each peer server
are different.
User needs to provide the RFC number which s/he is looking for. After searching in the local
folder, RFCQuery is sent to an active peer to check if the RFC file exists there. If the RFC file is
found, it is downloaded to the requesting peer at the same time. It is important that even if there
are no RFC files present in the Peer, there should be a folder named “RFCs” in the same path
from where the script is to be executed.


Peer-to-Peer with Distributed Index (P2P-DI) System for Downloading RFCs
Internet protocol standards are defined in documents called “Requests for Comments” (RFCs). RFCs are
available for download from the IETF web site (http://www.ietf.org/). Rather than using this centralized
server for downloading RFCs, you will build a P2P-DI system in which peers who wish to download an
RFC that they do not have in their hard drive, may download it from another active peer who does. All
communication among peers or between a peer and the registration server will take place over
TCP. Specifically, the P2P-DI system will operate as follows; additional details on each component of the
system will be provided shortly.
• A registration server (RS), running on a well-known host and listening on a well-known port, keeps
information about the active peers. The RS does not keep any information about the RFCs that the
various active peers may have.
• When a peer decides to join the P2P-DI system, it opens a connection to the RS to register itself. If
this is the first time the peer registers with the P2P-DI system, it is given a cookie by the RS which
identifies the peer. The cookie is used by the peer in all subsequent communication with the RS. For
the purposes of this project, the cookie can be a small integer, unique to each peer. The peer closes
this connection after registration. When a peer decides to leave the P2P-DI system, it opens a new
connection to the RS to inform it, and the RS marks the peer as inactive. Note, however, that a peer
may leave the system without issuing a leave request, e.g., because the peer host crashed or the user
turned off the system.
• Each peer maintains an RFC index with information about RFCs it has locally, as well as RFCs
maintained by other peers it has recently contacted. It also runs an RFC server that other peers
may contact to download RFCs. Finally, it also runs an RFC client that it uses to connect to the RS
and the RFC server of remote peers.
• When a peer PA wishes to download a specific RFC that it does not have locally, it opens a new
connection to the RS and requests a list of active peers. In response, the RS provides PA with a list of
all peers who are currently active; if no such active peer exists, an appropriate message is transmitted
to the requesting peer. Peer PA then opens a connection to one of the other active peers, say, PB, and
requests its RFC index. When PA receives the RFC index from PB, it (1) merges it with its own RFC
index, and (2) searches its new RFC index (after the merge) to find any active peer that has the RFC
it is looking for. If the RFC index indicates that some active peer PC has the RFC, PA opens a new
connection to the RFC server of PC to download the RFC (note, however that PC may have left the
system by this time, and this connection may be unsuccessful). If PA does not find any peer that has
the RFC, or if its connection to PC is unsuccessful, then it contacts another peer, say, PD, in the list
of active peers it received from the RS. This process continues until either PA successfully downloads
the RFC or the list of active peers is exhausted.
• The RFC server at a peer listens on a port specific to the peer ; in other words, this port is not
known in advance to any of the peers. The RFC server at each peer must be able to handle multiple
simultaneous connections for downloads (of the RFC index or an RFC document) by remote peers. To
this end, it has a main thread that listens to the peer-specific port. When a connection from a remote
peer is received, the main thread spawns a new thread that handles the downloading for this remote
peer; the main thread then returns to listening for other connection requests. Once the downloading
is complete, this new thread terminates.
