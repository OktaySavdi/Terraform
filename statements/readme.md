```tf
variable "public_ip_enabled" {
  type        = string
  default     = "false"
}

resource "azurerm_public_ip" "primary" {
  count = (var.public_ip_enabled == true ? 1 : 0)

  name                = local.virtual_machine_name
  location            = var.names.location
  resource_group_name = var.resource_group_name
  tags                = var.tags
}
```
```tf
resource "tls_private_key" "ssh_key" {
  count = (var.kernel_type == "linux" && var.admin_ssh_public_key == "" ? 1 : 0)

  algorithm = "RSA"
  rsa_bits  = 4096
}
```
