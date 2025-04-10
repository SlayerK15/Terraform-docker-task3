# Infrastructure as Code (IaC) with Terraform

## Task Overview
This repository contains my solution for Task 3 of the DevOps Internship program with Elevate Labs and MSME. The objective was to provision a local Docker container using Terraform, demonstrating the concept of Infrastructure as Code (IaC).

## Project Structure
- `main.tf` - The main Terraform configuration file
- `logs` - Execution logs for the Terraform commands
- `.gitignore` - File specifying which files/directories to ignore in version control

## Technologies Used
- **Terraform** - Used for infrastructure provisioning
- **Docker** - Container platform
- **NGINX** - Web server deployed in the container

## Implementation Details

### Terraform Configuration
The `main.tf` file is configured to:
1. Use the Docker provider from kreuzwerker
2. Pull the latest NGINX image
3. Create a container named "tutorial" with port 80 mapped to port 8000 on the host machine

```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {
  host = "npipe:////.//pipe//docker_engine"
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

### Steps Executed
1. **Initialization**: Used `terraform init` to initialize the working directory and download the required providers.
2. **Planning**: Used `terraform plan` to preview the actions Terraform would take to create the infrastructure.
3. **Application**: Used `terraform apply` to create the Docker image and container according to the configuration.
4. **Verification**: Verified the running container by accessing NGINX on `http://localhost:8000`.
5. **Destruction**: Used `terraform destroy` to clean up all resources when finished.

### Challenges Encountered
Initially encountered connection issues with Docker on Windows. Resolved by using the correct named pipe path for the Docker host:
```hcl
provider "docker" {
  host = "npipe:////.//pipe//docker_engine"
}
```

## Key Learning Outcomes
- Understanding the Infrastructure as Code concept and its benefits
- Learning to use Terraform for infrastructure provisioning
- Managing infrastructure lifecycle (create, update, destroy)
- Working with Terraform state management
- Configuring providers and resources
- Troubleshooting provider-specific connection issues

## Conclusion
This task demonstrated how to use Terraform to manage infrastructure as code, focusing on a simple use case of provisioning a Docker container. The implementation showcases the declarative nature of Terraform configurations and the workflow for creating and managing infrastructure. This approach enables consistent, repeatable, and version-controlled infrastructure deployments.