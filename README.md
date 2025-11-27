## 🌐 **Terraform & HCL Basics**

Terraform is an Infrastructure as Code (IaC) tool that lets you define cloud infrastructure using code.
The language used to write Terraform configurations is called HCL (HashiCorp Configuration Language).

Key points about HCL:

Human-readable and declarative.

Blocks define resources, variables, providers, etc.

Supports expressions, lists, maps, and functions.

File extension: .tf

## 🏗 **HCL Syntax**

HCL is mostly block-based, with key-value pairs inside blocks.

Basic structure:

```hcl
resource "RESOURCE_TYPE" "NAME" {
  key = "value"
  another_key = 123
}
```

## **1️⃣ Providers**

Define which cloud or service provider to use.

```hcl
provider "aws" {
  region = "us-east-1"
}

provider "google" {
  project = "my-gcp-project"
  region  = "us-central1"
}
```

---

## **2️⃣ Resources**

Define infrastructure resources.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "MyInstance"
  }
}
```

* Syntax: `resource "TYPE" "NAME" { ... }`
* Each resource creates one infrastructure object.

---

## **3️⃣ Data Sources**

Read existing infrastructure.

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}
```

---

## **4️⃣ Variables**

Input values to your Terraform configuration.

```hcl
variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
  default     = "t2.micro"
}

variable "allowed_ips" {
  type = list(string)
  default = ["0.0.0.0/0"]
}
```

* Use: `var.instance_type`

---

## **5️⃣ Outputs**

Export values after `terraform apply`.

```hcl
output "instance_ip" {
  value       = aws_instance.example.public_ip
  description = "Public IP of the EC2 instance"
}
```

* Can use in other modules with `module.<name>.<output>`

---

## **6️⃣ Locals**

Define reusable values.

```hcl
locals {
  env       = "production"
  full_name = "${local.env}-app"
}
```

---

## **7️⃣ Modules**

Reuse Terraform code.

```hcl
module "vpc" {
  source = "./modules/vpc"

  cidr_block = "10.0.0.0/16"
}
```

* Modules can be **local** or **remote (registry, Git)**.

---

## **8️⃣ Expressions & Types**

| Type    | Example                                |
| ------- | -------------------------------------- |
| String  | `"hello"`                              |
| Number  | `42`                                   |
| Boolean | `true` / `false`                       |
| List    | `["a", "b", "c"]`                      |
| Map     | `{ key1 = "value1", key2 = "value2" }` |

**Common expressions:**

```hcl
"${var.name}-server"   # string interpolation
length(var.list)       # list length
lookup(var.map, "key", "default")  # get value from map
```

---

## **9️⃣ Functions**

Terraform has built-in functions:

* **String:** `upper("abc")`, `lower("ABC")`, `trimspace(" text ")`
* **Numeric:** `min(2,5)`, `max(10,3)`
* **Collection:** `length(list)`, `merge(map1, map2)`, `concat(list1, list2)`
* **File:** `file("path/to/file")`, `filebase64("file.txt")`

---

## **🔟 Meta-Arguments**

Commonly used with resources and modules:

* `depends_on` – force dependency between resources
* `count` – create multiple instances
* `for_each` – iterate over a list/map

```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

---

## **11️⃣ Terraform Commands Cheat Sheet**

| Command              | Description                              |
| -------------------- | ---------------------------------------- |
| `terraform init`     | Initialize a Terraform working directory |
| `terraform validate` | Check syntax                             |
| `terraform fmt`      | Format code according to HCL standards   |
| `terraform plan`     | Preview changes                          |
| `terraform apply`    | Apply changes                            |
| `terraform destroy`  | Delete infrastructure                    |
