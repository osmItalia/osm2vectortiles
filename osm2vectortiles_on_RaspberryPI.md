##OSM2vectortiles su RaspberryPI

Il sistema e' stato testato su un RPI 2 model B e ha dato ottimi risultati.  
Il sistema operativo utilizzato e' raspbian 8 versione maggio 2016 (quella disponibile a settembre 2016).

La sd-card e' una  "SanDisk Ultra 8GB MicroSD HC I classe 10" ma il sistema accetta anche le "SanDisk Extreme MicroSD HC I U3" che sicuramente sono piu' veloci.

Innanzitutto bisogna provvedere a recuperare l'immagine del sistema operativo  
https://www.raspberrypi.org/downloads/raspbian/

E' sufficiente la versione lite

Per installare il sistema operativo sulla sd-card, da linux:  
In questo caso la sd-card viene vista come /dev/sdb

>  
> \#raspbian 8  
> sudo dd bs=4M if=2016-05-27-raspbian-jessie-lite.img of=/dev/sdb;  
> sudo sync;  
>  

>user:pwd - pi:raspberry

>

Una volta lanciato il sistema operativo e' bene provvedere ad una sua minimale configurazione a ad un'aggiornamento.

> sudo locale-gen "it_IT.UTF-8";  
> sudo dpkg-reconfigure locales;  
> sudo apt-get update;  
> sudo apt-get upgrade;  

Si puo' quindi procedere all'installazione dei componenti necessari per il funzionamento del tiles server

> curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -  
> sudo apt-get install -y nodejs  

Verifica della corretta installazione  

> node -v  
> npm -v  
> npm config get prefix  

Per evitare conflitti e problemi di permessi e' meglio crearsi una sotto-cartella specifica  

> mkdir ~/.npm-global;  
> npm config set prefix '~/.npm-global';  
> export PATH=~/.npm-global/bin:$PATH;  
> source ~/.profile;  

> npm config get prefix;

> npm install npm -g

Come test viene provata l'installazione di non so ben cosa (esempio riportato sul sito di node!)  
> \# test  
> npm install -g jshint;  

Di sqlite3 per il momento non credo esista una versione armhf, per cui RPI se lo deve ricompilare; a parte qualche warning tutto dovrebbe procedere senza problemi  
> \# sqlite3  
> npm install -g sqlite3;  

Ed infine si puo' procedere all'installazione vera e propria del tiles server  
> \# installa il tiles server  
> npm install -g tileserver-gl-light  

Si puo procedere ora al download del file contenente i tiles, direttamente dal sito http://osm2vectortiles.org/downloads/ si puo' scegliere tra planet/Country/City (sconsiglio il planet viste le dimensioni!!)  

Come esempio: citta' di Bologna  
> curl -o bologna_italy.mbtiles https://osm2vectortiles-downloads.os.zhdk.cloud.switch.ch/v2.0/extracts/bologna_italy.mbtiles  

Oppure tutta Italia  
> curl -o italy.mbtiles https://osm2vectortiles-downloads.os.zhdk.cloud.switch.ch/v2.0/extracts/italy.mbtiles  

Per lanciare il server:  
>   
> tileserver-gl-light bologna_italy.mbtiles  

oppure  
> tileserver-gl-light italy.mbtiles  

Si puo' inserire direttamente nel file /etc/rc.local il comando che esporta il path e lancia in automatico il server  
>\## rc.local  
>export PATH=/home/pi/.npm-global/bin:$PATH;  
>tileserver-gl-light /home/pi/bologna_italy.mbtiles &  

A questo punto il server e' raggiungibile all'indirizzo del Raspberry sulla porta 8080:  
> http://IP_RPI:8080

##Thats all !!

###P.S.  
Gli aggiornamenti sul sito http://osm2vectortiles.org/downloads/ non sembrano molto frequenti ad oggi (22 settembre) risultano ancora i file relativi al 20 giugno. Se vi e' la necessita' di aggiornare di frequente il tiles-server sicuramente bisogna provvedere a realizzare un server di generazione delle tiles.  


