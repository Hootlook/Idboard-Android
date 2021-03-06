# INTRO

Ce module affiche les notes des utilisateurs

The entry point is a list of domains: [Domains view mockup](https://zpl.io/b67NY1W).

By clicking a domain user have a possibility to see detailed marks for the chosen domain: [Detailed marks view mockup](https://zpl.io/a7BOM4K)

##  Architecture

### MarksActivity.kt

MarksActivity est l'activité principale pour les notes. Elle va récupérer la liste des notes via le service **MarkService.kt**

On commence par une liste de variable

```kotlin
var loader: ProgressBar? = null
val apiService = MarkService()
```

Explication des paramètres :

1. loader sert pour la barre de chargement
2. apiService permet de faire appel à notre **MarkService.kt**

On récupère nos notes grâce à notre service qui est le suivant

```kotlin
     apiService.getContext(this@MarksActivity)
     apiService.getAllMarksForAStudent {
        if(it!=null){
            loader?.visibility = View.GONE
            var domains = it
            if (domains != null) {
                domainList = domains as List<Domain>
                fragmentData.putParcelableArrayList("domains", domains as ArrayList<Domain>)
                fragmentDomains?.arguments = fragmentData
                if (!fragmentDomains!!.isAdded) {
                    fm.beginTransaction().add(R.id.frame_container, fragmentDomains as DomainsFragment)
                                .commit()
                    currentFragment = "domains"
                }
            }
        }
    }
```

Notre fonction s'occupe ensuite d'initialiser un adapter (**MessageAdapter**) qui remplira une recyclerView.

### Activity.kt

Le point d'entré du module est une activité: `fr/campusid/idboard/activities/MarksActivity.kt`

L'activité héberge deux fragments résponssable pour l'affichage de deux vue: la vue "Domain" et "Matters" avec Marks View:

```kotlin
private var fragmentDomains: Fragment? = null
private var fragmentMattersWithMarks: Fragment? = null
```
L'activité crée la requete au près du service 

```kotlin
apiService.getAllMarksForAStudent { }
```
- Affiche les fragments:
```kotlin
private fun replaceFragment(fragment: Fragment) {
    val fm: FragmentManager = supportFragmentManager
    fm.beginTransaction().apply {
        if (fragment.isAdded) {
            show(fragment)
        } else {
            add(R.id.frame_container, fragment)
        }
        fm.fragments.forEach {
            if (it != fragment && it.isAdded) {
                hide(it)
            }
        }
    }.commit()
}
```

```kotlin
fun goToMarks(position: Int) {  
    val matters = domainList[position].matters  
  fragmentData.putParcelableArrayList("matters", matters as ArrayList<Matter>)  
    fragmentMattersWithMarks?.arguments = fragmentData  
  replaceFragment(fragmentMattersWithMarks as MattersWithMarksFragment)  
    currentFragment = "matters"  
}
```

```kotlin
fun goToDomains() {  
    replaceFragment(fragmentDomains as DomainsFragment)  
    removeFragment(fragmentMattersWithMarks as MattersWithMarksFragment)  
    currentFragment = "domains"  
}
```

- et passe les données dans le fragment:

```kotlin
fragmentData.putParcelableArrayList("matters", matters as ArrayList<Matter>)
fragmentMattersWithMarks?.arguments = fragmentData
```

Layout pour l'activité: `layout/activity_marks.xml`

### DomainsFragment.kt

Ce fragment affiche les données de tous les domaines.

Il utilise [Recyclerview](https://developer.android.com/jetpack/androidx/releases/recyclerview) pour afficher une liste customisé d'éléments graphique (domains).

Recyclerview requiere l'adapteur ( `DomainListViewAdapter.kt` ) et le ViewHolder ( `DomainViewHolder.kt` ) pour assigner les contenu sur le bon élément graphique dans `component_domain.xml`.

`DomainListViewAdapter.kt`

```kotlin
init {
    mNameView = itemView.findViewById(R.id.md_name)
    mStatusView = itemView.findViewById(R.id.md_status)
    mOnModuleClickListener = onModuleClickListener
    itemView.setOnClickListener(this)
}

fun bind(domain: Domain) {
    color = when(domain.isValidate) {
        true -> ContextCompat.getColor(context, R.color.moduleGreen)
        false -> ContextCompat.getColor(context, R.color.moduleRed)
    }
    val status = if (domain.isValidate) "ValidÃ©" else "Non validÃ©"
    mNameView?.text = domain.description
    mStatusView?.text = status
    mStatusView?.setTextColor(color)
}
```

`component_domain.xml`

```xml
<TextView
    android:id="@+id/md_name"
    android:layout_width="270dp"
    android:layout_height="wrap_content"
    android:gravity="start"
    android:text="@string/module_name"
    android:textSize="19sp" />
```
Ce fragment implemente une interface custome `OnModuleClickListener` pour détecter le clic d'un utilisateur et le redirige sur le bon domaine:

```kotlin
override fun onModuleClick(position: Int) {  
  (activity as MarksActivity).goToMarks(position)  
}
```

Layout pour le fragment: `layout/fragment_domains.xml`.

### MattersWithMarksFragment.kt

Ce fragment affiche les matières avec les notes.

It uses two nested [Recyclerview](https://developer.android.com/jetpack/androidx/releases/recyclerview) to display a list of Matters with nested list of Marks.

Deux adapteurs (`MatterAdapter.kt` et `MarkAdapter.kt`) et deux ViewHolders (classe ViewHolder dans `MatterAdapter.kt` et `MarkAdapter.kt`) sont nécéssaires pour injecter le contenu dans le bon élément graphique dans `component_matter.xml` et `component_mark.xml`.

`MarkAdapter.kt`

```kotlin
class ViewHolder(view: View) : RecyclerView.ViewHolder(view)
{
    private var mSubjectView: TextView? = null
    private var mMarkView: TextView? = null
    private var mDateView: TextView? = null

    init {
        mSubjectView = itemView.findViewById(R.id.mk_subject)
        mMarkView = itemView.findViewById(R.id.mk_mark)
        mDateView = itemView.findViewById(R.id.mk_date)
    }

    fun bind (mark: Mark) {
        mSubjectView?.text = mark.name
        mMarkView?.text = mark.value.toString()
        mDateView?.text = mark.date
    }
}
```

`component_mark.xml`

```xml
<TextView
    android:id="@+id/mk_subject"
    android:layout_width="300dp"
    android:layout_height="wrap_content"
    android:gravity="end"
    android:textColor="@color/label_mark"
    android:text="@string/mark_subject"
    android:textSize="19sp" />
```

Ce fragment contient également un `onClickListener` pour détecter les saisies de l'utilsateur si il veut changer de vue (retourner sur la liste des Domaines):
```kotlin
View.OnClickListener {  
    (activity as MarksActivity).goToDomains()  
}
```
Layout pour le fragment: `layout/fragment_matters_with_marks.xml`

## Model des données

Trois classes sont crées pour contenir trois niveaux de données pour le module: `Domain.kt`, `Matter.kt` et `Mark.kt`.

`Mark.kt`

```kotlin
@Parcelize
data class Domain(

        @SerializedName("matters") val mattersList : List <Matters>? = null,
        @SerializedName("label") val label: String = ""
) : Parcelable
@Parcelize
data class Matters(
        @SerializedName("marks") val marksList : @RawValue List <Mark>,
        @SerializedName("reference") val reference: String = "",
        @SerializedName("label") val label: String = "",
        @SerializedName("marksNumber") val marksNumber: String = ""
) : Parcelable

data class Mark(
        @SerializedName("value") val value: String = "",
        @SerializedName("id") val id: String = "",
        @SerializedName("typeEvaluation") val typeEvaluation: String = "",
        @SerializedName("coefficient") val coefficient: String = "",
        @SerializedName("comments") val comments: String = "",
        @SerializedName("isJustifiedAbsence") val isJustifiedAbsence: String = ""

)
```
```@Parcelize``` Cet attribut est utilisé pour tranférer les données entre les fragments.

```kotlin
fragmentData.putParcelableArrayList("matters", matters as ArrayList<Matter>)
fragmentMattersWithMarks?.arguments = fragmentData
```

## Service
Ce service fait la requète et récupere les données (tous domaines avec les Matières et Notes pour l'utilisateur)
```kotlin
fun getAllMarksForAStudent(onResult: (List<Domain>?) -> Unit) { }
```
