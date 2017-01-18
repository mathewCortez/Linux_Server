
## i. The IP address and SSH port so your server can be accessed by the reviewer.

The public IP address is 35.167.25.97 and the SSH port is 2200

## ii. The complete URL to your hosted web application.

None yet

## iii. A summary of software you installed and configuration changes made.

The first thing I did was going from vagrant to signing in as root into AWS. Then I created the grader user using the sudo adduser grader. Then I added grader to the sudo group with the usermod -aG sudo grader command. I then added grader to the sudo group in the /etc/sudoers group. I added my IP to the end of 127.0.0.1 localhost line in the /etc/hosts file so I did not have the "Sudo: unable to resolve host error". I then changed the ssh port in /etc/ssh/sshd_config to listen to port 2200. I spent a day trying to login to grader because I was getting a public key error, so I just had to move the authorized_keys file to the grader .ssh directory to fix that and I ended up signing in from my local computer to AWS just to see if I could do that. After that I was not able to sign back into root, but that is supposed to happen so I did not think anything was wrong there. Next, in grader I ran sudo apt-get update and sudo apt-get upgrade to update everything. I configured the the timezone to be UTC using the sudo dpkg-reconfigure tzdata command and configured the firewall and enabled it.  I used the commads "sudo ufw allow 2200" for the ssh port, "sudo ufw allow http" which defaults to port 80, and the ntp "sudo ufw allow 123/tcp". Then I added this line "config.vm.network :forwarded_port, guest: 80, host: 8080" even though I was not getting to AWS through vagrant, and then I downloaded apache2. The index.html file for apache2 was not loading on localhost:8080 or localhost:80. So I started to change which ports apache was listening to trying to get the file to display. This did not work so I signed in to the AWS through vagrant and changed the ports apache2 was listening to 8080 in the ports.conf file and the 000-default.conf file and I changed the guest port to 8080 in the vagrant file. This did not work either. I then shut off the ufw to see if it was blocking one of the ports but nothing changed. So I downloaded wget to see if I could retrieve the file form localhost:8080 and I was able to do that so I am not sure why it is not displaying on chrome.

## iv. third-party resources you made use of to complete this project.

I downloaded wget to help find the source of some problems
wget -O - http://localhost:8080/index.html - I was able to get the apache2 index.html file with this command so I do not understand why it is not displaying on that URL.

# contents of ~/.ssh/udacity_key.rsa

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAoUidjduAJJeZ5hVpqY1vzjT90HeJZZhi+qL6VQRTebBHIrSa
U2q7D+ZsXGUg9PbDMyQNHalYuu4Bpzx8p209PoVnPcqJLKgbGLv2pcWUuRvjQy9F
YIkkq74wVlQhvAvz2LLNt11Ie8SZXSJNS9Aq6rqqBpdoplxh3sblT8Pil/ntUpcK
YQ8EL2jzb/a3idk4AIniB8caLCSjO3MyS9RtgkBI/uGMxISqVjh39el7ND+NIro/
fwnHBWEvBNLEmm+9mOnRMZiVWwxeXPYeR5A9LIbni9LU85eH7jJDzgbph4mfxH8V
uOUrvnoGjcAGYXVA5uU4k5eyyWorlkP28TfkzwIDAQABAoIBAF/Qmfkqi8gxYDZ2
Rh16bw+cH6V3PmLi0vp/VCSpu0Wx2bDehkhEQflCNOH3GkstRe3d3wTeFF6JAuZt
Ysi8dwPrkNf+uNuUyvQ4xmDZLm+CB9NkA91D3EtDY8gFlzz2BNmZ7+idPHB67XmM
5UrC0pw8ZMIhtQQQIfyNQLHQd48884BFMj3HpZvgEf6xBDfYybzGjzz2d7pzizCM
0kG92TD4djdmDgFQceX33g/ZJlPgPNc+qYjHyK+qmDokirReoHwK/RyzJZZegRVb
8Pqp5zJN0wYOi8SiWHTm+kCxCw6fzXXpAgB7RXJGKVFJFhsNO65YUH0QEAKvYtA3
Y5M6dHECgYEA1AIDKzvMFPtm4swvLiTuKrva7VJlPv3wNJSiWIU6UjsSBLTdz9vA
A+lBQrAxyCZaZN56QwcFlwmZSX2o4wmuqgd6Gmp/2MICpqHUveHj+YocyjUAiVZ7
DuwOhhhiMxjHv4FLuqjgMndW/f1bTGqSlOa5W1IF1MJoPcyNJzu2j/UCgYEAwsAa
Im4W5cOnE+wgaif8SBs302bMCqXJZyscABD0OHQa4AtI/f65+iOFmeAOemiTtjdQ
K1c5PWnrsV5QLs4lGzr9HP+QILqEt7FCw2qIZAJutWKy/DrS7ZrzoQsxGG6bZbBf
Za2opRQW3zj+K4SIjAWCrZpbqs0poiYsdO1k+zMCgYBTBKK3CEhnIvbr8qa6/A0j
QMJ+0hgBbbDk9hsIbMskrirlGuoM3fE31twOQC6OQK5+9zuLCbHfrguPYpyCoyT5
QcpHk9KST454L8C9xjneWn3hlJWsMegoNLmPOvchKR/21quP4VdBi8fN16srpkPV
+O82Wk0cPjBRmsrfRRu9DQKBgFCt4KXO0bGR4k+AjNUth3gfvnrXpUPr9onE9C6a
13Hjt5aFVlHTCxyzRo++oIDZfggI1i9+TPpCPSAXoEQjpn+namBvBzhnzL+EsdHe
+m3kDBUctGWFwQgqHy/iQQ6ME9iGvp6S2MC6l9cV90Xz+9V2GvLsdXlG0S6ZfeVa
y3C9AoGAVMAzWvC65gwNpBRdMlHAPRANm7M7xWfSDJ2dscpfXJhB8fifv6xU+2aJ
FHP0cuqYTR5SvryZFnzVyd4hLfzTe/7kIx1edbynDQ31mmC3yXctbtoGqnFPrq7M
qHUYLS0txnKRNiKe16fvzmZoWZ7McGStxkFH3ex8DgyGoO09AuE=
-----END RSA PRIVATE KEY-----




