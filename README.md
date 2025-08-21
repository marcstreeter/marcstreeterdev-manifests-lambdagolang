# Lambda Golang Template

This is a copier template for creating AWS Lambda functions using Golang.

## Features

- AWS Lambda function with Golang runtime
- Local development with Tilt and Docker
- Helm charts for Kubernetes deployment
- Terraform for AWS infrastructure
- GitHub Actions for CI/CD
- Interface service for local testing

## Usage

```bash
copier copy /path/to/templates/lambdagolang /path/to/new-project
```

## Template Variables

- `project_name`: The name of your project
- `project_description`: Short description of the project  
- `author_name`: Your name
- `author_email`: Your email
- `github_username`: Your GitHub username
- `lambda_timeout`: Lambda timeout in seconds (default: 30)
- `lambda_memory_size`: Lambda memory in MB (default: 128)
- `aws_region`: AWS region for deployment (default: us-east-1)
- `tilt_interface_port`: Local development port (default: 18080)

## Generated Structure

The template creates a complete Lambda project with:

- `src/`: Golang source code
- `terraform/`: AWS infrastructure as code
- `manifests/`: Helm charts for Kubernetes
- `interface/`: Local development interface
- `justfile`: Task automation
- `Dockerfile`: Container configuration