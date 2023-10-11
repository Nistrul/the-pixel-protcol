EC2 Key Pairs vs. Server Host Keys in AWS: Clearing the Confusion
When launching an EC2 instance on Amazon Web Services (AWS), users are typically prompted to select or create an EC2 key pair. This key pair, usually RSA-based, consists of a public and a private key. The public key is stored on the EC2 instance, specifically in the ~/.ssh/authorized_keys file of the default user (often ec2-user in Amazon Linux). This setup permits users to authenticate and SSH into the instance using the corresponding private key. At this stage, it's important to recognize that this key pair is primarily for user authentication.

However, as you initiate the SSH connection to your EC2 instance for the first time, you might encounter a prompt asking if you trust the server's fingerprint, often represented as an ECDSA or ED25519 fingerprint. This is where confusion commonly arises.

This prompt isn't about the EC2 key pair. Instead, it pertains to the server's host keys. Every SSH server, including those running on EC2 instances, has its unique set of host keys, responsible for identifying the server's identity to clients. When the SSH client connects to a server it hasn't seen before, it seeks verification from the user to ensure the server's identity is legitimate. The key types (like ECDSA or ED25519) displayed during this prompt represent the server's identity, not the user's authentication.

So, how can we ensure that we are connecting to the right EC2 instance, especially during the first connection? One recommended method is to obtain the host key fingerprint in advance using AWS Systems Manager (SSM):

Setting Up SSM: First, ensure that the EC2 instance has the necessary IAM role to allow SSM operations and that the AWS Systems Manager Agent (SSM Agent) is updated and running.

Retrieving the Host Key: Use SSM's "Run Command" to execute a command that retrieves the SSH host key fingerprint. For instance, for the ECDSA key, the command would be:

bash
Copy code
ssh-keygen -lf /etc/ssh/ssh_host_ecdsa_key.pub
Verifying the Key: The output will be the fingerprint of the host key. With this fingerprint in hand, you can confidently compare and verify the server's identity when connecting via SSH.

In summary, understanding the distinction between EC2 key pairs and server host keys is vital when working with AWS. The EC2 key pairs facilitate user authentication, whereas server host keys ensure server verification. By leveraging tools like AWS Systems Manager, users can ensure not just a seamless but also a secure experience with AWS.
