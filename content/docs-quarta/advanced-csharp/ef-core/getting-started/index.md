---
title: "Getting started"
# linkTitle:
date: 2023-09-30T19:07:56+02:00
draft: false
type: docs
description: "Entity Framework (EF) Core overview and first examples"
noindex: false
comments: false
nav_weight: 20
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

## Overview

Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/dotnet/efcore) and cross-platform version of the popular Entity Framework data access technology[^1].

EF Core can serve as an object-relational mapper (O/RM), which:

* Enables .NET developers to work with a database using .NET objects.
* Eliminates the need for most of the data-access code that typically needs to be written.  

EF Core supports many database engines, see [Database Providers](https://learn.microsoft.com/en-us/ef/core/providers/) for details.

## The model

With EF Core, data access is performed using a model. A model is made up of entity classes and a context object that represents a session with the database. The context object allows querying and saving data. For more information, see [Creating a Model](https://learn.microsoft.com/en-us/ef/core/modeling/).

EF supports the following model development approaches:

* Generate a model from an existing database.
* Hand code a model to match the database.
* Once a model is created, use [EF Migrations]([xref:core/managing-schemas/migrations/index](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/)) to create a database from the model. Migrations allow evolving the database as the model changes.  
  
A titolo illustrativo si riporta di seguito un esempio di uso del framework EF Core. Alcuni dettagli saranno chiariti in seguito.

```cs
using System.Collections.Generic;
using Microsoft.EntityFrameworkCore;

namespace Intro;
//definizione del DbContext
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(
          //questa è la stringa di connessione: varia in base al tipo di database utilizzato e alle configurazioni di accesso - maggiori dettagli in seguito...
            @"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True");
    }
}
//definizione del data model
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## Un esempio di come si usa EF Core

### Querying

Instances of your entity classes are retrieved from the database using [Language Integrated Query (LINQ)](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/). For more information, see [Querying Data](https://learn.microsoft.com/en-us/ef/core/querying/).
{{< bs/alert >}}
{{< markdownify >}}
Una volta associato il modello al database è possibile effettuare interrogazioni sui dati (queries) usando il linguaggio LINQ
{{< /markdownify >}}
{{< /bs/alert >}}

```cs
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

### Saving data

Data is created, deleted, and modified in the database using instances of your entity classes. See [Saving Data](https://learn.microsoft.com/en-us/ef/core/saving/) to learn more.  
{{< bs/alert >}}
{{< markdownify >}}
Il salvataggio, la cancellazione o la modifica dei dati nel database sono realizzati mediante operazioni su oggetti di entità (classi di oggetti che si mappano sulle righe delle tabelle del database).
{{< /markdownify >}}
{{< /bs/alert >}}

```cs
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## EF Core Model Tutorial 1 - *Console App*

*In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core[^2]*.  

{{< bs/alert >}}
{{< markdownify >}}
Lo svolgimento di questo esempio mostra come utilizzare Visual Studio per creare un progetto che utilizza EF Core con SQLite. La realizzazione di un progetto .NET può anche essere fatta, ricorrendo alla [.NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/), ossia ad un insieme di strumenti (`tools`) che permettono di sviluppare, compilare, eseguire e pubblicare applicazioni .NET dalla command-line. In questo corso di quarta si farà principalmente riferimento alle modalità operative di Visual Studio per la gestione di progetti in .NET, piuttosto che ai comandi della .NET CLI. Le ragioni di questa scelta sono legate al fatto che l'ambiente di sviluppo utilizzato a scuola è basato su Windows e Visual Studio. L'utilizzo della .NET CLI è invece particolarmente indicato per chi sviluppa con Visual Studio Code.
{{< /markdownify >}}
{{< /bs/alert >}}

### Prerequisiti

Install the following software:
[Visual Studio 2022 version 17.4 or later](https://www.visualstudio.com/downloads/) with this workload:

* **.NET desktop development (under Desktop && Mobile)**

### Create a new project

* Open Visual Studio
* Click **New project**
* Select **Console App** with the **C#** tag and click **Next**
* Enter **EFGetStarted** for the name and click **Create**
{{< bs/alert >}}
{{< markdownify >}}
In questo esempio e in altri che seguiranno verranno utilizzate alcune caratteristiche del linguaggio C# presenti a partire dalla [versione 9.0](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-9) ([`Top-level statements`](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-9#top-level-statements)) e [versione 10.0](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-10) ([`Global using directives`](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-10#global-using-directives), [`File-scoped namespace declaration`](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-10#file-scoped-namespace-declaration), [`Global and implicit usings`](https://devblogs.microsoft.com/dotnet/welcome-to-csharp-10/#global-and-implicit-usings))
{{< /markdownify >}}
{{< /bs/alert >}}

### Install Entity Framework Core

To install EF Core, you install the package for the EF Core database provider(s) you want to target. This tutorial uses SQLite because it runs on all platforms that .NET supports. For a list of available providers, see [Database Providers](https://learn.microsoft.com/en-us/ef/core/providers/).  

* **Tools > NuGet Package Manager > Package Manager Console**

* Run the following commands:

    ```ps1
    Install-Package Microsoft.EntityFrameworkCore.Sqlite
    ```

>Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**

### Create the model

Define a context class and entity classes that make up the model.  

* Right-click on the project and select **Add > Class**
* Enter **Model.cs** as the name and click **Add**
* Replace the contents of the file with the following code

```cs
//file: Model.cs
using Microsoft.EntityFrameworkCore;

namespace EFGetStarted;
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; } = null!;
    public DbSet<Post> Posts { get; set; } = null!;

    public string DbPath { get; }

    public BloggingContext()
    {
        var folder = Environment.SpecialFolder.LocalApplicationData;
        var path = Environment.GetFolderPath(folder);
        DbPath = System.IO.Path.Join(path, "blogging.db");
    }

    // The following configures EF to create a Sqlite database file in the
    // special "local" folder for your platform.
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite($"Data Source={DbPath}");
}

public class Blog
{
    public int BlogId { get; set; }
    public string? Url { get; set; }

    public List<Post> Posts { get; } = new();
}

public class Post
{
    public int PostId { get; set; }
    public string? Title { get; set; }
    public string? Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; } = null!;
}
```

EF Core can also [reverse engineer](https://learn.microsoft.com/en-us/ef/core/managing-schemas/scaffolding/) a model from an existing database.

>Tip: This application intentionally keeps things simple for clarity. [Connection strings](https://learn.microsoft.com/en-us/ef/core/miscellaneous/connection-strings) should not be stored in the code for production applications. You may also want to split each C# class into its own file.

### Create the database

{{< bs/alert warning >}}
{{< markdownify >}}
Importante: nel file `Program.cs` deve esserci la definizione dell’oggetto `BloggingContext`, altrimenti la migrazione va in errore, perché il tool che crea la migrazione non trova il `DBContext`.
{{< /markdownify >}}
{{< /bs/alert >}}

```cs
//File: Program.cs
using EFGetStarted;

