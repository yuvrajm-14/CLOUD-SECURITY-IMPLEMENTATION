![image](https://github.com/user-attachments/assets/47b8f315-5774-418c-957d-c5158d73dedb)![image](https://github.com/user-attachments/assets/5361e89b-c683-447a-9da9-1270638484fe)![image](https://github.com/user-attachments/assets/83ad29b6-3f4e-4c02-ae3b-c4d1cfbad005)# CLOUD-SECURITY-IMPLEMENTATION

**COMPANY**: CODTECH IT SOLUTIONS

**NAME**: YUVRAJ VISHVANATH MUDLIYAR

**INTERN ID**: CODHC203

**DOMAIN**: CLOUD COMPUTING

**DURATION**: 12 WEEKS

**MENTOR**: NEELA SANTOSH

#DESCRIPTION

STEP 1: Create IAM Users and Groups (AWS Console)
   1.1 Log in to AWS Console
•	Open your browser and go to: https://aws.amazon.com/console/
•	Click “Sign in to the Console”.
•	Enter your AWS account credentials.

   1.2 Go to IAM Service
•	In the AWS Management Console, go to the search bar at the top.
•	Type “IAM” and click on IAM (Identity and Access Management) to open the dashboard.

   1.3 Create IAM Groups
1.	In the left sidebar, click on “User groups”.
2.	Click the “Create group” button.
3.	Enter a Group name, such as:
o	AdminGroup – for full access
o	DeveloperGroup – for limited access to services
o	AuditGroup – for read-only access
4.	(Optional) Attach predefined policies like:
o	AdministratorAccess for Admins
o	ReadOnlyAccess for Auditors
o	Or leave it blank and attach policies later
5.	Click “Create group”.

   1.4 Create IAM Users
1.	In the left sidebar, click on “Users”.
2.	Click “Add users” (top right).
3.	Enter a user name, such as:
o	demoUser1
o	demoUser2
4.	Under Access type, check:
Check the  “AWS Management Console access”
o	Choose a custom password (easy for testing), or auto-generate one
o	Optionally: require password reset at next login
5.	Click Next to proceed to permissions.

   1.5 Assign Users to Groups
1.	On the “Set permissions” page, select:
o	“Add user to group”
2.	Check the box for the group you created (e.g., DeveloperGroup)
3.	Click Next -> See the Review Page.
4.	Click on Create user
5.	Download the credentials

   1.6 Enable MFA for Users (Users Should Compulsorily Enable MFA Once they get the User Credentials)
1.	Go to IAM → Users → Click on a user name
2.	Go to “Security credentials” tab
3.	Under Multi-factor authentication (MFA), click “Assign MFA device”
4.	Choose Virtual MFA device (like Google Authenticator)
5.	Follow the steps to scan the QR code and complete setup

STEP 2:  Secure Storage Setup (S3 Bucket Configuration)
We’ll now create a secure S3 bucket, block public access, and set security policies.

   2.1 Go to S3 Service
1.	In the AWS Console, search for “S3” and click to open it.
2.	Click “Create bucket”.

   2.2 Create a New S3 Bucket
1.	Region: Choose a nearby region (e.g., us-east-1) on top right corner
2.	Bucket name: demo-secure-bucket (or any unique name)
3.	Block Public Access:
o	 Check “Block all public access”
o	 This is important to prevent unauthorized access
4.	Leave the rest as default or modify as needed
5.	Click “Create bucket”

2.3 Enable Bucket Versioning (Optional but Recommended)
1.	In S3 → Click on your bucket → Go to “Properties”
2.	Scroll to “Bucket Versioning” → Click Edit
3.	Select Enable
4.	Click Save changes

2.4 Add a Bucket Policy (Force HTTPS Only)
1.	Go to Permissions tab of your bucket
2.	Click Bucket Policy
3.	Paste the following policy:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSSLRequestsOnly",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::demo-secure-bucket-task-4",
        "arn:aws:s3:::demo-secure-bucket-task-4/*"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "false"
        }
      }
    }
  ]
}
4.	Click Save
This ensures that only HTTPS traffic is allowed.

