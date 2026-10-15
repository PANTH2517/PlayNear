# PlayNear

**Mini Project : Mobile Application Development ( MAD )**

| | |
|---|---|
| **College** | SVKM's NMIMS , MPSTME , Shirpur Campus |
| **Programme** | B.Tech Computer Engineering |
| **Subject** | Mobile Application Development ( MAD ) |
| **Professor** | Dr. Atul Dusane |
| **Team** | Panth Haveliwala , Panjari Shah , Aditi Patil |

PlayNear is an Android app, built in Java and XML, for finding and creating recreational sports games at venues near you in Shirpur and Dhule. Its main feature is location. The app reads the phone's location, shows the distance to every game and venue, and lists the nearest first. Navigate opens Google Maps on the venue.

## Features

- **Register and Login:** one page for both. Passwords are stored as a salted SHA-256 hash.
- **Find Games:** all upcoming games, nearest first, with a distance in km. Filter by sport and by distance ( 2 , 5 or 10 km ). Tap a game to see the details and the players , then Join Game or Navigate.
- **Venues:** all venues, nearest first, filtered by sport. Tap a venue to Navigate. Add Venue lets you add a new one.
- **My Games:** the upcoming games you created or joined, with their status. Games that have already started are hidden. Tap a game to Leave Game or Navigate.
- **Create Game:** choose a sport and a venue, then pick a date and a start time with the date and time pickers, and set the maximum players ( 2 to 30 ).
- **Add Venue:** enter a name and address, then tap a map to place the venue marker, or use your current location.
- **Navigate:** opens Google Maps on the venue. Directions and Start are chosen inside Google Maps.
- **Status:** each game shows OPEN or FULL. A game that has started is treated as COMPLETED and is no longer shown.
- **Errors:** every error is shown as a Toast.

## Screens ( 7 activities )

| Activity | What it does |
|---|---|
| MainActivity | Register and Login |
| HomeActivity | Greeting and buttons to the other pages , Log Out |
| FindGamesActivity | Find , filter , Join and Navigate to games |
| VenuesActivity | List venues , Navigate , Add Venue |
| MyGamesActivity | Games you created or joined , Leave |
| CreateGameActivity | Create a game |
| AddVenueActivity | Add a venue by tapping a map |

Every page except Login has a Back button. It uses an explicit Intent.

## Tech used

- **Language:** Java and XML only
- **Layouts:** ConstraintLayout , TextView , EditText , Button , Spinner and ListView
- **Widgets:** ArrayAdapter , AlertDialog , Toast , DatePickerDialog and TimePickerDialog
- **Navigation and data:** explicit Intents with putExtra , startActivityForResult and SharedPreferences ( stores the logged-in user )
- **Location:** LocationManager and LocationListener , with a runtime permission request
- **Map:** WebView with OpenStreetMap ( Leaflet ) for picking a venue location
- **Navigate:** an implicit Intent to Google Maps
- **Database:** Firebase Realtime Database , accessed over HTTP with HttpURLConnection and read with JSONObject
- **Library:** AppCompat and ConstraintLayout only

## Database

All data is stored online, so every phone sees the same users, venues and games.

`DBHelper.java` holds all the database code. It talks to the Firebase Realtime Database over the internet. It sends GET , PUT and DELETE requests and reads the replies as JSON. It has methods such as `registerUserHelper` , `loginUserHelper` , `getGamesHelper` , `joinGameHelper` and `leaveGameHelper`.

```
users    u<id>      name , email , password ( salt:hash )
emails   <email>    user id ( used to find a user by email )
venues   v<id>      name , sport , address , lat , lng
games    g<id>      creatorId , creatorName , venueId , venueName , address , lat , lng ,
                    sport , date , time , maxPlayers , status ,
                    players { u<id> : name }
```

The database starts empty. Register a user, add venues and create games from the app.

### Original SQLite version

The first version of the app used a local SQLite database, written the classic way taught in our lab experiments. That code is kept in `OldDBHelper.java`. The whole file is commented out, so it is not part of the running app. It is also saved in git as the tag `sqlite-version`.

The database was later moved online on the advice of our professor. We took help of AI for that online integration ( the new `DBHelper.java` ). The method names stayed the same, so the activities did not change.

## How to run

1. Open the project folder in Android Studio.
2. Wait for Gradle to sync. The first sync needs internet.
3. Run the app on an emulator or a phone.
4. Allow the location permission when asked.

To test distances on the emulator, set a location in Extended controls , for example 21.35 , 74.88.

## Notes

- The app needs internet. Without it, a Toast says "No internet connection".
- The map in Add Venue is an OpenStreetMap page in a WebView, so it needs internet. "Use My Current Location" works without it.
- Navigate needs Google Maps installed on the phone.
- The Firebase database rules are in test mode ( open for a limited time ). Set proper rules in the Firebase console before the app is used by many people.
- Older versions are saved as git tags : `final-version` , `sqlite-version` , `version-before-simplify` and `full-version`.

## Project structure

```
app/src/main/java/com/example/playnear/
    MainActivity , HomeActivity , FindGamesActivity , VenuesActivity ,
    MyGamesActivity , CreateGameActivity , AddVenueActivity
    DBHelper        ( online database code )
    OldDBHelper     ( original SQLite code , commented )
    Game , Venue    ( small classes to hold one row )
app/src/main/res/layout/    one XML layout per activity , row.xml for list entries
```

## Requirements

- Android Studio
- Android 7.0 ( API 24 ) or higher
- Internet connection
