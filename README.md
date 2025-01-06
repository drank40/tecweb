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


# CAPITOLO 6: SERVLET

## [Architettura Java Enterprise Edition JEE]
Modello **a componente-container**.  
Il **J2EE** o **JEE server** contiene due container principali:

1. **Web Container**: contiene i componenti servlet e JSP.
2. **EJB Container**: contiene i **beans**.

### Web Client
È costituito dal **browser web** (client HTTP), che comunica via HTTP con il server e si occupa del rendering della pagina in **HTML** o altri mark-up languages come **XML**.

### Web Application
Un gruppo di risorse server-side, composto da:
- **Classi Java**
- **JSP**
- **Risorse statiche** (HTML, immagini, CSS)
- **JavaScript** e altri componenti attivi lato client.
- **Informazioni di configurazione e deployment**

---

## [Accesso alla Web Application]
1. Il client invia una **request** (GET o POST).
2. L'URL viene mappato su una Web Application.
3. La Web Application elabora la richiesta e genera la risposta.
4. La risposta viene inviata al client.

---

## [Servlet]
Una **servlet** è una classe **Java** che fornisce un servizio al client mediante protocolli di tipo **request/response** (HTTP).  
Genera contenuti dinamici e supera i limiti delle CGI.  
Viene eseguita direttamente in un **Web Container** e estende la classe `HttpServlet`, che offre metodi utili per la gestione delle richieste.

> **Tomcat** è un servlet container: gestisce l’esecuzione delle servlet in runtime, la creazione dei thread, le richieste HTTP e il ciclo di vita delle servlet.

### Processo di gestione della request
All’arrivo di una richiesta HTTP, il servlet container genera due oggetti:
- **Request**: rappresenta la chiamata al server effettuata dal client.
- **Response**: rappresenta le informazioni restituite al client.

Questi oggetti vengono passati alla servlet corrispondente.

---

## [Request]
L’oggetto **request** rappresenta la chiamata al server effettuata dal client e contiene informazioni su:
1. Chi ha effettuato la richiesta.
2. I parametri passati.
3. Gli **header** passati.

---

## [Response]
L’oggetto **response** rappresenta le informazioni restituite al client e può includere:
1. Dati in forma **testuale** (HTML, testo) o **binaria** (immagini).
2. **HTTP header** e **cookie**.

La servlet specifica il **content type** e ottiene un **output stream** su cui scrivere il contenuto della risposta.

---

## [Ciclo di vita della servlet]
1. Se non esiste un’istanza della servlet, il container la inizializza con il metodo `init()`.
2. Successivamente, vengono invocati i metodi `doGet()` o `doPost()` per generare la response.  
   Questi metodi hanno come parametri:
   ```java
   HttpServletRequest request
   HttpServletResponse response
   ```

---

## [Servlet e multithreading]
Normalmente, esiste una sola istanza della servlet, ma il container genera un thread per ogni richiesta.  
Questo approccio introduce **concorrenza**:  
Il metodo `service()` viene chiamato solo dopo l’invocazione di `init()`, e può essere eseguito da numerosi client in modo concorrente.  
È quindi necessario gestire le sezioni critiche usando:
- `synchronized`
- Semafori
- Meccanismi di mutua esclusione

### Single-threaded (deprecato)
In alternativa, si può indicare al container di creare un’istanza della servlet per ogni richiesta concorrente.  
Tuttavia, questa scelta è onerosa in termini di risorse.

---

## [Metodi utili]
- **`getParameter`**: restituisce il valore di un parametro individuato dal nome.
- **`getContextPath`**: restituisce informazioni sul contesto della web application.
- **`getQueryString`**: restituisce la stringa di query.
- **`getPathInfo`**: restituisce il path.
- **`getInputStream`**: consente di leggere il body della richiesta.

---

## [Deployment]
Il deployment di una servlet comprende:

1. Definizione dell’ambiente di runtime.
2. Mappatura delle URL sulle servlet.
3. Definizione delle impostazioni di default.
4. Configurazione delle caratteristiche di sicurezza dell’applicazione.

---

## [Web Archives]
I file con estensione **.war** rappresentano il modo standard per il deployment delle web application.  
Si tratta di file **JAR** con una struttura particolare.

### `web.xml`
È un file di configurazione XML che contiene:
- La mappatura tra URL e servlet.
- Il nome della servlet.
- La classe **Java** associata.
- Parametri di configurazione (nome-valore).

Esempio:
```java
String getInitParameter(String parName) // Ottiene un parametro di inizializzazione
```

---

## [Servlet Context]
Ogni web application esegue in un **contesto**.  
Esiste una corrispondenza **1-1** tra web app e contesto.

L’interfaccia **ServletContext**, accessibile tramite il metodo `getServletContext`, consente di accedere:
- Ai parametri di inizializzazione.
- Agli attributi del contesto.

> Il contesto è condiviso tra tutti gli utenti, le richieste e i componenti server-side della stessa web application.  
Gli attributi del contesto sono variabili globali e possono essere settati e ottenuti tramite appositi metodi.

---

## [Sessione]
La sessione è un’entità gestita dal **web container** e condivisa fra tutte le richieste provenienti dallo stesso client.  
Consente di mantenere lo stato di sessione e ha un **session ID** univoco.

### Metodo `getSession`
```java
public HttpSession getSession(boolean createNew)
```
- Se `createNew` è `true` e non esiste una sessione, ne crea una nuova.
- Se `createNew` è `false` e non esiste una sessione, ritorna `null`.

---

## [Scope]
Lo scope definisce il **tempo di vita** e l’**accessibilità** delle informazioni condivise:

1. **Request**:
   - Tempo di vita: fino all’invio della risposta.
   - Accessibile alla servlet corrente e alle servlet incluse o forwardate.
2. **Session**:
   - Tempo di vita: durata della sessione.
   - Accessibile a tutte le richieste dello stesso utente.
3. **Application**:
   - Tempo di vita: durata dell’applicazione.
   - Accessibile a tutte le richieste alla stessa web app, anche da utenti diversi.

---

