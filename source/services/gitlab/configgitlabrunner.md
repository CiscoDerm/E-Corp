# Configuration du gitlab runner

aller dans http://gitlab.ecorp.ad/admin/runners

choissir tag, description , et create runner

créer docker-compose.yml 

```sh
version: '3.9'
services:
  gitlab-runner:
    image: gitlab/gitlab-runner:latest
    container_name: gitlab-runner
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - gitlab_runner_config:/etc/gitlab-runner
      #- ./config.toml:/etc/gitlab-runner/config.toml
    restart: always
    networks:
      - gitlab_network
volumes:
  gitlab_runner_config:

networks:
  gitlab_network:
    external: true
```

docker-compose up -d

connect gitlab-runner

gitlab-runner register  --url http://gitlab.ecorp.ad  --token glrt-t1_JbTcazHVZ6osgwS6LDtC

un dialogue interactive s'ouvre : 
entrez, name , ruby;2.7 ( le nom qui est affiché) normalement, sut gitlab, c'est registerd !
et voila, new runner fait ! 