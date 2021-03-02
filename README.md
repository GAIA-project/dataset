# DATASET DESCRIPTION

Overall, in the Green Awareness In Action (GAIA, http://gaia-project.eu/) H2020 project a large-scale IoT deployment comprising various types of sensors (power consumption, temperature, light, particulate matter, etc.) has been implemented across school buildings in 3 European countries (Greece, Italy and Sweden). A typical installation in such a school contains 6 environmental nodes and a couple of power measurement nodes. The initial deployment has also been expanded and supported by the E2Data H2020 project (https://e2data.eu/).

In this dataset, we focus on the network level and omitted the part of the measurements related to actual environmental and energy-related aspects. The dataset contains network and quality of service measurements from 6 different school buildings. In these buildings, we have deployed 8 individual networks consisting of a total of 49 sensor devices. Out of these 8 networks, 5 are based on LoRa and 3 are based on IEEE 802.15.4. In Schools A, B and C we used the existing LoRa based infrastructure while in Schools D, E and F the existing IEEE 802.15.4 network. To compare these two networks in the same buildings, we also deployed two more temporary LoRa-based networks in Schools E and F. The dataset is organized in five experiments we conducted and contains periods of data from May 2019 to March 2020. A total of 92 days of experimentation were devoted to each of the experiments that will be presented in the next sections. The total size of the calculated statistics dataset is 40.2 MB (this figure includes only the statistics for the network and not the actual sensed parameters).

In each LoRa installation, we collected several parameters to evaluate its performance and the quality of the communication. As link state metrics, we collect the Received Signal Strength Indicator (RSSI) and Signal to Noise Ratio (SNR) for both ends of each gateway-node link as measured by the LoRa SX1276 module. In the dataset, the prefixes gateway_ and node_ denote where the signal is coming from. The payload is the size of the data portion of the message transmitted by the node in bytes. These data are collected from each individual message that reaches our servers, where it is averaged into 5-minute aggregates.

**Table 1** LoRa installation network performance parameters
| |	Column Names|
| --- | --- |
| RSSI | node_rssi	gateway_rssi |
| SNR	| node_snr	gateway_snr |
|Payload | payload	|

As network quality indicators, we provide the ones presented in Table 2. These statistics are collected from the gateway in 15-minute intervals. The *polling_cycles* is the number of times we have polled the network for data. In each cycle all the nodes known to the gateway are requested for data at least once. Also, *messages* is the number of valid data packets received by the gateway from a node and *publications* is the number of data packets successfully transmitted from the gateway to the server. To measure erroneous conditions, we collect the *crc_errors* and the *invalid_packets*, which are the number of packets failed the CRC validation and the number of packets that were malformed (i.e., the packet did not adhere to the format utilized in our network) respectively. We also independently collect the number of additional requests sent to a node if a reply was not received in retransmissions. Although *crc_errors* and *invalid_packets* may cause additional requests to be sent to the nodes, these cases are not accounted in retransmissions. In the dataset we have omitted periods when we were not receiving data from the network due to external factors such as loss of internet connectivity.

**Table 2** LoRa network quality indicators
| | Column Names |
| --- | --- |
| Polling Cycles | polling_cycles |
| Messages | messages |
| Publications | publications |
| CRC Errors | crc_errors |
| Invalid Packets | invalid_packets |
| Re-transmissions | retransmissions |
| Packet Delivery | Ratio	packet_delivery_ratio |
| Network Delivery Ratio | network_delivery_ratio |

In our LoRa network implementation, over the physical LoRa layer, the maximum number of data packets in every 15-minute period depends on the total number of IoT devices connected and, on the specific network configurations to guarantee the European limitation for the specific frequency ToA.

We are also interested in the connectivity between the GW and every node in our LoRa network. We quantify the quality of each link by calculating the Packet Delivery Ratio (PDR) for every node. The PDR of the link between node A and the GW can be measured as the ratio between the number of DPs received by the GW from node A, and the number of DRPs sent from the GW to node A. The GW makes one DRP and a maximum of 3 DRP retransmissions per node. We can also calculate additional metrics to further our understanding of the network. The CRC Error Ratio is the ratio of DRPs from the GW which resulted in a corrupted DP being received. Re-transmission Ratio is the ratio of DRPs required to be repeated, either because of CRC errors, malformed DRP or due to not receiving a reply.

For each IEEE 802.15.4 network, we collect the statistics presented in Table 3. These statistics are calculated every 15 minutes by analyzing the number and content of the packets received by the server from the gateway. env_messages is the number of messages received by the gateway from a node in the last 15 minute period. Every node in the IEEE 802.15.4 network is scheduled to send data to the gateway every 10 seconds, resulting in a maximum of 90 packets. Because the nodes can also send additional messages when movement is detected we separate those as pir_messages. We also encountered some cases where nodes would re-transmit their data packet despite the gateway having received the packet already, possibly due to MAC-level retransmissions and lost ACK messages. We record these additional messages as pir/env_duplicates.

**Table 3** IEEE 802.15.4 installation network performance parameters
| | Column Names |
| --- | --- |
| PIR Messages | pir_messages |
| PIR Duplicates | pir_duplicates |
| PIR Delivery Ratio | pir_delivery_ratio |
| Environmental Messages | env_messages |
| Environmental Duplicates | env_duplicates |
| Network Delivery Ratio | network_delivery_ratio |

For comparing our IEEE 802.15.4 and LoRa implementations, we use the Network Delivery Ratio (NDR) indicator. The NDR is defined as the ratio between the measured DPs and the potential maximum number of packets that could be delivered by each node in the network in a time period. We calculate this value in time intervals of 15 minutes. In practice, we try to quantify the end-to-end success of packet deliveries from a Node to the GW, disregarding how the transmissions are routed through single or multi hop network topologies and MAC and LAN protocols. The dataset also contains 3D representations of the installations to depict the placement of the devices and the extracted statistics for each of the performed experiments and locations.

Our dataset contains the data of the following 5 experiments:
1.	LoRa Configuration Comparison: In this experiment, we evaluate different Spreading Factor (SF) and signal Bandwidth (BW) configurations on the real installation of School A consisting of 6 nodes. The dataset consists of two periods, the first one is referred to as Configuration 1 (SF 9, BW 125 kHz) and it covers a period of 11 days, from 2019-05-17 to 2019-05-28. The second period is named Configuration 2 (SF 7, BW 500 kHz) and extents to 17 days from 2019-05-30 to 2019-06-16.
2.	Interference between LoRa networks: In this experiment, we provide data to indicate the influence of interference in the quality of the network buildings. The dataset collected from three real LoRa installations with Configuration 2 (SF 7, BW 500 kHz), namely Schools A, B and C, with 6, 7 and 8 nodes respectively and it covers a period 31 days from 202002-29 to 2020-03-31.
3.	LoRa vs IEEE 802.15.4 in different buildings: In this experiment, we compare our IEEE 802.15.4 and LoRa networks in real world installations. The data was collected from two of our schools, one deployed with an IEEE 802.15.4 network and the other one with LoRa. In this experiment, we compare both LoRa configurations described previously in School A with the EEE 802.15.4 network in School D with both networks consisting of 6 nodes. The data from School A is from 2019-05-17 to 2019-0528 for Configuration 1 and from 2019-05-30 to 201906-07 for Configuration 2. For School D the period is from 2019-06-07 to 2019-06-11.
4.	LoRa vs IEEE 802.15.4 in the same building: In this experiment, we compare our LoRa and IEEE 802.15.4 networks in the same buildings. The data was collected from two buildings of our installation where we deployed both LoRa and IEEE 802.15.4 networks in each building for a period of 24 days from 2019-09-27 to 2019-10-21. We used the existing IEEE 802.15.4 installation and we installed and additional LoRa-based device on each sensing point. Both gateways are placed in the same location. In this way, we created two networks that works parallel at the same time inside of the same building, which means are under the same using building conditions, with the exact same space distributions. Each network in School E consists of 6 nodes and in School F of 5 nodes. The LoRa network is configured with Configuration 2 (SF 7, BW 500kHz) that provided the highest message rate per node. The maximum number of packets per device that can be sent due to ToA regulations every 15 minutes are 235 in the 5 devices network and 193 in the 6 devices network. In this case, due to internet service outages, the IEEE 802.15.4 QoS data was post-processed to be in time-sync with the LoRa QoS data, to synchronize the data-points between networks.
5.	IEEE 802.15.4 Saturation: We also used the data from School D to quantify the effect of PIR Messages on the IEEE 802.15.4 network. In this experiment, we compare the aggregated packet ratio across the network with the packet ratio from environmental sensor readings and PIR sensor readings. In this case, we analyze data for 2 days from Sunday (2019-06-8) to Monday (2019-06-10) to show the difference between a non-working and a working day.

For a full description of the dataset and associated results, please refer to the following publication (available as open access at https://ieeexplore.ieee.org/document/9181518/):

*L. P. Fraile, S. Tsampas, G. Mylonas and D. Amaxilatis, "A Comparative Study of LoRa and IEEE 802.15.4-Based IoT Deployments Inside School Buildings," in IEEE Access, vol. 8, pp. 160957-160981, 2020, doi: 10.1109/ACCESS.2020.3020685.*