## [Include e Forward]
- **Include**: vengono passati i parametri `request` e `response`, e viene delegata l’elaborazione della risposta a un’altra servlet. Tuttavia, la risposta viene inviata sempre dalla servlet di base.
- **Forward**: delega a un’altra servlet la gestione della risposta. La risposta viene elaborata dalla servlet secondaria, ma il client non percepisce il cambiamento.


# CAPITOLO 7: JAVA SERVER PAGES

## [JSP]
Le **JSP** (Java Server Pages) sono uno dei due componenti di base della tecnologia **J2EE**.  
Sono template per la generazione di contenuto dinamico, estendendo **HTML** con codice **Java custom**.

### Richiesta a una JSP
1. La parte **HTML** viene trascritta sullo stream di output.
2. Il codice **Java** viene eseguito sul server per generare HTML dinamico.
3. Viene formata la pagina HTML (parte statica + parte dinamica) che viene restituita al client.

> Le JSP vengono trasformate in **servlet** dal container.

---

## [JspServlet]
Le richieste verso JSP sono gestite da una particolare servlet (in **Tomcat**: `JspServlet`) che:
1. Traduce la JSP in servlet.
2. Compila la servlet risultante in una classe.
3. Esegue la JSP.

Il ciclo di vita della JSP è:
```
Jsp -> Istanza -> Inizializzazione -> Service -> Destroy -> Finalize (garbage collection)
```

---

## [Servlet e JSP]
Le JSP sono nate per facilitare la progettazione grafica e l'aggiornamento delle pagine.  
Nelle servlet, infatti, la progettazione delle pagine è time-consuming, ripetitiva e soggetta a errori.

- Le **JSP** permettono di separare facilmente il lavoro fra grafici e programmatori.
- Le **servlet** offrono un **controllo completo** sull'applicazione e sono più adatte quando si devono fare differenziazioni in base ai parametri della richiesta.
- Le **JSP** semplificano la presentazione di documenti **HTML** o **XML** all'utente.

---

## [Come funzionano le JSP]
Ogni volta che arriva una richiesta, il server compone dinamicamente il contenuto della pagina.  
I tag `<%...%>` valutano le espressioni **Java** interne e ne restituiscono il risultato, permettendo di generare pagine dinamicamente.

- Prima arriva l'**header**, quindi la JSP deve effettuare tutte le modifiche all'header prima di creare il body.
- Non è possibile interrompere il processo di risposta, altrimenti il browser mostra solo una parte della pagina.

---

## [Tag JSP]
Esistono due tipi di tag JSP:
1. **Scripting Oriented Tag**
2. **XML Oriented Tag**

### Tipi di tag
1. **Dichiarazioni** (`<%!…%>`): per dichiarare variabili e metodi.
2. **Espressioni** (`<%=…%>`): per valutare espressioni **Java**.
3. **Scriptlet** (`<%...%>`): per aggiungere frammenti di codice **Java** eseguibile, usati principalmente per logiche di controllo (`if`, `for`).
4. **Direttive** (`<%@...%>`): comandi JSP valutati a tempo di compilazione.

#### Tipi di direttive
- **Page**: per importare package, dichiarare pagine di errore e definire il modello di esecuzione JSP (`language`, `import`, `session`).
- **Include**: per includere un altro documento.
- **Taglib**: per caricare una libreria di tag custom implementata dallo sviluppatore.

---

## [Built-in Objects]
In JSP esistono 9 oggetti già inclusi, utilizzabili senza dover creare istanze:

1. **page**: rappresenta l'istanza corrente della servlet.
2. **config**: contiene la configurazione della servlet (parametri di inizializzazione).
3. **request**: rappresenta la richiesta inviata alla pagina JSP.
4. **response**: rappresenta la risposta restituita al client.
5. **out**: gestisce l'I/O della pagina (stream di caratteri in output).
6. **session**: informazioni sul contesto della sessione utente.
7. **application**: fornisce informazioni sul contesto con uno scope di visibilità comune a tutti gli utenti (`ServletContext`).
8. **pageContext**: rappresenta l'insieme degli oggetti built-in di una JSP e fornisce informazioni sul contesto di esecuzione della pagina.
9. **exception**: connesso alla gestione degli errori.

---

## [Azioni JSP]
Le azioni JSP sono comandi per interagire con altre JSP, servlet o JavaBean:

1. **useBean**: crea un'istanza di JavaBean e le associa un identificativo.
2. **getProperty**: restituisce una proprietà indicata come oggetto.
3. **setProperty**: imposta il valore di una proprietà.
4. **include**: include nella JSP il contenuto generato dinamicamente da un'altra pagina locale, trasferendo temporaneamente il controllo a un'altra pagina.
5. **forward**: cede il controllo a un'altra JSP o servlet (vengono passati `request`, `response` e `session`, ma cambia il `pageContext`).
6. **plugin**: genera contenuto per scaricare un plug-in Java.

---

## [JavaBeans]
I **JavaBeans** sono il modello di base per i componenti **Java**.  
Un **JavaBean** non è altro che una classe **Java** dotata di alcune caratteristiche specifiche:

1. La classe deve essere **public**.
2. Deve avere un **costruttore senza argomenti** (necessario per la costruzione dinamica delle istanze).
3. Deve avere metodi `getProp()` e `setProp()` per gestire le proprietà, che sono elementi dello stato. Se la proprietà è di tipo `boolean`, il metodo getter diventa `isProp()`.
4. Deve seguire precise regole di registrazione dei metodi.

Un container di JavaBean si chiama **Bean Container** e permette di interfacciarsi con i bean utilizzando **Java Reflection**.

---

### Azioni sui JavaBean in JSP
- **useBean**: per istanziare un JavaBean.
  ```jsp
  <jsp:useBean id="nomeIstanza" class="classeJava" scope="..."/>
  ```
- **setProperty**: per impostare il valore di una proprietà.
  ```jsp
  <jsp:setProperty name="nomeIstanza" property="nomeProp" value="valore"/>
  ```
- **getProperty**: per ottenere il valore di una proprietà.
  ```jsp
  <jsp:getProperty name="nomeIstanza" property="nomeProp"/>
  ```

---

## [Model 1]
Un'architettura J2EE a due livelli è costituita da:
- **JSP** per il livello di presentazione.
- **JavaBean** per il livello di business logic.


