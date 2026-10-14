# Exercise 4 - Migrate a Java Mapping Scenario

In this exercise, you will migrate an integration scenario that uses a Java Mapping from SAP Process Orchestration to SAP Integration Suite using the Migration Tooling.

## Source ICO Overview

The following screenshots show the configuration of the source Integrated Configuration Object (ICO) and its related ESR artifacts in SAP Process Orchestration before migration.

**Operation Mapping** — The operation mapping **omSendAttachment_to_ConvertAttachment** in namespace `http://demo.sap.com/soap/java` (Software Component Version **EIP_SWCV, 1.0 of demo.sap.com**) maps source operation **siSendAttachment** to target operation **siCreateResponse**. The mapping program uses a **Java Class** named **readAttachment/ReadAttachment** from namespace `http://demo.sap.com/soap/java` with the **Read Attachments** flag enabled.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_01.png)

**Imported Archive** — The imported archive **iaReadAttachment** in the same Software Component Version contains the Java implementation. The archive includes **ReadAttachment.class** and **ReadAttachment.java** (both at path `readAttachment/`) as well as `classpath` and `project` entries. JDK Compliance is set to **JDK 1.5 (Default)**.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_02.png)

**Java Source Code** — The **ReadAttachment.java** class extends `AbstractTransformation` and implements the `transform` method. It reads the input attachment using `inputAttachments.getAllContentIds(false)`, extracts the attachment content as a String, and wraps it in an XML response: `<ns1:mtResponse xmlns:ns1="urn:sap.com:javaMapping"><Text>...</Text></ns1:mtResponse>`. The result is written to the output stream via `outputStream.write(outXML.getBytes())`.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_03.png)

**Inbound Processing** — The ICO **Inbound Processing** tab shows the sender details: Communication Component **BC_Sender**, Interface **siSendAttachment**, Namespace `http://demo.sap.com/soap/java`. The sender channel **CC_SOAP_SND_ATTACHMENT** uses a **SOAP** adapter on the Central Adapter Engine (SAP BASIS 7.50). Schema Validation is set to **No Validation**.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_04.png)

**Receiver Determination** — The **Receiver** tab defines a single **Local Rule** with no XPath condition, routing unconditionally to receiver **BC_Receiver**. If no receiver is found, the ICO proceeds with **Error Message**.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_05.png)

**Receiver Interfaces** — The **Receiver Interfaces** tab shows receiver **BC_Receiver** (Communication Component) with operation mapping **omSendAttachment_to_ConvertAttachment** mapped to target interface **siCreateResponse** in namespace `http://demo.sap.com/soap/java` (EIP_SWCV, 1.0 of demo.sap.com), Multiplicity **1**.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_06.png)

**Outbound Processing** — The **Outbound Processing** tab shows the configuration for interface **siCreateResponse** (`http://demo.sap.com/soap/java | EIP_SWCV, 1.0 of demo.sap.com`). The receiver channel **CC_SOAP_RCV_LOOPBACK** uses a **SOAP** adapter on the Central Adapter Engine (SAP BASIS 7.50). Schema Validation is set to **No Validation**.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_07.png)

**Receiver Channel** — The communication channel **CC_SOAP_RCV_LOOPBACK** is configured on component **BC_Receiver** as a SOAP **Receiver** (HTTP transport, SOAP 1.1) on the Central Adapter Engine. The Target URL is set to `https://dummy.de` with no authentication, no certificate, and no proxy configured.

<br>![](/exercises/ex4/images/SourceICO/04_00_SourceICO_08.png)

---

## Exercise 4.1 - Prepare the Script Collection

After completing these steps you will have created a Script Collection artifact in your integration package that will serve as the target for the AI-generated Groovy script during the Java Mapping migration.

1. Navigate to **Design > Integrations and APIs** and open your **User XX** integration package. The package currently contains 7 artifacts from the previous exercises. Select **Edit** to switch to edit mode.

