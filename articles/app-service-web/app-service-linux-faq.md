---
title: "Webové aplikace Azure App Service v systému Linux – nejčastější dotazy | Microsoft Docs"
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
ms.openlocfilehash: 6122f28b35d143ec26a379ae9aa8aee9bdaaff9e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a>Webové aplikace Azure App Service v systému Linux – nejčastější dotazy

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


S vydáním webové aplikace v systému Linux pracujeme na přidání funkcí a vylepšení naše platforma provádění. Tady jsou některé nejčastější dotazy (FAQ), které zákazníkům mít byla zprávu s požadavkem za posledních měsíců.
Pokud máte dotazy, komentáře na článek a jsme budete nejdříve hovor.

## <a name="built-in-images"></a>Předdefinované bitové kopie

**Otázka:** chcete rozvětvit předdefinované Docker kontejnerů, které poskytuje platformu. Kde najdu tyto soubory?

**Odpověď:** můžete najít všechny soubory Docker na [Githubu](https://github.com/azure-app-service). Můžete najít všechny kontejnery Docker na [úložiště Docker Hub](https://hub.docker.com/u/appsvc/).

**Otázka:** jaké jsou očekávané hodnoty pro spuštění souboru část, když je možné nakonfigurovat zásobník runtime?

**Odpověď:** pro Node.Js, určíte PM2 konfiguračního souboru nebo souboru skriptu. .NET Core zadejte název vaší zkompilované knihovny DLL. Pro Ruby můžete zadat Ruby skript, který chcete inicializovat vaší aplikace pomocí.

## <a name="management"></a>Správa

**Otázka:** co se stane při stisknutí tlačítka restartování na portálu Azure?

**Odpověď:** jde o ekvivalent Docker restartování.

**Otázka:** můžete použít Secure Shell (SSH) pro připojení k aplikaci kontejneru virtuální počítač (VM)?

**Odpověď:** Ano, můžete to udělat přes lokalitu SCM, zkontrolujte v následujícím článku na další informace [podpora SSH pro webové aplikace v systému Linux](./app-service-linux-ssh-support.md)

**Otázka:** vytvořit roviny Linux App Service pomocí sady SDK nebo šablonu ARM, jak to můžete dosáhnout?

**Odpověď:** je nutné nastavit `reserved` pole aplikace služby k `true`.

## <a name="continuous-integrationdeployment"></a>Průběžnou integraci a nasazení

**Otázka:** webová aplikace dál používá image staré kontejner Docker po aktualizovali bitovou kopii na úložiště Docker Hub. Podporujete průběžnou integraci a nasazení vlastní kontejnerů?

**Odpověď:** nastavit průběžnou integraci a nasazení pro Azure kontejneru registru nebo DockerHub Image kontrolou v následujícím článku [průběžné nasazování pomocí webové aplikace Azure v systému Linux](./app-service-linux-ci-cd.md). Pro privátní registrech můžete aktualizovat kontejneru zastavení a spuštění webové aplikace. Nebo můžete změnit nebo přidat nastavení fiktivní aplikace můžete vynutit aktualizaci vašeho kontejneru.

**Otázka:** podporují pracovní prostředí?

**Odpověď:** Ano.

**Otázka:** je možné používat **nasazení webu** nasazení webová aplikace?

**Odpověď:** Ano, je nutné nastavit aplikaci názvem `WEBSITE_WEBDEPLOY_USE_SCM` k `false`.

## <a name="language-support"></a>Podpora jazyků

**Otázka:** podporují nezkompilované aplikace .NET Core?

**Odpověď:** Ano.

**Otázka:** podporujete autora jako správce závislostí pro aplikace PHP?

**Odpověď:** Ano. Během nasazení Git by měl zjistit Kudu nasazujete aplikace PHP (díky přítomnost souboru composer.json) a aktivuje autora instalace pro vás.

## <a name="custom-containers"></a>Vlastní kontejnery

**Otázka:** používám vlastní vlastní kontejner. Moje aplikace se nachází v `\home\` adresáře, ale I nemůže najít Moje soubory při procházení obsahu pomocí [SCM lokality](https://github.com/projectkudu/kudu) nebo klient FTP. Kde jsou moje soubory?

**Odpověď:** nemůžeme připojit k serveru SMB pro sdílení `\home\` adresáře. Tím se přepíše veškerý obsah, který je k dispozici.

**Otázka:** používám vlastní vlastní kontejner. Nechci platformou připojit k serveru SMB pro sdílení `\home\`.

**Odpověď:** můžete to udělat nastavením `WEBSITES_ENABLE_APP_SERVICE_STORAGE` nastavení aplikace nastavte na `false`.

**Otázka:** Mé vlastní kontejner trvá dlouhou dobu spuštění a platformou restartuje kontejner před dokončením spuštění.

**Odpověď:** můžete nakonfigurovat čas platformou bude čekat před restartováním vašeho kontejneru. To můžete provést nastavením `WEBSITES_CONTAINER_START_TIME_LIMIT` nastavení aplikace nastavte na požadovanou hodnotu v sekundách. Výchozí hodnota je 230 sekund a maximální je 600 sekund.

**Otázka:** co je formát adresy url serveru privátní registru?

**Odpověď:** budete muset zadat registru úplnou adresu url včetně `http://` nebo `https://`.

**Otázka:** formát pro název bitové kopie v privátní registru možnosti?

**Odpověď:** budete muset přidat název úplnou bitovou kopii, včetně adresu url privátní registru (např. myacr.azurecr.IO/DotNet:Latest)

**Otázka:** chcete vystavit více než jeden port na mé vlastní kontejner bitovou kopii. Je to možné?

**Odpověď:** aktuálně, která není podporována.

**Otázka:** můžete zahrnout vlastní úložiště?

**Odpověď:** aktuálně, která není podporována.

**Otázka:** není možné prohlížet procesy systému nebo spuštění souboru Mé vlastní kontejner z webu Správce služeb. Co to znamená?

**Odpověď:** SCM lokality běží ve zvláštním kontejneru. Nelze zkontrolovat soubor systému nebo spuštění procesů kontejneru aplikace.

**Otázka:** Mé vlastní kontejner naslouchá na jiný port než port 80. Konfigurování aplikace my směrovat požadavky k tomuto portu

**Odpověď:** máme automatické zjišťování port, můžete také zadat aplikaci s názvem **WEBSITES_PORT**a jako hodnotu číslo portu očekávané. Dřív používal platformou `PORT` aplikace nastavení, jsme plánování přestat používat použití této aplikace, nastavení a přesouvat pomocí `WEBSITES_PORT` výhradně.

**Otázka:** je nutné implementovat HTTPS v mé vlastní kontejneru.

**Odpověď:** Ne, platformu zpracovává ukončení protokolu HTTPS na sdílené frontends.

## <a name="pricing-and-sla"></a>Ceny a smlouva SLA

**Otázka:** novinky ceny při používání verzi public preview?

**Odpověď:** budou se vám účtovat poloviční počet hodin, které běží vaše aplikace s normální ceny služby Azure App Service. To znamená, že můžete získat tak slevu 50 procent na normální Azure App Service – ceny.

## <a name="other"></a>Ostatní

**Otázka:** co jsou podporovanými znaky v názvech nastavení aplikace?

**Odpověď:** pro nastavení aplikace lze použít pouze A-Z,-z, 0 – 9 a podtržítka.

**Otázka:** kde mohou požadovat nové funkce?

**Odpověď:** můžete odeslat vaše nápad na [fóru pro zpětnou vazbu webové aplikace](https://aka.ms/webapps-uservoice). Přidejte "[Linux]" na název vaší představu.

## <a name="next-steps"></a>Další kroky
* [Co je Azure webové aplikace v systému Linux?](app-service-linux-intro.md)
* [Podpora SSH pro webové aplikace Azure v systému Linux](./app-service-linux-ssh-support.md)
* [Nastavení přípravných prostředí v Azure App Service](./web-sites-staged-publishing.md)
* [Průběžné nasazování pomocí Azure webové aplikace v systému Linux](./app-service-linux-ci-cd.md)
