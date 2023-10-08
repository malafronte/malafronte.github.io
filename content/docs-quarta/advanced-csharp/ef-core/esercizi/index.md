---
title: "Esercizi"
# linkTitle:
date: 2023-09-30T19:11:58+02:00
draft: false
type: docs
description: "Esercitazioni guidate sulla creazione di database e sull'esecuzione di query da programma C#, mediante LINQ"
noindex: false
comments: false
nav_weight: 90
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

## Introduzione

In questa sezione verranno mostrati alcuni esercizi completamente svolti relativamente alla creazione di un database mediante EF Core e all'esecuzione di query con il linguaggio LINQ, nella variante Fluent Method.

## Database *Romanzi*

<a class="btn btn-primary" href="https://github.com/malafronte/malafronte-doc-samples/tree/main/samples-quarta/EFCore/Romanzi" role="button">{{< icons/icon vendor=bootstrap name=github height=1em width=1em >}}&nbsp; Ottieni il codice</a>

1. Creare un progetto per un’applicazione `Console .Net Core`, chiamata `Romanzi` che utilizzi il Framework EF Core per implementare il database (`Romanzi.db`) con le seguenti classi:  

   **Autore**`(`<ins>`AutoreId`</ins>`, Nome, Cognome, Nazionalità )`  

   **Romanzo**`(`<ins>`RomanzoId`</ins>`, Titolo, AutoreId*, AnnoPubblicazione )`

   **Personaggio**`(`<ins>`PersonaggioId`</ins>`, Nome, RomanzoId*, Sesso, Ruolo )`  

    Dove l’attributo sottolineato rappresenta la `Primary Key`, mentre l’attributo con l’asterisco rappresenta una `Foreign Key`.  
    Assumere che:  
    * `Nome`, `Cognome`, `Nazionalità` nella la classe `Autore` siano di tipo `string`  
    * `Titolo` in `Romanzo` sia di tipo `string`;  
    * `AnnoPubblicazione` in `Romanzo` sia di tipo `int`  
    * `Nome`, `Sesso`, `Ruolo` in `Personaggio` siano di tipo `string`  
    >Nota N1: creare un progetto Console denominato `Romanzi` da Visual Studio  
    >Nota N2: installare le librerie per l’utilizzo di EF Core con SQLite (dalla Packet Manager Console di Visual Studio):

    ```ps1
    Install-Package Microsoft.EntityFrameworkCore.Tools
    Install-Package Microsoft.EntityFrameworkCore.Sqlite
    ```  
  
2. Creare la cartella `Model` con dentro le classi del modello dei dati  
3. Creare la cartella `Data` con al suo interno la classe `RomanziContext`  
   >Nota N3: la classe `RomanziContext` discende da `DbContext` ed espone una property per ciascuna tabella del database, come esemplificato di seguito:  

   ```cs
    public DbSet<NomeClasse> NomeTabella { get; set; }  
   ```

   >Nota N4: la classe `RomanziContext` potrebbe, se richiesto, implementare i metodi:

   ```cs  
   protected override void OnConfiguring(DbContextOptionsBuilder options);  
   protected override void OnModelCreating(ModelBuilder modelBuilder);  
   ```

4. Effettuare la migration del database e la creazione fisica del file di database di SQLite:  

   ```ps1
    Add-Migration InitialCreate
    Update-Database
   ```

5. Creare un metodo `static void PopulateDB()` nella classe `Program` che inserisce:  
Almeno 5 autori di nazionalità compresa tra "Americana", "Belga", "Inglese".  
Almeno 10 romanzi degli autori precedentemente inseriti  
Almeno 5 personaggi presenti nei romanzi precedentemente inseriti.

### Query sul database *Romanzi*

Q1: creare un metodo che prende in input la nazionalità e stampa gli autori che hanno la nazionalità specificata  
Q2: creare un metodo che prende in input il nome e il cognome di un autore e stampa tutti i romanzi di quell’autore  
Q3: creare un metodo che prende in input la nazionalità e stampa quanti romanzi di quella nazionalità sono presenti nel database  
Q4: creare un metodo che per ogni nazionalità stampa quanti romanzi di autori di quella nazionalità sono presenti nel database  
Q5: creare un metodo che stampa il nome dei personaggi presenti in romanzi di autori di una data nazionalità  

Seguendo lo stesso procedimento visto negli esempi precedenti si creino i file relativi alle classi `Romanzo`, `Autore`, `Personaggio` nella cartella `Model`:  
File `Romanzo.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Romanzi/Romanzi/Model/Romanzo.cs" >}}  

File `Autore.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Romanzi/Romanzi/Model/Autore.cs" >}}  

