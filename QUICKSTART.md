# GCP Free Tier + Cloud Code - Quick Start

## 🚀 Get Started in 3 Steps

### Step 1: Clone & Initialize
```bash
git clone https://github.com/cerisontrading-droid/gcp-cloudcode-automation.git
cd gcp-cloudcode-automation
export GCP_PROJECT_ID="your-project-id"
export GCP_INSTANCE_NAME="cloudcode-instance"
export GCP_REGION="us-west1"
export GCP_ZONE="us-west1-b"
```

### Step 2: Deploy Infrastructure
```bash
chmod +x deploy-gcp-free-tier.sh
./deploy-gcp-free-tier.sh
```

### Step 3: Deploy with Cloud Code
```bash
# VS Code: Cmd + Shift + P > Cloud Code: Deploy
# Or run Skaffold directly:
skaffold run --platform=gcp --default-repo=gcr.io/$GCP_PROJECT_ID
```

## 📊 Configuration

Edit `.env` with your settings:
- `GCP_PROJECT_ID`: Your GCP project
- `GCP_INSTANCE_NAME`: VM instance name
- `GCP_REGION`: us-west1, us-east1, or us-central1
- `GCP_ZONE`: us-west1-b (for us-west1 region)

## ✅ What's Deployed

- **Compute Engine**: e2-micro (1GB RAM, 2 vCPU)
- **Storage**: 30GB Standard persistent disk
- **Network**: Standard tier (cost-optimized)
- **Cost**: $0/month (Always-Free eligible)

## 🔗 Important Links

- [Full Documentation](./docs/CLOUDCODE-SETUP.md)
- [Skaffold Config](./skaffold.yaml)
- [GCP Free Tier Info](https://cloud.google.com/free/docs/gcp-free-tier)
- [Cloud Code Docs](https://cloud.google.com/code/docs)

## 📝 Files

- `deploy-gcp-free-tier.sh` - Infrastructure automation
- `skaffold.yaml` - Cloud Code configuration
- `.env.example` - Environment template
- `QUICKSTART.md` - This file

## 🆘 Troubleshooting

**SSH fails?**
```bash
gcloud compute ssh $GCP_INSTANCE_NAME --zone=$GCP_ZONE --project=$GCP_PROJECT_ID
```

**Deployment hangs?**
```bash
gcloud compute instances describe $GCP_INSTANCE_NAME --zone=$GCP_ZONE
```

**Cost concerns?**
All settings are Always-Free eligible. Monitor at: Google Cloud Console > Billing
