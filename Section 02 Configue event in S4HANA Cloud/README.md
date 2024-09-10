### Prerequisites:
    You have been assigned to the BR_CONF_EXPERT_BUS_NET_INT role in SAP S/4HANA CLOUD.

### 1. Create a communication arrangement in SAP S/4HANA Cloud using the sap_com_0092 communication scenario.

![Alt text](./img/image-12.png)
![Alt text](./img/image-14.png)
![Alt text](./img/image-13.png)
![Alt text](./img/image-15.png)
![Alt text](./img/image-16.png)

### 2. Configure event channel binding.

![Alt text](./img/image-17.png)
![Alt text](./img/image-18.png)
![Alt text](./img/image-19.png)
![Alt text](./img/image-20.png)
![Alt text](./img/image-22.png)
![Alt text](./img/image-23.png)
![Alt text](./img/image-24.png)
![Alt text](./img/image-25.png)

    Add another topic:
    sap/s4/beh/FreightOrder/Created/v1
    
![Alt text](./img/image-26.png)
![Alt text](./img/image-27.png)

com/sap/spaworkshop/ce/sap/s4/beh/FreightOrder/Created/v1

### 3. Save the event metadata to a JSON file 

We will use the JSON file to create an event consumption in Eclipse ADT.

Go back to your channel, scroll down to Event Metadata, and save the metadata as a .json file for later. This specification file contains all of the metadata of the event.

![Alt text](./img/image-28.png)

Just like [Metadata](./img/eventmeta.json)  


