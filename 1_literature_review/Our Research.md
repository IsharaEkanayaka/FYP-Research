problem: when we try to be resilient to poisoning attacks we use statistical techniques, those techniques compromise the privacy. we are exploring ways to being resilient while protecting the privacy.

Gap : how can we preserve strong privacy while being resilient to poisoning attacks?

1. Trusted Execution Environments(TEEs)
	1. Secure Enclave: The central server utilizes a Trusted Execution Environment (TEE), such as Intel SGX, which is a hardware-isolated, encrypted portion of the server's memory.
	2. Encrypted Transmission: Each client encrypts its individual model update using a key shared only with the TEE.
	3. Secure Inspection: The encrypted updates are sent to the server, where they are loaded into the TEE.
	4. Robustness Check: Inside the TEE, the updates are decrypted, and a robust aggregation algorithm (like WeiDetect or Multi-Krum) performs the required examination, outlier detection, and rejection.
	5. Safe Aggregation: Only the final, cleaned and aggregated global model update leaves the TEE.
2. Advanced Cryptography (Decoupling)
		using Secure Multiparty Computation (MPC) or advanced Homomorphic Encryption (HE)
		1. Decoupled Architecture: The system might use multiple servers. One set of servers handles the robust aggregation checks (security), and another set handles the cryptographic aggregation (privacy).
		2. Encrypted Outlier Detection: The latest methods are developing ways to perform certain distance calculations or aggregation checks on encrypted data (Homomorphic Encryption) or via secure protocol (MPC), allowing the detection of anomalies without decrypting the full individual model update.