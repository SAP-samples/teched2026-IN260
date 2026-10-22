# Exercise 2 - Migrate and Test an Asynchronous P2P Scenario

In this exercise, we will migrate an simple Point-to-Point Integrated Configuration containing a message mapping with function library.

## Source Integrated Configuration

The source Integrated Configuration is a Point-to-Point pattern containing a message mapping with with function library.

<br>![](/exercises/ex2/images/ICO_Interface.png)

As you can see from the message mapping in the Enterprise Services Repository, a function is used to remove the leading zeros.

<br>![](/exercises/ex2/images/ESR_MessageMapping.png)

## 2.1 - Create a Function Library

Before we run through the migration, we need to create a function library container where the function library is uploaded.

1. Open your previously created package, and switch to the **Artifacts** tab, then switch to **Edit** Mode. If you haven't created an own integration package yet, navigate to [Create an Integration package](/exercises/ex0/#create-an-integration-package), create a new package and then return. Otherwise, proceed with the next steps. Select the entry **Function Libraries** from the **Add** menu.

<br>![](/exercises/ex2/images/SNAG-0003.png)

2. In the upcoming dialog, enter a name such as **Migration_HO_FL_XX** with **XX** the participant number assigned to you. Then select **Add**.

<br>![](/exercises/ex2/images/SNAG-0004.png)


## 2.2 - Run the Migration Wizard

Next, we launch the migration tool which guides us through the migration of the integration scenario.

1. Click on **Migrate** to start the migration wizard.

<br>![](/exercises/ex2/images/SNAG-0005.png)

2.	In the migration wizard, select the SAP Process Orchestration system **POP** from the drop down menu, then click **Next Step**.

<br>![](/exercises/ex2/images/SNAG-0007.png)

3. In SAP Process Orchestration, only Integrated Configuration Objects (ICO) are supported. So keep the Object Type as **Integrated Configuration**. You can use the filter to filter out the list of ICOs and choose the appropriate scenario. Click on the **Selection icon. 

<br>![](/exercises/ex2/images/SNAG-0008.png)

4. Choose the interface **si_order_async_ob** with sender **BC_P2P_A1_XX** from the Items list.

<br>![](/exercises/ex2/images/SNAG-0009.png)

5. Once you have selected the scenario, click **Next Step**.

<br>![](/exercises/ex2/images/SNAG-0010.png)

6.	The best fit pattern is identified, in this case the pattern **Point-to-Point Asynchronous**. In this exercise, we stick to the migration approach **Standard**. To keep it simple, we choose not to decouple using a JMS queue. So, **deselect** the **Decouple with JMS Queue** flag. Then, click **Next Step**.

<br>![](/exercises/ex2/images/SNAG-0011.png)

7. In the next step, you can choose whether you like to work with reusable message mapping artifacts or not.
   [!NOTE]In this exercise, we like to use local mappings. So, **deselect** the **Enable Reusable Artifacts** flag. Then, click **Next Step**.

<br>![](/exercises/ex2/images/SNAG-0013.png)

8. In the next step, you can see the message mapping resources. Because the mapping contains a function within a function library, you need to select the function library container that where the function library will be uploaded. Select the **Select** button below the **Actions**.

<br>![](/exercises/ex2/images/SNAG-0015.png)

9. In the upcoming dialog, select the beforehand created function library container **Migration_HO_FL_XX**. Click **Select**.

<br>![](/exercises/ex2/images/SNAG-0016.png)

10. All should be green now, and you can click **Next Step**.

<br>![](/exercises/ex2/images/SNAG-0018.png)

11.	On the next screen, you need to configure your scenario which impacts the generated integration artifact. Here you have the option to define a scenario name, change the proposed sender component and sender interface name. Maintain a **Name** for your scenario following the pattern **CS_P2P_Async_FL_XX** (CS stands for Configuration Scenario) where **XX** is the user number assigned to you. Furthermore, replace the **XX** in the **Sender Component** name **BC_P2P_A1_XX** with the user number assigned to you. The rest can be kept as is. Then, click on **Review**.

<br>![](/exercises/ex2/images/SNAG-0019.png)

12.	Verify the information and click on **Migrate**.

<br>![](/exercises/ex2/images/SNAG-0020.png)

