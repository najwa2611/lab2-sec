vous trouverez les screenshots du process de l installation ainsi que toutes les commandes de rooting depuis le cmd dans ce repo git 

 LABO ROOTING ANDROID - ÉMULATEUR

## 📋 INFORMATIONS GÉNÉRALES

| Élément | Valeur |
|---------|--------|
| **Support** | AVD (Android Virtual Device) |
| **Objectif** | Comprendre rooting et impacts sur la sécurité |
| **Données** | Fictives uniquement |
| **Réseau** | Test (isolé) |

---

## 🖥️ ENVIRONNEMENT DE TEST

| Composant | Version / Spécification |
|-----------|------------------------|
| **AVD** | Pixel 6 Pro |
| **Android** | 12.0 (API 31) |
| **Image système** | Google APIs x86_64 |
| **Android Studio** | Version utilisée |
| **OS Hôte** | Windows |

---

## 🚀 COMMANDES EXÉCUTÉES

### 1. Vérification initiale

bash
adb devices

#### Résultat : emulator-5554   device
### 2. Démarrage de l'émulateur (mode écriture)

bash
emulator -avd "Pixel_6_Pro" -writable-system

### 3. Activation du root

bash
adb root

#### Résultat : restarting adbd as root

adb remount
#### Résultat : remount succeeded

adb shell id
#### Résultat : uid=0(root) gid=0(root) 

### 4. Vérifications système

bash
adb shell getprop ro.boot.verifiedbootstate

#### Résultat attendu : orange/yellow

adb shell getprop ro.boot.veritymode

#### Résultat : disable ou logging

adb shell ls -la /data/data/

#### Résultat : Liste des dossiers d'applications

### 5. Journalisation

bash
adb logcat -d > logcat_root_complet.txt
adb shell getprop > proprietes_systeme.txt
adb shell ls -la /data/data/ > apps_installees.txt
adb shell id > preuve_root.txt

#### RÉSULTATS OBTENUS
Vérification	Statut	Résultat
Root activé	 RÉUSSI	uid=0(root)
Accès /data/data/	 RÉUSSI	Lecture des dossiers apps
adb remount	 RÉUSSI	Système en lecture/écriture

## SCÉNARIOS DE TEST
Scénario	Description	Résultat
1	Vérification accès données protégées	 Accès réussi
2	Lecture logs système	 Logs accessibles
3	Analyse propriétés système	 Propriétés lues

## MATRICE DES RISQUES
Risque	Description
Intégrité non garantie	Conclusions biaisées sur la sécurité réelle
Surface d'attaque accrue	Exposition à des menaces externes
Données sensibles exposées	Violation potentielle de confidentialité
Instabilité système	Tests non reproductibles
Mélange comptes perso/test	Fuite d'informations personnelles
Mauvais nettoyage	Persistance de données sensibles
Réseau non isolé	Effets sur systèmes externes
Traçabilité insuffisante	Impossible d'auditer les tests

## MESURES DÉFENSIVES APPLIQUÉES
Mesure	Application
Réseau isolé	 PC en local uniquement
Données fictives	 Aucune donnée réelle
AVD dédié	 Uniquement pour ce labo
Wipe fin séance	 À effectuer
Journal détaillé	 Fichiers logs sauvegardés
Aucun compte perso	 Vérifié
APK contrôlées	 Apps système uniquement
Horodatage + captures	 Documentation complète

## RÉFÉRENCES OWASP
MASVS - Exigences vérifiées
Exigence	Description
STORAGE-1	Les données sensibles doivent être stockées de manière sécurisée avec chiffrement
NETWORK-1	Les communications réseau doivent utiliser TLS avec vérification des certificats
MASTG - Tests applicables
Test	Description
Vérifier stockage	Examiner les fichiers dans /data/data/[package]/shared_prefs/
Analyser logs	Détecter les fuites d'informations sensibles avec adb logcat

## NETTOYAGE FIN DU LAB

bash

# Fermer l'émulateur
adb emu kill

OU via Android Studio
Device Manager → Wipe Data

## FICHIERS GÉNÉRÉS
Fichier	Description
logcat_root_complet.txt	Logs système complet
proprietes_systeme.txt	Propriétés Android
apps_installees.txt	Liste des applications
preuve_root.txt	Preuve uid=0(root)
verity_state.txt	État de Verified Boot
