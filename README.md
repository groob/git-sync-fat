# git-sync 

Sync a git repo to a specific revision/branch.
Update the repo at an interval.

# Docker

https://hub.docker.com/r/groob/sync-fat/

# Example:

```
sync:
    build: git-sync/
    restart: always
    volumes_from:
        - munki-data
    environment:
        - GIT_SYNC_REPO=ssh://git@github.com/groob/munki-repo.git
        - GIT_SYNC_DEST=/data
        - GIT_SYNC_FAT=true
        - GIT_SYNC_BRANCH=master
        - GIT_SYNC_REV=origin/master
        - GIT_SYNC_WAIT=60
```

# Example with a private SSH key:

```
FROM groob/sync-fat:latest

COPY repo-key /

RUN \
  chmod 600 /repo-key && \
  echo "IdentityFile /repo-key" >> /etc/ssh/ssh_config && \
  echo -e "StrictHostKeyChecking no" >> /etc/ssh/ssh_config

CMD ["/git-sync"]
```
