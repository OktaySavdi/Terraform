
![image](https://user-images.githubusercontent.com/3519706/165931435-861a4c95-19a0-4f74-a5b6-3c55e24c4f26.png)

**Command**

![image](https://user-images.githubusercontent.com/3519706/165931545-d03dc7f3-fe93-4572-a31c-cb692e11c934.png)

**Result**

![image](https://user-images.githubusercontent.com/3519706/165931657-7696ce37-4b8c-4ba6-8fbc-5aab17d1a988.png)

**File Type**

![image](https://user-images.githubusercontent.com/3519706/165932393-09d9ae81-4c6d-4991-ac48-738a5fb1342a.png)

**Variable**

main.tf
```yaml
resource "local_file" "pet" { 
  filename = var.filename
  content = var.content 
} 
resource "random_pet" "my-pet" { 
  prefix = var.prefix 
  separator = var.separator
  length = var.length 
}     
```
variables.tf
```yaml
variable "filename" { 
  default = "/root/pets.txt"
  type = string
} 
variable "content" { 
  default = "We love pets!" 
  type = string
} 
variable "prefix" { 
  default = "Mrs"
  type = string
} 
variable "separator" { 
  default = "." 
} 
variable "length" { 
  default = "1" 
  type = number
}
```
![image](https://user-images.githubusercontent.com/3519706/165933751-ec3883a6-ac25-41ed-a4b1-d43ae366a004.png)

**Variable Type**
![image](https://user-images.githubusercontent.com/3519706/165934444-d51f439b-44cb-471a-b53e-730d340ed024.png)

![image](https://user-images.githubusercontent.com/3519706/165934629-132529d5-aab6-4aa3-8e3b-67c6ebc36224.png)

![image](https://user-images.githubusercontent.com/3519706/165934738-0e647e07-014f-45de-af1a-773e430d7817.png)

![image](https://user-images.githubusercontent.com/3519706/165934844-5b88ab0e-79d6-4b91-80e3-240af1b218bc.png)

![image](https://user-images.githubusercontent.com/3519706/165934937-e7303411-76e1-4495-8559-08fc01fa98b1.png)

![image](https://user-images.githubusercontent.com/3519706/165935052-ff80079d-98c3-4439-bac7-b7043610e2dd.png)

![image](https://user-images.githubusercontent.com/3519706/165935261-7689f277-a47e-400b-9f51-df48676ac35c.png)

![image](https://user-images.githubusercontent.com/3519706/165935335-73e63b6d-740b-433c-9537-d47d40d0e29e.png)






