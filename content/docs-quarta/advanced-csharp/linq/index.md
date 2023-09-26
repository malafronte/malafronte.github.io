---
title: "Linq"
# linkTitle:
date: 2023-09-26T19:06:49+02:00
draft: false
type: docs
description: "LINQ (Language Integrated Query)"
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

## What is LINQ?

LINQ (Language Integrated Query) is uniform query syntax in C# to retrieve data from different sources and formats. It is integrated in C#, thereby eliminating the mismatch between programming languages and databases, as well as providing a single querying interface for different types of data sources.
For example, SQL is a Structured Query Language used to save and retrieve data from a database. In the same way, LINQ is a structured query syntax built in C# to retrieve data from different types of data sources such as collections, ADO.Net DataSet, XML Docs, web service and MS SQL Server and other databases.

LINQ queries return results as objects. It enables you to uses object-oriented approach on the result set and not to worry about transforming different formats of results into objects.  

The following example demonstrates a simple LINQ query that gets all strings from an array which contains `a`.

```cs
namespace ArgomentiAvanzati
{
  internal class Program
  {
      static void Main()
      {
          // Data source
          string[] names = { "Bill", "Steve", "James", "Johan" };

          // LINQ Query 
          var myLinqQuery = from name in names
                            where name.Contains('a')
                            select name;
          // Query execution
          foreach (var name in myLinqQuery)
              Console.Write(name + " ");
      }
  }
}
```

In the above example, string array names is a data source. The following is a LINQ query which is assigned to a variable myLinqQuery.

```cs
from name in names
where name.Contains('a')
select name;
```

The above query uses query syntax of LINQ. You will learn more about it in the Query Syntax chapter. You will not get the result of a LINQ query until you execute it. LINQ query can be execute in multiple ways, here we used foreach loop to execute our query stored in myLinqQuery. The foreach loop executes the query on the data source and get the result and then iterates over the result set. Thus, every LINQ query must query to some kind of data sources whether it can be array, collections, XML or other databases. After writing LINQ query, it must be executed to get the result.

## Why LINQ?

To understand why we should use LINQ, let's look at some examples. Suppose you want to find list of teenage students from an array of Student objects.
Before C# 2.0, we had to use a 'foreach' or a 'for' loop to traverse the collection to find a particular object. For example, we had to write the following code to find all Student objects from an array of Students where the age is between 12 and 20 (for teenage 13 to 19):

### Example 1 - Use for loop to find elements from the collection

```cs
namespace ArgomentiAvanzati
{
  class Student
  {
      public int StudentID { get; set; }
      public string? StudentName { get; set; }
      public int Age { get; set; }

      public override string ToString()
      {
          return string.Format($"[StudentID = {StudentID}, StudentName = {StudentName}, Age = {Age}]");
      }
  }

  internal class Program
  {
      static void Main(string[] args)
      {
          Student[] studentArray = 
          {
            new Student() { StudentID = 1, StudentName = "John", Age = 18},
            new Student() { StudentID = 2, StudentName = "Steve",  Age = 21},
            new Student() { StudentID = 3, StudentName = "Bill",  Age = 25},
            new Student() { StudentID = 4, StudentName = "Ram" , Age = 20},
            new Student() { StudentID = 5, StudentName = "Ron" , Age = 31},
            new Student() { StudentID = 6, StudentName = "Chris",  Age = 17},
            new Student() { StudentID = 7, StudentName = "Rob", Age = 19},
          };
      };
          List<Student> students = new ();
          foreach (Student std in studentArray)
          {
              if (std.Age > 12 && std.Age < 20)
              {
                  students.Add(std);
              }
          }
          //write result
          foreach (var studente in students)
          {
              Console.WriteLine(studente);
          }
          Console.ReadLine();
      }
  }
}
```

Use of for loop is cumbersome, not maintainable and readable. C# 2.0 introduced delegate, which can be used to handle this kind of a scenario, as shown below.

### Example 2 - Use Delegates to Find Elements from the Collection

```cs
namespace ArgomentiAvanzati
{
  delegate bool FindStudent(Student std);
  class StudentExtension
  {
      public static List<Student> Where(Student[] stdArray, FindStudent del)
      {
          List<Student> resultList = new ();
          foreach (Student std in stdArray)
              if (del(std))
              {
                  resultList.Add(std);
              }
          return resultList;
      }
  }
  class Student
  {
      public int StudentID { get; set; }
      public string? StudentName { get; set; }
      public int Age { get; set; }

      public override string ToString()
      {
          return string.Format($"[StudentID = {StudentID}, StudentName = {StudentName}, Age = {Age}]");
      }
  }

  internal class Program
  {
      static void Main(string[] args)
      {
          Student[] studentArray = 
          {
            new Student() { StudentID = 1, StudentName = "John", Age = 18},
            new Student() { StudentID = 2, StudentName = "Steve",  Age = 21},
            new Student() { StudentID = 3, StudentName = "Bill",  Age = 25},
            new Student() { StudentID = 4, StudentName = "Ram" , Age = 20},
            new Student() { StudentID = 5, StudentName = "Ron" , Age = 31},
            new Student() { StudentID = 6, StudentName = "Chris",  Age = 17},
            new Student() { StudentID = 7, StudentName = "Rob", Age = 19},
          };
      };

          List<Student> students = StudentExtension.Where(studentArray, delegate (Student std)
          {
              return std.Age > 12 && std.Age < 20;
          });
          //in alternativa al delegate si può usare una lambda
          //List<Student> students = StudentExtension.Where(studentArray,
          //            std => std.Age > 12 && std.Age < 20);
          //write result
          foreach (var studente in students)
          {
              Console.WriteLine(studente);
          }
          Console.ReadLine();
      }
  }
}
```

So, with C# 2.0, you got the advantage of delegate in finding students with any criteria. You don't have to use a for loop to find students using different criteria. For example, you can use the same delegate function to find a student whose StudentId is 5 or whose name is Bill, as below:

```cs
List<Student> students = StudentExtension.where(studentArray, delegate(Student std) {
        return std.StudentID == 5;
    });

//Also, use another criteria using same delegate
List<Student> students = StudentExtension.where(studentArray, delegate(Student std) {
        return std.StudentName == "Bill";
    });
```