2.5 Upload Dummy Files
1.	In your bucket → Click “Upload”
2.	Add files like:
o	test.txt
3.	Click Upload

STEP 3: Data Encryption (S3 + AWS KMS)
In this step, we will configure data encryption to secure your S3 bucket:
1.	Encryption at Rest using AWS-managed or customer-managed keys.
2.	Encryption in Transit via HTTPS (this was already enforced in Step 2).

   3.1 Encryption at Rest — S3 Bucket-Level
AWS provides three encryption options for your S3 bucket:
•	SSE-S3: AWS handles encryption keys automatically (simplest option).
•	SSE-KMS: Uses AWS Key Management Service (KMS), providing more control and flexibility.
•	SSE-C: You manage your own encryption keys (for advanced use cases).
For best practice and flexibility, we will use SSE-KMS.

   3.1.1 Go to Your S3 Bucket
1.	Open the S3 Console → Buckets.
2.	Select your bucket: demo-secure-bucket-task-4.

   3.1.2 Enable Default Encryption (SSE-KMS)
1.	In your S3 bucket, go to the “Properties” tab.
2.	Scroll down to the “Default encryption” section.
3.	Click “Edit”.
4.	Choose the following:
o	 Enable to turn on encryption.
o	Select AWS Key Management Service key (SSE-KMS).
	Option 1: Use the AWS-managed key (aws/s3) — AWS handles key management automatically.
	Option 2: Create a Customer-managed KMS key (CMK) if you require full control (this option allows key rotation, auditing, and custom permissions).
5.	Click Save changes to apply the encryption setting.
   
   3.2 (Optional) Create a Customer-Managed KMS Key (CMK)
If you need more control over your encryption keys, you can create a Customer-Managed KMS Key (CMK). This gives you the ability to manage key policies, rotate keys, and control access to your keys.
🔧 Steps to Create a CMK:
1.	Go to the AWS Key Management Service (KMS) from the AWS Console.
2.	Click on “Create key”.
3.	Choose:
o	Symmetric (for encryption/decryption).
o	Encrypt and decrypt (set the key usage).
4.	Name the key: demo-kms-key (or any name you prefer).
5.	Add your IAM users or groups that should have permissions to use this key.
6.	Finish the key creation process.
7.	Go back to the S3 bucket settings and select this newly created key in the SSE-KMS encryption options.

   3.3 Test Encryption Works
To verify that the encryption is working, follow these steps:
1.	Go to your S3 bucket.
2.	Click Upload and select a file (e.g., encrypted-file.txt).
3.	Once the file is uploaded, click on the file name to view the object details.
4.	In the Properties section, under Encryption, confirm that it shows:
o	SSE-KMS (indicating that the file is encrypted using KMS).
o	The KMS Key ID should also be displayed, confirming the key used for encryption.

 3.4 Encryption in Transit (Already Enforced)
In Step 2.4, you already added a bucket policy to deny non-HTTPS traffic. This ensures that data in transit is always encrypted using HTTPS.
The policy in Step 2.4 looks like this:
"Condition": {
  "Bool": {
    "aws:SecureTransport": "false"
  }
}
This forces all communication to your bucket to be encrypted in transit. You're all set with this configuration.

STEP 5: VERIFY CONFIGURATION (Optional But Good Practice)
•	Go to CloudTrail → Check logs for user actions
•	Use IAM Access Analyzer to detect public access
•	Use S3 Access Analyzer to confirm no external access

STEP 6: WRITE A REPORT
Step 1: IAM Policies & User Setup
🧾 Actions Taken:
•	Accessed the IAM Dashboard in the AWS Console.
•	Created IAM user groups:
o	AdminGroup
o	DeveloperGroup
o	AuditGroup
•	Created individual IAM users (e.g., demoUser1, demoUser2).
•	Assigned users to the appropriate groups based on their roles (Admin, Developer, Auditor).
•	Enabled AWS Management Console access with custom passwords for each user.
•	(Optional) Set up Multi-Factor Authentication (MFA) for additional security.

