---
title: "plány aaaAzure využívání funkce a služby App Service | Microsoft Docs"
description: "Pochopte, jak Azure Functions škáluje toomeet hello potřeb vašich zatížení založeného na událostech."
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
ms.openlocfilehash: 3826915b93328635d9295efe90ecc421e6c56af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-consumption-and-app-service-plans"></a>Azure plány využívání funkce a služby App Service 

## <a name="introduction"></a>Úvod

Azure Functions můžete spustit ve dvou různých režimech: plánu spotřeby a plán služby Azure App Service. plánu spotřeby Hello automaticky přiděluje výpočetní výkon, pokud váš kód běží, horizontálně navýší kapacitu jako nezbytné toohandle zatížení a pak škáluje, pokud kód není spuštěna. Ano nemáte toopay nečinnosti virtuálních počítačů a nemáte dostatečnou kapacitu tooreserve předem. Tento článek se zaměřuje na plánu spotřeby hello. Podrobnosti o tom, jak funguje hello plán služby App Service najdete v tématu hello [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Pokud se nevyznáte v Azure Functions, přečtěte si téma hello [přehled Azure Functions](functions-overview.md).

Když vytvoříte aplikaci function app, je nutné nakonfigurovat plán hostování pro funkce hello aplikaci obsahuje. V obou režimech instanci hello *Azure Functions hostitele* provádí funkce hello. Typ Hello plán ovládacích prvků:

* Jak jsou hostiteli instance škálovat na více systémů.
* Hello prostředky, které jsou k dispozici tooeach hostitele.

V současné době je nutné vybrat typ plánu hello během vytváření hello hello funkce aplikace. Následně ho nelze změnit. 

Je možné škálovat mezi vrstvami na hello plán služby App Service. V plánu spotřeby hello Azure Functions automaticky zpracovává všechny přidělení prostředků.

## <a name="consumption-plan"></a>Plán Consumption

Pokud používáte plánu spotřeby, instance hostitele Azure Functions hello dynamicky přidat nebo odebrat podle hello počet příchozích událostí. Tento plán automaticky přizpůsobí a musíte platit za výpočetní prostředky jenom v případě, že funkce běží. V plánu spotřeby funkci můžete používat maximálně 10 minut. 

> [!NOTE]
> Hello výchozí časový limit pro funkce na plánu spotřeby je 5 minut. Hello hodnota může být vyšší too10 minut hello funkce aplikace tak, že změníte vlastnost hello `functionTimeout` v [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

Fakturace vychází čas spuštění a využité a je agregovat přes všechny funkce v rámci funkce aplikace. Další informace najdete v tématu hello [Azure Functions stránce s cenami].

plán spotřeby Hello je výchozí hello a nabízí následující výhody hello:
- Platí se pouze až nebudou spuštěny funkcí.
- Automaticky škálovat, i během období vysoké zatížení.

## <a name="app-service-plan"></a>Plán služby App Service

V App Service hello plánování, vaše aplikace funkce běží na vyhrazených virtuálních počítačích na Basic, Standard a Premium SKU, podobně jako tooWeb aplikace. Vyhrazených virtuálních počítačích jsou přidělené tooyour aplikace služby App Service, což znamená, že bude vždy spuštěn hello funkce hostitele.

Vezměte v úvahu plán služby App Service v následujících případech hello:
- Máte existující, nedostatečně virtuálních počítačů, které jsou již spuštěny jiné instance aplikace služby.
- Vaše aplikace toorun funkce předpokládáte nepřetržitě nebo téměř nepřetržitě.
- Budete potřebovat další možnosti procesoru nebo paměti, než je zadán plán spotřeby hello.
- Je nutné toorun delší než maximální dobu spuštění hello povolena v plánu spotřeby hello.

Virtuální počítač oddělí ze velikost modulu runtime a paměti. V důsledku toho nebude platit víc než hello náklady hello instance virtuálního počítače, které můžete přidělit. Podrobnosti o tom, jak funguje hello plán služby App Service najdete v tématu hello [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Plán služby App Service můžete ručně škálovat tak, že přidáte další instance virtuálních počítačů, nebo můžete povolit automatické škálování. Další informace najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Také můžete škálovat a vybrat jiný plán služby App Service. Další informace najdete v tématu [škálování aplikace v Azure](../app-service-web/web-sites-scale.md). Pokud plánujete toorun funkce jazyka JavaScript na plán služby App Service, měli byste vybrat plán, který má méně jádra. Další informace najdete v tématu hello [referenční dokumentace technologie JavaScript pro funkce](functions-reference-node.md#choose-single-core-app-service-plans).  

<!-- Note: hello portal links toothis section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###Always On

Pokud spustíte na plán služby App Service, měli byste povolit hello **Always On** nastavení tak, aby vaše aplikace funkce běží správně. Na plán služby App Service bude přejděte hello functions runtime nečinnosti po několika minutách nečinnosti, takže pouze aktivace protokolu HTTP bude "probuzení" funkcí. Toto je podobné toohow webové úlohy musí mít povolenou funkci Always On. 

Always On je k dispozici pouze na plán služby App Service. V plánu spotřeby hello platformy automaticky aktivuje funkce aplikace.

## <a name="storage-account-requirements"></a>Požadavky na účet úložiště

Spotřeba plánu nebo plán služby App Service funkce aplikace vyžaduje účet služby Azure Storage, který podporuje úložiště Azure Blob, fronty a tabulky. Azure Functions interně používá Azure Storage pro operace, jako je například Správa aktivační události a protokolování spuštěních funkce. Některé účty úložiště nepodporují fronty a tabulky, jako je například účty pouze objekt blob úložiště (včetně úložiště premium) a účty úložiště pro obecné účely s replikací zónově redundantní úložiště. Tyto účty jsou filtrovány z hello **účet úložiště** okno při vytváření aplikaci funkce.

toolearn Další informace o typy účtů úložiště, najdete v části [Představení služby Azure Storage hello](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-hello-consumption-plan-works"></a>Jak funguje plánu spotřeby hello

plánu spotřeby Hello automaticky přizpůsobí prostředků procesoru a paměti přidáním další instance hello funkce hostitele, na základě hello počtu událostí, které se jeho funkce aktivována. Každá instance hostitele funkce hello je omezená too1.5 GB paměti.

Při použití hello spotřeba hostování plán, jsou uloženy soubory kódu funkce v Azure sdílených složek v účtu úložiště hlavní hello. Pokud odstraníte účet úložiště hlavní hello, tento obsah se odstraní a nelze jej obnovit.

> [!NOTE]
> Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění tooa v zpracování nové objekty BLOB, je-li aplikaci funkce přešel nečinnosti. Po hello funkce aplikace běží, objekty BLOB jsou zpracovávány okamžitě. Tento počáteční tooavoid zpoždění, zvažte jednu hello následující možnosti:
> - Plán služby App Service použijte s povolenou funkci Always On.
> - Použijte jiný mechanismus tootrigger hello blob zpracování, např. zprávu fronty, který obsahuje název objektu blob hello. Příklad, naleznete v části [vstupní frontě aktivační události s objektem blob vazby](functions-bindings-storage-blob.md#input-sample).

### <a name="runtime-scaling"></a>Modul runtime škálování

Azure Functions využívá komponenty s názvem hello *škálování řadič* toomonitor hello frekvence událostí a zjistěte, zda tooscale out nebo snížit. Hello škálování řadiče pomocí heuristické metody pro každý typ aktivační události. Například pokud používáte aktivační procedury fronty Azure storage, škáluje na základě délka fronty hello a stáří hello hello nejstarší fronty zpráv.

Hello jednotka škálování je funkce aplikace hello. Při aplikaci funkce hello je škálovat na více systémů, další prostředky jsou přidělené toorun více instancí hello Azure Functions hostitele. Naopak jako výpočetní, že se snižuje vyžádání, hello škálování řadiče odebere funkce hostitele instance. Hello počet instancí se nakonec měřítko toozero při žádné funkce běží v rámci funkce aplikace.

![Škálování řadič sledování událostí a vytváření instancí](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Model fakturace

Fakturace plánu spotřeby hello je podrobně popsaná v hello [Azure Functions stránce s cenami]. Využití počty pouze hello čas kód funkce běží a je agregován na úrovni aplikace hello funkce. jednotky pro fakturaci se Hello následující: 
* **Spotřeba prostředků v GB sekundách (v GB-s)**. Vypočtená jako kombinace velikost paměti a dobu spuštění pro všechny funkce v rámci funkce aplikace. 
* **Spuštěních**. Počítají pokaždé, když funkce se spouštějí v odpovědi tooan události aktivované vazbu.

[Azure Functions stránce s cenami]: https://azure.microsoft.com/pricing/details/functions
