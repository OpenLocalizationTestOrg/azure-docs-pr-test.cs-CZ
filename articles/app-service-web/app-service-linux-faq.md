---
title: "aaaAzure webové aplikace App Service na nejčastější dotazy týkající se systému Linux | Microsoft Docs"
description: "Webové aplikace Azure App Service v systému Linux – nejčastější dotazy."
keywords: "služby Azure app service, webové aplikace, – nejčastější dotazy, linux, operačních systémů"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: c7798d9144d936eecdc0e191fc870b0ee0b220c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a>Webové aplikace Azure App Service v systému Linux – nejčastější dotazy

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Verze hello webové aplikace v systému Linux pracujeme na přidání funkcí a vylepšení tooour platformy provádění. Zde jsou uvedeny některé časté otázky (FAQ), které zákazníkům mít byla zprávu s požadavkem přes hello posledních měsíců.
Pokud máte dotazy, komentáře k článku hello a jsme budete nejdříve hovor.

## <a name="built-in-images"></a>Předdefinované bitové kopie

**Otázka:** chci poskytuje toofork hello předdefinované Docker kontejnery, které hello platformy. Kde najdu tyto soubory?

**Odpověď:** můžete najít všechny soubory Docker na [Githubu](https://github.com/azure-app-service). Můžete najít všechny kontejnery Docker na [úložiště Docker Hub](https://hub.docker.com/u/appsvc/).

**Otázka:** co jsou hello očekávaných hodnot pro hello oddílu spuštění souboru, když je možné nakonfigurovat hello runtime zásobníku?

**Odpověď:** pro Node.Js, určíte hello PM2 konfiguračního souboru nebo souboru skriptu. .NET Core zadejte název vaší zkompilované knihovny DLL. Pro Ruby, můžete zadat hello Ruby skriptu, které chcete tooinitialize vaší aplikace pomocí.

## <a name="management"></a>Správa

**Otázka:** co se stane při stisknutí tlačítka restartování hello v hello portál Azure?

**Odpověď:** to je hello ekvivalentní Docker restartování.

**Otázka:** můžete použít Secure Shell (SSH) tooconnect toohello aplikace kontejneru virtuální počítač (VM)?

**Odpověď:** Ano, můžete provést, prostřednictvím webu SCM hello, zkontrolujte hello následující článek informace [podpora SSH pro webové aplikace v systému Linux](./app-service-linux-ssh-support.md)

**Otázka:** chci toocreate roviny Linux App Service pomocí sady SDK nebo šablonu ARM, jak to můžete dosáhnout?

**Odpověď:** potřebujete tooset hello `reserved` pole aplikace hello služby příliš`true`.

## <a name="continuous-integrationdeployment"></a>Průběžnou integraci a nasazení

**Otázka:** webová aplikace dál používá image staré kontejner Docker po aktualizovali hello bitové kopie na úložiště Docker Hub. Podporujete průběžnou integraci a nasazení vlastní kontejnerů?

**Odpověď:** tooset až průběžné integraci a nasazení pro Azure kontejneru registru nebo DockerHub Image podle následujícího článku hello kontrola [průběžné nasazování pomocí webové aplikace Azure v systému Linux](./app-service-linux-ci-cd.md). Pro privátní registrech můžete aktualizovat hello kontejneru zastavení a spuštění webové aplikace. Nebo můžete změnit nebo přidat fiktivní aplikace nastavení tooforce aktualizaci vašeho kontejneru.

**Otázka:** podporují pracovní prostředí?

**Odpověď:** Ano.

**Otázka:** je možné používat **nasazení webu** toodeploy webová aplikace?

**Odpověď:** Ano, je nutné tooset aplikace názvem `WEBSITE_WEBDEPLOY_USE_SCM` příliš`false`.

## <a name="language-support"></a>Podpora jazyků

**Otázka:** podporují nezkompilované aplikace .NET Core?

**Odpověď:** Ano.

**Otázka:** podporujete autora jako správce závislostí pro aplikace PHP?

**Odpověď:** Ano. Během nasazení Git by měl zjistit Kudu nasazujete aplikace PHP (Děkujeme toohello přítomnost composer.json souboru) a aktivuje autora instalace pro vás.

## <a name="custom-containers"></a>Vlastní kontejnery

**Otázka:** používám vlastní vlastní kontejner. Moje aplikace se nachází v hello `\home\` adresáře, ale I nemůže najít Moje soubory při procházení obsahu hello pomocí hello [SCM lokality](https://github.com/projectkudu/kudu) nebo klient FTP. Kde jsou moje soubory?

**Odpověď:** nemůžeme připojit toohello sdílené složky SMB `\home\` adresáře. Tím se přepíše veškerý obsah, který je k dispozici.

**Otázka:** používám vlastní vlastní kontejner. Nechci hello platformy toomount toohello sdílené složky SMB `\home\`.

**Odpověď:** můžete to udělat pomocí nastavení hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` aplikace nastavení příliš`false`.

**Otázka:** Mé vlastní kontejner trvá dlouho toostart a hello platformy restartování hello kontejneru dříve, než se dokončí spuštění.

**Odpověď:** můžete nakonfigurovat čas hello hello platformy bude čekat před restartováním vašeho kontejneru. To můžete provést nastavení hello `WEBSITES_CONTAINER_START_TIME_LIMIT` hodnota požadovaného toohello nastavení aplikace v sekundách. Výchozí hodnota Hello je 230 sekund a maximální hello je 600 sekund.

**Otázka:** co je hello formát adresy url serveru privátní registru?

**Odpověď:** potřebujete tooprovide hello registru úplnou adresu url včetně `http://` nebo `https://`.

**Otázka:** co je hello formát pro název bitové kopie hello v privátní registru možnosti?

**Odpověď:** potřebujete tooadd hello úplnou bitovou kopii název včetně hello privátní registru url (např. myacr.azurecr.IO/DotNet:Latest)

**Otázka:** chci více než jeden port pro tooexpose na mé vlastní kontejner bitovou kopii. Je to možné?

**Odpověď:** aktuálně, která není podporována.

**Otázka:** můžete zahrnout vlastní úložiště?

**Odpověď:** aktuálně, která není podporována.

**Otázka:** není možné prohlížet procesy systému nebo spuštění souboru Moje vlastní kontejner z lokality SCM hello. Co to znamená?

**Odpověď:** hello SCM lokality běží ve zvláštním kontejneru. Nelze zkontrolovat hello systému souborů nebo spuštěných procesů kontejneru aplikace hello.

**Otázka:** Mé vlastní kontejner naslouchá tooa port než 80. Jak lze nastavit Moje aplikace tooroute hello požadavky toothat port?

**Odpověď:** máme automatické zjišťování port, můžete také zadat aplikaci s názvem **WEBSITES_PORT**a dejte mu hello hodnotu hello očekáváno číslo portu. Dřív byla hello platformě pomocí `PORT` aplikace nastavení, jsme plánování toodeprecate hello použití této aplikace, nastavení a přesunout toousing `WEBSITES_PORT` výhradně.

**Otázka:** potřebuji tooimplement HTTPS v mé vlastní kontejneru.

**Odpověď:** Ne, hello platformy zpracovává ukončení protokolu HTTPS v frontends hello sdílet.

## <a name="pricing-and-sla"></a>Ceny a smlouva SLA

**Otázka:** co je hello ceny při používání hello verzi public preview?

**Odpověď:** budou se vám účtovat polovinu hello počet hodin, které běží vaše aplikace s hello normální Azure App Service – ceny. To znamená, že můžete získat tak slevu 50 procent na normální Azure App Service – ceny.

## <a name="other"></a>Ostatní

**Otázka:** co jsou podporovány hello písmena v názvech nastavení aplikace?

**Odpověď:** můžete použít A-Z,-z, 0-9 a hello podtržítka pro nastavení aplikace.

**Otázka:** kde mohou požadovat nové funkce?

**Odpověď:** můžete odeslat vaše nápad na hello [fóru pro zpětnou vazbu webové aplikace](https://aka.ms/webapps-uservoice). Přidejte název "[Linux]" toohello vaše představu.

## <a name="next-steps"></a>Další kroky
* [Co je Azure webové aplikace v systému Linux?](app-service-linux-intro.md)
* [Podpora SSH pro webové aplikace Azure v systému Linux](./app-service-linux-ssh-support.md)
* [Nastavení přípravných prostředí v Azure App Service](./web-sites-staged-publishing.md)
* [Průběžné nasazování pomocí Azure webové aplikace v systému Linux](./app-service-linux-ci-cd.md)
