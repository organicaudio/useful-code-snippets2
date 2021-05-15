In shortcuts open specific url then copy and paste from below to perform action as described. This will open Google apps to continue actions. Apps have to be installed before actions will work.
```
Google App: googleapp://
Google Calendar com.google.calendar://
Google Calendar Create Event com.google.calendar://?action=create&title={{Title}}&description={{Description}}&location={{Location}}&add={{email,anotheremail}}&isallday={{0}}
Google Calenda View Event com.google.calendar://?action=view&type=event&eid={{insertId}}
Google Chrome googlechrome-x-callback://
Google Chrome Open Url googlechrome-x-callback://x-callback-url/open/?url={{urlhere.com}}
Google Chrome Open Url and Return googlechrome-x-callback://x-callback-url/open/?url={{urlhere.com}}&x-success={{shortcuts://}}&x-source={{Shortcuts}}
Google Chrome Search googlechrome-x-callback://x-callback-url/open/?url=https%3A%2F%2Fwww.google.com%2Fsearch%3Fq%3D{{Query}}
Google Chrome Search and Return googlechrome-x-callback://x-callback-url/open/?url=https%3A%2F%2Fwww.google.com%2Fsearch%3Fq%3D{{Query}}&x-success={{shortcuts://}}&x-source={{Shortcuts}}
Google Maps comgooglemaps://
Google Maps launch with Satellite View comgooglemaps://?views=satellite
Google Maps launch with Traffic view comgooglemaps://?views=traffic
Google maps launch with transit view comgooglemaps://?views=transit
Google Maps driving directions start/end point comgooglemaps://?saddr={{Start}}&daddr={{End}}&directionsmode=driving
Google Maps Search near area comgooglemaps://?q={{Query}}+near:{{Near Area}}
Google Sheets: googlesheets://url.com
```