

## Segtion 2: Configue event in S/4 Hana Cloud.

### Prerequisites:
    1, You have been assigned to role BR_CONF_EXPERT_BUS_NET_INT in S/4HANA CLOUD.

### Step 1: Create communication arrangement in S/4 Hana Cloud with communication scenario sap_com_0092 .

![Alt text](./img/image-12.png)
![Alt text](./img/image-14.png)
![Alt text](./img/image-13.png)
![Alt text](./img/image-15.png)
![Alt text](./img/image-16.png)

### Step 2: Configure event channel binding.

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

### Step 3: Save the event meta data to json file, we will use it to create event consumption in Eclipse ADT .

Go back to your channel and scroll down to Event Metadata and save the metadata as a .json file for further usage. This specification file contains all metadata of the event.

![Alt text](./img/image-28.png)

Just like [Metadata](./img/eventmeta.json)  