The C# team felt that they still needed to make the code even more compact and readable. So they introduced the extension method, lambda expression, expression tree, anonymous type and query expression in C# 3.0. You can use these features of C# 3.0, which are building blocks of LINQ to query to the different types of collection and get the resulted element(s) in a single statement.

### Example 3 - Use LINQ to Find Elements from the Collection

```cs
namespace ArgomentiAvanzati
{
  class Student
  {
      public int StudentID { get; set; }
      public string? StudentName { get; set; }
      public int Age { get; set; }

      public override string ToString()
      {
          return string.Format($"[StudentID = {StudentID}, StudentName = {StudentName}, Age = {Age}]");
      }
  }

  class Program
  {
      static void Main(string[] args)
      {
          Student[] studentArray = 
          {
            new Student() { StudentID = 1, StudentName = "John", Age = 18},
            new Student() { StudentID = 2, StudentName = "Steve",  Age = 21},
            new Student() { StudentID = 3, StudentName = "Bill",  Age = 25},
            new Student() { StudentID = 4, StudentName = "Ram" , Age = 20},
            new Student() { StudentID = 5, StudentName = "Ron" , Age = 31},
            new Student() { StudentID = 6, StudentName = "Chris",  Age = 17},
            new Student() { StudentID = 7, StudentName = "Rob", Age = 19},
          };

          // Use LINQ to find teenager students
          Student[] teenAgerStudents = studentArray.Where(s => s.Age > 12 && s.Age < 20).ToArray();

          // Use LINQ to find first student whose name is Bill 
          Student? bill = studentArray.Where(s => s.StudentName == "Bill").FirstOrDefault();

          // Use LINQ to find student whose StudentID is 5
          Student? student5 = studentArray.Where(s => s.StudentID == 5).FirstOrDefault();

          //write result
          foreach (var studente in teenAgerStudents)
          {
              Console.WriteLine(studente);
          }
          Console.WriteLine("bill: " + bill);
          Console.WriteLine("student5: " + student5);
      }
  }
}
```

As you can see in the above example, we specify different criteria using LINQ operator and lambda expression in a single statement. Thus, LINQ makes code more compact and readable and it can also be used to query different data sources. For example, if you have a student table in a database instead of an array of student objects as above, you can still use the same query to find students using the Entity Framework.

## Advantages of LINQ

• Familiar language: Developers don’t have to learn a new query language for each type of data source or data format.  
• Less coding: It reduces the amount of code to be written as compared with a more traditional approach.  
• Readable code: LINQ makes the code more readable so other developers can easily understand and maintain it.  
• Standardized way of querying multiple data sources: The same LINQ syntax can be used to query multiple data sources.  
• Compile time safety of queries: It provides type checking of objects at compile time.  
• IntelliSense Support: LINQ provides IntelliSense for generic collections.  
• Shaping data: You can retrieve data in different shapes.  

## LINQ API

We can write LINQ queries for the classes that implement `IEnumerable<T>` or `IQueryable<T>` interface.

{{< bs/alert >}}
{{< markdownify >}}
LINQ queries uses extension methods for classes that implement IEnumerable or IQueryable interface.
{{< /bs/alert >}}
{{< /markdownify >}}

### System.Linq.Enumerable

The Enumerable class includes extension methods for the classes that implement `IEnumerable<T>` interface, for example all the built-in collection classes implement `IEnumerable<T>` interface and so we can write LINQ queries to retrieve data from the built-in collections.

### System.Linq.Queryable

The `Queryable` class includes extension methods for classes that implement `IQueryable<t>` interface. The `IQueryable<T>` interface is used to provide querying capabilities against a specific data source where the type of the data is known. For example, Entity Framework api implements `IQueryable<T>` interface to support LINQ queries with underlying databases such as MS SQL Server.
Also, there are APIs available to access third party data; for example, LINQ to Amazon provides the ability to use LINQ with Amazon web services to search for books and other items. This can be achieved by implementing the IQueryable interface for Amazon.
The following figure shows the extension methods available in the Queryable class can be used with various native or third party data providers.

## Come usare LINQ?

Ci sono due modi per usare LINQ: mediante l’uso delle “LINQ query” oppure mediante l’uso dei “LINQ methods”.

### LINQ Query (Query Expression Syntax)

Query syntax is similar to SQL (Structured Query Language) for the database. It is defined within the C#.

LINQ Query Syntax:

```cs
from <range variable> in <IEnumerable<T> or IQueryable<T> Collection>

<Standard Query Operators> <lambda expression>

<select or groupBy operator> <result formation>
```

The LINQ query syntax starts with from keyword and ends with select keyword. The following is a sample LINQ query that returns a collection of strings which contains a word "Tutorials".

```cs
namespace ArgomentiAvanzati
{
    internal class Program
    {
        static void Main(string[] args)
        {
            // string collection
            IList<string> stringList = new List<string>() 
            {
              "C# Tutorials",
              "VB.NET Tutorials",
              "Learn C++",
              "MVC Tutorials" ,
              "Java"
            };
            // LINQ Query Syntax
            var result = from s in stringList
                         where s.Contains("Tutorials")
                         select s;

            foreach (var str in result)
            {
                Console.WriteLine(str);
            }
        }
    }
}
```

Query syntax starts with a `from` clause followed by a Range variable. The `from` clause is structured like "From rangeVariableName in IEnumerableCollection". In English, this means, from each object in the collection. It is similar to a foreach loop: `foreach(Student s in studentList)`.
After the From clause, you can use different Standard Query Operators to filter, group, join elements of the collection. There are around 50 Standard Query Operators available in LINQ. In the above figure, we have used `where` operator (aka clause) followed by a condition. This condition is generally expressed using lambda expression.
LINQ query syntax always ends with a `select` or `group` clause. The `select` clause is used to shape the data. You can select the whole object as it is or only some properties of it. In the above example, we selected the each resulted string elements.  
In the following example, we use LINQ query syntax to find out teenager students from the Student collection (sequence).