using var db = new BloggingContext();
```

The following steps use [migrations](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/) to create a database.

* Run the following commands in Package Manager Console (PMC)
  
  ```ps1
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  This installs the [PMC tools for EF Core](https://learn.microsoft.com/en-us/ef/core/cli/powershell). The Add-Migration command scaffolds a migration to create the initial set of tables for the model. The Update-Database command creates the database and applies the new migration to it.  

{{< bs/alert warning >}}
{{< markdownify >}}
Il codice di esempio inserito nel file `Model.cs` crea il database nella cartella:  `C:\Users\%USERNAME%\AppData\Local\`. Il percorso completo al file del database è dunque: `C:\Users\%USERNAME%\AppData\Local\blogging.db`. Assicurarsi che il file `blogging.db` non esista già nella posizione specificata, altrimenti la creazione del database (comando `Update-Database`) darà errore. Se provando a creare il database si ottiene il messaggio d’errore ***SQLite Error 1: 'table "Blogs" already exists'*** significa che il database esiste già. Se si vuole ricreare il database da capo bisogna cancellare il vecchio file e lanciare nuovamente il comando seguente nella PMC:
`Update-Database`
{{< /markdownify >}}
{{< /bs/alert >}}

Per aprire il file del database di SQLite, si utilizzi il programma `DB Browser`, cliccando sul pulsante **Open Database**

![DB Browser Open Database](DB_Browser_Open_Database.png#center)

Si selezioni il file del database di SQLite e si osservi le tabelle create:

![Created Tables in DB Browser](Created_Tables_in_DB_Browser.png#center)

Cliccando sul pulsante **Browse Data** si vede che non ci sono ancora dati nel database.

* **Inserire dati nel database**.  
  Per inserire dati nel database si modifichi il file `Program.cs` con il seguente codice:

    ```cs
    //File: Program.cs
    using EFGetStarted;

    using var db = new BloggingContext();
    // Note: This sample requires the database to be created before running.
    Console.WriteLine($"Database path: {db.DbPath}.");

    // Create
    Console.WriteLine("Inserting a new blog");
    db.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();

    // Read
    Console.WriteLine("Querying for a blog");
    var blog = db.Blogs
        .OrderBy(b => b.BlogId)
        .First();
    Console.WriteLine($"blog: {blog.Url}");
    ```

    Si mandi in esecuzione il progetto e si osservi il contenuto della tabella Blogs dal programma **DB Browser for SQLite**:

    ![First value inserted in Blogs table](First_VAlue_inserted_in_Blogs_table.png#center)

* **Modificare il contenuto del blog con BlogId=1 e aggiungere un post associato a questo blog**.  
    Per ottenere questo risultato si modifichi il codice del file `Program.cs` come segue:

    ```cs
    using EFGetStarted;

    using var db = new BloggingContext();
    // Note: This sample requires the database to be created before running.
    Console.WriteLine($"Database path: {db.DbPath}.");

    // Create
    //Console.WriteLine("Inserting a new blog");
    //db.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
    //db.SaveChanges();

    //// Read
    Console.WriteLine("Querying for a blog");
    var blog = db.Blogs
        .OrderBy(b => b.BlogId)
        .First();
    Console.WriteLine($"blog: {blog.Url}");

    // Update
    Console.WriteLine("Updating the blog and adding a post");
    blog.Url = "https://devblogs.microsoft.com/dotnet";
    blog.Posts.Add(
        new Post { Title = "Hello World", Content = "I wrote an app using EF Core!" });
    db.SaveChanges();
    ```

* **Cancellare un blog e tutti i post ad esso associati**.  
  Per ottenere questo risultato si modifichi il codice del file `Program.cs` come segue:

    ```cs
    using EFGetStarted;

    using var db = new BloggingContext();
    // Note: This sample requires the database to be created before running.
    Console.WriteLine($"Database path: {db.DbPath}.");

    // Create
    //Console.WriteLine("Inserting a new blog");
    //db.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
    //db.SaveChanges();

    //// Read
    Console.WriteLine("Querying for a blog");
    var blog = db.Blogs
        .OrderBy(b => b.BlogId)
        .First();
    Console.WriteLine($"blog: {blog.Url}");

    // Update
    //Console.WriteLine("Updating the blog and adding a post");
    //blog.Url = "https://devblogs.microsoft.com/dotnet";
    //blog.Posts.Add(
    //    new Post { Title = "Hello World", Content = "I wrote an app using EF Core!" });
    //db.SaveChanges();

    // Delete
    Console.WriteLine("Delete the blog");
    db.Remove(blog);
    db.SaveChanges();
    ```

    Si può osservare che il database è ritornato ad essere vuoto.

## EF Core Model Tutorial 2 - *Gestione Fatture e Clienti*

### Creazione progetto

Come nell’esempio precedente, creare in progetto **Console** `.NET Core`

* Open Visual Studio
* Click **New project**
* Select **Console App** with the **C#** tag and click **Next**
* Enter **GestioneFattureClienti** for the name and click **Create**

### Installazione di Entity Framework Core

* Enter required packages:
  * **Tools > NuGet Package Manager > Package Manager Console**
    Run the following commands:

    ```ps1
    Install-Package Microsoft.EntityFrameworkCore.Sqlite
    Install-Package Microsoft.EntityFrameworkCore.Tools
    ```

>Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**

### Creazione del modello

* In Visual Studio creare la cartella `Model` e al suo interno aggiungere la classe `Cliente` nel file `Cliente.cs` con il seguente codice:

    ```cs
    namespace GestioneFattureClienti.Model;
    public class Cliente
    {
        public int ClienteId { get; set; }
        public string RagioneSociale { get; set; } = null!;
        public string PartitaIVA { get; set; }=null!;
        public string? Citta { get; set; }
        public string? Via { get; set; }
        public string? Civico { get; set; }
        public string? CAP { get; set; }
        public List<Fattura> Fatture { get; } = new List<Fattura>();

        public override string ToString()
        {
            return string.Format($"[{nameof(ClienteId)}= {ClienteId}, {nameof(RagioneSociale)} = {RagioneSociale}, " +
                $"{nameof(PartitaIVA)} = {PartitaIVA}, {nameof(Citta)} = {Citta}, {nameof(Via)} = {Via}, {nameof(Civico)} = {Civico}, {nameof(CAP)} = {CAP}]");
        }
    }
    ```

* Aggiungere la classe `Fattura` nel file `Fattura.cs` all’interno della cartella `Model`, con dentro il seguente codice:

    ```cs
    namespace GestioneFattureClienti.Model;

    public class Fattura
    {
        public int FatturaId { get; set; }
        public DateTime Data { get; set; }
        public decimal Importo { get; set; }
        public int ClienteId { get; set; }
        public Cliente Cliente { get; set; } = null!;

        public override string ToString()
        {
            return string.Format($"{nameof(FatturaId)} = {FatturaId}, {nameof(Data)} = {Data.ToShortDateString()}, {nameof(Importo)} = {Importo}, {nameof(ClienteId)} = {ClienteId}");
        }
    }
    ```

* Creare una cartella `Data` e al suo interno aggiungere la classe `FattureClientiContext` nel file `FattureClientiContext.cs` con il codice:

    ```cs
    using GestioneFattureClienti.Model;
    using Microsoft.EntityFrameworkCore;
    namespace GestioneFattureClienti.Data;
    public class FattureClientiContext : DbContext
    {
        public DbSet<Fattura> Fatture { get; set; } = null!;
        public DbSet<Cliente> Clienti { get; set; } = null!;
        public string DbPath { get; }

        public FattureClientiContext()
        {
            //https://www.hanselman.com/blog/how-do-i-find-which-directory-my-net-core-console-application-was-started-in-or-is-running-from
            var folder = AppContext.BaseDirectory;
            //La BaseDirectory restituisce la cartella dove si trova l'assembly (.dll e .exe del programma compilato)
            //il database, per comodità, è inserito nella cartella di progetto, dove si trova anche il file Program.cs 
            var path = Path.Combine(folder, "../../../FattureClienti.db");
            DbPath = path;
        }
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) 
            => optionsBuilder.UseSqlite($"Data Source={DbPath}");
    }
    ```

A questo punto nel progetto dovrebbe essere presente una struttura di file e cartelle come quella riportata nella figura seguente:

![Solution Explorer Image](SolutionExplorerImage.png#center)

* Salvare tutti i file da Visual Studio
* Eseguire il seguente comando nella Package Manager Console (PMC)
  
  ```ps1
  Add-Migration InitialCreate
  Update-Database
  ```

Una volta eseguita la migration e aggiornato il database, dovrebbe essere presente un file di database di SQLite `FattureClienti.db` con le tabelle corrispondenti agli oggetti `Fatture` e `Clienti` (di tipo `DBSet<Fattura>` e `DBSet<Cliente>` rispettivamente).
Dopo aver eseguito la migration e l’aggiornamento del database, la struttura dei file e delle cartelle dovrebbe essere come quella della figura seguente:

![Solution Explorer Image complete](SolutionExplorerImageComplete.png#center)

* Si scriva il codice nella classe `Program` per testare le funzionalità di EF Core con SQLite, come nell'esempio seguente:

```cs
//File Program.cs
using GestioneFattureClienti.Data;
using System.Text;
using GestioneFattureClienti.Model;
using Microsoft.EntityFrameworkCore.Storage;
using Microsoft.EntityFrameworkCore.Infrastructure;

