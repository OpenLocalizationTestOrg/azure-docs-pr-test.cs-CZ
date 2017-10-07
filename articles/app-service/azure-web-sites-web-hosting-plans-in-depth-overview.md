---
title: "podrobný přehled plánů služby App Service aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: b384790d9e69b234ca69ac591164c48a4b6ed210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a>Podrobný přehled plánů Azure App Service

Plány aplikační služby představují hello kolekce toohost fyzické prostředky, které používá vaše aplikace.

Plány služby App Service definují:

- Oblast (západní USA, Východ USA, atd.)
- Počet škálování (jeden, dva, tři instance atd.)
- Velikost instance (malé, střední, velké)
- SKU (Free, Shared, Basic, Standard, Premium)

Webové aplikace, mobilní aplikace, aplikace API, funkce aplikace (nebo funkce), v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) všechny spuštění v rámci plánu služby App Service.  Aplikace v hello stejném předplatném, oblasti můžete sdílet plán služby App Service. 

Všechny aplikace přiřazené tooan **plán služby App Service** sdílet prostředky hello definovaná tímto. Tato sdílení úsporu nákladů při hostování více aplikacemi v jednom plánu služby App Service.

Vaše **plán služby App Service** možné škálovat od **volné** a **sdílené** SKU příliš**základní**, **standardní**, a **Premium** SKU, která poskytuje přístup k prostředkům toomore a funkcí společně hello způsobem.

Pokud váš plán služby App Service je nastaven příliš**základní** SKU nebo vyšší, pak můžete řídit hello **velikost** a škálovat počet hello virtuálních počítačů.

Například pokud váš plán je nakonfigurované toouse dva "malá" instance ve vrstvě služby na úrovni standard hello, všechny aplikace, které jsou přidruženy tento plán spustit na obou instancí. Aplikací mít také funkce úrovně služby na úrovni standard toohello přístup. Instancích plánu, na kterých běží aplikace jsou plně spravovaná a vysoce dostupné.

> [!IMPORTANT]
> Hello **SKU** a **škálování** z hello služby App Service určuje plán hello náklady a není hello počet aplikací, které jsou v ní umístěné.

Tento článek popisuje hello klíčové vlastnosti, například vrstvy a škále plán služby App Service a jak se do play při správě aplikací.

## <a name="apps-and-app-service-plans"></a>Aplikace a plány služby App Service

Aplikace ve službě App Service může být přidružen pouze jeden plán služby App Service v daném okamžiku.

Aplikace a plány jsou součástí **skupiny prostředků**. Skupiny prostředků slouží jako hranice životního cyklu hello každý prostředek, který je v něm. Můžete použít toomanage skupiny prostředků všechny hello částí aplikace společně.

Vzhledem k tomu, že jedna skupina prostředků může mít víc plány služby App Service, kterou můžete přidělit různé aplikace toodifferent fyzické prostředky.

Například můžete oddělit prostředků mezi vývoj, testování a provozním prostředí. S samostatných prostředí pro provoz a vývoj nebo testování umožňuje izolovat prostředky. Tímto způsobem zatížení testování proti nové verze aplikace není pokouší o hello stejné prostředky jako produkční aplikací, které slouží skutečnou zákazníků.

Pokud máte více plánů v jedna skupina prostředků, můžete také definovat aplikaci, která přesahuje zeměpisné oblasti.

Například vysokou dostupností aplikaci spuštěnou ve dvou oblastech obsahuje alespoň dva plánům, jeden pro každou oblast a jedna aplikace, které jsou spojené s každou plánu. V takovém případě jsou všechny kopie hello aplikace hello pak obsažené v jedna skupina prostředků. Skupinu prostředků s více schématy a víc aplikací mít umožňuje snadno toomanage, řízení a zobrazit stav hello aplikace hello.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Vytvořit plán služby App Service nebo použít existující

Když vytvoříte aplikaci, měli byste zvážit vytvoření skupiny prostředků. Na hello druhé straně, pokud je tato aplikace součástí větší aplikace, vytvořte ji v rámci skupiny prostředků hello, který je přidělen pro větší aplikace.

Zda je aplikace hello zcela nové aplikace nebo součást většího, můžete toouse existující plán toohost ji nebo vytvořte novou. Toto rozhodnutí se více dotaz kapacitu a očekávané zatížení.

Doporučujeme, abyste izoluje aplikace do nové služby App Service při plánování:

- Aplikace je náročná.
- Aplikace má různých faktorech škálování z hello jiné aplikace hostované v existující plán.
- Aplikace musí prostředků v jiné zeměpisné oblasti.

Tímto způsobem můžete přidělit novou sadu prostředků pro vaši aplikaci a získáte větší kontrolu nad vaší aplikace.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

