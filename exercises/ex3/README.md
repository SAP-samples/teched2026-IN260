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

10. On step **4. Message Mapping from ESR**, we like to reuse message mappings that have been already uploaded to Cloud Integration. Leave the flag **Enable Reusable Artifacts** checked and select **Select** to select the integration package where the reusable message mappings reside.

<br>![](/exercises/ex3/images/03_01_Migrate_10.png)

11. [!NOTE] In the upcoming **Package Selection** dialog, select the **Migration Hands-on - Solution** package.

<br>![](/exercises/ex3/images/03_01_Migrate_10a.png)

12. For the other two message mappings, select the same package.

<br>![](/exercises/ex3/images/03_01_Migrate_10b.png)

13. Once all packages have been selected, select the reusable message mapping artifact from the drop down menu. Do this for all message mappings.

<br>![](/exercises/ex3/images/03_01_Migrate_10c.png)

14. Once all reusable message mappings have been selected, select **Next Step** to proceed.

<br>![](/exercises/ex3/images/03_01_Migrate_10d.png)

15. On step **5. Message Mapping Resources**, the wizard identifies **3 dependent resources** for the message mappings from ESR: **mm_order_to_order_sw**, **mm_order_to_order_hw**, and **mm_order_to_order_any** — all referring to the beforehand selected reusable artifacts, Type **Message Mapping**, and Status **To be reused**. Select **Next Step**.

<br>![](/exercises/ex3/images/03_01_Migrate_11.png)

16. On step **6. Scenario**, the **Scenario Details** are pre-populated: **Name** and **ID** are set to **CS_CBR_XX**, the **Description** summarises the migration configuration, and **Runtime Profile** is set to **Cloud Integration**. Under **Sender Details**, **Sender Interface** is **si_order_async_ob** and **Sender Component** is **BC_CBR_XX**. Under **Inbound Processing Integration Flow**, the **JMS Queue Name** is set to **PIPX01**.

<br>![](/exercises/ex3/images/03_01_Migrate_12.png)

17. Replace the placeholder **XX** in **Name**, **ID**, and **Sender Component** with your user number. The **Sender Component** field is now showing your updated value (e.g. **BC_CBR_XX** with your number).

<br>![](/exercises/ex3/images/03_01_Migrate_13.png)

18. Scroll down to review the remaining scenario details. The **Inbound Conversion Integration Flow** shows **Sender Component** **BC_CBR_XX** and **End Point** **/CS_CBR_XX/pip03**. The **Outbound Processing Integration Flow** table lists both receivers: **XYZ201** (interface **si_order_async_ob**) and **XYZ200** (interfaces **si_order_sw_async_ib** and **si_order_hw_async_ib**), each with their XPath conditions and generated endpoints. Select **Review**.

<br>![](/exercises/ex3/images/03_01_Migrate_14.png)

19. Step **7. Review** shows a full summary of all migration settings: **Process Orchestration System** (POP, Version 7.50), **Process Orchestration Artifacts** (Integrated Configuration, ICO name), **Pattern** (Content-Based Routing Asynchronous, Pipeline approach), and **Message Mapping from ESR** (mm_order_to_order_sw, mm_order_to_order_hw, mm_order_to_order_any). Verify all settings and select **Migrate** to start the migration.

<br>![](/exercises/ex3/images/03_01_Migrate_15.png)

---

## Exercise 3.2 - Inspect the Migration Result

After completing these steps you will have reviewed the artifacts, channels, Partner Directory entries, and XSLT mapping that the Migration Wizard created.

1. The migration completes with a **Success** banner. The **Artifact Details** tab lists the 5 scenario-specific integration flows created by the Pipeline approach: **CS_CBR_XX_PIP01_InboundProcessing_BC_CBR_XX**, **CS_CBR_XX_PIP03_InboundConversion_BC_CBR_XX**, **CS_CBR_XX_PIP07_OutboundProcessing_XYZ200_IFIndex_1**, **CS_CBR_XX_PIP07_OutboundProcessing_XYZ200_IFIndex_2**, and **CS_CBR_XX_PIP07_OutboundProcessing_XYZ201_IFIndex_1**. Select **Channels and Resources** to continue.