```cs
namespace ArgomentiAvanzati
{
  public class Student
  {
      public int StudentID { get; set; }
      public string? StudentName { get; set; }
      public int Age { get; set; }
  }
  internal class Program
  {
      static void Main(string[] args)
      {
          // Student collection
          IList<Student> studentList = new List<Student>() 
          {
            new Student() { StudentID = 1, StudentName = "John", Age = 13},
            new Student() { StudentID = 2, StudentName = "Mario",  Age = 21},
            new Student() { StudentID = 3, StudentName = "Bill",  Age = 18},
            new Student() { StudentID = 4, StudentName = "Ram" , Age = 20},
            new Student() { StudentID = 5, StudentName = "Ron" , Age = 15}
          };

          // LINQ Query Syntax to find out teenager students
          var teenAgerStudent = from s in studentList
                                where s.Age > 12 && s.Age < 20
                                select s;
          Console.WriteLine("Teen age Students:");

          foreach (Student std in teenAgerStudent)
          {
              Console.WriteLine(std.StudentName);
          }
      }
  }
}
```

### Points to Remember on Query Syntax

1. As name suggest, Query Syntax is same like SQL (Structure Query Language) syntax.
2. Query Syntax starts with from clause and can be end with Select or GroupBy clause.
3. Use various other operators like filtering, joining, grouping, sorting operators to construct the desired result.
4. Implicitly typed variable - var can be used to hold the result of the LINQ query.

## LINQ Methods (Fluent)

Method syntax (also known as fluent syntax) uses extension methods included in the Enumerable or Queryable static class, similar to how you would call the extension method of any class.
The following is a sample LINQ method syntax query that returns a collection of strings which contains a word "Tutorials".

```cs
// string collection
IList<string> stringList = new List<string>() 
{ 
    "C# Tutorials",
    "VB.NET Tutorials",
    "Learn C++",
    "MVC Tutorials" ,
    "Java" 
};

// LINQ Method Syntax
var result = stringList.Where(s => s.Contains("Tutorials"));
```

The extension method Where() is defined in the Enumerable class. If you check the signature of the Where extension method, you will find the Where method accepts a predicate delegate as `Func<Student, bool>`. This means you can pass any delegate function that accepts a Student object as an input parameter and returns a Boolean value as shown in the below figure. The lambda expression works as a delegate passed in the Where clause.
The following example shows how to use LINQ method syntax query with the `IEnumerable<T>` collection.

```cs
// Student collection
IList<Student> studentList = new List<Student>() 
    { 
      new Student() { StudentID = 1, StudentName = "John", Age = 13},
      new Student() { StudentID = 2, StudentName = "Mario",  Age = 21},
      new Student() { StudentID = 3, StudentName = "Bill",  Age = 18},
      new Student() { StudentID = 4, StudentName = "Ram" , Age = 20},
      new Student() { StudentID = 5, StudentName = "Ron" , Age = 15} 
    };

// LINQ Method Syntax to find out teenager students
var teenAgerStudents = studentList.Where(s => s.Age > 12 && s.Age < 20)
                                  .ToList<Student>();

```

Altro esempio:

```cs
namespace ArgomentiAvanzati
{
  public class Program
  {
      public static void Main()
      {
          // Student collection
          IList<Student> studentList = new List<Student>() 
          {
              new Student() { StudentID = 1, StudentName = "John", Age = 13},
              new Student() { StudentID = 2, StudentName = "Mario",  Age = 21},
              new Student() { StudentID = 3, StudentName = "Bill",  Age = 18},
              new Student() { StudentID = 4, StudentName = "Ram" , Age = 20},
              new Student() { StudentID = 5, StudentName = "Ron" , Age = 15}
          };

          Func<Student, bool> isStudentTeenAger = s => s.Age > 12 && s.Age < 20;
          var teenAgerStudent = studentList.Where(isStudentTeenAger);

          Console.WriteLine("Teen age Students:");

          foreach (Student std in teenAgerStudent)
          {
              Console.WriteLine(std.StudentName);
          }

          //oppure
          var teenAgerStudents = from s in studentList
                                  where isStudentTeenAger(s)
                                  select s;

          Console.WriteLine("Teen age Students:");
          foreach (Student std in teenAgerStudents)
          {
              Console.WriteLine(std.StudentName);
          }
      }
  }

  public class Student
  {
      public int StudentID { get; set; }
      public string? StudentName { get; set; }
      public int Age { get; set; }

  }
}
```

### Standard Query Operators

Standard Query Operators in LINQ are actually extension methods for the `IEnumerable<T>` and `IQueryable<T>` types. They are defined in the `System.Linq.Enumerable` and `System.Linq.Queryable` classes. There are over 50 standard query operators available in LINQ that provide different functionalities like filtering, sorting, grouping, aggregation, concatenation, etc.
**Standard Query Operators in Query Syntax**:

```cs
var teenAgerStudents = from s in studentList
                                  where s.Age > 12 && s.Age < 20
                                  select s;
```

**Standard Query Operators in Method Syntax**:

```cs
var teenAgerStudents = studentList.Where(s => s.Age > 12 && s.Age < 20)
                                  .ToList<Student>();
```

{{< bs/alert >}}
{{< markdownify >}}
Standard query operators in query syntax is converted into extension methods at compile time. So both are same.
{{< /bs/alert >}}
{{< /markdownify >}}
Standard Query Operators can be classified based on the functionality they provide. The following table lists all the classification of Standard Query Operators:
| **Classification** | **Standard Query Operators** |
| --- | --- |
| **Filtering** | Where, OfType |
| **Sorting** | OrderBy, OrderByDescending, ThenBy, ThenByDescending, Reverse |
| **Grouping** | GroupBy, ToLookup |
| **Join** | GroupJoin, Join |
| **Projection** | Select, SelectMany |
| **Aggregation** | Aggregate, Average, Count, LongCount, Max, Min, Sum |
| **Quantifiers** | All, Any, Contains |
| **Elements** | ElementAt, ElementAtOrDefault, First, FirstOrDefault, Last, LastOrDefault, Single, SingleOrDefault |
| **Set** | Distinct, Except, Intersect, Union |
| **Partitioning** | Skip, SkipWhile, Take, TakeWhile |
| **Concatenation** | Concat |
| **Equality** | SequenceEqual |
| **Generation** | DefaultEmpty, Empty, Range, Repeat |
| **Conversion** | AsEnumerable, AsQueryable, Cast, ToArray, ToDictionary, ToList |

