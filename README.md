## GitHub Actions S3 Sync Workflow
This repository contains a GitHub Actions workflow that synchronizes the contents of this repository with an Amazon S3 bucket.

### Workflow Description
The workflow performs the following steps:
1. Removes all existing content from the S3 bucket s3://github.nasir.id.
2. Copies all content from the GitHub repository to the S3 bucket s3://github.nasir.id.

This ensures that the S3 bucket always reflects the current state of the GitHub repository.

### Prerequisites
To use this workflow, you need to have:

 - An Amazon Web Services (AWS) account
 - An S3 bucket 
 - AWS credentials (Access Key ID and Secret Access Key) with permissions to read and write to the S3 bucket

 ### Setup
 1. Store your AWS credentials as GitHub secrets:

Go to your GitHub repository
Click on Settings > Secrets and variables > Actions
Add the following secrets:
```
AWS_ACCESS_KEY_ID: Your AWS Access Key ID
AWS_SECRET_ACCESS_KEY: Your AWS Secret Access Key
```

2. Ensure your workflow file (e.g., .github/workflows/s3-sync.yml) is present in your repository.


### Workflow File
Here's an example of what your workflow file might look like:

name: Sync to S3
```
on:
  push:
    branches:
      - main  # or your default branch name

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: your-aws-region  # e.g., us-east-1
    
    - name: Remove existing content from S3
      run: aws s3 rm s3://github.nasir.id --recursive
    
    - name: Sync repository contents to S3
      run: aws s3 sync . s3://github.nasir.id
```
Note: Replace your-aws-region and bucket name with the appropriate AWS region where your S3 bucket is located.

### Usage
Once set up, this workflow will automatically run whenever you push changes to the main branch of your repository. It will sync the contents of your repository to the specified S3 bucket.

###  Security Note
Important: Ensure that your AWS credentials have the minimum required permissions to perform the S3 operations. It's recommended to use an IAM role with limited scope for added security.
