---
title: "Používání Azure Table Storage se sadou WebJobs SDK"
description: "Další informace o použití úložiště tabulek Azure pomocí WebJobs SDK. Vytváření tabulek, přidání entity do tabulky a čtení existující tabulky."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 451432cc-c780-4310-85d3-84f44fe48afe
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 13cfc788c14d714df7022ce003d34691cf73d121
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-with-the-webjobs-sdk"></a>Používání Azure Table Storage se sadou WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# ukázek kódu, které ukazují, jak číst a zapisovat tabulky úložiště Azure pomocí [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.

V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md) nebo [více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Některé zobrazit fragmenty kódu `Table` atribut používaných ve funkcích, které jsou [názvem ručně](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), která je není pomocí jeden z atributů aktivační události. 

## <a id="ingress"></a>Postup přidání entity do tabulky
K přidání entity do tabulky, použijte `Table` atribut s `ICollector<T>` nebo `IAsyncCollector<T>` parametr kde `T` Určuje schéma entity, které chcete přidat. Konstruktoru atributu přijímá řetězcový parametr, který určuje název tabulky. 

Následující ukázka kódu přidá `Person` entity do tabulky s názvem *příjem příchozích dat*.

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() { 
                        PartitionKey = "Test", 
                        RowKey = i.ToString(), 
                        Name = "Name" }
                    );
            }
        }

Obvykle typ můžete používat s `ICollector` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to. Z následujících `Person` třídy práce s kódem zobrazeným v předchozím `Ingress` metoda.

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

Pokud chcete pracovat přímo s úložištěm Azure API, můžete přidat `CloudStorageAccount` parametr podpis metody.

## <a id="monitor"></a>Sledování v reálném čase
Protože funkce příchozí přenos dat často zpracování velkých objemů dat, řídicím panelu WebJobs SDK poskytuje data monitorování v reálném čase. **Volání protokolu** části oznamuje, zda je stále spuštěna funkce.

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

**Volání podrobnosti** stránky sestavy průběhu funkce (počet entit, které jsou zapsány) je spuštěn a vám dává příležitost k přerušení ho. 

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

Po dokončení funkce **volání podrobnosti** stránky sestavy počet řádků, které jsou zapsány.

![Funkce pro příjem příchozích dat bylo dokončeno](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <a id="multiple"></a>Jak si více entit z tabulky.
Čtení tabulky, použijte `Table` atribut s `IQueryable<T>` parametr kde zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`.

Následující ukázka kódu čte a zaznamená všechny řádky z `Ingress` tabulky:

        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}", 
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <a id="readone"></a>Informace o načtení jedné entity z tabulky.
Došlo `Table` konstruktoru atributu s dva další parametry, které umožňují zadat klíč oddílu a klíč řádku, pokud chcete vytvořit vazbu k jedné tabulky entity.

Následující ukázka kódu čte pro řádek tabulky `Person` entit na základě oddílu klíč a řádek hodnot klíče dostali zprávu fronty:  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


`Person` Třídy v tomto příkladu nemá k implementaci `ITableEntity`.

## <a id="storageapi"></a>Jak používat rozhraní API .NET úložiště přímo do tabulky
Můžete také `Table` atribut s `CloudTable` objekt pro větší flexibilitu při práci s tabulkou.

Následující kód používá ukázka `CloudTable` objekt, který chcete přidat do jedné entity *příjem příchozích dat* tabulky. 

        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

Další informace o tom, jak používat `CloudTable` objektu, najdete v části [postup používání úložiště Table z .NET](../cosmos-db/table-storage-how-to-use-dotnet.md). 

## <a id="queues"></a>Související témata předmětem článek s postupy fronty
Informace o způsobu zpracování zpracování tabulky aktivovány zprávu fronty, nebo pro scénáře WebJobs SDK, které nejsou specifické pro zpracování tabulky, najdete v části [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Následující témata popsaná v tomto článku:

* Asynchronní funkce
* Více instancí
* Řádné vypnutí
* Použití atributů WebJobs SDK v tělo funkce
* Sada SDK připojovacích řetězců v kódu
* Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* Ruční aktivaci funkce
* Zápis protokolů

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s tabulek Azure. Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

