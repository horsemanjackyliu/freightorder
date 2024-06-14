
## Segtion 1: Configure Event Mesh in BTP subaccount.

### Step 1:Create Event Mesh Subscription
Since I have created event mesh subscription, so I can not create again. But if you have not created event mesh subscription, you can create a new event mesh subscription.
![Alt text](./image-1.png)
### Step 2: Assign Event Mesh role collections to your user account.
![Alt text](./image.png)

![Alt text](./image-2.png)
### Step 3: Create Event Mesh instance.
![Alt text](./image-3.png)
![Alt text](./image-6.png)
you can use json as the following file:

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

### Step 4: Create Event Mesh instance service key
![Alt text](./image-8.png)
![Alt text](./image-9.png)
![Alt text](./image-10.png)
![Alt text](./image-11.png)

We will use the copied service key information to create communication arrangements in S/4 Hana Cloud in following Segment.

