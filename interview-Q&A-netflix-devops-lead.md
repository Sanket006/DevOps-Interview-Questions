# DevOps Lead Interview – Netflix

# Round 1 – Systems at Scale, Kubernetes, Cloud & Linux

---

## 1. Fine-Grained Service Discovery (1000+ Microservices) using Envoy / Istio

### Objective

Design scalable, low-latency, failure-tolerant service discovery across 1000+ services.

### Architecture Strategy

* Use Istio control plane (Istiod) for service registry aggregation.
* Leverage Kubernetes API + external service registries (if hybrid cloud).
* Enable xDS APIs for dynamic Envoy config distribution.
* Use locality-aware routing + zone failover.
* Enable endpoint sharding to reduce config blast radius.
* Use ServiceEntry for external services.

### Scalability Controls

* Enable incremental xDS.
* Use Sidecar resource scoping per namespace.
* Partition control plane per domain if needed.

### Observability

* Distributed tracing (Jaeger/Zipkin).
* Envoy metrics via Prometheus.
* Circuit breaking + outlier detection.

### Failure Handling

* Automatic retries with exponential backoff.
* Health check based endpoint ejection.

---

## 2. eBPF + Cilium for Runtime Network Security

### Why eBPF?

Kernel-level packet filtering without iptables bottleneck.

### Implementation

* Use Cilium as CNI.
* Define Kubernetes NetworkPolicies.
* Use CiliumClusterwideNetworkPolicy.
* Enable L7 filtering (HTTP, gRPC).
* Enforce identity-based security instead of IP-based.

### Advantages over Traditional CNI

* No iptables rule explosion.
* Higher performance.
* Native observability via Hubble.
* Deep visibility at syscall/network layer.
* Policy enforcement without sidecars.

---

## 3. Multi-Cloud Strategy (Routing, IAM, Secrets)

### Cross-Cloud Routing

* Global Anycast + Geo-routing.
* Use cloud interconnect (AWS Direct Connect / GCP Interconnect).
* Service mesh federation.

### IAM Strategy

* Use OIDC federation.
* Centralized identity broker.
* Short-lived STS tokens.

### Secret Sync

* Use HashiCorp Vault.
* Enable dynamic secrets.
* Replicate via Vault replication.

---

## 4. Intermittent systemd Failures on EKS Nodes

### Detection

* CloudWatch logs.
* Node Problem Detector.
* Kubelet event logs.

### Healing

* Auto Scaling Group replacement.
* Enable maxUnavailable rolling node upgrade.
* Use node taints + cordon.

---

## 5. Custom AMI Vetting Strategy

### Kernel Validation

* Check kernel modules.
* Validate cgroup v2 compatibility.
* eBPF compatibility tests.

### Runtime Validation

* Stress testing.
* Chaos tests.
* CIS hardening scan.

---

## 6. Advanced Kubernetes Probes

### Strategy

* Use gRPC health checks.
* Add synthetic business logic checks.
* Separate readiness vs liveness vs startup probes.

---

## 7. DNS-Level Outage in Mesh

### Mitigation

* Enable Envoy DNS caching.
* Reduce TTL.
* Use static ServiceEntry fallback.
* Deploy CoreDNS HPA.

---

## 8. Terraform remote_state Timeout

### Recovery

* Check backend health.
* Enable state locking.
* Restore from backup.
* Move to versioned backend (S3 + DynamoDB lock).

---

# Round 2 – RCA, Fire Drills, Chaos

## 1. mTLS Broken After Envoy Rollout

### RCA Path

* Compare old vs new config.
* Check certificates.
* Validate SAN.
* Check PeerAuthentication + DestinationRule.

---

## 2. HPA Not Scaling

### Debug Path

* Check metrics-server.
* Validate target CPU request.
* Check Cloud provider scaling limits.

---

## 3. Sidecar Cache Latency Spike

### Debug

* Check CPU throttling.
* Inspect cache hit ratio.
* Analyze P99 latency.

---

## 4. 504 Errors in Playback

### Triage

* Trace request path.
* Inspect upstream timeouts.
* Validate service dependencies.

---

## 5. NAT Cost Surge

### Possible Causes

* Increased cross-AZ traffic.
* Pods without PrivateLink.
* New external API calls.

---

# Round 3 – Leadership & Engineering Influence

## 1. SLO Ownership Culture

* Error budget policy.
* Blameless postmortems.
* SLO dashboards visible org-wide.

---

## 2. Multi-Region Failover Without DNS

* Global load balancer.
* Traffic shadowing.
* Data replication.

---

## 3. Chaos Testing for Release

* Inject latency.
* Kill nodes.
* Validate graceful degradation.

---

## 4. Proving ROI of Infra Modernization

* Show reduced MTTR.
* Deployment frequency metrics.
* Cost optimization graphs.

---

# Final Note

This structured approach demonstrates architectural depth, operational maturity, debugging skills, and leadership mindset expected from a Netflix-scale DevOps Lead.
