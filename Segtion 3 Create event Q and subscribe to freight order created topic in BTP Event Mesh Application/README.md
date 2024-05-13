---
noteId: "e0ad05e0932611eeb554f34f44c206ba"
tags: []

---

## Segtion 3: Create event Q and subscribe to freight order created topic in BTP Event Mesh Application.
### Step 1: Create event Q.
![Alt text](./img/image-26.png)
![Alt text](./img/image-27.png)
![Alt text](./img/image-28.png)
![Alt text](./img/image-29.png)
### Step 2 : Queue subscribtion.
![Alt text](./img/image-30.png)
    Add topic:
    com/sap/spaworkshop/ce/sap/s4/beh/FreightOrder/Created/v1
![Alt text](./img/image-31.png)

    Add another topic:
    com/sap/spaworkshop/ce/sap/s4/beh/FreightOrder/Created/*
![Alt text](./img/image-32.png)
