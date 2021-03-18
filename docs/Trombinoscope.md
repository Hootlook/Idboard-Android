<h1>Résumé</h1>
Ce module permet d'afficher les étudiants de la classe de l'utilisateur.

<h1>Architecture</h1>
**TrombinoscopeActivity.kt**
Il s'agit de l'activité principale pour le trombinoscope. Elle va récupérer la liste des élèves ainsi que le nom de la classe via le service "StudentService.kt"

```kotlin
val apiService = StudentService()
apiService.getAllStudentByClass()
```

La fonction va permettre d'initialiser l'adaptateur pour remplir une recyclerView avec la liste des étudiants de la classe de l'utilisateur.

**StudentAdapter.kt**
Cette classe permet de récupérer la liste des étudiant à afficher et va l'adapter pour la vue.
Elle permet de créer un ViewHolder avec 2 champs texte, un pour le numéro de l'étudiant et l'autre
pour son nom.
Elle va ensuite binder le ViewHolder pour permettre d'afficher la liste des élèves.

**StudentService.kt**
Il s'agit de la classe qui sert d'intermédiaire entre l'application et l'API. Elle possède 1 fonction: getAllStudentByClass. Elle contient deux fonctions override onResult et onFailure.

**Student.kt**
Il s'agit de la classe modèle des étudiants. Elle contient : 

data class Student :
- "classLabel" : String
- "dateStart" : String
- "dateEnd" : String
- "photoPath" : String
- "students" : List <Students>

data class Students :
- "firstName" : String
- "lastName" : String
- "idboard" : String
- "avatarURL" : String

La classe possède une fonction "fromJSONObject" pour faire instancier un utilisateur à partir d'un JSON.

**activity_trombinoscope.xml & component_trombinoscope.xml**
Il s'agit des deux pages qui gèrent l'affichage du module trombinoscope.
L'activité gère l’entièreté de la page, et place correctement tous les éléments, tandis que le component permet d'afficher la liste des élèves.