File `Personaggio.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Romanzi/Romanzi/Model/Personaggio.cs" >}}  

Si installino i pacchetti necessari per l'utilizzo di EF Core con SQLite, utilizzando la PMC di Visual Studio:  

```ps1
    Install-Package Microsoft.EntityFrameworkCore.Tools
    Install-Package Microsoft.EntityFrameworkCore.Sqlite
```  

Si crei la cartella `Data` con dentro il file `RomanziContext.cs`, relativo alla definizione della classe `RomanziContext`:  
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Romanzi/Romanzi/Data/RomanziContext.cs" >}}  

Dalla PCM di Visual Studio si eseguano i comandi per la generazione della migrazione e la creazione del database.

```ps1
    Add-Migration InitialCreate
    Update-Database
```

La struttura del progetto dovrebbe essere simile a quella riportata nella figura seguente:

![Struttura Progetto Romanzi](ProgettoRomanzi-Struttura-File.png#center)
Si scriva nel file `Program.cs` il codice relativo a:

* Popolamento del database
* Gestione dell'input
* Esecuzione delle query richieste dalla traccia  

File `Program.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Romanzi/Romanzi/Program.cs" >}}

## Database *Università*

<a class="btn btn-primary" href="https://github.com/malafronte/malafronte-doc-samples/tree/main/samples-quarta/EFCore/Universit%C3%A0" role="button">{{< icons/icon vendor=bootstrap name=github height=1em width=1em >}}&nbsp; Ottieni il codice</a>

Creare un’applicazione `.Net Core` per la gestione dei corsi di laurea universitari.
Usando il framework EF Core creare il database dei corsi di laurea e degli studenti, sapendo che nel model sono previste le seguenti classi:  

**Studente** `(`<ins>`Matricola`</ins>`, Nome, Cognome, CorsoLaureaId*, AnnoNascita )`  
**CorsoLaurea**`(`<ins>`CorsoLaureaId`</ins>`, TipoLaurea, Facoltà )`  
**Frequenta** `(`<ins>`Matricola*`</ins>,<ins>`CodCorso*`</ins> `)`  
**Corso** `(`<ins>`CodiceCorso`</ins>`, Nome, CodDocente* )`  
**Docente**`(`<ins>`CodDocente`</ins>`, Nome, Cognome, Dipartimento )`  
Dove:
Facoltà è un enumerativo che contiene i valori: `Medicina`, `Ingegneria`, `MatematicaFisicaScienze`, `Economia`  
`Dipartimento` è un enumerativo che contiene i valori: `Matematica`, `Fisica`, `Economia`, `IngegneriaCivile`, `IngegneriaInformatica`, `Medicina`.  
`TipoLaurea` è un enumerativo che contiene i valori: `Triennale`, `Magistrale`  
Nello schema precedente si è usata la convenzione di indicare la chiave primaria con attributi sottolineati, mentre la chiave esterna è indicata con un asterisco.

La creazione del modello e del DBContext procede in maniera del tutto analoga a quanto già visto negli esempi precedenti.  
Per il data Model i file sono:  
File `Corso.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Model/Corso.cs" >}}
File `CorsoLaurea.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Model/CorsoLaurea.cs" >}}
File `Docente.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Model/Docente.cs" >}}
File `Frequenta.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Model/Frequenta.cs" >}}
File `SharedEnums.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Model/SharedEnums.cs" >}}
File `Studente.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Model/Studente.cs" >}}

Il DbContext è definito attraverso il file `UniversitàContext.cs` relativo alla definizione della classe `UniversitàContext`:  
File `UniversitàContext.cs`:  
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Data/Universit%C3%A0Context.cs" >}}

### Query sul database Università

Dopo aver fatto la Migration del database, si scriva il codice delle query seguenti:  
Q1: Stampare l'elenco degli studenti  
Q2: Stampare l'elenco dei corsi  
Q3: Modificare il docente di un corso di cui è noto l’id  
Q4: Stampare il numero di corsi seguiti dallo studente con id = 1  
Q5: Stampare il numero di corsi seguiti dallo studente con Nome="Giovanni" e Cognome ="Casiraghi"  
Q6: Stampare il numero di corsi seguiti da ogni studente  
Q7: Stampare i corsi seguiti dallo studente con Nome="Piero" e Cognome ="Gallo"  

File `Program.cs`:
{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/Universit%C3%A0/Universit%C3%A0/Program.cs" >}}
