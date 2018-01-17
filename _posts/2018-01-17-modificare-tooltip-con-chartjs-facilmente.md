---
layout: post
title: Chartjs, come modificare facilmente i tooltip
subtitle: 
bigimg: /img/chartjs.png
---

Se state leggendo questo articolo è perchè come me siete alle prese con <a href="http://www.chartjs.org/">Chart.js</a>.<br />
Questa famosa Libreria permette di creare dei grafici molto interessanti e funzionali in tempo breve, ma bisogna saperla utilizzare.<br />
Oggi vi mostro come creare dei tooltip custom.<br />
Avevo la necessità di rimuovere il valore della fetta del mio grafico a torta, e inserire solo la label, che conteneva delle informazioni che volevo visualizzare in grafico.

Sapendo che il grafico si crea con l'html:

{% highlight html %}
<canvas id="chart-ex" width="600" height="400"></canvas>
{% endhighlight %}

e inserendo questo js,

{% highlight javascript %}
new Chart(document.getElementById("chart-ex"), {
    "type": "doughnut",
    "data": {
        "labels": ["Red", "Blue", "Yellow"],
        "datasets": [{
            "label": "My First Dataset",
            "data": [300, 50, 100],
            "backgroundColor": ["rgb(255, 99, 132)",
                "rgb(54, 162, 235)",
                "rgb(255, 205, 86)"]
        }
        ]
    }
});
{% endhighlight %}

per modificare il valore printato del tooltip, è necessario aggiungere il valore <b>Option</b> al js, in questo modo:

{% highlight javascript %}
"options": {
        tooltips: {
            callbacks: {
                label: function(tooltipItem, data) {
                    var label = data.labels[tooltipItem.index];
                    return label;
                }
            }
        }
    }
{% endhighlight %}

così facendo, viene rimossa dalla visualizzazione il valore della fetta del grafico, facendo comparire solamente la label corrispondente.

E adesso tocca a voi, avete già utilizzato questa libreria? che ve ne pare?
