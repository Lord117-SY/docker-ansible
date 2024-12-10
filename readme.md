# Ansible Docker

Ce projet va contenir le code des images que nous utiliserons via les pipelines de déploiement gitlab pour éxecuter des playbook ansible.

Le dossier ansible 2.9 doit contenir un code capable d'exécuter ansible sur les vieilles versions de suse linux (11)
Le dossier ansible 2.10  doit contenir un code capable d'exécuter ansible sur les  versions plus récentes de suse linux (12+)

## ENV applicatif

- ubuntu 24.04 sera l'image de base
- miniconda3 est utilisé pour nous permettre de choisir la version de python souhaitée

## Configuration

les dossiers files vont contenir: 
- la configuration sudo pour l'utilisateur ansible (nécessaire dans le cas où l'ont fait réaliser des tâches par localhost)
- la configuration par défaut d'ansible à date on se contente de supprimer les warnings provoquées par une vieille version de python
- le fichier bashrc qui va contenir  la configuration miniconda nous permettant de passer d'un environnement à un autre
- le fichier entrypoint qui va lancer un daemon ssh pour permettre à ansible de se connecter sur localhost ainsi qu'un bash.

## Utilisation

Pour instancier le conteneur depuis votre poste:
```bash
docker run -it -v chemin_de_votre_point_de_montage:chemin_dans_le_conteneur ansible:2.10
#exemple
# docker run -it -v /home/toto/mon_super_playbook:/mnt/tmp/ ansible:2.10
```

Une fois le conteneur démarré on se contente de lancer notre playbook avec la commande suivante:
```bash
ansible-playbook -i <chemin_vers_le_fichier_d_inventaire> <chemin_vers_le_playbook>
```

## intégration dans les chaines gitlab

> todo