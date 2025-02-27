# COnfiguration gitlab

mkdir gitlab 
cd gitlab 
vim docker-compose.yml
```yml
version: '3.9'
services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    container_name: gitlab
    dns:
      - 192.168.4.3
    volumes:
      - gitlab_config:/etc/gitlab
      - gitlab_logs:/var/log/gitlab
      - gitlab_data:/var/opt/gitlab
    environment:
      GITLAB_OMNIBUS_CONFIG:  |
        external_url 'http://gitlab.ecorp.ad'
        gitlab_rails['ldap_enabled'] = true
        gitlab_rails['ldap_servers'] = YAML.load <<-EOS
        main:
          label: 'ECorp AD'
          host: 'ecorpad.ecorp.ad'
          port: 389
          uid: 'sAMAccountName'
          bind_dn: 'CN=ecorp,CN=Users,DC=ecorp,DC=ad'
          password: 'USER_PASSWORD'
          encryption: 'plain'
          active_directory: true
          lowercase_usernames: false
          allow_username_or_email_login: false
          base: 'CN=Users,DC=ecorp,DC=ad'
          user_filter: ''
          group_base: 'CN=Groups,DC=ecorp,DC=ad'
          admin_group: 'GitLab Admins'
          sync_ssh_keys: false
        EOS
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.gitlab.rule=Host(`gitlab.ecorp.ad`)"
      - "traefik.http.services.gitlab.loadbalancer.server.port=80"
      - "traefik.http.routers.gitlab.entrypoints=http"

    networks:
      - traefik_proxy
      - gitlab_network
    restart: always

volumes:
  gitlab_config:
  gitlab_logs:
  gitlab_data:

networks:
  traefik_proxy:
    external: true
  gitlab_network:
    external: true
```

pas oublier de remplacer USER_PASSWORD

docker-compose up -d