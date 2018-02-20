---
layout: post
title: GRUNT.JS for dummies
subtitle: 
bigimg: /img/grunt.jpg
thumbnail: /img/thumb_grunt.png
---

Purtroppo GoogleMaps non ha un'interfaccia che permette di creare delle mappe custom, con i marker che vogliamo (i puntatori per intenderci), quanti ne vogliamo e perchè no, modificare anche l'icona per rendere la mappa più gradevole alla vista.

Qui sotto vediamo come creare e popolare una mappa, inserendo quanti marker vogliamo e modificandole l'aspetto.

ma incominciamo!

prima di tutto dobbiamo inserire l'html, così:

{% highlight html %}
 <div id="map"></div>
{% endhighlight %}

Molto semplicemente, e ora passiamo alla parte JS.
per inizializzare la mappa, bisogna prima di tutto inserire questa porzione di codice:

{% highlight javascript %}
function initialize() {
var map = new google.maps.Map(document.getElementById('map'), {
        zoom: 13,
        center: new google.maps.LatLng(51.50, -0.11),
        mapTypeId: google.maps.MapTypeId.ROADMAP
      });

{% endhighlight %}

ma vediamolo nel dettaglio:
<b><i>var</i> map</b>: va a prendere la classe che abbiamo dato al nostro div sopra.
<b>zoom</b>: molto semplicemente, possiamo decidere il livello dello zoom che avrà la nostra mappa, ovviamente più è grande il numero più avrà zoom la mappa.
<b>center</b>: qui si decide quale sarà il centro della mappa che andremo a creare, in questo caso noi abbiamo indicato delle cordinate, ma è possibile inserire anche un marker.
<b>mapTypeId</b>: da questa opzione si decide il tipo di mappa che vogliamo visualizzare

* roadmap: Quella di default,
* satellite: visualizza la mappa satellitare,
* hybrid: è un ibrido tra i primi due,
* terrain: selezionando questa opzione è possibile visualizzare la mappa con uno stile che da risalto ai terreni e alle sue informazioni.

Ora, dopo che abbiamo deciso come sarà la nostra mappa, possiamo popolare la nostra mappa, inserirendo queste righe di codice:

{% highlight javascript %}
var locations = [
        ['<span style="font-weight: bold">London Eye</span>, London', 51.503399, -0.119519],
        ['<span style="font-weight: bold">Big Ben</span>', 51.510357, -0.116773],
        ['<span style="font-weight: bold">Buckingham Palace</span>, London', 51.501476, -0.140634],
      ];
{% endhighlight %}

questa parte di codice è molto semplice, in questa variabile, passiamo un array contentente le informazioni che ci servono per popolare la mappa, infatti nella prima parte inseriamo il markup e il testo che vogliamo che si visualizzi una volta cliccato sul puntatore, nella seconda parte le coordinate del marker.

Infine, dobbiamo inserire questa ultima parte di codice, per modificare le icone e fare un ciclo for per visualizzare correttamente i marker:

{% highlight javascript %}
 var infowindow = new google.maps.InfoWindow();
  
      var marker, i;
  
      var iconBase = 'http://maps.google.com/mapfiles/ms/micons/';
          var icons = [iconBase + 'red-dot.png',
                      iconBase + 'ferry.png',
                      iconBase + 'ferry.png',
                      iconBase + 'ferry.png'];
  
  
      for (i = 0; i < locations.length; i++) {  
        marker = new google.maps.Marker({
          position: new google.maps.LatLng(locations[i][1], locations[i][2]),
          map: map,
          icon: icons[i]
        });
  
        google.maps.event.addListener(marker, 'click', (function(marker, i) {
          return function() {
            infowindow.setContent(locations[i][0]);
            infowindow.open(map, marker);
          }
        })(marker, i));
      }
  }
{% endhighlight %}

da come si evince dal codice sopra, possiamo dividere la porzione in tre sezioni:

<b>modifica delle icone</b>: la prima porzione di codice modifica con una semplice concatenazioni di variabili e con un array, l'icona che verrà visualizata al posto del puntatore di default (si può trovare l'elenco di tutte le icone cercando velocemente su google).
<b>Il cliclo for</b>: molto semplicemente con questo ciclo andiamo a stampare tutti i marker andando a prendere le coordinate che abbiamo inserito nel primo array.
infine, andiamo a intercettare l'<b>evento</b> che al click apre la modale, inserendo tutte le impostazioni per crearla correttamente.

Infine, per far funzionare correttamente la mappa, dobbiamo inserire lo script che gestisce le API che abbiamo utilizzato in questo post, lo possiamo inserire infondo alla nostra pagina, così:

{% highlight html %}
 <script async defer src="https://maps.googleapis.com/maps/api/js?key=AIzaSyAgGqDyRzOb655kefklsqI12vpj2idk8Es&callback=initialize"> </script>
{% endhighlight %}

e se abbiamo creato la nostra mappa correttamente, dovremmo avere una mappa simile a questa: 

   <div id="map"></div>
      <script>
         function initialize() {
      
  
      var map = new google.maps.Map(document.getElementById('map'), {
        zoom: 13,
        center: new google.maps.LatLng(51.50, -0.11),
        mapTypeId: google.maps.MapTypeId.ROADMAP
      });
  
      var locations = [
        ['<span style="font-weight: bold">London Eye</span>, London', 51.503399, -0.119519],
        ['<span style="font-weight: bold">Big Ben</span>', 51.510357, -0.116773],
        ['<span style="font-weight: bold">Buckingham Palace</span>, London', 51.501476, -0.140634],
      ];
      
      var infowindow = new google.maps.InfoWindow();
  
      var marker, i;
  
      var iconBase = 'http://maps.google.com/mapfiles/ms/micons/';
          var icons = [iconBase + 'red-dot.png',
                      iconBase + 'ferry.png',
                      iconBase + 'ferry.png',
                      iconBase + 'ferry.png'];
  
  
      for (i = 0; i < locations.length; i++) {  
        marker = new google.maps.Marker({
          position: new google.maps.LatLng(locations[i][1], locations[i][2]),
          map: map,
          icon: icons[i]
        });
  
        google.maps.event.addListener(marker, 'click', (function(marker, i) {
          return function() {
            infowindow.setContent(locations[i][0]);
            infowindow.open(map, marker);
          }
        })(marker, i));
      }
  }
      </script>
      <script async defer
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyAgGqDyRzOb655kefklsqI12vpj2idk8Es&callback=initialize">
      </script>

E questo è tutto!
Come al solito, per qualsiasi dubbio o problemi, non esitate a commentare!

