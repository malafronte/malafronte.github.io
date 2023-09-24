---
title: "VS Code Front-end Web Development"
linkTitle: "VS Code Front-end Web Dev"
date: 2023-09-17T16:37:46+02:00
draft: false
type: docs
description: " "
noindex: false
comments: false
nav_weight: 40
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

## Visual Studio Code

L'installazione di Visual Studio Code è già stata descritta su questo sito [qui]({{< ref "/docs-quarta/dev-tools/vs-code-installation" >}} "Installazione di Visual Studio Code"). In questa sezione si vedrà come configurarlo per lo sviluppo web con le tecnologie HTML, CSS e JavaScript.

## Primi passi con Visual Studio Code (VS Code)

Nel corso del programma di quarta l'utilizzo di VS Code è stato piuttosto limitato (utilizzato come semplice editor di testo, oppure come editor predefinito per Git). Nell'ambito del programma di quinta VS Code è usato in maniera molto più estesa poiché è uno strumento molto versatile e potente per lo sviluppo web. VS Code nasce come editor di testo che, grazie ad una miriade di plugin è in grado di diventare un potente IDE (Integrated Development Environment) per tantissimi linguaggi.  
In VS Code viene utilizzato il concetto di workspace al posto di quello di soluzione e di progetto, tipico di altri IDE, come ad esempio, Visual Studio 2022.  

La documentazione di VS Code è molto dettagliata ed estesa. Di seguito si riportano alcune pagine della documentazione particolarmente utili:

