<meta charset="utf-8"/>

# Interattività

Oggi consideriamo uno dei poteri magici per antonomasia: far sparire e riapparire le cose. Se lo fa David Copperfield con la statua della libertà (https://www.youtube.com/watch?v=823GNH4Rczg), non c'è ragione perché non lo possiamo fare noi con gli elementi della nostra pagina!

Ricordiamo alcuni passi nella visualizzazione di una pagina:

1. l'html della pagina viene ricevuta dalla rete, oppure viene letto da un file
2. il contenuto dell'html viene convertito in una _struttura dati_ che il browser tiene in memoria
3. se noi modifichiamo questa struttura dati, il browser subito ridisegna la pagina, adeguandola ai cambiamenti
4. abbiamo almeno due modi per modificare la struttura:
    - in Chrome: aprire la console, andare su "Elements", modificare quello che vogliamo
    - lanciare "incantesimi" tramite il DOM (Document Object Model)

Cos'è il *DOM*? &Egrave; l'interfaccia con cui possiamo cambiare la struttura della pagina utilizzando *javascript*!

Qui consideriamo la libreria `jQuery`, che addolcisce un po' l'interfaccia... (approfondire)

Vediamo subito come, con `jQuery`, si possa operare l'incantesimo della sparizione.

Consideriamo il codice di questa pagina:

```html
    <!DOCTYPE html >
    <html>
      <head>
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
      </head>
      <body>
        <h1 id="titolo">Fammi sparire!</div>
      </body>
    </html>
```

Mettiamolo in un file, apriamo il file con un browser (qui facciamo riferimento a Chrome), apriamo la console, e scriviamo il comando:

    $("#titolo").fadeOut() 

Et voilà! Il titolo è sparito!
Analizziamo la struttura del nostro incantesimo: `$` vuol dire che vogliamo usare l'oggetto `jQuery`. Segue una stringa tra parentesi `("#titolo")`. Questa è una cosa che abbiamo già visto: è un _selettore_, esattamente come quelli del `css`! Ci serve per stabilire su quali elementi vogliamo lanciare l'incantesimo...
Poi segue, dopo il punto (`.`), il nome dell'incantesimo. In questo caso il nostro si chiama `fadeOut`, e serve a far sparire le cose!
Per finire, ci sono le parentesi `()`, che _invocano_ (si dice proprio così, anche tecnicamente) l'incantesimo.

Quali altri incantesimi possiamo lanciare?
Beh, ovviamente ce ne dovrà essere uno che fa _ricomparire_ il nostro titolo! Provate a digitare, nella console:

    `$("#titolo").fadeIn()`

Ecco fatto! Il titolo è riapparso!

Altri incantesimi notevoli sono:

    - `.css`: consente di variare proprietà `css` di un elemento
    - `.text`: consente di variare il testo di un elemento
    [...]

Un termine tecnico per un incantesimo che posso lanciare su un certo oggetto è _metodo_ dell'oggetto.

Ok, lanciare incantesimi dalla console può essere divertente (oppure no), ma il punto non è tutto qui, ovviamente...

Per esempio, potremmo (dovremmo?) chiederci: come faccio a far sparire un oggetto quando l'utente compie una certa azione?
Tipo: come faccio a far sparire un oggetto quando l'utente clicca un pulsante, o un altro oggetto della pagina?

Per scoprire come fare, dobbiamo prima imparare a _definire_ dei nuovi incantesimi!
Vi faccio vedere come si fa con un esempio:

```javascript
    function scompareturTitulum() {
        $("#titolo").fadeOut()
    }
```

La parola magica (detta anche _parola chiave_) `function` vuol dire che stiamo definendo un nuovo incantesimo. 

Segue poi il nome dell'incantesimo, e le parentesi `()`. Quindi, tra parentesi graffe `{}`, troviamo la lista delle cose da fare per lanciare il nostro incantesimo. In questo caso, _invochiamo_ l'incantesimo `fadeOut` sull'oggetto selezionato con `$("#titolo")`.

Ora che abbiamo definito questo incantesimo cosa possiamo farci? Beh, possiamo invocarlo:

```javascript
    scompareturTitulum()
```

Ed ecco scomparire di nuovo il nostro titolo...

Ma a cosa ci può mai servire definire un incantesimo? Ci sono molte ragioni nobilissime. Quella di cui ci occupiamo qui è che questo ci consente lanciare incantesimi _quando si verifica qualche evento_.

Per esempio, possiamo aggiungere un altro elemento alla nostra pagina, e volere che, quando ci clicchiamo sopra, il titolo sparisca. 
Modifichiamo l'`html` della nostra pagina in questo modo:

```html
CTYPE html >
<html>
  <head>
    <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
    <style>
      #pulsante {
          display: inline-block;
          border: 1px solid black;
          padding: 3px;
      }
      
    </style>
  </head>
  <body>
    <div id="titolo">Fammi sparire!</div>
    <div id="pulsante">Clicca qui!</div>
  </body>
</html>

```

Per fare questo, in javascript si usa un altro tipo di incantesimo (un *meta*incantesimo!), chiamato _event handling_, che recita più o meno così: "Quando qualcuno clicca sull'elemento `#pulsante`, lancia l'incantesimo `scompareturTitulum`". La formula è questa:

```
    $("#pulsante").on("click", scompareturTitulum)
```

Provate a digitarla nella console (attenzione: dovete avere prima definito la funzione `scompareturTitulum`, come descritto sopra), e poi a cliccare sul "pulsante". Visto? Scomparsum est!

Vediamo megli come è fatto questo (*meta*)incantesimo: la parte di selezione già la conosciamo (`$("#pulsante")`), e vuol dire che stiamo lanciando un incantesimo all'elemento con `id="pulsante"`. Poi viene il nome: `on`, che in inglese vuol dire, tra l'altro, _quando_ . Poi questo incantesimo prende delle informazioni, tra le parentesi: `"click"` e `scompareturTitulum`. La prima, `"click"`, è una stringa (si notino le virgolette) che indica il particolare evento da considerare. In questo caso, come è intuitivo, l'evento è il click sull'elemento considerato. La seconda informazione è invece l'incantesimo che vogliamo lanciare quando si verifica l'evento, rappresentato qui dal suo nome (attenzione, _senza_ le virgolette).

Ora abbiamo però un'esigenza ulteriore: vogliamo incorporare questi incantesimi all'interno della pagina html... cioè, vogliamo ovviamente che l'utente non debba aprire la console e digitare gli incantesimi quando carica la nostra pagina. Ebbene, niente di più semplice! Mettiamo tutti gli incantesimi in un tag speciale, `<script>`, alla fine della pagina:

```html
	<!DOCTYPE html >
	<html>
	  <head>
		<script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
		<style>
		  #pulsante {
			  display: inline-block;
			  border: 1px solid black;
			  padding: 3px;
		  }
		  
		</style>
	  </head>
	  <body>
		<div id="titolo">Fammi sparire!</div>
		<div id="pulsante">Clicca qui!</div>
	  </body>
	  <script>
		function scompareturTitulum() {
			$("#titolo").fadeOut()
		}
		$("#pulsante").on("click", scompareturTitulum)
	  </script>
	</html> 
```