Console.OutputEncoding = Encoding.UTF8;
Thread.CurrentThread.CurrentCulture = System.Globalization.CultureInfo.GetCultureInfo("it-IT");

ModalitàOperativa modalitàOperativa = ModalitàOperativa.Nessuna;
//colore di default della console
Console.ForegroundColor = ConsoleColor.Cyan;
//gestione del menu
bool uscitaDalProgramma = false;
do
{
    //gestione della scelta 
    bool correctInput = false;
    do
    {
        Console.WriteLine("Inserire la modalità operativa [CreazioneDb, LetturaDb, ModificaFattura, CancellazioneFattura, CancellazioneDb, Nessuna]");
        correctInput = Enum.TryParse(Console.ReadLine(), true, out modalitàOperativa);
        if (correctInput)
        {
            switch (modalitàOperativa)
            {
                case ModalitàOperativa.CreazioneDb:
                    CreazioneDb();
                    break;
                case ModalitàOperativa.LetturaDb:
                    LetturaDb();
                    break;
                case ModalitàOperativa.ModificaFattura:
                    ModificaFattura();
                    break;
                case ModalitàOperativa.CancellazioneFattura:
                    CancellazioneFattura();
                    break;
                case ModalitàOperativa.CancellazioneDb:
                    CancellazioneDb();
                    break;
                default:
                    WriteLineWithColor("Non è stata impostata nessuna modalità operativa", ConsoleColor.Yellow);
                    break;
            }
        }
        if (!correctInput)
        {
            Console.Clear();
            WriteLineWithColor("Il valore inserito non corrisponde a nessuna opzione valida.\nI valori ammessi sono: [Creazione, Lettura, Modifica, Cancellazione, Nessuna]", ConsoleColor.Red);
        }
    } while (!correctInput);
    Console.WriteLine("Uscire dal programma?[Si, No]");
    uscitaDalProgramma = Console.ReadLine()?.ToLower().StartsWith("si") ?? false;
    Console.Clear();
} while (!uscitaDalProgramma);

