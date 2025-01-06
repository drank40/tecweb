# CAPITOLO 0: INTRODUZIONE

## WORLD WIDE WEB:
Creato per la condivisione di documenti in forma ipertestuale. L'ipertesto è una rete di documenti (pagine) che in modo unidirezionale permettono di saltare (navigare) tramite collegamenti (link) ad altri documenti. Questi documenti contengono risorse di tutti i tipi.

L'idea geniale di Berners Lee fu quella di fondere i concetti di ipertesto e di rete. Infatti, le pagine risiedono in server geograficamente distribuiti (world wide) che generano così una rete (web) tramite i collegamenti tra loro.

Per realizzare tutto ciò si ha bisogno di:

### **3 elementi concettuali:**
1. **Un meccanismo** per localizzare il documento.
2. **Un protocollo** per accedere al documento.
3. **Un linguaggio** per descrivere il documento (crearlo).

### **2 elementi fisici:**
1. **Server:** permette di erogare le risorse che costituiscono il documento.
2. **Client:** permette di rappresentare/visualizzare i documenti e navigare tra loro.

### Formula:
**WWW = URL + HTTP + HTML**

### Modello CLIENT / SERVER

- **Client** (ATTIVO: web browser)
  1. Usano URL per identificare le risorse.
  2. Usano HTTP per collegarsi al SERVER.
  3. Richiedono pagine web al server e ne visualizzano il contenuto.

- **Server** (PASSIVO: web HTTP server)
  1. Rimangono in ascolto di un eventuale client, usano HTTP per collegarsi e fornire informazioni.

---

# CAPITOLO 1: URI

### Come il client identifica il server e viceversa? Come il client riconosce la risorsa? Quale protocollo usare per accedere alla risorsa?

**URI** = Uniform Resource Identifier

### Definizione:
- **U = Uniforme:** gli URI rispettano una sintassi standard semplice e regolare.
  - Vantaggi:
    - Comune semantica.
    - Possibilità di usare nello stesso contesto diversi identificatori anche con protocolli di accesso diversi.
    - Facilità di introduzione di nuovi tipi di identificatori.

### Sintassi generale degli URI:
```
<scheme>:<scheme-specific-part>
```

Per esempio per gerarchie:
```
<scheme>://<authority><path>?<query>
```

### Tipi di URI:

1. **URN (Uniform Resource Name):**
   - Identifica una risorsa per mezzo di un nome che deve essere globalmente unico e restare valido anche se la risorsa diventa indisponibile o cessa di esistere. (es. codice ISBN).

2. **URL (Uniform Resource Locator):**
   - Identifica una risorsa per mezzo del meccanismo di accesso primario. Tipicamente il nome dello schema rappresenta il protocollo utilizzato, la parte rimanente dipende da esso.

### Struttura dell'URL:
```
<protocol>://[<username>:<password>@]<host>[:<port>][/<path>[?<query>][#fragment]]
```
- **Protocol:** descrive il protocollo da usare per accedere al server.
- **Username e Password:** credenziali per l'autenticazione.
- **Host:** indirizzo server su cui risiede la risorsa (IP logico o fisico).
- **Port:** definisce la porta da usare (se non indicato si usa la porta standard, 80 per HTTP).
- **Path:** percorso che identifica la risorsa nel file system del server.
- **Query:** stringa che consente di passare uno o più parametri (`par1=val&par2=val2`).

### URI OPACA:
- Non è soggetta a parsing.

### URI GERARCHICA:
- È soggetta a ulteriori operazioni di parsing (ad esempio per separare l'indirizzo del server dal percorso all'interno del file system).

#### Operazioni sugli URI gerarchici:
1. **Normalizzazione:** eliminazione di caratteri speciali ("." e "..").
2. **Risoluzione:** processo che a partire da un'URI originaria si ottiene un'URI risultante. La URI originaria viene risolta tramite una terza URI (base).
3. **Relativizzazione:** processo inverso della risoluzione.

---

# CAPITOLO 2: HTTP

