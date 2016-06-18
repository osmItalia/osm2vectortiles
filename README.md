# osm2vectortiles
Come installare una versione funzionante di *osm2vectortiles* su Ubuntu Server 14.04 LTS.

La documentazione del progetto *osm2vectortiles* si trova al link [http://osm2vectortiles.org/docs/](http://osm2vectortiles.org/docs/)

## Installazione

### Prerequisiti

Installare `node` e `npm` col comando

    sudo apt-get install nodejs npm

### Installare `tileserver-gl-light`

Installare `tileserver-gl-light` con `npm`

    sudo npm install -g tileserver-gl-light

### Scaricare i dati della mappa

Per scaricare i dati della mappa occorre andare al link [http://osm2vectortiles.org/downloads/](http://osm2vectortiles.org/downloads/) e selezionare i dati che si vogliono poi visualizzare.

Una volta selezionati si scaricano con un comando del tipo:

    curl -o italy.mbtiles https://osm2vectortiles-downloads.os.zhdk.cloud.switch.ch/v2.0/extracts/italy.mbtiles

### Lanciare il tileserver

Per far partire il server delle tile vettoriali è sufficiente lanciare il seguente comando:

    tileserver-gl-light italy.mbtiles

### Scegliere il tipo di mappa

Per scegliere il tipo di mappa bisogna collegarsi al sito http://localhost:8080/ (o quale sia il dominio a cui è raggiungibile il server).

![Schermata della scelta del tipo di mappa](http://osm2vectortiles.org/media/tileserver-gl-light.png)

### Personalizzare l'HTML

L'HTMl di esempio è già funzionante così come viene fornito purché il server delle tile rimanga acceso e fornisca quindi le tile vettoriali.

In caso si vogliano implementare ulteriori personalizzazioni si può fare riferimento alla documentazione di [Mapbox GL](https://www.mapbox.com/mapbox-gl-js/api/).

## Risoluzione dei problemi

### Dipendenze non del tutto soddisfatte

Può capitare le dipendenze dei pacchetit siano rotte. Per controllare bisogna lanciare il comando 

    npm -g list

e controllare che non ci siano pacchetti marcati come `missing`. Ad esempio

    npm ERR! missing: mkdirp@~0.5.0, required by node-pre-gyp@0.6.28
    npm ERR! missing: nopt@~3.0.1, required by node-pre-gyp@0.6.28
    npm ERR! missing: npmlog@~2.0.0, required by node-pre-gyp@0.6.28
    npm ERR! missing: rc@~1.1.0, required by node-pre-gyp@0.6.28
    npm ERR! missing: request@2.x, required by node-pre-gyp@0.6.28
    npm ERR! missing: rimraf@~2.5.0, required by node-pre-gyp@0.6.28
    npm ERR! missing: semver@~5.1.0, required by node-pre-gyp@0.6.28
    npm ERR! missing: tar@~2.2.0, required by node-pre-gyp@0.6.28
    npm ERR! missing: tar-pack@~3.1.0, required by node-pre-gyp@0.6.28

In caso siano presenti dipendenze non soddisfatte bisogna installare i singoli pacchetti mancanti con un comando del tipo

    sudo npm install -g nome_pacchetto@0.1.2

dove `0.1.2` è la versione richiesta indicata nella lista di cui sopra. Ad esempio

    sudo npm install -g mkdirp@~0.5.0

Una votla soddisfatte tutte le dipendenze bisogna rilanciare l'installazione di `tileserver-gl-light` con il comando

    sudo npm install -g tileserver-gl-light

### Il comendo `node` non è presente

Se si incappa nell'errore

> /usr/bin/env: node: No such file or directory

il comando `node` non è presente nel sistema.

Il comando `node` non è un vero e proprio comando, ma un link simbolico al comando `nodejs`. Se il link simbolico non fosse stato creato in fase di installazione di NodeJS basta crearo col comando

    sudo ln -s /usr/bin/nodejs /usr/bin/node

### `tileserver-gl-light` restituisce `TypeError`

Se si incappa nell'errore

> TypeError: Object function Object() { [native code] } has no method 'assign'
    at /usr/lib/node_modules/tileserver-gl-light/src/serve_data.js:28:14

come segnalato in questa issue: https://github.com/osm2vectortiles/osm2vectortiles/issues/346 bisogna installare `object-assign`

    sudo npm install -g object-assign

e aggiungere la riga di codice

    Object.assign = require('object-assign')

al file `src/serve_data.js`

### `Error: listen EADDRINUSE`

Se si incappa in questo errore probabilmente la porta `8080` è già in uso sul server. Per cambiare la porta usata da `tilserver-gl-light` basta lanciare il comando con l'argomento `-p`. Ad esempio

    tileserver-gl-light italy.mbtiles -p 8200

