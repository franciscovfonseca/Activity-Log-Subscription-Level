<br>

<h1 align="center">Activity Log (Subscription Level)</h1>

<br>

<p align="center">
<img width="800" src="https://github.com/user-attachments/assets/0fa0d577-c433-47a3-a3d7-db5e599d0437" alt="Banner"/>
<br />

<br />

In this lab we’re going to continue **Bringing In Logs** from our various Resources inside of our **Azure Environment**.

In this particular one we're going to bring in the **Activity Log**.

<br>

>   <details close> 
>   
> **<summary> 📝 Explanation</summary>**
> 
> <br>
> 
> **Activity Log** is basically **Subscription Level Logging** ➜ so anything you do **Inside the Subscription**.
> 
> This includes Creating, Deleting or Changing Resources ➜ for example Changing NSGs.
> 
> Essentially everything you do in the Azure Portal involving Resources ➜ those Logs will be Created from what's called the **Activity Log**.
> 
> <br>
> 
> Just as a Reminder ➜ this is the Activity Level:
>   
> ![azure portal](https://github.com/user-attachments/assets/2727e56c-f7ee-478b-99f3-ecbabe939fd6)
>   
> ☝️ We refer to this as **Management Plane** ➜ which involves actually changing configurations in the Azure Portal.
> 
>   </details>

<br>

<br>

So in this Lab we'll go to the Activity Log in **Azure Monitor** and choose to Export other various Log Types into our **Log Analytics Workspace**.

Then we're going to Create some Resource Groups ➜ Delete them ➜ and then we'll Query them in our LAW.

✅  Just to make sure the Logs are coming in.

<br>

<br>

<br>

<details close> 
<summary> <h2> 1️⃣ Export Azure Activity Logs to Log Analytics Workspace</h2> </summary>
<br>

> Activity Log is where all those Management Plane Activities come from
>
> These include Creating and Deleting Resources, Creating Resource Groups, etc. ➜ so that's the kind of stuff where's going to be logging

<br>

We can go back to the **Azure Portal** ➜ search for **Monitor** ➜ and click on the **Activity log** blade

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

Then we'll click on **⚙️ Export Activity Logs**:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

And we're going to ➕ **Add diagnostic setting** to create another Diagnostic Setting:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

- The **"Diagnostic setting name"** can be ```ds-azure-activity```

- For the **Logs’ Categories** ➜ we can select ☑️ all the options

- **"Destination details"** ➜ check ☑️ **Send to Log analytics workspace** ➜ select ```LAW-Cyber-Lab-01```
    - ⚠️ Again ➜ Make sure it’s going to the correct one ➜ not the *DefaultWorkspace*

- Click 💾 Save

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ We can click back on **Diagnostic settings** and confirm that ```ds-azure-activity``` was successfully created:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2> 2️⃣ Generate some Logs</h2> </summary>
<br>

In this next section of the Lab we're going to:

1. Create a new **Resource Group** called ```Scratch-Resource-Group```

2. Then we'll also create another **Resource Group** called ```Critical-Infrastructure-Wastewater```

3. After that, we'll **Delete** both of these **Resource Groups**

4. And finnally, we're going to do some **Queries against the Logs those actions Generated**

<br>

<h2></h2>

<br>

<h3> ❶ Create a new Resource Group named “Scratch-Resource-Group”</h3>
<br>

Back inside the **Azure Portal** ➜ search for **Resource Groups** ➜ and click on ➕ **Create**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

- We'll name the first Resource Group ```Scratch-Resource-Group```

- For the **Region** ➜ select ```(US) East US 2```

- Then click **Review + Create**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<br>

<h2></h2>

<br>

<h3> ❷ Create another new Resource Group named “Critical-Infrastructure-Wastewater”</h3>
<br>

For the Next Resource Group we'll follow the same steps as for the previous one.

- We'll name this second Resource Group ```Critical-Infrastructure-Wastewater```

- Again ➜ for the **Region** ➜ select ```(US) East US 2```

- Then click **Review + Create**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<br>

✅ We can confirm that both of our new Resource Groups were created:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<br>

<h2></h2>

<br>

<h3> ❸ Delete both of the new Resource Groups</h3>
<br>

Back to the **Azure Portal** ➜ go to our **Resource groups** ➜ click on ```Scratch-Resource-Group```

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

We'll click on 🗑️ **Delete resource group**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

And then we'll Delete the Resource Group:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<br>

<h2></h2>

<br>

<br>

We'll do the exact same thing to **Delete** the ```Critical-Infrastructure-Wastewater``` Resource Group:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<br>

<h2></h2>

<br>

<h3> ❹ Query for the Deletion of Critical Resource Groups</h3>
<br>

We'll now copy the following **Query** and paste into our **Log Analytics Workspace** to Inspect the **Delete Logs** we just Generated:

<br>

// ***Querying for the deletion of critical Resource Groups***:

```commandline
AzureActivity
| where ResourceGroup startswith "Critical-Infrastructure-"
| order by TimeGenerated
```

<br>

✅ We can confirm that the **Activity Logs** are properly being Generated & Forwarded to our LAW:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)




