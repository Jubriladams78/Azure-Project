# Azure Blob Storage Static Website Lab

> **Tech Stack:** Azure Blob Storage · Azure CLI · Azure Portal · Static Web Hosting
> **Author:** Jubril Adams | AZ-104 Lab Series

## Overview

This lab demonstrates deploying a public-facing static website using **Azure Blob Storage** with static website hosting enabled. Instead of provisioning a full VM or App Service, this project showcases cost-effective, serverless-style web hosting using Azure Storage.

This is part of my hands-on AZ-104 preparation lab series.

---

## Architecture

```
Internet
   │
   ▼
Azure Blob Storage Account
   └── $web container (static website hosting enabled)
         ├── index.html
         └── 404.html
```

---

## Lab Objectives

- [x] Create an Azure Resource Group
- [x] Provision a Storage Account with LRS redundancy
- [x] Enable Static Website hosting on the Storage Account
- [x] Upload web content to the `$web` container
- [x] Configure public access and CORS rules
- [x] Verify the website is publicly accessible via the Azure-provided endpoint
- [x] Review access logs and monitor blob metrics via Azure Monitor

---

## Steps Performed

### 1. Create Resource Group
```bash
az group create \
  --name rg-static-site-lab \
  --location eastus
```

### 2. Create Storage Account
```bash
az storage account create \
  --name ststaticsite$RANDOM \
  --resource-group rg-static-site-lab \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2 \
  --allow-blob-public-access true
```

### 3. Enable Static Website Hosting
```bash
az storage blob service-properties update \
  --account-name <storage-account-name> \
  --static-website \
  --index-document index.html \
  --404-document 404.html
```

### 4. Upload Content
```bash
az storage blob upload-batch \
  --account-name <storage-account-name> \
  --source ./website \
  --destination '$web'
```

---

## Screenshots

Screenshots of each step are included in this repository:
- Resource Group creation
- Storage Account configuration
- Static website endpoint verification
- Blob container and uploaded files

---

## Key Concepts Demonstrated

| Concept | Details |
|---|---|
| Azure Storage Account | StorageV2, Standard LRS |
| Static Website Hosting | `$web` container with index/404 docs |
| Public Blob Access | Configured at container level |
| Azure CLI | Provisioning and deployment via CLI |
| Cost Optimization | Storage-based hosting vs VM/App Service |

---

## Lessons Learned

- Azure Blob Storage static hosting is ideal for single-page apps and documentation sites with minimal cost
- CORS rules must be explicitly configured for cross-origin access
- The `$web` container is auto-created when static website hosting is enabled
- Storage account names must be globally unique and 3–24 lowercase alphanumeric characters

---

## Related Resources

- [Azure Static Website Documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)
- [AZ-104 Study Guide](https://learn.microsoft.com/en-us/certifications/exams/az-104)

---

*Part of the Jubril Adams AZ-104 Lab Portfolio — [View All Projects](https://github.com/Jubriladams78)*
