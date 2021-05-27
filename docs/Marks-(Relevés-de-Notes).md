# INTRO

The module is responsible for displaying user's marks.

The entry point is a list of domains: [Domains view mockup](https://zpl.io/b67NY1W).

It uses HTTP service to get the data.

By clicking a domain user have a possibility to see detailed marks for the chosen domain: [Detailed marks view mockup](https://zpl.io/a7BOM4K)

Ce module gere l'affichage 

##  Architecture

### MarksActivity.kt

MarksActivity est l'activité principale pour les notes. Elle va récupérer la liste des notes via le service **MarkService.kt**

On commence par une liste de variable

```kotlin
var loader: ProgressBar? = null
var apiService = MarkService()
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

The module's entry point is an activity: `fr/campusid/idboard/activities/MarksActivity.kt`

The activity hosts two fragments responsible for two views: Domain View and Matters with Marks View:

```kotlin
private var fragmentDomains: Fragment? = null
private var fragmentMattersWithMarks: Fragment? = null
```
The activity is responsible for:
- requesting the data using the HTTP service:

```kotlin
apiService.getAllMarksForAStudent {}
```
- managing (displaying) the fragments:
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
- and passing Data into the fragment:

```kotlin
fragmentData.putParcelableArrayList("matters", matters as ArrayList<Matter>)
fragmentMattersWithMarks?.arguments = fragmentData
```

Layout for the activity: `layout/activity_marks.xml`

### DomainsFragment.kt

The fragment is responsible for displaying all the domains data.

It uses [Recyclerview](https://developer.android.com/jetpack/androidx/releases/recyclerview) to display a list of customized graphical elements (domains).

Recyclerview requires the adapter ( `DomainListViewAdapter.kt` ) and a ViewHolder ( `DomainViewHolder.kt` ) to assign the content to the right graphical element in `component_domain.xml`.

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
Fragment implements a custom interface `OnModuleClickListener` to detect user's click what he/she wants to see details of the chosen domain:
```kotlin
override fun onModuleClick(position: Int) {  
  (activity as MarksActivity).goToMarks(position)  
}
```


Layout for the fragment: `layout/fragment_domains.xml`.

### MattersWithMarksFragment.kt

The fragment is responsible for displaying the Matters with Marks

It uses two nested [Recyclerview](https://developer.android.com/jetpack/androidx/releases/recyclerview) to display a list of Matters with nested list of Marks.

Two adapters (`MatterAdapter.kt` and `MarkAdapter.kt`) and a two ViewHolders (class ViewHolder in `MatterAdapter.kt` and `MarkAdapter.kt`) are required to assign the content to the right graphical element in `component_matter.xml` and `component_mark.xml`.

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
Fragment contains also an `onClickListener` to detect user's input if he/she wants to change the view (go back to list of Domains):
```kotlin
View.OnClickListener {  
    (activity as MarksActivity).goToDomains()  
}
```
Layout for the fragment: `layout/fragment_matters_with_marks.xml`

## Data model

Three classes are created to hold three levels of nested data for the module: `Domain.kt`, `Matter.kt` and `Mark.kt`.

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
```@Parcelize``` annotation is used to be able to send the data between fragments.

```kotlin
fragmentData.putParcelableArrayList("matters", matters as ArrayList<Matter>)
fragmentMattersWithMarks?.arguments = fragmentData
```

## Service
The service is responsible for collecting the data (all domains with matters and mark for the user).
```kotlin
fun getAllMarksForAStudent(onResult: (List<Domain>?) -> Unit){
    }
```
