# MOTIVATION 
For testing infrastructure automation or provisioning with something like ansible, which requires ssh access to the target machine, you'd want to test this in a safe environment before going live.

# PROCESS

To build the image run docker build -t IMAGE_NAME . , 
If it is done then can run the image using docker run IMAGE_NAME -p 22:22. 

Finally can connect to the container using the user you created , in this case it will be test so ssh test@ip_address enter the password in the prompt and  all setup


