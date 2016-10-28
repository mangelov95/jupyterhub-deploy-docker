# Build as jupyterhub/systemuser
# Run with the DockerSpawner in JupyterHub

FROM jupyterhub/singleuser

MAINTAINER Project Jupyter <jupyter@googlegroups.com>

USER root
RUN userdel jovyan
ENV SHELL /bin/bash
ADD systemuser.sh /srv/singleuser/systemuser.sh
# smoke test entrypoint
RUN USER_ID=65000 USER=systemusertest sh /srv/singleuser/systemuser.sh -h && userdel systemusertest

# Insert additional Python libraries here
RUN pip install lightning-python
RUN pip2 install lightning-python #Added to satisfy Python2 notebooks
# End of additional Python libraries

RUN apt-get update && apt-get install -y libpam-ldapd \
                                         libnss-ldapd
COPY ./files/ldap.conf /etc/ldap.conf
COPY ./files/nsswitch.conf /etc/nsswitch.conf
COPY ./files/profile.d/xauthority.sh /etc/profile.d/xauthority.sh
COPY ./files/profile.d/OMPI_openib_if_include.sh /etc/profile.d/OMPI_openib_if_include.sh
COPY ./files/ldap.conf /etc/ldap.conf
COPY ./files/nslcd.conf /etc/nslcd.conf

RUN sed -i 's/.named//g' /etc/nslcd.conf

COPY ./files/security/access.conf /etc/security/access.conf
RUN sed -i -e 's/- : ALL/+ : ALL/g' /etc/security/access.conf


CMD ["sh", "/srv/singleuser/systemuser.sh"]
