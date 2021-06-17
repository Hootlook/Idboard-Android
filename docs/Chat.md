<h1>Résumé</h1>
Ce module permet d'afficher un chat pour discuter entre étudiant avec une connection avec un compte google.

<h1>Architecture</h1>
**ChatGlobalActivity.kt**
Il s'agit de l'activité principale pour le chat.
Il utilise le log de google pour gerer les utilisateurs.

Fonction :
onCreate :
	Créer l'activité du chat.

onCreateViewHolder : 
	Affiche la liste des messages.

onBindViewHolder : 
	Assigne le message au view holder.

onItemRangeInserted :
	Gère la position du message.

beforeTextChanged :
	Gère l'état avant de l'activité le changement de texte.

onTextChanged : 
	Gère l'état de l'activité au moment du changement de texte.

afterTextChanged :
	Gère l'état de l'activité après le changement de texte.

onStart / onPause / onResume / onDestroy :
	Differentes Actions pour gérer les états du chat.

onCreateOptionsMenu :
	Gestion de la création du menu.

onOptionsItemSelected :
	Gestion de la séléction dans le menu.

onActivityResult :
	Gestion de ce que retourne l'activité.

putImageInStorage :
	Enregistre l'image de l'utilisateur.