<br>

<br>



We’ll go to our **Log Analytics Workspace** ➜ and click on the **"Tables"** blade:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

There should be 2 tables called **"SignInLogs"**  and **"AuditLogs"**  ➜ so we’ll search for them:


WE’LL COME BACK TO THIS!!!!!!!!!!!!!!!!!!!!!


<br>

<br>

<br>

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Create a Dummy User</h2> </summary>
<br>

> We’ll go back to Microsoft Entra ID and create a User called **"dummy_user"**.
>
> Doing this should generate an Audit Log.

<br>

Go to **"Microsoft Entra ID"** ➜ and click on the **"Users"** blade:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

We’ll Add a User by clicking on **"Create a user"**

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

- We can Name it ```dummy_user```
- Copy and Save the **Auto-generated Password**
- Click **"Review + create"** to Create the New User:

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

Once the New User is Created ➜ we'll open a **New Private Browsing Tab** ➜ And go to **portal.azure.com**

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

Now we'll attempt to Log in with the New User's Credentials.

<br>

>   <details close> 
>   
> **<summary> 💡 </summary>**
>   
> Creating the User should have generated an **AuditLog**
> 
> And the attempting to Sig In with the User's crendentials should generated a **SignInLog**
> 
>   </details>

<br>

It'll have us change our **Password** ➜ so we'll just change it to ```Cyberlab123!```

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

✅ So we were able to Log In as the **Dummy User**:

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

  </details>

<h2></h2>

<details close> 
<summary> <h2>4️⃣ Assign the Dummy User the Role of Global Administrator</h2> </summary>
<br>

>   <details close> 
>   
> **<summary> 💡 Summary</summary>**
>   
> If you remember from the previous lab ➜ it is a big deal to assign someone in your company the **Global Administrator Role**.
>   
> Usually that will envolve some kind of change-management and more than one person overseeing it.
> 
> Or at least it will envolve more than one person knowing that that assignment is going on.
> 
>   </details>

<br>

Go back to the **"Users"** page in the Azure Portal ➜ and click on the ```dummy_user```

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

💡 Assigning **Global Admin** to this Dummy User should generate another **AuditLog**

So we can go to the **"Assigned roles"** blade ➜ and click on ➕ **Add assignments**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

search for ```global administrator``` ➜ select ☑️ **Global Administrator** ➜ and **"Add"** the Role:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ We can verify that the Dummy User was succcessfully assigned the **Global Administrator Role**:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

  </details>

<h2></h2>

<details close> 
<summary> <h2>5️⃣ Delete the Dummy User</h2> </summary>
<br>

