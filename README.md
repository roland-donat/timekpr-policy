# timekpr-policy

Politique de temps d'écran appliquée par [timekpr](https://mjasnik.gitlab.io/timekpr-next/)
sur les machines de la maison, via l'agent `timekpr-agent`.

Ce dépôt ne contient qu'un fichier de **données** : `policy.yaml`. Il fait
autorité. Chaque machine le télécharge au démarrage, puis toutes les heures, le
valide, et réapplique les limites correspondantes.

## Fonctionnement

```
édition locale  ──►  git push  ──►  raw.githubusercontent.com
                                            │
                        (au boot, puis toutes les heures, et au retour de veille)
                                            ▼
                            machine ── validation ── cache local ── timekpra
```

Une machine hors ligne, ou qui reçoit un fichier invalide, **conserve et
applique la dernière politique valide connue**. Couper le réseau ne lève donc
aucune limite.

Comptez jusqu'à ~5 minutes entre un `push` et sa visibilité : c'est le cache CDN
de `raw.githubusercontent.com`.

## Ce que ce dépôt ne contient pas

Aucun identifiant. Les secrets — identifiants Pronote, et l'URL de
l'établissement qui contient son code UAI — vivent dans un fichier `secrets.env`
qui ne quitte jamais les machines. `policy.yaml` ne référence que les **noms**
des variables d'environnement correspondantes (`url_env`, `login_env`,
`password_env`).

## Format

Une clé par enfant, plus une section `main`. Voir les commentaires dans
`policy.yaml`. Les valeurs de temps sont en **minutes**, du lundi au dimanche.

L'agent refuse un fichier qui ne passe pas la validation structurelle : YAML
valide, `machines` non vide, listes de 7 entiers positifs, et au moins un enfant
correspondant à un utilisateur existant sur la machine.

## Outillage

Le code de l'agent et du déploiement est séparé et n'est pas publié ici.
