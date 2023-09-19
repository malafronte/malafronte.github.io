---
title: "Git Basics"
# linkTitle:
date: 2023-09-17T20:02:50+02:00
draft: false
type: docs
description: " " 
noindex: false
comments: false
nav_weight: 30
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

## Primi passi con Git

In questa sezione verranno mostrati alcuni concetti e comandi di base per iniziare a lavorare con Git e GitHub. Per molti concetti si farà riferimento al corso su Udemy di Colt Steele [The Git & Github Bootcamp](https://www.udemy.com/course/git-and-github-bootcamp/) le cui slide sono liberamente accessibili su [Canva](https://www.canva.com/):

* [Introducing Git](https://www.canva.com/design/DAETQyFE6pM/mLt1oYF8gP_mqBS3ghb-BA/view?utm_content=DAETQyFE6pM&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Learning Git](https://www.canva.com/design/DAEXMu7dx04/x1kkoUK-g_j6UtObmUwbDA/view?utm_content=DAEXMu7dx04&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton#1)  
* [Git Basics](https://www.canva.com/design/DAEPH_Lq4Wk/Wp_d5Rvk_OjVvgPH0xmzhg/view?utm_content=DAEPH_Lq4Wk&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)  
* [More about committing](https://www.canva.com/design/DAEXMibkysc/4PgPWiQqZ5UwCxMruH6BmQ/view?utm_content=DAEXMibkysc&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton#1)
* [Git Branching](https://www.canva.com/design/DAEPOwX2Zzs/90STrbMXNysYIkSsxUCu-g/view?utm_content=DAEPOwX2Zzs&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)  
* [Merging Branches](https://www.canva.com/design/DAEUZEra8W0/b4I77uG1YJAu4q6UOTIG6Q/view?utm_content=DAEUZEra8W0&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)  
* [Comparisons with Git Diff](https://www.canva.com/design/DAEVUT6HslA/tbdbyITzamUidWfk-HcSug/view?utm_content=DAEVUT6HslA&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Git Stashing](https://www.canva.com/design/DAEPsQa6BFE/uNs08sHSGN1XziSUt1BLHQ/view?utm_content=DAEPsQa6BFE&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Undoing Stuff & Time Traveling](https://www.canva.com/design/DAEPZZHOafo/uagxrNdvbI_wDpjfNpK_4w/view?utm_content=DAEPZZHOafo&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Github Basics](https://www.canva.com/design/DAEPtdekgz0/L9rfbid7gCFMGEZBLJcmlw/view?utm_content=DAEPtdekgz0&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Fetching & Pulling](https://www.canva.com/design/DAEPyYicrxQ/EaXIXD_WWryEq7Z7YUSVlg/view?utm_content=DAEPyYicrxQ&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Github Odds & Ends](https://www.canva.com/design/DAEVGSMC0ew/9f6udCe20KrYAfwoD0zqHA/view?utm_content=DAEVGSMC0ew&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Git workflow (for collaboration)](https://www.canva.com/design/DAEP32iZwVc/4se77vKYGwT5_9NpxFbZoQ/view?utm_content=DAEP32iZwVc&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)  
* [Rebasing](https://www.canva.com/design/DAEVkyNcwWI/qt8pRN3JA1lP9ckYeImxeQ/view?utm_content=DAEVkyNcwWI&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Git Tags](https://www.canva.com/design/DAEV5aEpUOQ/lfUIjJz2atC6fGT9KOv2kg/view?utm_content=DAEV5aEpUOQ&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Git Behind The Scenes](https://www.canva.com/design/DAEV-h9bSG4/R6FyldDe8CO8Wfn8z92yRA/view?utm_content=DAEV-h9bSG4&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)  
* [Reflogs](https://www.canva.com/design/DAEWorNx5_Q/piCbRO6BWwv9_ae_mahECA/view?utm_content=DAEWorNx5_Q&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)
* [Git Aliases](https://www.canva.com/design/DAEWcidQeSI/m6VuuSGBgvBNsRcxfweqDA/view?utm_content=DAEWcidQeSI&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)

In aggiunta alle slide del corso di Git ci sono molte altre risorse utili alla comprensione di Git e GitHub. In particolare:

* [GIT CHEAT SHEET](https://education.github.com/git-cheat-sheet-education.pdf) (un riassunto dei comandi principali di Git)
* [Getting started with Git](https://docs.github.com/en/get-started/getting-started-with-git). Guida focalizzata sul [setup iniziale di Git](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git), [caching delle credenziali per connessioni https](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git), [clonazione da remote](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories), [gestione dei remote](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories), [associazione di un editor di testo a Git](https://docs.github.com/en/get-started/getting-started-with-git/associating-text-editors-with-git), [configurazione dei line endings (terminatori di riga) per Git](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings), [gestione del .gitignore file](https://docs.github.com/en/get-started/getting-started-with-git/ignoring-files)
* [The Beginner’s Guide to Git & GitHub](https://docs.github.com/en/get-started/quickstart). Guida focalizzata principalmente sulle funzionalità di GitHub, e tra queste le più interessanti sono [Fork a repo](https://docs.github.com/en/get-started/quickstart/fork-a-repo), [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow), [Contributing to projects](https://docs.github.com/en/get-started/quickstart/contributing-to-projects)
* [GitHub Docs](https://docs.github.com/en). La documentazione compelta di GitHub.

## Connessione a GitHub

Esistono diversi modi per connettere un repository locale ad un remote su GitHub, in particolare:

* Connessione HTTPS
* Connessione SSH
* GitHub CLI (non trattata a lezione)

### Connessione a GitHub mediante SSH

Utilizzando il protocollo SSH, è possibile connettersi e autenticarsi a server e servizi remoti. Con le chiavi SSH,è possibile connettersi a GitHub senza fornire il proprio nome utente e il token di accesso personale ad ogni visita. È inoltre possibile utilizzare una chiave SSH per firmare le commit.  
Per effettuare una connessione autenticata SSH occorre, prima di tutto, creare una coppia di chiavi (pubblica e privata) SSH, come descritto nella guida [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), successivamente bisogna aggiungere la chiave pubblica al proprio profilo GitHub, come descritto nella guida [Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).  

Qundo si genera una coppia di chiavi SSH viene richiesta una password per proteggere l'utilizzo della chiave da persone non autorizzate, inoltre la password va associata all'ssh agent in modo che questo possa usare la chiave corretta tutte le volte che ci si connette a GitHub. La prima volta che ci si connette a GitHub l'ssh agent chiede se ci si vuole davvero connettere a GitHub e se si vuole aggiungere l'host all'elenco degli host noti. In questo coso bisogna rispondere affermativamente. Il risultato di questa scelta è che l'ssh agent crea alcune righe nel file known_hosts con il dominio di GitHub e alcune informazioni estratte della chiave ssh utilizzata.  
In alcune situazioni si potrebbe riscontrare un problema con la connessione SSH, con un errore del tipo `ssh: Could not resolve hostname github.com`, come descritto [qui](https://stackoverflow.com/a/9393431). In questo caso, la soluzione potrebbe essere semplicemente quella di aggiornare la cache del DNS con `ipconfig /flushdns` in Windows.

