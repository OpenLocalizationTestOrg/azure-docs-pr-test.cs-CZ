---
title: "aaaI používat mobilní služby, jak mi pomůže App Service?"
description: "Zjistěte, jaké výhody přináší služba App Service tooyour existující projekty Mobile Services."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 315cc6eedcdca6c3f9f9bb9fd5ec7baf655b7e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started"></a>Používám Mobile Services, jak mi pomůže App Service?
## <a name="overview"></a>Přehled
Vaše stávající služba Mobile Service je v bezpečí a bude i nadále podporována. Ale existují počet výhody hello *Azure App Service* poskytuje platformu pro své mobilní aplikace, které nejsou dostupné dnes v Mobile Services:

* Jednodušší, snazší a nákladově efektivní nabídka pro aplikace, které obsahují jak webové, tak mobilní klienty
* Nové hostitelské funkce včetně webových úloh, vlastní záznamy CNAME, lepší monitorování
* Předpřipravená integrace s nástrojem Traffic Manager
* Připojení tooyour místních prostředků a připojení VPN pomocí virtuální sítě v přidání tooHybrid připojení
* Monitorování, výstrahy a řešení potíží pro aplikace pomocí nástrojů NewRelic a AppInsights
* Širší spektrum hello základní výpočetní prostředky a ceny
* Integrované automatické škálování, vyrovnávání zatížení a monitorování výkonu
* Integrované funkce pro fázování, zálohování, vrácení zpět a testování za provozu

## <a name="new-hosting-features"></a>Nové hostitelské funkce
V *Azure App Service* hello *mobilní aplikace* hello back-end kód běží ve stejném kontejneru jako webové aplikace a aplikace API. Takto můžete využít všech funkcí hello v tomto kontejneru, včetně některých funkcí, které nejsou aktuálně dostupné v Mobile Services:

* Přidání neustále běžící back-end logiky přes webové úlohy
* Zajištění, že back-end kód bude vždy spuštěn
* Použití vlastních záznamů CNAME tooprovide popisné a stabilní názvy tooyour koncových bodů mobilního back-endu
* Geografické škálování aplikace pomocí nástroje Traffic Manager
* Zahrnutí jakýchkoli knihoven nebo balíčků, které chcete použít
* (Pro .NET) Využití jakékoli funkce technologie ASP.NET, včetně MVC
* (Pro platformu Node.js) Využívejte všechny čistý JavaScript library hello uzlu ekosystém, včetně běžné knihovny MVC.

## <a name="access-on-premises-data-using-vnet"></a>Přístup k lokálním datům přes virtuální síť
S Mobile Services Dnes už můžete tooaccess hybridní připojení místních prostředků. Existují však situace, kdy je lepší použít řešení se sítí VPN. S *Azure App Services* můžete pro back-end mobilní aplikace použít Azure VNet.

## <a name="use-your-favorite-backend-language"></a>Využití oblíbeného jazyka pro back-end
*Aplikační služba Azure* nabízí širší a bohatší podporu platforem ASP.NET a Node.js, včetně přístup k nejnovějším modulům runtime toohello.

## <a name="set-up-automatic-scale"></a>Nastavení automatického škálování
S Mobile Services běžely všechny instance back-end kódu na malých virtuálních počítačích. *Aplikační služba Azure* vám umožní tooselect hello velikost virtuálního počítače z mnohem širší možnosti. Můžete také rychle škálovat nahoru nebo na toohandle všechny příchozí zákazníka zatížení, podle různých metrik výkonu.

## <a name="be-in-hello-know"></a>Být v hello "vědět"
Tooissues reagovat v reálném čase díky monitorování a výstrahám tooautomatically oznámit vy a váš tým. Integrate pokročilé analýzy aplikací a monitorování funkcí z nástrojů New Relic a AppInsights tooget ještě podrobnější informace o výkonu mobilní aplikace. S *Azure App Service* nyní můžete nastavit upozornění na základě různých metrik výkonu, buď programově nebo prostřednictvím hello portálu Azure.

## <a name="keep-your-assets-safe"></a>Zabezpečení prostředků
Back-end a databáze je možné automaticky zálohovat. Kód a data jsou v bezpečí po havárii a snadno obnovený, což vám toorun vaší firmě s jistotou.

## <a name="ready-stage-go"></a>Připravit, naplánovat, start!
S *Azure App Service* je nyní možné pro své mobilní aplikace vytvořit několik privátních testovacích a přípravných prostředí. Pomocí těchto tooperform před nasazením. Swap tooproduction bez výpadků. Webové aplikace se načítají předem, zajištění hello mají zákazníci maximální pohodlí.

Výhody *App Service* pro existující Mobile Service můžete začít využívat tak, že si projdete tento [kurz](app-service-mobile-migrating-from-mobile-services.md).
