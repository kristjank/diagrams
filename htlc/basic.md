```mermaid
sequenceDiagram
  title: A Two Way HTLC Atomic Swap (300 ARK <-> 3000 XQR)
  participant A1 as ARK Chris
  participant A2 as ARK Lars
  participant ...

  A1->>A2: 1. htlc_lock (id:1, amount: 300ARK, rec: ARK Lars, secret_hash)
  Note over A1,A2: S1: 300 ARK locked for ARK Lars <br/>Only ARK Chris knows the SECRET

  participant X1 as XQR Chris
  participant X2 as XQR Lars
  X2->>X1: 2. htlc_lock (id:2, amount: 3000XQR, rec: XQR Chris, secret_hash)
  Note over X1,X2: S2: 3000 XQR locked for XQR Chris <br/>Lock transactions uses the SAME <br/> secret_hash as in S1.
  
  X1->>X1: 3. htlc_claim (id:3, claim-tx-id:2, secret_raw)
  Note over X1: S3: XQR Chris CLAIMS <br/> locked tx [id:2] and <br/> REVEALS the SECRET, <br/> receives 3000 XQR
  
  A2 ->> A2: 4. htlc_claim(id:4, claim-tx-id:1, secret_raw)
  Note over A2: S4: ARK Lars CLAIMS<br/>locked tx [id:1] and <br/> receives 300 ARK. <br/> SECRET was revealed<br/> in S3.
```