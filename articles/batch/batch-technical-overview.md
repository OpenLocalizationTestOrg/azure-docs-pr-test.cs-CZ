---
title: "aaaAzure Batch spouští rozsáhlé paralelní výpočetní řešení v cloudu hello | Microsoft Docs"
description: "Další informace o použití hello služby Azure Batch pro rozsáhlé paralelní a úlohy HPC"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: acc52e46330c465f81951441d9067371098cf63a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Spuštění vnitřně paralelních úlohy pomocí služby Batch

Azure Batch je služba platformy pro efektivní spuštění aplikace rozsáhlé paralelní a vysoce výkonné computing (HPC) v cloudu hello. Azure Batch plány výpočetně náročných toorun ve spravované kolekci virtuálních počítačů, a dokáže automaticky škálovat výpočetní prostředky toomeet hello potřeby vašich úloh.

Pomocí služby Azure Batch můžete snadno definovat Azure výpočetní prostředky tooexecute aplikace paralelně a škálovaně. Neexistuje žádné toomanually potřeba vytvářet, konfigurovat a spravovat HPC cluster, jednotlivé virtuální počítače, virtuální sítě nebo komplexních úloh a úloh plánování infrastruktury. Azure Batch pro vás tyto úlohy automatizuje nebo zjednodušuje.

## <a name="use-cases-for-batch"></a>Případy použití služby Batch
Batch je spravovaná služba Azure, která slouží k *dávkovému zpracování* a *dávkovým výpočtům* – spouštění velkého objemu podobných úloh pro požadovaný výsledek. Dávkové zpracování nejčastěji používají organizace, které pravidelně zpracovávají, transformují a analyzují velké objemy dat.

Služba Batch pracuje s vnitřně paralelními aplikacemi a úlohami (také známé jako „jednoduše paralelně zpracovatelné“). Vnitřně paralelní úlohy se snadno rozdělují na více úkolů, které provádějí práci současně na mnoha počítačích.

![Paralelní úkoly][1]<br/>

Mezi příklady úloh, které se běžně zpracovávají touto technikou, patří:

* Modelování finančních rizik
* Analýza dat klimatu a hydrologie
* Vykreslování, analýza a zpracování obrázků
* Kódování a překódování multimédií
* Genetická sekvenční analýza
* Analýza zátěže v inženýrství
* Testování softwaru

Batch můžete také provádět paralelní výpočty s fází reduce na konci hello a provést složitější úlohy v prostředí HPC, jako [rozhraní MPI (Message Passing)](batch-mpi.md) aplikace.