<br>![](/exercises/ex4/images/04_01_Prepare_01.png)

2. The package is now in edit mode (**Mode: Editable**). Select **Edit** to confirm, then open the **Add** dropdown in the toolbar.

<br>![](/exercises/ex4/images/04_01_Prepare_02.png)

3. The **Add** dropdown lists the available artifact types. Select **Script Collection** from the list.

<br>![](/exercises/ex4/images/04_01_Prepare_03.png)

4. The **Add Script Collection** dialog opens. Leave **Create** selected. Enter **SC_JAVAMapping_XX** as both **Name** and **ID** (replace **XX** with your user number). **Runtime Profile** is set to **Cloud Integration**. Select **Add** to create the artifact.

<br>![](/exercises/ex4/images/04_01_Prepare_04.png)

---

## Exercise 4.2 - Run the Migration Wizard

After completing these steps you will have migrated the Java Mapping ICO using the Standard approach and a new integration flow **CS_JAVA_FL_XX** will have been created in your integration package, with the Java mapping converted to a Groovy script using AI.

1. Back in the **User XX** package on the **Artifacts** tab, select **Migrate** from the toolbar to open the Migration Wizard.

<br>![](/exercises/ex4/images/04_02_Migrate_01.png)

2. The **Migrate Process Orchestration Artifacts** dialog opens on step **1. Process Orchestration System**. Open the **Name** dropdown and select **POP**.

<br>![](/exercises/ex4/images/04_02_Migrate_02.png)

3. The system details are populated automatically — **Version: 7.50**, **Integration Directory** address `http://po4pimasoncal:50000` with Location ID **pimas34**, and **ES Repository** with the same address and Location ID. Select **Next Step**.

<br>![](/exercises/ex4/images/04_02_Migrate_03.png)

4. On step **2. Process Orchestration Artifacts**, the **Object Type** is set to **Integrated Configuration**. Click the browse icon next to the **Name** field to open the scenario selection dialog.

<br>![](/exercises/ex4/images/04_02_Migrate_04.png)

5. The **Select: Scenario** dialog opens listing the available ICOs. Locate and select **|BC_Sender|siSendAttachment|http://demo.sap.com/soap/java||** from the list. The selected ICO name is now shown in the **Name** field. Select **Next Step**.

<br>![](/exercises/ex4/images/04_02_Migrate_05.png)

6. On step **3. Pattern**, the wizard has auto-detected the pattern as **Point-to-Point Synchronous** — a synchronous point-to-point integration pattern between sender and receiver systems. Under **Select Migration Approach**, only **Standard** is available for this pattern. Select **Next Step**.

<br>![](/exercises/ex4/images/04_02_Migrate_06.png)

7. On step **4. Message Mapping from ESR**, the wizard shows that message mappings and their referenced objects will be imported as local references in the integration flow, while function libraries and imported archive objects will be created as global artifacts. Leave **Enable Reusable Artifacts** unchecked and select **Next Step**.

<br>![](/exercises/ex4/images/04_02_Migrate_07.png)

8. On step **5. Message Mapping Resources**, the wizard identifies **1 dependent resource** with **3 entries**: **mmResponse** (Message Mapping, Local, To be created) and **mtResponse** (WSDL, Local, To be created — listed twice for sender and receiver). Select **Next Step**.

<br>![](/exercises/ex4/images/04_02_Migrate_08.png)

9. Step **6. Java Mapping from ESR** is a new step specific to Java Mapping scenarios. The wizard has identified **1 Java Mapping**: **iaReadAttachment** from the ESR. The **Import Method** is set to **Create with AI** — the Java source will be converted to a Groovy script using AI. The **Groovy Script** name is pre-filled as **iaReadAttachment**. Click the **Script Collection Artifact** dropdown to select the target script collection.

<br>![](/exercises/ex4/images/04_02_Migrate_09.png)

