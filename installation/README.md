# Installation du serveur

## Génération et export de clés SSH

  Toutes les connexions sur le serveur dédié se feront via SSH.
  Pour cela, sur votre machine locale, générer votre paire de clés SSH publique et privée.

  ```
  legeric@st-local-machine:~$ ssh-keygen -t rsa -b 4096
  legeric@st-local-machine:~$ more ~/.ssh/id_rsa.pub
  ```

  Pour exporter votre clé publique sur le serveur dédié :
  - Se connecter sur l’interface Web Jenkins
  - Lancer le job « export_ssh_public_key »
  - Renseigner les paramètres d’exécution :
      - ADDRESS_IP =	adresse IP ou DNS du serveur dédié
      - USERNAME = Le nom de l'utilisateur présent sur le serveur dédié
      - PASSWORD = Le mot de passe de l'utilisateur présent sur le serveur dédié
      - SSH_PUBLIC_KEY = copiez-y votre clé publique SSH
  - Cliquer sur « Build »

## Préparation du serveur dédié (action pré-installation)

  1. Installation du programme `sudo` et ajout de l'utilisateur `deploy`

    - Se connecter sur l’interface Web Jenkins
    - Lancer le job « install_sudo_and_deploy_user »
    - Renseigner les paramètres d’exécution :
        - ADDRESS_IP =	adresse IP ou DNS du serveur dédié
        - USERNAME = Le nom de l'utilisateur présent sur le serveur dédié
        - PASSWORD = Le mot de passe de l'utilisateur présent sur le serveur dédié
        - ROOT_PASSWORD = Le mot de passe du super-utilisateur `root`
    - Cliquer sur « Build »

    Ce job installe le programme `sudo` et ajoute l'utilisateur `deploy` sur le serveur dédié.
    Cet utilisateur sera utilisé pour les déploiements avec Ansible.

  2. Copie de la clé SSH de l'utilisateur jenkins sur le serveur dédié

    - Lancer le job « export_ssh_public_key »
    - Renseigner les paramètres d’exécution :
        - ADDRESS_IP =	adresse IP ou DNS du serveur dédié
        - USERNAME = deploy
        - PASSWORD = deploy
        - SSH_PUBLIC_KEY = copiez-y la clé publique SSH de l'utilisateur jenkins
    - Cliquer sur « Build »

## Installation du serveur dédié

  - Se connecter sur l’interface Web Jenkins
  - Lancer le job « install_server »
  - Renseigner les paramètres d’exécution :
      - commentaire = noter un commentaire explicite
      - ADDRESS_IP =	adresse IP ou DNS du serveur dédié
      - MACHINE_NAME = Nom du serveur dédié
      - debug = Oui (cocher la case)
      - installComplete = Oui (cocher la case)
  - Cliquer sur « Build »

  Vérifier que le job s'est déroulé jusqu'au bout sans échec.
