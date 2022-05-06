![image](https://user-images.githubusercontent.com/3519706/165941838-b9d2b207-f30a-4b86-8fc6-fd1f5263f792.png)
![image](https://user-images.githubusercontent.com/3519706/165941970-79645de1-810b-42b4-ad57-7384a659f687.png)
```yaml
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
