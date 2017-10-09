---
title: "aaaBest postupy pro službu Azure App Service"
description: "Zjistěte, osvědčené postupy a řešení problémů pro službu Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: dariagrigoriu
ms.openlocfilehash: a1d3127f5a746aa43f7b56b45f17c45df9087bee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-app-service"></a>Osvědčené postupy pro Azure App Service
Tento článek shrnuje doporučené postupy pro používání [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Společné umístění
Při sestavování řešení například webovou aplikaci a databázi prostředků Azure jsou umístěné v různých oblastech hello důsledky může zahrnovat následující hello:

* Zvýší latence při komunikaci mezi prostředky
* Peněžní poplatky za odchozí data přenos mezi oblastmi uvedených na hello [Azure stránce s cenami](https://azure.microsoft.com/pricing/details/data-transfers).

Společné umístění v hello stejné oblasti je nejvhodnější pro sestavování řešení například webovou aplikaci prostředků Azure a účet databáze nebo úložiště použít toohold obsah nebo data. Při vytváření prostředky, měli byste si ověřit, jestli že jsou v hello stejné oblasti Azure, pokud máte konkrétní obchodní nebo návrh důvod pro ně není toobe. Můžete přesunout toohello aplikace služby App Service stejné oblasti jako databáze s využitím hello [služby App Service funkce klonování](app-service-web-app-cloning-portal.md) aktuálně dostupné pro plán aplikační služby Premium aplikace.   

## <a name="memoryresources"></a>Pokud aplikace vyžaduje více paměti, než se očekávalo
Pokud jste si všimli spotřebovává více paměti, než se očekávalo, jak je indikován při monitorování aplikace nebo služby doporučení zvažte hello [aplikace služby Automatické opravy funkce](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Jednu z možností hello pro funkci Automatické opravy hello trvá vlastní akce podle prahové hodnoty paměti. Akce span hello spektrum z e-mailové oznámení tooinvestigation prostřednictvím zmírnění tooon místě výpisu paměti podle recyklace hello pracovní proces. Automatické opravy lze nakonfigurovat pomocí souboru web.config a prostřednictvím popisný uživatelské rozhraní dle instrukcí v tomto příspěvku na blogu pro hello [rozšíření lokality podporu služby aplikace](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Když aplikace spotřebovávat více procesorů, než se očekávalo
Pokud jste Všimněte si, že aplikace využívá více procesorů než očekávané nebo vyskytne opakuje vzroste využití procesoru indikován monitorování nebo doporučení zvažte vertikálním navýšení kapacity nebo škálování hello plán služby App Service. Pokud vaše aplikace statefull, vertikálním navýšení kapacity je je hello jedinou možností, pokud je vaše aplikace bezstavové, škálování na více systémů vám poskytne větší flexibilitu a vyšší potenciální škálování. 

Další informace o "bezstavové" aplikace "statefull" vs můžete přehrát toto video: [plánování škálovatelné začátku do konce vícevrstvé aplikace na webové aplikace Microsoft Azure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Další informace o možnostech škálování a automatické škálování služby App Service najdete v tématu: [škálování webové aplikace v Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>Když vyčerpání prostředků soketů
Obvyklým důvodem vyčerpává odchozí připojení TCP je hello pomocí klientské knihovny, které nejsou implementované tooreuse připojení TCP, nebo v případě hello vyšší úroveň protokolu, jako je například – udržování připojení HTTP není využít. Přečtěte si dokumentaci hello pro každou z knihovny hello odkazuje hello aplikací v tooensure váš plán služby App Service jsou nakonfigurované nebo získat přístup v kódu pro efektivní opakované použití odchozí připojení. Navíc dodržíte hello knihovně dokumentace pokyny pro správné vytvoření a vydání nebo čištění tooavoid úniku připojení. Tato vyšetřování knihovny klienta se může být zmírnit dopad průběh škálování toomultiple instance.  

## <a name="appbackup"></a>Pokud vaše aplikace Zálohování spustí selhání
dva nejčastější příčiny, proč se nezdaří zálohování aplikace Hello: úložiště nejsou platné nastavení a konfiguraci databáze je neplatný. Tyto chyby obvykle stát, pokud se změny toostorage nebo prostředky databáze nebo změny jak tooaccess tyto prostředky (například pověření aktualizovat pro databázi hello vybraný v nastavení zálohování hello). Zálohování obvykle spouštět podle plánu a vyžadují přístup toostorage (pro výstup hello zálohování souborů) a databází (pro kopírování a čtení obsah toobe zahrnuté v záloze hello). Hello výsledek selhání tooaccess některý z těchto prostředků by chyby konzistentní zálohování. 

Při selhání zálohování, přečtěte si poslední toounderstand výsledky se děje typu selhání. V případě hello úložiště přístup chyb zkontrolujte a aktualizujte nastavení úložiště hello použít v konfiguraci zálohování hello. V případě hello selhání přístupu k databázi zkontrolujte a aktualizovat vaše připojení řetězce jako součást nastavení aplikace; poté pokračujte tooupdate tooproperly vaše konfigurace zálohování zahrnout požadované hello databáze. Další informace o zálohování aplikace najdete v tématu hello [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md) dokumentaci.

## <a name="nodejs"></a>Když jsou nové aplikace Node.js nasazené tooAzure služby App Service
Výchozí konfigurace Azure App Service pro aplikace Node.js je určený toobest barvy hello potřeby většiny běžných aplikací. Pokud konfigurace pro aplikaci Node.js by mít užitek z přizpůsobené vyladění výkonu tooimprove nebo optimalizovat využití prostředků procesoru a paměti nebo síťových prostředků, může zkontrolovat Naše osvědčené postupy a řešení potíží. V tomto článku dokumentace popisuje nastavení modulu iisnode hello může být nutné popisuje tooconfigure pro vaši aplikaci Node.js hello různé scénáře nebo problémy, které vaše aplikace může být čelí a ukazuje, jak tooaddress tyto problémy: [osvědčené postupy a Průvodci odstraňováním potíží uzlu aplikace v Azure App Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   

