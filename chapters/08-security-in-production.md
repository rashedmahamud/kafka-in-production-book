
# Chapter 8: Security in Production

Securing your Kafka cluster is critical to protect data and ensure compliance in production environments.

## Enabling TLS, SASL, and ACLs

- Use **TLS encryption** to secure data in transit between clients and brokers.
- Configure **SASL** for authentication, with mechanisms like SCRAM or GSSAPI (Kerberos).
- Implement **ACLs (Access Control Lists)** to restrict topic and cluster operations by user or group.

## Real Issues with Misconfigured Authentication

- Misconfigured security settings can lead to unauthorized access or connectivity failures.
- Always validate your security setup in test environments before production rollout.
- Monitor for authentication errors and unauthorized access attempts.

## Network Hardening and Access Control

- Isolate Kafka clusters behind firewalls or private networks.
- Limit access to brokers and Zookeeper/KRaft nodes only to trusted clients and admins.
- Use network segmentation and VPNs where appropriate.

---

Security is often overlooked but critical — make it part of your Kafka deployment process.

---

Ready for Chapter 9?
