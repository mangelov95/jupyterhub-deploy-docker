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

CMD ["sh", "/srv/singleuser/systemuser.sh"]