# CAPITOLO 8: JAVASCRIPT (ECMAScript)

**JavaScript** è un linguaggio di scripting sviluppato per dare interattività **lato client** alle pagine HTML.  
Permette la creazione di **pagine web attive**, ovvero "pagine dinamiche" meglio definite come "attive".

## [JavaScript e Java]
JavaScript e Java sono due linguaggi completamente diversi.  
L'unica similitudine è che entrambi hanno una sintassi simile a quella di **C**:

1. **JavaScript è interpretato**: eseguito da un interprete interno al browser, non è compilato.
2. **JavaScript è object-based ma non class-based**.
3. **JavaScript è debolmente tipizzato**: le variabili possono cambiare tipo dinamicamente, anche se i dati stessi hanno un tipo specifico.

JavaScript consente di accedere e modificare elementi HTML, reagire agli eventi, validare i dati inseriti e interagire con il browser.

---

## Caratteristiche del linguaggio
- JavaScript è **case-sensitive**: distingue tra lettere maiuscole e minuscole.
- Le istruzioni terminano con il punto e virgola (`;`).
- Ammessi commenti sia in stile C (`/* */`) sia in stile C++ (`//`).
- Gli identificatori non possono iniziare con una cifra.

---

## [Variabili]
Le variabili vengono dichiarate con la parola chiave `var`.  
Non hanno un tipo specifico e possono contenere valori di qualunque tipo.  
JavaScript ha uno **scope globale** e **locale**, ma non **di blocco**.  
Una variabile non inizializzata assume il valore `undefined`.  
Una variabile può essere definita come `null`.

---

## [Tipi primitivi]
1. **NUMBER**: rappresenta numeri in virgola mobile a 8 byte.  
   Non c'è distinzione tra interi e numeri reali. Esistono i valori speciali:
   - `NaN` (Not a Number) per operazioni non ammesse.
   - `Infinity` per rappresentare l'infinito.
2. **BOOLEAN**: può assumere solo i valori `true` e `false`.

Il tipo viene determinato dinamicamente in base al valore assegnato.

---

## [Oggetti]
Gli oggetti sono tipi composti che contengono delle **proprietà** (attributi).  
Ogni proprietà ha un **nome** e un **valore**, e si accede ad essa con l'operatore punto (`.`).  
Le proprietà possono essere aggiunte dinamicamente.

Esempio:
```javascript
var o = new Object();
o.nome = "Mario";
o.età = 30;
```

`new Object()` è un costruttore che inizialmente crea un oggetto vuoto.

---

## [Array]
Gli array sono tipi composti con elementi accessibili tramite indice numerico, che parte da 0.  
In JavaScript, gli oggetti possono essere usati come **array associativi**, ovvero array con chiavi di tipo stringa anziché numerico.

---

## [Stringhe]
Le stringhe sono dati di tipo primitivo che sembrano oggetti.  
Sono rappresentate come sequenze di caratteri in formato **UNICODE** a 16 bit.

---

## [Regular Expression]
Le espressioni regolari possono essere create con:
```javascript
var r = new RegExp("[abc]");
```

---

## Tipi di dati
- **Tipi valore**: numeri e booleani.
- **Tipi riferimento**: array, oggetti e stringhe (le stringhe sono un tipo eccezionale).

---

## [Funzioni]
Le funzioni sono frammenti di codice che possono essere definiti una volta e utilizzati più volte.  
Ammettono parametri privi di tipo e restituiscono un valore il cui tipo non è definito.  
Una funzione può essere assegnata a una variabile.

Esempio:
```javascript
var saluta = function(nome) {
  return "Ciao " + nome;
};
```

---

## [Metodi]
Un **metodo** è una funzione assegnata a una proprietà di un oggetto.  
Un **costruttore** è una funzione il cui scopo è costruire un oggetto.

---

## Oggetti predefiniti
JavaScript include diversi oggetti predefiniti, tra cui:
- **Date**
- **Math**
- **Document**

---

## [Operatori]
JavaScript supporta operatori simili a quelli di **C** e **Java**, come:
- `delete`
- `void`
- `typeof`
- `===` (uguaglianza stretta)
- `!==` (non uguaglianza stretta)

---

## [Istruzioni]
1. **Espressioni**: assegnamenti, invocazioni di funzioni e metodi.
2. **Istruzioni composte**: blocchi di istruzioni delimitati da parentesi graffe.
3. **Istruzione vuota**: punto e virgola senza nulla prima.
4. **Istruzioni etichettate**: istruzioni con un'etichetta davanti (es. `label: istruzione;`).
5. **Strutture di controllo**: `if`, `for`, `while`.
6. **Definizioni e dichiarazioni**: `var`, `function`.
7. **Istruzioni speciali**: `break`, `continue`, `return`.

---

## [Oggetto globale]
Tutte le variabili e le funzioni appartengono all'oggetto globale.  
Funzioni dell'oggetto globale:
1. `eval(expr)`: valuta la stringa `expr`.
2. `isFinite(number)`: verifica se un numero è finito.
3. `isNaN(value)`: verifica se un valore non è un numero.
4. `parseInt(str)`: converte una stringa in un intero.
5. `parseFloat(str)`: converte una stringa in un numero a virgola mobile.

---

## [Come includere JavaScript]

### Interno
```html
<script type="application/javascript">
  // codice JavaScript
</script>
```

### Esterno
```html
<script language="Javascript" src="nomefile.js"></script>
```

---

## [Browser Object Model]
Per interagire con la pagina HTML, JavaScript utilizza una gerarchia di oggetti predefiniti denominata **Browser Objects** e **DOM Objects**.

Sebbene la pagina venga generata dinamicamente, senza l'uso degli eventi non si ha una reale interattività con l'utente.  
L'uso più comune è quello di generare pagine diverse in base al tipo di browser o alla risoluzione dello schermo.

La pagina corrente è rappresentata dall'oggetto **document**.

---

## Oggetti del Browser Object Model
1. **navigator**: rileva il tipo di browser.
2. **screen**: fornisce informazioni sullo schermo.
3. **document**: rappresenta la pagina corrente.

