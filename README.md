# Resume Lambda Go

A Go implementation of a serverless AWS Lambda function with local development environment using Kubernetes and Tilt.

## Features

- **Go Lambda Function**: High-performance Lambda function built with Go
- **Local Development**: Full Kubernetes development environment with Tilt
- **API Documentation**: Interactive API documentation using Scalar UI
- **Infrastructure as Code**: Terraform for AWS resource management
- **CI/CD Pipeline**: GitHub Actions for automated deployment
- **Container Support**: Docker containers for both Lambda and interface services

## Quick Start

### Prerequisites

- Go 1.23+
- Docker & Kubernetes (Docker Desktop recommended)
- [Tilt](https://docs.tilt.dev/install.html)
- [Just](https://github.com/casey/just) command runner
- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [AWS CLI](https://aws.amazon.com/cli/) (configured)
- [GitHub CLI](https://cli.github.com/) (for deployment setup)

### Local Development

1. **Clone and setup**:
   ```bash
   git clone <repository-url>
   cd resume-lambda-go
   cp .env.example .env
   # Edit .env with your configuration
   ```

2. **Start local development**:
   ```bash
   just start
   ```

3. **Access services**:
   - Lambda API: http://localhost:18035
   - API Documentation: http://localhost:18030

### Available Commands

```bash
just --list                 # Show all available commands

# Development
just build                  # Build Go binary
just test                   # Run tests
just clean                  # Clean build artifacts
just fmt                    # Format code
just deps                   # Install dependencies

# Local Environment
just start                  # Start Tilt development environment
just stop                   # Stop Tilt

# AWS Deployment
just setup                  # Complete infrastructure setup
just console                # Open AWS console
just teardown              # Destroy infrastructure
```

## Architecture

### Local Development
- **Lambda Service**: Go application running in Kubernetes
- **Interface Service**: Nginx serving API documentation
- **Port Forwarding**: 
  - 18035 → Lambda service (8080)
  - 18030 → Interface service (80)

### AWS Production
- **Lambda Function**: Serverless Go binary (`provided.al2023` runtime)
- **S3 Bucket**: Artifact storage with versioning
- **DynamoDB**: Application data storage
- **CloudWatch**: Logging and monitoring
- **IAM Roles**: Secure service permissions

## Directory Structure

```
├── src/                    # Go source code
│   └── main.go            # Lambda handler
├── terraform/             # Infrastructure as Code
│   ├── main.tf           # AWS resources
│   ├── variables.tf      # Configuration
│   └── backend.tf        # S3 state backend
├── interface/             # API documentation UI
│   ├── index.html        # Scalar UI interface
│   ├── openapi.yaml      # API specification
│   └── nginx.conf        # Nginx configuration
├── manifests/             # Kubernetes/Helm charts
│   ├── Chart.yaml        # Helm chart metadata
│   ├── values.yaml       # Configuration values
│   └── templates/        # K8s resource templates
├── .github/workflows/     # CI/CD pipeline
└── Tiltfile              # Local development orchestration
```

## Deployment

### Initial Setup
1. Configure AWS credentials: `aws configure`
2. Login to GitHub CLI: `gh auth login`
3. Run complete setup: `just setup`

This will:
- Create S3 bucket for Terraform state
- Deploy AWS infrastructure
- Configure GitHub repository secrets
- Set up CI/CD pipeline

### Continuous Deployment
Push to `main` branch triggers automatic deployment via GitHub Actions.

## API Documentation

The service includes interactive API documentation available at http://localhost:18030 during local development. The API specification is defined in `interface/openapi.yaml`.

## Environment Variables

Copy `.env.example` to `.env` and configure:

- `PROJECT_NAME`: Project identifier for resource naming
- `AWS_REGION`: AWS region for deployment
- `TERRAFORM_STATE_BUCKET`: S3 bucket for Terraform state

## Contributing

1. Make changes to the Go code in `src/`
2. Test locally with `just start`
3. Run tests with `just test`
4. Format code with `just fmt`
5. Commit and push to trigger deployment# marcstreeterdev-manifests-lambdagolang
