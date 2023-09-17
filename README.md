# sniffer.sh
A script to assess and determine how often and what kind of traffic is coming to and from a website.

The ultimate goal of this project is to write a script to assess and determine how often and what kind of traffic is coming to and from a website. Such objective is achieved via the usage of a set of tools, i.e., a command-line packet analyzer (TCPDump) and a network protocol analyzer (Wireshark).

In order to streamline the process of capturing & dumping the data packets, a simple shell script may be leveraged that automates the steps, filters the relevant data and dumps it into a .pcap file, to be subsequently analyzed with Wireshark.

The first step is to write the shell script to use TCPDump and capture network traffic (data packages) between our local host and the targeted website - which, for privacy and security reasons, I’ve redacted from the script and substituted it with “<url>”. The final version of the script is as follows:

sudo tcpdump -XXtttt -c 15 -C 5 -G 900 host <url> -w captured.pcap

The script breakdown is:

sudo → TCDump requires super-user privileges to capture packets. If we do not include the sudo command, we will most likely get an error such as “tcpdump: <network interface>: You don’t have permission to perform this capture on that device”.

tcpdump → the packet analyzer we’ll use to capture the traffic and dump the data into the .pcap file.

-#XXtttt → the “XX” prints the data of each packet both in hexadecimal and ASCII notation, while the “tttt” prints a timestamp, as hours, minutes, seconds, and fractions of a second since midnight, all preceded by the date.

-c 15 → the “-c” option stands for “count” and determines how many packets we would like to capture before exiting. In this case, we’ve set it to “15” packets, so it wouldn’t run indefinitely.

-C 5 → the “-C” determines the maximum file size, which means taht, before writing a raw packet to a savefile, we want TCPDump to check whether the file is currently larger than <file size> and, if so, to close the current savefile and open a new one.

-G 900 → the “-G” option rotates the dump file specified with the -w option every <seconds>, meaning it will overwrite the file with new packets every <seconds>.

host <url> → determines the source/destination host of the packets to be captured from/to. 

-w captured.pcap → the “-w” option writes the raw packets to a file (in this case, “captured.pcap”) rather than parsing and printing them out. The .pcap extension is the default for packets captured.

After writing and saving the script (in this case, we’ll call it “sniffer.sh”), we must make it an executable file. We accomplish this by using the “chmod” command within the terminal to specify the execute mode for the script (i.e., to make it an executable file):

$ chmod +e sniffer.sh

Than, we may run our script using TCPDump by excuting it in the terminal:

$ sniffer.sh

Now, in order to see any results, we must open a browser and log on to the specific url we want to monitor. Once we do that, we’ll have results showing on the terminal and the data packets should be dumped to the savefile. 

After that, all we need to do is to open the file with TCPDump, using the “-r” (read) option:

$ sudo tcpdump -r captured.pcap

Alternatively, we may run Wireshark and open the captured.pcap file to analyze the traffic.

Disclaimer: This project is for educational purposes only and based on the “Analyze Network Traffic with TCPDump” Coursera Project Network, by Harrison Kong (https://www.coursera.org/projects/analyze-network-traffic-with-tcpdump). It should not be used with harmful intent against any third-party networks.
