# ENCRYPTING S3 DATA WITH SSE-C VIA THE CLI
<img width="1738" height="1096" alt="image" src="https://github.com/user-attachments/assets/40c7cf50-ca82-49c2-b4a9-8ef53cdd20f5" />

## Objectives
  1. Generate a 256-bit encryption key with openssl and calculate its MD5 checksum
  2. upload a file to S3 with SSE-C so S3 encrypts it with your own key
  3. Download the encrypted object and decrypt it with the same key
  4. Show that S3 refuses the object when the key is not supplied
  5. Read a bucket policy that requires SSE-C on every upload

## Getting started
Configure your CLI

<img width="1080" height="147" alt="image" src="https://github.com/user-attachments/assets/6217bbc3-3b9e-4396-ad2c-f3d38c90a753" />

## Generate a Customer-Managed Encryption Key
### Generate key using openssl

<img width="377" height="103" alt="image" src="https://github.com/user-attachments/assets/ad1c25fc-5cf0-4b6e-a0fe-2abf734f7087" />

### Base64-encode the key

<img width="508" height="219" alt="image" src="https://github.com/user-attachments/assets/e6cb4ced-8757-4074-8259-8a0398eaa4bf" />

The key requires to be base64 encoded to be used on AWS CLI

### Calculate an MD5 hash of the key

<img width="579" height="98" alt="image" src="https://github.com/user-attachments/assets/9978d66b-8f9b-4997-a856-a1cccf6ff641" />

The MD5 checksum of the encryption key is required by S3 to ensure the integrity of the encryption key.

## Create a test file

Test file we would be uploading to S3

<img width="619" height="102" alt="image" src="https://github.com/user-attachments/assets/68ef4fb4-fbbc-41eb-8564-5f8d2875d740" />

## Upload the file to an S3 Bucket

We would upload the file to S3 with the SSE-C key we generated.

<img width="1154" height="211" alt="image" src="https://github.com/user-attachments/assets/e27982f4-6bff-4c25-be9a-f00c92dd51e4" />

## Observe the uploaded object

Checking our S3 Bucket to view the object we uploaded

<img width="623" height="98" alt="image" src="https://github.com/user-attachments/assets/1b241021-1c8c-4fd6-9eae-da6804e80330" />

## Download the encrypted file and Decrypt it

<img width="1155" height="306" alt="image" src="https://github.com/user-attachments/assets/94a74391-7362-47bb-b5ee-4f1e56af2438" />

The command downloads the object from the S3 Bucket saving it on your machine as new-data.txt.






