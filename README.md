# Understanding ACL Disabled in AWS S3 with Example Task

This guide explains what **ACL Disabled** means in Amazon S3 and how access is managed using **Bucket Policies** instead of Access Control Lists (ACLs). It then walks through a detailed hands-on example of applying folder-specific permissions using policies in JSON format.

---

## 🧩 What Does ACL Disabled Mean?

When you **disable ACLs** while creating an S3 bucket, you remove the ability to manage access through the legacy *Access Control List (ACL)* mechanism.  
Instead, all access control is handled exclusively through **IAM policies** and **bucket policies**.

## 🧠 Key Takeaways
- **ACL Disabled** shifts control entirely to bucket policies.  
- **Effect** defines allow/deny behavior.  
- **Action** defines specific S3 operations (Get, Put, Delete).  
- **Principal** defines who the rule applies to (`*` = everyone).  
- **ARN** uniquely identifies your bucket or object.

This means:

  <img width="621" height="435" alt="image" src="https://github.com/user-attachments/assets/5081f67b-e8bb-4b6c-8e4d-ca5a31229843" />

- No per-object permissions via ACL checkboxes.
- Access is governed only by **policies** in JSON format.
- Simplifies security by following the **“one source of truth”** principle for permissions.

In short, when ACLs are disabled, **you control access through code**, not by toggling settings.

---

## 🧰 Why Use ACL Disabled?
1. **Centralized Access Management:** One unified policy defines all permissions.
2. **Improved Security:** Reduces accidental public exposure.
3. **Recommended by AWS:** Modern best practice for access control.
4. **Automation-Friendly:** JSON-based control can be versioned and replicated.

---

## 🧪 Example Task: Managing Folder-Level Access with ACL Disabled

Let’s put this into practice by creating a bucket with specific folder permissions using **Bucket Policies**.

---

## 🪣 Step 1: Create the Bucket
1. Open **AWS Management Console → S3**.
2. Click **Create bucket**.
3. Enter a **unique bucket name** (must be globally unique).
4. Select **General Purpose** as the bucket type.
5. Under **Object Ownership**, choose **ACLs disabled (recommended)**.
6. Scroll down, uncheck **Block all public access**, and **acknowledge** the warning.
7. Click **Create bucket**.

---

## 🗂 Step 2: Create Folders and Upload Files
Create the following folders inside your bucket:
- `images`
- `mydata`
- `myfiles`
- `videos`

Upload **two Objects** inside each folder.

---

## ⚙️ Step 3: Define Access Rules

| Folder | Rule |
|:--|:--|
| `images` | One object must be private |
| `mydata` | Entire folder private |
| `myfiles` | Files cannot be deleted |
| `videos` | No new uploads allowed |

---

## 🧾 Step 4: Create Bucket Policies

We’ll create **five policies** to enforce these rules.

1. Go to **Permissions → Bucket Policy → Edit**.  
2. Copy your **Bucket ARN** (example: `arn:aws:s3:::bucket4501`).  
3. Click **Policy Generator** to create the JSON policies.

### **Policy 1 – Make All Objects Public**
```
Effect: Allow
Principal: *
Action: s3:GetObject
Resource: arn:aws:s3:::bucket-name/*
```

### **Policy 2 – Make One Image Private**
```
Effect: Deny
Principal: *
Action: s3:GetObject
Resource: arn:aws:s3:::bucket-name/images/private-image.jpg
```

### **Policy 3 – Make MyData Folder Private**
```
Effect: Deny
Principal: *
Action: s3:GetObject
Resource: arn:aws:s3:::bucket-name/mydata/*
```

### **Policy 4 – Prevent Deletion in MyFiles**
```
Effect: Deny
Principal: *
Action: s3:DeleteObject
Resource: arn:aws:s3:::bucket-name/myfiles/*
```

### **Policy 5 – Prevent Uploads in Videos**
```
Effect: Deny
Principal: *
Action: s3:PutObject
Resource: arn:aws:s3:::bucket-name/videos/*
```

Once all are added, click **Generate Policy**, copy the output JSON, and paste it into the **Bucket Policy Editor**.  
Click **Save changes**.

---

## 🔍 Step 5: Verify Policies
Test your configuration:
- Access the **public image** (should open).  
- Try opening the **private image** or **mydata files** (should be denied).  
- Try **deleting files** in `myfiles` (should fail).  
- Try **uploading** to `videos` (should fail).

---

## ✅ Final Outcome
You’ve successfully:
- Created an **ACL-disabled** S3 bucket.  
- Used **JSON-based policies** for fine-grained access control.  
- Enforced **folder-specific permissions** without using ACLs.

This method ensures cleaner, safer, and modern AWS S3 access management.

## Author
**Rushikesh Dase**  
Cloud Computing Enthusiast | AWS Learner  
📧 daserushikesh@gmail.com  
🔗 https://www.linkedin.com/in/rushi-dase  
**Category:** | AWS | Cloud Computing | EC2 | VPC | Load Balancing | RDS | S3 |
