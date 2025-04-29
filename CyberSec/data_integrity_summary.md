
# Data Integrity and Digital Security Notes

## Hashing Algorithms

### MD5 (Message Digest 5)
- Developed by Ron Rivest.
- Produces a 128-bit hash value.

### SHA (Secure Hash Algorithm)
- SHA-0 (initial version)
- SHA-2 family:
  - SHA-224 (224-bit)
  - SHA-256 (256-bit)
  - SHA-384 (384-bit)
  - SHA-512 (512-bit)

---

## Types of Data Integrity Control

- **Hashing Algorithms**
- **Password Salting**
- **Keyed-Hash Message Authentication Code (HMAC)**

### Salting
- Salting is used to make hashing more secure.
- It adds random data to the input, creating a different hash value even for identical inputs.

### HMAC (Hash-Based Message Authentication Code)
- Strengthens hashing algorithms by using an additional secret key.

---

## Digital Signatures and the Law

- Provide the same function as handwritten signatures for electronic documents.
- Used to detect if someone edits a document after signing.
- Many countries treat digital signatures as legally valid.
- Digital signatures also provide non-repudiation (proof that the sender cannot deny sending the message).

---

## How Digital Signature Technology Works

- Based on asymmetric cryptography (public and private key pairs).

---

## Digital Certificates

### Basics of Digital Certificates
- Equivalent to an electronic passport.
- Enable users, hosts, and organisations to exchange information securely over the internet.
- Authenticate and verify users.
- Provide confidentiality.

### Construction of Digital Certificates
- Must follow a standard structure: **X.509** is the standard.
- **PKI (Public Key Infrastructure)** defines the policies, roles, and procedures.

---

## Database Integrity

- Databases provide an efficient way to store, retrieve, and analyse data.
- As data collection grows, protecting sensitive data becomes important for cybersecurity professionals.
- Data integrity refers to the accuracy, consistency, and reliability of stored data.

### Four Types of Database Integrity

1. **Entity Integrity**
   - Every row must have a unique identifier (Primary Key).
2. **Domain Integrity**
   - Data stored in a column must follow the same format.
3. **Referential Integrity**
   - Table relationships must remain consistent.
   - Cannot delete a record if it is referenced by another table.
4. **User-Defined Integrity**
   - Custom rules set by users that do not fit into other categories.

---

## Data Integrity Enforcement

### Database Validation

- **Size**: Check the number of characters in a data item.
- **Format**: Ensure data conforms to the specified format.
- **Consistency**: Ensure codes in related data items are consistent.
- **Range**: Ensure data falls within minimum and maximum values.
- **Check Digit**: Perform an extra calculation to generate a check digit for error detection.

---

## Maintaining Database Filing System

- Proper filing is critical.
- A database is made up of tables, records, fields, and data within each field.
- To maintain integrity:
  - Follow specific filing rules.
  - Ensure every table has a Primary Key (Entity Integrity).
  - Manage NULL values correctly (they signify missing or unknown data).
  - Enforce Referential Integrity to maintain consistency through Foreign Keys.
