# Configurer des pipelines pour GITLAB


## créer un gitlab-ci.yml

*pour vérifier la syntaxe python :*
```sh
stages:
  - test

test-python:
  stage: test
  script:
    - set -e
    - apk add --no-cache python3 py3-pip
    - python3 -m venv /venv  
    - . /venv/bin/activate  
    - pip install --upgrade pip  
    - pip install flake8
    - flake8 .
  tags:
    - runner1
  variables:
    GITLAB_CI_SERVER: "http://192.168.16.3"
    GIT_STRATEGY: "clone"
    GIT_DEPTH: "1"
```


*pour vérifier mkdocs et l'exposer :*
```sh
stages:
  - verify
  - deploy

variables:
  MKDOCS_DIR: "/builds/dev/emp/documentation"  # Chemin du projet dans le runner
  SITE_DIR: "$MKDOCS_DIR/site"                # Dossier de sortie du build
  HOST_SITE_DIR: "/home/ecorp/ecorp/mkdocs/site"  # Chemin sur l'hôte

verify_mkdocs:
  stage: verify
  image: python:3.11
  script:
    - echo "Installation des dépendances..."
    - pip install mkdocs mkdocs-material
    
    - echo "Construction de la documentation..."
    - mkdocs build --strict
    
    - echo "Vérification des fichiers générés..."
    - ls -la $SITE_DIR
  artifacts:
    paths:
      - $SITE_DIR/
    expire_in: 1 week
  tags:
    - runner1

deploy:
  stage: deploy
  image: docker:latest
  services:
    - docker:dind
  script:
    - echo "Synchronisation des fichiers..."
    - docker cp site/. mkdocs:/usr/share/nginx/html/
    - echo "Rechargement de Nginx..."
    - docker exec mkdocs nginx -s reload
  tags:
    - runner1
  only:
    - main

```


