# Exercise 3 - Migrate and Test a CBR Async Scenario

  In this exercise, we will migrate a Content-Based Routing scenario where the source ICO routes messages asynchronously based on XPath conditions
  on the supplier's country code. The sender communicates via a REST adapter in JSON format, requiring a JSON-to-XML conversion. Depending on the
  condition result, the message is routed to either receiver **XYZ200** or **XYZ201**. If no condition matches, the message is ignored.

  ## Source ICO Overview

  The following screenshots show the configuration of the source Integrated Configuration Object (ICO) in SAP Process Orchestration before
  migration.

  **Sender Channel** — The sender communication channel **REST_Sender4OrderJSON** is configured on component **BC_CBR_XX** using a REST adapter
  (HTTP transport, JSON message format). The input message format is JSON with element name **mt_order** and namespace
  `http://demo.sap.com/pip/samples`. JSON-to-XML conversion is enabled.

  <br>![](/exercises/ex3/images/SourceICO/03_00_SourceICO_01.png)

  **Inbound Processing** — The ICO **Inbound Processing** tab shows the sender details: Communication Component **BC_CBR_XX**, Interface
  **si_order_async_ob**, Namespace `http://demo.sap.com/pip/samples`. The sender channel **REST_Sender4OrderJSON** uses a REST adapter on the
  Central Adapter Engine.

  <br>![](/exercises/ex3/images/SourceICO/03_00_SourceICO_02.png)

  **Receiver Determination** — The **Receiver** tab defines two Local Rules routing based on XPath conditions on the supplier country code:
  messages where `(/p1:mt_order/supplier/countryCode = DE)` are sent to **XYZ200**, all other messages (`≠ DE`) go to **XYZ201**. If no receiver
  is found, the message is ignored.

  <br>![](/exercises/ex3/images/SourceICO/03_00_SourceICO_03.png)

  **Receiver Interfaces** — The **Receiver Interfaces** tab lists the receiver-specific interface mappings. For receiver **XYZ200**, two
  conditions apply: messages with item category **Software** use operation mapping **om_order_to_order_sw** → interface **si_order_sw_async_ib**;
  all other messages use **om_order_to_order_hw** → interface **si_order_hw_async_ib**. For receiver **XYZ201**, interface **si_order_async_ob**
  is used directly.

  <br>![](/exercises/ex3/images/SourceICO/03_00_SourceICO_04.png)

  **Receiver Channel** — The receiver communication channel **HTTP_Receiver** is configured on component **XYZ200** using an HTTP_AAE adapter
  (HTTP 1.1, POST). The target host points to the Cloud Integration tenant endpoint with path `/http/ho/migration/mocked/receiver`. SSL and basic
  authentication are configured.

  <br>![](/exercises/ex3/images/SourceICO/03_00_SourceICO_05.png)

  **Outbound Processing** — The **Outbound Processing** tab shows three assigned receiver interfaces across receivers XYZ200
  (si_order_sw_async_ib, si_order_hw_async_ib) and XYZ201 (si_order_async_ob), all using the HTTP_Receiver channel with HTTP_AAE adapter on the
  Central Adapter Engine.

  <br>![](/exercises/ex3/images/SourceICO/03_00_SourceICO_06.png)

---

## Exercise 3.1 - Run the Migration Wizard

After completing these steps you will have migrated the CBR Async ICO using the Pipeline approach and a set of scenario-specific integration flows will have been created in your integration package.

1. Navigate to **Design > Integrations and APIs** and open your **User XX** integration package. Select **Edit** to switch to edit mode.

<br>![](/exercises/ex3/images/03_01_Migrate_01.png)

2. On the **Artifacts** tab, select **Migrate** from the toolbar to open the Migration Wizard.

<br>![](/exercises/ex3/images/03_01_Migrate_02.png)

3. The **Migrate Process Orchestration Artifacts** dialog opens. On step **1. Process Orchestration System**, open the **Name** dropdown and select **POP**.

<br>![](/exercises/ex3/images/03_01_Migrate_03.png)

