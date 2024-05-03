# Blob 
Learning how to do crud methods (create, read, update and delete.)
### AWS S3
- Simple storage service
- Stores and retrieves any type of data from anytime from anywhere, with no organisation
- Hosts static website on cloud
- Blobs need to be stored in AWS bucket, Azure container you can't have buckets in buckets
- You just chuck things in there
- You can organise it, you can put files in folders
- Default is everything is private
- Config to make public
- If blobs are public there is URL/ endpoint and you can access from anywhere
- Built from ground up, so that there is redundancy you can tweak this
- Azure there are blobs stored in three places within same data center, can be different zones or regions
- access via AWS console, AWS cli, Python boto3
- Costs a lot less keeping it private
  
1. 
```bash
sudo apt update -y

sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
```
2. setup python 3 up and AWS cli
```bash
# check if pyhton 3 is already installed
python3 --version 

sudo apt-get install python3

sudo apt-get install python3-pip -y

#install aws cli
sudo pip install awscli 

# version check
aws --version 
```
3. renaming commands - Alias (doesn't stick)
```bash
alias python=python3
```
4. Authenticate with specific file with access key ID and secret access ID
```bash
aws configure
(# paste access key)
(# paste secret key)
# default region name: eu-west-1
# default output: json
aws s3 ls
# should see a list of buckets
```
4. Doing things with aws s3
```bash
# make bucket
aws s3 mb s3://tech258-muyis-first-bucket # --region eu-west-1

# list bucket
 aws s3 ls s3://tech258-muyis-first-bucket

# upload file to bucket
# first make file
echo This is the first line in a test file > test.txt

# then copy into bucket 
aws s3 cp test.txt s3://tech258-muyis-first-bucket

# download files from bucket
mkdir downloads

# sync to copy
aws s3 sync s3://tech258-muyis-first-bucket .

# rm to remove a file
aws s3 rm s3://tech258-muyis-first-bucket/test.txt
```
:warning: :exclamation_mark: Warning Will delete everything in bucket without confirmation :exclamation_mark: :warning:
```bash
# remove everything in a bucket DANGEROUS
aws s3 rm s3://tech258-muyis-first-bucket --recursive

# remove bucket only works for empty buckets
aws s3 rb s3://tech258-muyis-first-bucket

# remove bucket and contents
aws s3 rb s3://tech258-muyis-first-bucket --force
```
You can access blobs via AWS console online

## Scripts
### List all the S3 buckets
```python
import boto3
s3 = boto3.client('s3')
response = s3.list_buckets()
for bucket in response['Buckets']:
    print(bucket['Name'])
```

### Create an S3 Bucket
```python
import boto3
s3 = boto3.client('s3')
bucket_name = 'tech257-muyis-test-boto3'
s3.create_bucket(Bucket=bucket_name)
```

### Upload Data/File to the S3 Bucket
```python
import boto3
s3 = boto3.client('s3')
bucket_name = 'tech257-muyis-test-boto3'
file_path = 'path_to_your_file'
s3.upload_file(file_path, bucket_name, 'file_name_in_bucket')
```

### Download/Retrieve Content/File from the S3 Bucket
```python
import boto3
s3 = boto3.client('s3')
bucket_name = 'tech257-muyis-test-boto3'
file_path = 'path_to_save_downloaded_file'
s3.download_file(bucket_name, 'file_name_in_bucket', file_path)
```

### Delete Content/File from the S3 Bucket
```python
import boto3
s3 = boto3.client('s3')
bucket_name = 'tech257-muyis-test-boto3'
s3.delete_object(Bucket=bucket_name, Key='file_name_in_bucket')
```

### Delete the Bucket
```python
import boto3
s3 = boto3.resource('s3')
bucket_name = 'tech257-muyis-test-boto3'
bucket = s3.Bucket(bucket_name)
for key in bucket.objects.all():
    key.delete()
bucket.delete()
```