Per ottenere una reale interattività, si utilizza il meccanismo degli **eventi**, modificando dinamicamente la struttura della pagina agendo sul **DOM**.

> **DHTML = JavaScript + DOM + CSS**

---

## [Oggetto Document]
L'oggetto `document` presenta 4 collezioni di oggetti (sono array):
1. `anchors`
2. `forms`
3. `images`
4. `links`

---

## [jQuery]
**jQuery** è una libreria JavaScript pensata per semplificare la vita del programmatore web.  
Offre funzionalità per:
- Attraversamento del DOM.
- Animazioni.
- Gestione degli eventi.
- Interazioni AJAX.



# CAPITOLO 9: AJAX

## Nuovo modello dettato da DHTML
Due livelli di eventi:

1. **Eventi Locali**: portano a una modifica diretta del DOM da parte di **JavaScript**.
2. **Eventi Remoti**: ottenuti tramite ricaricamento della pagina, che viene modificata lato server in base ai parametri passati in **GET** o **POST** (*postback*).

### Modello sincrono
Ancora però rimane un modello **sincrono**, in cui l'utente attende la risposta dal server.

---

## [AJAX]
AJAX è un modello di interazione nato per superare queste limitazioni e garantire quindi **asincronicità**.  
**AJAX = Asynchronous JavaScript And XML** (anche se non è un acronimo vero e proprio).

Non è una nuova tecnologia in sé, ma è basato su tecnologie standard combinate insieme.  
L'idea alla base di AJAX è quella di interagire direttamente con il server senza ricaricare la pagina.  
L'oggetto JavaScript fondamentale è **XMLHttpRequest**, che consente di ottenere dati dal server in modo asincrono.

### Processo AJAX
1. Evento → Si istanzia un oggetto **XMLHttpRequest**.
2. Si configura l'oggetto associandogli una funzione di callback.
3. Si effettua una chiamata asincrona al server.
4. Il server elabora la richiesta e risponde.
5. Il browser invoca la funzione di callback che elabora il risultato e aggiorna il DOM per mostrare i dati ricevuti.

---

## [XMLHttpRequest]: Metodi

1. **open()**  
   Inizializza la richiesta da formulare al server.  
   ```javascript
   open(method, uri, true)
   ```
   - `method`: stringa che assume il valore **GET** o **POST**.
   - `uri`: stringa che identifica la risorsa da ottenere.
   - `async`: `true` indica una richiesta asincrona (impostato normalmente su `true`).

2. **setRequestHeader()**  
   Consente di impostare gli header HTTP della richiesta da mandare.  
   Parametri:
   - `nomeHeader`: nome dell'header.
   - `valore`: valore dell'header.  
   Può essere invocato più volte, una per ogni header da impostare. Per richieste **GET**, gli header sono opzionali.  
   **Nota**: impostare `Connection: close` è importante per garantire che la connessione venga chiusa una volta completata l'elaborazione.

3. **send()**  
   Invia la richiesta al server.  
   Parametro:
   - `body`: stringa che costituisce il body della richiesta HTTP.

---

## [XMLHttpRequest]: Proprietà

1. **readyState**  
   Indica lo stato della richiesta, è un intero che assume 5 valori:
   - `0`: **uninitialized** → l'oggetto esiste ma non è stato chiamato `open()`.
   - `1`: **open** → il metodo `open()` è stato invocato ma `send()` non ha ancora inviato i dati.
   - `2`: **sent** → il metodo `send()` è stato eseguito.
   - `3`: **receiving** → la risposta ha cominciato ad arrivare.
   - `4`: **loaded** → l'operazione è completata.

2. **onreadystatechange()**  
   Approccio basato su eventi: occorre registrare una funzione di **callback** che viene richiamata in modo asincrono ad ogni cambio di stato di `readyState`.  
   ```javascript
   xhr.onreadystatechange = handlerCallback();
   ```
   L'assegnamento della funzione va fatto prima della chiamata `send()`.

3. **status()**  
   Contiene un valore intero corrispondente al codice HTTP dell'esito della richiesta:
   - `200`: successo.
   - `403`, `404`: errori lato cliente.
   - `500`: errori lato server.

4. **statusText()**  
   Contiene una descrizione testuale del codice HTTP restituito dal server.

5. **responseText()**  
   Contiene il body della risposta HTTP come stringa.

6. **responseXML()**  
   Contiene il body della risposta convertito in un documento **XML**.

7. **getResponseHeader(headername)**  
   Consente di leggere gli header HTTP che descrivono la risposta del server.  
   Utilizzabile solo nella funzione di callback, invocabile solo quando la richiesta è conclusa (**readyState == 4**).

---

## [Callback]
La funzione di callback viene invocata ad ogni variazione di `readyState`.  
Occorre verificare manualmente il valore di `readyState` all'interno della callback.  
Esempio:
```javascript
if (xhr.readyState == 4 && xhr.status == 200) {
    // Operazioni da eseguire con la risposta ricevuta
}
```

---

## [Vantaggi e Svantaggi di AJAX]

### Svantaggi
1. **Linearità dell'interazione**: se la comunicazione non viene gestita correttamente, il cliente potrebbe attendere una risposta che non arriva mai o ricevere risposte errate.
2. Si opta per interrompere le richieste che non terminano in tempo utile.

### Metodo abort()
Consente l'interruzione delle operazioni di invio o ricezione.  
Non ha senso chiamarlo nella **callback**, ma va chiamato in un'altra funzione che deve essere invocata in modo asincrono mediante:
```javascript
setTimeout(funzioneAs, timeout);
```

### Problemi di debug, test e mantenimento
AJAX introduce difficoltà nella fase di debug e nel mantenimento del codice.

---

## [XML]
L'utilizzo di **XML** come formato di scambio fra client e server porta a:
- Generazione e utilizzo di una quantità elevata di byte.
- Difficoltà di lettura e manutenzione.
- Onerosità in termini di risorse di elaborazione, poiché **JavaScript** è interpretato.

---

## [JSON = JavaScript Object Notation]
Formato per lo scambio di dati considerato molto più comodo di **XML**.  
Vantaggi:
- Più leggero in termini di dati scambiati.
- Più semplice ed efficiente da elaborare.
- Più leggibile.
- Supportato da un numero maggiore di linguaggi.  

