---
title: "aaaHow tooScale aplikaci služby App Service Environment"
description: "Škálování aplikace ve službě App Service Environment"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: jimbe
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.openlocfilehash: 08916eac056c46bf8cb6edffbf96285317b32062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-apps-in-an-app-service-environment"></a>Škálování aplikací ve službě App Service Environment
V hello Azure App Service jsou obvykle tři věci, které je možné škálovat:

* ceny plán
* velikost pracovního procesu 
* počet instancí.

V App Service Environment není bez nutnosti tooselect nebo změňte hello ceny plánu.  Z hlediska funkce je již v cenová úroveň schopnosti Premium.  

S ohledem tooworker velikostí můžete přiřadit Dobrý den, správce App Service Environment hello velikost hello výpočetních prostředků toobe použít pro každý fond pracovních procesů.  To znamená pracovní skupina 1 může mít s P4 výpočetní prostředky a pracovní fond 2 s P1 výpočetní prostředky, v případě potřeby.  Nemají toobe v pořadí velikost.  Podrobnosti ohledně hello velikosti a jejich cenovou najdete v tématu dokumentu hello zde [Azure App Service – ceny][AppServicePricing].  Zůstane hello škálování možnosti pro webové aplikace a plány služby App Service v toobe App Service Environment:

* Výběr fondu pracovního procesu
* počet instancí

Změna buď položky se provádí prostřednictvím hello vhodné pro vaše App Service Environment hostované plány služby App Service uživatelském rozhraní.  

![][1]

Nelze škálovat vaše ASP nad rámec hello počet dostupných výpočetních prostředků ve fondu hello pracovního procesu, který je vaše prostředí ASP v.  Pokud třeba výpočetní prostředky v tomto fondu pracovních procesů je třeba tooget tooadd správce vaší App Service Environment je.  Pro informace hello zde přečíst informace o kolem opětovná konfigurace vaší App Service Environment: [jak tooConfigure služby App Service environment][HowtoConfigureASE].  Můžete také využít tootake hello App Service Environment škálování funkcí tooadd kapacity podle plánu nebo metriky.  Podrobnosti o konfiguraci automatického škálování pro prostředí hello App Service Environment, samotné najdete v části tooget [jak tooconfigure škálování App Service Environment][ASEAutoscale].

Můžete vytvořit více aplikaci pomocí výpočetní prostředky z fondů jiný pracovní plány služby, nebo můžete použít hello stejný fond pracovních procesů.  Pro příklad, pokud máte k dispozici výpočetní prostředky (10) v pracovním fond 1, můžete plán služby toocreate jednu aplikaci pomocí (6) výpočetní prostředky a druhý služby app service plán, který používá (4) výpočetní prostředky.

### <a name="scaling-hello-number-of-instances"></a>Škálování hello počet instancí
Při prvním vytvoření webové aplikace ve službě App Service Environment začíná 1 instance.  Můžete pak škálování tooadditional instance tooprovide další výpočetní prostředky pro vaši aplikaci.   

Pokud vaše App Service Environment má dostatečnou kapacitu, a to je poměrně jednoduché.  Přejdete tooyour plán služby App Service, kde hello lokalit mají tooscale nahoru a vyberte škálování.  Otevře se hello uživatelského rozhraní, kde můžete ručně nastavit hello škálování pro vaši ASP nebo konfigurace pravidel automatického škálování pro vaši ASP.  aplikace jednoduše sad škálování toomanually ***škálovat podle*** příliš***počet instancí, který nastavím ručně***.  Odsud přetáhněte hello posuvníku toohello požadovaného množství nebo zadejte v hello pole Další toohello posuvníku.  

![][2] 

Hello škálování pravidla pro webovou stránku ASP.NET v App Service Environment pracovní hello stejné jako obvykle.  Můžete vybrat ***procento využití procesoru*** pod ***škálovat podle*** a vytvořte pravidla automatického škálování pro vaši ASP na základě procento využití procesoru nebo je můžete vytvořit složitější pravidla pomocí ***pravidla plánu a výkonu ***.  toosee více dokončení podrobné informace o konfiguraci automatického škálování použití hello Průvodce zde [škálování aplikace v Azure App Service][AppScale]. 

### <a name="worker-pool-selection"></a>Výběr fondu pracovního procesu
Jak již bylo uvedeno dříve, hello pracovní fond výběru se získají z hello uživatelského rozhraní ASP.  Otevřete okno hello hello ASP má tooscale a vyberte fond pracovních procesů.  Zobrazí se všechny hello fondy pracovních procesů, které jste nakonfigurovali ve službě App Service Environment.  Pokud máte jenom jeden pracovní proces fondu pak uvidíte pouze jeden fond hello uvedené.  toochange jaké pracovní fond vaší ASP je v, jednoduše vyberte fond pracovních procesů hello chcete, aby váš plán služby App Service toomove k.  

![][3]

Před přesunutím vaší ASP z tooanother jeden pracovní proces fondu je důležité toomake se, že máte dostatečnou kapacitu pro vaše prostředí ASP.  V seznamu hello fondy pracovních procesů nejen je uveden název fondu hello pracovního procesu, ale můžete také zjistit, kolik pracovníci jsou k dispozici v tomto fondu pracovních procesů.  Ujistěte se, že existují toocontain k dispozici dostatek instance vašeho plánu služby App Service.  Pokud třeba více výpočetní prostředky ve fondu pracovních procesů hello chcete toomove potřebujete, pak tooadd správce vaší App Service Environment je.  

> [!NOTE]
> Přesun Klientova že studený způsobí, že ASP z fondu jeden pracovní proces se spustí aplikace hello v tomto prostředí ASP.  Požadavky toorun to může způsobit pomalu jako je vaše aplikace cold zahájeno hello nové výpočetní prostředky.  Hello studený start se vyhnout pomocí hello [aplikace horké až schopností] [ AppWarmup] ve službě Azure App Service.  modul inicializace aplikace Hello popsanou v článku hello funguje i pro studenou spustí vzhledem k tomu, že proces inicializace hello je také volána, když aplikace jsou cold zahájeno nové výpočetní prostředky. 
> 
> 

## <a name="getting-started"></a>Začínáme
tooget začít s prostředí App Service najdete v části [jak tooCreate App Service Environment][HowtoCreateASE]

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
