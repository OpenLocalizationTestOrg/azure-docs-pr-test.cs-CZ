---
title: "Azure plány využívání funkce a služby App Service | Microsoft Docs"
description: "Pochopte, jak Azure Functions rozšiřuje podle potřeby vaší událostmi řízené zatížení."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70e77e09b2e2116153159167af61776398904a3c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-consumption-and-app-service-plans"></a>Azure plány využívání funkce a služby App Service 

## <a name="introduction"></a>Úvod

Azure Functions můžete spustit ve dvou různých režimech: plánu spotřeby a plán služby Azure App Service. Plánu spotřeby automaticky přiděluje výpočetní výkon, když kód běží, horizontálně navýší kapacitu podle potřeby pro zpracování zatížení a potom škáluje, pokud kód není spuštěna. Ano nemusí platit pro nečinnosti virtuální počítače a nemusíte předem záložní kapacita. Tento článek se zaměřuje na plánu spotřeby. Podrobnosti o tom, jak funguje plán služby App Service najdete v tématu [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Pokud se nevyznáte v Azure Functions, přečtěte si téma [přehled Azure Functions](functions-overview.md).

Když vytvoříte aplikaci function app, je nutné nakonfigurovat plán hostování pro funkce, které obsahuje aplikaci. V obou režimech instanci *Azure Functions hostitele* provádí funkce. Typ plánu ovládacích prvků:

* Jak jsou hostiteli instance škálovat na více systémů.
* Prostředky, které jsou k dispozici na každém hostiteli.

V současné době je nutné vybrat typ plánu při vytváření aplikace funkce. Následně ho nelze změnit. 

Je možné škálovat mezi vrstvami na plán služby App Service. Azure Functions plánu spotřeby automaticky zpracovává všechny přidělení prostředků.

## <a name="consumption-plan"></a>Plán Consumption

Pokud používáte plánu spotřeby, instance hostitele Azure Functions dynamicky přidat nebo odebrat závislosti na počtu příchozích událostí. Tento plán automaticky přizpůsobí a musíte platit za výpočetní prostředky jenom v případě, že funkce běží. V plánu spotřeby funkci můžete používat maximálně 10 minut. 

> [!NOTE]
> Výchozí hodnota časového limitu pro funkce na plánu spotřeby je 5 minut. Hodnota je možné zvýšit na 10 minut pro aplikaci funkce Změna vlastnosti `functionTimeout` v [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

Fakturace vychází čas spuštění a využité a je agregovat přes všechny funkce v rámci funkce aplikace. Další informace najdete v tématu [Azure Functions stránce s cenami].

Spotřeba plán je výchozí a nabízí následující výhody:
- Platí se pouze až nebudou spuštěny funkcí.
- Automaticky škálovat, i během období vysoké zatížení.

## <a name="app-service-plan"></a>Plán služby App Service

V plánu služby App Service vaše funkce aplikace běží na vyhrazených virtuálních počítačích na Basic, Standard a Premium SKU, podobně jako webové aplikace. Vyhrazených virtuálních počítačích jsou přiděleny aplikace služby App Service, což znamená, že na hostiteli funkce vždy běží.

Vezměte v úvahu plán služby App Service v následujících případech:
- Máte existující, nedostatečně virtuálních počítačů, které jsou již spuštěny jiné instance aplikace služby.
- Funkce aplikací pro spuštění nepřetržitě nebo téměř nepřetržitě očekáváte.
- Budete potřebovat další možnosti procesoru nebo paměti, než je zadán plánu spotřeby.
- Budete muset spustit delší než maximální dobu spuštění povolena v plánu spotřeby.

Virtuální počítač oddělí ze velikost modulu runtime a paměti. V důsledku toho nebude platit víc než náklady na instanci virtuálního počítače, které můžete přidělit. Podrobnosti o tom, jak funguje plán služby App Service najdete v tématu [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Plán služby App Service můžete ručně škálovat tak, že přidáte další instance virtuálních počítačů, nebo můžete povolit automatické škálování. Další informace najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Také můžete škálovat a vybrat jiný plán služby App Service. Další informace najdete v tématu [škálování aplikace v Azure](../app-service-web/web-sites-scale.md). Pokud plánujete spouštět funkce jazyka JavaScript na plán služby App Service, měli byste vybrat plán, který má méně jádra. Další informace najdete v tématu [referenční dokumentace technologie JavaScript pro funkce](functions-reference-node.md#choose-single-core-app-service-plans).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###Always On

Pokud spustíte na plán služby App Service, měli byste povolit **Always On** nastavení tak, aby vaše aplikace funkce běží správně. Na plán služby App Service bude přejděte functions runtime nečinnosti po několika minutách nečinnosti, takže pouze aktivace protokolu HTTP bude "probuzení" funkcí. Toto je podobná jak webové úlohy musí mít povolenou funkci Always On. 

Always On je k dispozici pouze na plán služby App Service. V plánu spotřeby platformou automaticky aktivuje funkce aplikace.

## <a name="storage-account-requirements"></a>Požadavky na účet úložiště

Spotřeba plánu nebo plán služby App Service funkce aplikace vyžaduje účet služby Azure Storage, který podporuje úložiště Azure Blob, fronty a tabulky. Azure Functions interně používá Azure Storage pro operace, jako je například Správa aktivační události a protokolování spuštěních funkce. Některé účty úložiště nepodporují fronty a tabulky, jako je například účty pouze objekt blob úložiště (včetně úložiště premium) a účty úložiště pro obecné účely s replikací zónově redundantní úložiště. Tyto účty jsou filtrovány z **účet úložiště** okno při vytváření aplikaci funkce.

Další informace o typech účtu úložiště najdete v tématu [Představení služby Azure Storage](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Jak funguje s plánem spotřeba

Plánu spotřeby automaticky přizpůsobí prostředků procesoru a paměti přidáním další instance funkce hostitele, na základě počtu událostí, které se jeho funkce aktivována. Každá instance hostitele funkce je omezený na 1,5 GB paměti.

Při spotřeby hostování plán, jsou uloženy soubory kódu funkce na Azure sdílených složek v účtu úložiště hlavní. Pokud odstraníte účet úložiště hlavní, tento obsah se odstraní a nelze jej obnovit.

> [!NOTE]
> Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění při zpracování nové objekty BLOB, pokud aplikaci funkce přešel nečinnosti. Po aplikaci funkce běží, objekty BLOB jsou zpracovávány okamžitě. Abyste se vyhnuli Tato počáteční prodleva, zvažte jednu z následujících možností:
> - Plán služby App Service použijte s povolenou funkci Always On.
> - Použijte jiný mechanismus pro aktivaci objektu blob zpracování, např. zprávu fronty, který obsahuje název objektu blob. Příklad, naleznete v části [vstupní frontě aktivační události s objektem blob vazby](functions-bindings-storage-blob.md#input-sample).

### <a name="runtime-scaling"></a>Modul runtime škálování

Azure Functions využívá komponenty s názvem *škálování řadiče* ke sledování frekvence událostí a určení, zda horizontální navýšení kapacity nebo snížit. Škálování řadiče pomocí heuristické metody pro každý typ aktivační události. Například pokud používáte aktivační procedury fronty Azure storage, škáluje na základě délka fronty a stáří nejstarší zprávy ve frontě.

Jednotka škálování je funkce aplikace. Když je funkce aplikace škálovat na více systémů, další prostředky jsou přiděleny spustit víc instancí služby Azure Functions hostitele. Naopak jako výpočetní, že se snižuje vyžádání, řadičem škálování odebere funkce hostitele instance. Počet instancí se nakonec měřítko nula. Pokud žádná funkce běží v rámci funkce aplikace.

![Škálování řadič sledování událostí a vytváření instancí](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Model fakturace

Fakturace plánu spotřeby je podrobně popsán na [Azure Functions stránce s cenami]. Využití počty jenom čas, kód funkce běží a je agregován na úrovni aplikace funkce. Tady jsou jednotky pro fakturaci: 
* **Spotřeba prostředků v GB sekundách (v GB-s)**. Vypočtená jako kombinace velikost paměti a dobu spuštění pro všechny funkce v rámci funkce aplikace. 
* **Spuštěních**. Počítají pokaždé, když provedení funkce v reakci na událost, spuštěnou vazbu.

[Azure Functions stránce s cenami]: https://azure.microsoft.com/pricing/details/functions
