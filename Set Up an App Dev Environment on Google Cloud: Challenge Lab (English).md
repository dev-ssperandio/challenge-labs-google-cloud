# Set Up an App Dev Environment on Google Cloud: Challenge Lab

## Task 1: Create a Bucket
1. In the **Navigation Menu**, click on **Cloud Storage** > **Buckets**.
2. Click on **CREATE**.
3. On the **Create a Bucket** screen, fill in the name suggested by your lab.
4. Click on **CREATE** and then on **CONFIRM**.

## Task 2: Create a Pub/Sub Topic
1. In the **Navigation Menu**, click on **VIEW ALL PRODUCTS** and search for **Pub/Sub** in the search bar.
2. In the **Topics** option in Pub/Sub, click on **CREATE TOPIC**.
3. In the **Topic ID** field, fill in the name defined by the lab.
4. Click on **CREATE**.

## Task 3: Create the Thumbnail Cloud Run Function
1. In the **Navigation Menu**, click on **VIEW ALL PRODUCTS** and search for **Cloud Run Function** in the search bar.
2. Click on **CREATE FUNCTION**.
3. In the **Environment** field, select the **Cloud Run Function** option.
4. In **Function Name**, fill in the name defined by the lab.
5. In **Region**, fill in the region defined by the lab.
6. In **Trigger Type**, select **Cloud Storage**.
7. In **Bucket**, fill in the name of the bucket created earlier, click on **BROWSE** and **SELECT**.
8. In the **Runtime, build, connections and security settings** area, in **Maximum Number of Instances**, set it to 5.
9. Click on the **NEXT** button and then on **ENABLE**.
10. In **Entry Endpoint**, fill in the name of the Cloud Function suggested by your lab.
11. Replace the code in `index.js` with the first code presented in task 3.
12. Replace the code in `package.json` with the second code presented in task 3.
13. Click on **DEPLOY**.

## Task 4: Test the Infrastructure
1. Go back to **Cloud Storage** > **Buckets**.
2. Click on the name of the bucket created in this lab.
3. Alternatively, you can use the image suggested in task 4 to upload it to the bucket. Save it to your computer.
4. Click on **UPLOAD > UPLOAD FILE** and select the image.
5. Click on **REFRESH**.

## Task 5: Remove the Previous Cloud Engineer
1. In the **Navigation Menu**, go to **IAM & Admin** > **IAM**.
2. Click on **View By Roles**.
3. In **Viewer**, click on the pencil icon of the other user indicated by the lab.
4. Click on the trash icon and then on **SAVE**.

**Congratulations!**
