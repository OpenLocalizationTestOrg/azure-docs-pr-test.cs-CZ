---
title: "Podrobný přehled plánů služby Azure App Service | Microsoft Docs"
description: "Zjistěte, jak plánů služby App Service pro pracovní Azure App Service a jak budou využívat vaše prostředí pro správu."
keywords: "app service, azure app service, škálování, škálovatelné, plán služby App Service, náklady služby App Service"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: f97be571d104e3cc1c6ee732886fa7133ba0dc83
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a>Podrobný přehled plánů Azure App Service

Plány aplikační služby představují kolekci fyzické prostředky, které jsou použity k hostování vaší aplikace.

Plány služby App Service definují:

- Oblast (západní USA, Východ USA, atd.)
- Počet škálování (jeden, dva, tři instance atd.)
- Velikost instance (malé, střední, velké)
- SKU (Free, Shared, Basic, Standard, Premium)

Webové aplikace, mobilní aplikace, aplikace API, funkce aplikace (nebo funkce), v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) všechny spuštění v rámci plánu služby App Service.  Plán služby App Service můžete sdílet aplikací ve stejném předplatném a oblast. 

Všechny aplikace, které jsou přiřazené **plán služby App Service** sdílet prostředky definované ho. Tato sdílení úsporu nákladů při hostování více aplikacemi v jednom plánu služby App Service.

Váš **plán služby App Service** se může škálovat od skladových jednotek (SKU) úrovní **Free** a **Shared** po SKU úrovní **Basic**, **Standard** a **Premium** a vy při tom získáte přístup k dalším prostředkům a funkcím.

Pokud váš plán služby App Service je nastavený na **základní** SKU nebo vyšší, pak můžete řídit **velikost** a škálování počtu virtuálních počítačů.

Například pokud váš plán je nakonfigurovaný na použití dvě "malá" instance ve vrstvě služby na úrovni standard, všechny aplikace, které jsou přidruženy tento plán spustit na obou instancí. Aplikace také mají přístup k funkcím vrstvy služby na úrovni standard. Instancích plánu, na kterých běží aplikace jsou plně spravovaná a vysoce dostupné.

> [!IMPORTANT]
> **SKU** a **Škálování** plánu služby App Service určují cenu, ne počet aplikací hostovaných ve službě.

Tento článek popisuje klíčové vlastnosti, například vrstvy a škále plán služby App Service a jak se do play při správě aplikací.

## <a name="apps-and-app-service-plans"></a>Aplikace a plány služby App Service

Aplikace ve službě App Service může být přidružen pouze jeden plán služby App Service v daném okamžiku.

Aplikace a plány jsou součástí **skupiny prostředků**. Skupiny prostředků slouží jako hranice životního cyklu pro každý prostředek, který je v něm. Skupiny prostředků můžete spravovat všechny součásti aplikace společně.

Vzhledem k tomu, že jedna skupina prostředků může mít víc plány služby App Service, kterou můžete přidělit různé aplikace na různé fyzické prostředky.

Například můžete oddělit prostředků mezi vývoj, testování a provozním prostředí. S samostatných prostředí pro provoz a vývoj nebo testování umožňuje izolovat prostředky. Tímto způsobem není testování proti nové verze aplikace zatížení pokouší o stejné prostředky jako produkční aplikací, které slouží skutečnou zákazníků.

Pokud máte více plánů v jedna skupina prostředků, můžete také definovat aplikaci, která přesahuje zeměpisné oblasti.

Například vysokou dostupností aplikaci spuštěnou ve dvou oblastech obsahuje alespoň dva plánům, jeden pro každou oblast a jedna aplikace, které jsou spojené s každou plánu. V takovém případě jsou všechny kopie aplikace pak obsažené v jedna skupina prostředků. Skupinu prostředků s více schématy a víc aplikací mít usnadňuje správu, řízení a zobrazení stavu aplikace.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Vytvořit plán služby App Service nebo použít existující

Když vytvoříte aplikaci, měli byste zvážit vytvoření skupiny prostředků. Na druhé straně Pokud je tato aplikace součástí větší aplikace, vytvořte ho v rámci skupiny prostředků, který je přidělen pro větší aplikace.

Zda aplikace je zcela nové aplikace nebo součást většího, můžete použít existující plán ji hostovat nebo vytvořte novou. Toto rozhodnutí se více dotaz kapacitu a očekávané zatížení.

Doporučujeme, abyste izoluje aplikace do nové služby App Service při plánování:

- Aplikace je náročná.
- Aplikace má různých faktorech škálování z jiných aplikací hostovaná v existující plán.
- Aplikace musí prostředků v jiné zeměpisné oblasti.

Tímto způsobem můžete přidělit novou sadu prostředků pro vaši aplikaci a získáte větší kontrolu nad vaší aplikace.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

