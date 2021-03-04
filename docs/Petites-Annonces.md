# Résumé

 Ce module affiche la liste des petites annonces.

La page de détails permet de voir en détail une annonce.

Les petites annonces se basent sur ces modèles : 
- [mockup de la liste des annonces](https://zpl.io/blxkANe).
- [mockup du détail d'une annonce](https://zpl.io/VDdroMg).

# Architecture

<h3><a name="AnnoncesActivity">AnnoncesActivity.kt</a></h3>

Activité principale pour les annonces. Elle récupère la liste des annonces via [le service](#annonceService).

```kotlin
AnnonceService().getAll(onAnnonceResponse, this)
```

La fonction onAnnonceResponse s'occupe ensuite d'initialiser un [adapter](#annonceAdapter) qui remplira une recyclerView.

La fonction onErrorResponse affiche un [toast](https://developer.android.com/guide/topics/ui/notifiers/toasts) si une erreur survient.

```kotlin
onAdClick().getAll(item: Annonce, position: Int)
```
la classe Intent est instancié pour récupérer des datas et les envoyer vers une autre Activity

<h3><a name="DetailsAnnoncesActivity">DetailsAnnoncesActivity.kt</a></h3>

Activité secondaire s'affichant après un click sur une annonce de la liste.

```kotlin
sendEmail(email :String)
```
Fonction permettant de récupérer le mail de l'annonce et d'ouvrir un client mail afin d'envoyer un mail à cette adresse ! 

<h3><a name="AdListViewAdapter">AdListViewAdapter.kt</a></h3>

Classe récupérant la liste des annonces à afficher et l'adaptant pour la view. Elle commence par créer un [ViewHolder](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/RecyclerView.ViewHolder) possédant trois champs de texte, un pour la date, un pour le titre et un pour le prix.

Elle bind ensuite le ViewHolder ainsi créé et la liste pour afficher toutes les annonces.

Une fonction getItemCount est disponible.

<h3><a name="annonceService">AnnonceService.kt</a></h3>

Classe servant d'intermédiaire entre l'application et l'API. Elle possède une fonction : getAll. Cette fonction prends pour paramètre une fonction onAnnonceResponse et une fonction onErrorResponse.


<h3><a name="Annonce">Annonce.kt</a></h3>

Modèle des Annonces. Elle contient les membres :

* id (int)
* description(string)
* idBusinnessEntity (string)
* nom(string)
* prix(string)
* date(string)
* photo(string)
* user(User)
La classe possède une fonction fromJSONObject pour créer une Annonce à partir d'un objet JSON.