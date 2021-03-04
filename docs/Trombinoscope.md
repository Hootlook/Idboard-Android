<h1>Résumé</h1>
Ce module permet d'afficher les étudiants de la classe de l'utilisateur.
Le trombinoscope se base sur ce modèle: <a href="https://app.zeplin.io/project/5e187cb2853c8c01a6240e8d/screen/5e187dff7c824ea796c41913">mockup du trombinoscope</a>

<h1>Architecture</h1>
**TrombinoscopeActivity.kt**
Il s'agit de l'activité principale pour le trombinoscope. Elle va récupérer la liste des élèves ainsi que le nom de la classe via le service "UserService.kt"

```kotlin
UserService().get(onUser, this, "2010258" )
UserService().getAllByPromo(onUserResponse, this, nomDeLaClasse )
```

La première fonction permet de récupérer le nom de la classe en fonction d'un ID qui correspond à celui d'un étudiant.
La seconde fonction va permettre d'initialiser l'adaptateur pour remplir une recyclerView.
De plus cette classe contient la méthode "onErrorResponse" qui permet d'afficher un message d'erreur dans le cas ou il y a un problème.

**TrombiAdapter.kt**
Cette classe permet de récupérer la liste des étudiant à afficher et va l'adapter pour la vue.
Elle permet de créer un ViewHolder avec 2 champs texte, un pour le numéro de l'étudiant et l'autre
pour son nom.
Elle va ensuite binder le ViewHolder pour permettre d'afficher la liste des élèves.

**UserService.kt**
Il s'agit de la classe qui sert d'intermédiaire entre l'application et l'API. Elle possède 2 fonctions: getAll et getAllByPromo. Ces deux fonctions prennent pour paramètre une fonction onMessageReponse, onErrorResponse, et une information pour connaitre la classe de l'utilisateur.

**User.kt**
Il s'agit de la classe modèle des utilisateurs. Elle contient : 
- idBusinessEntity
- password
- FirstName
- Name
- DateOfBirth
- PhotoPath
- phone
- mail
- classeName

La classe possède une fonction "fromJSONObject" pour faire instancier un utilisateur à partir d'un JSON.

**activity_trombinoscope.xml & component_trombinoscope.xml**
Il s'agit des deux pages qui gèrent l'affichage du module trombinoscope.
L'activité gère l’entièreté de la page, et place correctement tous les éléments, tandis que le component permet d'afficher la liste des élèves.
