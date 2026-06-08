Docker Permission Issue

Problem:

Docker commands required sudo access.

Resolution:

Added ec2-user to the Docker group:

 # sudo usermod -aG docker ec2-user

Refreshed the SSH session and verified access.