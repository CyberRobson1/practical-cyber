Lab 01: Network Traffic Analysis & Protocol Investigation


Hello everyone! These are my raw notes. To be totally honest, I was actually pretty anxious before starting this. I kept overthinking it, so please keep that in mind as you read this!

Getting Started

I started by cloning my GitHub repo. I was surprised at how easy it was. I used "mkdir" to build the folders and "touch" to create the files. It felt like I was actually building my own workspace.

I had a little trouble opening Wireshark at first. I tried writing "open wireshark" which didn't work. Then I remembered I could check the version to see if it was even there. I ran "wireshark --version" and saw version 4.6.4 pop up. Then I just launched it by typing "wireshark" and the GUI opened.  First win of the day.


What am I even looking for?
I wanted to generate traffic that would show me some protocols so I could actually understand them, not just read about them. I decided to look for: DNS, HTTP, ICMP, and TLS. I also added the TCP Handshake and ARP because they felt important for the "full story."


The "Surgery"
Generating the traffic felt like doing a high-class surgery, haha! I picked the "eth0" interface because that's the "Front Door" where all the packets enter.

I visited Google for a secure connection and then "neverssl.com" for an insecure one. To make sure I had everything, I opened my terminal and ran a ping command. Seeing those 4 pairs of ICMP requests and replies show up in Wireshark felt so satisfying. In total, I captured 1709 packets. It was a bit overwhelming at first, but I'm learning how to find the signal in the noise.


My Thoughts as I go (The Identity Check)
I ran "ifconfig" because I forgot my own IP. I found out my ip and the gateway one.
I had a moment of panic because I saw the gateway asking for MY address in ARP. I realized this is just the virtual environment's way of making sure it knows exactly where to send my data. It’s just the router doing its job to keep us connected.

The Investigation

ARP (Address Resolution Protocol)
What it is: ARP is what my computer uses to find the hardware (MAC) address of another device. Since I only know the IP, ARP finds the physical address so the data can actually be sent.
My Finding: I saw 3 pairs of results (6 packets). I realized it’s just the computer checking the connection every 30 seconds to make sure the gateway is still there.

DNS (Domain Name System)
What it is: This is how my browser finds the IP address for a website name. I give it the name, and it gives me back the numbers.
My Finding: I had 80 packets of noise! I used dns.qry.name contains "neverssl" and got it down to 20. I found the website's IP. Looking at the "Answers" section in the response felt like finding a secret code.

TCP Handshake (Transmission Control Protocol)
What it is: This protocol makes sure data gets delivered reliably. Before the data moves, it uses a 3-way handshake to make sure both the client and the server are ready to talk.
My Finding: This was the coolest part. I saw the SYN, SYN/ACK, and ACK. I also noticed that the connection didn't always say "Goodbye" politely; sometimes it just used an RST (Reset) to cut the line.

ICMP (Internet Control Message Protocol)
What it is: This is used for testing and diagnostics. When I ping a server, I’m sending a tiny packet just to see if the other side can talk back.
My Finding: I captured the 4 pairs of pings I did in the terminal. In Wireshark, they look so clean: Request, Reply, Request, Reply. It proves the target is alive and reachable.

HTTP vs. TLS
What it is: HTTP is the way web pages are sent, but it isn't protected. TLS is the layer that encrypts that traffic so it stays private.
My Finding: Firefox tried to use a secure connection (TLS) even for NeverSSL! Modern browsers try to be safe first. I had to use an Incognito window to finally see the unencrypted HTTP data. I followed the stream and there it was: Clear text. Red lines for me, blue lines for the server. I could read everything.


Final Reflection
I feel a lot better now that this first lab is done. The "heaviness" I felt this morning is gone. I learned that networking isn't just a bunch of letters like ARP and DNS—it’s a conversation. I'm excited to keep exploring and capture good findings.
