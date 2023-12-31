# AWS-ALX
Cloud Project


Static websites have fixed content with no backend processing. They can contain HTML pages, images, style sheets, and all files that are needed to render a website. However, static websites do not use server-side scripting or a database. If you want your static webpages to provide interactivity and run programming logic, you can use JavaScript that runs in the user's web browser.
You can easily host a static website on Amazon Simple Storage Service (Amazon S3) by uploading the content and making it publicly accessible. No servers are needed, and you can use Amazon S3 to store and retrieve any amount of data at any time, from anywhere on the web.



After completing this lab, I am able to:

•	Create a bucket in Amazon S3

•	Upload content to your bucket

•	Enable access to the bucket objects

•	Update the website

                                       Task 1: Creating a bucket in Amazon S3
In this task, I created an S3 bucket and configured it for static website hosting.

1.	In the AWS Management Console, on the Services menu, choose S3.

2.	Choose Create bucket
An S3 bucket name is globally unique, and the namespace is shared by all AWS accounts. After you create a bucket, the name of that bucket cannot be used by another AWS account in any AWS Region unless you delete the bucket.
Thus, for this lab, I will use a bucket name that includes a random number, such as: website-123
    
 3.	For Bucket name, enter: website-<123> (replace <123> with a random number)
Public access to buckets is blocked by default. Because the files in your static website will need to be accessible through the internet, you must permit public access.

o	Verify the AWS Region is set to us-east-1 (if it is not, choose the us-east-1 Region)
     
4.	In the Object Ownership section, select ACLs enabled, then verify Bucket owner preferred is selected.
      
5.	Clear Block all public access, then select the box that states I acknowledge that the current settings may result in this bucket and the objects within becoming public.
6.	Choose Create bucket.
You can use tags to add additional information to a bucket, such as a project code, cost center, or owner.

7.	Choose the name of your new bucket.

8.	Choose the Properties tab.

9.	Scroll to the Tags panel.

10.	Choose Edit then Add tag and enter:

•	Key: Department
•	Value: Marketing

11.	Choose Save changes to save the tag.
Next, I configured the bucket for static website hosting.

12.	Stay in the Properties console.

13.	Scroll to the Static website hosting panel.

14.	Choose Edit

15.		Configure the following settings:

o	 Static web hosting: Enable

o	Hosting type: Host a static website

o	Index document: index.html

	Note: I entered this value, even though it is already displayed.
o	Error document: error.html

16.	Choose Save changes

17.	In the Static website hosting panel, choose the link under Bucket website endpoint.
    I  received a 403 Forbidden message because the bucket permissions have not been configured yet. I Keep this tab open in your web browser so that you can return to it later.


![Screenshot (40)](https://github.com/leye3664/AWS-ALX/assets/85311688/00102b51-e405-4352-bd4d-0bcefd99ddf8)

My bucket has now been configured to host a static website.

                    Task 2: Uploading content to your bucket
In this task, I uploaded the files that will serve as your static website to the bucket.

18.	I Right-click each of these links and download the files to your computer:
Ensure that each file keeps the same file name, including the extension.
o	index.html
o	script.js
o	style.css

19. I	Returned to the Amazon S3 console and in the website-<123> bucket you created earlier, choose the Objects tab.

20.	Choose Upload

21.	Choose Add files

22.	Locate and select the three files that I downloaded.

23.	If prompted, choose I acknowledge that existing objects with the same name will be overwritten.

24.	Choose Upload
Your files are uploaded to the bucket.
o	Choose Close

                                  Task 3: Enabling access to the objects
Objects that are stored in Amazon S3 are private by default. This ensures that your organization's data remains secure.
In this task, I made the uploaded objects publicly accessible.
First, confirm that the objects are currently private.

25. I	Returned to the browser tab that showed the 403 Forbidden message.

26.	Refresh the webpage.
If you accidentally closed this tab, go to the Properties tab, and in the Static website hosting panel choose the Endpoint link again.
You should still see a 403 Forbidden message. 
Analysis: This response is expected! This message indicates that your static website is being hosted by Amazon S3, but that the content is private.
You can make Amazon S3 objects public through two different ways:

o	To make either a whole bucket public, or a specific directory in a bucket public, use a bucket policy.

o	To make individual objects in a bucket public, use an access control list (ACL).
It is normally safer to make individual objects public because this avoids accidentally making other objects public. However, if you know that the entire bucket contains no sensitive information, you can use a bucket policy.
You will now configure the individual objects to be publicly accessible.

27. I	Returned to the web browser tab with the Amazon S3 console (but do not close the website tab).

28. I	Selected all three objects.

29.	In the Actions menu, choose Make public via ACL.
A list of the three objects is displayed.

30.	 I Choose Make public
My static website is now publicly accessible.

31.	Return to the web browser tab that has the 403 Forbidden message.

32.	Refresh the webpage.
You should now see the static website that is being hosted by Amazon S3.


                                  Task 4: Updating the website
You can change the website by editing the HTML file and uploading it again to the S3 bucket.
Amazon S3 is an object storage service, so you must upload the whole file. This action replaces the existing object in your bucket. You cannot edit the contents of an object—instead, the whole object must be replaced.

33.	On my computer, I loaded the index.html file into a text editor (for example, Notepad or TextEdit).

34.	Find the text Served from Amazon S3 and replace it with Created by <YOUR-NAME>, substituting your name for <YOUR-NAME> (for example, Created by Jane).

35.	Save the file.

36.	Return to the Amazon S3 console and upload the index.html file that you just edited.

37.	Select index.html and use the Actions menu to choose the Make public via ACL option again.

38.	Return to the web browser tab with the static website and refresh the page.
Your name should now be on the page.
 
Your static website is now accessible on the internet. Because it is hosted on Amazon S3, the website has high availability and can serve high volumes of traffic without using any servers.
You can also use your own domain name to direct users to a static website that is hosted on Amazon S3. To accomplish this, you could use the Amazon Route 53 Domain Name System (DNS) service in combination with Amazon S3.

![Screenshot 2023-11-18 154610](https://github.com/leye3664/AWS-ALX/assets/85311688/33a92523-71cd-4fa8-b909-56f9cb623300)

