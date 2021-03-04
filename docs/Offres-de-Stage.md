# Internship Offers / Offres de stage
## Abstract
This module contains 2 screens: a list of available internship offers and a screen displaying the details of an offer

## Internship Listing

`activities/StagesActivity.kt` contains the code setting up the screen, namely instanciating the recyclerView with datas queried using `services/InternshipOfferService.kt`. The model name for an Internship Offer is `Stage`.

`res/layout/activity_stages.xml` is the main layout for this screen.

Upon touching one of the Internship Offers listed, one is redirected to the details of this offer. 

### RecyclerView

The RecyclerView handling the list generation uses the adapter `adapters/StageRecyclerAdapter.kt`. It uses the layout `res/layout/component_stage_list_item.xml` for its list items.

This adapter is mostly empty, and its main purpose is to hold the inner class `StageViewHolder`, which populate the layout for each list element.

## Internship Details

`activities/StageDetailsActivity.kt` contains the code populating the layout with datas. It does not query the back-end, as it obtains its data through the intent passed to it.

`res/layout/activity_stages_details.xml` is the main layout for this screen.