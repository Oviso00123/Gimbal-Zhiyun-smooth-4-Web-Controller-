What is this?

A web page (one file: index.html) that talks to the Smooth 4 over Bluetooth. It shows an on-screen joystick: move your finger and the gimbal moves. You can also use the keyboard (arrow keys or WASD) or a gamepad.

It was made for this setup: a phone is mounted on the gimbal for video calls, and you steer the gimbal from another device (PC or a second phone).

What you need
A Zhiyun Smooth 4 gimbal, turned on
Chrome or Edge (PC or Android). Safari and Firefox do not work: they have no Bluetooth for web pages
The Zhiyun app closed: the gimbal accepts only one connection at a time
The page opened from an https:// address (for example GitHub Pages) or from http://localhost
How to use it (5 steps)
Open the page: https://YOUR-USERNAME.github.io/smooth4-web-controller/
Press Connetti (Connect) and pick your gimbal from the list
Set a low speed for your first tests
Move the knob (or use the arrow keys). When you let go, the gimbal stops
If in doubt, press STOP (or the space bar)

💡 If it won't connect, type the service UUID in the field. The page tries fee9 by itself, which other Zhiyun gimbals use. The interface is in Italian for now. Translations are welcome!

What works and what doesn't (the honest truth)
Feature	Status
Bluetooth connection and joystick	✅ Tested on the author's Smooth 4
Message error check (CRC)	✅ Verified on 5 real messages
Up/down movement (tilt)	✅ Confirmed by Karim Jedda's article
Left/right (pan) and rotation (roll)	🟡 Very likely, please check on your gimbal
Battery, version, position, modes (PF, L, F, POV, GO)	🟡 Taken from another model (Weebill-S). Not verified on the Smooth 4
Saved positions, face tracking, gestures, voice commands	💡 Ideas for the future, not built yet

The page has a Laboratory and a Commands panel so you can test things yourself. Watch what the Log shows.

How it works (in simple words)

The gimbal receives small Bluetooth messages. Each message says "move this axis with this strength". The strength is a number from 0 to 4095, where 2048 means stopped.

The gimbal only moves while messages keep arriving. That's why the page repeats them every 0.07 seconds while you hold the knob. Technical details are in PROTOCOL.md.

Want to help? 🙌

Do you own a Smooth 4 (or another Zhiyun gimbal)?

Open the page and try the buttons in the Commands panel
Copy what you see in the Log
Open an Issue here on GitHub with your gimbal model and what happened

Even one line helps us complete the command table.

🙏 Thanks and credits

This project exists thanks to other people's work.

Karim Jedda: heartfelt thanks! His article Controlling a Zhiyun Smooth 4 gimbal with Raspberry Pi made this work much easier. The real Smooth 4 messages, the Bluetooth addresses and the starting idea come from there.
Peter Maguire: his study of the Weebill-S gimbal (article and bleebil repository) explains how Zhiyun messages are built, the fee9 service and many commands.
The code was written with the help of an AI assistant (Claude, by Anthropic).
Disclaimer

Use everything at your own risk. Do your first tests at low speed with the gimbal still and unobstructed. The names Zhiyun and Smooth belong to their owners.

#Italiano
Cos'è questo progetto?

È una pagina web (un solo file: index.html) che parla con lo Smooth 4 tramite Bluetooth. Ti mostra un joystick sullo schermo: muovi il dito e il gimbal si muove. Puoi usare anche la tastiera (frecce o WASD) o un gamepad.

L'idea nasce da qui: il telefono è montato sul gimbal per fare videochiamate, e il gimbal si comanda da un altro dispositivo (PC o secondo telefono).

Cosa ti serve
Un gimbal Zhiyun Smooth 4, acceso
Chrome o Edge (PC o Android). Safari e Firefox non funzionano: non hanno il Bluetooth per le pagine web
L'app Zhiyun chiusa: il gimbal accetta un solo collegamento alla volta
La pagina aperta da un indirizzo https:// (per esempio GitHub Pages) oppure da http://localhost
Come si usa (5 passi)
Apri la pagina: https://TUO-UTENTE.github.io/smooth4-web-controller/
Premi Connetti e scegli il tuo gimbal dalla lista
Metti la velocità bassa per i primi test
Muovi il pallino (o usa le frecce). Quando lasci, il gimbal si ferma
In caso di dubbio premi STOP (o la barra spaziatrice)

💡 Se non si collega, scrivi l'UUID del service nel campo apposito. La pagina prova da sola fee9, che è quello usato da altri gimbal Zhiyun.

Cosa funziona e cosa no (la verità)
Funzione	Stato
Collegamento Bluetooth e joystick	✅ Provato sullo Smooth 4 dell'autore
Controllo errori dei messaggi (CRC)	✅ Verificato su 5 messaggi reali
Movimento su/giù (tilt)	✅ Confermato dall'articolo di Karim Jedda
Destra/sinistra (pan) e rotazione (roll)	🟡 Molto probabile, da verificare sul tuo gimbal
Batteria, versione, posizione, modi (PF, L, F, POV, GO)	🟡 Presi da un altro modello (Weebill-S). Non verificati sullo Smooth 4
Posizioni salvate, inseguimento del volto, gesti, comandi vocali	💡 Idee per il futuro, non ancora fatte

La pagina ha un Laboratorio e un pannello Comandi per provare le cose da soli. Guarda cosa risponde il Registro.

Come funziona (spiegato semplice)

Il gimbal riceve piccoli messaggi Bluetooth. Ogni messaggio dice: "muovi questo asse con questa forza". La forza è un numero da 0 a 4095, dove 2048 significa fermo.

Il gimbal si muove solo finché i messaggi continuano ad arrivare. Per questo la pagina li ripete ogni 0,07 secondi finché tieni premuto. I dettagli tecnici sono in PROTOCOL.md.

Vuoi aiutare? 🙌

Hai uno Smooth 4 (o un altro gimbal Zhiyun)?

Apri la pagina e prova i pulsanti del pannello Comandi
Copia quello che vedi nel Registro
Apri una Issue qui su GitHub e scrivi il modello del gimbal e cosa è successo

Anche una sola riga aiuta a completare la tabella dei comandi.

🙏 Ringraziamenti

Questo progetto esiste grazie al lavoro di altre persone.

Karim Jedda: grazie di cuore! Il suo articolo Controlling a Zhiyun Smooth 4 gimbal with Raspberry Pi ha semplificato moltissimo il lavoro. Da lì arrivano i messaggi reali dello Smooth 4, gli indirizzi Bluetooth e l'idea di partenza.
Peter Maguire: il suo studio sul gimbal Weebill-S (articolo e repository bleebil) spiega come sono fatti i messaggi della famiglia Zhiyun, il service fee9 e molti comandi.
Il codice è stato scritto con l'aiuto di un assistente IA (Claude, di Anthropic).
Avviso

Usa tutto a tuo rischio. Fai i primi test con velocità bassa e con il gimbal fermo e libero. I nomi Zhiyun e Smooth appartengono ai loro proprietari.
