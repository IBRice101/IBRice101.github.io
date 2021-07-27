# WebGoat Notes

[OWASP's WebGoat](https://owasp.org/www-project-webgoat/) application is a deliberately insecure web app designed specifically for users to attack, defend, and learn the intricacies of web application security. OWASP, the Open Web Application Security Project, created this application in order to provide a safe and legal environment in which to test developers' security knowledge and tools.

If you'd like to run WebGoat yourself, you need to ensure your device, or the container that it's in, is not accessible to the wider internet, as the platform makes your network extremely vulnerable, WebGoat binds to localhost by default to minimise exposure but better safe than sorry.

## Setup

I will be running WebGoat inside a docker container on my Raspberry Pi 4b, this is my preferred route for a multitude of reasons, primarily though my intention with the pi is to convert it into a containerised homelab setup, over time I intend to get as many Pis as are required for certain services, cluster them, and use them to run a web server, network storage, etc. As such, learning docker is essential for my (eventual) homelab config.

As WebGoat is not configured to run natively on ARM based CPUs, however, I will need to pull down an alternative docker image, which can be found at [cambarts/webgoat-8.0-rpi](https://hub.docker.com/layers/cambarts/webgoat-8.0-rpi/latest/images/sha256-e7bb9e5df7009ad6176409f5058d1466206f0323331837378a1dea4acad5be95?context=explore).

Presuming docker is not available on your system, the full list of commands required to go from nothing to running WebGoat is as follows:

```sh
sudo apt-get update && sudo apt-get upgrade # update the system

curl -fsSL https://get.docker.com -o get-docker.sh # fetch the install script
sudo sh get-docker.sh # run the install script

sudo usermod -aG docker pi # add the pi user (default) to the docker group

docker run hello-world # test docker's working

sudo docker pull cambarts/webgoat-8.0-rpi # fetch the docker image
sudo docker run -p 8080:8080 -t cambarts/webgoat-8.0-rpi # run the docker image
```

after this process is complete (if without error), navigate to http://[IP ADDRESS OF DEVICE]:8080/WebGoat, and sign up for an account

## Login

- Very weak credential policy
  - Username:
    - between 6-10 characters
    - can only contain lowercase letters, digits, and the '-' character
  - Password:
    - Only between 6 and 10 characters

## General