> [!TIP]
> Pokud máte služby App Service Environment, najdete v dokumentaci specifické pro prostředí App Service zde: [vytvořit plán služby App Service ve službě App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Prázdný plán služby App Service můžete vytvořit z prostředí procházení plán aplikační služby nebo jako součást vytváření aplikace.

V [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní**a potom vyberte **webové aplikace** nebo jiného typu aplikace služby App Service.

![Vytvoření aplikace v portálu Azure.][createWebApp]

Pak můžete vybrat nebo vytvořit plán služby App Service pro novou aplikaci.

 ![Vytvořte plán služby App Service.][createASP]

Chcete-li vytvořit plán služby App Service, klikněte na tlačítko **[+] vytvořit nové**, typ **plán služby App Service** název a potom vyberte odpovídající **umístění**. Klikněte na tlačítko **cenová úroveň**a potom vyberte příslušné cenovou úroveň pro službu. Vyberte **zobrazit všechny** do zobrazení více ceny možnosti, jako například **volné** a **sdílené**. Až vyberete cenovou úroveň, klikněte na **vyberte** tlačítko.

## <a name="move-an-app-to-a-different-app-service-plan"></a>Přesuňte aplikace na jiný plán služby App Service

Aplikace můžete přesunout na jiný plán služby App Service v [portál Azure](https://portal.azure.com). Aplikace můžete přesouvat mezi plány také plány jsou ve stejné skupině prostředků a geografické oblasti.

Přesunutí aplikace do jiného plánu:

- Přejděte do aplikace, který chcete přesunout.
- V **nabídky**, vyhledejte **plán služby App Service** části.
- Vyberte **plán služby App Service změnu** ke spuštění procesu.

**Plán služby App Service změnu** otevře **plán služby App Service** selektor. V tomto okamžiku můžete vybrat existující plán přesunout do této aplikace.

> [!IMPORTANT]
> Vyberte plán služby App Service uživatelského rozhraní se filtrují podle následujících kritérií:
> - Existuje v rámci stejné skupiny prostředků
> - Existuje ve stejné zeměpisné oblasti
> - Existuje v rámci stejné webový prostor
>
> Webový prostor je logická konstrukce v App Service, která definuje seskupení prostředků serveru. Geografické oblasti (například západní USA) obsahuje mnoho Webspaces aby bylo možné přidělit zákazníky používající službu aplikace. V současné době není možné přesunout mezi Webspaces prostředky služby App Service.
>

![Selektor plán služby App Service.][change]

Každý plán má svou vlastní cenová úroveň. Například přesun lokalitu z úrovně Free plán úrovně Standard, umožňuje všechny aplikace, které jsou přiřazené k použití funkcí a prostředků na plán úrovně Standard.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonování aplikace na jiný plán služby App Service

Pokud chcete přesunout aplikace v jiné oblasti, jeden alternativou je aplikace klonování. Klonování zhotoví kopii aplikaci v nové nebo existující plán služby App Service v libovolné oblasti.

Můžete najít **klonování aplikace** v **nástroje pro vývoj** části nabídky.

> [!IMPORTANT]
> Klonování má určitá omezení, které si můžete přečíst o na [klonování aplikace Azure App Service pomocí portálu Azure](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Škálovat plán služby App Service

Existují tři způsoby, jak škálovat plán:

- **Změna plánu je cenová úroveň**. Plán v základní vrstvě lze převést na Standard a všechny aplikace, které jsou přiřazené k použití funkcí na plán úrovně Standard.
- **Změňte velikost instance plánu**. Jako příklad můžete změnit plán v základní vrstvě, který používá malé instance na používání velké instancí. Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť a prostředky procesoru, které nabízí větší velikost instance.
- **Změnit počet instancí plánu**. Například standardní plán, který je na tři instance škálovat na více systémů můžete škálovat na 10 instancí. Plán Premium můžete škálovat na více systémů na 20 instance (přičemž podléhá dostupnosti). Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť a prostředky procesoru, které nabízí větší počet instancí.

Můžete změnit cenovou úroveň a instance velikost klepnutím **vertikálně navýšit kapacitu** položky v části nastavení pro aplikace nebo plán služby App Service. Změny použít na plán služby App Service a ovlivňují všechny aplikace, které je hostitelem.

 ![Nastavte hodnoty škálování aplikace.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Vyčištění plán služby App Service

> [!IMPORTANT]
> **Plánů služby App Service** mají žádné aplikace přidružené k je stále platit poplatky vzhledem k tomu, že budou nadále záložní kapacita výpočty.

Nechcete neočekávané poplatky, když se odstraní poslední aplikace hostovaná v plán služby App Service, je taky odstranit výsledné prázdný plán služby App Service.

## <a name="summary"></a>Souhrn

Plány aplikační služby představují sadu funkcí a kapacity, které můžete sdílet mezi aplikacemi. Plány služby App Service poskytují flexibilitu pro přidělovat konkrétní aplikace sadu prostředků a dále optimalizovat využití vaší prostředků Azure. Tímto způsobem, pokud chcete ušetřit peníze u svého testovacího prostředí, můžete plán sdílet mezi více aplikacemi. Pomocí příjmu v několika oblastech a plány a také můžete maximalizovat propustnost pro produkční prostředí.

## <a name="whats-changed"></a>Co se změnilo

- Průvodce změnou z webů na službu App Service, najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
