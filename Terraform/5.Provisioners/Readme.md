<h1>Provisioning and Provisioners</h1>

### Understanding Provisioners in Terraform

**Provisioners** in Terraform are mechanisms that allow you to execute scripts on a local or remote machine during resource creation or destruction. They are often used for bootstrapping, configuration management, or performing other post-deployment tasks.

### Remote-exec and Local-exec Provisioners

#### Local-exec Provisioner

The **local-exec** provisioner executes commands on the machine where Terraform is being run. This is useful for tasks such as running local scripts or commands after a resource has been created.

- **Example**:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    provisioner "local-exec" {
      command = "echo ${aws_instance.example.public_ip} >> ip_addresses.txt"
    }
  }
  ```

#### Remote-exec Provisioner

The **remote-exec** provisioner executes commands on a remote machine, typically the instance being provisioned. It requires an SSH or WinRM connection.

- **Example**:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    connection {
      type        = "ssh"
      user        = "ubuntu"
      private_key = file("path/to/private-key")
      host        = self.public_ip
    }

    provisioner "remote-exec" {
      inline = [
        "sudo apt-get update",
        "sudo apt-get install -y nginx"
      ]
    }
  }
  ```

### Applying Provisioners at Creation and Destruction

Provisioners can be specified to run during the creation or destruction of a resource.

#### During Resource Creation

Provisioners are often used to set up software, create directories, or perform other configuration tasks when a resource is created.

- **Example**:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    provisioner "remote-exec" {
      inline = [
        "sudo apt-get update",
        "sudo apt-get install -y nginx"
      ]
    }
  }
  ```

#### During Resource Destruction

Provisioners can also be used to perform cleanup tasks when a resource is destroyed.

- **Example**:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    provisioner "local-exec" {
      when    = "destroy"
      command = "echo 'Instance ${self.id} is being destroyed' >> destroy_log.txt"
    }
  }
  ```

### Failure Handling for Provisioners

Handling provisioner failures is important to ensure robust and reliable infrastructure management. Terraform provides several mechanisms to control the behavior of provisioners upon failure.

#### Retry Mechanisms

You can specify the number of retries and the interval between them if a provisioner fails.

- **Example**:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    provisioner "remote-exec" {
      inline = [
        "sudo apt-get update",
        "sudo apt-get install -y nginx"
      ]
      retries = 3
      retry_interval = "30s"
    }
  }
  ```

#### Timeouts

You can specify a timeout for provisioners to avoid hanging indefinitely.

- **Example**:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    provisioner "remote-exec" {
      inline = [
        "sudo apt-get update",
        "sudo apt-get install -y nginx"
      ]
      timeout = "5m"
    }
  }
  ```

#### On Failure

The `on_failure` attribute defines the behavior if the provisioner fails. It can be set to `continue` (to proceed with the rest of the plan) or `fail` (to stop execution).

- **Example**:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    provisioner "remote-exec" {
      inline = [
        "sudo apt-get update",
        "sudo apt-get install -y nginx"
      ]
      on_failure = "continue"
    }
  }
  ```

### Summary

On Day 5, you've learned about the power of provisioners in Terraform, including the differences between `remote-exec` and `local-exec` provisioners. You've explored how to apply provisioners during resource creation and destruction, and how to handle failures with retry mechanisms, timeouts, and the `on_failure` attribute. This knowledge allows you to customize and automate your infrastructure management more effectively.