Porovnání mezi službou Batch a dalšími možnostmi řešení prostředí HCP najdete v části [Řešení Batch a HPC](batch-hpc-solutions.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="scenario-scale-out-a-parallel-workload"></a>Scénář: Horizontální navýšení kapacity paralelní úlohy
Běžné řešení, které používá rozhraní API služby Batch toointeract hello s hello služby Batch, zahrnuje škálování vnitřně paralelní práce – například hello vykreslování obrázků pro 3D scény – na fond výpočetních uzlů. Tento fond výpočetních uzlů může být "vykreslovací farma", který poskytuje desítky, stovky nebo dokonce tisíce jader tooyour vykreslování úlohy, například.

Hello následující diagram ukazuje běžné pracovní postup služby Batch s klientskou aplikaci nebo hostované služby Batch toorun pomocí paralelní úlohy.

![Pracovní postup řešení Batch][2]

V tomto běžném scénáři vaše aplikace nebo služba zpracovává výpočetní úlohu ve službě Azure Batch provedením hello následující kroky:

1. Nahrát hello **vstupní soubory** a hello **aplikace** bude zpracovávat tyto soubory tooyour účet úložiště Azure. Hello vstupní soubory mohou být všechna data, která vaše aplikace zpracuje, například data finančního modelování nebo video soubory toobe převodu. soubory aplikace Hello může být jakékoli aplikace, která se používá pro zpracování dat hello, jako je aplikace pro 3D vykreslování nebo převaděč médií.
2. Vytvoření dávky **fondu** výpočetních uzlů ve vašem účtu Batch – tyto uzly jsou hello virtuálních počítačů, které budou vaše úkoly provádět. Zadejte vlastnosti, například hello [velikost uzlu](../cloud-services/cloud-services-sizes-specs.md), jejich operačního systému a hello umístění ve službě Azure Storage tooinstall aplikace hello při hello uzly připojí hello fondu (hello aplikace, který jste odeslali v kroku #1). Můžete také konfigurovat fond hello příliš[automaticky škálovat](batch-automatic-scaling.md) v odpovědi toohello zatížení, které vaše úlohy generují. Automatické škálování dynamicky upraví hello počet výpočetních uzlů ve fondu hello.
3. Vytvoření dávky **úlohy** toorun hello zatížení hello fond výpočetních uzlů. Když vytvoříte úlohu, přiřaďte ji k fondu služby Batch.
4. Přidat **úlohy** toohello úlohy. Když přidáte úkoly tooa úlohy, hello služba Batch automaticky naplánuje hello úlohy pro spuštění na hello výpočetních uzlech ve fondu hello. Každý úkol používá hello aplikace, který jste nahráli tooprocess hello vstupní soubory.
   
   * 4a. Před spuštěním úlohy, můžete stáhnout hello data (vstupní soubory hello), která je tooprocess toohello výpočetním uzlu, které je přiřazen. Pokud aplikace hello již není nainstalován v uzlu hello (viz krok #. 2), ho můžete stáhnout zde místo. Po dokončení stahování hello hello úkoly spustí v jejich přiřazených uzlech.
5. Jako hello úkoly spouštějí, se můžete dotazovat Batch toomonitor hello průběh hello úloh a jejich úkolů. Klientská aplikace nebo služba komunikuje přes protokol HTTPS hello služby Batch. Vzhledem k tomu může být monitorování tisíce úkolů spuštěných v tisících výpočetních uzlů, ujistěte se, příliš[efektivní dotazování služby Batch hello](batch-efficient-list-queries.md).
6. Dokončení úlohy hello, mohou odesílat data tooAzure jejich výsledek úložiště. Soubory můžete také načíst přímo z hello systému souborů na výpočetním uzlu.
7. Když funkce monitorování zjistí, že jste dokončili hello úkoly ve vaší úloze, klientská aplikace nebo služba může stáhnout hello výstupní data pro další zpracování nebo vyhodnocení.

Zachovat v paměti je to jednorázové jednoduché toouse Batch, a tento scénář popisuje pouze několik dostupných funkcí. Například můžete spustit [několik paralelních úkolů](batch-parallel-node-tasks.md) na každém výpočetním uzlu, a můžete použít [úkoly přípravy a dokončení úlohy](batch-job-prep-release.md) tooprepare hello uzlů pro úlohy a poté je vyčistit.

## <a name="next-steps"></a>Další kroky
Teď, když máte přehled hello služby Batch, je čas toodig hlubší toolearn použití ho tooprocess paralelní úlohy náročné na výkon.

* Čtení hello [přehled funkcí Batch pro vývojáře](batch-api-basics.md), základní informace pro každý, kdo Příprava toouse dávky. Hello článek obsahuje podrobnější informace o prostředky služby Batch, například fondy, uzlů, úlohy a úkoly a hello mnoho funkcí rozhraní API, které můžete použít při vytváření aplikace Batch.
* Další informace o hello [nástroje a rozhraní API služby Batch](batch-apis-tools.md) dostupné pro vytváření řešení Batch.
* [Začínáme s knihovnou Azure Batch hello pro .NET](batch-dotnet-get-started.md) toolearn jak toouse C# a hello tooexecute knihovny Batch .NET jednoduché úlohy pomocí běžné pracovní postup služby Batch. Tento článek by měl být jeden z vašich prvních zastávek při učení jak toouse hello služby Batch. K dispozici je také [verze Python](batch-python-tutorial.md) hello kurzu.
* Stáhnout hello [ukázky kódů v Githubu] [ github_samples] toosee, jak můžete pomocí ukázkové úlohy Batch tooschedule a proces rozhraní jak C# a Python.
* Podívejte se na hello [Batch studijní] [ learning_path] tooget představu o hello prostředky k dispozici tooyou jako jste další toowork pomocí služby Batch.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