Esempio:
```javascript
var ogg = { "nome": "valore" };
```
La sintassi di **JSON** si basa su quella delle costanti oggetto e array di JavaScript, infatti sono molto simili.  
Con `eval` è possibile trasformare una stringa JSON in un oggetto JavaScript.

---

## [Parser JSON]
Si preferisce utilizzare parser appositi anziché `eval` per motivi di sicurezza e performance.

### Parser JABSORB
```javascript
JSON.parse(strJSON);    // Converte una stringa JSON in un oggetto JavaScript
JSON.stringify(objJSON); // Converte un oggetto JavaScript in una stringa JSON
```

### Parser GSON
**GSON** è una libreria di **Java** per il parsing/deparsing di oggetti JSON, usata principalmente lato server.  
Consente rappresentazioni custom per gli oggetti.

Esempio:
```java
Gson gson = new Gson();
gson.toJson(oggetto);              // Da oggetto Java a JSON
Obj ogg = gson.fromJson(json, Obj.class);  // Da JSON a oggetto Java
```


---

# CAPITOLO 10: FRAMEWORK REACT

## [REACT.js]
React.js è una libreria JavaScript per la creazione di interfacce utente web. Rientra tra gli strumenti utili per lo sviluppo front-end di web application. Ogni codice scritto in React esegue all'interno del browser, essendo una libreria JavaScript. Permette di invocare API lato server ma non interagisce direttamente con database o altre sorgenti dati sul back-end.

Si ispira alla metodologia SPA (Single Page Application), dove un'applicazione web interagisce col browser per modificare le pagine dinamicamente in funzione dei dati ricevuti dal back-end.

Lo sviluppo della pagina avviene tramite la creazione di **componenti**, che interagiscono con le API di React.js per manipolare il DOM e generare elementi di interfaccia utente.

## [VIRTUAL DOM]
Il Virtual DOM è una copia del DOM reale mantenuta in memoria. Quando avviene un evento, invece di manipolare direttamente il DOM del browser, React aggiorna il Virtual DOM. Successivamente, confronta la nuova versione con la precedente (diffing) e applica al DOM reale solo le modifiche necessarie, ottimizzando così il rendering della pagina.

## [VANTAGGI]
1. **Approccio a componenti:** consente di costruire interfacce complesse attraverso mattoncini semplici.
2. **Efficienza nel rendering:** grazie al Virtual DOM, React riduce il numero di operazioni necessarie sul DOM reale.
3. **Riutilizzabilità:** i componenti possono essere riutilizzati in diverse parti dell'applicazione.

## [REACT ELEMENT]
Un React Element è un oggetto semplice e immutabile che descrive cosa si vuole visualizzare sullo schermo.

## [REACT COMPONENT]
Un componente è un oggetto dinamico che riceve input sotto forma di **props** e restituisce un React Element, descrivendo cosa deve apparire sullo schermo.

### Tipi di componenti:
1. **Class Components:** hanno caratteristiche aggiuntive come lo state e i metodi di ciclo di vita.
2. **Function Components:** funzioni JavaScript che accettano props e restituiscono React Element. Supportano gli hooks per gestire state ed effetti.

### Esempio di componente:
```javascript
function Saluto(props) {
  return <h1>Ciao, {props.nome}!</h1>;
}
```

## [JSX]
JSX (JavaScript XML) permette di mescolare codice HTML e JavaScript. Consente di scrivere tag HTML direttamente nel codice JavaScript senza usare metodi come `createElement` o `appendChild`.

Esempio:
```javascript
const elemento = <h1>Ciao mondo!</h1>;
```
Il browser non può interpretare direttamente JSX, quindi è necessario un compilatore come Babel per trasformarlo in JavaScript.

## [PROPS]
Le props sono proprietà immutabili passate ai componenti per configurarne il comportamento. Sono contenute in un oggetto passato come argomento alla funzione componente.

## [STATE]
Lo state è un oggetto mutabile che contiene dati privati di un componente e ne determina il comportamento e il rendering.

Esempio:
```javascript
class Contatore extends React.Component {
  constructor(props) {
    super(props);
    this.state = { conteggio: 0 };
  }
  incrementa = () => {
    this.setState({ conteggio: this.state.conteggio + 1 });
  };
  render() {
    return (
      <div>
        <p>Conteggio: {this.state.conteggio}</p>
        <button onClick={this.incrementa}>Incrementa</button>
      </div>
    );
  }
}
```

## [GESTIONE DEGLI EVENTI]
React gestisce gli eventi in modo simile al DOM, ma con una sintassi leggermente diversa.

Esempio:
```javascript
<button onClick={this.handleClick}>Cliccami</button>
```

Gli eventi in React sono eventi sintetici, cioè oggetti cross-browser basati sugli eventi del DOM.

## [FORM]
I form in React sono controllati attraverso lo state del componente. L'input dell'utente aggiorna lo stato del componente, che a sua volta aggiorna il valore del campo.

Esempio:
```javascript
class FormEsempio extends React.Component {
  constructor(props) {
    super(props);
    this.state = { valore: '' };
  }
  handleChange = (event) => {
    this.setState({ valore: event.target.value });
  };
  render() {
    return (
      <form>
        <input type="text" value={this.state.valore} onChange={this.handleChange} />
      </form>
    );
  }
}
```

## [CONFRONTO CON ALTRI FRAMEWORK]
1. **Angular:** sviluppato da Google, utilizza TypeScript ed è un framework completo per la realizzazione di SPA.
2. **Vue.js:** framework progressivo, facile da integrare in progetti esistenti.
3. **JQuery:** libreria JavaScript per la manipolazione del DOM e le richieste AJAX, meno usata nelle nuove applicazioni.

---

Questo capitolo fornisce una panoramica di React.js, una delle librerie più popolari per lo sviluppo di interfacce utente moderne, evidenziandone i principali concetti e vantaggi.


# CAPITOLO 11: JAVAMODEL2, EJB, SPRING

## [MODEL 1]
Pattern semplice in cui il codice responsabile per la presentazione dei contenuti è mescolato con la logica di business.  
**Suggerito per piccole applicazioni.**