### Definizione:
**HTTP (HyperText Transfer Protocol)**: protocollo di livello applicativo usato per trasferire risorse web da client/server.

### Caratteristiche principali:
- Gestisce sia le richieste URL del cliente sia le risposte del server.
- **Protocollo stateless:** né il server né il client mantengono informazioni relative ai messaggi precedentemente scambiati.

### Componenti principali:
1. **Client:** programma applicativo che stabilisce una connessione al fine di inviare delle richieste.
2. **Server:** programma applicativo che accetta connessioni al fine di ricevere richieste ed inviare specifiche risposte con le risorse richieste.
3. **Connessione:** circuito virtuale stabilito a livello di trasporto tra due applicazioni per comunicare.
4. **Messaggio:** unità base di comunicazione HTTP definita come una sequenza di byte concettualmente atomica (viene interpretato nella sua interezza).
   - **Request:** messaggio HTTP di richiesta.
   - **Response:** messaggio HTTP di risposta.

### Versioni di HTTP:
- **HTTP v1.0:** basato su TCP (non è persistente).
  - Processo: server in ascolto → cliente apre connessione su una porta → server accetta → cliente manda richiesta → server manda risposta e chiude connessione.

- **HTTP v1.1:** (persistente)
  - Possibile specificare coppie multiple di richiesta e risposta nella stessa connessione.
  - Il server lascia aperta la connessione dopo la risposta e può ricevere altre richieste. Chiude la connessione quando specificato dal messaggio o quando non usata per molto tempo (**timeout**).

- **HTTP v1.1 con pipelining:**
  - Il cliente può mandare più richieste ancor prima di aver ricevuto le risposte, mantenendo l'ordine tramite il protocollo TCP.

- **HTTP v2:**
  - Basato sul **multiplexing** e il protocollo **SPDY**.

### Messaggi HTTP:
1. **Header:** contiene le informazioni necessarie per identificazione del messaggio.
2. **Body:** contiene i dati trasportati dal messaggio.

I messaggi **Response** contengono i dati relativi alle risorse richieste dalla **Request**.

I dati sono codificati secondo il formato specificato dall'header (di solito **MIME**).

### Comandi della Request:
1. **GET:** chiede una risorsa ad un server, passando i parametri tramite URL.
2. **POST:** chiede una risorsa ad un server, passando i parametri nel body del messaggio.
3. **PUT:** chiede la memorizzazione sul server di una risorsa all'URL specificato.
4. **DELETE:** chiede la cancellazione di una risorsa riferita all'URL specificato.
5. **HEAD:** simile al GET ma il server risponde solo con gli header senza body.
6. **OPTIONS:** chiede informazioni sulle opzioni disponibili per la comunicazione.
7. **TRACE:** invoca il loop-back remoto a livello applicativo del messaggio di richiesta.

### Codici di stato:
1. **1xx:** Informational: risposta temporanea durante lo svolgimento della richiesta.
2. **2xx:** Successful: il server ha ricevuto, capito e accettato la richiesta.
3. **3xx:** Redirection: il server ha capito la richiesta ma sono necessarie ulteriori azioni.
4. **4xx:** Client Error: errore da parte del client.
5. **5xx:** Server Error: errore interno del server.

### Server concorrenti e multiprocesso:
- **Concorrenti:** gestiscono più richieste contemporaneamente.
- **Multiprocesso:** se arrivano più richieste sulla stessa porta le smistano a un thread diverso.

### Cookie:
Struttura dati che si muove tra client e server, possono essere generati da entrambi. Dopo la loro creazione vengono passati ad ogni richiesta e risposta. Hanno lo scopo di fornire supporto per il mantenimento dello stato (**HTTP stateless**).

#### Struttura di un cookie:
1. **Key:** identificatore.
2. **Value:** valore (max 255 caratteri).
3. **Path:** posizione nell'albero di un sito al quale è associato.
4. **Domain:** dominio in cui è stato generato.
5. **Max-Age:** numero di secondi di vita (scadenza di una sessione).
6. **Secure:** indica che vengono trasferiti solo se il protocollo è sicuro (HTTPS).
7. **Version:** versione del protocollo di gestione dei cookie.