static void CreazioneDb()
{
    FattureClientiContext db = new ();
    //verifichiamo se il database esista già
    //https://medium.com/@Usurer/ef-core-check-if-db-exists-feafe6e36f4e
    //https://stackoverflow.com/questions/33911316/entity-framework-core-how-to-check-if-database-exists
    if (db.Database.GetService<IRelationalDatabaseCreator>().Exists())
    {
        WriteLineWithColor("Il database esiste già, vuoi ricrearlo da capo? Tutti i valori precedentemente inseriti verranno persi. [Si, No]", ConsoleColor.Red);
        bool dbErase = Console.ReadLine()?.ToLower().StartsWith("si") ?? false;
        if (dbErase)
        {
            //cancelliamo il database se esiste
            db.Database.EnsureDeleted();
            //ricreiamo il database a partire dal model (senza dati --> tabelle vuote)
            db.Database.EnsureCreated();
            //inseriamo i dati nelle tabelle
            PopulateDb(db);
        }
    }
    else //il database non esiste
    {
        //ricreiamo il database a partire dal model (senza dati --> tabelle vuote)
        db.Database.EnsureCreated();
        PopulateDb(db);
    }

    static void PopulateDb(FattureClientiContext db)
    {
        //CreazioneDb di Clienti - gli id vengono generati automaticamente come campi auto-incremento quando si effettua l'inserimento, tuttavia
        //è bene inserire esplicitamente l'id degli oggetti quando si procede all'inserimento massivo gli elementi mediante un foreach perché
        //EF core potrebbe inserire nel database gli oggetti in un ordine diverso rispetto a quello del foreach
        // https://stackoverflow.com/a/54692592
        // https://stackoverflow.com/questions/11521057/insertion-order-of-multiple-records-in-entity-framework/
        List<Cliente> listaClienti = new()
        {
            new (){ClienteId=1, RagioneSociale= "Cliente 1", PartitaIVA= "1111111111", Citta = "Napoli", Via="Via dei Mille", Civico= "23", CAP="80100"},
            new (){ClienteId=2, RagioneSociale= "Cliente 2", PartitaIVA= "1111111112", Citta = "Roma", Via="Via dei Fori Imperiali", Civico= "1", CAP="00100"},
            new (){ClienteId=3, RagioneSociale= "Cliente 3", PartitaIVA= "1111111113", Citta = "Firenze", Via="Via Raffaello", Civico= "10", CAP="50100"}
        };

        //CreazioneDb delle Fatture - 
        List<Fattura> listaFatture = new()
        {
            new (){FatturaId=1, Data= DateTime.Now.Date, Importo = 1200.45m, ClienteId = 1},
            new (){FatturaId=2, Data= DateTime.Now.AddDays(-5).Date, Importo = 3200.65m, ClienteId = 1},
            new (){FatturaId=3, Data= new DateTime(2019,10,20).Date, Importo = 5200.45m, ClienteId = 1},
            new (){FatturaId=4, Data= DateTime.Now.Date, Importo = 5200.45m, ClienteId = 2},
            new (){FatturaId=5, Data= new DateTime(2019,08,20).Date, Importo = 7200.45m, ClienteId = 2}
        };

        Console.WriteLine("Inseriamo i clienti nel database");
        listaClienti.ForEach(c => db.Add(c));
        db.SaveChanges();
        Console.WriteLine("Inseriamo le fatture nel database");
        listaFatture.ForEach(f => db.Add(f));
        db.SaveChanges();
    }
}

