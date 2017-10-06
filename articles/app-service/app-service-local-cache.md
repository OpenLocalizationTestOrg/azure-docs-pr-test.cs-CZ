---
title: "Přehled místní mezipaměti služby aplikace aaaAzure | Microsoft Docs"
description: "Tento článek popisuje, jak tooenable, změny velikosti a dotazování hello stav hello funkce Azure App Service místní mezipaměti"
services: app-service
documentationcenter: app-service
author: SyntaxC4
manager: yochayk
editor: 
tags: optional
keywords: 
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2016
ms.author: cfowler
ms.openlocfilehash: 220331ac7e15352a434d63266701071024d868c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-local-cache-overview"></a>Přehled služby Azure App Service místní mezipaměti
Službě Azure web app obsah se uloží do úložiště Azure a je prezentované až trvanlivý způsobem jako sdílenou složku obsahu. Tento návrh určený toowork s celou řadu aplikací a má hello následující atributy:  

* obsah Hello je sdílet mezi více instancí virtuálního počítače (VM) hello webové aplikace.
* obsah Hello je odolné a lze upravit tak, že spustíte webové aplikace.
* Soubory diagnostická data a soubory protokolu jsou k dispozici v části hello stejné sdílené složce obsahu.
* Publikování nový obsah přímo aktualizace hello složky obsahu. Můžete okamžitě zobrazení hello stejné obsahu prostřednictvím webu SCM hello a hello funkční webovou aplikaci (obvykle některé technologie, jako je ASP.NET iniciovat restartování webové aplikace na některé změny tooget hello nejnovější obsah souboru).

I mnoho webové aplikace pomocí jednoho nebo všech těchto funkcí, třeba některé webové aplikace jen vysoce výkonné, jen pro čtení úložiště obsahu, který můžete spustit z s vysokou dostupností. Tyto aplikace mohou těžit z instance virtuálního počítače konkrétní místní mezipaměti.

Funkce Azure App Service místní mezipaměti Hello poskytuje webové role zobrazení obsahu. Tento obsah je mezipaměť zápisu. ale zahození obsahu úložiště, které se asynchronně vytvoří při spuštění webu. Když hello mezipaměti je připraven, lokalita hello je vypnuté toorun hello uložené v mezipaměti obsahu. Webové aplikace, které běží v místní mezipaměti mají hello následující výhody:

* Jsou to imunní toolatencies, ke kterým dochází, když přistupují k obsahu v Azure Storage.
* Jsou to upgrady imunní toohello plánované nebo neplánované výpadky způsobené nástrojem a žádné jiné poruchy s Azure Storage, ke kterým došlo na serverech, které slouží hello sdílené složce obsahu.
* Mají menšímu počtu restartování aplikace kvůli změny toostorage sdílené složky.

## <a name="how-local-cache-changes-hello-behavior-of-app-service"></a>Jak místní mezipaměti mění chování hello služby App Service
* místní mezipaměti Hello je kopie hello /site a /siteextensions složek hello webové aplikace. Vytváří se na hello místní instance virtuálního počítače při spuštění webové aplikace. Hello velikost hello místní mezipaměti na webové aplikace je omezená too300 MB ve výchozím nastavení, ale můžete zvýšit až too2 GB.
* místní mezipaměť Hello je pro čtení a zápis. Ale všechny změny budou zahozeny při hello webové aplikace přesune virtuální počítače a získá restartování. Pro aplikace, které obsahují důležitá data v úložišti obsahu hello byste neměli používat místní mezipaměti.
* Webové aplikace můžete dál diagnostických dat a souborů protokolu toowrite, stejně jako aktuálně. Soubory protokolu a data, ale jsou uloženy místně na hello virtuálních počítačů. Poté budou zkopírovány přes pravidelně toohello sdíleného úložiště obsahu. Hello kopie toohello sdíleného úložiště obsahu je nejlepší možný úsilí – voláními zápisu by mohly být ztraceny z důvodu tooa nečekané chybě instance virtuálních počítačů.
* Dojde ke změně ve struktuře složek hello hello LogFiles a složek s daty pro webové aplikace, které používají místní mezipaměti. Jsou teď podsložky v hello úložiště LogFiles Data a složky, které podle vzoru pro pojmenovávání hello "Jedinečný identifikátor" + časové razítko. Každý z podsložky hello odpovídá tooa instance virtuálního počítače, kde běží hello webovou aplikaci, nebo byla spuštěna.  
* Publikování změn toohello webové aplikace prostřednictvím některý z mechanismů publikování hello publikuje toohello sdíleného úložiště obsahu. Jedná se o návrhu vzhledem k tomu, že chceme hello publikován obsahu toobe trvanlivý. hello místní mezipaměti toorefresh hello webové aplikace, musí být toobe restartovat. To vypadat třeba nadměrné krok? toomake hello cyklu bezproblémové, najdete v informacích hello později v tomto článku.
* D:\Home bude odkazovat toohello místní mezipaměti. D:\Local bude pokračovat, odkazující toohello dočasného virtuálního počítače konkrétní úložiště.
* zobrazení obsahu Hello výchozí hello SCM lokality bude pokračovat toobe, který hello sdílené úložiště obsahu.

## <a name="enable-local-cache-in-app-service"></a>Povolit místní mezipaměti ve službě App Service
Pomocí kombinace nastavení vyhrazené aplikace nakonfigurujete místní mezipaměti. Tato nastavení aplikací můžete nakonfigurovat pomocí hello následující metody:

