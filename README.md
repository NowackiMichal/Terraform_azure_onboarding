# Terraform_onboarding

## Step-01: Introduction
- Understand basic Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create Resources
terraform apply 

# Terraform Destroy to Destroy Resources
terraform destroy 
```

## Step-02: Review terraform manifests
- Pre-Conditions-1: Get Azure Regions and decide the region where you want to create resources
```t
# Get Azure Regions
az account list-locations -o table
```
- **Pre-Conditions-2:** If not done earlier, complete `az login` via Azure CLI. We are going to use Azure CLI Authentication for Terraform when we use Terraform Commands. 
```t
# Azure CLI Login
az login

# List Subscriptions
az account list

# Set Specific Subscription (if we have multiple subscriptions)
az account set --subscription="SUBSCRIPTION_ID"
```
```t
# Terraform Settings Block
terraform {
  required_version = ">= 1.0.0"
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = ">= 2.0" # Optional but recommended in production
    }    
  }
}
# Configure the Microsoft Azure Provider
provider "azurerm" {
  features {}
}
# Create Resource Group 
resource "azurerm_resource_group" "my_demo_rg1" {
  location = "eastus"
  name = "my-demo-rg1"  
}
```
## Step-03: Terraform Configuration Language Syntax
1. Blocks
2. Arguments, Attributes & Meta-Arguments
3. Identifiers
```t
# Template
<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>"   {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}

# Azure Example
# Create a resource group
resource "azurerm_resource_group" "myrg" { # Resource BLOCK
  name = "myrg-1" # Argument
  location = "East US" # Argument 
}
# Create Virtual Network
resource "azurerm_virtual_network" "myvnet" { # Resource BLOCK
  name                = "myvnet-1" # Argument
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.myrg.location # Argument with value as expression
  resource_group_name = azurerm_resource_group.myrg.name # Argument with value as expression
}
```
- Arguments can be `required` or `optional`
- Attribues (output parameters - values we can reference after resource is created) format looks like `resource_type.resource_name.attribute_name`
- Meta-Arguments change a resource type's behavior (Example: count, for_each)

Terraform Top-Level blocks:
1. Terraform Settings Block
2. Provider Block
3. Resource Block
4. Input Variables Block
5. Output Values Block
6. Local Values Block
7. Data Sources Block
8. Modules Block
