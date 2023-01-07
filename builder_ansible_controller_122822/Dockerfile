#
# This script generates an ansible controller
# It tunes the config file for ansible 
# it copies a tuned hosts file that will push playbooks to devices named "ansiblerecipient.local"
# it generates an ssh key that can be copied to the recipient devices so that ansible playbooks can run.

FROM        ubuntu
COPY ./ansible.cfg /etc/ansible/
COPY ./hosts /etc/ansible/
#ADD /Users/Patrick/Development/Playbooks/DockerVolume /home/pi/Playbooks

RUN     apt-get update && apt-get install -y ansible vim iputils-ping rsync
# generate a new SSH key
RUN ssh-keygen -t rsa -b 4096 -N "" -f /root/.ssh/id_rsa
# copy the public key to the authorized_keys file
RUN cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
# set the correct permissions on the authorized_keys file
RUN chmod 600 /root/.ssh/authorized_keys