> [!TIP]
> Pokud máte služby App Service Environment, můžete zkontrolovat hello dokumentace konkrétní tooApp prostředí Service zde: [vytvořit plán služby App Service ve službě App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Prázdný plán služby App Service můžete vytvořit z hello prostředí procházení plán aplikační služby nebo jako součást vytváření aplikace.

V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní**a potom vyberte **webové aplikace** nebo jiného typu aplikace služby App Service.

![Vytvoření aplikace v hello portálu Azure.][createWebApp]

Pak můžete vybrat nebo vytvořit hello plán služby App Service pro novou aplikaci hello.

 ![Vytvořte plán služby App Service.][createASP]

Klikněte na tlačítko toocreate plán služby App Service **[+] vytvořit nové**, typ hello **plán služby App Service** název a potom vyberte odpovídající **umístění**. Klikněte na tlačítko **cenová úroveň**a potom vyberte příslušné cenovou úroveň služby hello. Vyberte **zobrazit všechny** tooview více ceny možnosti, jako například **volné** a **sdílené**. Po výběru hello cenové úrovně, klikněte na tlačítko hello **vyberte** tlačítko.

## <a name="move-an-app-tooa-different-app-service-plan"></a>Přesunutí aplikace tooa jiný plán služby App Service

Aplikace tooa jiný plán služby App Service můžete přesunout v hello [portál Azure](https://portal.azure.com). Aplikace můžete přesunout mezi plány také plány hello jsou v hello stejnou skupinu prostředků a geografické oblasti.

toomove plán tooanother aplikace:

- Přejděte toohello aplikace, které chcete toomove.
- V hello **nabídky**, vyhledejte hello **plán služby App Service** části.
- Vyberte **plán služby App Service změnu** toostart hello procesu.

**Plán služby App Service změnu** otevře hello **plán služby App Service** selektor. V tomto okamžiku můžete vybrat existující plán toomove do této aplikace.

> [!IMPORTANT]
> plán služby App Service vyberte Hello uživatelského rozhraní se filtrují podle hello následující kritéria:
> - Existuje v rámci hello stejné skupiny prostředků
> - Existuje v hello stejné zeměpisné oblasti
> - Existuje v rámci hello stejný webový prostor
>
> Webový prostor je logická konstrukce v App Service, která definuje seskupení prostředků serveru. Geografické oblasti (například západní USA) obsahuje mnoho Webspaces v pořadí tooallocate zákazníky používající službu aplikace. V současné době prostředky služby App Service nejsou možné toobe přesouvat mezi Webspaces.
>

![Selektor plán služby App Service.][change]

Každý plán má svou vlastní cenová úroveň. Přesun lokalitu z úrovně Standard tooa úroveň Free, například umožňuje všechny aplikace, které jsou přiřazeny tooit toouse hello funkce a prostředky hello úrovně Standard.

## <a name="clone-an-app-tooa-different-app-service-plan"></a>Klonování aplikace tooa jiný plán služby App Service

Pokud chcete toomove hello aplikace tooa jiné oblasti, jeden alternativou je aplikace klonování. Klonování zhotoví kopii aplikaci v nové nebo existující plán služby App Service v libovolné oblasti.

Můžete najít **klonování aplikace** v hello **nástroje pro vývoj** hello nabídky.

> [!IMPORTANT]
> Klonování má určitá omezení, které si můžete přečíst o na [klonování aplikace Azure App Service pomocí portálu Azure](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Škálovat plán služby App Service

Existují tři způsoby tooscale plánu:

- **Změnit plán hello je cenová úroveň**. Plán v základní vrstvě hello může být převedená tooStandard a všechny aplikace přiřazené tooit toouse hello funkce úrovně Standard hello.
- **Změňte velikost instance plánu hello**. Jako příklad plán v základní vrstvě hello, který používá malé instance může být změněné toouse velké instancí. Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť hello a prostředky procesoru, které hello větší velikost nabídky instanci.
- **Změnit počet instancí plánu hello**. Například standardní plán, který je škálovat instance toothree lze škálovat too10 instance. Plán Premium můžete škálovat instance too20 (subjektu tooavailability). Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť hello a prostředky procesoru, které hello větší počet nabídky instanci.

Můžete změnit hello cenové úrovně a instance velikosti kliknutím hello **vertikálně navýšit kapacitu** položky v části nastavení pro aplikace hello nebo hello plán služby App Service. Změny použít toohello plán služby App Service a ovlivňují všechny aplikace, které ji hostuje.

 ![Nastavte hodnoty tooscale aplikaci.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Vyčištění plán služby App Service

> [!IMPORTANT]
> **Plánů služby App Service** mají žádné aplikace přidružené k toothem stále platit poplatky vzhledem k tomu, že budou pokračovat v práci tooreserve hello výpočetní kapacitu.

tooavoid neočekávané poplatky, když se odstraní poslední aplikace hello hostovaná v plán služby App Service hello výsledné prázdný, je taky odstranit plán služby App Service.

## <a name="summary"></a>Souhrn

Plány aplikační služby představují sadu funkcí a kapacity, které můžete sdílet mezi aplikacemi. Udělte hello flexibilitu tooallocate konkrétní aplikace tooa sadu prostředků a dále optimalizovat využití vaší prostředků Azure plánů služby App Service. Tímto způsobem, pokud chcete toosave peníze u svého testovacího prostředí, můžete plán sdílet mezi více aplikacemi. Pomocí příjmu v několika oblastech a plány a také můžete maximalizovat propustnost pro produkční prostředí.

## <a name="whats-changed"></a>Co se změnilo

- Průvodce toohello změnu z tooApp weby služby, najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