>   <details close> 
>   
> **<summary> 💡 Summary</summary>**
>   
> This will generate another **Audit Log**.
>   
> We should be able to eventually see all thsi actions we took in side of our Log analytics Workspace.
> 
>   </details>

<br>

We'll just go back to the **"Microsoft Entra ID"** page ➜ clik on the **"Users"** Blade

Then inside the **"dummy_user"** ➜ **"Overview"** Blade ➜ click on 🗑️ **Delete** ➜ Press **"OK"**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ That should have also created an **Audit Log** in the AuditLogs table.

  </details>

<h2></h2>

<details close> 
<summary> <h2>6️⃣ Observe AuditLogs in Log Analytics Workspace</h2> </summary>
<br>

Let's go back to our Log Analytics Workspace ```LAW-Cyber-Lab-01```

Clik on the **"Logs"** Blade ➜ and we'll Run the Query ```AuditLogs```

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ We can basically see the "trail" of what we did with the Dummy User:
1. Add User ➜ Created the Dummy User

2. Change User Password ➜ Changed the Password of the Dummy User after Login In using an Incognito Window.

3. Add member to role ➜ Assigned the Global Administrator Role to the Dummy User

4. Delete user ➜ Deleted the Dummy User

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>7️⃣ Simulate Brute Force Attack against Microsoft Entra ID</h2> </summary>
<br>

> We're going to Create an "Attacker User" to perform Brute-Force Attacks against our Microsoft Entra ID.
> 
> We'll then attempt to login 10 times from the Portal just to Generate some Logs and then we'll inspect those Failed SigInLogs.

<br>

Go to **"Microsoft Entra ID"** ➜ clik on the **"Users"** Blade

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

 We're then going to **"Create new User"**:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

- We can name this New User ```attacker```
- Copy and Save the **Auto-generated Password**
- Click **"Review + create"**

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

Once the New User is Created ➜ we'll open a **New Private Browsing Tab** ➜ And go to **portal.azure.com**

![image](https://github.com/user-attachments/assets/b7eb6fe6-f0ba-421c-8b9b-51baf0f9f958)

We'll attempt to properly Log in once with the Credentials of the ```attacker``` user:

![image](https://github.com/user-attachments/assets/1b5478ad-fe53-4e52-b17e-e022ad75e1ca)

And then we'll **Reset the Password** to ```Cyberlab123!``` and Sign In.

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

We can confirm that we Successfully Signed In with the User **"attacker"**

We can then close the Browsing Window ➜ and that in it of itself will terminate the session (Log out)

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

We're now going to Fail some Sign Ins with this **"attacker"** User on purpose.

Open another **Private Browsing Window** ➜ And go to **portal.azure.com**

We'll attempt to Login 10 times with a **Wrong Password**:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

After that ➜ try to Login with the **Correct Password** ➜ to **Generate a Successfuly SignInLog**:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

  </details>

<h2></h2>

<details close> 
<summary> <h2>8️⃣ Observe SigninLogs in Log Analytics Workspace</h2> </summary>
<br>

We'll now go back to our **Log Analytics Workspace** ➜ and check the ```SignInLogs``` ➜ **Run the Query**

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

✅ You can go through the **"ResultDescription"** Tab an Identify all the **Invalid** Login Attempts:

![azure portal](https://github.com/user-attachments/assets/42c1fe46-b2c3-4330-8a86-bd32748cb890)

<h2></h2>

  </details>

<br>

<br>

<br>

# Summary

<br>

This was a long Lab, but it was very important to understand the AuditoLogs and the SignInLogs that were coming in from **Microsoft Entra ID**.

<br>

In the Next Lab we're going to start looking at the **Subscription Level Logs** ➜ which is going to be the **Activity Log**.

The Activity Logs involve all the creating and changing of Resources inside of the Azure Portal.


<br />

<br />

<br />  

<br /> 

<br />

<br />  

<br /> 

<br />

<br />

 
