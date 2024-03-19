# Coding Security in Mind

Be aware of your responsibility to the users.

## Project Scale
- Do not keep any credentials inside public repositories.

## Coding Scale

- Use char[] instead of string [stackoverflow discussion](https://stackoverflow.com/questions/8881291/why-is-char-preferred-over-string-for-passwords/8881376#8881376).
  - from [Java Cryptography Architecture guide](https://docs.oracle.com/javase/6/docs/technotes/guides/security/crypto/CryptoSpec.html#PBEEx):
    > It would seem logical to collect and store the password in an object of type java.lang.String. However, here's the caveat: Objects of type String are immutable, i.e., there are no methods defined that allow you to change (overwrite) or zero out the contents of a String after usage. This feature makes String objects unsuitable for storing security sensitive information such as user passwords. You should always collect and store security sensitive information in a char array instead.
