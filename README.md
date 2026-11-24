# 🫀 Simulateur d'alerte — Staying Alive

Simulateur **pédagogique** d'alerte d'arrêt cardiaque pour les formations *Citoyen Sauveteur*. Il reproduit sur smartphone le déroulé d'une alerte reçue via une application de type **Staying Alive** (Bon Samaritain) ou **SAUV Life** (SAMU / Urgences), afin d'entraîner les apprenants dans des conditions proches du réel.

> ⚠️ **Simulation uniquement.** Aucune alerte réelle n'est envoyée, aucune donnée n'est transmise, aucun service de secours n'est contacté. Ce projet n'est affilié à aucune des applications citées.

---

## Fonctionnalités

- **Choix de l'application simulée** : Staying Alive (thème vert) ou SAUV Life (thème rouge).
- **Scénario personnalisable** par le formateur :
  - lieu précis de la victime ;
  - distance estimée ;
  - emplacement du défibrillateur (DAE) ;
  - délai avant déclenchement : 45 s, 60 s ou 75 s.
- **Écran de verrouillage** factice (heure et date en direct) qui masque le compte à rebours, pour créer un effet de surprise.
- **Alerte « Arrêt cardiaque »** plein écran avec signal sonore, boutons *J'accepte la mission* / *Indisponible / Refuser*.
- **Écran de mission** : chronomètre, radar simulé, lieu à rejoindre, DAE le plus proche, bouton *Je suis sur place*.
- **Gestes d'urgence** : checklist interactive (sécuriser la zone, vérifier conscience et respiration, alerter le 15/112 et demander un DAE, débuter la RCP) avec chronomètre.
- **Métronome de RCP à 110 battements/min**, sonore et visuel, pour caler le rythme des compressions.
- Interface **mobile first**, utilisable aussi sur tablette et ordinateur.

## Déroulé d'une simulation

```
Configuration ──► Écran verrouillé ──► Alerte ──► Mission ──► Gestes d'urgence ──► Retour formateur
   (formateur)      (compte à rebours)   │          (radar, DAE)   (checklist + métronome)
                                         └─► Refus : retour à la configuration
```

1. Le formateur règle le scénario et appuie sur **ARMER LA SIMULATION**.
2. Le téléphone passe en plein écran, empêche la mise en veille (si le navigateur le permet) et affiche un faux écran de verrouillage.
3. À la fin du délai, l'alerte se déclenche avec un bip. Le formateur peut aussi la déclencher immédiatement **en touchant l'écran de verrouillage**.
4. L'apprenant accepte ou refuse la mission, rejoint la victime, puis suit la checklist et le métronome.
5. **Terminer l'exercice** ramène à l'écran de configuration et libère le plein écran et la veille d'écran.

## Utilisation

Le projet tient dans **un seul fichier HTML**, sans dépendance, sans build et sans connexion réseau.

1. Télécharger `simulateur_staying_alive.html`.
2. L'ouvrir dans un navigateur récent (Chrome, Safari, Firefox, Edge), sur ordinateur ou smartphone.
3. Pour l'utiliser sur un téléphone : transférer le fichier (mail, AirDrop, clé USB…) ou l'héberger sur n'importe quel serveur statique.

Pour un test local avec serveur :

```bash
python3 -m http.server 8000
# puis ouvrir http://localhost:8000/simulateur_staying_alive.html
```

### Conseils pour la formation

- **Activer le volume** du téléphone avant d'armer : les bips et le métronome utilisent la Web Audio API.
- Armer la simulation **depuis un geste utilisateur** (le bouton prévu) : les navigateurs l'exigent pour autoriser le son, le plein écran et le maintien de l'écran allumé.
- Penser à désactiver le mode silencieux sur iPhone.

## Compatibilité

| Fonction | API utilisée | Comportement si non supportée |
|---|---|---|
| Son (alerte, métronome) | Web Audio API | Simulation silencieuse |
| Écran toujours allumé | Screen Wake Lock API | Le téléphone peut se mettre en veille |
| Plein écran | Fullscreen API | Affichage normal dans le navigateur |

Toutes ces fonctions sont facultatives : en cas d'échec, la simulation continue de fonctionner.

## Structure du projet

```
.
├── simulateur_staying_alive.html   # Application complète (HTML + CSS + JavaScript)
└── README.md
```

Dans le fichier HTML :

| Élément | Rôle |
|---|---|
| `<section id="setup">` | Configuration du scénario |
| `<section id="lock">` | Faux écran de verrouillage |
| `<section id="alert">` | Alerte d'arrêt cardiaque |
| `<section id="missionScreen">` | Mission en cours (radar, lieu, DAE) |
| `<section id="emergencyScreen">` | Checklist des gestes d'urgence et métronome |
| `<script>` | Logique : navigation entre écrans, compte à rebours, chronomètres, sons |

La navigation repose sur une fonction `show(id)` qui affiche un seul écran à la fois.

## Personnalisation

- **Valeurs par défaut du scénario** : attribut `value` des champs `#place`, `#distance` et `#dae`.
- **Délais proposés** : boutons `data-delay` dans la section `#setup` (valeur en secondes).
- **Rythme du métronome** : `60000/110` dans le gestionnaire de `#metroButton` (et le libellé « 110 BPM » dans le HTML).
- **Couleurs** : variables CSS déclarées dans `:root` (`--red`, `--green`, `--bg`…).
- **Étapes de la checklist** : balises `<label class="checkrow">` de la section `#emergencyScreen`.

## Limites connues

- Le simulateur ne remplace ni une formation certifiante ni les consignes des secours : en situation réelle, **appelez le 15 ou le 112**.
- Le radar est une illustration statique : aucune géolocalisation n'est utilisée.
- Aucune donnée n'est enregistrée : l'état est réinitialisé au rechargement de la page.

## Licence

À définir par l'auteur du projet.