static void LetturaDb()
{

    //recuperiamo i dati dal database
    FattureClientiContext db = new ();
    //Nel caso in cui il database fosse stato eliminato ricreiamo un nuovo database vuoto a partire dal Model
    db.Database.EnsureCreated();
    //Il codice seguente è scritto in modo che se anche non ci fossero dati nelle tabelle non verrebbero sollevate eccezioni
    Console.WriteLine("Recuperiamo i dati dal database - senza alcuna elaborazione");
    List<Cliente> listaClienti = db.Clienti.ToList();
    List<Fattura> listaFatture = db.Fatture.ToList();
    Console.WriteLine("Stampa dei clienti");
    listaClienti.ForEach(c => Console.WriteLine(c));
    Console.WriteLine("Stampa delle fatture");
    listaFatture.ForEach(f => Console.WriteLine(f));

    Console.WriteLine("Recuperiamo i dati dal database - uso di WHERE");
    Console.WriteLine("Recuperiamo i dati dal database - trovare le fatture fatte da almeno tre giorni");
    db.Fatture.Where(f => f.Data < DateTime.Now.AddDays(-2)).ToList().ForEach(f => Console.WriteLine(f));
    Console.WriteLine("Recuperiamo i dati dal database - trovare l'importo complessivo delle fatture fatte da almeno tre giorni ");
    Console.WriteLine($"Importo complessivo: {db.Fatture.Where(f => f.Data < DateTime.Now.AddDays(-2)).Sum(f => (double)f.Importo):C2}");
    Console.WriteLine("Recuperiamo i dati dal database - trovare l'importo medio delle fatture fatte da almeno tre giorni ");
    var count = db.Fatture.Where(f => f.Data < DateTime.Now.AddDays(-2)).Count();
    //se non ci sono elementi nella collection i metodi Average, Max e Min sollevano l'eccezione InvalidOperationException
    if (count > 0)
    {
        Console.WriteLine($"Importo medio: {db.Fatture.Where(f => f.Data < DateTime.Now.AddDays(-2)).Average(f => (double)f.Importo):C2}");
        Console.WriteLine("Recuperiamo i dati dal database - trovare l'importo massimo delle fatture fatte da almeno tre giorni ");
        Console.WriteLine($"Importo massimo: {db.Fatture.Where(f => f.Data < DateTime.Now.AddDays(-2)).Max(f => (double)f.Importo):C2}");
        Console.WriteLine("Recuperiamo i dati dal database - trovare l'importo minimo delle fatture fatte da almeno tre giorni ");
        Console.WriteLine($"Importo minimo: {db.Fatture.Where(f => f.Data < DateTime.Now.AddDays(-2)).Min(f => (double)f.Importo):C2}");
        Console.WriteLine("Recuperiamo i dati dal database - trovare il numero delle fatture fatte da almeno tre giorni ");
        Console.WriteLine("Numero fatture: " + count);
    }
    Console.WriteLine("Recuperiamo i dati dal database - uso di WHERE e JOIN");
    Console.WriteLine("trovare il nome e l'indirizzo dei clienti che hanno speso più di 5000 EUR");
    var clientiConSpesa5000Plus = db.Fatture.Where(f => f.Importo > 5000).Join(db.Clienti,
        f => f.ClienteId,
        c => c.ClienteId,
        (f, c) => new { NumeroFattura = f.FatturaId, DataFattura = f.Data, NomeCliente = c.RagioneSociale, Indirizzo = c.Via + " N." + c.Civico + " " + c.CAP + " " + c.Citta });
    clientiConSpesa5000Plus.ToList().ForEach(c => Console.WriteLine(c));

    //altro modo - uso delle navigation properties
    Console.WriteLine("Recuperiamo i dati dal database - Uso di Navigation Property per ottenere i dati dei clienti a partire dalle fatture");
    var clientiConSpesa5000Plus2 = db.Fatture.Where(f => f.Importo > 5000).
        Select(f => new
        {
            NumeroFattura = f.FatturaId,
            DataFattura = f.Data,
            NomeCliente = f.Cliente.RagioneSociale,
            Indirizzo = f.Cliente.Via + " N." + f.Cliente.Civico + " " + f.Cliente.CAP + " " + f.Cliente.Citta
        });
    clientiConSpesa5000Plus2.ToList().ForEach(c => Console.WriteLine(c));
}