## [MODEL 2]
Più complesso e articolato, separa chiaramente il livello di presentazione dei contenuti dalla logica utilizzata per manipolare e processare i contenuti stessi.  
**Suggerito per applicazioni medio-grandi.**

Dato che è basato su una separazione netta fra logica di business e di presentazione, viene associato al paradigma **MVC (Model-View-Controller)**:

- **MODEL**: rappresenta il livello dei dati, incluse le operazioni di accesso e modifica.
- **VIEW**: gestisce la presentazione e l'interfaccia utente, effettua il rendering dei contenuti presi dal Model.  
  Accede ai dati tramite il Model e gira l'input utente verso il Controller.
- **CONTROLLER**: funziona come intermediario tra Model e View, definisce il comportamento dell'applicazione.  
  Accede alle funzionalità incapsulate dal Model, fa il dispatching delle richieste utente e seleziona la View per la presentazione. Interpreta l'input utente e gestisce le azioni che devono essere eseguite dal Model.  
  Si occupa di eseguire la logica di business necessaria per ottenere il contenuto da mostrare, mette il contenuto (sotto forma di JavaBean) in un messaggio e decide a quale **View** (JSP normalmente) passare la richiesta.

### Componenti principali
- **Servlet / EJB Session Bean**: logica di business (**Controller**)
- **JSP**: logica di presentazione (**View**)
- **Model**: rappresentato da ogni struttura dati

Abbiamo già visto come l'utilizzo del **container** risolva i problemi della struttura monolitica o modulare.

---

## [SOLUZIONI A CONTAINER PROPRIETARIE e STANDARD-BASED]

### 1. Soluzioni Proprietarie
- Il **container** serve per fornire servizi di sistema.
- I **componenti** servono per la logica di business.
- Il contratto container-componente è ben definito ma in modo **proprietario** (le modalità sono definite dal produttore o sviluppatore del framework e non seguono standard universali).

### 2. Standard Aperti
- Il **container** fornisce i servizi di sistema in modo ben definito in accordo a standard industriali (**J2EE**).  
  Gode della portabilità del codice perché basato su API in standard aperti e bytecode Java.

---

## APPLICATION SERVER JEE
Piattaforma **open** e **standard** per lo sviluppo, deployment e gestione di applicazioni **enterprise n-tier**, **web-enabled**, **server-centric** e basate su componenti.

### [Modelli N-Tier in J2EE]
1. **Modello 4-Tier**:  
   Cliente HTML | JSP/Servlet | EJB | JDBC (API fornita da Java per connettersi, interagire e gestire DB relazionali)

2. **Modello 3-Tier**:  
   Cliente HTML | JSP/Servlet | JDBC

3. **Modello 3-Tier**:  
   Applicazioni standalone EJB client-side | EJB | JDBC

4. **Applicazioni Enterprise B2B**:  
   Interazioni tra piattaforme J2EE tramite messaggi JMS o XML-based

---

## [FUNZIONI DI SERVIZIO DEL CONTAINER]

1. **Supporto al ciclo di vita**  
   (attivazione e disattivazione del servitore, mantenimento dello stato e recupero delle informazioni tramite DB)

2. **Supporto al sistema dei nomi**  
   (federazione con altri container, discovery del servitore e del servizio)

3. **Supporto alla qualità del servizio**  
   (tolleranza ai guasti, selezione tra possibili deployment)

---

## [EJB CONTAINER]
Sviluppo e deployment semplificato di applicazioni **Java**.  
Amplifica i benefici del modello a componenti lato server.  
Fornisce i servizi di sistema.  
**Modello a container pesante** (contrapposto a **Spring**, container leggero).

Le applicazioni **EJB** e i loro componenti devono essere **debolmente accoppiati**, il loro comportamento è definito tramite interfacce e non si occupano della gestione delle risorse.  
Sono **N-Tier**:
1. **Session Tier**: API verso l'applicazione
2. **Entity Tier**: API verso le sorgenti dati

### EJB Server
- L'**Application Server** contiene il **container pesante** attivo.
- Il cliente può interagire **remotamente** (tramite interfacce) con le componenti **EJB**.
- I **descrittori di deployment** forniscono istruzioni al container su come gestire e controllare il comportamento delle componenti J2EE.  
  Semplificano la portabilità del codice e sono sostituibili con annotazioni da Java 5.

---

## [COMPONENTI BEAN]

### 1. Session Bean
Lavorano tipicamente per un singolo cliente ed hanno una vita media relativamente breve (**non sono persistenti**).  
Non rappresentano dati in un DB ma possono accedere e modificare questi dati.  
Utilizzati per modellare oggetti di processo o di controllo specifici per un particolare cliente, e per coordinare interazioni fra bean.  
Servono a muovere la logica applicativa di business dal lato cliente a quello servitore.

- **Stateless**: esegue una richiesta e restituisce un risultato senza salvare informazioni relative a quel cliente.
- **Stateful**: mantiene uno stato specifico per un cliente.

### 2. Entity Bean
Forniscono una vista ad oggetti dei dati mantenuti in un DB.  
Componenti sincronizzati con i relativi DB relazionali.  
Accesso condiviso per clienti differenti.  
I componenti rimangono nel sistema fino a che i dati esistono nel DB (**long lived**).

### 3. Message Driven Bean
Svolgono il ruolo di consumatori di messaggi asincroni.  
È importante che siano **asincroni** perché gestiscono messaggi in arrivo da una coda o un argomento.  
Non possono essere direttamente invocati dai clienti ma sono attivati in seguito all'arrivo di un messaggio.  
Sono **stateless**.

---

## [SB e EB]

| **Caratteristiche**         | **Session Bean (SB)**                            | **Entity Bean (EB)**                              |
|-----------------------------|-------------------------------------------------|-------------------------------------------------|
| **Rappresenta**              | Processo di business                            | Dati di business                                 |
| **Istanza**                  | Per cliente                                     | Condivisa tra clienti multipli                  |
| **Durata**                   | Short-lived (pari a quella del cliente)         | Long-lived (pari a quella dei dati nel DB)      |
| **Sopravvivenza a crash**    | No                                              | Sì                                              |

