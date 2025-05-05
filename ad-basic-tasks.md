<h1 id="top"> 🧩 Active Directory: Basic Tasks Walkthrough (Beginner Edition) </h1>

Welcome! Active Directory can feel like quantum mechanics from the outside looking in, so we’ll keep things super simple in this walkthrough. You’ll perform some beginner-friendly but essential Active Directory tasks inside the lab environment you *should* have already set up. If you haven't completed the [Lab Setup Guide](../lab-setup-guide.md), I recommend starting there first.

This walkthrough assumes **zero Active Directory experience** (like I had before this) and is designed to guide you slowly and clearly through common tasks. You’ll be working inside your Windows Server 2019 VM (`DC01`) and your Windows 10/11 client machine (`CLIENT01`), both of which should already be part of the same domain.

---

## 📜 Table of Contents

- [📁 Step 1. Create Organizational Units (OUs)](#-step-1-create-organizational-units-ous)
- [👤 Step 2. Create User Accounts](#-step-2-create-user-accounts)
- [👥 Step 3. Create Security Groups](#-step-3-create-security-groups)
- [🔗 Step 4. Add Users to Groups](#-step-4-add-users-to-groups)
- [🧠 Step 5. Create and Link a GPO](#-step-5-create-and-link-a-gpo)
- [🖥️ Step 6. Move CLIENT01 into the Appropriate OU](#step-6)
- [🧪 Step 7. Test the GPO](#-step-7-test-the-gpo)
- [📦 Wrapping Up](#-wrapping-up)
- [💬 Questions or Feedback?](#questions-or-feedback)

---

## 📁 Step 1. Create Organizational Units (OUs)

OUs (Organizational Units) help organize users, groups, and computers into logical containers. You’ll start by creating two OUs: one for IT Support and one for HR.

1. Open **Server Manager** → **Tools** → **Active Directory Users and Computers**.

![Users and Computers](images/ad-tasks/ou/01-create-ou.PNG)

2. In the left panel, expand your domain (`corp.local`).
3. Right-click `corp.local` → **New** → **Organizational Unit**.

![New OU](images/ad-tasks/ou/02-new-ou.PNG)

4. Name it: `Departments`, and click OK.
   - 🔒 **Best practice**: Leave **"Protect container from accidental deletion"** checked. For *lab* purposes, however, you're the boss.

![Departments OU](images/ad-tasks/ou/03-departments.PNG)

5. Right-click `Departments` → **New** → **Organizational Unit** again.
   - 💡 **Tip**: You can also left click `Departments` and right-click in the detail window to bring up the menu. I'll show you in the screenshot.

![New OU, again](images/ad-tasks/ou/04-department-ou.PNG)

6. Use this process to create two sub-OUs:
   - `HR` (everyone's favorite...)
   - `ITSupport` (should be everyone's favorite)

<p float="left">
  <img src="images/ad-tasks/ou/05-department-hr.PNG" alt="HR Sub-OU" width="400" />
  <img src="images/ad-tasks/ou/06-department-itsupport.PNG" alt="ITSupport Sub-OU" width="400" />   
</p>

🎉 You now have a clean structure for organizing your user accounts and departments.

[🔝 Back to Top](#top)

---

## 👤 Step 2. Create User Accounts

Let’s create one user for each department.

1. In **Active Directory Users and Computers**, right-click `HR` → **New** → **User**.
   - 💡 **Tip**: Remember, you can also left click `HR` and right-click in the detail window to bring up the menu. I'll stop pestering you with reminders going forward. 

![New HR User](images/ad-tasks/user/01-new-hr-user.PNG)

2. Use these details:
   - **First name**: `John`
   - **Last name**: `Smith`
   - **User logon name**: `jsmith`
3. Click `Next`.

![John Smith](images/ad-tasks/user/02-jsmith.PNG)

4. Set a password.
   - Don't forget this one. Maybe write it down this time...
   - 🔒 **Best practice**: Leave “User must change password at next logon” checked. This is a security habit used in real-world environments. For *lab* purposes, however, you're the boss. 
5. Click `Next`, then click `Finish`.

![Set Password](images/ad-tasks/user/03-jsmith-pass.PNG)

6. Repeat for the `ITSupport` OU:

![New HR User](images/ad-tasks/user/05-new-itsupport-user.PNG)

7. Use these details:
   - **First name**: `Jane`
   - **Last name**: `Doe`
   - **User logon name**: `jdoe`
8. Click `Next`.

![Jane Doe](images/ad-tasks/user/06-jdoe.PNG)

![Set Password](images/ad-tasks/user/07-jdoe-pass.PNG)

🎉 Way to go Neo. You've just mastered creating new users in Active Directory!

[🔝 Back to Top](#top)

---

## 👥 Step 3. Create Security Groups

Groups are used to manage permissions or apply policies to multiple users at once. Let's create some groups!

1. In **Active Directory Users and Computers**, right-click `HR` → **New** → **Group**.

![New HR Group](images/ad-tasks/groups/01-new-hr-group.PNG)

2. Use these details:
   - Group name: `Managers`
   - Group scope: `Global`
   - Group type: `Security`
4. Click `OK`.

![Managers Group](images/ad-tasks/groups/02-name-managers-group.PNG)

🧑‍💻 Repeat for `ITSupport`:

![New HR Group](images/ad-tasks/groups/03-new-itsupport-group.PNG)

- Use these details:
   - Group name: `HelpDesk`
   - Group scope: `Global`
   - Group type: `Security`
- Click `OK`.

![Managers Group](images/ad-tasks/groups/04-name-helpdesk-group.PNG)

🎉 It's starting to look like you know what you're doing! I'm sure glad one of us does...

[🔝 Back to Top](#top)

---

## 🔗 Step 4. Add Users to Groups

Let’s assign our users to their department groups.

1. Still in **Active Directory Users and Computers**, double-click `jsmith` in the `HR` OU.

![Double Clicky](images/ad-tasks/groups/05-double-click-jsmith.PNG)

2. Go to the **Member Of** tab, then click `Add...`

![Member Of Tab](images/ad-tasks/groups/06-member-of-tab.PNG)

3. Type `Managers`, click `Check Names`, then click `OK`.

![Check Name: Managers](images/ad-tasks/groups/07-managers-check-names.PNG)

![Name Checked: Managers](images/ad-tasks/groups/08-managers-ok.PNG)

4. Click `OK` to close **John Smith Properties** window.

![Close Properties](images/ad-tasks/groups/09-close-jsmith-properties.PNG)

🧑‍💻 Do the same for `jdoe` in `ITSupport`:

![Double Clicky](images/ad-tasks/groups/10-double-click-jdoe.PNG)

![Member Of Tab](images/ad-tasks/groups/11-member-of-tab.PNG)

- Add `jdoe` to `HelpDesk`.

![Check Name: Managers](images/ad-tasks/groups/12-helpdesk-check-names.PNG)

![Name Checked: Managers](images/ad-tasks/groups/13-helpdesk-ok.PNG)

🎉 Now we're cookin'. Keep up the great work, we're almost done!

[🔝 Back to Top](#top)

---

## 🧠 Step 5. Create and Link a GPO

We’ll now create a **Group Policy Object (GPO)** that displays a message when users `HR` users log in. This will help confirm the GPO is working.

1. Open **Server Manager** → **Tools** → **Group Policy Management**.

![Server Manager - Tools](/images/ad-tasks/gpo/01-tools.PNG)

2. Expand `corp.local` → `Departments`.
3. Right-click `HR` → **Create a GPO in this domain, and Link it here...**

![Create a GPO](/images/ad-tasks/gpo/02-create-gpo.PNG)

4. Name it: `HR Login Message`, then click OK.

![Name GPO](/images/ad-tasks/gpo/03-name-hr-gpo.PNG)

5. Right-click the new GPO under `HR` and click `Edit`.

![Edit HR GPO](/images/ad-tasks/gpo/04-edit-hr-gpo.PNG)

6. In the **Group Policy Management Editor**, navigate to:
   - **Computer Configuration** → **Policies** → **Windows Settings** → **Security Settings** → **Local Policies** → **Security Options**
7. Find and double-click **Interactive logon: Message title for users attempting to log on**.

![Interactive Logon:](/images/ad-tasks/gpo/05-message-title.PNG)

8. Set the title to: `HR Notice`

![Interactive Logon:](/images/ad-tasks/gpo/06-set-title.PNG)

9. Find and double-click **Interactive logon: Message text for users attempting to log on**.

![Interactive Logon:](/images/ad-tasks/gpo/07-message-text.PNG)

10. Set the message to: `This system is for HR use only.`

![Set Title](/images/ad-tasks/gpo/08-set-message.PNG)

✔️ This message will show every time a user from the `HR` OU logs into CLIENT01.

🎉 Nicely done! Just think of the tea you could spill around the office with this kind of power. 🤔

[🔝 Back to Top](#top)

---

<h2 id="step-6"> 🖥️ Step 6. Move CLIENT01 into the Appropriate OU </h2>

To make sure GPOs apply correctly, you need to move your Windows 10/11 client machine (`CLIENT01`) into the OU you're working with.

I learned this the hard way...

Follow these steps:

1. In **Active Directory Users and Computers**, expand your domain (`corp.local`) and click on **Computers**.

![AD Users and Computers](/images/ad-tasks/gpo/09-users-and-computers.PNG)

2. In the right panel, right-click `CLIENT01`, the click `Move...`.

![Move CLIENT01](/images/ad-tasks/gpo/10-move-client.PNG)

3. Choose the `HR` OU
4. Click `OK`.

![Select HR](/images/ad-tasks/gpo/11-move-to-hr.PNG)

✅ **Why this matters:** GPOs targeting computers will only work if the computers are in the right OU. Even if the GPO affects users, putting client machines in the correct OU ensures consistent results when testing. 

🎉 Now you're ready to test! 

[🔝 Back to Top](#top)

---

## 🧪 Step 7. Test the GPO

Now let's test that the login message works.

1. On `CLIENT01`, log in as: `CORP\jsmith`.

![Log In](/images/ad-tasks/test/01-jsmith-log-in.PNG)

2. You may see the login message you set in the GPO before reaching the desktop, but if not:
   - Open **Command Prompt** on `CLIENT01` and run: ```gpupdate /force```
   - Restart and try again.

![Force Update](/images/ad-tasks/test/02-force-update.PNG)

3. Now you should see the login message!

![Login Message](/images/ad-tasks/test/03-log-in-message.PNG)

[🔝 Back to Top](#top)

---

## 📦 Wrapping Up

Awesome job! You just completed some of the most common beginner tasks in Active Directory:

- Creating and organizing OUs
- Creating users and groups
- Assigning users to groups
- Creating and linking a Group Policy Object
- Testing the effects of your configuration

🎉 Great job! I was mostly confident you could do it. 😉 

This walkthrough is meant to build confidence and familiarity. Once you're comfortable here, you’ll be ready to explore more advanced tasks—like password policies, folder redirection, or managing permissions.

[🔝 Back to Top](#top)

---

## <h2 id="questions-or-feedback"> 💬 Questions or Feedback? </h2>

If you find anything confusing, run into trouble, or just want to reach and tell me how awesome I am for making this, feel free to [open an issue](https://github.com/learnbuilddeploy/active-directory-home-lab/issues) or email me at learnbuilddeploylabs@gmail.com.

[🔝 Back to Top](#top)