#### Autenticazione:
1. **Tramite IP:** non funziona se l'IP non è pubblico o è assegnato dinamicamente (DHCP).
2. **HTTP Basic:** username e password vengono chieste dal server e inviate dal client in seguito (dati codificati).
3. **Form:** normalmente usando un POST.

#### Sicurezza:
1. **SSL:** Secure Sockets Layer.
2. **TLS:** Transport Layer Security, basato sulla crittografia a chiave pubblica (private key + public key + certificato / HTTPS). Gestisce confidenzialità, autenticità e integrità.

### Proxy, Gateway e Tunnel:
1. **Proxy:** programma applicativo che può agire sia come client che come server, al fine di effettuare richieste per conto di altri clienti. Interpreta e se necessario riscrive la request prima di inoltrarle.
2. **Gateway:** server che agisce da intermediario per altri server, riceve le request come se fosse il server originario.
3. **Tunnel:** programma applicativo che agisce tra due connessioni per garantire sicurezza e criptaggio.

### Caching:
La web cache memorizza i documenti che la attraversano così che, se necessario, vengano usate quelle risorse temporaneamente memorizzate nella cache nelle successive request.

1. **User Agent Cache:** il client contiene una cache delle pagine visitate dall'utente (URL mandati).
2. **Forward Proxy Cache:** copia della cache. Se ha già la pagina, non serve mandare la request al server e elabora direttamente la risposta come se fosse il server.

---

# CAPITOLO 5: WEB DINAMICO

## Web Statico
Modello basato sul concetto di ipertesto distribuito. L'insieme dei contenuti è prefissato staticamente. Modello semplice ma limitato.

## Web Dinamico
Il server richiede l'esecuzione dinamica di un'applicazione legata al contesto per interpretare una richiesta GET (URL con query).

### **CGI (Common Gateway Interface):**
Standard per interfacciare applicazioni esterne con web server. Il programma CGI viene eseguito dinamicamente in risposta alla chiamata e produce output che costituisce la risposta alla richiesta HTTP.

- **Client:** invia al server la richiesta di eseguire un programma CGI con alcuni parametri e dati in ingresso.
- **Server:** attraverso l'interfaccia standard CGI chiama il programma e passa i parametri.
- **CGI:** esegue le operazioni necessarie e manda al server i dati elaborati (che saranno mandati al client).

### Comunicazione fra Server e CGI:
1. **Variabili d'ambiente** del sistema operativo.
2. **Parametri sulla linea di comando:** il programma CGI viene lanciato in un processo pesante (shell di sistema operativo che interpreta i parametri passati con GET).
3. **Standard Input:** con il metodo POST.
4. **Standard Output:** per restituire al server la pagina HTML da mandare al client.

Il server, dall'URL della GET, deve capire che "nomeCGI" è un programma CGI. Tutti i programmi CGI devono essere in un'apposita directory, e il server deve essere configurato con il path specificato per i programmi CGI.

Ogni volta che viene invocata una CGI, viene creato un processo che viene distrutto alla fine dell'elaborazione.

### Limiti delle CGI:
Se si volessero aggiungere più programmi CGI con funzioni diverse ma tutte collegate allo stesso database, tutte dovrebbero implementare gli stessi dati, causando ripetizioni. Per ovviare a questo problema si potrebbe realizzare una sola CGI che implementa tutte le funzionalità, creando un'applicazione monolitica e perdendo i vantaggi della modularità.

---

### Application Server
La soluzione migliore è quella di realizzare un contenitore in cui far vivere le funzioni server-side. L'**application server** è un container che si preoccupa di fornire i servizi di cui le applicazioni hanno bisogno, tramite le funzioni server-side.

#### **Componente:**
Unità che svolge una funzione specifica ed interagisce con altre componenti. Non può eseguire senza il supporto del container.

- Funzioni riguardo il ciclo di vita, sistema di nomi, qualità del servizio e sicurezza.