13.	Upon successful migration, you can gather the information of the generated artifacts. You can click through the different tabs. On the first tab, **Artifact Details**, you see the generated integration flow. From here, you can open the integration flow by clicking on **View Artifact**.

<br>![](/exercises/ex2/images/SNAG-0021.png)

14.	If you switch to the **Channels and Resources** tab, you can see the source adapters on SAP Process Orchestration beeing mapped to the corresponding adapter on Cloud Integration.

<br>![](/exercises/ex2/images/SNAG-0022.png)

15. On the **Next Steps** tab, you get further information provided of potential next steps. Close the migration wizard.

<br>![](/exercises/ex2/images/SNAG-0024.png)

## 2.3 - Configure and Deploy

You should now see a new integration flow created in your integration package. Next, we need to configure and deploy both the integration flow and the function library.

1. Select the function library container **Migration_HO_FL_XX** to open its editor.

<br>![](/exercises/ex2/images/SNAG-0025.png)

2. As you can see, a new function library has been added to the function library container. Click **Deploy**.

<br>![](/exercises/ex2/images/SNAG-0027.png)

3. Confirm the upcoming dialog.

<br>![](/exercises/ex2/images/SNAG-0028.png)

4. Once you have confirmed the upcoming dialog, you should see a toast message on the bottom that the artifact has been deployed. Then navigate back to your package.

<br>![](/exercises/ex2/images/SNAG-0030.png)

5. Select entry **Configure** from the **Actions** menu of your integration flow.

<br>![](/exercises/ex2/images/SNAG-0035.png)

6. On the upcoming dialog, you need to configure your integration flow. In our case, the tenants have been prepared in such a way that most settings can be kept as migrated from the ICO. You only need to change the end point of your integration flow because this needs to be unambiguous within the tenant. Change the **Address** of the sender http adapter which has been automatically preset based on the source sender REST adapter settings on SAP Process Orchestration. Here, replace the **XX** in the **Address** with the number assigned to you. Then, click on **Save** and finally click on ***Deploy** to trigger the deployment.

<br>![](/exercises/ex2/images/SNAG-0036.png)

7. Confirm the upcoming dialogs.

<br>![](/exercises/ex2/images/SNAG-0040.png)

8. Once you have confirmed the upcoming dialogs, you should see a toast message on the bottom that your integration flow has been deployed.

<br>![](/exercises/ex2/images/SNAG-0042.png)

9. Let's navigate to the monitoring page to check the deployment status. Navigate to **Monitor > Integrations and APIs** from the left navigation pane. From here, select the **Manage Integration Content** tile.

<br>![](/exercises/ex2/images/SNAG-0044.png)

10. In the **Manage Integration Content** page, filter for your user number **XX**. Confirm that all your artifacts are in status **Started**.

<br>![](/exercises/ex2/images/SNAG-0046.png)

## 2.4 - Test and Monitor

To test your configuration scenario, we use the Bruno API client application for which we have provided a collection with pre-configured sample requests. As a prerequisite to test your integration scenario using the Bruno API client, you should have gone through [Setup Bruno API client](../ex0#setup-bruno-api-client/). If not, do the setup, then come back and proceed with the steps below.

1. Open the Bruno application on your laptop, expand the **Teched 26 Migration Hands-on** collection the folder **P2P Async with Function Library** and select the POST request. In the provided URL, the **participant** variable should hold your user number **XX** assigned to you assuming that you have properly configured the parameter in the Bruno API client setup. Trigger a message by selecting the **Send** button on the upper right.

<br>![](/exercises/ex2/images/SNAG-0049.png)

2. Upon success, you will receive **200 OK** status. As you can see from the response, the order number and the partner number have been trimmed.

<br>![](/exercises/ex2/images/SNAG-0050.png)

3. Navigate back to the **Manage Integration Content** page, and click the **Monitor Message Processing** link of your deployed integration flow.

<br>![](/exercises/ex2/images/SNAG-0051.png)

4. This will navigate you to the message monitor. You should see a message processing log in status **Completed**.

<br>![](/exercises/ex2/images/SNAG-0052.png)

## Summary

You've now successfully migrated and tested your scenario.

Continue to - [Exercise 3 - Content-Based-Router using Pipeline approach](../ex3/README.md)
