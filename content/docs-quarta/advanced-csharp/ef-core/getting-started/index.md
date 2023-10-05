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

{{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/EFGetStarted/EFGetStarted/Model.cs" >}}

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
Il codice di esempio inserito nel file `Model.cs` crea il database nella cartella:  `C:\Users\%USERNAME%\AppData\Local\`. Assicurarsi che il file `blogging.db` non esista già nella posizione specificata, altrimenti la creazione del database (comando `Update-Database`) darà errore. Se provando a creare il database si ottiene il messaggio d’errore ***SQLite Error 1: 'table "Blogs" already exists'*** significa che il database esiste già. Se si vuole ricreare il database da capo bisogna cancellare il vecchio file e lanciare nuovamente il comando seguente nella PMC:
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

    {{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/GestioneFattureClienti/GestioneFattureClienti/Model/Cliente.cs" >}}

* Aggiungere la classe `Fattura` nel file `Fattura.cs` all’interno della cartella `Model`, con dentro il seguente codice:

    {{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/GestioneFattureClienti/GestioneFattureClienti/Model/Fattura.cs" >}}

* Creare una cartella `Data` e al suo interno aggiungere la classe `FattureClientiContext` nel file `FattureClientiContext.cs` con il codice:

    {{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/GestioneFattureClienti/GestioneFattureClienti/Data/FattureClientiContext.cs" >}}

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

### Interazione con la base di dati

* Si scriva il codice nella classe `Program` per testare le funzionalità di EF Core con SQLite, come nell'esempio seguente:

    {{< ghcode "https://raw.githubusercontent.com/malafronte/malafronte-doc-samples/main/samples-quarta/EFCore/GestioneFattureClienti/GestioneFattureClienti/Program.cs" >}}

[^1]: [EF Core](https://learn.microsoft.com/en-us/ef/core/)
[^2]: [Getting Started with EF Core](https://learn.microsoft.com/en-us/ef/core/get-started/overview/first-app?tabs=visual-studio)
