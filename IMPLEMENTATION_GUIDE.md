# Complete Implementation Guide: GCP Free Tier + Cloud Code Automation

## 🎯 Project Overview

This repository provides a **complete, production-ready automation package** for deploying Next.js, Node.js, and containerized applications to Google Cloud Platform's Always-Free tier using Cloud Code.

**Key Achievement**: Deploy and manage applications with ZERO monthly costs using GCP's Always-Free tier.

---

## 📦 What's Included

### Core Files
1. **deploy-gcp-free-tier.sh** - Automated GCP infrastructure setup script
2. **skaffold.yaml** - Kubernetes/Cloud Code deployment configuration
3. **Dockerfile.nextjs** - Multi-stage Docker build optimized for Next.js
4. **docker-compose.yml** - Local development environment with optional PostgreSQL
5. **SETUP_CLOUD_CODE.md** - Detailed Cloud Code configuration guide
6. **QUICKSTART.md** - Quick start guide for immediate setup
7. **.env.example** - Environment variables template
8. **.gitignore** - Standard Node.js/Next.js gitignore

---

## 🚀 Quick Start (5 Minutes)

### Step 1: Prerequisites
```bash
# Verify installations
node --version        # v18+
docker --version      # Latest
gcloud --version      # Latest
git --version         # Latest
```

### Step 2: Clone & Setup
```bash
git clone https://github.com/cerisontrading-droid/gcp-cloudcode-automation.git
cd gcp-cloudcode-automation

# Copy environment variables
cp .env.example .env.local
```

### Step 3: GCP Authentication
```bash
# Login to GCP
gcloud auth login

# Create a new GCP project
gcloud projects create my-project-id

# Set as default
gcloud config set project my-project-id

# Enable billing (Free tier requires billing account)
gcloud beta billing accounts list
```

### Step 4: Run Deployment Script
```bash
# Make executable
chmod +x deploy-gcp-free-tier.sh

# Run with your project ID
export GCP_PROJECT_ID="my-project-id"
./deploy-gcp-free-tier.sh
```

The script will:
- ✅ Create e2-micro Compute Engine instance
- ✅ Set up VPC and firewall rules
- ✅ Configure networking for Cloud Code
- ✅ Initialize Skaffold configuration
- ✅ Prepare for Cloud Code deployment

### Step 5: Install Cloud Code Extension
1. Open VS Code
2. Go to Extensions (Cmd+Shift+X on Mac)
3. Search for "Cloud Code"
4. Click Install
5. Reload VS Code

### Step 6: Deploy Your App
1. Open your app folder in VS Code
2. Copy Dockerfile.nextjs and docker-compose.yml to your project
3. Press Cmd+Shift+P and search "Cloud Code: Run on Kubernetes"
4. Select deployment configuration
5. Cloud Code will build, push, and deploy automatically

---

## 📋 Architecture Overview

### Infrastructure Stack
```
┌─────────────────────────────────────────┐
│     Your Application (Next.js/Node.js)  │
└──────────────┬──────────────────────────┘
               │
               ↓
      ┌────────────────┐
      │  Cloud Code    │ (VS Code Extension)
      │  + Skaffold    │
      └────────┬───────┘
               │
               ↓
      ┌────────────────────┐
      │  Artifact Registry │ (Container Images)
      │  (Free: 0.50/GB)   │
      └────────┬───────────┘
               │
               ↓
      ┌─────────────────────────┐
      │  e2-micro Instance      │
      │  (Always Free)          │
      │  - 1 vCPU, 1GB RAM      │
      │  - 30GB Standard Disk   │
      │  - $0/month             │
      └─────────────────────────┘
```

### Technology Stack
- **Container Runtime**: Docker
- **Deployment Orchestration**: Skaffold
- **Development Tool**: Cloud Code (VS Code)
- **Infrastructure**: GCP Compute Engine
- **Container Registry**: Artifact Registry
- **Networking**: Cloud VPC & Cloud Firewall

---

## 💰 Cost Breakdown (Always-Free Tier)