<br>![](/exercises/ex3/images/03_02_Result_01.png)

2. The **Channels and Resources** tab shows the adapter mapping for each integration flow. The sender **BC_CBR_XX** uses a **REST** → **HTTPS** adapter (channel **REST_Sender4OrderJSON**) in the Inbound Processing flow. The three receiver flows for **XYZ200** and **XYZ201** each use an **HTTP_AAE** → **HTTP** adapter (channel **HTTP_Receiver**). Select **Partner Directory** to continue.

<br>![](/exercises/ex3/images/03_02_Result_02.png)

3. The **Partner Directory** tab shows the parameterized properties stored centrally. The partner **CS_CBR_XX** has been created with a **String Parameter** (**InboundConversionEndpoint_BC_CBR_XX**), a **Binary Parameter** (**receiverDetermination**), and an **Alternative Partner** entry (Agency **BC_CBR_XX**, Scheme **SenderInterface**, ID **si_order_async_ob**). Select the PID link **CS_CBR_XX** to inspect the entries.

<br>![](/exercises/ex3/images/03_02_Result_03.png)

4. The **Manage Partner Directory** view opens. Select **CS_CBR_XX** from the partner list. The **String Parameters** tab shows **InboundConversionEndpoint_BC_CBR_XX** with value **/CS_CBR_XX/pip03**.

<br>![](/exercises/ex3/images/03_02_Result_04.png)

5. Switch to the **Binary Parameters** tab. The entry **receiverDetermination** has Content-Type **xsl** and Status **Exists**. Select the **...** action menu and choose **Download** to inspect the generated XSLT.

<br>![](/exercises/ex3/images/03_02_Result_05.png)

6. The downloaded XSLT shows the generated receiver determination logic. It implements the XPath routing: if `(/p1:mt_order/supplier/countryCode != 'DE')` the message goes to **XYZ201** (endpoint `/CS_CBR_XX/pip07/XYZ201/IFIndex1`); if `(/p1:mt_order/supplier/countryCode = 'DE')` the message goes to **XYZ200** — with a further item category sub-condition routing **Software** items to IFIndex1 and all others to IFIndex2. Unmatched messages use `<Type>Ignore</Type>`.

<br>![](/exercises/ex3/images/03_02_Result_06.png)

7. Switch to the **Alternative Partners** tab. The entry shows Agency **BC_CBR_XX**, Scheme **SenderInterface**, ID **si_order_async_ob** — this is used by the Pipeline to identify the scenario from the inbound message.

<br>![](/exercises/ex3/images/03_02_Result_07.png)

8. Back in the migration result dialog, select **Next Steps** tab. Review the next steps guidance. Select **Close** to dismiss the dialog.

<br>![](/exercises/ex3/images/03_02_Result_08.png)

9. Select **Close** to close the migration result and return to the **User XX** integration package.

<br>![](/exercises/ex3/images/03_02_Result_09.png)

---

## Exercise 3.3 - Deploy the Integration Flows

After completing these steps all 5 scenario-specific integration flows will be deployed and running on the Cloud Integration runtime.

1. Back in the **User XX** package on the **Artifacts** tab, the 5 CBR integration flows are listed with status **Created** (version 1.0.0). Select all 5 CBR flows using their checkboxes, then open the **Actions** dropdown in the toolbar and select **Configure**.

<br>![](/exercises/ex3/images/03_03_Deploy_01.png)

2. The **Configure Selected Artifacts** dialog opens. On the **Sender** tab, verify the sender configuration: **Sender** is **BC_CBR_XX**, **Adapter Type** is **HTTPS**, **Address** is **/ho/cbr/order/json/XX** (replace **XX** with your user number), **Authorization** is **User Role**, and **User Role** is **ESBMessaging.send**. Select **Deploy All**.

<br>![](/exercises/ex3/images/03_03_Deploy_02.png)

