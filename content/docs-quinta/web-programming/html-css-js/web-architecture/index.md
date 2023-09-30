---
title: "Web Architecture"
# linkTitle:
date: 2023-09-18T00:24:47+02:00
draft: false
type: docs
description: "Architettura del web"
noindex: false
comments: false
nav_weight: 10
# nav_icon:
#   vendor: bootstrap
#   name: toggles
#   color: '#e24d0e'
authors:
  - malafronte
series:
  - Docs
categories:
#  - 
tags:
#  - 
images:
#  - 
# menu:
#   main:
#     weight: 100
#     params:
#       icon:
#         vendor: bs
#         name: book
#         color: '#e24d0e'
---
<style>p {text-align: justify}</style>
Lo sviluppo di applicazioni web coinvolge diverse tecnologie che concorrono alla creazione di contenuti e servizi. In questa guida vedremo come sono strutturati i siti web statici, ossia quei siti web che contengono pagine create staticamente e che sono realizzate mediante l'utilizzo di HTML, CSS e JavaScript.  

Per prima cosa occorre chiarire lo schema di riferimento di una applicazione web (Web Application). Quando un utente di un host client (computer, dispositivo mobile, etc.) effettua una richiesta di accesso ad una pagina di un sito web, il browser [^1] dell’utente, effettua una richiesta, tramite il protocollo HTTP[^2] (o HTTPS[^3]), di quella pagina al server HTTP che gestisce il sito. Tale server può recuperare la pagina richiesta da uno storage locale (memoria di massa), se si tratta di una pagina statica, oppure può richiedere ad un'altra applicazione sul server, o su un altro host remoto, di generare dinamicamente la pagina. Quest'ultima applicazione server potrebbe essere scritta in ASP.NET, oppure con un'altra tecnologia, come NodeJS, PHP, Go, etc. Una volta che l'HTTP server ha ottenuto la pagina richiesta, la restituisce al browser del client. Il contenuto inviato al browser del client è un file di testo contenente HTML, che può essere accompagnato da altri file contenenti fogli di stile (CSS) e codice scritto tipicamente in JavaScript.  
Tutta la comunicazione tra il browser dell'utente (il client) e l'HTTP Server avviene attraverso il protocollo HTTP per le connessioni non protette da cifratura e con il protocollo HTTPS per le connessioni protette da cifratura.  
![Architettura Client Server](Client-Server.drawio.png#center)  
Quando la pagina richiesta è recuperata direttamente dallo storage locale al server si parla di pagina statica, o in generale di **sito web statico**. Quando la pagina richiesta è invece generata dinamicamente da una applicazione web, ricorrendo all’esecuzione di codice sul server ed eventualmente all’interazione con un database si parla di **pagina dinamica**, o in generale di **sito web dinamico**. Generalmente dal momento in cui avviene la richiesta al momento in cui viene restituita la pagina al client passano pochi millisecondi e l’utente ha la percezione che la pagina sia fornita quasi istantaneamente, sia nel caso di pagine statiche che di pagine dinamiche.
In questa prima parte del corso verranno mostrate le tecnologie per la creazione di pagine web statiche ed in particolare HTML, CSS e Javascript.

* **HTML** è un linguaggio di markup che permette di definire la struttura di una pagina, ossia i componenti costitutivi, ad esempio i paragrafi, i bottoni, i link, e così via.
  
* **CSS** è un linguaggio utilizzato per definire lo stile degli elementi di una pagina HTML, come ad esempio, il colore, la dimensione, il tipo di font da utilizzare etc.
  
* **JavaScript** è un linguaggio di programmazione, che attualmente ha grande importanza nello sviluppo web. Storicamente, in passato, era usato per dare interattività alle pagine web all’interno del browser visto che HTML e CSS non sono dei linguaggi di programmazione (non possono codificare un algoritmo). Attualmente JavaScript è utilizzato anche in applicazioni cosiddette lato server, per effettuare la creazione delle pagine dinamiche sul server (si veda, ad esempio, NodeJS).  
  
In questa prima parte del corso verranno illustrate le tecnologie per generare pagine web statiche che sono costituite da pagine HTML, con fogli di stile CSS e con script JavaScript eseguiti localmente nel browser dell’utente. Lo scenario tipico di utilizzo di un sito web di questo tipo prevede che l’utente richieda una determinata pagina web dal server e che il server risponda inviando al browser dell’utente la pagina HTML richiesta con anche gli elementi CSS e JavaScript necessari.
Esistono diversi server HTTP utilizzati nelle applicazioni commerciali e, tra questi, ricoprono un ruolo preminente i seguenti:

* [Apache Web Server](https://httpd.apache.org/)
  
* [NGINX](https://www.nginx.com/)

Si tratta di software che trovano utilizzo in un numero altissimo di siti web, come si può vedere dalle statistiche di utilizzo descritte nel [NetCraft Survey](https://www.netcraft.com/resources/?topic=web-server-survey).  
Ulteriori chiarimenti su [come funziona Internet](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/How_does_the_Internet_work) e [come funziona il web](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works) si trovano nelle [guide di Mozilla MDN](https://developer.mozilla.org/en-US/docs/Learn).

[^1]:[Wikipedia](https://it.wikipedia.org/wiki/Browser): *In informatica il browser Web (o semplicemente browser /ˈbraʊzə(r)/), in italian navigatore Web,è un'applicazione per l'acquisizione, la presentazione e la navigazione di risorse sul Web. Tali risorse (come pagine web, immagini o video) sono messe a disposizione sul World Wide Web (la rete globale che si appoggia su Internet), su una rete locale o sullo stesso computer dove il browser è in esecuzione. Il programma implementa da un lato le funzionalità di client per il protocollo HTTP, che regola il download delle risorse dai server web a partire dal loro indirizzo URL; dall'altro quelle di visualizzazione dei contenuti ipertestuali (solitamente all'interno di documenti HTML) e di riproduzione di contenuti multimediali (rendering). Tra i browser più utilizzati vi sono Google Chrome, Mozilla Firefox, Microsoft Edge, Safari, Opera e Internet Explorer.*  

[^2]: [Wikipedia](https://it.wikipedia.org/wiki/Hypertext_Transfer_Protocol): *In telecomunicazioni e informatica l'HyperText Transfer Protocol (HTTP) (in italiano: protocollo di trasferimento ipertesto) è un protocollo a livello applicativo usato come principale sistema per la trasmissione d'informazioni sul web ovvero in un'architettura tipica client-server. Le specifiche del protocollo sono gestite dal World Wide Web Consortium (W3C). Un server HTTP generalmente resta in ascolto delle richieste dei client sulla porta 80 usando il protocollo TCP a livello di trasporto.*

[^3]:[Wikipedia](https://it.wikipedia.org/wiki/HTTPS): *In telecomunicazioni e informatica l'HyperText Transfer Protocol over Secure Socket Layer (HTTPS), (anche noto come HTTP over TLS,HTTP over SSL e HTTP Secure) è un protocollo per la comunicazione sicura attraverso una rete di computer utilizzato su Internet. La porta utilizzata generalmente (ma non necessariamente) è la 443. Consiste nella comunicazione tramite il protocollo HTTP (Hypertext Transfer Protocol) all'interno di una connessione criptata, tramite crittografia asimmetrica, dal Transport Layer Security (TLS) o dal suo predecessore, Secure Sockets Layer (SSL) fornendo come requisiti chiave: un'autenticazione del sito web visitato;protezione della privacy (riservatezza o confidenzialità);integrità dei dati scambiati tra le parti comunicanti.*
