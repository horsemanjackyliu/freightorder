

## Step 1: Create Service definition in Eclipse Adt.
![Alt text](image.png)
Name:ZFREIGHTORDER_SERVICE
Description: Service definition for  Freight Order
![Alt text](image-1.png)
![Alt text](image-2.png)
![Alt text](image-3.png)
Adjust the code as the following:
```
@EndUserText.label: 'Service definition for  Freight Order'
define service ZFREIGHTORDER_SERVICE {
  expose ZC_FREIGHTORDER;
  expose ZC_FORDER_ITEMS;
}
```
![Alt text](image-4.png)

Push ***Command + S*** in Mac and activate it .

## Step 2: Create Service binding for ZFREIGHTORDER_SERVICE in Eclipse Adt.
![Alt text](image-5.png)
Name:ZFREIGHTORDER_SRV_BIND
Description: Service Binding for ZFREIGHTORDER_SERVICE
![Alt text](image-11.png)
![Alt text](image-7.png)
Activate the service binding .
![Alt text](image-8.png)
Publish the service binding.
![Alt text](image-9.png)
![Alt text](image-10.png)





