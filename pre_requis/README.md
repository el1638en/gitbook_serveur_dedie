# Pré-requis

## Repository Nexus

  - Installer un repository Nexus. Trouver [ici](https://www.elastic.co/fr/products/elasticsearch) un exemple d'installation.

  - Créer le repository nommé  `ansible-galaxy`.

## PIC Jenkins

  1. Installation une PIC Jenkins

   - Installer un serveur Jenkins. Vous trouverez [ici](https://www.elastic.co/fr/products/elasticsearch) une documentation d'installation.

   - En tant que super-user `root`, se connecter au serveur Jenkins et installer les packages `sshpass`, `expect` :

    ```
    root@jenkins-server:~# apt install -y sshpass expect
    ```

  2. Création des variables d'environnement

    Initialiser les 3 variables globales suivantes :
    - Se connecter sur l’interface Web Jenkins ⇒ Administrer Jenkins ⇒ Propriétés globales
    - Cocher la case `Variables d'environnement`
    - Ajouter les variables globales ci-dessous :
      - `NEXUS_REPO_URL` : renseigner l'URL du repository Nexus.
      - `NEXUS_REPO_USERNAME` : le login de l'utilisateur Nexus (`admin` par défaut).
      - `NEXUS_REPO_PASSWORD` : le mot de passe de l'utilisateur Nexus (`admin123` par défaut).

  3. Initialisation du script  `install_sudo_and_user_deploy_environment.sh`

    Dans ce gitbook, vous trouverez le fichier `serveur_dedie/scripts/install_sudo_and_user_deploy_environment.sh`.

    - Sur le serveur Jenkins, créer le répertoire `/var/lib/jenkins/custom_scripts/`

     ```
     root@jenkins-server:~# mkdir /var/lib/jenkins/custom_scripts
     root@jenkins-server:~# vi /var/lib/jenkins/custom_scripts/install_sudo_and_user_deploy_environment.sh
     ```

   - Copiez-y le contenu du script `install_sudo_and_user_deploy_environment.sh`.

   - Attribuer les droit à l'utilisateur `jenkins` sur le répertoire `/var/lib/jenkins/custom_scripts/`

     ```
     root@jenkins-server:~# chown -R jenkins:jenkins /var/lib/jenkins/custom_scripts
     ```

  3. Releases de rôles ANSIBLE

    - Se connecter sur l’interface Web Jenkins
    - Lancer le job « snapshot_or_release_ansible_role »
    - Renseigner les paramètres d’exécution :
        - ROLE_NAME =	clean_services
        - ROLE_VERSION = 1.0.0
        - RELEASE = Oui(cocher la case)
        - LATEST = Oui(cocher la case)
    - Cliquer sur « Build »

    Faire la release pour les rôles suivants :
    - tools,
    - firewall,
    - ssh,
    - fail2ban,
    - portsentry,
    - ntp,
    - admin_tools,
    - docker,
    - monit