| Service | Free Tier | Monthly Cost |
|---------|-----------|---------------|
| Compute Engine (e2-micro) | 1 instance/month | $0 |
| Persistent Disk (30GB) | 30GB | $0 |
| Network Ingress | First 1GB/month | $0 |
| Cloud Build | First 120 min/day | $0* |
| Cloud Logging | First 50GB/month | $0 |
| **Total** | - | **$0/month** |

*Requires optional Cloud Build setup for CI/CD

---

## 🔧 Advanced Usage

### Using with PostgreSQL
```bash
# Start app with database
docker-compose --profile database up -d

# Access database
docker exec -it gcp-cloud-code-db psql -U postgres -d app_db
```

### Custom Environment Variables
Edit `.env.local`:
```bash
GCP_PROJECT_ID=your-project-id
GCP_REGION=us-west1
NODE_ENV=production
DATABASE_URL=postgresql://user:pass@db:5432/app
```

### Scaling Beyond Free Tier
To scale to production (with costs):

1. **Upgrade Instance**: Change e2-micro to e2-standard-2
2. **Add Cloud SQL**: Managed PostgreSQL database
3. **Enable Cloud CDN**: Global edge caching
4. **Set up Cloud Monitoring**: Production observability
5. **Configure Cloud Armor**: DDoS protection

---

## 🛠️ Troubleshooting

### "Authentication required"
```bash
gcloud auth login
gcloud auth application-default login
```

### "Quota exceeded"
Check quotas:
```bash
gcloud compute quotas list
```

### "Docker build fails"
```bash
# Clear Docker cache
docker system prune -a

# Rebuild image
docker build -t app:latest .
```

### "Cloud Code can't connect"
Re-authenticate:
```bash
# Clear credentials
rm -rf ~/.config/gcloud

# Re-authenticate
gcloud auth login
```

---

## 📚 File Descriptions

### deploy-gcp-free-tier.sh
Automated bash script that:
- Creates GCP Compute Engine instance (e2-micro)
- Configures VPC and firewall
- Sets up Cloud SDK
- Validates Always-Free eligibility

### skaffold.yaml
Defines how Skaffold:
- Builds Docker images
- Manages artifacts
- Configures deployment
- Handles image tagging

### Dockerfile.nextjs
Multi-stage Docker build:
- Stage 1: Dependencies (npm ci)
- Stage 2: Builder (npm run build)
- Stage 3: Runtime (optimized image)

### docker-compose.yml
Local development configuration:
- Next.js app service (port 3000)
- Optional PostgreSQL (port 5432)
- Shared network for communication
- Volume mounts for live reload

---

## 🔒 Security Best Practices

### Environment Variables
✅ **Do**: Use `.env.local` for sensitive data
```bash
GCP_SERVICE_ACCOUNT_KEY=/path/to/key.json
DATABASE_PASSWORD=secure_password
```

❌ **Don't**: Commit secrets to Git
```bash
# Ensure .gitignore includes:
.env
.env.local
.env.*.local
```

### Container Security
✅ Use non-root user in Dockerfile
✅ Scan images with: `gcloud container images scan`
✅ Use specific base image versions (not `latest`)

### Network Security
✅ Configure firewall rules (port 443 for HTTPS)
✅ Enable Cloud Armor for DDoS protection
✅ Use Cloud VPN for private connections

---

## 📖 Additional Resources

- **Cloud Code Docs**: https://cloud.google.com/code/docs
- **Skaffold Documentation**: https://skaffold.dev/docs
- **GCP Always-Free**: https://cloud.google.com/free/docs/always-free-usage-limits
- **Next.js on GCP**: https://cloud.google.com/solutions/deploy-next-js-cloud-run
- **Docker Best Practices**: https://docs.docker.com/develop/dev-best-practices/

---

## 📝 Project Status

- ✅ Automated infrastructure setup
- ✅ Cloud Code integration
- ✅ Skaffold configuration
- ✅ Docker multi-stage build
- ✅ Local dev environment
- ✅ Production-ready setup
- 🚀 Ready for deployment

---

## 🤝 Contributing

Improvements welcome! Please:
1. Test locally with docker-compose
2. Validate GCP deployment
3. Update documentation
4. Submit pull request

---

## 📄 License

MIT License - Free to use and modify

---

**Built with ❤️ for developers who want enterprise-grade deployments at zero cost.**

Start deploying now: `./deploy-gcp-free-tier.sh`
