# firebird-ctf-2024-writeup

 Insecure Webcam

From the challenge name Insecure Webcam, I assume that the pcap file must contains video/audio transmitting packets 

Step 1 

I filter out all of the other packets except for the RTP protocol because the Real-time Transport Protocol (RTP) can deliver audio and video over IP network. 

![Capture](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/52468f97-7816-4376-bb7b-d5667195888d)

Step 2  

I used the telephony > RTP > RTP streams in wireshark 

to dump the RTP packet to the capture.rtpdump  


![Capture3](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/94da7ada-33eb-4724-b16f-bdd9438f8354)
![Capture2](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/3fe5b5b0-bc6e-4600-8962-f2132f84112f)



Step 3 

After googling for a while, I found out that a tool on GitHub called RTP tool can stream rtpdump file in localhost by using the rtpplay command inside. 

 

RTP Tools: https://github.com/irtlab/rtptools.git 

 

Therefore, I installed it by cloning the RTP Tools on GitHub and build it on win 10 with visual studio 2022. 


Step 4 


![Capture5](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/87f6bd03-b176-4d88-b17a-92942722044f)

I used the rtpplay tool to stream on localhost and try to receive the stream video.
![Capture6](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/140b6785-fdfe-4480-9a07-5769e0800402)

![Capture7](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/c95cd384-b0ec-4549-8389-c57385f52148)

But it fails and it says need SDP file for streaming the video. 


Step 5 

After googling for a while, I realized I have missed some information with the RTSP protocol, which establishes and controls the media stream between client devices and servers by exchanging information between them. 

 

Therefore, I do TCP following on this RTSP packet and I see the information below can be used in the SDP file with some modifications 
![Capture8](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/94162e99-fe7d-45e4-aac5-8326824969ac)
![Capture9](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/9d5465fb-4627-4c4e-bede-af15166b865c)


The modified content in sdp file. 

![Capture10](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/06783cbc-b0f3-432f-bee0-56ae7a902c55)

Step 6 

Therefore, run the rtpplay command again and open the out2.sdp file with VLC player to receive the streaming video. 
![Capture5](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/f8316292-ba89-4cfd-8f51-d372bcdbe652)
![Capture11](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/56aa7365-d884-49ab-a386-014146058bef)

![Capture12](https://github.com/EvanLeung1917/firebird-ctf-2024-writeup/assets/128569165/a2eb3917-279e-4c3b-a698-78c1fe5a0905)


Then, I successfully retrieve the flag: 
firebird{d0n7_us3_RTP_in_y0ur_w3bcam} 