#### Stato:
1. **Stateful:** esiste stato dell'interazione tra client e server.
2. **Stateless:** non si tiene traccia dello stato. Ogni messaggio è indipendente dagli altri. Non genera problemi solo se il protocollo applicativo è progettato con operazioni idempotenti (stessa risposta).

#### Tipi di stato:
1. **Stato di esecuzione:** insieme dei dati parziali di un'elaborazione.
2. **Stato di sessione:** insieme dei dati che caratterizzano un'interazione con uno specifico utente (definito da tempo di vita e accessibilità).
3. **Stato informativo persistente:** viene mantenuto in una struttura persistente come un database.

#### Sessione:
Lo stato associato ad una sequenza di pagine visualizzate da un utente, contiene tutte le informazioni necessarie durante l'esecuzione.

#### Conversazione:
Sequenza di pagine di senso compiuto (definito anche dalle interfacce di I/O utili per la comunicazione tra le pagine).

### Architettura 3-Tier:
1. **Database Server** →
2. **Application Server** →
3. **Web Server** (distribuzione verticale).

Ad ogni livello si può replicare il servizio su più macchine (distribuzione orizzontale).

### Replicazione:
1. **Database:** normalmente stateful, difficile da replicare perché si deve preservare l'atomicità.
2. **Applicazione:** stato di sessione prevalentemente. Alcuni framework permettono replicazione tramite tecniche di clustering (raggruppare i dati in gruppi con stesse caratteristiche).
3. **Web Server:** stateless per HTTP, quindi facilmente replicabile.

---

# CAPITOLO 6: SERVLET

## Architettura Java Enterprise Edition (JEE)
Il modello JEE si basa su un'architettura a componente-container. Un **JEE server** contiene due tipi di container:
1. **Web Container:** ospita componenti Servlet e JSP.
2. **EJB Container:** ospita Enterprise JavaBeans.

### Web Application
Una web application è un gruppo di risorse server-side che include:
- Classi Java
- JSP
- Risorse statiche (HTML, immagini, CSS)
- Componenti client-side (JavaScript)
- Informazioni di configurazione e deployment

### Accesso alla Web Application
1. Il client invia una richiesta GET o POST.
2. Il server mappa l'URL su una web application.
3. La web application elabora la risposta.
4. La risposta viene inviata al client.

### Servlet
Una **Servlet** è una classe Java che fornisce servizi ai client mediante protocolli di tipo request/response (solitamente HTTP). Le servlet:
- Generano contenuti dinamici.
- Estendono la classe `HttpServlet`.
- Sono eseguite in un web container.

Quando arriva una richiesta HTTP:
1. Il **Servlet Container** genera un oggetto `HttpServletRequest` e un oggetto `HttpServletResponse`.
2. Passa questi oggetti alla servlet.

#### Ciclo di vita della Servlet
1. Se non esiste un'istanza della servlet, il container la inizializza usando `init()`.
2. Viene invocato il metodo `service()`, che a sua volta chiama `doGet()` o `doPost()` in base al tipo di richiesta.
3. Quando il container decide di distruggere la servlet, viene invocato `destroy()`.

#### Servlet e Multithreading
Di default, una servlet ha un'istanza condivisa tra più thread. Ogni richiesta genera un nuovo thread, quindi è necessario gestire la concorrenza.

---

# CAPITOLO 7: JAVA SERVER PAGES (JSP)

Le **Java Server Pages (JSP)** sono pagine web che contengono codice Java incorporato all'interno di tag HTML.

### Funzionamento delle JSP
1. La parte HTML viene trascritta sullo stream di output.
2. Il codice Java viene eseguito sul server per generare contenuto dinamico.
3. Viene creata la pagina HTML risultante e inviata al client.

### Scriptlet e Direttive JSP
- **Scriptlet (`<% ... %>`):** permette di inserire codice Java direttamente nella pagina.
- **Dichiarazioni (`<%! ... %>`):** usate per dichiarare variabili e metodi.
- **Espressioni (`<%= ... %>`):** restituiscono il risultato di un'espressione Java.
- **Direttive (`<%@ ... %>`):** istruzioni per il compilatore JSP (es. importazione di classi).

