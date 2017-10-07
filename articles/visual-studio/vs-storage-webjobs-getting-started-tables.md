---
title: "aaaGetting Začínáme s Azure storage a Visual Studio připojené služeb (webové úlohy projekty)"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby pomocí Azure Table storage v Azure WebJobs projektu v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 80d9f8d8b493ce612623dfed7e89325fb154a1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Začínáme s Azure Storage (webová úloha Azure projekty)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Přehled
Tento článek obsahuje zobrazit C# ukázek kódu, které ukazují, jak toouse hello Azure WebJobs SDK verze 1.x s hello služby úložiště Azure table. Ukázky kódu Hello použít hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verze 1.x.

Hello služba úložiště Azure Table umožňuje toostore velkých objemů strukturovaná data. Hello služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z vnitřní a vnější hello cloudu Azure. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.  V tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) Další informace.

Některé hello fragmenty kódu ukazují hello **tabulky** atribut používaných ve funkcích, které se nazývají ručně, tedy ne prostřednictvím jeden z atributů hello aktivační události.

## <a name="how-tooadd-entities-tooa-table"></a>Jak tooadd entity tooa tabulky
tooadd entity tooa tabulky, použijte hello **tabulky** atribut s **ICollector<T>**  nebo **IAsyncCollector<T>**  parametr kde **T** Určuje schéma hello hello entit chcete tooadd. konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello hello tabulky.

Přidá technologie Hello následující ukázka kódu **osoba** entity tooa tabulku s názvem *příjem příchozích dat*.

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

Obvykle hello typ můžete používat s **ICollector** je odvozena z **TableEntity** nebo implementuje **ITableEntity**, ale nemusí to. Některý z následujících hello **osoba** třídy práce s kódem hello uvedené v předchozí hello **příjem příchozích dat** metoda.

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

Pokud chcete toowork přímo s hello úložiště Azure API, můžete přidat **CloudStorageAccount** podpis metody toohello parametr.

## <a name="real-time-monitoring"></a>Sledování v reálném čase
Protože funkce příchozí přenos dat často zpracování velkých objemů dat, hello řídicím panelu WebJobs SDK poskytuje data monitorování v reálném čase. Hello **volání protokolu** části řekne, pokud je stále spuštěná hello funkce.

![Příjem příchozích dat funkci spouštění](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

Hello **volání podrobnosti** stránky sestavy hello funkce průběh (počet entit, které jsou zapsány) při běží a poskytuje tooabort možnost ho.

![Příjem příchozích dat funkci spouštění](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

Když funkce hello dokončení hello **volání podrobnosti** stránky sestavy hello počet řádků, které jsou zapsány.

![Funkce pro příjem příchozích dat bylo dokončeno](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-tooread-multiple-entities-from-a-table"></a>Jak tooread více entit z tabulky.
tooread tabulky, použijte hello **tabulky** atribut s **IQueryable<T>**  parametr kde zadejte **T** je odvozena z **TableEntity**nebo implementuje **ITableEntity**.

Hello následující ukázka kódu čte a zaznamená všechny řádky z hello **příjem příchozích dat** tabulky:

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

### <a name="how-tooread-a-single-entity-from-a-table"></a>Jak tooread jedné entity z tabulky.
Je **tabulky** konstruktoru atributu s dva další parametry, které umožňují určit hello klíč oddílu a klíč řádku, pokud chcete toobind tooa jedné tabulky entity.

Hello následující ukázka kódu čte pro řádek tabulky **osoba** entit na základě oddílu klíč a řádek hodnot klíče dostali zprávu fronty:  

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


Hello **osoba** třídy v tomto příkladu nemá tooimplement **ITableEntity**.

## <a name="how-toouse-hello-net-storage-api-directly-toowork-with-a-table"></a>Jak toouse hello .NET API úložiště přímo toowork s tabulkou
Můžete taky hello **tabulky** atribut s **CloudTable** objekt pro větší flexibilitu při práci s tabulkou.

Hello následující kód používá ukázka **CloudTable** objektu tooadd jedné entity toohello *příjem příchozích dat* tabulky.

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

Další informace o tom, toouse hello **CloudTable** objektu, najdete v části [Začínáme s Azure Table storage pomocí rozhraní .NET](../storage/storage-dotnet-how-to-use-tables.md).

## <a name="related-topics-covered-by-hello-queues-how-tooarticle"></a>Související témata předmětem hello fronty postupy tooarticle
Informace o tom, jak zpracování tabulky toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není zpracování, najdete v části konkrétní tootable [Začínáme s Azure Queue storage a Visual Studio připojené služeb (webové úlohy projekty) ](../storage/vs-storage-webjobs-getting-started-queues.md).

## <a name="next-steps"></a>Další kroky
Tento článek poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s tabulek Azure. Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

