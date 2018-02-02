---
layout: post
title: GRUNT.JS for dummies
subtitle: 
bigimg: /img/grunt.jpg
thumbnail: /img/thumb_grunt.png
---

Scrivo questo articolo per le persone che come me che, approcciandosi a questo task runner, vedevano solo un muro insormontabile.
Però avendo tempo a disposizione, e dovendo fare dei task con sass, ho voluto sperimentare Grunt e non è per niente difficile!
Partiamo con ordine, mi sono letto la maggior parte della documentazione presente sul sito <br />( https://gruntjs.com) e con questo articolo vedremo come fare i due task più importanti e indispensabili:

<strong>Sass</strong> (preprocessa i file scss generando i file css)
<strong>Watch</strong> (crea un task che monitora sempre i file scss generando automaticamente il file css)

<h2>Ma a cosa serve Grunt?</h2>
In poche parole, esegue dei compiti che gli assegnamo in modo automatizzato installando dei plugin che ci servono, che possono differenziarsi da progetto a progetto.
Potremmo aver bisogno del plugin Autoprefixer se abbiamo bisogno di inserirli per i vecchi browser, o aver bisogno di minimizzare i file css una volta compilati.

Per funzionare Grunt però, ha bisogno che ci sia installato node.js (per installarlo basta scaricarlo dal sito ufficiale, https://nodejs.org/en/download/ sia per windows che per mac è un normale installer).

Dopo aver installato node,js, siamo pronti con il nostro primo progetto, ma come utilizzare e installare Grunt?

Da terminale, entrati nella cartella del progetto, digitare:
{% highlight javascript %}
npm install -g grunt-cli
{% endhighlight %}
tutto qui! 
Per controllare che grunt sia stato installato correttamente, nella cartella del progetto, dovrebbe essere comparsa la cartella node_modules che prima non c’era.

ora, oltre alla cartella node_modules, per far funzionare correttamente grunt bisogna creare altri due file, <strong>package.json</strong> ed un altro chiamato <strong>Gruntfile.js</strong> e inserirli nella root del progetto.

Il file <strong>package.json</strong> contiene le informazioni essenziali che servono a Grunt per funzionare, come nell’esempio sottostante:
{% highlight javascript %}
{
  "name": "testgrunt",
  "description": “test grunt",
  "author": "dasan",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "^1.0.1",
  }
}
{% endhighlight %}

In questo file scriveranno automaticamente i plugins che installerai e vengono salvati i numeri di versioni degli stessi.

l’altro file da creare è <strong>Gruntfile.js</strong>.  
in questo file andrai a comandare grunt, inserendo tutte le istruzioni, le opzioni dei plugin stessi.

possiamo incominciare a inserire queste due righe:
{% highlight javascript %}
module.exports = function(grunt) {
  grunt.initConfig({
	  });
};
{% endhighlight %}

<h2>Plugin</h2>

Grunt per funzionare correttamente ha bisogno dei plugin, essendo solo un contenitore.
in quest’articolo prendiamo e vediamo come utilizzare due plugin:
<ul>
<li> <strong>Watch</strong> https://github.com/sindresorhus/grunt-sass</li>
<li><strong>scss</strong> https://github.com/gruntjs/grunt-contrib-watch</li>
</ul>

di norma, sempre se non specificato all’interno della documentazione del plugin, per installarlo, è necessario andare nella cartella del progetto tramite terminale, e digitare:
{% highlight javascript %}
npm install grunt-contrib-sass —save-dev 
{% endhighlight %}

(nel caso del plugin sass)
a questo punto il plugin è installato (puoi verificarlo sempre dalla cartella node_modules e da package.json che nel frattempo si è aggiornato).

Quel che resta da fare è andare nel <strong>Gruntfile.js</strong> ed impostarlo (come è indicato nella documentazione). 
Per il plugin appena installato, Ad esempio, aggiungiamo:
{% highlight javascript %}
module.exports = function(grunt) {
  grunt.initConfig({
    sass: {                              
        dist: {                            
          options: {                       
            style: 'expanded'
          },
          files: {                         
            './css/style.css': './sass/style.scss',       
          }
        }
      }, 
    });
      grunt.loadNpmTasks('grunt-contrib-sass');
};
{% endhighlight %}

in poche parole abbiamo detto a grunt che, se scriviamo sul terminale il task <strong>“grunt sass”</strong> lui prenderà il file sass, lo processerà facendolo diventare un file css, nelle cartelle che abbiamo inserito come impostazione.
salvandolo il file, il nostro primo task è terminato.
per vederlo in funzione, è necessario scrivere nel terminale, all’interno della cartella del progetto, <strong>“grunt sass”</strong> (dove sass è una label modificabile a vostro piacimento, io per abitudine metto come nome task il nome del plugin) e vedere che automaticamente, il file sass che gli abbiamo dato in pasto, viene processato e viene creato il file css nella cartella che abbiamo indicato.

molto semplice, non è vero?

il secondo plugin che voglio mostrarvi è <strong>Watch</strong>, un plugin che permette di scrivere il codice senza preoccuparsi del terminale, visto che genera automaticamente il file css.

Come dice il nome, il plugin watch “guarda” il file Scss in attesa di modifiche, e quando le trova, aggancia il task descritto in precedenza (sass) e lo lancia automaticamente.

possiamo inserire questo task molto facilmente, come nell’esempio sotto:
{% highlight javascript %}
watch: {
        sass: {
          files: ['sass/*.scss'],
          tasks: ['sass'],
        }
      } 
{% endhighlight %}

se prendiamo il file grunt.js nella sua totalità, avremo un file così generato:
{% highlight javascript %}
module.exports = function(grunt) {
  grunt.initConfig({
    sass: {                              
        dist: {                            
          options: {                       
            style: 'expanded'
          },
          files: {                         
            './css/style.css': './sass/style.scss',       
          }
        }
      },  
      watch: {
        sass: {
          files: ['sass/*.scss'],
          tasks: ['sass'],
        }
      } 
    });
      grunt.loadNpmTasks('grunt-contrib-sass');
      grunt.loadNpmTasks('grunt-contrib-watch');
};
{% endhighlight %}

come vedete, come primo task abbiamo inserito Sass, che genera il file scss, e come secondo task, abbiamo inserito il plugin Watch,  così, se opportunamente richiamato, gestisce lui tutta la procedura di process dei file Scss.

e questo è tutto!
dalla barriera insormontabile, è diventato uno strumento utile per velocizzare il nostro lavoro.
e voi utilizzate Grunt? che plugin e task utilizzate?

