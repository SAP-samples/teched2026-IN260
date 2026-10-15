# Exercise 1 - Migration Assessment

In this exercise, you will learn how to use the Migration Assessment capability of SAP Integration Suite. This offering helps you estimating the technical efforts involved in the migration process and evaluates how various integration scenarios might be migrated. In this exercise, you won't do any configuration, you just browse through the application and reports to familiarize yourself with the tool.

## Exercise 1.1 - Launch the Migration Assessment and Checkout the Rules Framework

After completing these steps you will be able to understand what rules the assessment is based on.

1. If you haven't done so, login to the tenant provided to you with your user **userXX** with **XX** the user number assigned to you and the credentials provided to you.
      
2. In the SAP Integration Suite landing page, scroll down to **Capabilities**, and select **Create Requests** from the **Assess Migration Scenarios** tile.

<br>![](/exercises/ex1/images/01_01_Assessment_01.png)
   
3. In the Migration Assessment application, navigate to **Configure > Rules**. Here you can find a list of rules predefined by SAP. Rules are a set of characteristics according to which the application evaluates whether an integration scenario can be migrated and what effort you can expect. As an example, select the rule **ReceiverJDBCDriver**.

**Note:** You can’t add custom rules or edit the standard rules. You can only view the standard rules.

<br>![](/exercises/ex1/images/01_01_Assessment_03.png)  

4. Open the variant **MAIN_ReceiverJDBCDriver**. Here, you can see all the parameters of the rules, such as Rule Match Value, Assessment Category, and the Weight assigned to each rule match value. Based on these weights, the application calculates the estimated effort, which means that some parameters, and therefore rules, have a bigger influence on the final estimation than others.

<br>![](/exercises/ex1/images/01_01_Assessment_04.png)

## Exercise 1.2 - Browse through the Data Extraction Overview

In this exercise, you will assess your integration data extraction and evaluation results.

1. In the Migration Assessment application, navigate to **Request**. Select the **Data Extractions** tile.

<br>![](/exercises/ex1/images/01_02_Assessment_01.png)

2. The Data Extractions list shows your existing extraction requests. Select the request **POP-Extract** (Status: Completed). A warning popup appears indicating that some scenarios were not fully extracted. Select **View Dashboard**.

<br>![](/exercises/ex1/images/01_02_Assessment_02.png)

3. The **Extraction Dashboard** for the *POP-Extract* request opens. The pie chart shows **78 extracted integration scenarios** in total: 71 extracted successfully, 7 extracted with warnings, and 0 with errors. To switch to the list view, select the **table icon** in the top right corner of the chart.

<br>![](/exercises/ex1/images/01_02_Assessment_03.png)

4. The table view lists all 78 extracted integration scenarios with their interface, namespace, communication parties, and extraction status. For scenarios marked **Extracted with warning**, select the **...** context menu and choose **View Details**.

<br>![](/exercises/ex1/images/01_02_Assessment_04.png)

5. The **Extraction Log** dialog opens for the selected scenario. It shows the warning details — in this case, the extraction could not identify a *SoftwareComponentVersionID* for the Default Service Interface. Review the log entries and select **Close**.

<br>![](/exercises/ex1/images/01_02_Assessment_05.png)

## Exercise 1.3 - Browse through the Scenario Evaluation Results

In this exercise, you will assess your integration scenarios using the information from the data extraction and evaluation requests. You can
access and download analysis of your scenario evaluation runs with details specific to your integration scenarios, for example, adapters and an
overview of the rules used in the evaluation. Further, you can download details about the latest evaluation run in one of two formats
<b>.xlsx</b> file and <b>.pdf</b> file.

1. Navigate to **Request** and select the **Scenario Evaluation** tile.

<br>![](/exercises/ex1/images/01_03_Assessment_01.png)

2. An existing evaluation request **POP-Eval** is already available (Status: Completed, based on the *POP-Extract* data extraction). Select the
 **Open Dashboard** icon to access the results of the evaluation.

<br>![](/exercises/ex1/images/01_03_Assessment_02.png)

3. The dashboard shows an analysis of your scenario evaluation run **POP-Eval-Run-1**. The **Overview** tab shows scenarios grouped by
assessment categories (48 Ready to migrate, 24 Adjustment required, 6 Evaluation required) and by effort estimation (S/M/L/XL t-shirt sizing).
Scroll down to explore the Modernization Recommendations and adapter statistics.

<br>![](/exercises/ex1/images/01_03_Assessment_03.png)

