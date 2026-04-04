## A key is a secret value used by the encryption system.

During encryption, the key is used to lock the readable data and turn it into unreadable text. 

During decryption, the correct key is used to unlock that text and get back the original readable data.

---

## Simple example

Suppose your original text is:

```text
HELLO
```

After encryption, it may become something unreadable like:

```text
Q8#Lm2
```

Then decryption with the correct key changes it back to:

```text
HELLO
```

---

AWS Key Management Service, is an AWS managed service used to create and control the cryptographic keys used to encrypt data

KMS key protects the encryption key, and the encryption key protects the data. 

---

## Flow:

1. KMS generates a data key.  
2. That data key encrypts the text into ciphertext.  
3. KMS also returns an encrypted copy of the data key.  
4. Later, when the data must be read, KMS decrypts the encrypted data key.  
5. That plaintext data key is then used to decrypt the ciphertext back into the original text

---

When you create the EBS volume with encryption enabled, EBS uses your chosen KMS key, or the default aws/ebs KMS key if you do not specify one. EBS gets a unique encrypted data key or volume key and stores that encrypted key with the volume metadata. 

## When you attach the encrypted EBS volume to EC2

### 1.Write flow: EC2 → EBS

The application writes data to the disk normally. From the EC2 instance point of view, it feels like a normal disk write.  
Before the data is stored on the EBS volume, EBS uses the decrypted volume key to encrypt the data blocks.  
The encrypted blocks are then written to the EBS volume.

### 2.Read flow: EBS → EC2

EC2 requests the data from the attached EBS volume.  
EBS reads the encrypted blocks from disk.  
EBS uses the same decrypted volume key in memory to decrypt those blocks.  
The plaintext data is returned to the EC2 instance, so the application sees normal readable data.

---

## Very simple picture

- KMS protects the main key.
- EBS uses a decrypted volume key to encrypt and decrypt disk blocks.
- EC2/app reads and writes normally, without doing manual encryption steps

---

## steps

### 1) Create the KMS key

In the AWS Console:  

- Open KMS
- Go to Customer managed keys
- Choose Create key
- Select Symmetric key type for EBS encryption
- Choose Encrypt and decrypt
- Give it an alias like alias/ebs-prod-key
- Set admins and key users
- Finish creation

### 2) Use that KMS key with an EBS volume

#### Option A: While creating a new EBS volume

- Open EC2
- Go to Volumes
- Choose Create volume
- Enable Encryption
- In KMS key, select your customer managed key
- Create the volume
- Then attach the volume to the EC2 instance

EBS uses the KMS key you specify when creating encrypted volumes and snapshots.

#### Option B: While launching a new EC2 instance

- Launch EC2
- In the storage section, look at the EBS volume settings
- Turn on Encrypted
- Choose your KMS key
- Launch the instance

That makes the instance’s EBS volume encrypted with the KMS key you selected.

---

## Note:If you enable EBS encryption and do not specify a KMS key, Amazon EBS uses the default AWS managed key for EBS in your account, which is aws/ebs

---

## Here is each term in simple words.

### 1. KMS key

A KMS key is the main encryption key object inside AWS KMS. 

### 2. Customer managed key

This is the key you create and control. You can choose its alias, key policy, rotation, enable or disable it, and schedule deletion.  

Simple example: you create a key called alias/prod-ebs-key and use it for encrypting EBS volumes.

### 3. AWS managed key

This is created by AWS for an AWS service in your account. You can see it, but you do not fully manage its lifecycle.  

Simple example: you enable EBS encryption and do not choose your own key, so AWS uses the default managed key like aws/ebs.

### 4. AWS owned key

This is fully controlled by AWS and used internally by AWS services. You usually do not manage it and do not get the same visibility as customer managed keys.  

Simple example: some AWS service encrypts its internal data using keys fully handled by AWS behind the scenes.

### 5. Alias

An alias is just a friendly name for a KMS key.  

Instead of remembering a long key ID, you can use something readable like alias/my-app-key.

### 6. Key policy

A key policy is the main permission policy attached directly to the KMS key. It decides who can use or manage that key. Every KMS key has exactly one key policy.  

Simple example: key policy says only the DevOps role can use this key for decrypt operations.

### 7. IAM policy

IAM policy is the normal AWS permission policy attached to a user, group, or role. In KMS, IAM policy matters, but the key policy is still central.  

Simple example: your EC2 role has IAM permission for kms:Decrypt, but it still may not work unless the key policy also allows it.

### 8. Grant

A grant is a temporary or scoped permission to use a KMS key. It is often used when an AWS service needs to use the key on your behalf.  

Simple example: an AWS service needs temporary permission to encrypt or decrypt something using your key without you editing the key policy each time.

### 9. Data key

This is the most confusing one, but it is important. A data key is the key that usually encrypts the actual data. KMS generates it and returns two versions: one plaintext copy and one encrypted copy.  

Simple example: your EBS or S3 data is encrypted using a data key, and the KMS key protects that data key.

### 10. Envelope encryption

This means:

- the data key encrypts the real data
- the KMS key encrypts and protects the data key

So KMS usually protects the key that protects the data.
