```
  _____         _       _____    _    ____   ___   ____   __  __
 |_   _|__  ___| |__   | ____|__| |  |___ \ / _ \ |___ \ / / / /
   | |/ _ \/ __| '_ \  |  _| / _` |    __) | | | |  __) / /_/ /
   | |  __/ (__| | | | | |__| (_| |   / __/| |_| | / __/ \_, /
   |_|\___|\___|_| |_| |_____\__,_|  |_____|\___(_)_____(_)/_/

                        Session IN-260
          SAP Integration Suite -- Migration Tooling
          Exercise 4: Migrate a Java Mapping Scenario
```

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

## Exercise 4.1 - Run the Migration Wizard

After completing these steps you will have...

1. Click here.
<br>![](/exercises/ex4/images/04_01_0010.png)

## Exercise 4.2 - Review Migration Results

After completing these steps you will have...

1. Click here.
<br>![](/exercises/ex4/images/04_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 5](../ex5/README.md)
