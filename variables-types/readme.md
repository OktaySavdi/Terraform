
**Variable Types**

**1. String Variables:**
- `string`: A simple text string.
```yaml
variable "region" {
  description = "Azure region"
  type        = string
  default     = "East US"
}
```
**2. Number Variables:**
- `number`: A numeric value.
```yaml
variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 3
}
```
**3. Boolean Variables:**
- `bool`: A boolean (true/false) value.
```yaml
variable "enable_logging" {
  description = "Enable logging"
  type        = bool
  default     = true
}
```
**4. List Variables:**
- `list(type)`: An ordered list of elements of the specified type.
```yaml
variable "subnets" {
  description = "List of subnets"
  type        = list(string)
  default     = ["subnet-1", "subnet-2"]
}
```
**5. Map Variables:**
- `map(type)`: A map of key-value pairs where keys and values have specified types.
```yaml
variable "tags" {
  description = "Key-value tags"
  type        = map(string)
  default     = {
    Environment = "dev"
    Owner       = "John Doe"
  }
}
```
**6. Object Variables:**
- `object({ attribute1 = type1, attribute2 = type2, ... })`: An object with defined attributes and their types.
```yaml
variable "subnet" {
  description = "Subnet configuration"
  type = object({
    name           = string
    address_prefix = string
  })
  default = {
    name           = "subnet-1"
    address_prefix = "10.0.1.0/24"
  }
}
```
**7. Tuple Variables:**
- `tuple([type1, type2, ...])`: An ordered tuple of elements with specified types.
```yaml
variable "subnet_cidr_blocks" {
  description = "List of subnet CIDR blocks"
  type        = tuple([string, string])
  default     = ["10.0.1.0/24", "10.0.2.0/24"]
}
```
**8. Set Variables (Experimental):**
- `set(type)`: A collection of unique elements of the specified type.
- Note: Sets are experimental and may not be available in all Terraform versions.
```yaml
variable "unique_ids" {
  description = "Set of unique IDs"
  type        = set(string)
  default     = ["id-1", "id-2"]
}
```
**9. Any Type Variables:**
- `any`: A variable that can accept any type of value.
```yaml
variable "dynamic_value" {
  description = "A variable with any type"
  type        = any
  default     = "example"
}
```
