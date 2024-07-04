
## 1. Create IAM App

In Eclipse right-click your package ZFREIGHT_ORDER_EVENT_PACKAGE and select New > Other Repository Object.

![alt text](image.png)
![alt text](image-1.png)

Create a new IAM App:

- Name: ZFREIGHTORDER_IAM_APP
- Description: FreightOrder Management IAM APP

![alt text](image-2.png)
![alt text](image-3.png)

Click Finish.

![alt text](image-44.png)
![alt text](image-45.png)

Maintain Item Id

![alt text](image-4.png)
![alt text](image-5.png)


Select following:

- Service Type: OData V4
- Service Name: ZFREIGHTORDER_SRV_BIND

![alt text](image-6.png)

Click on Publish Locally

![alt text](image-7.png)

## 2. Create business catalog

![alt text](image.png)
![alt text](image-8.png)

![alt text](image-9.png)

Create a new business catalog:
- Name: ZFREIGHTORDER_BC2
- Description: Freight Order Management Business Catalog

![alt text](image-10.png)

![alt text](image-11.png)
![alt text](image-12.png)


Fill IAM App field with **ZFREIGHTORDER_IAM_APP_EXT** and click next.


![alt text](image-13.png)
![alt text](image-14.png)


## 3. Create business role
Right-click on your ABAP system in the project explorer of ADT and select Properties. Click ABAP Development and copy the system URL.

![alt text](image-15.png)
![alt text](image-16.png)

Log in to your ABAP system as an administrator.

![alt text](image-17.png)

Select Maintain Business Roles.
![alt text](image-18.png)

Click New.

![alt text](image-19.png)


Create new business role:

- Business Role ID: BR_Z_FREIGHTORDER
- Business Role Description: Freight Order Business Role
![alt text](image-20.png)

![alt text](image-21.png)
![alt text](image-22.png)


Assign you user account to the business role.

![alt text](image-23.png)
![alt text](image-24.png)

![alt text](image-25.png)

## 4: Create transport request or use default transport request

Log in to your system and select the Export Customizing Transports tile.

![alt text](image-26.png)

Click Go.

![alt text](image-27.png)

Now you can see all transport request.

![alt text](image-28.png)

Create new transport request:

- Description: Transport_Request_FreightOrder
- Type: Customizing Request
![alt text](image-29.png)
Click Create.


![alt text](image-31.png)


If the Transport Category is not default, click **change Category** to change it to default .

![alt text](image-30.png)

## 5. Create Launchpad Space.

![alt text](image-34.png)


Select the Manage Launchpad Spaces tile.


![alt text](image-32.png)
![alt text](image-33.png)

Create new space and page:

- Space ID: ZFREIGHTORDER_APP
- Space description: Space for Freight Order
- Space title: Freight Order Management Application
- Page ID: ZFREIGHTORDER_APP
- Page description: Page for Freight Order Management
- Page title: Freight Order Management
- Transport: Transport_Request_FreightOrder

![alt text](image-35.png)
![alt text](image-36.png)

Go back to the SAP Fiori homepage and select the Maintain Business Roles tile.

![alt text](image-37.png)
![alt text](image-38.png)
![alt text](image-39.png)
![alt text](image-40.png)
![alt text](image-41.png)

Open the Manage Launchpad Pages tile.

![alt text](image-42.png)
![alt text](image-43.png)
![alt text](image-47.png)
![alt text](image-48.png)
















