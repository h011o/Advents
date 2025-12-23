# Forensics

## The First Strike

* Provided PCAP can be open using wireshark. 
* We can use `FTP` as a filter and going through different protocols to find multiple failed login attempts.

<img width="1302" height="114" alt="image" src="https://github.com/user-attachments/assets/aa9f638c-dc50-43b6-bd64-ec70882d88ad" />

* Using `frame contains "logged in"` helps us find our required login attempt.

<img width="1144" height="114" alt="image" src="https://github.com/user-attachments/assets/900bdf8e-fc3f-46fd-83ee-8493c42cd743" />


Flag : `csd{Elf67_snowball}`