static void ModificaFattura()
{

    FattureClientiContext db = new ();
    //la modifica ha senso solo se il database esiste e ha almeno un paio di fatture inserite 
    //Nel caso in cui il database fosse stato eliminato ricreiamo un nuovo database vuoto a partire dal Model
    db.Database.EnsureCreated();
    //Il codice seguente è scritto in modo che se anche non ci fossero dati nelle tabelle non verrebbero sollevate eccezioni
    var numeroFatture = db.Fatture.Count();
    if(numeroFatture > 2)
    {
        Console.WriteLine("\nModifichiamo i dati nel database");
        Console.WriteLine("Modifichiamo l'importo della prima fattura");

        Console.WriteLine("\nle fatture prima della modifica sono:\n");

        //stampiamo la lista delle fatture per vedere il risultato
        //se il database non esiste listaFatture sarà null.

        List<Fattura> listaFatture = db.Fatture.ToList();
        Console.WriteLine("Stampa delle fatture");
        listaFatture.ForEach(f => Console.WriteLine(f));

        //accediamo all'elemento da modificare
        Fattura? fattura = db.Fatture.Find(1);//trova l'entity con il valore della chiave (FatturaId) specificato
        if (fattura != null)//se ho trovato la fattura con FatturaId specificato
        {
            //modifichiamo la fattura
            fattura.Importo *= 1.2m;//incremento del 20% l'importo
            db.SaveChanges();//aggiorno il database

            //stampiamo la lista delle fatture per vedere il risultato
            listaFatture = db.Fatture.ToList();
            Console.WriteLine("Stampa delle fatture dopo la modifica");
            listaFatture.ForEach(f => Console.WriteLine(f));
        }
        Console.WriteLine("\nmodifichiamo anche la seconda fattura");
        //non è possibile usare ElementAt - https://stackoverflow.com/questions/5147767/why-is-the-query-operator-elementat-is-not-supported-in-linq-to-sql
        Fattura? secondaFattura = db.Fatture.Skip(1)?.First();
        if (secondaFattura != null)
        {
            secondaFattura.Importo *= 1.3m;
            db.SaveChanges();
            //stampiamo la lista delle fatture per vedere il risultato
            listaFatture = db.Fatture.ToList();
            Console.WriteLine("Stampa delle fatture");
            listaFatture.ForEach(f => Console.WriteLine(f));
        }
        Console.WriteLine("\nmodifichiamo anche l'ultima");
        //non è possibile usare ElementAt - https://stackoverflow.com/questions/5147767/why-is-the-query-operator-elementat-is-not-supported-in-linq-to-sql
        Fattura? ultimaFattura = db.Fatture.OrderBy(f => f.FatturaId).Last();
        if (ultimaFattura != null)
        {
            ultimaFattura.Importo *= 1.5m;
            db.SaveChanges();
            //stampiamo la lista delle fatture per vedere il risultato
            listaFatture = db.Fatture.ToList();
            Console.WriteLine("Stampa delle fatture");
            listaFatture.ForEach(f => Console.WriteLine(f));
        }
    }
    else
    {
        WriteLineWithColor("Non ci sono abbastanza Fatture nel database per eseguire la funzionalità richiesta", ConsoleColor.Red);
    }
}