Outcome:
•	The root account is no longer used for daily operations, enhancing security.
•	Users are granted role-based, least-privilege access according to their job responsibilities.

Step 2: Secure Storage Setup (S3)
Actions Taken:
•	Created a secure S3 bucket named demo-secure-bucket-task-4.
•	Enabled Block all public access to ensure the bucket remains private.
•	Enabled Bucket Versioning to facilitate object recovery and tracking of changes.
•	Added a bucket policy to enforce HTTPS-only access:
json
CopyEdit
"Condition": {
  "Bool": {
    "aws:SecureTransport": "false"
  }
}
•	Uploaded test files (e.g., test.txt, sample.json) for validation and permission testing.
•	Enabled Server Access Logging, sending logs to a separate log bucket named demo-logs-bucket.

Outcome:
•	The S3 bucket and its contents are private, secure, and monitored.
•	All uploads and downloads are restricted to secure HTTPS connections only.
•	Access activity is logged, providing a record of operations for auditing purposes.

Step 3: Data Encryption (S3 + KMS)
Actions Taken:
•	Enabled SSE-KMS encryption on the S3 bucket for encryption at rest.
o	Used the AWS-managed key (aws/s3).
o	(Optional) Created and used a custom KMS key (demo-kms-key) for more control over encryption.
•	Verified that objects uploaded to the bucket are encrypted at rest.
•	Ensured encryption in transit is enforced by the HTTPS-only bucket policy added in Step 2.

Outcome:
•	All data at rest is securely encrypted using AWS KMS.
•	All transfers to/from the S3 bucket are encrypted using HTTPS, ensuring data security in transit.

Step 5: Verify Configuration (Optional But Good Practice)
Actions Taken:
•	Checked CloudTrail logs for user actions and activities to ensure compliance and track usage.
•	Used IAM Access Analyzer to detect any public access settings for IAM resources.
•	Used S3 Access Analyzer to confirm that no external access is allowed to the S3 bucket.

Outcome:
•	CloudTrail logs provided a detailed history of user actions for auditing purposes.
•	IAM Access Analyzer confirmed that no IAM resources had public access.
•	S3 Access Analyzer validated that the bucket did not allow any unintended external access, ensuring its security.



#OUTPUT

![image](https://github.com/user-attachments/assets/44e50928-de5e-4b9d-bac5-53527678745c)


![image](https://github.com/user-attachments/assets/bcd4aa40-cbf6-4adb-aed1-bab2ea12a912)


![image](https://github.com/user-attachments/assets/b125a8b1-6dda-4c6c-a9fa-a834da34f94d)

![image](https://github.com/user-attachments/assets/676dd501-ac32-4599-a695-1542727a8877)

![image](https://github.com/user-attachments/assets/eef1ecdf-5a69-4b65-a317-6741f4a31fd3)

![image](https://github.com/user-attachments/assets/413078dc-a296-48fa-bb23-22f909224387)

![image](https://github.com/user-attachments/assets/164c4b2c-8d06-465e-baca-5f0cf08feef5)

![image](https://github.com/user-attachments/assets/54b78047-18f5-41f4-a2b0-94152a93e071)



![image](https://github.com/user-attachments/assets/759aea5c-8aa9-4453-ae76-d17558832d04)


![image](https://github.com/user-attachments/assets/8ebe2adb-8349-4c20-9103-529428df4286)


![image](https://github.com/user-attachments/assets/c93a050b-2401-4a08-824a-8226625b5871)


![image](https://github.com/user-attachments/assets/9db6e864-5ffc-427d-947e-383dea52b7af)



![image](https://github.com/user-attachments/assets/6412739a-fa8b-45dc-b3c2-c2f85c7158d5)
