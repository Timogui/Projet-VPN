https://github.com/Timogui/Projet-VPN

Projet:
VPN 

Personnes:
- Jinkyu Lee
- Timothé Guinard
- Saad EL MISSAOUI
- Mathis

## 1. Comment se connecter

### A. Par un terminal
- Télécharger vpnjinq.pem sur son pc
- Ouvrir un terminal
- Se déplacer jusqu'au fichier avec vpnjinq.pem dedans
- entrer la commande ```ssh -i "vpnjinq.pem" ubuntu@51.20.255.255```


### B. Par WireGuard
- Télécharger WireGuard
- Télécharger timotimo.conf
- Ouvrir WireGuard et ajouter un tunnel
- Selectionner timotimo.conf
- Activer le tunnel

### Note :
Passer par WireGuard coupera temporairement votre connection
## 2. Architecture

### Définition du réseau et des hôtes
Un serveur VPN a été déployé sur un serveur AWS (environnement Ubuntu) en utilisant PiVPN et WireGuard.

### Répartition des services
- **Serveur VPN :** Gestion des connexions VPN des clients via WireGuard
- **Tableau de bord Web :** Utilisation de [WGDashboard](https://github.com/donaldzou/WGDashboard) pour gérer les paramètres VPN via une interface web
- **Authentification sécurisée :** Ajout de l'authentification en deux étapes (Google Auth) pour sécuriser l'accès à WGDashboard

### Mise en œuvre des bonnes pratiques
- **Sécurisation du serveur VPN :** Configuration du pare-feu et application du principe des privilèges minimaux
- **Application d'un système d'authentification :** Intégration de Google Auth sur WGDashboard pour renforcer la sécurité
- **Automatisation et surveillance :** Utilisation de WGDashboard pour faciliter la gestion des utilisateurs et des configurations VPN

### Configuration de la solution (système, réseau, services)
1. Création d'une instance de serveur Ubuntu sur AWS
2. Installation et configuration de WireGuard avec PiVPN
3. Installation et configuration de WGDashboard
4. Mise en place de l'authentification en deux étapes avec Google Auth

---

## 3. Utilisation  du serveur

### Utilisation de WGDashboard
- **Ajout et gestion des utilisateurs VPN**
  - Connexion à WGDashboard et ajout d'un utilisateur
  - Modification et application des paramètres WireGuard

- **Modification des paramètres WireGuard**
  - Modification des configurations client depuis WGDashboard
  - Application des changements via l'interface WireGuard

### Connexion au VPN
1. Installer le client WireGuard (Windows, macOS, Linux, Android, iOS pris en charge)
2. Télécharger le fichier de configuration depuis WGDashboard
3. Importer le fichier dans le client WireGuard et activer la connexion VPN


### Processus d'authentification Google Auth
1. Connexion à WGDashboard nécessitant une authentification via Google Auth
2. Scanner le code QR avec l'application Google Authenticator pour configurer la 2FA
3. Entrer le code OTP généré pour finaliser la connexion

### Commandes principales de PiVPN
- `pivpn add` : Ajouter un nouvel utilisateur VPN
- `pivpn list` : Lister les utilisateurs VPN enregistrés
- `pivpn revoke nom_utilisateur` : Révoquer l'accès VPN d'un utilisateur spécifique
- `pivpn remove nom_utilisateur` : Supprimer un utilisateur spécifique
- `pivpn debug` : Vérifier et résoudre les problèmes VPN
- `pivpn update` : Mettre à jour PiVPN

### Accès à WGDashboard
1. Ouvrir un navigateur web et entrer `http://<IP_du_serveur>:10086` (port par défaut : 10086)
2. Saisir les identifiants administrateur sur l'écran de connexion
3. Entrer le code OTP généré par l'application Google Authenticator
4. Accéder à la gestion des utilisateurs et des paramètres VPN

Cette documentation permet l'exploitation et la gestion du service VPN.