3. Confirm the address shows your user number (e.g. **/ho/cbr/order/json/XX**) and select **Deploy All** to trigger deployment.

<br>![](/exercises/ex3/images/03_03_Deploy_03.png)

4. A **Deploy Confirmation** dialog appears showing that **5 artifacts** are about to be deployed with **Runtime Profile: Cloud Integration**. Select **Deploy** to confirm.

<br>![](/exercises/ex3/images/03_03_Deploy_04.png)

5. The deployment is triggered. A toast notification confirms **All artifacts are triggered for deployment**. The artifact list still shows the flows selected.

<br>![](/exercises/ex3/images/03_03_Deploy_05.png)

6. Navigate to **Monitor > Integrations and APIs** in the left navigation. The **Manage Integration Content** tile shows all 35 artifacts, with deployment still in progress (**31 Started**). Wait until all flows show **Started**.

<br>![](/exercises/ex3/images/03_03_Deploy_06.png)

7. In **Manage Integration Content**, search for **XX** to filter your flows. Select **CS_CBR_XX_PIP07_OutboundProcessing_XYZ200_IFIndex_2** — it shows **Status: Starting** with the message *The Integration Flow is starting*.

<br>![](/exercises/ex3/images/03_03_Deploy_07.png)

8. After a few seconds, refresh the list. Select **CS_CBR_XX_PIP01_InboundProcessing_BC_CBR_XX** — it now shows **Status: Started**, **The Integration Flow is deployed successfully**, and the public **Endpoint**: `https://workshop-eu-02b.it-cpi035-rt.cfapps.eu20-001.hana.ondemand.com/http/ho/cbr/order/json/XX`. Copy this endpoint URL — you will need it for testing.

<br>![](/exercises/ex3/images/03_03_Deploy_08.png)

9. All 5 CBR flows now show **Started** in the integration content list. Deployment is complete.

<br>![](/exercises/ex3/images/03_03_Deploy_09.png)

---

## Exercise 3.4 - Test the Migrated CBR Scenario

After completing these steps you will have verified all three routing paths of the CBR scenario: DE supplier with Software item (→ XYZ200 IFIndex_1), DE supplier with non-Software item (→ XYZ200 IFIndex_2), and non-DE supplier (→ XYZ201).

1. Open **Bruno** and navigate to **Teched 26 Migration Hands-on XX > CBR > Post Order - DE & Software**. The request body contains a JSON order with **supplier.countryCode: DE** and **item.category: Software**. Select **Send**.

<br>![](/exercises/ex3/images/03_04_Test_01.png)

2. Bruno returns **200 OK** (695 ms). The raw response body confirms the message was accepted by the Cloud Integration endpoint.

<br>![](/exercises/ex3/images/03_04_Test_02.png)

3. Navigate to **Monitor > Integrations and APIs > Manage Integration Content**, search for **XX**, and select **CS_CBR_XX_PIP01_InboundProcessing_BC_CBR_XX**. Under **Artifact Details**, select **Monitor Message Processing** to open the message monitor filtered to this artifact.

<br>![](/exercises/ex3/images/03_04_Test_03.png)

4. The **Monitor Message Processing** view shows **Messages (1)** with status **Completed** (397 ms). The detail panel on the right shows **Message processing completed successfully**, with **Sender: BC_CBR_XX** and **Application Message Type: si_order_async_ob**. Copy the **Correlation ID** to trace the full pipeline.

<br>![](/exercises/ex3/images/03_04_Test_04.png)

5. Paste the Correlation ID into the **ID** filter field and press search. The view now shows **Messages (5)** — the complete pipeline trace for this message: **CS_CBR_XX_PIP01_InboundProcessing** (397 ms), **CS_CBR_XX_PIP03_InboundConversion** (11 ms), **Pipeline v2 Generic Step02 - Integrated Messaging Runtime Async** (2 sec 24 ms → Receiver: XYZ200), **CS_CBR_XX_PIP07_OutboundProcessing_XYZ200_IFIndex_1** (686 ms → Receiver: XYZ200), and **Pipeline v2 Generic Step06 - Outbound Processing** (1 sec 31 ms → Receiver: XYZ200). All messages completed successfully.

