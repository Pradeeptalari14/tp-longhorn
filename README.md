# Longhorn Cloud-Native Block Storage Studio (tp-longhorn)

[![SLSA Level 3](https://img.shields.io/badge/SLSA-Level%203-success?style=flat-square&logo=sigstore)](https://slsa.dev)
[![DevSecOps 9+](https://img.shields.io/badge/DevSecOps-9%2B%20Pillars-blue?style=flat-square&logo=kubernetes)](https://talaripradeep.info)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

> **Production Repository:** [`Pradeeptalari14/tp-longhorn`](https://github.com/Pradeeptalari14/tp-longhorn)  
> **Interactive Studio Workspace:** [https://talaripradeep.info/tools/longhorn/](https://talaripradeep.info/tools/longhorn/)  
> **Visual Flow Canvas:** [https://talaripradeep.info/studios/flow/?studio=longhorn](https://talaripradeep.info/studios/flow/?studio=longhorn)  
> **Architecture Center:** [https://talaripradeep.info/architecture/](https://talaripradeep.info/architecture/)

---

## 🏛️ Overview
Cloud-native distributed block storage for Kubernetes with CSI storage classes, cross-node replication, and S3 backups.

This repository is built following the **9+ DevSecOps / SRE Governance Principles**:
1. **Cosign SLSA L3**: Keyless container and manifest attestation recorded to Rekor.
2. **Semgrep AST Guardrails**: Pre-commit AST rules preventing taint paths and plaintext credentials.
3. **CycloneDX SBOM**: Syft-generated Software Bill of Materials with Grype zero High/Critical CVE gating.
4. **Kyverno Admission Policies**: Enforcing CIS Kubernetes hardening, non-root users, and read-only filesystems.
5. **eBPF In-Kernel Runtime Observability**: Tetragon tracing and live syscall profiling.
6. **GitOps Zero-Drift Synchronization**: ArgoCD/Flux continuous declarative reconciliation.
7. **MELT Golden Signals**: OpenTelemetry traces, Prometheus metrics, and structured logging.
8. **Automated SRE Remediation**: Self-healing circuits and SLO compliance.
9. **Zero-Trust Microsegmentation**: Strict SPIFFE/mTLS and encrypted storage volumes.

---

## 📄 Primary Configuration: `storageclass.yaml`

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: longhorn-fast-ssd
provisioner: driver.longhorn.io
allowVolumeExpansion: true
reclaimPolicy: Delete
volumeBindingMode: Immediate
parameters:
  numberOfReplicas: "3"
  staleReplicaTimeout: "2880"
  fromBackup: ""
  fsType: "ext4"
  dataLocality: "best-effort"
  encrypted: "true"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-primary-pvc
  namespace: database
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: longhorn-fast-ssd
  resources:
    requests:
      storage: 250Gi

```

---

## 🚀 Quickstart & Verification

```bash
# 1. Clone repository
git clone https://github.com/Pradeeptalari14/tp-longhorn.git
cd tp-longhorn

# 2. Run verification drill
bash scripts/validate.sh
```

---

## 📜 License
MIT License. Copyright (c) 2026 Talari Pradeep. All rights reserved.
