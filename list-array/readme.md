```tf
variable "region" {
  type        = list(string)
  default     = [
    "westeurope",
    "germanywestcentral",
    "canadacentral"
 ]
}

resource "azurerm_resource_group" "rg" {
  count = length(var.region)
  name     = var.rg
  location = var.deneme[count.index]

  tags = local.common_tags
}
```