### Oggetti impliciti
Le JSP forniscono alcuni oggetti impliciti che possono essere usati direttamente senza dichiarazione:
1. `request`
2. `response`
3. `session`
4. `application`
5. `out`
6. `config`
7. `pageContext`

---

# CAPITOLO 8: JAVASCRIPT

**JavaScript** è un linguaggio di scripting interpretato lato client. Permette di:
- Aggiungere interattività alle pagine web.
- Modificare dinamicamente il DOM.
- Gestire eventi utente (es. click, input).

### Caratteristiche di JavaScript
1. **Interpretato:** eseguito direttamente dal browser senza compilazione.
2. **Debolmente tipizzato:** le variabili possono cambiare tipo dinamicamente.
3. **Orientato agli oggetti:** supporta oggetti, ma non classi come in Java.

### Inclusione di JavaScript
1. **Interno:**
   ```html
   <script>
     // codice JavaScript
   </script>
   ```
2. **Esterno:**
   ```html
   <script src="script.js"></script>
   ```

### Eventi
JavaScript può gestire eventi tramite funzioni di callback associate agli elementi del DOM.

Esempio:
```html
<button onclick="saluta()">Cliccami</button>
<script>
  function saluta() {
    alert('Ciao!');
  }
</script>
```

---

# CAPITOLO 9: AJAX

**AJAX (Asynchronous JavaScript And XML)** permette di:
- Aggiornare parti di una pagina web senza ricaricare l'intera pagina.
- Effettuare richieste asincrone al server.

### Oggetto `XMLHttpRequest`
È l'oggetto principale per effettuare richieste AJAX.

Esempio:
```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', 'pagina.html', true);
xhr.onload = function() {
  if (xhr.status === 200) {
    document.getElementById('risultato').innerHTML = xhr.responseText;
  }
};
xhr.send();
```

---

# CAPITOLO 10: FRAMEWORK REACT

**React** è una libreria JavaScript per la creazione di interfacce utente.

### Virtual DOM
React utilizza un **Virtual DOM**, una copia del DOM reale, per ottimizzare il rendering delle pagine. Solo le modifiche vengono applicate al DOM reale.

### Componenti
React permette di suddividere l'interfaccia in **componenti**, ognuno dei quali rappresenta una parte autonoma della UI.

Esempio di componente:
```javascript
function Benvenuto(props) {
  return <h1>Ciao, {props.nome}</h1>;
}
```

---

# CAPITOLO 11: NODE.JS

**Node.js** è un ambiente di runtime per JavaScript lato server.

### Caratteristiche principali
1. **Event-driven:** basato su un modello asincrono e orientato agli eventi.
2. **Single-threaded:** utilizza un singolo thread per gestire molte connessioni contemporaneamente.

### Esempio di server HTTP con Node.js
```javascript
const http = require('http');
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Ciao dal server Node.js!');
});
server.listen(3000, () => {
  console.log('Server in ascolto su porta 3000');
});
```

---

# CAPITOLO 12: JSF E WEBSOCKET

## JSF (Java Server Faces)
**JSF** è un framework Java per la creazione di applicazioni web basate su componenti.

### Ciclo di vita JSF
1. Viene creata la richiesta.
2. Viene costruito l'albero dei componenti.
3. Viene elaborata la richiesta e aggiornato il modello.
4. Viene resa la risposta al client.

## WebSocket
**WebSocket** è una tecnologia che permette una comunicazione bidirezionale full-duplex tra client e server su una singola connessione TCP.

### Esempio di utilizzo WebSocket
**Client-side:**
```javascript
let socket = new WebSocket('ws://localhost:8080');
socket.onmessage = function(event) {
  console.log('Messaggio dal server:', event.data);
};
socket.send('Ciao server!');
```

**Server-side (Java):**
```java
@ServerEndpoint("/websocket")
public class MyWebSocket {
  @OnMessage
  public void onMessage(String message, Session session) {
    System.out.println("Messaggio ricevuto: " + message);
    session.getBasicRemote().sendText("Risposta dal server");
  }
}
```

---
