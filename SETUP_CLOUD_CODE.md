# Cloud Code Setup & Deployment Guide

This guide will help you set up Cloud Code and deploy your application to GCP with automated Skaffold configuration.

## Prerequisites

- VS Code installed (https://code.visualstudio.com)
- GCP free account (https://console.cloud.google.com)
- Google Cloud SDK installed (gcloud CLI)
- Docker installed locally
- Node.js 18+ (for Next.js/Node.js projects)

## Step 1: Install Google Cloud SDK

### macOS
```bash
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init
```

### Linux
```bash
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init
```

### Windows
Download: https://cloud.google.com/sdk/docs/install-sdk#windows

## Step 2: Authenticate with GCP

```bash
gcloud auth login
gcloud config set project YOUR_PROJECT_ID
```

Set your project ID:
```bash
GCP_PROJECT_ID="your-gcp-project-id"
gcloud config set project $GCP_PROJECT_ID
```

## Step 3: Install Cloud Code Extension for VS Code

1. Open VS Code
2. Go to Extensions (Cmd+Shift+X on Mac)
3. Search for "Cloud Code"
4. Click "Install" on the official Google Cloud Code extension
5. VS Code will prompt you to reload - click "Reload"

## Step 4: Configure GCP Billing & APIs

### Enable Required APIs:
```bash
gcloud services enable compute.googleapis.com
gcloud services enable container.googleapis.com
gcloud services enable artifactregistry.googleapis.com
gcloud services enable cloudbuild.googleapis.com
```

### Set up Artifact Registry (for container images):
```bash
# Create Artifact Registry repository
REGION="us-west1"
REPO_NAME="cloud-code-repo"

gcloud artifacts repositories create $REPO_NAME \
  --repository-format=docker \
  --location=$REGION

# Verify
gcloud artifacts repositories list --location=$REGION
```

## Step 5: Clone and Run the Deployment Script

```bash
# Clone the repository
git clone https://github.com/cerisontrading-droid/gcp-cloudcode-automation.git
cd gcp-cloudcode-automation

# Set your GCP project ID
export GCP_PROJECT_ID="your-gcp-project-id"
export GCP_REGION="us-west1"

# Run the deployment script
chmod +x deploy-gcp-free-tier.sh
./deploy-gcp-free-tier.sh
```

The script will:
- Create a GCP Compute Engine instance (e2-micro - always free)
- Set up networking (VPC and firewall)
- Configure Skaffold for local development
- Prepare the environment for Cloud Code deployment

## Step 6: Set Up Your Application for Cloud Code

### For Next.js Projects:
1. Copy the `skaffold.yaml` to your application directory
2. Create a `Dockerfile` in your project root:

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

3. Create a `.dockerignore`:
```
node_modules
.git
.env.local
.next
dist
```

### For Node.js Projects:
1. Create a `Dockerfile`:

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

2. Copy the `skaffold.yaml` and update the image name

## Step 7: Launch Cloud Code in VS Code

1. Open your application folder in VS Code
2. Open the Command Palette (Cmd+Shift+P)
3. Type: `Cloud Code: Run on Kubernetes cluster`
4. Or use the Cloud Code sidebar
5. Select your deployment configuration
6. Cloud Code will automatically:
   - Build your Docker image
   - Push to Artifact Registry
   - Deploy using Skaffold
   - Stream logs in VS Code

## Step 8: Monitor and Debug

### View Logs in VS Code:
- Click the "Cloud Code" icon in the VS Code sidebar
- Expand your deployment to see logs
- Click any log entry to see details

### SSH into Your Instance:
```bash
gcloud compute ssh instance-name --zone=us-west1-b
```

### View GCP Console:
```bash
# Open in browser
gcloud console
```

## Environment Variables Configuration

1. Copy `.env.example` to `.env.local`:
```bash
cp .env.example .env.local
```

2. Edit `.env.local` with your values:
```bash
GCP_PROJECT_ID=your-project-id
GCP_REGION=us-west1
DOCKER_REGISTRY_REGION=us-west1
ARTIFACT_REPO=cloud-code-repo
APP_PORT=3000
```

3. For production, update the skaffold.yaml:
```yaml
env:
  - name: GCP_PROJECT_ID
    value: ${GCP_PROJECT_ID}
  - name: NODE_ENV
    value: production
```

## Skaffold Configuration Breakdown

The `skaffold.yaml` includes:

```yaml
apiVersion: skaffold/v4beta6
kind: Config
metadata:
  name: gcp-cloud-code-automation

build:
  artifacts:
    - image: DOCKER_IMAGE_NAME
      docker:
        dockerfile: Dockerfile

deploy:
  kubectl:
    defaultNamespace: default
```

## Cost Optimization Tips

### Stay in the Always-Free Tier:
- Use `e2-micro` instances (1 vCPU, 1GB memory)
- Limited to 1 instance per month
- Standard storage only (30GB)
- US regions (us-west1, us-central1, us-east1)

### Monitor Costs:
```bash
gcloud billing accounts list
gcloud compute instances list
gcloud compute disks list
```

## Troubleshooting

### Docker build fails locally:
```bash
# Clear Docker cache
docker system prune -a

# Rebuild
docker build -t app:latest .
```

### Cloud Code can't connect to GCP:
```bash
# Re-authenticate
gcloud auth login
gcloud auth application-default login

# Verify configuration
gcloud config list
```

### Skaffold issues:
```bash
# Install latest Skaffold
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64
chmod +x skaffold
sudo mv skaffold /usr/local/bin

# Test Skaffold
skaffold version
```

### Port conflicts:
```bash
# Update skaffold.yaml to use different port:
ports:
  - port: 8080
    protocol: TCP
```

## Next Steps

1. **Deploy your first app**: Follow Step 6 and 7 with your application
2. **Set up CI/CD**: Use Cloud Build for automated deployments
3. **Enable monitoring**: Set up Cloud Logging and Cloud Monitoring
4. **Configure custom domains**: Use Cloud DNS for domain management

## Resources

- Cloud Code Documentation: https://cloud.google.com/code/docs
- Skaffold Docs: https://skaffold.dev/docs
- GCP Always-Free Guide: https://cloud.google.com/free/docs/always-free-usage-limits
- Next.js Deployment: https://nextjs.org/docs/deployment/vercel

## Support & Issues

If you encounter issues:
1. Check GCP quotas: `gcloud compute quotas list`
2. Review Cloud Build logs: `gcloud builds log [BUILD_ID]`
3. Check application logs: `kubectl logs -f deployment/[APP_NAME]`

---

**Happy Deploying! 🚀**