static void CancellazioneFattura()
{
    FattureClientiContext db = new ();
    //Nel caso in cui il database fosse stato eliminato ricreiamo un nuovo database vuoto a partire dal Model
    db.Database.EnsureCreated();
    //Il codice seguente è scritto in modo che se anche non ci fossero dati nelle tabelle non verrebbero sollevate eccezioni
    Console.WriteLine("\nEliminiamo un dato dal database");
    Console.WriteLine("\nPrima della cancellazione le fatture sono:");
    List<Fattura> listaFatture = db.Fatture.ToList();
    Console.WriteLine("Stampa delle fatture");
    listaFatture.ForEach(f => Console.WriteLine(f));
    Console.WriteLine("\nEliminiamo la terza fattura");
    //accediamo alla terza fattura - possiamo accedere o mediante chiave, oppure in base ad un elenco da cui prendiamo la terza
    //decidiamo di eliminare la fattura con FatturaId=3
    Fattura? fatturaDaEliminare = db.Fatture.Find(3);
    if (fatturaDaEliminare != null)//se abbiamo trovato la fattura con id =3
    {
        Console.WriteLine($"eliminiamo la fattura con id {fatturaDaEliminare.FatturaId}");
        db.Remove(fatturaDaEliminare);
        db.SaveChanges();
        //stampiamo nuovamente l'elenco delle fatture per verificare che la fattura con id specifico è stata eliminata
        listaFatture = db.Fatture.ToList();
        Console.WriteLine("Stampa delle fatture");
        listaFatture.ForEach(f => Console.WriteLine(f));
    }
    else
    {
        WriteLineWithColor("Non è stato possibile eliminare la fattura indicata, perché non esiste", ConsoleColor.Red);
    }
}

