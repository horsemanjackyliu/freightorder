---
noteId: "5501cda0d76e11eeb58c775b10b8c912"
tags: []

---

## Prerequisites:

-  You have finished the [initial setting up for SAP Business Application Studio](https://help.sap.com/docs/bas/sap-business-application-studio/getting-started).
-  You have [created Dev Space with type SAP Fiori in SAP Business Application Studio](https://help.sap.com/docs/bas/sap-business-application-studio/working-in-dev-space-manager)

## Step 1:  Create Fiori Application in SAP Business Application Studio.

![Alt text](image-13.png)
![Alt text](image-14.png)
![Alt text](image-15.png)
![Alt text](image-16.png)
![Alt text](image-17.png)
![Alt text](image-18.png)
![Alt text](image-19.png)
![Alt text](image-2.png)
![Alt text](image-3.png)
![Alt text](image-4.png)
![Alt text](image-5.png)
![Alt text](image-6.png)
![Alt text](image-7.png)
![Alt text](image-8.png)
![Alt text](image-9.png)


## Step 2, Add custom controller to Object Page view.
![alt text](image-20.png)
![alt text](image-21.png)
![alt text](image-22.png)
![alt text](image-23.png)

Adjust FoController as the following code .

```javascript
sap.ui.define(['sap/ui/core/mvc/ControllerExtension','sap/ui/model/json/JSONModel','sap/m/MessageToast','sap/base/security/URLWhitelist','sap/ui/core/message/Message', 'sap/ui/core/message/MessageType'], function (ControllerExtension,JSONModel,MessageToast,URLWhitelist,Message, MessageType) {
	'use strict';

	return ControllerExtension.extend('freightordermgt.ext.controller.FoController', {
		// this section allows to extend lifecycle hooks or hooks provided by Fiori elements
		pDialog : null,
		pdf_content: null,
		uploadfiles: function() {

			if (!this.pDialog) { 
				this.base.getExtensionAPI()
				.loadFragment({
					name: "freightordermgt.ext.fragment.fileselector",
					controller: this
				}).then((oDialog)=>{
					this.pDialog = oDialog;
					oDialog.open();
				 var oFileUploader = this.pDialog.getContent('uploadSet')[0];
				 if(oFileUploader){
					oFileUploader.removeAllItems();
				 }				
				}).catch(error=>{alert(error.message)})
			}else{
				var oFileUploader =  this.pDialog.getContent('uploadSet')[0];
				if(oFileUploader){
				   oFileUploader.removeAllItems();
				}
				this.pDialog.open();

			}
		
		},
		showPdf: function(oEvent){
			let showPdf = null;
			var oModel = this.base.getExtensionAPI().getModel();
		   //  var sPath = oModel.getBindings()[0].getContext().sPath;
		   var fileRep =  oModel.getBindings().filter(bn=>{ return bn.sPath=='Repositoryid' })[0].vValue
		   var fileObj =  oModel.getBindings().filter(bn=>{ return bn.sPath=='Fileobjectid' })[0].vValue

			const sFunctionname = "com.sap.gateway.srvd.zfreightorder_service.v0001.downloadfile";
			var sPath = this.getView().getBindingContext().getPath();
			const oFunction = oModel.bindContext(`${sPath}/${sFunctionname}(...)`);
	
		   oFunction.execute().then(function(){
			const oContext = oFunction.getBoundContext();
			var filename = oContext.getProperty('FILENAME');
			var stream = oContext.getProperty('STREAM');
			 stream = stream.replaceAll('_','/').replaceAll('-','+');
			 const deccont = atob(stream);			
			const byteNumbers = new Array(deccont.length);
			for (let i = 0; i < deccont.length; i++) {
				byteNumbers[i] = deccont.charCodeAt(i);
			}
			const byteArray = new Uint8Array(byteNumbers);
			var blob = new Blob([byteArray],{type: "application/pdf"});
			const pdfurl = URL.createObjectURL(blob);
			let oPdfmodel = new JSONModel({
				Source: pdfurl,
				Title: filename,
				Height: "1000px"
			});
			URLWhitelist.add("blob");
			this.getView().setModel(oPdfmodel,"pdf");
		}.bind(this)).catch(err=>{
				console.log('function err happened');
				console.log(err);
			});

			if (fileRep){
				showPdf = true ;
			}else{
				showPdf = false;
			};
 
			let oPdfview = new JSONModel({
				Viewshow: showPdf
			});
			 this.getView().setModel(oPdfview,"pdfview");	

		},
		onUploadSet:  function(oEvent) {
			var oFileUploader = this.pDialog.getContent('uploadSet')[0];
			var oFile = oFileUploader.getItems()[0].getFileObject();
			var sFileName = oFile.name;
			var iFileLen = oFile.size;
			var sFileType = oFile.type;
			// console.log(this.pdf_content);
			const oExtensionAPI = this.base.getExtensionAPI();
			// var view1 = this.getView();
			var oModel = oExtensionAPI.getModel();
			const sFunctionname = "com.sap.gateway.srvd.zfreightorder_service.v0001.uploadfile";
			var sPath = this.getView().getBindingContext().getPath();

			// let sRaw = String.raw`${this.pdf_content}`;
			
			let sMimtype = sFileName.split(".")[sFileName.split(".").length-1];

			

			const oFunction = oModel.bindContext(`${sPath}/${sFunctionname}(...)`);
			oFunction.setParameter("filename",sFileName);
			oFunction.setParameter("mime_type",sMimtype);
			var content = btoa(this.pdf_content);

			oFunction.setParameter("stream",content); 
		   oFunction.execute().then(result=>{
			MessageToast.show("File Uploaded into BTP CMIS!");
			// console.log(result);
			// this.pDialog.close();
		}).catch(err=>{
			console.log(err);
		   }).finally(this.pDialog.close());
			
	
			  /* TODO:Call to OData */
		  },
		  onCloseDialog: function (oEvent) {        
			// alert("close clicked");   
			  this.pDialog.close();
		  },
		  onItemRemoved:function (oEvent) {
            console.log("File Remove/delete Event Fired!!!")  
            /* TODO: Clear the already read excel file data */          
        },
        onUploadSetComplete: function (oEvent) {

            var oFileUploader = this.pDialog.getContent('uploadSet')[0];
            var oFile = oFileUploader.getItems()[0].getFileObject();
            var reader = new FileReader();

            reader.onload = (e)=>{
                this.pdf_content = e.currentTarget.result;
                // console.log(this.pdf_content);
            }
			reader.readAsBinaryString(oFile);
            MessageToast.show("Upload Successful");

            /* TODO: Read excel file data*/
        },
        onItemRemoved:function (oEvent) {
            console.log("File Remove/delete Event Fired!!!")  
            /* TODO: Clear the already read excel file data */          
         },

		override: {
			/**
             * Called when a controller is instantiated and its View controls (if available) are already created.
             * Can be used to modify the View before it is displayed, to bind event handlers and do other one-time initialization.
             * @memberOf freightordermgt.ext.controller.FoController
             */
			onInit: function () {

			},

			routing: {
				onAfterBinding: function (oBindingContext, mParameters) {
					var extensionAPI = this.base.getExtensionAPI(),
						messages = [
							new Message({
								message: "onAfterBinding: Context bound",
								type: MessageType.Information
							})
						];
					extensionAPI.showMessages(messages);
				}

			},

			onAfterRendering: function(){
				let oPdfview = new JSONModel({
					Viewshow: false
				});

				this.getView().setModel(oPdfview,"pdfview");


			}
		}
	});
});

```

## Step 3, Add custom action in Object Page view Header for Uploading File.
![alt text](image-20.png)
![alt text](image-33.png)
![alt text](image-34.png)
![alt text](image-35.png)


## Step 4, Add custom section for freight order items in Object Page view.

![alt text](image-24.png)
![alt text](image-26.png)
![alt text](image-25.png)
![alt text](image-27.png)

Adjust the fragement code as the following:
```xml
<core:FragmentDefinition xmlns:core="sap.ui.core" xmlns="sap.m" xmlns:macros="sap.fe.macros">

	<macros:Table id="itemtable" metaPath="_ITEMS/@com.sap.vocabularies.UI.v1.PresentationVariant" header="Items" personalization="Column" readOnly="{ui>/isEditable}"
	
	></macros:Table>
</core:FragmentDefinition>

```

## Step 5, Add fragment for uploading file under ext/fragment
![alt text](image-28.png)
![alt text](image-29.png)
Adjust the fragement code as the following:
```XML
<core:FragmentDefinition  xmlns="sap.m" xmlns:l="sap.ui.layout"  xmlns:core="sap.ui.core" xmlns:u="sap.ui.unified" xmlns:upload="sap.m.upload" >
	<Dialog id="uploadDialogSet" title="File Upload">
        <content>
            <upload:UploadSet uploadEnabled="true" id="uploadSet" items="{path: '/', templateShareable: false}" fileTypes="png,pdf,txt" maxFileNameLength="200" beforeUploadStarts=".extension.freightordermgt.ext.controller.FoController.onBeforeUploadStart" uploadCompleted=".extension.freightordermgt.ext.controller.FoController.onUploadSetComplete" afterItemRemoved=".extension.freightordermgt.ext.controller.FoController.onItemRemoved"
            terminationEnabled="true">
                <upload:UploadSetItem id="_IDGenUploadSetItem1" visibleRemove="true" visibleEdit="false" fileName="{name}" url="/upload">
                    <upload:attributes>
                        <ObjectAttribute id="_IDGenObjectAttribute1" title="Uploaded by" text="{user}" active="false"/>
                    </upload:attributes>
                </upload:UploadSetItem>
            </upload:UploadSet>
        </content>
        <buttons>        
            <Button id="_IDGenButton1" text="Upload" press=".extension.freightordermgt.ext.controller.FoController.onUploadSet" icon="sap-icon://upload-to-cloud" type="Emphasized"/>
            <Button id="cancel" press=".extension.freightordermgt.ext.controller.FoController.onCloseDialog" text="Cancel" icon="sap-icon://cancel"/>
        </buttons>
	</Dialog>
</core:FragmentDefinition>

```
## Step 6, Add custom section for Attachment View in Object Page view.
![alt text](image-30.png)
![alt text](image-31.png)
![alt text](image-32.png)

Adjust the fragement code as the following:
```xml
<core:FragmentDefinition xmlns:core="sap.ui.core" xmlns="sap.m" xmlns:macros="sap.fe.macros" validationSuccess=".extension.freightordermgt.ext.controller.FoController.onFragVali"   >
	<ScrollContainer id="_IDGenScrollContainer1"
		height="100%"
		width="100%"
		horizontal="true"
		vertical="true" visible="{pdfview>/Viewshow}">
		<FlexBox id="_IDGenFlexBox1" direction="Column" renderType="Div" class="sapUiSmallMargin">
			<PDFViewer id="_IDGenPDFViewer1" source="{pdf>/Source}" isTrustedSource="true" title="{pdf>/Title}" height="{pdf>/Height}" >
				<layoutData>
					<FlexItemData id="_IDGenFlexItemData1" growFactor="1" />
				</layoutData>
			</PDFViewer>
		</FlexBox>
	</ScrollContainer>
</core:FragmentDefinition>

```

## Step 6, Add custom action for Attachment View in Object Page view Header.
![alt text](image-36.png)
![alt text](image-37.png)
![alt text](image-38.png)

## Step 7, Application preview and testing.
![alt text](image-39.png)
![alt text](image-40.png)
![alt text](image-41.png)
![alt text](image-42.png)
![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-45.png)



## Step 8: Deploy the Fiori application to BTP ABAP Environment.
![alt text](image-46.png)
![alt text](image-47.png)
![alt text](image-48.png)
![alt text](image-49.png)
![alt text](image-50.png)

Check the fiori application in Eclipse ADT .
![alt text](image-51.png)



