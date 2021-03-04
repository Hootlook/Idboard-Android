<h1>Résumé</h1>
La page de login permet de se connecter à l'application grâce au numero IDBoard ainsi que son mot de passe

<h1>Architecture</h1>
<h3><a name="messageAdapter">LoginActivity.kt</a></h3>

LoginActivity contient un système de cache pour pouvoir avoir nos informations personnelles sauvegardé sur le téléphone lors de notre connexion.

Toutes les informations sont stockées dans une classe qui se nomme CurrentUser et qui contient toutes les informations suivantes
```
private var PRIVATE_MODE= 0
private val MYPREFERENCE = "mypref"
private val NOM = "nomKey"
private val NUMEROIDBOARD = "numeroidboardKey"
private val PASSWORD = "passwordKey"
private  val PRENOM = "prenomKey"
private val NAISSANCE ="naissanceKey"
private val ADRESSE = "adresseKey"
private val TELEPHONE = "telephoneKey"
private val EMAIL = "emailKey"
```
PRIVATE_MODE permet de protéger l’accès à nos données , il faut le déclarer de nouveau dans une autre activity pour pouvoir accéder aux données.

MYPREFERENCE permet de donner un nom à notre stockage.

- NOM
- NUMEROIDBOARD 
- PASSWORD
 -PRENOM
- NAISSSANCE
- ADRESSE
 -TELEPHONE
- EMAIL 

sont les différentes valeurs qui seront dans le cache.

Nous avons 2 vérifications :
- Le numéro de l'IDBoard doit avoir une longueur de 7 chiffres
- Le mot de passe doit être "test"

Si les conditions sont bonnes , alors on fait un appel à notre backend
 
```
 UserService().get(onUserResponse, this, user_name);
```

et on utilise 
```
var onUserResponse = fun(user: User) {
        CurrentUser.getInstance(this).setCacheUser(user)
        val intent = Intent(this, MainActivity::class.java)
        startActivity(intent)
    }
```

onUserResponse appel la classe CurrentUser qui contient le système de cache et l'applique sur notre réponse
intent permet de lancer la classe MainActivity.

```
override fun onErrorResponse(error: VolleyError?) {
        numero_id_board?.setError("Identifiant ou mot de passe incorrect !")
        render.setAnimation(Attention().Bounce(numero_id_board!!))
        render.setDuration(800)
        render.start()
    }
```

onErrorResponse est utilisé lors d'une erreur lors de l'appel. Ici la plupart du temps, cela viendrait d'un soucis d'identifiants ou de mot de passe.

Notre page de login à la possibilité de se souvenir de nous , toujours grâce à un système de cache

```
var loginPreferences = getSharedPreferences("loginPrefs",Context.MODE_PRIVATE)
var loginPrefsEditor = loginPreferences.edit()
```
On récupère dans une variable notre cache qui à comme nom "loginprefs" et on utilise le Context.MODE_PRIVATE qui permet de protéger l'accès à nos données.
Et on ouvre l’édition grâce à la fonction edit().

Si le bouton "se souvenir de moi" est cliqué , alors on insère les informations dans le cache
```
 if (saveLoginCheckBox.isChecked()) {
                loginPrefsEditor.putBoolean("saveLogin", true);
                loginPrefsEditor.putString("username", user_name);
                loginPrefsEditor.putString("password", pass);
                loginPrefsEditor.apply();
```
On insère grâce à la fonction putString() ou putBoolean() selon le type de données.
Le premier paramètre est le nom de la donnée dans le cache
user_name correspond à notre numéro IDBOARD
pass correspond à notre mot de passe
On utilise apply() pour appliquer les modifications dans le cache , sinon ça ne fonctionne pas.

Pour récupérer les données de notre cache on utilise
```
 loginPreferences.getString("username", ""))
 loginPreferences.getString("password", ""))
```
Au lieu d'utiliser "putString" , on utilise "getString" qui permet de recuperer l'information que l'on veut dans le cache.

Attention à bien mettre en premier paramètre le bon nom comme ici "username".

Le deuxième paramètre doit être vide.