<br>![](/exercises/ex3/images/03_04_Test_05.png)

6. Select **Pipeline v2 Generic Step02 - Integrated Messaging Runtime Async** to inspect the routing decision. The **Properties** tab shows the **Custom Headers**: **partnerID: CS_CBR_XX**, **QualityOfService: ExactlyOnce**, **ReceiverJMSQueue: PIPQ04**, **SenderAlias: BC_CBR_XX**, **SenderJMSQueue: PIPX01**, **tenantStage: PRD** — confirming the message was routed to XYZ200 via the correct queue.

<br>![](/exercises/ex3/images/03_04_Test_06.png)

7. In Bruno, select **Post Order - DE & Keyboards**. The request body contains **supplier.countryCode: DE** and **item.category: Keyboards** (non-Software). Select **Send**.

<br>![](/exercises/ex3/images/03_04_Test_07.png)

8. Bruno returns **200 OK** (292 ms). The raw response confirms message acceptance.

<br>![](/exercises/ex3/images/03_04_Test_08.png)

9. Back in the monitor, the second message trace shows **Messages (5)** — the pipeline routed to **XYZ200 IFIndex_2** (Keyboards, non-Software path). The **CS_CBR_XX_PIP01_InboundProcessing** entry (29 ms) confirms a new message arrived. The **Step02 Properties** tab shows **ReceiverJMSQueue: PIPQ04** and Receiver **XYZ200**, this time resolved to IFIndex_2.

<br>![](/exercises/ex3/images/03_04_Test_09.png)

10. In the monitor, refresh to see **Messages (2)** for the Inbound Processing artifact — the second message completed in 29 ms. Copy the new Correlation ID to trace the full pipeline for the Keyboards test case.

<br>![](/exercises/ex3/images/03_04_Test_10.png)

11. Filtering by the new Correlation ID shows **Messages (5)** — the complete pipeline for the DE/Keyboards order, all **Completed**, routed to **XYZ200** via **CS_CBR_XX_PIP07_OutboundProcessing_XYZ200_IFIndex_2**.

<br>![](/exercises/ex3/images/03_04_Test_11.png)

12. In Bruno, select **Post Order - NL & Notebooks**. The request body contains **supplier.countryCode: NL** (non-DE) and **item.category: Notebooks**. Select **Send**.

<br>![](/exercises/ex3/images/03_04_Test_12.png)

13. Bruno returns **200 OK** (644 ms). The raw response confirms message acceptance.

<br>![](/exercises/ex3/images/03_04_Test_13.png)

14. Navigate to **Monitor > Integrations and APIs** overview. The **Monitor Message Processing** tile now shows **16 Messages** and **16 Completed Messages** in the past hour — all three test messages plus the pipeline steps processed successfully. The **Manage Integration Content** tile shows **35 Started, 0 Error**.

<br>![](/exercises/ex3/images/03_04_Test_14.png)

15. Open the **Message Status Overview** and set **Overview By** to **Artifacts**. The table shows **8 line items**: all CBR pipeline flows with **Completed: 3** each (one per test message) for the Inbound and Outbound Processing steps. This confirms all three routing paths executed correctly.

<br>![](/exercises/ex3/images/03_04_Test_15.png)

16. Change **Overview By** to **Senders**. The table shows **2 senders**: **BC_CBR_XX** with 15 completed messages (3 test messages × 5 pipeline steps) and **BC_P2P_A1_XX** with 1 completed message from the previous exercise.

<br>![](/exercises/ex3/images/03_04_Test_16.png)

17. Navigate to **Monitor Message Processing** and filter by **Sender: BC_CBR_XX** and **Status: Completed**. The full message log shows **15 messages** covering all three test runs — the complete pipeline execution history for the CBR scenario.

<br>![](/exercises/ex3/images/03_04_Test_17.png)

## Summary

You've now successfully migrated and tested your scenario.

Continue to - [Exercise 4 - Migration agent](../ex4/README.md)
