example1
```tf
variable "region" {
  type = map(string)
  default = {
    dev  = "westeurope"
    test = "germanywestcentral"
    prod = "canadacentral"
  }
}

resource "azurerm_resource_group" "rg" {
  for_each = var.region
  name     = "test"
  location = each.value

  tags = local.common_tags
}
```
example2
```tf
resource "azurerm_role_assignment" "assgin_role" {
  count              = length(var.assignments)
  scope              = var.assignments[count.index].scope
  role_definition_id = var.assignments[count.index].role_definition_id
  principal_id       = var.assignments[count.index].principal_id
}

variable "assignments" {
  description = "The list of role assignments to this service principal"
  type = list(object({
    scope              = string
    role_definition_id = string
    principal_id       = string
  }))
  default = []
}

#call
assignments = [
    {
      scope              = "/subscriptions/<sub_id>/resourceGroups/<rg_name>"
      role_definition_id = "/subscriptions/${var.subscription}${data.azurerm_role_definition.ccwlnr.id}"
      principal_id       = azurerm_user_assigned_identity.managedIdentity.principal_id
    },
    {
      scope              = "/subscriptions/<sub_id>/resourceGroups/<rg_name>/providers/Microsoft.Network/virtualNetworks/<vnet_name/subnets/<subnet_name>"
      role_definition_id = "/subscriptions/${var.subscription}${data.azurerm_role_definition.cvnu.id}"
      principal_id       = azurerm_user_assigned_identity.managedIdentity.principal_id
    }
  ]
}
```
example3
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

resource "harbor_project_member_group" "main" {
  project_id    = harbor_project.main.id
  for_each      = var.define_group
  group_name    = each.value.group_name
  role          = each.value.role
  type          = "ldap"
  ldap_group_dn = each.value.ldap_group_dn
}
```
exampl4
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
example5
```tf
variable "assign_group" {
  type = object({
    users  = list(string)
    groups = list(string)
  })

  default = {
    users  = []
    groups = []
  }

  description = "Defines the users and groups that will be admins in the namespace."
}

resource "kubernetes_role_binding_v1" "group_role_binding" {
  
  metadata {
    name        = "${var.namespace}-role-binding"
    namespace   = var.namespace
    labels      = var.labels
    annotations = var.annotations
  }

  role_ref {
    api_group = "rbac.authorization.k8s.io"
    kind      = "Role"
    name      = "${var.namespace}-access-read-only"
  }
  
  # Users
  dynamic "subject" {
    for_each = var.assign_group.users
    content {
      kind      = "User"
      name      = subject.value
      api_group = "rbac.authorization.k8s.io"
    }
  }

  # Groups
  dynamic "subject" {
    for_each = var.assign_group.groups
    content {
        name      = subject.value
        namespace = var.namespace
        kind      = "Group"
        api_group = "rbac.authorization.k8s.io"
    }
  }
}
 
#call
 assign_group = {
    users = []
    groups = [
      "35111183-211a0-1111-b611-11111",
      "4222285-22227-42223-a222-b222dc"
    ]
  }
```
example6
```tf
variable "assign_group" {
  description = "Group ids"
  type        = list(any)
}

resource "kubernetes_role_binding" "group_role_binding" {
  count    = length(var.assign_group)
  metadata {
    name        = "${var.namespace}-role-binding"
    namespace   = var.namespace
    labels      = var.labels
    annotations = var.annotations
  }

  role_ref {
    api_group = "rbac.authorization.k8s.io"
    kind      = "Role"
    name      = "${var.namespace}-access-read-only"
  }

  subject {
    api_group = "rbac.authorization.k8s.io"
    kind      = "Group"
    name      = "${var.assign_group[count.index]}"
    namespace   = var.namespace
  }
}

#call
assign_group     = ["35111183-211a0-1111-b611-11111", "4222285-22227-42223-a222-b222dc"]
```