10. The dropdown shows the available script collections in the package. Select **SC_JAVAMapping_XX** (the script collection you created in Exercise 4.1). The **Script Collection Artifact** is now set to **SC_JAVAMapping_XX** and the **Groovy Script** name remains **iaReadAttachment**. Select **Next Step**.

<br>![](/exercises/ex4/images/04_02_Migrate_10.png)

11. On step **7. Scenario Details**, the pre-populated fields show: **Name** and **ID** set to **CS_JAVA_FL_XX**, **Runtime Profile: Cloud Integration**. Under **Inbound Processing**: **Sender Interface: siSendAttachment**, **Sender Component: BC_Sender**. Scroll down to review the Outbound Processing section.

<br>![](/exercises/ex4/images/04_02_Migrate_11.png)

Replace **XX** in the **Name** and **ID** fields with your user number. The **Outbound Processing** section shows receiver **BC_Receiver** with interface **siCreateResponse** (namespace `http://demo.sap.com/soap/java`) — no condition. Select **Review** to proceed.

12. Step **8. Review** shows a full summary of all migration settings: **Process Orchestration System** (POP, Version 7.50), **Process Orchestration Artifacts** (ICO `|BC_Sender|siSendAttachment|http://demo.sap.com/soap/java||`), **Pattern** (Point-to-Point Synchronous, Standard approach), **Message Mapping from ESR** (mmResponse), and **Java Mapping from ESR** — with a note that Java mappings with Import Method **Create** will be converted to Groovy scripts with AI. Verify all settings.

<br>![](/exercises/ex4/images/04_02_Migrate_12.png)

Select **Migrate** to start the migration.

<br>![](/exercises/ex4/images/04_02_Migrate_13.png)

---

## Exercise 4.3 - Review Migration Results

After completing these steps you will have reviewed the generated integration flow artifact, the channel mapping, and the migration report for the Java Mapping scenario.

1. The migration completes with a **Success** banner. The **Artifact Details** tab shows one scenario-specific integration flow: **CS_JAVA_FL_XX**. Select **Report** (top-right) to download the migration report PDF, then proceed through the tabs.

<br>![](/exercises/ex4/images/04_03_Result_01.png)

2. Select the **Channels and Resources** tab. The table shows the adapter mapping for the integration flow **CS_JAVA_FL_XX**: **Sender BC_Sender** uses SOAP → SOAP (channel **CC_SOAP_SND_ATTACHMENT**); **Receiver BC_Receiver** uses SOAP → SOAP (channel **CC_SOAP_RCV_LOOPBACK**).

<br>![](/exercises/ex4/images/04_03_Result_02.png)

3. Select the **Next Steps** tab. Review the post-migration guidance — in particular: for unsupported Java mappings, create the corresponding Groovy script resource in the Script Collection artifact and implement the mapping manually. Since the AI-generated script was created automatically, proceed to verify it in the next exercise. Select **Close** to dismiss the dialog.

<br>![](/exercises/ex4/images/04_03_Result_03.png)

4. Now check the report: The report confirms: **Integration Flow Artifact CS_JAVA_FL_XX was successfully created**, System: POP, Version 7.50, Pattern: P2P_SYNC, Migration Approach: Standard, Type: ICO, PO Artifact Name: `|BC_Sender|siSendAttachment|http://demo.sap.com/soap/java||`. The **Channel Mapping Details** table confirms Sender **BC_Sender** (SOAP → SOAP, CC_SOAP_SND_ATTACHMENT) and Receiver **BC_Receiver** (SOAP → SOAP, CC_SOAP_RCV_LOOPBACK).

<br>![](/exercises/ex4/images/04_03_Result_04.png)

5. Close the report. Back in the **User XX** package, the artifact list now shows **CS_JAVA_FL_XX** (Integration Flow, version 1.0.0, Status: Created) and **Migration_JavaMapping** (Script Collection, version 1.0.0, Status: Created) — the AI-generated Groovy script is stored in the script collection.
<br>![](/exercises/ex4/images/04_03_Result_05.png)