* [Azure Portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-hello-azure-portal"></a>Nakonfigurujte místní mezipaměti pomocí hello portálu Azure
<a name="Configure-Local-Cache-Portal"></a>

Můžete povolit místní mezipaměti na základě aplikací na web pomocí tohoto nastavení aplikace:`WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Nastavení aplikace portálu Azure: místní mezipaměti](media/app-service-local-cache/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Nakonfigurujte místní mezipaměti pomocí Azure Resource Manager
<a name="Configure-Local-Cache-ARM"></a>

```

...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],

"properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-hello-size-setting-in-local-cache"></a>Změňte nastavení velikosti hello v místní mezipaměti
Ve výchozím nastavení, velikost místní mezipaměti hello je **300 MB**. To zahrnuje hello /site a /siteextensions složek, které jste zkopírovali z hello obsahu úložiště, stejně jako jakoukoli místně vytvořené složky protokoly a data. tooincrease toto omezení, použijte nastavení aplikace hello `WEBSITE_LOCAL_CACHE_SIZEINMB`. Můžete zvýšit velikost hello až příliš**2 GB** (2000 MB) na webové aplikace.

## <a name="best-practices-for-using-app-service-local-cache"></a>Osvědčené postupy pro použití místní mezipaměti služby aplikace
Doporučujeme použít místní mezipaměti ve spojení s hello [přípravného prostředí](../app-service-web/web-sites-staged-publishing.md) funkce.

* Přidat hello *trvalé* nastavení aplikace `WEBSITE_LOCAL_CACHE_OPTION` s hodnotou hello `Always` tooyour **produkční** slot. Pokud používáte `WEBSITE_LOCAL_CACHE_SIZEINMB`, také přidat jako trvalé nastavení tooyour produkční slot.
* Vytvoření **pracovní** pozice a publikování tooyour pracovní pozici. Obvykle není nastavený hello pracovní pozici toouse místní mezipaměti tooenable bezproblémové životního cyklu sestavení nasazení testování pro přípravu, je-li získat výhody hello místní mezipaměti pro hello produkční slot.
* Testování webu proti vaše pracovní pozici.  
* Až budete připravení, vydávání [operace prohození](../app-service-web/web-sites-staged-publishing.md#Swap) mezi pracovní a provozní přihrádek.  
* Trvalé nastavením patří název a trvalé tooa slot. Proto když hello pracovní pozici získá prohodily do produkčního prostředí, bude dědit nastavení aplikace hello místní mezipaměti. Hello nově vzájemně zaměněny produkčním slotu probíhat hello místní mezipaměti po několika minutách a bude možné provozní teplotu jako součást slotu zahřívání po odkládacího souboru. Proto po dokončení prohození slotů hello produkčního slotu. bude spuštěna před hello místní mezipaměti.

## <a name="frequently-asked-questions-faq"></a>Nejčastější dotazy
### <a name="how-can-i-tell-if-local-cache-applies-toomy-web-app"></a>Jak poznám, pokud platí místní mezipaměti toomy webové aplikace?
Pokud vaše webová aplikace potřebuje vysoce výkonné a spolehlivé úložiště obsahu, nepoužívá hello ukládání obsahu toowrite důležitých dat za běhu a je menší než 2 GB v celková velikost, potom odpověď hello je "Ano"! tooget hello celková velikost složky /site a /siteextensions, můžete použít rozšíření lokality hello "Využití disku pro aplikace webových Azure".  

### <a name="how-can-i-tell-if-my-site-has-switched-toousing-local-cache"></a>Jak poznám, pokud mé lokality přepnul toousing místní mezipaměti?
Pokud používáte funkci místní mezipaměti hello s pracovní prostředí, operace prohození hello dokončena až místní mezipaměti je jestli. toocheck, pokud vaše lokalita běží proti místní mezipaměti, můžete zkontrolovat hello pracovní proces prostředí proměnná `WEBSITE_LOCALCACHE_READY`. Použijte pokyny hello na hello [proměnnou prostředí pracovní proces](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) tooaccess stránku hello proměnné prostředí pracovní proces na několik instancí.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-toohave-them-why"></a>Právě publikování nových změn, ale toohave zřejmě není webová aplikace je. Proč?
Pokud vaše webová aplikace používá místní mezipaměti, bude nutné toorestart nejnovější změny lokality tooget hello. Nechcete pracoviště tooa toopublish změny? V části Možnosti slotu hello hello předchozího osvědčené postupy oddílu.

### <a name="where-are-my-logs"></a>Kde jsou moje protokoly?
S místní mezipaměť vypadá trochu odlišně složek s daty a protokoly. Však hello struktura vaší podsložky zůstane stejný text hello, s tím rozdílem, že jsou v podsložce s hello nestled hello podsložky formátu "Jedinečný identifikátor virtuálního počítače" + časové razítko.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Jsou povolena místní mezipaměť, avšak Moje webové aplikace stále získá restartovat. Co to znamená? Ačkoli že místní mezipaměti pomohly s častým aplikace restartuje.
Místní mezipaměti Zabraňte související s úložištěm webové aplikace restartuje. Však vaší webové aplikace může stále projít restartování během upgradu plánované infrastruktury hello virtuálních počítačů. Hello celkové restartování aplikace, které prostředí s místní mezipaměti povolena by měl být méně.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-toohello-faster-local-drive"></a>Nepodporuje místní mezipaměti vyloučit všechny adresáře nebudou zkopírovány toohello rychlejší místní jednotku?
Jako součást hello krok, který zkopíruje obsah úložiště hello budou vyloučeny všechny složky s názvem úložiště. To pomáhá s scénáře, kde obsahu webu může obsahovat úložiště řízení zdrojů, nemusí být potřeba v operaci tooday den hello webové aplikace. 
