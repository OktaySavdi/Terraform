example1
```tf
variable "define_group" {
  type = map(any)
  default = {
    group1 = {
      group_name    = "mygroupname1"
      role          = "projectadmin"
      ldap_group_dn = "CN=my_group1,OU=my_ou,DC=com,DC=tr"
    }
    group2 = {
      group_name    = "mygroupname2"
      role          = "developer"
      ldap_group_dn = "CN=my_group2,OU=my_ou,DC=com,DC=tr"
    }
  }
}
```
```tf
resource "harbor_project_member_group" "main" {
  project_id    = harbor_project.main.id
  for_each      = var.define_group
  group_name    = each.value.group_name
  role          = each.value.role
  type          = "ldap"
  ldap_group_dn = each.value.ldap_group_dn
}
```
example2
```tf
variable "ip-config" {
  default = [
    {
     name = "instance-ip1"
     allocation = "Dynamic"
     primary = true
    },
    {
     name = "instance-ip2"
     allocation = "Dynamic"
     primary = true
    }
  ]
}

resource "azurerm_network_interface" "example" {
  name                = "${var.prefix}-instance1"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  dynamic ip_configuration {
    for_each = var.ip-config
    content {
      name                          = lookup(ip_configuration.value, "name")
      subnet_id                     = azurerm_subnet.example.id
      private_ip_address_allocation = lookup(ip_configuration.value, "allocation")
      primary                       = lookup(ip_configuration.value, "primary")
    }
  }
}
```
