Cominciamo con il punto 1, per scaldarci. Dobbiamo far comparire uno `0` al centro della pagina, contornato da un bordo rotondo tratteggiato. Facile, no?

Partiamo impostando il nostro html:

<html>
  <head>
  </head>
  <body>
  </body>
</html>

Ora andiamo a inserire l'elemento che rappresenterà il nostro cronografo, e che conterrà lo `0` iniziale. Propongo un `div`, con un `id` per riferirvisi dopo...

```html
<html>
  <head>
  </head>
  <body>
    <div id="chrono">0</div>
  </body>
</html>
```

Ok, ora si tratta solo di aggiungere un po' di stile... mettiamo un tag `style` nell'elemento `head` del documento, e facciamo un po' make-up a `#chrono`:

```html
<html>
  <head>
    <style>
      #chrono {
        [qui ci andrà lo stile...]
      }
    </style>
  </head>
  <body>
    <div id="chrono">0</div>
  </body>
</html>
```

Cosa mettiamo nello `style`? Per prima cosa, dobbiamo posizionare il nostro `div` proprio in centro alla pagina:

```css
          position: absolute;
          left: 50%; top: 50%;
```

`position:absolute` dice che vogliamo posizionare il nostro elemento dove vogliamo noi, e non lasciarlo al flusso di posizionamento del browser. Quindi, subito dopo, le proprietà `left` e `top` ci dicono dove vogliamo posizionarlo, in termini di distanze rispettivamente dal lato sinistro e dal lato superiore della finestra. Le percentuali si riferiscono alla dimensione della finestra (esperimento: provate a vedere cosa succede ridimensionando la finestra...)

I più raffinati tra di voi noteranno che il div non è _perfettamente_ centrato. Questo perché la posizione che abbiamo specificato si riferisce non al centro del div, ma al suo angolo superiore sinistro. Se vogliamo davvero fare i precisi (come è richiesto dal testo, peraltro) possiamo dare l'ultima pennellata così:

```css
    position: absolute;
    left: 50%; top: 50%;
    transform: translate(-50%,-50%);
```

In questo modo, trasliamo `#chrono` verso sinistra e verso l'alto di una quantità pari alla metà della sua larghezza e altezza. Ora siamo proprio in centro!

Ora mettiamoci il bordo:

```css
    [...]
    border: 1px black dashed;
    border-radius: 50%;
```

Così abbiamo messo il bordo, ma l'effetto non è carino: il bordo è stretto attorno allo `0`. Inoltre, quando lo `0` cambierà, il bordo si adeguerà. Noi vorremmo qualcosa di più simile ad una cornice. Possiamo ottenere questo effetto settando esplicitamente le dimensioni del nostro elemento:

```css
    height: 100px; width: 100px;
``` 

Poi ci servono un altro paio di cosette, per posizionare il testo al centro del bordo dell'elemento: 

```css
    line-height: 100px;
    text-align: center;
```

Esercizio: cerca su internet a cosa servono queste due proprietà, e prova a spiegarti perché centrano il testo in mezzo (soprattutto `line-height`)

Ecco il codice che risolve completamente il primo punto:

```html
<html>
  <head>
    <script src="jquery.min.js"></script>
    <style>
    #chrono {
        font-family: Arial;
        font-size: 30px;
        border: 1px black dashed;
        border-radius: 50%;
        height: 100px; width: 100px;
        position: absolute;
        line-height: 100px;
        text-align: center;
        left: 50%; top: 50%;
        transform: translate(-50%,-50%);
    }
    </style>
  </head>
  <body>
    <div id="chrono">0</div>
  </body>
</html>
```


Ok, ora passiamo al punto 2: dobbiamo fare in modo che quado clicchiamo sul nostro "cronografo", questo cominci ad incrementare il suo valore, di un'unità al secondo.

Qui, l'avrete intuito, serve qualche incantesimo javascript!

Partiamo per gradi, per riscaldarci: facciamo in modo che, cliccando sul cronografo, questo passi da `0` a `1`.

Per prima cosa, specifichiamo che vogliamo caricare `jquery`. Immaginiamo di avere messo il file di `jquery` nella stessa cartella del file `html`, e di averlo chiamato `jquery.min.js`. Per caricarlo, ci basta specificare, nell'elemento `head`: 

```html
    <script src="jquery.min.js"></script>
```

Ora prepariamo l'incantesimo per scrivere `1` nel cronografo. Chiamiamolo già `conta`, in vista del fatto che sarà la funzione che chiameremo per far partire il conteggio. Mettiamo la definizione della funzione in fondo al `body`

```html
    <body>
    [...]
        <script>
        function conta() {
            $("#chrono").text(1)
        }
    </script>
    </body>
```

La funzione `conta` usa l'operatore `$` per individuare l'elemento con `id` uguale a `chrono`, e per cambiargli il testo con il _metodo_ `text`

Ora colleghiamo il click su `#chrono` all'invocazione della funzione `conta`:

```javascript
    $("#chrono").on("click", conta)
```

Provate: ora cliccando sul cronografo il valore dovrebbe cambiare in `1`

Bene, ora che ci siamo riscaldati, possiamo provare a risolvere il punto 2: fare in modo che il cronografo aumenti di 1 ogni secondo.

Un possibile ragionamento potrebbe essere: definiamo una _variabile_ con la quale tenere il conto:

```javascript
    var t = 0;
```

Quindi, facciamo in modo che `conta` incrementi la variabile, e poi aggiorni il testo di `#chrono`:

```javascript
function conta() {
    t = t + 1;
    $("#chrono").text(t)
}

```

Provate a fare così: caricate la pagina, aprite la console, e invocate la funzione `conta()`. Dovreste vedere che il valore si incrementa.
Questo però non basta, ovviamente! Vogliamo che la funzione venga invocata ogni secondo! Usiamo allora un trucco che abbiamo già visto: `conta` si richiama dopo un secondo, usando `setTimeout`:

```javascript
function conta() {
    t = t + 1;
    $("#chrono").text(t)
    setTimeout(conta, 1000)
}

```

Ora, per vedere se tutto funziona, invochiamo la funzione `conta` alla fine dello script, e clicchiamo sul cronografo. Dovremmo vedere il cronografo contare, e ci siamo portati a casa anche questi due ghiotti punti!
