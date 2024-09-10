
## 1. Create an event consumption model in Eclipse

Now we’re going to create an event consumption model using the .json file that you downloaded in last Segtion 2 step 3.

- 1. In ADT, open your SAP BTP, ABAP environment system based on guide.

- 2.	Create a new ABAP Package if you don’t have one. Make sure that your package name starts with the letter ‘Z’.  For example:

- Name: ZFREIGHT_ORDER_EVENT_PACKAGE
- Description: event consumption for freight order

![Alt text](img/image.png)
![Alt text](img/image-1.png)
![Alt text](img/image-2.png)
![Alt text](img/image-3.png)
![Alt text](img/image-4.png)

- 3.  Right-click your package and choose New > Other ABAP Repository Object > Business Services > Event Consumption Model, and click Next to launch the creation wizard.

![Alt text](img/image-5.png)
![Alt text](img/image-6.png)

- 4. Fill in the fields and upload the .json file you saved previously into the new event consumption ADT wizard. This will then automatically generate everything you need for this event consumption model, such as the event handler custom code, authorization defaults values, and inbound service.

- Name: Is created with the Prefix and Identifier 
- Description: event consumption model for freight order
- Namespace/ Prefix/ Identifier : Z and FREIGHTORDERCREATEDEVENT
- Event specification File: the .json file that you created
- 5. click Next.

![Alt text](img/image-7.png)
![Alt text](img/image-8.png)
![Alt text](img/image-9.png)
![Alt text](img/image-10.png)
![Alt text](img/image-11.png)

### 2. Create a communication scenario for the event consumption model

Now we will create a communication scenario for the Event Consumption Model that we just generated, and we will show you how to use it later for event consumption. This communication scenario will be used in the productive system to create a communication arrangement with the generated event consumption model. You will need an sap_com_0092 communication arrangement in the productive system for the connection from the productive system to the Event Mesh instance in the SAP BTP, ABAP environment system. These two scenarios combined will then enable the event consumption with the generated Event Consumption Model.

Right-click your package and choose New > Other ABAP Repository Object > Cloud Communication Management > Communication Scenario. Click **Next**.

![Alt text](img/image-12.png)
![Alt text](img/image-13.png)

Enter the following name and description:

- Name:Z_EVENT_SCENARIO_FREIGHT_ORDER
- Description: event consupmtion scenario for freight order
Click **Next**.

![Alt text](img/image-14.png)
![Alt text](img/image-15.png)
![Alt text](img/image-16.png)
![Alt text](img/image-17.png)
![Alt text](img/image-18.png)

Press Ctrl+S to save the communication scenario.

![Alt text](img/image-19.png)

### 3. Create database table for handler method
Right-click your package and choose New > Other ABAP Repository Object > Database Table and click **Next** .

![Alt text](img/image-20.png)
![Alt text](img/image-21.png)

Enter the following name and description:

- Name: ZT_FREIGHTORDER
- Description: Database table for freight order

![Alt text](img/image-22.png)
![Alt text](img/image-23.png)
![Alt text](img/image-24.png)

Define the table with the following code

``` 
 @EndUserText.label : 'Database table for freight order'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table ztb_freightorder {

  key client                  : abap.clnt not null;
  key transportationorderuuid : sysuuid_x16 not null;
  transportationorder         : abap.char(20) not null;
  carrier_uuid                : sysuuid_x16;
  carrier                     : abap.char(10);
  shipper_uuid                : sysuuid_x16;
  shipper                     : abap.char(10);
  shipper_address_id          : abap.char(40);
  consignee_uuid              : sysuuid_x16;
  consignee                   : abap.char(10);
  consignee_address_id        : abap.char(40);
  transp_purg_org             : abap.char(8);
  transp_purg_group           : abap.char(8);
  purg_org_company_code       : abap.char(4);
  carrier_account_number      : abap.char(14);
  transp_ord_invoicing_carri  : abap.char(2);
  created_by_user             : abap.char(12);
  creation_date_time          : xsddatetime_iso;
  transportationmode          : abap.char(2);
  transportationordertype     : abap.char(4);
  transportationshippingtype  : abap.char(3);
  transppurgorgextid          : abap.char(20);
  transportation_mode_catego  : abap.char(1);
  transp_ord_execution_is_bl  : abap_boolean;
  transportation_order_exec   : abap.char(2);
  repositoryid                : sysuuid_x16;
  fileobjectid                : abap.char(43);

}
```

Save and activate the table.

### 4. Define handle event methods


Navigate to Business Service > Event Consumption models > ZFREIGHTORDERCREATEDEVENT           0001 > Classes > ZCL_FREIGHTORDERCREATEDEVENT which is generated (or you can navigate to the Source Code Library > ZCL_FREIGHTORDERCREATEDEVENT). Here you can see the create Method.

![Alt text](img/image-25.png)

Remove comment from these lines:

```  
DATA ls_business_data TYPE STRUCTURE FOR HIERARCHY Z_BusinessPartner_Created_v1.      
  ls_business_data = io_event->get_business_data( ).
```
You need to adjust the code as the following in your method. pLease don't just copy and paste.

```
   DATA ls_business_data TYPE STRUCTURE FOR HIERARCHY Z_FreightOrder_Created_v1_568B. //this line maybe deifferent .
   data wa type ztb_freightorder .
   ls_business_data = io_event->get_business_data( ).
   wa-carrier = ls_business_data-Carrier.
   wa-transportationmode = ls_business_data-TransportationMode.
   wa-transportationorder = ls_business_data-TransportationOrder .
   wa-transportationordertype = ls_business_data-TransportationOrderType .
   wa-transportationorderuuid = ls_business_data-TransportationOrderUUID .
   wa-transportationshippingtype = ls_business_data-TransportationShippingType .
   INSERT ztb_freightorder from @wa .
```
![Alt text](img/image-26.png)

**Please save and activate the class**

### 5. Acctivate the  Define handle event methods

Please right click the communication scenario and select activate to activate the communication scenario.







