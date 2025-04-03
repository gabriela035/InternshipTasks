# THE STEPS TAKEN TO SOLVE THE EXERCISE

## OPEN A DOCKER UBUNTU CONTAINER
Open a terminal and run `docker run -it ubuntu.` Each container has an image, so if Ubuntu Container's image does not exist, the command will also create an image for it:
![image](https://github.com/user-attachments/assets/ac793817-edbd-4d36-9fb0-50b2fb02b1c0)

It should also appear at Docker Desktop at Containers and Images
![image](https://github.com/user-attachments/assets/2c15a11e-5d4d-4848-8b9b-8156a3fdf0c2)
![image](https://github.com/user-attachments/assets/9820e0f5-fd75-4bd4-bc4e-37aa5bd30104)

## PUBLIC IP OF cloudflare.com
Perform a DNS (Domain Name System) lookup on digwebinterface.com  using the command `dig A cloudflare.com` where:
- `dig` = Domain Information Groper, retrieve various DNS records
- `A` = IPv4 address
 (got useful information from https://community.cloudflare.com/t/cloudflare-ip-address/564125/4)
In my case, it returned *104.16.132.229*

## Map IP address 8.8.8.8 to hostname google-dns
On Windows, open Notepad as an administrator, navigate to `C:\Windows\System32\drivers\etc\hosts` , add `8.8.8.8 google-dns` to hosts file and then save file. To verify the mapping, run `ping google-dns` and it should be responses from 8.8.8.8
![image](https://github.com/user-attachments/assets/ec6a6a47-7dec-4a75-877e-983c3d09cd84)
(got useful information from https://stackoverflow.com/questions/35095979/how-to-map-a-local-ip-to-a-hostname and https://www.youtube.com/watch?v=aLy9oDHkJpM&ab_channel=PoweruserGuideTV)

## Check if the DNS Port is Open for google-dns
After a Google search, we know 53 is the DNS port. Run `nslookup google.com 8.8.8.8`, wich is a command that queries a DNS server (Google's 8.8.8.8) to resolve a domain name.
If the DNS server is accessible and port 53 is open, it will receive a valid response with an IP address. If port 53 is blocked or DNS is unavailable, it will get an error, as in my case
![image](https://github.com/user-attachments/assets/298322c3-161d-4921-83e9-23119e8c0ee9)

that means DNS is not accessible. (thank you ChatGPT)

## Modify the System to Use Google’s Public DNS
### Change the nameserver to 8.8.8.8 instead of the default local configuration
### Perform another public IP lookup for cloudflare.com and compare the results
