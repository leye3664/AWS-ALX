 Lab overview and objectives

In this lab, I used Amazon Simple Storage Service (Amazon S3) to build a static website and implement architectural best practices to protect and manage your data.

After completing this lab, I am able to:

•	Host a static website by using Amazon S3

•	Implement one way to protect your data with Amazon S3

•	Implement a data lifecycle strategy in Amazon S3

•	Implement a disaster recovery (DR) strategy in Amazon S3

At the end of this lab, my architecture looked like the following example:

![Screenshot 2023-11-18 222503](https://github.com/leye3664/AWS-ALX/assets/85311688/55f6d153-458c-43b0-8bb5-188d4b893590)

A business request for the café: Launching a static website (Challenge #1)
Sofía mentions to Nikhil that she would like the café to have a website that will visually showcase the café's offerings. It would also provide customers with business details, such as the location of the store, business hours, and telephone number.
Nikhil is happy that he was asked to create the first website for the café.
For this first challenge, i will take on the role of Nikhil and use Amazon S3 to create a basic website for the café.

        Task 1: Extracting the files that you need for this lab
1.First  I downloaded the files. Notice that i have an index.html file, and two folders that contain Cascading Style Sheets (CSS) and image files.
  
        Task 2: Creating an S3 bucket to host your static website
In this task, I created an S3 bucket and configured it to host your static website.
2.	Open the Amazon S3 console.

3. I	Created a bucket to host my static website.
o	I Created the bucket in the N. Virginia (us-east-1) AWS Region.

o	Tip: I disabled Block all public access.

4.	Enable static website hosting on your bucket.
o	Tip: I used the index.html file for your index document.


        Task 3: Uploading content to your S3 bucket
In this task, I uploaded the static files to your S3 bucket.

5.	I Uploaded the index.html file and the css and images folders to your S3 bucket.

6.	In a separate web browser tab, I opened the endpoint link for my static website.




        Task 4: Creating a bucket policy to grant public read access
Frank shares his plan to create many new types of pastries for the café. I realized that I will need to upload an image for each new dessert that he creates, and enable public access on that object. I don't want to do this process manually. Instead, I decided to create a bucket policy that automatically makes each object public when it's uploaded to the folder.

7.	I Created a bucket policy that grants read-only permission to public anonymous users by using the Bucket Policy editor.


{
  "Version": "2012-10-17",
  
  "Statement": [

    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}

Hint: If you get stuck, refer to the examples in the AWS Documentation.

8.	Confirm that the website for the café is now publicly accessible.

Congratulations! I now have a static website for the café.
![Screenshot (42)](https://github.com/leye3664/AWS-ALX/assets/85311688/7800837e-3c03-4a5c-9727-298a954b7dce)


New business requirement: Protecting website data (Challenge #2)
I showed Sofía the new website, and she's very impressed. Good job!
I and Sofía discussed that I will likely need to make many updates to the website as the number of café offerings expands.
Olivia, an AWS Solutions Architect and café regular, advises me to implement a strategy to prevent the accidental overwrite and deletion of website objects.
I already need to make some changes to the website, so I decide that this would be a good time to explore object versioning.

      Task 5: Enabling versioning on the S3 bucket
In this task, I will enable versioning on my S3 bucket and confirm that it works.

9.	In the S3 console, I enabled versioning on my S3 bucket.
Note: Notice that after you enable versioning, you can't disable it.

10.	In your favorite text editor, open the index.html file. For example, you could use Notepad++ or TextWrangler.

11.	I Modified the file according to the following instructions:

o	Locate the first line that has the embedded CSS code bgcolor="aquamarine" in the HTML, and change it to bgcolor="gainsboro".


![Screenshot (43)](https://github.com/leye3664/AWS-ALX/assets/85311688/ef31f64d-1cf5-4200-9879-3e0f45c80a83)

o	Locate the line that has the embedded CSS code bgcolor="orange" in the HTML, and change it to bgcolor="cornsilk".

o	Locate the second line that has the embedded CSS code bgcolor="aquamarine" in the HTML, and change it to bgcolor="gainsboro".

![Screenshot (45)](https://github.com/leye3664/AWS-ALX/assets/85311688/53891328-bc0f-441d-8b90-00e055aeec42)


o	I Saved the changes.

12.I Uploaded the updated file to your S3 bucket.

13.I Reloaded the web browser tab with my website and notice the changes.

14.To see the latest version of the index.html file, go to your bucket and choose List versions. You should see both versions of this file in the dropdown menu.


Architecture best practice

In this task, I used one technique to implement the architecture best practice of protecting your data.
According to the Well-Architected Framework, versioning can be part of a larger data lifecycle management process. Before you architect any system, foundational practices that influence security should be in place. For example, data classification provides a way to categorize organizational data based on levels of sensitivity. Encryption protects data by rendering it unintelligible to unauthorized access. These tools and techniques are important because they support objectives such as preventing financial loss or complying with regulatory obligations.



New business requirement: Optimizing costs of S3 object storage (Challenge #3)
Now that I enabled versioning, I realized that the size of the S3 bucket will continue to grow as you upload new objects and versions. To save costs, I decided to implement a strategy to retire some of those older versions.


          Task 6: Setting lifecycle policies
In this task, I will set a lifecycle policy to automatically move older versions of the objects in your source bucket to S3 Standard-Infrequent Access (S3 Standard-IA). The policy should also eventually expire the objects.


15.I Configured two rules in the website bucket's lifecycle configuration. I created two separate rules. Do not configure two transitions in a single rule:

o	In one rule, move previous versions of all source bucket objects to S3 Standard-IA after 30 days

o	In the other rule, delete previous versions of the objects after 365 days

Hint: If you get stuck, refer to the AWS Documentation for guidance.

Note: To limit the scope of the replication to a particular bucket object (for example, the index.html file), you create a tag for the object before you create the lifecycle rule.
Good! You should now have a lifecycle configuration that will move previous versions of your source bucket objects to S3 Standard-IA after 30 days. The policy will also permanently delete the objects that are in S3 Standard-IA after 365 days.


{
  
  "Rules": [
   
    {
      "ID": "TransitionToS3StandardIA",
      "Status": "Enabled",
      "Filter": {
        "Tag": {
          "Key": "Retention",
          "Value": "30days"
        }
      },
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "STANDARD_IA"
        }
      ]
    }
  ]
}



{
  "Rules": [
   
    {
      "ID": "DeletePreviousVersions",
      "Status": "Enabled",
      "Filter": {
        "Prefix": "",
        "Tag": {
          "Key": "Versioning",
          "Value": "Previous"
        }
      },
      "Expiration": {
        "Days": 365
      }
    }
  ]
}


Architecture best practice
In this task, I implemented the architecture best practice of defining data lifecycle management.

According to the Well-Architected Framework, in practice, your lifecycle strategy should be based on the criticality and sensitivity of your data, and legal and organizational requirements. You should consider factors such as data retention duration, data destruction, data access management, data transformation, and data sharing.

New business requirement: Enhancing durability and planning for DR (Challenge #4)
The next time Olivia comes to the café, I would tell her about the updates to the website. I described the measures that I took to protect the website's static files from being accidentally overwritten or deleted. Olivia tells me that cross-Region replication is another feature of Amazon S3 that  can also be used to back up and archive critical data.

                  Task 7: Enabling cross-Region replication
In this task, I will enable cross-Region replication on your source S3 bucket.

16.	In a different Region than my source bucket, I created a second bucket and enabled versioning on it. The second bucket is my destination bucket.

17.	On my source S3 bucket, I enabled cross-Region replication. I created the replication rule, I made sure that :

o	I Replicated the entire source bucket.

o	I Used the CafeRole for the AWS Identity and Access Management (IAM) role. This IAM role gives Amazon S3 the permissions to read objects from the source bucket and replicate them to the destination bucket.

o	If you encounter the warning The replication rule is saved, but it might not work, you can ignore it and proceed to the next step.

Hint: If you get stuck, refer to the AWS Documentation for guidance.

Note: CafeRole has the following permissions:


Version: 2012-10-17
Statement:
  - Action:
  - s3:ListBucket
  - s3:ReplicateObject
  - s3:ReplicateDelete
  - s3:ReplicateTags
  - s3:Get*
    Resource:
  - '*'
    Effect: Allow




This access policy allows the role to perform the replication tasks on all S3 buckets. In a real production environment, you should restrict the policy to apply only to your source and destination S3 buckets. For more information about creating an IAM role, refer to Setting Up Permissions for Replication.

18.	I Made a minor change to the index.html file and uploaded the new version to your source bucket.

19.	I Verified that the source bucket now has three versions of the index.html file.

20.	I Confirmed that the new object was replicated to my destination bucket. I reloaded the browser tab.

21.	I went to my source bucket and deleted the latest version.

Architecture best practice

In this task, I implemented the architecture best practice of automating disaster recovery.
According to the Well-Architected Framework, the start of your DR strategy is having backups and redundant workload components in place. You should use AWS or third-party tools to automate system recovery and route traffic to the DR site or Region.


![Screenshot (47)](https://github.com/leye3664/AWS-ALX/assets/85311688/a1256b6a-0c9f-4e8a-b8a2-adc8449b72e1)
