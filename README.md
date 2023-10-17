
# Setting Up Scheduled e2e Testing on G C P

## Step 1: Prepare Test Code and Repository

Make sure The e2e testing code is stored in a GitHub repository and that have configured scripts to run these tests. 

## Step 2: Create a Cloud Function 

`https://cloud.google.com/functions/docs/deploy`

```
Option:
Deploy from:
[1] local machine
[2] Cloud Storage
[3] source repository
```
1. In the Google Cloud Console, navigate to Cloud Functions.
2. Click "Create Function."

   a. function name.
   
   b. Choose a runtime.Node.js, Python, or another supported runtime.

   c. In the trigger type, select "HTTP."
   
   d. In the `index.js` file, write the code for your Cloud Function. This code will trigger your tests. Here's an example Node.js Cloud Function code snippet that calls your testing script:(from GPT)

   ```javascript
   const child_process = require('child_process');
   
   exports.runE2ETests = (req, res) => {
     // Execute your testing script here
     const testProcess = child_process.spawn('your-e2e-test-command', ['--args']);
   
     testProcess.stdout.on('data', (data) => {
       console.log(`stdout: ${data}`);
     });
   
     testProcess.stderr.on('data', (data) => {
       console.error(`stderr: ${data}`);
     });
   
     testProcess.on('close', (code) => {
       console.log(`Tests completed, exit code: ${code}`);
     });
   
     res.status(200).send('e2e tests initiated.');
   };
    ```

   e. Deploy it as a Cloud Function.

## Step 3: Set Up a Cloud Scheduler Job 

`https://cloud.google.com/scheduler/docs/overview`

1. In the Google Cloud Console, navigate to Cloud Scheduler.
2. Click "Create Job."

   a. name for the job.
   
   b. In "Target," select "HTTP."
   
   c. In "URL," enter the HTTP trigger URL of your Cloud Function.
   
   d. Choose the request type as "GET."
   
   e. Provide a description of the job.
   
   f. In "Time zone," select the appropriate time zone.
   
   g. In "Schedule," specify when want the tests to run, e.g., "0 * * * *" for running every hour.

   h. Click "Create."

## Step 4: Set Up Google Calendar Reminder (Optional)

1. Open Google Calendar.

2. Create a new event or reminder for scheduling your tests. In the event's notification settings, add a notification method.

3. In the notification settings, choose "Browser" and paste the URL of the Cloud Scheduler job. This ensures that the Cloud Scheduler job will be invoked when the event triggers.

## Step 5: Deployment and Testing

Use simple practice to ensure that the above steps will run as expected
