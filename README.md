# Streamlining IT Procurement: Automating Standard Laptop Orders with Flow Designer

A ServiceNow project that automates the fulfillment of **Standard Laptop** requests from the Service Catalog. When an employee orders a Standard Laptop, the request goes for approval. Once it is approved, a flow updates the request and creates a Catalog Task assigned to the **Hardware** group, with no manual task creation.

- **Platform:** ServiceNow Personal Developer Instance (Brazil release)
- **Tools used:** Workflow Studio / Flow Designer, Service Catalog
- **Application scope:** Global

---

## Problem Statement

IT teams often create fulfillment tasks for laptop requests by hand after a manager approves them. This is slow, easy to forget, and tasks can end up with the wrong team.

## Solution

A Flow Designer flow named **Standard laptop task** is attached to the **Standard Laptop** catalog item. The flow:

1. Starts when the Standard Laptop item is requested (Service Catalog trigger)
2. Sends the Requested Item for approval
3. If the request is **approved**:
   - Updates the Requested Item's short description
   - Creates a **Catalog Task** assigned to the **Hardware** assignment group

**Benefits:** faster processing, no manual task creation, and approved requests always reach the right team.

---

## Flow Structure

```
TRIGGER:  Service Catalog
  1. Ask For Approval          (Record: Requested Item, Approver: System Administrator)
  2. If  Approval State is Approved
        then
        3. Update Requested Item Record   (Short description = "Standard Laptop request approved")
        4. Create Catalog Task            (Short description = "Standard Laptop – Hardware fulfillment",
                                           Assignment group = Hardware)
```

---

## Milestone 1: Flow

**Activity 1:** Create a flow that creates a catalog task, updates the short description after approval, and assigns the task to the Hardware group.

1. Open ServiceNow → **All** → search **Flow Designer** (opens **Workflow Studio**).
2. Click **New → Flow**.
3. Flow properties:
   - **Flow name:** `Standard laptop task`
   - **Application:** Global
   - **Run as:** System user
4. Click **Build flow**.
5. **Trigger:** Add a trigger → **Service Catalog** → Done.
6. **Action 1: Ask For Approval**
   - Record: `Trigger → Requested Item Record`
   - Table: Requested Item [sc_req_item]
   - Rule: Approve when **Anyone approves**, approver **System Administrator**
7. **Flow Logic: If**
   - Condition label: `If approved`
   - Condition: `1 - Ask For Approval → Approval State` **is** `Approved`
8. **Action 3: Update Record** (inside the *then* branch)
   - Record: `Trigger → Requested Item Record`
   - Field: Short description = `Standard Laptop request approved`
9. **Action 4: Create Catalog Task** (inside the *then* branch)
   - Requested Item: `Trigger → Requested Item Record`
   - Short description = `Standard Laptop – Hardware fulfillment`
   - Assignment group = **Hardware**
10. **Save** and **Activate** the flow (status: Active / Published).

![Flow in Workflow Studio](screenshots/01-flow.png)

---

## Milestone 2: Flow Assignment

**Activity 1:** Assign the flow to the Standard Laptop service catalog item.

1. **All** → search **Maintain Items** → open it.
2. Search the Name field for **Standard Laptop** and open the record.
3. Open the **Process Engine** tab.
4. Remove other automations (Workflow and Execution Plan stay empty).
5. Set **Flow** = `Standard laptop task`.
6. Click **Update** to save.

![Process Engine tab](screenshots/02-process-engine.png)

---

## Milestone 3: Service Catalog

**Activity 1:** Place an order through the Service Catalog and get approval.

1. **All** → **Service Catalog** → **Hardware** → **Standard Laptop**.
2. Click **Order Now**. A Request (REQ) and Requested Item (RITM) are created.
3. Open **My Approvals** (or the RITM's Approvers list) and **Approve** the request.
4. Open the RITM and verify:
   - The short description changed to *Standard Laptop request approved*
   - The **Catalog Tasks** related list contains a new task with Assignment group **Hardware**

![Order placed](screenshots/03-order.png)
![Approval](screenshots/04-approval.png)
![Catalog task assigned to Hardware](screenshots/05-catalog-task.png)

---

## Testing / Results

| Step | Expected result | Status |
|---|---|---|
| Order Standard Laptop | REQ and RITM created, approval requested | ✅ |
| Approve request | Flow continues into the *If approved* branch | ✅ |
| RITM short description | Updated to "Standard Laptop request approved" | ✅ |
| Catalog Task | Created and assigned to **Hardware** | ✅ |

---

## Conclusion

The project shows how Flow Designer can automate an everyday IT procurement process. Linking the **Standard laptop task** flow to the **Standard Laptop** catalog item means every approved laptop request automatically gets a fulfillment task routed to the Hardware team. This cuts manual effort and turnaround time and gives every order a consistent, trackable process.

---

## Team

| Name | Role |
|---|---|
| Santhiya B | Team Lead |
| Nikhila S | Member |
| Salatenancyea S | Member |
| Yogadarsini R | Member |
| Mohamed Yusuf S | Member |

## Demo

▶️ Demo video: https://youtu.be/yAT5ZiicXCc
