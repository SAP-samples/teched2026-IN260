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