---

## [SERVIZI CONTAINER-BASED]

### Resource Pooling
- Tecnica per gestire risorse limitate in modo efficiente riutilizzandole invece di crearle e distruggerle.
- Ogni **EJB Container** mantiene un insieme di istanze del bean pronte per servire le richieste del cliente.

---

## [SPRING]

Framework **a container leggero** per applicazioni **Java SE** e **Java EE**.

### Funzionalità chiave
1. **Dependency Injection (DI)**:  
   Modo per fornire a un oggetto le sue dipendenze necessarie senza che l'oggetto debba preoccuparsi di crearle o gestirle.
2. **Inversion of Control (IoC)**:  
   Il controllo viene delegato a un container o framework interno che gestisce inizializzazione e dipendenze.

---

# CAPITOLO 12: NODE.js

L'obiettivo è quello di cercare nuovi modelli più **asincroni** e **scalabili** (capacità di gestire una crescita del carico di lavoro senza perdere prestazioni e stabilità).

## [NODE.js]
- Supporto **I/O asincrono non bloccante**
- Tecnologia **server-side**
- Non utilizzo di **thread/processi dedicati**
- Maggiore **scalabilità**
- Verso server **stateless**

### Caratteristiche principali
1. È necessario **server-side** un ambiente di esecuzione JavaScript che ospiti **Google Chrome V8 Engine**.
2. Progettato per estrema concorrenza e scalabilità (senza thread o processi dedicati, definiti ad esempio a livello applicazione).
3. **Non bloccante**
4. **Event Loop**: al posto dei thread viene usato un event loop (meccanismo che permette l'esecuzione non bloccante del codice).  
   Utilizza un **singolo thread** per gestire più operazioni contemporaneamente. Mantiene una coda di eventi e **callback** da eseguire; ogni volta che un'operazione asincrona (come una chiamata I/O) termina, il relativo callback viene aggiunto alla coda e poi eseguito.
   
   - È presente quindi un **unico thread** che fa ripetutamente fetching di eventi dalla coda, salva lo stato e passa a processare il prossimo evento in coda.
   - Usa framework con meccanismi per I/O asincrono.

### I/O Bloccante
Le operazioni I/O bloccanti fermano il progresso di un thread in attesa della lettura.  
Nei server tradizionali si utilizza il multi-threading per limitare l'attesa, ma ogni thread passa comunque la maggior parte del tempo in attesa di I/O. Questo porta a un overhead significativo e a un elevato uso di memoria.

In **Node.js**, che è **single-thread**, ogni funzione che fa operazioni I/O viene gestita in modo asincrono e non bloccante tramite callback.  
Node supporta decine di migliaia di connessioni concorrenti senza costo di **context switching** (usando un unico thread). Ciò implica che le task nell'**event loop** devono essere eseguite velocemente per non **bloccare la coda**.

- Viene usato lo stesso JavaScript engine sia lato browser che lato server.
- Uso di interfaccia ad eventi per ogni operazione del sistema operativo.

---

## [QUANDO USARE NODE.js]
Node.js è particolarmente usato per:
- Creazione di **web server** e strumenti di networking.
- Utilizzo di una **collezione di moduli** per varie funzionalità.
- Sviluppo rapido di applicazioni web grazie a un **insieme di moduli** (framework).

---

## [UNIFORMITÀ DI JAVASCRIPT LATO CLIENTE E SERVER]
Una delle caratteristiche distintive di Node.js è l'uso di **JavaScript** sia lato client che lato server, eliminando il "context switch" di linguaggio per il web developer.

- **Client-side**: JavaScript utilizza ampiamente il DOM, senza accesso a file o persistent storage.
- **Server-side**: JavaScript lavora prevalentemente con file e/o persistent storage, senza DOM.

---

## [MODULI NODE.js]
Node.js include circa **20 moduli** predefiniti, alcuni di basso livello e altri di alto livello.

### NPM
**NPM (Node Package Manager)** permette a chiunque di creare un modulo Node.js con funzionalità aggiuntive e pubblicarlo su npm.  
NPM è un package manager di grande successo che semplifica il riuso di codice JavaScript in forma di moduli.  
I comandi di npm vengono eseguiti tramite linea di comando.

### Listener ed Emitter
- **Listener**: funzione da chiamare quando un evento associato viene lanciato.
- **Emitter**: segnale che un evento è accaduto.

L'emissione di un evento causa l'invocazione di tutte le funzioni listener in modo **sincrono** e **bloccante**, nell'ordine con cui sono stati registrati.

---

## [STREAM]
Node.js presenta moduli che producono o consumano flussi di dati (**stream**).  
È possibile costruire stream anche dinamicamente e aggiungere moduli sul flusso.

---

## [MODULO NET]
Il **modulo NET** serve a fare da wrapper per le chiamate di rete del sistema operativo (incapsulandole).  
Permette di creare il server collegandosi a una porta e di mettersi in ascolto per le connessioni.

---

## [MODULO EXPRESS]
**Express** è il framework più utilizzato oggi per lo sviluppo di applicazioni web su Node.js.  
Offre eseguibili per la rapida generazione di applicazioni.

---

## [ALTRI MODULI]
- **HTTP**: metodi ed eventi per creare un HTTP server
- **URL**: metodi per risoluzione e parsing dell'URL
- **QUERYSTRING**: metodi per prendere i parametri stringa di una query
- **PATH**: metodi per gestire il file path
- **FS**: metodi, classi ed eventi per lavorare con i file
- **UTIL**: funzioni utili per i programmatori

---

## [HTTP/3 e QUIC]
**QUIC** è un protocollo di trasporto basato su **UDP**. A differenza di HTTP/1.1 e HTTP/2, infatti, HTTP/3 è basato su una connessione QUIC, progettata per sostituire TCP in scenari web più moderni.

- **TLS** è integrato in QUIC e combina trasporto e sicurezza in un'unica implementazione, riducendo il tempo necessario per stabilire la connessione.
- Ogni flusso di pacchetti è indipendente, e QUIC gestisce la perdita di pacchetti in modo più efficiente rispetto a TCP (che bloccherebbe l'intero flusso).

### Applicazioni in tempo reale
In applicazioni in **tempo reale**, come le videochiamate, in cui è necessaria una rapidità elevata di **handshaking**, si usa **QUIC**.  
È adatto anche in contesti con un'alta probabilità di perdita di pacchetti.

### Svantaggi di QUIC
1. Non tutte le reti supportano QUIC.
2. Maggiore carico sulla CPU (a causa dell'overhead).
3. Debugging più complesso.
4. Consumo maggiore in termini di larghezza di banda.


# CAPITOLO 13: JSF E WEBSOCKET

## [JSF = Java Server Faces]
Evoluzione di **JSP**, è un framework per applicazioni web.  
Anch'essa basata su componenti, sia nella pagina web che collegati a componenti **server-side**.  
Offre ricche **API** per la rappresentazione dei componenti, gestione dello stato e degli eventi, e un'ampia libreria di **tag**.

### Backing Bean
Componenti collegati ai componenti server-side, registrati tramite l'annotazione `@ManagedBean`.  
In generale, contengono la logica di business (**Controller**), il cui risultato finale è la produzione di dati (**Model**).

### FacesServlet
Servlet predefinita inclusa in JSF, che si occupa di gestire le richieste per le pagine JSF (mapping tramite `web.xml`).  
La gestione del ciclo di vita dell'applicazione Facelets è svolta dal **container JSF**.

### [Ciclo di Vita]
1. **Deployment** dell'applicazione sul server.
2. Quando arriva una richiesta, viene creato un **albero dei componenti** contenuti nella pagina (messo in `FacesContext`).
3. L'albero dei componenti viene popolato con valori dal **BackingBean** e gestisce eventuali eventi.
4. Viene costruita una **View** sulla base dell'albero dei componenti.
5. L'albero viene deallocato automaticamente.
6. In caso di chiamate successive, l'albero viene riallocato.

---

## [JSF Managed Bean]
Sono semplici **JavaBean** che seguono le regole standard dei normali bean, ma con ulteriori metodi chiamati **ACTION**:
- Vengono invocati automaticamente in risposta ad azioni utente o eventi.
- Simili a classi **Action** in **Struts**.

### 4 Scope dei Managed Bean
1. **Application**
2. **Session**
3. **Request**
4. **Scopeless**: possono essere acceduti da altri bean e sono soggetti a garbage collection come ogni oggetto Java.  
   (*Garbage collection*: gestione e liberazione della memoria occupata da oggetti o variabili non più in uso).

---

## [Templating]
Permette l'utilizzo di pagine come base (**template**) per altre pagine.  
Sono definiti dei tag specifici per il templating:

- `ui:insert`: parte di un template in cui potrà essere inserito contenuto.
- `ui:define`: definisce il contenuto con cui la pagina riempie il template.
- `ui:component`: definisce un componente creato e aggiunto nell'albero.

---

## [WebSocket]
I **WebSocket** superano i limiti del modello **HTTP** quando è necessaria una comunicazione **bidirezionale**.  
Esistono diverse tecniche tradizionali per simulare la comunicazione bidirezionale, ma presentano vari limiti:

### Tecniche tradizionali
1. **Polling**  
   Il cliente fa polling a intervalli prefissati e il server risponde immediatamente.  
   Soluzione ragionevole quando la periodicità è nota e costante, ma inefficiente se il server non ha dati da trasferire.

2. **Long Polling**  
   Il cliente manda una richiesta iniziale e il server attende fino a che ha dati da inviare.  
   Quando il cliente riceve la risposta, reagisce mandando immediatamente una nuova richiesta.  
   Ogni request/response si appoggia a una nuova connessione.

3. **Streaming / Forever Response**  
   Il cliente manda una richiesta iniziale e il server attende fino a che ha dati da inviare.  
   Il server risponde con uno streaming su una connessione mantenuta sempre aperta per aggiornamenti push (risposte parziali).  
   - **Half-duplex**: dati trasmessi in una sola direzione alla volta.
   - I proxy intermedi potrebbero avere difficoltà a gestire risposte parziali.

4. **Multiple Connections**  
   Usa il long polling su due connessioni HTTP separate:
   - Una connessione per il long polling tradizionale.
   - Una connessione per i dati inviati dal cliente verso il server.  
   Presenta complessità di coordinazione e overhead per le due connessioni per ogni cliente.

---

## [Principali caratteristiche dei WebSocket]
1. **Bidirezionali**: client e server possono scambiarsi messaggi in qualsiasi momento.
2. **Full-duplex**: dati trasmessi contemporaneamente in entrambe le direzioni.
3. **Unica connessione long running**: la connessione rimane aperta per tutta la durata della comunicazione.
4. **Upgrade di HTTP**: non introduce un nuovo protocollo.
5. **Uso efficiente di banda e CPU**.

---

## [Elementi base dei WebSocket]
1. **Handshake**: il cliente avvia la connessione e il server risponde accettandola.
2. Entrambi gli endpoint vengono notificati che la socket è aperta.
3. Entrambi gli endpoint possono inviare messaggi e chiudere la socket in qualsiasi momento.

### Frame
I dati possono essere frammentati in più **frame**.

- **Client**: utilizza **JavaScript**.
- **Server**: utilizza **programmazione JEE** (uso di annotazioni).

---

## [API dei WebSocket]
Le API permettono di gestire il ciclo di vita e la comunicazione tramite eventi e messaggi:

### Eventi principali
1. **onOpen**: chiamato quando la connessione viene aperta.
2. **onClose**: chiamato quando la connessione viene chiusa.
3. **onError**: chiamato quando si verifica un errore.
4. **onMessage**: chiamato quando arriva un messaggio.
5. **send**: invia un messaggio.

### Sessione
Si può utilizzare la sessione con `session.getBasicRemote`, che restituisce un endpoint utile per l'invio di messaggi (tramite `sendText`, `sendBinary` o `sendPing`).  
Tramite encoder e decoder si possono inviare e ricevere messaggi **testuali** o **binary**, serializzando o deserializzando **JSON** o **XML** in oggetti Java.

---

## [Costruttore e API JavaScript]
### Costruttore WebSocket
```javascript
WebSocket(url, [protocols])
