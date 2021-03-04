<h1>Résumé</h1>
Le drawer permet d'afficher un menu de navigation contenant toutes les fonctionnalités de l'application sous forme de sous-menu.

<h1>Architecture</h1>
<h3>BaseActivity.kt & activity_base.xml</h3>

<h3><a name="messageAdapter">BaseActivity.kt</a></h3>

BaseActivity contient les informations pour afficher le profil ,  les menus et les sous menus.

La drawer contient un profil qui contient nos infos personnelles :
```
 profile(prenomEtNom, adresseMail) {
                    iconUrl = "https://avatars3.githubusercontent.com/u/887462?v=3&s=460"
                    identifier = 101
                }
```

C'est grâce à un système de cache que l'on obtient nos infos personnelles :

```        val prenom = sharedPref.getString(PRENOM, "")
        val nom = sharedPref.getString(NOM, "")
        val prenomEtNom = prenom + " " + nom
        val adresseMail = sharedPref.getString(EMAIL , "")
```

Pour afficher un menu , on utilise cette ligne de code :
```       
expandableItem("Mes informations") {
identifier = 19
selectable = false
}
```
identifier correspond au numéro d'identifiant du menu;
selectable permet de choisir ou non si le menu est "selectionnable" , c'est à dire que si l'on clique dessus , il reste coloré.

Et pour afficher un sous menu , on ajoute à l’intérieur de "expandableItem" ce code :
```       
expandableItem("Mes informations") {
  identifier = 19
  selectable = false
  secondaryItem("Planning") {
   identifier = 2002
   selectable = false
   onClick(openActivity(MainActivity::class))
  }
```

On utilise donc secondaryItem qui est un sous menu.
Le nouveau paramètre permet de lier le clique à une activité , ici si on clique sur le sous menu "Planning" on ouvre le MainActivity.

Pour utiliser le drawer dans votre activity , il suffit de faire un héritage de la classe BaseActivity() et remplacé l'interieur de la classe par ceci.

```       
class ExempleActivity: BaseActivity() {

    override fun applyContentView(){
        setContentView(R.layout.activity_exemple)
    }

    override fun getToolbar(): Toolbar {
        return this@ExempleActivity.toolbar
    }
}

```

R.layout.activity_exemple correspond au xml de votre activity.
ExempleActivity.toolbar correspond à l'id de la toolbar qui sera dans le xml de votre activity (voir ci dessous).

<h3><a name="messageAdapter">activity_base.xml</a></h3>

Ce fichier xml contient la toolbar
```       
<androidx.appcompact.widget.Toolbar
 android:id="@+id/toolbar"
 android:layout_width="45dp"
 android:layout_height="match_parent"
 android:background="@drawable/background"
 android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
 app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
 />  
```
ainsi que d'autres informations non importantes au fonctionnement de la toolbar

Pour utiliser la toolbar dans un autre xml , il suffit de copier coller le xml.