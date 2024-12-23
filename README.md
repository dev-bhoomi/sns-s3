
# Get SNS Notification on S3 File Upload
This setup sends an SNS notification whenever a file is uploaded to an S3 bucket.


## Key Feature

- Real-Time Notifications:
Receive instant notifications whenever a file is uploaded to your S3 bucket.

- Integration with SNS: Uses Amazon SNS to send notifications to email, SMS, or other endpoints.

- Customizable Events: Configure notifications for specific actions like file uploads (PUT), deletions, or updates.

- Multiple Endpoints: Notify multiple recipients or systems (e.g., emails, Lambda, SQS) through a single SNS topic.

- Secure Setup: Ensures only authorized S3 buckets can publish events to SNS.

- Scalable and Reliable: Works seamlessly for high volumes of file uploads in S3 buckets.
## Steps

### **Step 1: Create an SNS Topic**

1. Go to the **SNS Console** in AWS.
2. Click **Create Topic** and choose **Standard**.
3. Name the topic (e.g., `S3UploadTopic`) and click **Create Topic**.
4. **Add a Subscription**:
    - Click **Create Subscription**.
    - Set **Protocol** to `Email`.
    - Enter your email address in the **Endpoint**.
    - Click **Create Subscription**.
5. **Confirm the Subscription**:
    - Check your email and confirm the subscription by clicking the link.

---

### **Step 2: Add Permissions to the S3 Bucket**

1. Go to the **S3 Console** and select your bucket.
2. Add the following bucket policy to allow S3 to send events to SNS:Replace:
    
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Service": "s3.amazonaws.com"
                },
                "Action": "sns:Publish",
                "Resource": "arn:aws:sns:region:account-id:S3UploadTopic"
            }
        ]
    }
    
    ```
    
    - `region` (e.g., `us-east-1`).
    - `account-id` (your AWS account ID).
    - `S3UploadTopic` (your SNS topic name).

---

### **Step 3: Set Up S3 Event Notifications**

1. Go to the **S3 bucket properties**.
2. Scroll to **Event notifications** and click **Create Event Notification**.
3. Set:
    - **Name**: e.g., `FileUploadEvent`.
    - **Event Types**: Choose **PUT** (file upload).
    - **Destination**: Select **SNS Topic** and choose the topic you created.
4. Save the settings.

---

### **Step 4: Test the Setup**

1. **Upload a File**:
    - Upload any file to your S3 bucket.
2. **Check Your Email**:
    - You should get an email notification from SNS about the file upload.
## Services
- S3: Detects file uploads.
- SNS: Sends a notification (email, SMS, etc.).
