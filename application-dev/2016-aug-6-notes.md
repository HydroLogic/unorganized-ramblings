
## To-do

### September 6th, 2016

#### Summary

There are a number of different branches we can work on. I've highlighted the two I think we need to focus on in order to start talking with clients. I would appreciate input on level of difficulty.

**High priority**

* Building policy level schema and storage in MongoDB or Postgresql
  - Definitely need to make schema our first priority and interaction with the schema forms
    + I need help prioritizing schema characteristics or I'll just overload everything (as you can porbably tell from the random notes below)
  - Should we add in complex financial information or hold off and try for a most basic financial schema?
* Drag and drop csv / excel input of location information
  - Would like it if we could make it automatic that if a flat-file or excel is dragged to screen it pops up an import form
    + Only able to do 5 locations if not logged in
    + If excel is very difficult then let's focus on flat-file
  - Drop a pin for each location and fill out forms
  - Ideally we would include normalizing of header / automatic matching to our schema

**Low priority**

* Outline building from satellite imagery and calculate surface area
* Multiple building breakout for a policy


#### Schema Discussion

The following is an overview of how I see the application evolving and what to keep in mind as we move forward. I'm trying to figure all this out as I go so bare with me. I have examples in parathesis. Looking at this from the top down we have the following:

Large Company Outline:
* Company (Allstate/Statefarm/Chubb)
  - Manager
    + Portfolio (Wind/Flood/Earthquake) - Can be combined with Account for now
      * Account (Subways/U-haul/Wafflehouse)
        - Site/Location (Single address)
          + Building (Office/Storage/Garage)

I would like to focus right now on the lower half of this ladder. Let's keep in mind the above but focus on being able to set up a stored DB and all the policy information associated with the users interaction. At its very basic we are looking at this:

* User has a login which creates a unique UserID
  - User has interface which they can create new portfolio/account
    + Menu pops up and shows all accounts and a (+) for a new account
      * Click on (+) get a blank screen with title bar on top showing the account name (Untitled Account) is default
        - Can click on the name of the account to change the name
      * Drag and drop locations to screen or start entering addresses
      * Save to account


##### Location Schema Single Location -

* Geographical Information Tab -
  *This should be filled out by either the input to the geocode or the JSON returned from the Geocode*
  - Latitude / Longitude
  - Address
    + Unit
    + Street Name
  - City
  - Postal Code
  - County
  - State

* Building Information Tab -
  - Building Name
  - Occupancy Type
  - Building Type (Building Class)
  - Foundation Type
  - Basement (Yes/No)
  - Floor Area (Square feet)
  - Number of Stories
  - Year Built

* Financial Information Tab -
  - Replacement Value - Total value
  - Coverage -
    + Attachment
    + Limit
    + Deductible
  - Coverage Type
    *Each of the below should have a field for Limit / Deductible*
    + Structure
    + Contents
    + Time Element


* Detailed Form Options:
  - Financial information
    + Replacement Value
    + Insured Value:
      * Structure Insured Value
      * Contents Insured Value
      * Business Interruption / Time Element Insured
  - Building information -
    + Building Class:
      * Residential
      * Commercial
      * Industrial
      * Aggriculture
    + Construction Class
      * Wood Frame
      * Masonry
      * Reinforced Concrete
      * Steel Frame
      * Mobile Home
    + Building Characteristics
      * First Floor Elevated (yes/no) if yes:
        - Slab
        - Crawl Space
        - Basement (if basement automatically select yes)
      * Basement (yes/no) if yes:
        - Finished basement (yes/no)
        - Basement contents (yes/no)
      * Number Stories
        - 1
        - 2
        - 3-5
        - 5-10
        - 10+
* Ignore all these, just copying for later

    + Roof Age
      * < 5 years
      * 5-10 years
      * > 10 years
    + Roof Type
      * Hip
      * Flat
      * Gable
    + Roof Covering
      * Tiles
      * Shingles
      * Rated shingles
      * Rated shingles w secondary water barrier
    + Roof Connections
      * Not connected
      * Toenail
      * Straps
      * Clips
    + Window Protection
      * Poor shutters
      * Good shutters
      * Impact resistant glass


```json
  {
  "PropertyID":"XXXXXXXXXX",
  "PropertyName":"U-haul",
  "Buildings": [

  ]
  }
```


* Portfolio ID
* Portfolio Name
* Account ID
* Account Name
* Financials:
  - Account Limit
  - Account Deductible
  -

Building Schema:

* Policy ID
* Account ID
* Location Name
* Building Name
* Financial
  - Location Limit
  - Location Deductible
    + Deductible type
      * Net -
      * Additional -

##### Outline building option:

1. Geocode pin as normal
  * Create unique policy ID for location
    - Allows for a unique identifier in order to save more buildings to policy information
  * Create building ID for first pin
    - Allow for subsequent new building ID's as pins are dropped
2. Add in leaflet draw menu in the top left (polygon and point only) to drop extra pins / draw building polygon outline
  * If polygons are drawn, calculate polygon area and automatically fill form menu
3.




















