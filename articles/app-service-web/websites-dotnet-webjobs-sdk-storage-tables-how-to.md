---
title: "aaaHow toouse úložiště tabulek Azure s hello WebJobs SDK"
description: "Zjistěte, jak toouse Azure table storage s hello WebJobs SDK. Vytváření tabulek, přidejte tootables entity a čtení existující tabulky."
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
ms.openlocfilehash: 8e28c69df4a934646add9e50c6de28e76dca1636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-with-hello-webjobs-sdk"></a>Jak toouse Azure table storage s hello WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# – ukázky kódu, které zobrazují jak tooread a zápisu úložiště Azure tabulky pomocí [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.

Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md) nebo příliš[více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Některé hello fragmenty kódu ukazují hello `Table` atribut používaných ve funkcích, které jsou [názvem ručně](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), která je není pomocí jeden z atributů hello aktivační události. 

## <a id="ingress"></a>Jak tooadd entity tooa tabulky
tooadd entity tooa tabulky, použijte hello `Table` atribut s `ICollector<T>` nebo `IAsyncCollector<T>` parametr kde `T` Určuje schéma hello hello entit chcete tooadd. konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello hello tabulky. 

Přidá technologie Hello následující ukázka kódu `Person` entity tooa tabulku s názvem *příjem příchozích dat*.

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

Obvykle hello typ můžete používat s `ICollector` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to. Některý z následujících hello `Person` třídy práce s kódem hello uvedené v předchozí hello `Ingress` metoda.

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

Pokud chcete toowork přímo s hello úložiště Azure API, můžete přidat `CloudStorageAccount` podpis metody toohello parametr.

## <a id="monitor"></a>Sledování v reálném čase
Protože funkce příchozí přenos dat často zpracování velkých objemů dat, hello řídicím panelu WebJobs SDK poskytuje data monitorování v reálném čase. Hello **volání protokolu** části řekne, pokud je stále spuštěná hello funkce.

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

Hello **volání podrobnosti** stránky sestavy hello funkce průběh (počet entit, které jsou zapsány) při běží a poskytuje tooabort možnost ho. 

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

Když funkce hello dokončení hello **volání podrobnosti** stránky sestavy hello počet řádků, které jsou zapsány.

![Funkce pro příjem příchozích dat bylo dokončeno](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <a id="multiple"></a>Jak tooread více entit z tabulky.
tooread tabulky, použijte hello `Table` atribut s `IQueryable<T>` parametr kde zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`.

Hello následující ukázka kódu čte a zaznamená všechny řádky z hello `Ingress` tabulky:

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

### <a id="readone"></a>Jak tooread jedné entity z tabulky.
Došlo `Table` konstruktoru atributu s dva další parametry, které umožňují určit hello klíč oddílu a klíč řádku, pokud chcete toobind tooa jedné tabulky entity.

Hello následující ukázka kódu čte pro řádek tabulky `Person` entit na základě oddílu klíč a řádek hodnot klíče dostali zprávu fronty:  

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


Hello `Person` třídy v tomto příkladu nemá tooimplement `ITableEntity`.

## <a id="storageapi"></a>Jak toouse hello .NET API úložiště přímo toowork s tabulkou
Můžete taky hello `Table` atribut s `CloudTable` objekt pro větší flexibilitu při práci s tabulkou.

Hello následující kód používá ukázka `CloudTable` objektu tooadd jedné entity toohello *příjem příchozích dat* tabulky. 

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

Další informace o tom, toouse hello `CloudTable` objektu, najdete v části [jak toouse úložiště Table z .NET](../cosmos-db/table-storage-how-to-use-dotnet.md). 

## <a id="queues"></a>Související témata předmětem hello fronty postupy tooarticle
Informace o tom, jak zpracování tabulky toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není zpracování, najdete v části konkrétní tootable [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Obsahuje následující témata v tomto článku hello následující:

* Asynchronní funkce
* Více instancí
* Řádné vypnutí
* Použití atributů WebJobs SDK v hello tělo funkce
* Nastavení hello SDK připojovacích řetězců v kódu
* Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* Ruční aktivaci funkce
* Zápis protokolů

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s tabulek Azure. Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

