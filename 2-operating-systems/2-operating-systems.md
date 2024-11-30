# Operating systems

Two broad categories: Windows and Unix

# Anatomy

User -> Shell -> Kernel -> Hardware

# Unix-based operating system components

1. File
2. Process (POSIX standard)

# SSH

* Hashing
  - A way to store large info into small characters
  - Types: MD5 (not recommended - can be looked up with rainbow table), SHA1 (not recommended - can be looked up with rainbow table), SHA256 (recommended)
  - One-way algorithm
* Encryption
  - Two-way algorithm
* OpenSSL
  - Cryptographic library toolkit for running hash functions
  - Built into MacOS but can use Homebrew to upgrade
  - `echo password >> foo && openssl md5 foo`
* Salt
  - Random input to combine with original input
  - input + salt + hash function = hash
* Cryptographic hash
  - A hash with a salt. Without a salt, you can look it up in the rainbow table if you know the input
* Ransomware
  - Malicious software that encrypts all data in the system using a hashing function
* (SSH)[https://www.youtube.com/watch?v=RfolgB-rVe8] (Secure socket shell)
  - Includes public and private key pair
  - One-way encryption
    1. Private / public key generated on laptop
    2. Laptop shares public key to server
    3. Laptop attempts SSH connection to server
    4. Server encrypts data using public key and sends to laptop
    5. Laptop decrypts data using private key to prove identity and establish ssh connection
  - (SSH into your laptop)[https://medium.com/@moligninip/how-to-connect-to-your-home-laptop-from-anywhere-with-ssh-604a7aee26a5]
