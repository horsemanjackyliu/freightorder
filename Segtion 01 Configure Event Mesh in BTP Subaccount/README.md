### 1. Create an Event Mesh subscription
If you don’t have an Event Mesh subscription, create a new one. 
![Alt text](./image-1.png)
### 2. Assign Event Mesh role collections to your user account.
![Alt text](./image.png)

![Alt text](./image-2.png)
### 3. Create an Event Mesh instance.
![Alt text](./image-3.png)
![Alt text](./image-6.png)
You can use the following JSON file:

```json
{
    "emname": "spaworkshop",
    "namespace": "com/sap/spaworkshop",
    "version": "1.1.0",
    "options": {
      "management": true,
      "messagingrest": true,
      "messaging": true
    },
    "rules": {
      "queueRules": {
          "publishFilter": [
              "${namespace}/*"
          ],
          "subscribeFilter": [
              "${namespace}/*"
          ]
      },
      "topicRules": {
          "publishFilter": [
              "${namespace}/*"
          ],
          "subscribeFilter": [
              "${namespace}/*"
          ]
      }
    }
  }
  ```
  ![Alt text](./image-5.png)
![Alt text](./image-7.png)

### 4. Create an Event Mesh instance service key
![Alt text](./image-8.png)
![Alt text](./image-9.png)
![Alt text](./image-10.png)
![Alt text](./image-11.png)

We will use the copied service key to create communication arrangements in SAP S/4Hana Cloud.

