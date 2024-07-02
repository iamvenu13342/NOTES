<h1>Building Reusable Infrastructure with Modules</h1>

### Creating Modular Infrastructure with Terraform Modules

**Terraform modules** allow you to organize and reuse infrastructure code. A module is a container for multiple resources that are used together. Modules make your configurations more modular, reusable, and maintainable.

#### Creating a Simple Module

1. **Directory Structure**:
   - Create a directory for your module:
     ```sh
     mkdir -p modules/aws_instance
     ```

2. **Module Files**:
   - `main.tf`: Define your resources.
   - `variables.tf`: Define variables for the module.
   - `outputs.tf`: Define outputs for the module.

   ```hcl
   // main.tf
   resource "aws_instance" "example" {
     ami           = var.ami
     instance_type = var.instance_type
     tags = var.tags
   }
   
   // variables.tf
   variable "ami" {
     description = "The AMI to use for the instance"
     type        = string
   }
   
   variable "instance_type" {
     description = "The type of instance to create"
     type        = string
   }
   
   variable "tags" {
     description = "Tags to apply to the instance"
     type        = map(string)
   }
   
   // outputs.tf
   output "instance_id" {
     value = aws_instance.example.id
   }
   ```

3. **Using the Module**:
   - Create a root configuration file that uses the module.

   ```hcl
   // main.tf in root configuration
   module "web_server" {
     source        = "./modules/aws_instance"
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
     tags = {
       Name        = "ExampleInstance"
       Environment = "Dev"
     }
   }

   output "web_server_instance_id" {
     value = module.web_server.instance_id
   }
   ```

### Local Values and Data Sources

#### Local Values

**Local values** simplify complex expressions and enhance readability by assigning a name to an expression.

- **Example**:
  ```hcl
  locals {
    instance_name = "ExampleInstance-${var.environment}"
  }
  
  resource "aws_instance" "example" {
    ami           = var.ami
    instance_type = var.instance_type
    tags = {
      Name = local.instance_name
    }
  }
  ```

#### Data Sources

**Data sources** fetch data from existing resources or external systems.

- **Example**: Fetching the latest AMI
  ```hcl
  data "aws_ami" "latest" {
    most_recent = true
    owners      = ["amazon"]
    filter {
      name   = "name"
      values = ["amzn2-ami-hvm-*-x86_64-gp2"]
    }
  }
  
  resource "aws_instance" "example" {
    ami           = data.aws_ami.latest.id
    instance_type = var.instance_type
    tags = var.tags
  }
  ```

### Using Variables and Inputs with Modules

Modules can use variables to customize their behavior. This makes them more flexible and reusable.

#### Example: Passing Variables to a Module

- **Define Variables in Module**:
  ```hcl
  // variables.tf in module
  variable "instance_type" {
    description = "The type
