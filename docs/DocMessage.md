<h1>Résumé</h1>
Le module permet de voir les messages globaux reçu (convention de stage...).

<h1>Architecture</h1>
<h3>MessagesGlobauxActivity.kt</h3>

MessagesGlobauxActivity est l'activité principale pour les messages globaux. Elle va récupérer la liste des messages globaux via le service <strong>MessageService.kt</strong>.

On commence par une liste de variable

    `var loader: ProgressBar? = null
	 val apiService = MessageService()`

Explication des paramètres :
<ul>
    <li>loader sert pour la barre de chargement</li>
    <li>apiService permet de faire appel à notre <strong>MessageService.kt</strong></li>
</ul>

<p>On récupère nos messages globaux grâce à notre service qui est le suivant</p>

    `apiService.getAll {
    	if(it != null) {
	    	val adapter = MessageAdapter(it)
		    loader?.visibility = View.GONE
		    val recyclerView = findViewById(R.id.lisView_messages) as RecyclerView
		    recyclerView.layoutManager = LinearLayoutManager(this, RecyclerView.VERTICAL, false)
		    recyclerView.adapter = adapter
	    }
     }`


<p>Notre fonction s'occupe ensuite d'initialiser un adapter (<Strong>MessageAdapter</strong>) qui remplira une recyclerView.</p>

<h3>MessageAdapter.kt</h3> 
<p>MessageAdapter.kt est l'adapter qui permet de récupérer la liste des messages globaux et va l'adapter pour la vue. Elle permet de créer un ViewHolder avec 2 champs : une date et une valeur. Elle bind ensuite le ViewHolder pour permettre l'affichage des messages.</p>

<h3>MessageService.kt</h3> 
<p>MessageService.kt est la classe qui sert d'intermédiaire entre l'application et l'API. Elle possède une fonction getAll.</p>

<h3>Message.kt</h3> 
<p>Message.kt est le modèle des messages.Elle contient :</p>
<ul>
    <li>id</li>
    <li>title</li>
    <li>content</li>
    <li>dateStart</li>
    <li>priority</li>
</ul>