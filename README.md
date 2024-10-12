# CICD Pipeline for Docker Image Build and Security Scan

## Overview
This project implements a Continuous Integration/Continuous Deployment (CICD) pipeline using GitHub Actions to automate the processes of building, security scanning, and signing Docker images. It also integrates Slack notifications to keep users informed about the pipeline’s progress and status updates.

## How to Use
### 1. Pipeline Overview:
   - **Branch Trigger**: The pipeline is triggered on pushes to branches matching the pattern `release/v[0-9]+.[0-9]+`.
   - **Security Scan**: A Trivy vulnerability scan is performed on the repository. If critical vulnerabilities are found, the pipeline stops, and the report is sent to Slack.
   - **Docker Image Build**: If the security scan passes, the Docker image is built and pushed to Docker Hub. The image is signed using Cosign for security.
   - **Slack Notifications**: Notifications are sent to Slack to report the build status and any scan failures.

### 2. Set Up Environment Variables:
   Make sure the following secrets are set in the GitHub repository:
   - **DOCKERHUB_USERNAME**: Docker Hub username.
   - **DOCKERHUB_TOKEN**: Docker Hub token.
   - **COSIGN_PRIVATE_KEY**: Cosign private key.
   - **SLACK_WEBHOOK_URL**: Slack Webhook URL for notifications.
   - **SLACK_TOKEN**: Slack token for uploading files.
   - **SLACK_CHANNEL_ID**: Slack channel ID for uploading files.

### 3. Running the Pipeline:
   The pipeline runs automatically when pushing to a `release/vX.Y` branch. If want to skip the security scan for a particular commit, include `#NORUN` in the commit message.

### 4. Slack Notifications:
   - Success notifications include Docker Hub image links.
   - Failure notifications contain vulnerability reports and are sent automatically to the Slack channel specified.


## Command line options

| Option    | Description                        | Default       |
|-----------|------------------------------------|---------------|
|`--fortune`| Fortune file, one fortune per line | ./fortune.txt |
|`--static` | Static assets directory            | ./static      |
|`--port`   | Port to bind to                    | 3000          |

## Versions and Container Images
The release version corresponds to the container image version

## Public key

The public key to verify the image is in the repository `cosign.pub`. To verify the image use [cosign](https://github.com/sigstore/cosign)

```
cosign verify --key cosign.pub ghcr.io/chukmunnlee/go-fortune:<tag>
```