* [Introductory Videos](https://code.visualstudio.com/docs/getstarted/introvideos). Una serie di brevi video che spiegano come configurare velocemente VS Code per le attività più comuni
* [Tips and tricks](https://code.visualstudio.com/docs/getstarted/tips-and-tricks). Un elenco di suggerimenti e scorciatoie per adattare VS Code alle proprie esigenze
* [Settings](https://code.visualstudio.com/docs/getstarted/settings). Impostazioni di VS Code
* [Workspace](https://code.visualstudio.com/docs/editor/workspaces). Concetto di VS Code Workspace
* [HTML](https://code.visualstudio.com/docs/languages/html). Supporto di HTML in VS Code
* [Debug](https://code.visualstudio.com/docs/editor/debugging). Aspetti generali relativi al debug in VS Code
* [Javascript Debugging](https://code.visualstudio.com/Docs/languages/javascript#_debugging). Aspetti specifici del debug di codice Javascript
* [Key Bindings for Visual Studio Code](https://code.visualstudio.com/docs/getstarted/keybindings). Impostazioni e scorciatoie da tastiera per VS Code. Nella documentazione è anche disponibile la [tabella riassuntiva delle scorciatoie predefinite per Windows](https://go.microsoft.com/fwlink/?linkid=832145)
* [Node.js debugging in VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

## Workspace[^1]

*A Visual Studio Code "workspace" is the collection of one or more folders that are opened in a VS Code window (instance). In most cases, you will have a single folder opened as the workspace but, depending on your development workflow, you can include more than one folder, using an advanced configuration called Multi-root workspaces.*

*The concept of a workspace enables VS Code to:*

* *Configure settings that only apply to a specific folder or folders but not others.*
* *Persist [task](https://code.visualstudio.com/docs/editor/tasks) and [debugger launch](https://code.visualstudio.com/docs/editor/debugging) configurations that are only valid in the context of that workspace.*
* *Store and restore UI state associated with that workspace (for example, the files that are opened).*
* *Selectively enable or disable extensions only for that workspace.*

*You may see the terms "folder" and "workspace" used interchangeably in VS Code documentation, issues, and community discussions. Think of a workspace as the root of a project that has extra VS Code knowledge and capabilities.*

> **Note:** *It is also possible to open VS Code without a workspace. For example, when you open a new VS Code window by selecting a file from your platform's **File** menu, you will not be inside a workspace. In this mode, some of VS Code's capabilities are reduced but you can still open text files and edit them.*

## How do I open a VS Code "workspace"?

*The easiest way to open a workspace is using the **File** menu and selecting one of the available folder entries for opening. Alternatively if you launch VS Code from a terminal, you can pass the path to a folder as the first argument to the `code` command for opening.*

## Single-folder workspaces

*You don't have to do anything for a folder to become a VS Code workspace other than open the folder with VS Code. Once a folder has been opened, VS Code will automatically keep track of things such as your open files and editor layout so the editor will be as you left it when you reopen that folder. You can also add other folder-specific configurations such as workspace-specific [settings](https://code.visualstudio.com/docs/getstarted/settings) (versus global user settings), [task definitions](https://code.visualstudio.com/docs/editor/tasks), and [debugging launch](https://code.visualstudio.com/docs/editor/debugging) files.*

## VS Code User Settings

*VS Code provides several different scopes for [settings](https://code.visualstudio.com/docs/getstarted/settings). When you open a workspace, you will see at least the following two scopes:  
User Settings - Settings that apply globally to any instance of VS Code you open.  
Workspace Settings - Settings stored inside your workspace and only apply when the workspace is opened.*

### User settings

La configurazione delle impostazioni (Settings) si può fare con:  
Settings Editor: **File** > **Preferences** > **Settings**, oppure con la scorciatoia (`Ctrl+,`).  
Aprendo direttamente il file `settings.json`:  **Command Palette** (`Ctrl+Shift+P`) e poi scrivendo `Preferences: Open User Settings (JSON)`

## Configurazione di Visual Studio Code per lo sviluppo web

VS Code fornisce già un supporto di base allo [sviluppo di pagine HTML](https://code.visualstudio.com/docs/languages/html) e al [debug nel browser](https://code.visualstudio.com/docs/nodejs/browser-debugging), tuttavia per avere un'esperienza di sviluppo più professionale conviene installare alcuni plugin.  
*Install an extension to add more functionality. Go to the **Extensions** view (Ctrl+Shift+X) and type `html` to see a list of relevant extensions to help with creating and editing HTML.:*  
In particolare è opportuno installare:  

* [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css)
* [IntelliSense for CSS class names in HTML](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion)
* [Microsoft Edge Tools for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-edgedevtools.vscode-edge-devtools)  
  * [Documentazione di Microsoft Edge Tools for VS Code](https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/landing/)
* [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)

## Installazione di Node.js

Sebbene non sia strettamente necessario, conviene installare anche Node.js per poter effettuare l'esecuzione e il debug del codice JavaScript direttamente il locale, senza doverlo necessariamente caricare nel browser.
L'installazione di Node.js si effettua facilmente scaricando l'installer dalla [pagina di download di Node.js](https://nodejs.org/en/download). L'integrazione di Node.js in VS Code è molto semplice, come mostrato nel [tutorial per Node.js di VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## Ciao Mondo! da una pagina web in VS Code

Si supponga di voler creare un progetto web (con HTML, CSS e JS) in una cartella chiamata esempio1. È possibile creare la cartella direttamente da VS Code, oppure crearla dalla console e poi richiamare VS Code come indicato negli [esempi di utilizzo di VS Code dalla command line](https://code.visualstudio.com/docs/getstarted/tips-and-tricks#_command-line) della documentazione.  

![Start VS Code from PowerShell](RunVSCodeFromConsole.png#center)

La cartella su cui è stato aperto VS Code è il workspace corrente.

### Creazione di un nuovo file

Per creare un file in VS Code ci sono diversi modi:  

* con la combinazione di tasti `Ctrl+n` (nuovo file), seguita dalla combinazione `Ctrl+s` (salva il file corrente)
* selezionando l'icona `new file` nel file explorer accanto alla cartella del workspace  
  ![New file in VS Code](NewFileInVSCode.png#center)

## Snippets in VS Code

Per scrivere il codice di una pagina HTML è possibile partire da uno [snippet](https://code.visualstudio.com/docs/editor/userdefinedsnippets) di pagina che corrisponde ad una sorta di modello di partenza che solleva lo sviluppatore dallo scrivere alcune parti di codice ripetitive. In realtà esistono snippets per diversi linguaggi e per attivarli in VS Code basta digitare `Ctrl+space` e poi scrivere il tipo di snippet che si vuole richiamare. Nel caso di una pagina HTML per attivare lo snippet che crea il modello di pagina HTML si può digitare `Ctrl+space` e poi digitare `HTML:5`, oppure iniziare a scrivere direttamente `html` e poi dovrebbero apparire automaticamente gli snippet relativi.  

![HTML Snippets](HTMLSnippetsInVSCode.png#center)

Lo snippet relativo ad `HTML:5` produce come risultato il codice seguente:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

## Emmet in Visual Studio Code[^2]

Emmet è uno strumento integrato nell'editor di VS Code che semplifica e velocizza di molto la digitazione del codice.  

*Support for [Emmet](https://emmet.io/) snippets and expansion is built right into Visual Studio Code, **no extension required**. [Emmet 2.0](https://code.visualstudio.com/blogs/2017/08/07/emmet-2.0) has support for the majority of the [Emmet Actions](https://docs.emmet.io/actions/) including expanding [Emmet abbreviations and snippets](https://docs.emmet.io/cheat-sheet/).*

### How to expand Emmet abbreviations and snippets

*Emmet abbreviation and snippet expansions are enabled by default in `html`, `haml`, `pug`, `slim`, `jsx`, `xml`, `xsl`, `css`, `scss`, `sass`, `less` and `stylus` files, as well as any language that inherits from any of the above like `handlebars` and `php`.*

*When you start typing an Emmet abbreviation, you will see the abbreviation displayed in the suggestion list. If you have the suggestion documentation fly-out open, you will see a preview of the expansion as you type. If you are in a stylesheet file, the expanded abbreviation shows up in the suggestion list sorted among the other CSS suggestions.*  

Quando si ha molto testo su una riga è possibile fare il **wrap del testo** con la combinazione `alt+z`, oppure dalla Command Palette `(ctrl+shift+p)`, digitando **View: Toggle Word Wrap**.

## Utilizzo di Live Server

[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) è un plugin di VS Code che permette di avere un server HTTP direttamente all'interno di VS Code e di lanciare nel browser le pagine HTML di cui si vuole vedere la preview. Live Server permette di vedere il risultato delle modifiche del codice in tempo reale quando è abbinato alla funzione `auto save` di VS Code.  

![Live Server inside VS Code](LiveServerInVSCode.png#center)

Live Server si può attivare anche dal pulsante che si trova in basso a destra in VS Code con la scritta `Go Live`.

![Activate Live Server in VS Code](ActivateLiveServerInVSCode.png)

## Utilizzo di Microsoft Edge Tools for VS Code

[Microsoft Edge Tools](https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/landing/) permette di avviare i Developer Tools di Microsoft Edge direttamente dentro Visual Studio Code.  

![Microsoft Edge Tools in VS Code](EdgeToolsInVSCode.png#center)

## Browser debugging in VS Code

Visual Studio Code supporta il [debug](https://code.visualstudio.com/docs/editor/debugging) per moltissimi linguaggi di programmazione. Per il debug del codice JavaScript si può vedere la sezione [debugging](https://code.visualstudio.com/Docs/languages/javascript#_debugging) nella documentazione relativa alla [programmazione in JavaScript](https://code.visualstudio.com/Docs/languages/javascript), oppure, per il debug del codice eseguito nel browser, la documentazione relativa a [Browser debugging in VS Code](https://code.visualstudio.com/docs/nodejs/browser-debugging).  
Per il debug del codice Javascript nel browser, basta aprire la sezione Debug e cliccare sul pulsante Debug. Si apre un menu che permette di scegliere il tipo di debug che si vuole eseguire.  

![Debug in VS Code](DebugInVSCode1.png#center)  

Ad esempio per eseguire il debug con Chrome, basta selezionare la voce `Web App (Chrome)`.  

![Debug with Chrome](DebugInVSCode2.png#center)

### Browser Debug con codice JavaScript

Nel caso di pagine web che contengono anche codice JavaScript il debug può essere fatto in diversi modi:  

1. direttamente all'interno di VS Code  
2. lanciando una sessione di debug remota all'interno di un browser (Chrome, Edge, etc.).  

Si supponga di avere un progetto web con i file HTML, CSS, JS come nell'esempio seguente:  
{{< bs/toggle name=esempio1 style=tabs >}}

  {{< bs/toggle-item HTML >}}
    {{< highlight html >}}
<!-- file: index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset='utf-8'>
    <title>Page Title</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <link rel='stylesheet' type='text/css' media='screen' href='main.css'>
    <script src='main.js'></script>
</head>
<body>
    <h1>Questa è una intestazione h1</h1>
    <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Architecto, at sit tenetur amet, neque dolorum assumenda illum corporis obcaecati dolores aspernatur dignissimos. Eveniet sed, voluptatum pariatur a deserunt explicabo repudiandae mollitia quasi illum voluptatem voluptate vero. Ex doloribus quaerat odit quod obcaecati repellendus tempore at, facilis aspernatur natus, quos dolore?</p>
</body>
</html>
    {{< /highlight >}}
  {{< /bs/toggle-item >}}

  {{< bs/toggle-item CSS >}}
    {{< highlight css >}}
/*file: main.css*/
body {
  background-color: linen;
}

h1 {
  color: maroon;
  margin-left: 40px;
}
    {{< /highlight >}}
  {{< /bs/toggle-item >}}
  
  {{< bs/toggle-item JS >}}
    {{< highlight js >}}
// file: main.js
console.log("Hello World");
var uno = true;
if (uno == true) {
  console.log("uno is true");
}
    {{< /highlight >}}
  {{< /bs/toggle-item >}}

{{< /bs/toggle >}}

Per effettuare il debug del codice si può procedere come segue:  

* Si seleziona il file html che si intende lanciare, `index.html` nell'esempio.
* Si seleziona nel menu principale di VS Code il tab relativo al debug e si clicca sul pulsante **Run and Debug**
* La prima volta che si clicca sul pulsante **Run and Debug** viene chiesto quale browser utilizzare; le volte successive viene ricordata la scelta precedente. Ad esempio, se si sceglie Chrome, il file HTML verrà aperto in nel browser selezionato.

Per effettuare il debug in maniera efficiente conviene creare una configurazione di debug.

### Configurazione di debug

Per creare una configurazione di debug si può cliccare sul link che si trova sotto al pulsante di debug e poi selezionare `Web App (Chrome)`.  

![Debug Configuration in VS Code](DebugConfigInVSCode.png#center)

Oppure, in alternativa, si può cliccare su `Show all automatic configurations` e poi scegliere il tipo di configurazione di debug richiesta. Supponendo di scegliere `Web App (Chrome)`, viene creato il file `launch.json` nella cartella `.vscode` all'interno del workspace.  
Il file creato è qualcosa di simile a quello riportato di seguito:  

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Open index.html",
            "file": "c:\\Users\\genna\\source\\repos\\5IA\\web-examples\\esempio1\\index.html"
        }
    ]
}
```

I dettagli della configurazione sono ben descritti nella sezione [launch configurations](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations) della documentazione di VS Code. Se si prova a cliccare sul menu di debug, si vede che la schermata di debug è cambiata e adesso c'è un pulsante verde a forma di freccia che permette di eseguire il contenuto del file `index.html`.

![Run or Debug in VS Code](RunDebugInVSCode.png#center)

Cliccando sul pulsante a forma di freccia verde (pulsante di debug), oppure premendo il tasto F5 della tastiera, viene aperto il browser Chrome con la pagina `index.html`.  
> **Nota:** digitando il tasto `ctrl +F5` viene avviato il Run al posto del Debug. La differenza tra Run e Debug è la stessa già vista con Visual Studio:
>
>* il Debug fa partire l'applicazione e la connette al debugger con la possibilità di fermare l'esecuzione del codice in un punto, vedere lo stato delle variabili, etc.
>* il Run fa partire l'applicazione, senza collegarla al debugger.

![Running index.html in Chrome](RunningHtmlPageInChrome.png#center)

Cliccando sul pulsante a forma di ingranaggio nella sezione Run and Debug è possibile aprire il file `launch.json` e cliccando sul bottone **Add Configuration** è possibile aggiungere altre configurazioni.

![Add configurations in VS Code](AddConfigurationsInVSCode.png#center)

La configurazione appena mostrata permette di eseguire il debug del file `index.html` all'interno del browser Chrome, tuttavia è specifica per un singolo file. Per realizzare una configurazione che permetta di effettuare il Run e Debug di un qualunque file all'interno del workspace, occorre procedere diversamente:

* Se il file `launch.json` esiste già, si clicca sul pulsante **Add Configuration** e poi si scrive il tipo di configurazione che si vuole ottenere. Per esempio, per eseguire i file HTML, CSS e JS in Chrome, basta scrivere `chrome` e poi accettare lo snippet di configurazione con il tasto `tab`.
  ![Add Configuration in launch.json file of VS Code](AddConfInLaunchJsonFileVSCode.png#center)
  Il risultato è un file `json` con i seguenti valori:

  ```json
  {
      // Use IntelliSense to learn about possible attributes.
      // Hover to view descriptions of existing attributes.
      // For more information, visit: <https://go.microsoft.com/fwlink/?linkid=830387>
      "version": "0.2.0",
      "configurations": [
          {
              "name": "Launch Chrome",
              "request": "launch",
              "type": "chrome",
              "url": "http://localhost:8080",
              "webRoot": "${workspaceFolder}"
          },
          {
              "type": "chrome",
              "request": "launch",
              "name": "Open index.html",
              "file": "c:\\Users\\genna\\source\\repos\\5IA\\web-examples\\esempio1\\index.html"
          }
      ]
  }
  ```

  Come si vede, è stata aggiunta una configurazione di nome `Launch Chrome` e se si prova a mandare in debug l'applicazione si vedrà che ci sono due possibilità:  

  ![Multiple launch configurations in VS Code](MultipleLaunchConfigurationsInVSCode.png)

  Se si lancia la configurazione `Launch Chrome` si vedrà che in realtà la pagina corrispondente a `index.html` non viene comunque caricata.

  ![Page not loaded in VS Code](PageNotLoadedInVSCode.png#center)

  Il motivo del mancato caricamento sta nel fatto che con la configurazione utilizzata il file `index.html` non è caricato dal file system, ma attraverso l'url `http://localhost:8080`. Ciò presuppone che ci sia un HTTP Server che risponda sul `localhost` alla porta `8080`. Per far funzionare questa configurazione occorre che ci sia un HTTP Server in ascolto sulla porta indicata nel file `launch.json`. Ci sono diverse soluzioni che si possono utilizzare, ma la più semplice è quella di attivare il **Live Server** che permette di servire in http le pagine direttamente dal workspace di VS Code.  
  Quando **Live Server** parte, è in ascolto sulla porta 5500 per impostazione predefinita, ma è possibile configurarlo per mettersi in ascolto su un numero di porta differente, oppure per fare in modo che ogni volta scelga una porta a caso. Di seguito si riportano le schermate relative alla configurazione di **Live Server**:  

  ![Live Server Configuration in VS Code - fig. 1](LiveServerConfigurationInVSCode1.png#center)

  ![Live Server Configuration in VS Code - fig. 2](LiveServerConfigurationInVSCode2.png#center)

  Supponendo di voler scrivere una configurazione di debug che permetta di eseguire il debug di una qualsiasi pagina HTML, CSS, JS all'interno del workspace, avendo come server HTTP il **LIve Server** in ascolto sulla porta 5500, il file `launch.json` diventa:

  ```json
  {
      // Use IntelliSense to learn about possible attributes.
      // Hover to view descriptions of existing attributes.
      // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
      "version": "0.2.0",
      "configurations": [
          {
              "name": "Launch Chrome",
              "request": "launch",
              "type": "chrome",
              "url": "http://localhost:5500",
              "webRoot": "${workspaceFolder}"
          }
      ]
  }
  ```

  Con questa configurazione, quando il **Live Server** è in ascolto sulla porta `5500` e si lancia la configurazione di debug `Launch Chrome` la pagina `index.html` verrà correttamente messa in debug, e se fossero presenti dei breakpoint nel codice JavaScript, l'esecuzione si fermerà proprio sul primo breakpoint quando l'esecuzione arriverà all'istruzione corrispondente.  

  ![Successful execution of a page in Chrome](SuccessfulExecutionOfPageInChromeVSCode.png#center)

  Esiste anche una configurazione specifica per Edge:

  ```json
  {
      // Use IntelliSense to learn about possible attributes.
      // Hover to view descriptions of existing attributes.
      // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
      "version": "0.2.0",
      "configurations": [
          {
              "name": "Launch Edge",
              "request": "launch",
              "type": "edge",
              "url": "http://localhost:5500",
              "webRoot": "${workspaceFolder}"
          },
          {
              "name": "Launch Chrome",
              "request": "launch",
              "type": "chrome",
              "url": "http://localhost:5500",
              "webRoot": "${workspaceFolder}"
          }
      ]
  }
  ```

* Se il file `launch.json` non è ancora stato creato nella cartella `.vscode`, è possibile crearlo digitando la combinazione di tasti `Ctrl+p`, e scrivendo il termine `debug`. Apparirà un elenco di possibili configurazioni. Cliccando su `Add Configuration` è possibile creare un file di configurazione in `.vscode/launch.json`. Ad esempio, selezionando `Chrome` verrà creata la stessa configurazione vista nelle sezioni precedenti.

## Debug di codice Javascript con Node.js

Il codice Javascript può essere eseguito e/o sottoposto a debug anche se non è caricato all'interno di un browser. Infatti, se si è installato Node.js è possibile eseguirlo direttamente in VS Code, dal momento che VS Code ha un [supporto nativo per Node.js](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).  
L'esecuzione del codice JavaScript da VS Code può essere fatto ricorrendo alla funzione di [Auto Attach](https://code.visualstudio.com/docs/nodejs/nodejs-debugging#_auto-attach), lanciando dal terminale di VS Code l'esecuzione di node su un file contenente codice JavaScript:  

![Node.js Auto Attach in VS Code](AutoAttachNodeJsVSCode.png#center)

![Node.js Auto Attach in VS Code](AutoAttachNodeJsVSCode2.png#center)

l'impostazione `Auto Attach` si può configurare dalla **Command Palette** (`Ctrl+Shift+P`), digitando `Debug: Toggle Auto Attach`, oppure dalla barra di stato in basso.

Anche per Node.js è possibile creare una configurazione di debug al'interno del file `launch.json`. Basta aggiungere un'altra configurazione e scegliere `Node.js: Launch Program`. Il file `launch.json` diventa:  

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
      {
        "name": "Launch Program",
        "program": "${workspaceFolder}/main.js",
        "request": "launch",
        "skipFiles": ["<node_internals>/**"],
        "type": "node"
      },
      {
        "name": "Launch Edge",
        "request": "launch",
        "type": "edge",
        "url": "http://localhost:5500",
        "webRoot": "${workspaceFolder}"
      },
      {
        "type": "chrome",
        "request": "launch",
        "name": "Launch Chrome against localhost",
        "url": "http://localhost:5500",
        "webRoot": "${workspaceFolder}"
      }
    ]
}
```

Con questa configurazione si può lanciare il file `main.js` direttamente dal menu di debug, selezionando la configurazione appropriata:  

![Launch Program - Node.js in VS Code](LaunchProgramNodeJsInVSCode.png)

[^1]: [Workspaces](https://code.visualstudio.com/docs/editor/workspaces)
[^2]: [Emmet in Visual Studio Code](https://code.visualstudio.com/docs/editor/emmet)
