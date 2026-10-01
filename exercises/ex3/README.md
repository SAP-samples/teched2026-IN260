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
