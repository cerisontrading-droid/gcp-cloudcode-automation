# 🚀 GCP Free Tier + Cloud Code Automation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/cerisontrading-droid/gcp-cloudcode-automation.svg)](https://github.com/cerisontrading-droid/gcp-cloudcode-automation/stargazers)

## Overview

Complete automation package for deploying applications to Google Cloud Platform's Always-Free tier using Cloud Code. Zero monthly costs. Full production-ready setup.

### Key Features

✅ **Zero Cost** - Always-Free tier eligible configuration  
✅ **Cloud Code Ready** - VS Code & IntelliJ integration  
✅ **Fully Automated** - Complete infrastructure & deployment setup  
✅ **Production Grade** - Error handling, validation, monitoring  
✅ **Any Application** - Works with Next.js, Node.js, Docker, etc.  

## 🎯 What You Get

### Infrastructure (Automated)
- **e2-micro Compute Engine** instance (2 vCPU, 1GB RAM)
- **30GB Standard persistent disk** (Always-Free)
- **Debian 12 Linux** OS
- **Standard network tier** (cost-optimized)
- **HTTP/HTTPS firewall rules** (port 80 & 443)
- **Monthly Cost**: $0 (Always-Free eligible)

### Deployment Tools
- **Skaffold** - Automated build, push, deploy
- **Google Cloud Build** - Container image building
- **Container Registry (GCR)** - Image storage
- **Cloud Code** - IDE integration (VS Code/IntelliJ)

## 📦 Files Included

| File | Purpose |
|------|----------|
| `deploy-gcp-free-tier.sh` | Complete infrastructure deployment automation |
| `skaffold.yaml` | Cloud Code & Skaffold configuration |
| `QUICKSTART.md` | 3-step getting started guide |
| `.env.example` | Configuration template |
| `.gitignore` | Git exclusions |

## ⚡ Quick Start

### 1. Clone Repository
```bash
git clone https://github.com/cerisontrading-droid/gcp-cloudcode-automation.git
cd gcp-cloudcode-automation
```

### 2. Configure Environment
```bash
cp .env.example .env
# Edit .env with your GCP project details
export GCP_PROJECT_ID="your-project-id"
export GCP_INSTANCE_NAME="cloudcode-instance"
export GCP_REGION="us-west1"
export GCP_ZONE="us-west1-b"
```

### 3. Deploy Infrastructure
```bash
chmod +x deploy-gcp-free-tier.sh
./deploy-gcp-free-tier.sh
```

### 4. Deploy Application

**Option A: Cloud Code (Recommended)**
```bash
# VS Code: Cmd + Shift + P > Cloud Code: Deploy
# IntelliJ: Run > Cloud Code: Deploy
```

**Option B: Skaffold CLI**
```bash
skaffold run --platform=gcp --default-repo=gcr.io/$GCP_PROJECT_ID
```

## 🔧 Configuration

### Required Environment Variables
```bash
GCP_PROJECT_ID          # Your GCP project ID
GCP_INSTANCE_NAME       # VM instance name (e.g., cloudcode-instance)
GCP_REGION              # us-west1, us-east1, or us-central1
GCP_ZONE                # Zone within region (e.g., us-west1-b)
```

### Free Tier Defaults (DO NOT CHANGE)
```bash
GCP_MACHINE_TYPE=e2-micro              # Always-Free eligible
GCP_BOOT_DISK_SIZE=30                  # 30GB Always-Free
GCP_BOOT_DISK_TYPE=pd-standard         # Standard disk (Always-Free)
GCP_NETWORK_TIER=STANDARD              # Cost-optimized
```

## 📚 Documentation

- **[QUICKSTART.md](./QUICKSTART.md)** - Get started in 3 steps
- **[skaffold.yaml](./skaffold.yaml)** - Cloud Code configuration
- **[.env.example](./.env.example)** - Configuration template

## 🏗️ Architecture

```
┌─────────────────────────┐
│   Local Development     │
│  (Cloud Code IDE)       │
└───────────┬─────────────┘
            │ (Skaffold)
            ▼
┌─────────────────────────┐
│  Google Cloud Build     │ ← Builds Docker images
└───────────┬─────────────┘
            │
            ▼
┌─────────────────────────┐
│  Container Registry     │ ← Stores images
└───────────┬─────────────┘
            │
            ▼
┌─────────────────────────┐
│  e2-micro VM Instance   │ ← Runs application
│  (Always-Free Tier)     │
└─────────────────────────┘
```

## 💰 Cost Analysis

### Included (Always-Free)
- 1× e2-micro instance
- 30GB standard persistent disk
- 5GB egress from North America
- 1GB Cloud Build memory-month
- Cloud Code IDE integration

### What Costs Extra (Avoided in This Setup)
- Premium network tier
- Machine snapshots/backups
- SSD/Premium disks
- Data transfer outside North America
- Ops Agent (disabled)

**Monthly Cost: $0.00 USD**

## 🔐 Security

✅ Firewall rules restrict traffic to ports 80/443 only  
✅ SSH key-based authentication (gcloud managed)  
✅ Standard network tier (no premium data fees)  
✅ Proper IAM role assignments  
✅ Environment variables in .gitignore  

## 🛠️ Troubleshooting

### SSH Connection Fails
```bash
gcloud compute ssh $GCP_INSTANCE_NAME --zone=$GCP_ZONE --project=$GCP_PROJECT_ID
```

### Check Instance Status
```bash
gcloud compute instances describe $GCP_INSTANCE_NAME --zone=$GCP_ZONE
```

### View Deployment Logs
```bash
skaffold run -v debug --platform=gcp
```

### Reset Everything
```bash
# Delete instance
gcloud compute instances delete $GCP_INSTANCE_NAME --zone=$GCP_ZONE --quiet

# Delete firewall rules  
gcloud compute firewall-rules delete allow-http-* --quiet
```

## 📖 Learning Resources

- [Cloud Code Docs](https://cloud.google.com/code/docs)
- [Skaffold Documentation](https://skaffold.dev/docs/)
- [GCP Free Tier](https://cloud.google.com/free/docs/gcp-free-tier)
- [Compute Engine Docs](https://cloud.google.com/compute/docs)

## 🤝 Contributing

Contributions welcome! Please submit issues and pull requests.

## 📄 License

MIT License - see LICENSE file for details

## ⭐ Support

If this automation helps, please star the repo!

---

**Ready to deploy?** → Start with [QUICKSTART.md](./QUICKSTART.md)