4. Continue scrolling to see the full **Modernization Recommendations** bar chart broken down by category: Clean Core (3), Integration Style
(34), Mapping (1), Protocol (50), Security (48).

<br>![](/exercises/ex1/images/01_03_Assessment_04.png)

5. Continue scrolling to see the **Sender Adapters** and **Receiver Adapters** charts at the bottom of the Overview tab.

<br>![](/exercises/ex1/images/01_03_Assessment_05.png)

6. Switch to the **Integration Scenarios** tab to see all 78 scenarios listed with interface, namespace, communication parties, t-shirt size,
   assessment category, and message throughput for the last 1, 3, and 6 months.

<br>![](/exercises/ex1/images/01_03_Assessment_06.png)

7. Select **Export** to download the evaluation results as an Excel spreadsheet. A **Download Report** dialog appears — keep the language set to
  **English** and select **Download**.

<br>![](/exercises/ex1/images/01_03_Assessment_07.png)
<br>![](/exercises/ex1/images/01_03_Assessment_08.png)

8. On the first tab **Full Evaluation Results** of the downloaded Excel file, all integration scenarios are listed with the rules applied, sub
  characteristics, values, weights, and assessment categories.

<br>![](/exercises/ex1/images/01_03_Assessment_09.png)

9. On tab **Eval by Integration Scenario**, scenarios are summarized with their overall weight, t-shirt size, assessment category, and min/max
  effort required in hours.

<br>![](/exercises/ex1/images/01_03_Assessment_10.png)

10. On tab **Scenario Technical Details**, technical details are listed per integration scenario, including push/polling mode, iFlow name,
  interface type, and sender/receiver communication parties and components.

  <br>![](/exercises/ex1/images/01_03_Assessment_11.png)

11. On tab **Recommendations**, modernization recommendations are listed per integration scenario with the modernization category, possible
  modernization item, and specific recommendation (e.g., Business Event for Integration Style scenarios).

  <br>![](/exercises/ex1/images/01_03_Assessment_12.png)

12. The **Effort Estimation** tab provides a detailed effort summary table broken down by sizing classification (S/M/L/XL) and assessment
  category, with total min/max/average effort in days — including repeatable factor discounts.

  <br>![](/exercises/ex1/images/01_03_Assessment_13.png)
  
13. On tab **API Recommendations**, alternative API-based implementation options are listed for IDoc and BAPI objects, including API name,
  technical name, protocol (OData, SOAP), and the target SAP S/4HANA solution.

  <br>![](/exercises/ex1/images/01_03_Assessment_14.png)

14. On tab **Business Event Recommendations**, recommendations for event-based alternatives are listed per object, including business event
  name, technical name, and the target SAP S/4HANA solution.

  <br>![](/exercises/ex1/images/01_03_Assessment_15.png)

15. Navigate back to the Migration Assessment application. To also generate the PDF report, open the **...** context menu for **POP-Eval** and
  select **Generate Report**.

  <br>![](/exercises/ex1/images/01_03_Assessment_16.png)

16. A **Download Report** dialog appears — keep the language set to **English** and select **Download** to generate the PDF report.

  <br>![](/exercises/ex1/images/01_03_Assessment_17.png)

17. The downloaded PDF report opens. It is titled *"Move from SAP Process Orchestration to SAP Integration Suite — Migration Assessment Report"*
  and covers the system **POP**. Browse through the report to review its structure.

  <br>![](/exercises/ex1/images/01_03_Assessment_18.png)

18. Navigate to page 4 of the PDF report. It contains the **Assessment Categories** definitions (Ready to migrate, Adjustment required,
  Evaluation required) and the **Scenario Categorization Summary** pie chart.

  <br>![](/exercises/ex1/images/01_03_Assessment_19.png)

  19. Navigate further to the **Modernization Recommendations** section of the PDF report. It lists all recommendation categories (Clean Core,
  Integration Style, Mapping, Protocol, Security) with their applicable scenarios and specific recommendations.

  <br>![](/exercises/ex1/images/01_03_Assessment_20.png)

  20. Once done reviewing, close the PDF report and navigate back to the SAP Integration Suite landing page by clicking on the **SAP sign**.
 
  ---

  ## Summary

  You've now made yourself familiar with the major capabilities of the Migration Assessment tool. You are now all set to migrate an integration
  artifact with the Migration tool.

  Continue to - [Exercise 2 - Migrate a Point-to-Point scenario](../ex2/README.md)


