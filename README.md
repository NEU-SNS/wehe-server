# Wehe

This software is licensed under the Apache License (see LICENSE.txt).

How to run a replay step by step:

Set up Wehe server:

* Follow the steps in rootcahowto.txt to generate a self-signed certificate for secured communication.
* Change the folders.txt to where the replay files are ( e.g., /replayTraces/Amazon_12122018/ by default)
* Start the replay servers by ./restartServers.sh

Using the Wehe app to test:

* Assume the server address is 1.2.3.4. Change the default setting in Settings to manually set the server address to 1.2.3.4 before running a Wehe test.

Using the python Wehe client:

* Assume the server address is 1.2.3.4. Add another entry in ```class Instance``` in python_lib.py for 'test_server : 1.2.3.4' . You can then run a replay of the recorded traffic

```bash
python replay_client.py --pcap_folder=the/dir/to/pcap --serverInstance= test_server
```

How to create your own replay

Record the traffic by using tcpdump, e.g., running tcpdump while watching a video and save the pcap file as app1.pcap. We will use the parser script to process the pcap and create the replay files (in JSON format) that can be used by the client and server.

Assume the pcap is stored in the/dir/to/pcap.

On the client:

* Create the pickle with original payload

```bash
sudo python replay_parser.py --pcap_folder=the/dir/to/pcap
```

* To randomize the payload in the original traffic (replace the original content with random strings)

```bash
sudo python replay_parser.py --pcap_folder=the/dir/to/pcap --randomPayload=True --pureRandom=True
```

* Do bit inversion in the original traffic (invert every bit in the content)

```bash
sudo python replay_parser.py --pcap_folder=the/dir/to/pcap --randomPayload=True --bitInvert=True
```

* Copy the replay directory (the/dir/to/pcap) to the server via scp


## Containerization

This API has been containerized. To run it within a container first go to the cloned directory and build with 
```
sudo docker build . -t wehe
```

Then run with 
```
sudo docker run -v <where/the/certs/are>:/wehe/ssl -v <where/to/save/the/output/on/host>:/data/RecordReplay --env SUDO_UID=$UID --net=host -itd wehe
```

Remove d from `-itd` to run outside of detached mode and see the output in STDOUT