static void CancellazioneDb()
{
    FattureClientiContext db = new ();
    WriteLineWithColor("Attenzione tutto il database andrà eliminato! Questa operazione non può essere revocata.\nSei sicuro di voler procedere? [Si, No]", ConsoleColor.Red);
    bool dbErase = Console.ReadLine()?.ToLower().StartsWith("si") ?? false;
    if (dbErase)
    {
        if (db.Database.EnsureDeleted())
        {
            Console.WriteLine("Database cancellato correttamente");
        }
        else
        {
            Console.WriteLine("Il database non esisteva e non è stata fatta alcuna azione");
        }
    }
}

//stampa a console con il colore di foreground selezionato e successivamente ripristina il colore precedente
static void WriteLineWithColor(string text, ConsoleColor consoleColor)
{
    ConsoleColor previousColor = Console.ForegroundColor;
    Console.ForegroundColor = consoleColor;
    Console.WriteLine(text);
    Console.ForegroundColor = previousColor;
}
enum ModalitàOperativa
{
    CreazioneDb,
    LetturaDb,
    ModificaFattura,
    CancellazioneFattura,
    CancellazioneDb,
    Nessuna
}
```

[^1]: [EF Core](https://learn.microsoft.com/en-us/ef/core/)
[^2]: [Getting Started with EF Core](https://learn.microsoft.com/en-us/ef/core/get-started/overview/first-app?tabs=visual-studio)
