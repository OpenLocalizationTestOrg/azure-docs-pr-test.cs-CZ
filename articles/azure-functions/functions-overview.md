---
title: "aaaAzure přehled Functions | Microsoft Docs"
description: "Pochopit, jak toouse Azure Functions toooptimize asynchronních úloh v minutách."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 668f9055a007fee662a4c7ec774d3c0a1d847800
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-functions"></a>Funkce tooAzure Úvod  
Azure Functions je řešení umožňující snadno spouštět malé části kódu, nebo "funkce" v cloudu hello. Můžete napsat právě hello kód, které že budete potřebovat pro hello problém, bez obav o celou aplikaci nebo hello toorun infrastruktury. Služba Functions může ještě zvýšit produktivitu vývoje, a navíc k vývoji můžete použít jazyk podle vlastní volby, například C#, F#, Node.js, Python nebo PHP. Platí se pouze za hello v době běhu kódu a důvěřovat Azure tooscale podle potřeby. Služba Azure Functions umožňuje v Microsoft Azure vytvářet aplikace bez serveru.

Toto téma obsahuje obecný přehled Azure Functions. Pokud chcete toojump práva a začít pracovat s Azure Functions, začněte s [vytvoření první funkce Azure](functions-create-first-azure-function.md). Pokud hledáte další odborné informace o funkcích, přečtěte si téma hello [referenční informace pro vývojáře](functions-reference.md).

## <a name="features"></a>Funkce
Toto jsou některé klíčové funkce Azure Functions:

* **Volba jazyka** – Pište funkce pomocí jazyka C#, F#, Node.js, Python, F#, PHP, Batch, Bash nebo libovolného spustitelného jazyka.
* **Platím za použití cenový model** – platíte jenom pro hello čas strávený spuštěním kódu. V tématu Plánování možnost v hello hostování spotřeba hello [části týkající se cen](#pricing).  
* **Přineste si vlastní závislosti** – Functions podporuje NuGet a NPM, takže můžete používat své oblíbené knihovny.  
* **Integrované zabezpečení** – Chraňte funkce aktivované protokolem HTTP pomocí poskytovatelů OAuth, jako jsou Azure Active Directory, Facebook, Google, Twitter a účet Microsoft.  
* **Zjednodušená integrace** – Snadné využívání služeb Azure a nabídek softwaru jako služby (SaaS). V tématu hello [části týkající se integrace](#integrations) některé příklady.  
* **Flexibilní vývoj** – kódujte funkce přímo hello portálu nebo nastavte průběžnou integraci a nasaďte kód prostřednictvím GitHub, Visual Studio Team Services a další [podporovaných vývojových nástrojů](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Open-source** -hello Functions runtime je typu open source a [dostupná na Githubu](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Co můžu dělat s Functions?
Azure Functions je vynikající řešení pro zpracování dat, integraci systémů, práci s hello internet věcí (IoT) a vytváření jednoduchých rozhraní API a mikroslužeb. Zvažte Functions pro úlohy, jako je image nebo pořadí zpracování, údržba souborů, nebo pro všechny úlohy, které chcete toorun podle plánu. 

Functions poskytuje šablony tooget, které jste začali s klíčovými scénáři, včetně hello následující:

* **BlobTrigger** -při přidání toocontainers objektů BLOB Azure Storage procesu. Tuto funkci můžete použít k změně velikosti imagí.
* **EventHubTrigger** -reagovat tooevents doručit tooan centra událostí Azure. Toto je obzvlášť užitečné pro scénáře instrumentace aplikací, zpracování činnosti nebo pracovního postupu uživatele a internetu věcí (IoT).
* **Obecný webhook** – Zpracování žádostí webhooku protokolu HTTP z jakékoli služby, která podporuje webhooky.
* **Webhook Githubu** -reakce tooevents, ke kterým dochází v úložištích Githubu. Příklady najdete v tématu [Vytvoření webhooku nebo funkce rozhraní API](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** -spustit hello provádění kódu pomocí požadavku HTTP.
* **QueueTrigger** -toomessages reagovat, když dorazí do fronty Azure Storage. Příklad, naleznete v části [vytvoření funkce Azure s vazbou tooan služba Azure](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** -připojit vaše tooother kódu služby nebo místní služby pomocí naslouchání toomessage front Azure. 
* **ServiceBusTopicTrigger** -připojit vaše tooother kódu služby nebo místní služby prostřednictvím registrace tootopics Azure. 
* **TimerTrigger** – Provádění úkolů čištění nebo jiných dávkových úkolů podle předdefinovaného plánu. Příklad najdete v tématu [Vytvoření funkce zpracování událostí](functions-create-an-event-processing-function.md).

Azure Functions podporuje *aktivační události*, které jsou způsoby toostart provádění kódu, a *vazby*, které jsou způsoby toosimplify kódování pro vstupní a výstupní data. Podrobný popis hello triggerů a vazeb podporovaných Azure Functions poskytuje, naleznete v části [referenční vývojáře triggerů a vazeb Azure Functions](functions-triggers-bindings.md).

## <a name="integrations"></a>Integrace
Azure Functions se integruje s celou řadou služeb Azure a služeb třetích stran. Tyto služby mohou aktivovat funkci a spustit provádění, nebo mohou sloužit jako vstup a výstup kódu. Azure Functions podporuje následující integrace služeb Hello. 

* Azure Cosmos DB
* Azure Event Hubs 
* Azure Mobile Apps (tabulky)
* Azure Notification Hubs
* Azure Service Bus (fronty a témata)
* Azure Storage (objekt blob, fronty a tabulky) 
* GitHub (webhooky)
* Místní (pomocí služby Service Bus)
* Twilio (SMS zprávy)

## <a name="pricing"></a>Kolik stojí Functions?
Azure Functions nabízí dva druhy cenových plánů, zvolte hello ten, který nejlépe vyhovuje vašim potřebám: 

* **Spotřeba plán** – když funkce spuštěná, Azure poskytuje všechny nezbytné výpočetní prostředky hello. Nemáte tooworry o správu prostředků a platíte jenom hello dobu, kdy byl kód spuštěný. 
* **Plán služby App Service** – Spouštějte funkce stejně jako webové a mobilní aplikace nebo aplikace API. Pokud už používáte služby App Service pro jiné aplikace, můžete funkce spustit na hello stejné plánování bez dalších poplatků. 

Úplné podrobnosti o cenách jsou dostupné na hello [stránce ceny Functions](https://azure.microsoft.com/pricing/details/functions/). Další informace o škálování funkcí najdete v tématu [jak tooscale Azure Functions](functions-scale.md).

## <a name="next-steps"></a>Další kroky
* [Vytvoření první funkce Azure](functions-create-first-azure-function.md)  
  Rovnou začít a vytvořit svoji první funkci pomocí rychlého startu Azure Functions hello. 
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Poskytuje další odborné informace o modulu runtime Azure Functions hello a odkaz pro kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Jak tooscale Azure Functions](functions-scale.md)  
  Popisuje plány služby, které jsou dostupné s Azure Functions, včetně hello spotřeba hostingový plán a jak toochoose hello správného plánu. 
* [Další informace o Azure App Service](../app-service/app-service-value-prop-what-is.md)  
  Azure Functions využívá platformu Azure App Service hello hlavní funkce, jako jsou nasazení, proměnné prostředí a diagnostiky. 

