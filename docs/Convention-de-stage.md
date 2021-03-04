Ce module affiche la liste des Convention d'un étudiant.

La page de détails permet de voir en détail une convention.

Les conventions se basent sur ces modèles :

* [mockup de la liste des conventions](https://zpl.io/aBGdpmO).
* [mockup du détail d'une convention](https://zpl.io/brX0pYW).

# Architecture

<h3><a name="ConventionStageActivity">ConventionStageActivity.kt</a></h3>

Activité principale pour les conventions. Elle récupère la liste des conventions via [le service](#ConventionService).

```kotlin
loginPreferences.getString("username", "")?.let { it: String ->
            ConventionService().getAllByUser(onConventionResponse,this, it)
        }
```

La fonction onConventionResponse s'occupe ensuite d'initialiser un [adapter](#ConventionListViewAdapter) qui remplira une recyclerView.

La fonction onErrorResponse affiche un [toast](https://developer.android.com/guide/topics/ui/notifiers/toasts) si une erreur survient.

```kotlin
onConventionClick().getAll(item: Convention, position: Int)
```

la classe Intent est instancié pour récupérer des datas et les envoyer vers une autre Activity

<h3><a name="DetailsAnnoncesActivity">DetailsAnnoncesActivity.kt</a></h3>

Activité secondaire s'affichant après un click sur une Convention de la liste.


<h3><a name="ConventionListViewAdapter">ConventionListViewAdapter.kt</a></h3>

Classe récupérant la liste des conventions à afficher et l'adaptant pour la view. Elle commence par créer un [ViewHolder](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/RecyclerView.ViewHolder) possédant trois champs de texte, un pour la date, un pour le nom de l'entreprise et un pour le statut de la convention.

Elle bind ensuite le ViewHolder ainsi créé et la liste pour afficher toutes les conventions.

Une fonction getItemCount est disponible.

<h3><a name="ConventionService">ConventionService.kt</a></h3>



Classe servant d'intermédiaire entre l'application et l'API. Elle possède une fonction : getAllByUser. Cette fonction prends pour paramètre une fonction onResponse, une fonction onErrorResponse et l'id de l'utilisateur connecté.

<h3><a name="Convention">Convention.kt</a></h3>


Modèle des Conventions. Elle contient les membres :

 - id (Int)
 - langue (String) 
 - status (String) 
 - representantFirstName (String) 
 - representantLastName (String) 
 - entreprise (String) 
 - dateCreation (String) 
 - fonction (String) 
 - tel (String) 
 - mail (String) 
 - telEntreprise (String) 
 - mailEntreprise (String) 
 - site (String) 
 - type (String) 
 - tache (String) 
 - salaire (String) 
 - dateDebut (String) 
 - environment (String) 
 - dateFin (String) 

La classe possède une fonction fromJSONObject pour créer une Annonce à partir d'un objet JSON.