4. The system details are populated automatically — **Version: 7.50**, **Integration Directory** address and Location ID, and **ES Repository** address and Location ID. Select **Next Step**.

<br>![](/exercises/ex3/images/03_01_Migrate_04.png)

5. On step **2. Process Orchestration Artifacts**, the **Object Type** is already set to **Integrated Configuration**. Click the browse icon next to the **Name** field to open the scenario selection dialog.

<br>![](/exercises/ex3/images/03_01_Migrate_05.png)

6. The **Select: Scenario** dialog opens listing 78 ICOs. Locate and select **|BC_CBR_XX|si_order_async_ob|http://demo.sap.com/pip/samples||** from the list.

<br>![](/exercises/ex3/images/03_01_Migrate_06.png)

7. The selected ICO name is now shown in the **Name** field. Select **Next Step**.

<br>![](/exercises/ex3/images/03_01_Migrate_07.png)

8. On step **3. Pattern**, open the **Pattern** dropdown. The available patterns are shown — select **Content-Based Routing Asynchronous** (condition based integration pattern where messages are routed to a single receiver).

<br>![](/exercises/ex3/images/03_01_Migrate_08.png)

9. The pattern is set to **Content-Based Routing Asynchronous**. Under **Select Migration Approach**, select **Pipeline** — this approach provides monitoring, operations, built-in and reusable error handling, and the ability to restart message processing. Select **Next Step**.

<br>![](/exercises/ex3/images/03_01_Migrate_09.png)

10. On step **4. Message Mapping from ESR**, the wizard shows that message mappings and their referenced objects will be imported as local references in the integration flow, while function libraries and imported archive objects will be created as global artifacts. Leave **Enable Reusable Artifacts** unchecked and select **Next Step**.

<br>![](/exercises/ex3/images/03_01_Migrate_10.png)

11. On step **5. Message Mapping Resources**, the wizard identifies **3 dependent resources** for the message mappings from ESR: **mm_order_to_order_sw**, **mm_order_to_order_hw**, and **mm_order_to_order_any** — all with Target Reference Type **Local**, Type **Message Mapping**, and Status **To be created**. Select **Next Step**.

<br>![](/exercises/ex3/images/03_01_Migrate_11.png)

12. On step **6. Scenario**, the **Scenario Details** are pre-populated: **Name** and **ID** are set to **CS_CBR_XX**, the **Description** summarises the migration configuration, and **Runtime Profile** is set to **Cloud Integration**. Under **Sender Details**, **Sender Interface** is **si_order_async_ob** and **Sender Component** is **BC_CBR_XX**. Under **Inbound Processing Integration Flow**, the **JMS Queue Name** is set to **PIPX01**.

<br>![](/exercises/ex3/images/03_01_Migrate_12.png)

13. Replace the placeholder **XX** in **Name**, **ID**, and **Sender Component** with your user number. The **Sender Component** field is now showing your updated value (e.g. **BC_CBR_XX** with your number).

<br>![](/exercises/ex3/images/03_01_Migrate_13.png)

14. Scroll down to review the remaining scenario details. The **Inbound Conversion Integration Flow** shows **Sender Component** **BC_CBR_XX** and **End Point** **/CS_CBR_XX/pip03**. The **Outbound Processing Integration Flow** table lists both receivers: **XYZ201** (interface **si_order_async_ob**) and **XYZ200** (interfaces **si_order_sw_async_ib** and **si_order_hw_async_ib**), each with their XPath conditions and generated endpoints. Select **Review**.

<br>![](/exercises/ex3/images/03_01_Migrate_14.png)

15. Step **7. Review** shows a full summary of all migration settings: **Process Orchestration System** (POP, Version 7.50), **Process Orchestration Artifacts** (Integrated Configuration, ICO name), **Pattern** (Content-Based Routing Asynchronous, Pipeline approach), and **Message Mapping from ESR** (mm_order_to_order_sw, mm_order_to_order_hw, mm_order_to_order_any). Verify all settings and select **Migrate** to start the migration.

<br>![](/exercises/ex3/images/03_01_Migrate_15.png)