6. Select **CS_JAVA_FL_XX** to open the integration flow and select **Edit** to inspect the flow in detail.

<br>![](/exercises/ex4/images/04_03_Result_06.png)

7. Scroll out to see the full integration flow. You can also use the **Beautify** button on the right to auto-arrange the layout.

<br>![](/exercises/ex4/images/04_03_Result_07.png)

8. Select the SOAP sender channel to inspect it. The **SOAP** channel panel opens at the bottom.

<br>![](/exercises/ex4/images/04_03_Result_08.png)

9. Select the **Connection** tab of the sender SOAP channel. The **CONNECTION DETAILS** show **Address: /CS_JAVA_FL_XX**, **Service Definition: Manual**, **Message Exchange Pattern: Request-Reply**, **Authorization: User Role**, **User Role: ESBMessaging.send**. Note the address placeholder **XX** — replace it with your user number before deploying.

<br>![](/exercises/ex4/images/04_03_Result_09.png)

10. Scroll down to the second local subprocess. The **Local Integration Process 2** pool contains the Java Mapping step: **Start → iaReadAttachment → End**, with an **Exception SubProcess** (Error Start → Error End). The **Local Integration Process** panel at the bottom shows **Name: Local Integration Process 2**. This subprocess holds the AI-converted Groovy script.

<br>![](/exercises/ex4/images/04_03_Result_10.png)

11. Select the **iaReadAttachment** Groovy Script step. The **Groovy Script** panel opens at the bottom. On the **Processing** tab, **Script File** shows `/script/v2/iaReadAttachment.groovy` — a link to the Groovy script in the Script Collection artifact **SC_JAVAMapping_XX** created earlier.

<br>![](/exercises/ex4/images/04_03_Result_11.png)

12. Navigate back to the **User XX** package and open **SC_JAVAMapping_XX**. The Script Collection view shows **References (1)** under **Scripts 2.x**: **iaReadAttachment** (Groovy Script, 2 KB). The right panel shows the AI-generated Groovy script **iaReadAttachment.groovy** — the header comment reads `// AI Generated groovy script from the Java Mapping object of PI/PO`. The script implements the `ReadAttachment` class with a `transform(Message message)` method that reads the SOAP attachment using `message.getAttachments()`, extracts content via `DataHandler`, and wraps the result in the same XML output structure as the original Java mapping. The **Problems** panel at the bottom shows **No data** — the script passed all upgrade readiness and general script checks.

<br>![](/exercises/ex4/images/04_03_Result_12.png)

13. The script is ready for review. Verify that the AI-converted logic matches the original Java mapping behavior shown in the Source ICO Overview. Once satisfied, return to the **CS_JAVA_FL_XX** integration flow and proceed with deployment.

---

## Summary

In this exercise, we focused specifically on the migration of a Java Mapping scenario from SAP Process Orchestration to SAP Integration Suite. The Migration Tooling identified the Java mapping from the ESR and converted it to a Groovy script using AI, storing it in the Script Collection artifact **SC_JAVAMapping_XX**. The resulting integration flow **CS_JAVA_FL_XX** uses a Standard migration approach with SOAP sender and receiver adapters.

> **Note:** We will not deploy this integration flow as part of this session. The purpose of this exercise was to demonstrate how Java Mappings are handled during migration — specifically the AI-assisted conversion to Groovy scripts.

### Upcoming Enhancements

The Java Mapping migration feature is actively being developed. Future releases will include:

- AI-generated scripts will be visually flagged with an AI symbol for differentiation.
- Direct migration of Java Mappings containing `.class` files into Groovy scripts will be supported.
- Improved error handling mechanisms for failed Java mappings for better visibility and transparency.
- Broader coverage for:
  - Multiple Java classes with single entry points.
  - Java classes with external library dependencies.
  - Migration of custom adapter modules (both sender and receiver) to Groovy scripts.
  - Integration within standalone script collection artifact editor for added flexibility.