Gli esempi di tutte le funzioni presenti nella tabella precedente sono reperibili [qui](https://www.tutorialsteacher.com/linq/linq-standard-query-operators)

### Points to Remember on LINQ Method Syntax

1. As name suggest, Method Syntax is like calling extension method.
2. LINQ Method Syntax aka Fluent syntax because it allows series of extension methods call.
3. Implicitly typed variable `var` can be used to hold the result of the LINQ query.

## Alcuni esempi di codice LINQ

```cs
using System.Collections;

namespace LINQgym
{
    class Student
    {
        public int StudentID { get; set; }
        public string? StudentName { get; set; }
        public int Age { get; set; }
        public double MediaVoti { get; set; }

        public override string ToString()
        {
            return String.Format($"[StudentID = {StudentID}, StudentName = {StudentName}, Age = {Age}, MediaVoti = {MediaVoti}]");
        }
    }

    class Assenza
    {
        public int ID { get; set; }
        public DateTime Giorno { get; set; }
        public int StudentID { get; set; }
    }

    class Persona
    {
        public string? Nome { get; set; }
        public int Eta { get; set; }

        public override string ToString()
        {
            return String.Format($"[Nome = {Nome}, Età = {Eta}]");
        }
    }


    class Program
    {
        //stiamo definendo un tipo di puntatore a funzione
        delegate bool CondizioneRicerca(Student s);

        public static void AzioneSuElemento(Student s)
        {
            Console.WriteLine(s);
        }

        //metodo statico
        public static bool VerificaCondizione(Student s)
        {
            return s.Age >= 18 && s.Age <= 25;
        }

        static void Main(string[] args)
        {

            Student[] studentArray1 = {
            new Student() { StudentID = 1, StudentName = "John", Age = 18 , MediaVoti= 6.5},
            new Student() { StudentID = 2, StudentName = "Steve",  Age = 21 , MediaVoti= 8},
            new Student() { StudentID = 3, StudentName = "Bill",  Age = 25, MediaVoti= 7.4},
            new Student() { StudentID = 4, StudentName = "Ram" , Age = 20, MediaVoti = 10},
            new Student() { StudentID = 5, StudentName = "Ron" , Age = 31, MediaVoti = 9},
            new Student() { StudentID = 6, StudentName = "Chris",  Age = 17, MediaVoti = 8.4},
            new Student() { StudentID = 7, StudentName = "Rob",Age = 19  , MediaVoti = 7.7},
            new Student() { StudentID = 8, StudentName = "Robert",Age = 22, MediaVoti = 8.1},
            new Student() { StudentID = 9, StudentName = "Alexander",Age = 18, MediaVoti = 4},
            new Student() { StudentID = 10, StudentName = "John", Age = 18 , MediaVoti = 6},
            new Student() { StudentID = 11, StudentName = "John",  Age = 21 , MediaVoti = 8.5},
            new Student() { StudentID = 12, StudentName = "Bill",  Age = 25, MediaVoti = 7},
            new Student() { StudentID = 13, StudentName = "Ram" , Age = 20, MediaVoti = 9 },
            new Student() { StudentID = 14, StudentName = "Ron" , Age = 31, MediaVoti = 9.5},
            new Student() { StudentID = 15, StudentName = "Chris",  Age = 17, MediaVoti = 8},
            new Student() { StudentID = 16, StudentName = "Rob2",Age = 19  , MediaVoti = 7},
            new Student() { StudentID = 17, StudentName = "Robert2",Age = 22, MediaVoti = 8},
            new Student() { StudentID = 18, StudentName = "Alexander2",Age = 18, MediaVoti = 9},
            };

            List<Student> studentList1 = studentArray1.ToList();

            //Studiamo la clausola Where

            //definiamo delle condizioni di ricerca
            //primo modo: uso di Func con lambda
            Func<Student, bool> condizioneDiRicerca = s => s.Age >= 18 && s.Age <= 25;
            //secondo modo: uso di un delegato implementato attraverso lambda
            CondizioneRicerca condizioneDiRicerca2 = s => s.Age >= 18 && s.Age <= 25;
            //terzo modo: uso di un delegato che punta a un metodo precedentemente definito
            CondizioneRicerca condizioneDiRicerca3 = VerificaCondizione;
            //quarto modo: usiamo direttamente la lambda - il più comodo

            Student[] studentResultArray;
            List<Student> studentResultList;

            //utilizzo dei LINQ extension methods

            //trovare tutti gli studenti che hanno età compresa tra 18 e 25 anni, caso dell'array
            studentResultArray = studentArray1.Where(s => s.Age >= 18 && s.Age <= 25).ToArray();
            //trovare tutti gli studenti che hanno età compresa tra 18 e 25 anni, caso della lista
            studentResultList = studentList1.Where(s => s.Age >= 18 && s.Age <= 25).ToList();
            //uso di delegati o di metodi 
            //https://stackoverflow.com/questions/1906787/cast-delegate-to-func-in-c-sharp
            studentResultArray = studentArray1.Where(new Func<Student, bool>(condizioneDiRicerca2)).ToArray();
            studentResultList = studentList1.Where(condizioneDiRicerca).ToList();
            //oppure 
            studentResultArray = studentArray1.Where(new Func<Student, bool>(condizioneDiRicerca3)).ToArray();
            studentResultList = studentList1.Where(VerificaCondizione).ToList();

            Console.WriteLine("stampa su Array con Action");
            Array.ForEach(studentResultArray, AzioneSuElemento);

            Console.WriteLine("stampa su Lista con Action");
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            //USO DELLA VERSIONE CON INDICE DELLA WHERE

            //il metodo Where ha anche una versione con l'indice della collection
            //in questo esempio prendiamo solo quelli che verificano la condizione sull'età e hanno indice pari
            Console.WriteLine("selezioniamo solo quelli che verificano la condizione e hanno indice pari");

            studentResultArray = studentArray1.Where(
                (s, i) => (s.Age >= 18 && s.Age <= 25) && i % 2 == 0).ToArray();
            Console.WriteLine("stampa su array");
            Array.ForEach(studentResultArray, s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            studentResultList = studentList1.Where(
                (s, i) => (s.Age >= 18 && s.Age <= 25) && i % 2 == 0).ToList();
            Console.WriteLine("stampa su list");
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            //E' possibile anche far applicare più volte la where per ottenere filtraggi multipli
            Console.WriteLine("doppia where: quelli che verificano la condizione e che hanno ID>3");
            studentResultList = studentList1.
                Where(s => s.Age >= 18 && s.Age <= 25).
                Where(s => s.StudentID > 3).ToList();
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age + " ID = " + s.StudentID));

            //utilizzo di LINQ query
            studentResultArray = (from s in studentArray1
                                  where s.Age >= 18 && s.Age <= 25
                                  select s).ToArray();

            studentResultList = (from s in studentResultList
                                 where s.Age >= 18 && s.Age <= 25
                                 select s).ToList();
            Console.WriteLine("Stampa del risultato con LINQ query");
            Array.ForEach(studentResultArray, s => Console.WriteLine(s.StudentName + " age = " + s.Age));
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            //foreach(var elem in studentResult)
            //{
            //    Console.WriteLine(elem);
            //}
            //Console.WriteLine(studentList1);

            //Studiamo la clausola OfType

            //nel caso di collection di tipo diverso è possibile trarre vantaggio dal metodo OfType
            IList mixedList = new ArrayList();
            mixedList.Add(5);
            mixedList.Add("numero uno");
            mixedList.Add(true);
            mixedList.Add("numero due");
            mixedList.Add(new Student() { StudentID = 10, Age = 30, StudentName = "Roberto" });
            List<string> mixedListResult = mixedList.OfType<string>().ToList();
            //IList mixedListResult2 =
            //    (from s in mixedList.OfType<Student>()
            //     where s.Age > 20
            //     select s).
            //    ToList();
            List<Student> mixedListResult2 =
            mixedList.OfType<Student>().Where(s => s.Age > 20).ToList();
            Console.WriteLine("\nStampa del risultato con OfType method");
            mixedListResult.ForEach(s => Console.WriteLine(s));
            mixedListResult2.ForEach(s => Console.WriteLine(s));

            //Studiamo la clausola OrderBy

            //ordiniamo una lista di elementi
            Console.WriteLine("\nOrdiniamo gli elementi di una lista con la clausola OrderBy");
            Console.WriteLine("\nOrdiniamo in base all'età - LINQ method");
            //per invertire l'ordine esiste anche OrderByDescending
            studentResultArray = studentArray1.OrderBy(s => s.Age).ToArray();
            Console.WriteLine("stampa su array");
            Array.ForEach(studentResultArray, s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            studentResultList = studentList1.OrderBy(s => s.Age).ToList();
            Console.WriteLine("\nstampa su list");
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            Console.WriteLine("\nOrdiniamo in base all'età - LINQ query");
            studentResultArray = (from s in studentArray1
                                  orderby s.Age
                                  select s).ToArray();
            Console.WriteLine("stampa su array");
            Array.ForEach(studentResultArray, s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            studentResultList = (from s in studentList1
                                 orderby s.Age
                                 select s).ToList();
            Console.WriteLine("\nstampa su list");
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            //ordinamenti multipli
            Console.WriteLine("\nOrdinamenti multipli - LINQ method");
            studentResultArray = studentArray1.OrderBy(s => s.Age).ThenBy(s => s.StudentName).ToArray();
            Console.WriteLine("stampa su array");
            Array.ForEach(studentResultArray, s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            studentResultList = studentList1.OrderBy(s => s.Age).ThenBy(s => s.StudentName).ToList();
            Console.WriteLine("\nstampa su list");
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            Console.WriteLine("\nOrdinamenti multipli - LINQ query");
            studentResultArray = (from s in studentArray1
                                  orderby s.Age, s.StudentName descending
                                  select s).ToArray();
            Console.WriteLine("stampa su array");
            Array.ForEach(studentResultArray, s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            studentResultList = (from s in studentList1
                                 orderby s.Age, s.StudentName descending
                                 select s).ToList();
            Console.WriteLine("\nstampa su list");
            studentResultList.
                ForEach(s => Console.WriteLine(s.StudentName + " age = " + s.Age));

            //Studiamo la clausola select - proiettiamo gli elementi della sequenza in una nuova forma
            Console.WriteLine("\nClausola select - LINQ method");
            Console.WriteLine("\nUso di tipi anonimi");
            //uso di tipi anonimi
            Array.ForEach(
                studentArray1.
                Select(s => new { Nome = s.StudentName, Eta = s.Age }).
                ToArray(),
                Console.WriteLine);
            studentList1.
                Select(s => new { Nome = s.StudentName, Eta = s.Age }).
                ToList().
                ForEach(p => Console.WriteLine(p.Nome));

            //uso di tipi diversi - il risultato di una elaborazione LINQ diventa una lista di tipo diverso
            Console.WriteLine("\nUso di tipi diversi");
            Persona[] personaResultArray = studentArray1.
                Select(s => new Persona() { Nome = s.StudentName, Eta = s.Age }).ToArray();
            Array.ForEach(personaResultArray, s => Console.WriteLine(s.Nome + " " + s.Eta));

            List<Persona> personaResultList =
                studentList1.
                Select(s => new Persona() { Nome = s.StudentName, Eta = s.Age }).
                ToList();
            personaResultList.ForEach(p => Console.WriteLine(p));

            Console.WriteLine("\nClausola select - LINQ query");
            var studentNames = (from s in studentArray1
                                select new { Nome = s.StudentName }).ToArray();
            Array.ForEach(studentNames, s => Console.WriteLine(s));
            var personNamesList = (from s in studentList1
                                   select new Persona() { Nome = s.StudentName, Eta = s.Age }).ToList();
            personNamesList.ForEach(p => Console.WriteLine(p.Nome + " " + p.Eta));

            //Group By
            //IEnumerable<IGrouping< int, Student>>
            var groupedResult = studentList1.GroupBy(s => s.Age);

            foreach (var group in groupedResult)
            {
                Console.WriteLine("Group key(Age) = {0}", group.Key);
                foreach (var student in group)
                {
                    Console.WriteLine("Student = {0}", student);
                }
                //calcoliamo una funzione di gruppo: min, max, avg, count
                // funzione count: quanti studenti con la stessa età
                Console.WriteLine("Numero studenti con la stessa età nel gruppo = {0}", group.Count());
                Console.WriteLine("Valore medio dei voti = {0}", group.Average(s => s.MediaVoti));
                Console.WriteLine("Voto massimo nel gruppo = {0}", group.Max(s => s.MediaVoti));
                Console.WriteLine("Voto minimo nel gruppo = {0}", group.Min(s => s.MediaVoti));
                //C# implementa anche il metodo ToLookup che fa la stessa cosa di GroupBy ma la 
                //differenza sta nel fatto che con grandi basi di dati ToLookup carica tutto il risultato in memoria
                //GroupBy carica il risultato associato a una chiave quando serve
                //https://stackoverflow.com/questions/10215428/why-are-tolookup-and-groupby-different
                //https://stackoverflow.com/a/10215531

            }
            Console.WriteLine("STAMPA RAGGRUPPAMENTO PER NOME");
            var groupedResult2 = studentList1.GroupBy(s => s.StudentName);

            foreach (var group in groupedResult2)
            {
                Console.WriteLine("Chiave di raggruppamento (Nome) = " + group.Key);
                foreach (var student in group)
                {
                    Console.WriteLine(student);
                }
                Console.WriteLine("Numero studenti omonimi: " + group.Count());
                Console.WriteLine("Voto medio degli omonimi: " + group.Average(s => s.MediaVoti));

            }

            //intersezione tra due collection - Join
            //creiamo un elenco di assenze di studenti
            List<Assenza> assenzeList1 = new List<Assenza>
            {
                new Assenza(){ID = 1, Giorno = DateTime.Today, StudentID = 1 },
                new Assenza(){ID = 2, Giorno = DateTime.Today.AddDays(-1) ,StudentID = 1 },
                new Assenza(){ID = 3, Giorno = DateTime.Today.AddDays(-3), StudentID = 1 },
                new Assenza(){ID = 4, Giorno = new DateTime(2020,11,30), StudentID = 2 },
                new Assenza(){ID = 5, Giorno = new DateTime(2020,11,8), StudentID = 3 }

            };
            //vogliamo riportare il nome dello studente e le date delle sue assenze 
            //facciamo una join tra la lista degli studenti e la lista delle assenze degli studenti e poi facciamo la proiezione del risultato su un nuovo oggetto
            var innerJoinStudentiAssenze = studentList1.Join(assenzeList1,
                s => s.StudentID,
                a => a.StudentID,
                (s, a) => new { ID = s.StudentID, Nome = s.StudentName, GiornoAssenza = a.Giorno });

            foreach (var obj in innerJoinStudentiAssenze)
            {
                Console.WriteLine($"ID = {obj.ID}, Nome = {obj.Nome}, GiornoAssenza = {obj.GiornoAssenza.ToShortDateString()}");
            }
            Console.ReadLine();


        }
    }
}
```

## Esercitazione – LINQ al Museo

Creare i seguenti [POCO](https://en.wikipedia.org/wiki/Plain_old_CLR_object):
Artista (Id, Nome, Cognome, Nazionalità)
Opera (Id, Titolo, Quotazione, FkArtistaId)
Personaggio (Id, Nome, FkOperaId)
Creare per ciascun POCO una collection (lista) di tali oggetti:
artisti è una lista di oggetti di tipo Artista;
opere è una lista di oggetti di tipo Opera;
personaggi è una lista di oggetti di tipo Personaggio;
Nota: le property che iniziano con Fk indicano una foreign key ossia una una property che “punta” ad una property di un altro oggetto. Ad esempio, i valori di FkOperaId devono corrispondere a valori dell’Id nella collection delle opere. In altri termini una foreign key è una property i cui valori devono corrispondere a valori già presenti nella collection a cui puntano.
Effettuare le seguenti query:

1. Stampare le opere di un dato autore (ad esempio Picasso)
2. Riportare per ogni nazionalità (raggruppare per nazionalità) gli artisti
3. Contare quanti sono gli artisti per ogni nazionalità (raggruppare per nazionalità e contare)
4. Trovare la quotazione media, minima e massima delle opere di Picasso
5. Trovare la quotazione media, minima e massima di ogni artista
6. Raggruppare le opere in base alla nazionalità e in base al cognome dell’artista (Raggruppamento in base a più proprietà)
7. Trovare gli artisti di cui sono presenti almeno 2 opere
8. Trovare le opere che hanno personaggi
9. Trovare le opere che non hanno personaggi
10. Trovare l’opera con il maggior numero di personaggi

### Svolgimento

```cs
//file Artista.cs
namespace LinqAlMuseo
{
    public class Artista
    {
        public int Id { get; set; }
        public string Nome { get; set; } = null!;
        public string Cognome { get; set; } = null!;
        public string? Nazionalita { get; set; }

        public override string ToString()
        {
            return string.Format($"[ID = {Id}, Nome = {Nome},  Cognome = {Cognome}, Nazionalità = {Nazionalita}]"); ;
        }

    }
}

//file Opera.cs
namespace LinqAlMuseo
{
    public class Opera
    {
        public int Id { get; set; }
        public string Titolo { get; set; } = null!;

        public decimal Quotazione { get; set; }

        public int FkArtista { get; set; }
        public override string ToString()
        {
            return String.Format($"[ID = {Id}, Titolo = {Titolo}, Quotazione = {Quotazione},  FkArtista = {FkArtista}]"); ;
        }

    }
}

//file Personaggio.cs
namespace LinqAlMuseo
{
    public class Personaggio
    {
        public int Id { get; set; }
        public string Nome { get; set; } = null!;
        public int FkOperaId { get; set; }
        public override string ToString()
        {
            return string.Format($"[ID = {Id}, Nome = {Nome}, FkOperaId = {FkOperaId}]"); ;
        }

    }
}

//file Program.cs - utilizzo di Top Level Statements
//Esercizio: LINQ al museo
using LinqAlMuseo;
using System.Globalization;
using System.Text;

//creazione delle collection
//si parte da quelle che non puntano a nulla, ossia quelle che non hanno chiavi esterne

IList<Artista> artisti = new List<Artista>()
            {
                new Artista(){Id=1, Cognome="Picasso", Nome="Pablo", Nazionalita="Spagna"},
                new Artista(){Id=2, Cognome="Dalì", Nome="Salvador", Nazionalita="Spagna"},
                new Artista(){Id=3, Cognome="De Chirico", Nome="Giorgio", Nazionalita="Italia"},
                new Artista(){Id=4, Cognome="Guttuso", Nome="Renato", Nazionalita="Italia"}
            };

//poi le collection che hanno Fk
IList<Opera> opere = new List<Opera>() {
                new Opera(){Id=1, Titolo="Guernica", Quotazione=50000000.00m , FkArtista=1},//opera di Picasso
                new Opera(){Id=2, Titolo="I tre musici", Quotazione=15000000.00m, FkArtista=1},//opera di Picasso
                new Opera(){Id=3, Titolo="Les demoiselles d’Avignon", Quotazione=12000000.00m,  FkArtista=1},//opera di Picasso
                new Opera(){Id=4, Titolo="La persistenza della memoria", Quotazione=16000000.00m,  FkArtista=2},//opera di Dalì
                new Opera(){Id=5, Titolo="Metamorfosi di Narciso", Quotazione=8000000.00m, FkArtista=2},//opera di Dalì
                new Opera(){Id=6, Titolo="Le Muse inquietanti", Quotazione=22000000.00m,  FkArtista=3},//opera di De Chirico

            };

IList<Personaggio> personaggi = new List<Personaggio>() {
                new Personaggio(){Id=1, Nome="Uomo morente", FkOperaId=1},//un personaggio di Guernica 
                new Personaggio(){Id=2, Nome="Un musicante", FkOperaId=2},
                new Personaggio(){Id=3, Nome="una ragazza di Avignone", FkOperaId=3},
                new Personaggio(){Id=4, Nome="una seconda ragazza di Avignone", FkOperaId=3},
                new Personaggio(){Id=5, Nome="Narciso", FkOperaId=5},
                new Personaggio(){Id=6, Nome="Una musa metafisica", FkOperaId=6},
            };

//impostiamo la console in modo che stampi correttamente il carattere dell'euro e che utilizzi le impostazioni di cultura italiana
Console.OutputEncoding = Encoding.UTF8;
Thread.CurrentThread.CurrentCulture = new CultureInfo("it-IT");

//Le query da sviluppare sono:
//Effettuare le seguenti query:
//1)    Stampare le opere di un dato autore (ad esempio Picasso)
//2)    Riportare per ogni nazionalità (raggruppare per nazionalità) gli artisti
//3)    Contare quanti sono gli artisti per ogni nazionalità (raggruppare per nazionalità e contare)
//4)    Trovare la quotazione media, minima e massima delle opere di Picasso
//5)    Trovare la quotazione media, minima e massima di ogni artista
//6)    Raggruppare le opere in base alla nazionalità e in base al cognome dell’artista (Raggruppamento in base a più proprietà)
//7)    Trovare gli artisti di cui sono presenti almeno 2 opere
//8)    Trovare le opere che hanno personaggi
//9)    Trovare le opere che non hanno personaggi
//10)   Trovare l’opera con il maggior numero di personaggi


//svolgimento delle query richieste
//1) Stampare le opere di un dato autore (ad esempio Picasso)
Console.WriteLine("**** 1) Stampare le opere di un dato autore (ad esempio Picasso)\n");
//facciamo prima il filtraggio con la Where e poi la join
var opereDiArtista = artisti.Where(a => a.Cognome == "Picasso").Join(opere,
    a => a.Id,
    o => o.FkArtista,
    (a, o) => o.Titolo);
opereDiArtista.ToList().ForEach(t => Console.WriteLine(t));
//altro metodo: facciamo prima la Join e poi il filtraggio con la Where sull'autore
Console.WriteLine();
var opereDiArtista2 = artisti.Join(opere,
    a => a.Id,
    o => o.FkArtista,
    (a, o) => new { a, o }).Where(t => t.a.Cognome == "Picasso");
opereDiArtista2.ToList().ForEach(t => Console.WriteLine(t.o.Titolo));
Console.WriteLine();
//altro modo:
//step n.1: calcoliamo l'id di Picasso
//step. n.2: calcoliamo le opere di quell'autore
var autore = artisti.Where(a => a.Cognome == "Picasso").FirstOrDefault();
if (autore != null)
{
    var opereDiArtista3 = opere.Where(o => o.FkArtista == autore.Id);
    opereDiArtista3.ToList().ForEach(t => Console.WriteLine(t.Titolo));
}


//2) Riportare per ogni nazionalità (raggruppare per nazionalità) gli artisti
Console.WriteLine("\n**** 2) Riportare per ogni nazionalità (raggruppare per nazionalità) gli artisti\n");

//raggruppare gli artisti per nazionalità
var artistiPerNazionalità = artisti.GroupBy(a => a.Nazionalita);
foreach (var group in artistiPerNazionalità)
{
    Console.WriteLine($"Nazionalità: {group.Key}");
    foreach (var artista in group)
    {
        Console.WriteLine($"\t{artista.Nome} {artista.Cognome}");
    }
}

//3) Contare quanti sono gli artisti per ogni nazionalità (raggruppare per nazionalità e contare)
Console.WriteLine("\n**** 3) Contare quanti sono gli artisti per ogni nazionalità (raggruppare per nazionalità e contare)\n");

foreach (var group in artistiPerNazionalità)
{
    Console.WriteLine($"Nazionalità: {group.Key} Numero artisti: {group.Count()}");
}

//4)Trovare la quotazione media, minima e massima delle opere di Picasso
Console.WriteLine("\n**** 4) Trovare la quotazione media, minima e massima delle opere di Picasso\n");

//troviamo le opere di Picasso
var opereDiPicasso = artisti.Where(a => a.Cognome == "Picasso")
    .Join(opere,
    a => a.Id,
    o => o.FkArtista,
    (a, o) => o).ToList();
//troviamo le quotazioni
var quotazioneMinima = opereDiPicasso.Min(o => o.Quotazione);
var quotazioneMedia = opereDiPicasso.Average(o => o.Quotazione);
var quotazioneMassima = opereDiPicasso.Max(o => o.Quotazione);
//stampiamo il risultato
Console.WriteLine($"Quotazione minima = {quotazioneMinima}, " +
    $"quotazione media = {quotazioneMedia:F2}, quotazione massima = {quotazioneMassima}");

//5)Trovare la quotazione media, minima e massima di ogni artista
Console.WriteLine("\n**** 5) Trovare la quotazione media, minima e massima di ogni artista\n");

//raggruppiamo per artista (usando FkArtista) e poi su ogni gruppo di opere calcoliamo le funzioni di gruppo
var operePerArtista = opere.GroupBy(o => o.FkArtista);

foreach (var group in operePerArtista)
{
    Console.Write($"Id artista = {group.Key} ");
    //voglio conoscere i dettagli dell'artista di cui so l'id
    var artista = artisti.Where(a => a.Id == group.Key).FirstOrDefault();
    if (artista != null)
    {
        Console.Write($"{artista.Nome} {artista.Cognome} ");
    }
    Console.WriteLine($"Quotazione minima = {group.Min(o => o.Quotazione):C2};" +
        $" media = {group.Average(o => o.Quotazione):C2};" +
        $" massima = {group.Max(o => o.Quotazione):C2}");
}

//stessa query - versione con inner join
//effettuiamo prima la join tra opere e artisti e poi il raggruppamento
var opereDiArtistaGroupBy = artisti.Join(opere,
    a => a.Id,
    o => o.FkArtista,
    (a, o) => new { a, o }).GroupBy(t => t.a.Id);

foreach (var group in opereDiArtistaGroupBy)
{
    Console.Write($"Id artista = {group.Key} |");
    var artistaOpera = group.FirstOrDefault();
    if (artistaOpera != null)
    {
        Console.Write($"Cognome = {artistaOpera.a.Cognome}");
    }
    Console.WriteLine($" | Quotazione media ={group.Average(t => t.o.Quotazione):C2} " +
    $" | Quotazione minima = {group.Min(t => t.o.Quotazione):C2} " +
    $" | Quotazione massima = {group.Max(t => t.o.Quotazione):C2} ");
}

//6)    Raggruppare le opere in base alla nazionalità e in base al cognome dell’artista (Raggruppamento in base a più proprietà)
Console.WriteLine("\n**** 6) Raggruppare le opere in base alla nazionalità e in base al cognome dell’artista (Raggruppamento in base a più proprietà)\n");
var opereDiArtistaGroupByMultiplo = artisti.Join(opere,
    a => a.Id,
    o => o.FkArtista,
    (a, o) => new { a, o }).GroupBy(t => new { t.a.Nazionalita, t.a.Cognome });

foreach (var group in opereDiArtistaGroupByMultiplo)
{
    //Console.WriteLine($"Chiave di raggruppamento = {group.Key}");
    //foreach (var item in group)
    //{
    //    Console.WriteLine($"Elemento = {item}");
    //}

    Console.WriteLine($"{group.Key.Nazionalita} {group.Key.Cognome} ");
    foreach (var item in group)
    {
        Console.WriteLine($"\tOpera = {item.o.Titolo}");
    }
}

//7)Trovare gli artisti di cui sono presenti almeno 2 opere
Console.WriteLine("\n**** 7) Trovare gli artisti di cui sono presenti almeno 2 opere\n");
//intanto calcolo gli artisti di cui ho almeno un'opera
var artistiConAlmeno1Opera = artisti.Join(opere,
    a => a.Id,
    o => o.FkArtista,
    (a, o) => a);
//per calcolare gli artisti di cui sono presenti almeno due opere procedo così:
//raggruppo gli artisti per FkArtista e successivamente filtro in base al conteggio degli 
//elementi in ogni gruppo
Console.WriteLine("Artisti con almeno due opere");
var artistiConAlmeno2Opere = opere.GroupBy(o => o.FkArtista)
    .Where(g => g.Count() >= 2).Join(artisti,
    g => g.Key,
    a => a.Id,
    (g, a) => a);
foreach (var artista in artistiConAlmeno2Opere)
{
    Console.WriteLine(artista);
}

//altra variante - riporta gli artisti con il relativo numero di opere
opere.GroupBy(o => o.FkArtista).
Select(group => new { group.Key, NumeroOpere = group.Count() }).
Where(g => g.NumeroOpere >= 2).
Join(artisti,
a2 => a2.Key,
a => a.Id,
(a2, a) => new { ID = a.Id, NomeArtista = a.Cognome, a2.NumeroOpere }
).ToList().ForEach(s => Console.WriteLine(s));

//foreach (var group in artistiConAlmeno2Opera)
//{
//    Console.WriteLine($"{group.Key}");
//}

//8)Trovare le opere che hanno personaggi
Console.WriteLine("\n**** 8) Trovare le opere che hanno personaggi\n");
//le opere con personaggi (è una semplice join)
var opereConPersonaggi = opere.Join(personaggi,
    o => o.Id,
    p => p.FkOperaId,
    (o, p) => o);
Console.WriteLine("Opere con personaggi");
foreach (var opera in opereConPersonaggi)
{
    Console.WriteLine(opera);
}

//9)Trovare le opere che non hanno personaggi
Console.WriteLine("\n**** 9) Trovare le opere che non hanno personaggi\n");
//opere senza personaggi: dall'insieme delle opere prendo solo quelle che non sono contenute in opereConPersonaggi
var opereSenzaPersonaggi = opere
    .Where(o => !opereConPersonaggi.Contains(o));

Console.WriteLine("Opere senza personaggi");
foreach (var opera in opereSenzaPersonaggi)
{
    Console.WriteLine(opera);
}

//10)Trovare le opere con il maggior numero di personaggi
Console.WriteLine("\n**** 10) Trovare le opere con il maggior numero di personaggi\n");
//primo step: calcolo il numero di personaggi per opera
var personaggiPerOpera = personaggi.
                GroupBy(p => p.FkOperaId).
                Select(group => new { IdOpera = group.Key, NumeroPersonaggi = group.Count() });
//secondo step: dobbiamo filtrare gli oggetti in modo da prendere solo quelli con il numero massimo di personaggi
var numeroMassimoPersonaggi = personaggiPerOpera.Max(t => t.NumeroPersonaggi);
//terzo step: filtro i dati in modo da prendere solo gli oggetti con il numero massimo di personaggi
var opereConMaxNumeroPersonaggi = personaggiPerOpera
    .Where(t => t.NumeroPersonaggi == numeroMassimoPersonaggi)
    .Join(opere,
    t => t.IdOpera,
    o => o.Id,
    (t, o) => new { id = o.Id, Titolo = o.Titolo, Personaggi = t.NumeroPersonaggi });
foreach (var item in opereConMaxNumeroPersonaggi)
{
    Console.WriteLine(item);
}

```
