# Coding Security in Mind

Be aware of your responsibility to the users.

## Project Scale Principles
- Do not keep any credentials inside public repositories.

## Coding Scale Principles

- Use char[] instead of string [stackoverflow discussion](https://stackoverflow.com/questions/8881291/why-is-char-preferred-over-string-for-passwords/8881376#8881376).
  - from [Java Cryptography Architecture guide](https://docs.oracle.com/javase/6/docs/technotes/guides/security/crypto/CryptoSpec.html#PBEEx):
    > It would seem logical to collect and store the password in an object of type java.lang.String. However, here's the caveat: Objects of type String are immutable, i.e., there are no methods defined that allow you to change (overwrite) or zero out the contents of a String after usage. This feature makes String objects unsuitable for storing security sensitive information such as user passwords. You should always collect and store security sensitive information in a char array instead.

## API Security

- **Ensure User Authentication & Access Control:**
CRUD APIs must prioritize user authentication and access control to ensure that only authorized entities can interact with and modify data. This involves setting up a robust system that verifies users' identities and checks their permissions before allowing requested operations.

- **Validate Input Data:**
Another critical security measure is input validation, which aims to prevent various attacks such as SQL injection or cross-site scripting (XSS). CRUD APIs should thoroughly check all input data to ensure it aligns with expected formats, lengths, and types, thus fortifying defenses against potential vulnerabilities.

- **Implementing Data Encryption:**
Data encryption is essential for safeguarding sensitive information in case of breaches. CRUD APIs should employ encryption techniques at either the application or database level, using strong algorithms and keys to enhance data security.

- **Enforce API Rate Limits:**
To mitigate the risk of denial-of-service (DoS) attacks, CRUD APIs need to enforce rate limits on request rates. This can involve restricting the number of requests per unit of time or tailoring limits based on user or application identity to maintain service availability and integrity.

- **Establish Audit Trails & Logging:**
Maintaining comprehensive audit trails and logs is crucial for tracking data access and usage. These logs not only aid in detecting and investigating security incidents but also ensure compliance with regulatory requirements regarding data access and handling.
