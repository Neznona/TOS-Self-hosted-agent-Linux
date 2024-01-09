# TOS-Self-hosted-agent-Linux


## Installation d'un agent local linux

Nous allons voir comment installer un agent local azure sur une debian 12.

Les prérequis :
- L'accès à une debian 12 (où le copier coller est possible)
- Accès à internet
- Une organisation Azure Devops
- Un utilisateur dans le groupe "Project Collection Administrators"

> En ce qui concerne ce dernier prérequis, demander à l'admin de votre l'organisation, aller dans votre organisation puis "organisation setting" > Permission > "User of" et on va ajouter l'utilisateur au groupe.

Pour commencer on va aller dans notre "Organization setting" dans Azure, puis "Agent pool", dans "Default" on va faire "new agent" et choisir Linux.

Sur notre debian on va suivre les étapes :

```bash
~/$ mkdir myagent && cd myagent
~/myagent$ tar zxvf ~/Downloads/vsts-agent-linux-x64-3.227.2.tar.gz
```
On veillera bien à être dans le bon dossier, sinon cela pourrait créer des soucis plus tard dans notre pipeline.
Viens la configuration, dans notre dossier on va lancer notre script :
```bash
~/myagent$ ./config.sh
```
On nous demandera un "PAT" ou Personnal Acces Token. Pour cela on va devoir le générer.
Dans notre Azure on va aller dans les settings de notre utilisateur. Puis la page correspondante, on va créer un nouveau PAT, il est important de garder de coté la chaine de caractère donné à la fin et de l'insérer lors de la configuration.
On lancera aussi le ./run.sh pour lancer l'écoute.

Pour indiquer à notre YAML qu'il doit utiliser notre agent il faut intégrer ce bout de code:

    pool:
      name: default (nom de notre pool)
      demands:
       - agent.name -equals debian (nom de notre agent)


Si une erreur du type *"##[error]No agent found in pool Default which satisfies the specified demands: agent.name -equals debian, java, Agent.Version -gtVersion 2.163.1"* arrive sur notre pipeline, il faut simplement installer les composants manquants (ici debian) sur notre debian et relancer notre machine.
Dans le cadre de notre projet java est indispensable pour sonar.
On va pouvoir suivre ce lien : [Install Java on debian 11](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-debian-11) et surtout **redémarrer** notre agent.

Pour vérifier qu'un Job s'est bien lancé avec notre agent il y a deux façons de procéder : Directement regarder dans la pipeline si elle s'est lancé ou aller dans "Organization settings" puis "Agent pool", dans "Default" on va voir les jobs réussi ou non de notre agent.

#

La documentation associé : 
https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/linux-agent?view=azure-devops
 
> Written with [StackEdit](https://stackedit.io/).
