

<b>Project description</b><br>
I am a security analyst working for a company that wants to detect certain TCP/IP packets on the server, specifically web traffic. The company wants to test an external server by looking at TCP/IP requests and responses, as well as look at inappropriate website access through the server.
The IT manager wants to be able to capture certain ethernet network traffic and be able to detect certain IP addresses as well.

<b>Objective 1 – Install on Ubuntu and set up Wireshark for users belonging to the Wireshark group.</b><br>
To get the latest version of Wireshark the command sudo add-apt-repository ppa:wireshark-dev/stable is typed. To just get Wireshark just add-apt install wireshark would have surficed.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Installing%20Wireshark%20on%20Ubuntu.PNG"/>

In addition a user needed to be added to the wireshark group, and this had to be done using sudo permission. The command is sudo usermod -aG wireshark $USER which will add the existing user in the group.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Adding%20a%20new%20user%20to%20the%20wireshark%20group%20with%20sudo%20permission.PNG"/>

Finally, the now logged in user logs out, and logs in as himself. This is necessary because as a security analyst I want to use the principle of least admission. The user needs to have access to Wireshark but not be able save any files under root nor have any other sudo permissions.

<b>Objective 2 - Start a packet capture on an ethernet port and save it to file</b><br>
Opening Wireshark showed the available interfaces. Lookback capture for instance shows the packet capture from a machine to itself such as running one’s own web application on local host.
Here en will be used for ethernet (ens5).
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/The%20Wireshark%20interface.PNG"/>
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Capture%2C%20stop%20capturing%20and%20save%20packets%20in%20Wireshark.PNG"/>

<b>Objective 3 - Use a display filter to detect HTTPS packets</b><br>
A display filter was used, and not a capture filter. Specifically port 443 to capture only HTTPS traffic.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20captured%20packets%20using%20tcp%20port%20443.PNG"/>
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Information%20about%20the%20captured%20package%20using%20tcp%20port%20443.PNG"/>


<b>Objective 3 extra - Use a display filter to detect HTTPS packets</b><br>
First only HTTP traffic (not HTTPS) from a website was captured, and saved to a file.
HTTP uses port 80 which is very well known, so after using tcp.port == 80 as a display filter in Wireshark interface, the result can be shown in the attached image below.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20captured%20packets%20for%20HTTP%20traffic.PNG"/>

<b>Objective 4 - Visit a web page and detect its IP address using a display filter</b><br>
There may be an incident where a certain known IP address causes problem for the server.
We can use the filter tls.handshake == 1 as shown below.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Client%20Hello%20for%20two%20websites.PNG"/>
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20all%20traffic%20from%20that%20website.PNG"/>

When the website is sending packets, this is the results in Wireshark with chosen display settings.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20when%20the%20website%20is%20sending%20something.PNG"/>

Any other cases when we are sending something to that website, this is the results in Wireshark with chosen display settings.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20all%20cases%20when%20we%20are%20sending%20something%20to%20that%20website.PNG"/>

<b>Objective 5 - Locate all HTTPS packets from a capture not containing a certain IP address</b><br>

It is important to locate import data to avoid clutter.

Traffic from one of the websites
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Traffic%20from%20one%20of%20the%20websites.PNG"/>


Traffic from one of the websites (only HTTPS)
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Traffic%20from%20one%20of%20the%20websites%20and%20only%20HTTPS.PNG"/>

Traffic that does not come from that website and is HTTPS
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/The%20traffic%20that%20does%20not%20come%20from%20that%20website%20and%20is%20HTTPS.PNG"/>

Traffic not from that website and are both HTTP and HTTPS
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Traffic%20not%20from%20that%20website%20and%20are%20both%20HTTP%20and%20HTTPS.PNG"/>

<b>Objective 6 – use Wireshark to create a capture file, and then use a display filter to list all https and http packets. Then eliminate one IP address from the capture using a filter.</b><br>
First step was using tcp.port == 80 as a filter which singled out one IP known to be http.
The next step is to eliminate that and show all traffic for the rest of the IP addresses.
<img src = "https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/blob/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Capstone%20final.PNG"/>
That displays the final result.

<b>Summery</b><br>
The purpose of this project was to capture packets, save them and use filters to display certain HTTP and HTTPS packets. The packets to examined are TCP/IP packets going to or from a server.
It also clearly demonstrates an understanding of security principles at work.







 


