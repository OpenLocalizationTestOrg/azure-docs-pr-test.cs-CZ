---
title: "aaaTroubleshoot Analytics ve službě Azure Application Insights | Microsoft Docs"
description: "Problémy s Application Insights analytics? Začněte tady. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: bwren
ms.openlocfilehash: e3be2fbc0237440d3b8a736484434a9f276296c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a>Řešení potíží s analýzami v nástroji Application Insights
Problémy s [Application Insights Analytics](app-insights-analytics.md)? Začněte tady. Analytics je nástroj pro efektivní vyhledávání hello služby Azure Application Insights.

## <a name="limits"></a>Omezení
* V současné době výsledky dotazu jsou omezené toojust přes týden posledních dat.
* Prohlížeče jsme test na: nejnovější vydání systému Chrome, okraj a prohlížeče Internet Explorer.

## <a name="known-incompatible-browser-extensions"></a>Rozšíření známé kompatibilní prohlížeče
* Ghostery

Zakázat rozšíření hello nebo použijte jiný prohlížeč.

## <a name="e-a"></a>"Neočekávané error.
![Došlo k neočekávané chybě obrazovky](./media/app-insights-analytics-troubleshooting/010.png)

Běhovém portálu – neošetřené výjimky došlo k vnitřní chybě.

* Vyčištění mezipaměti hello prohlížeče. 

## <a name="e-b"></a>403... Zkuste to prosím tooreload
![403... Zkuste to prosím tooreload](./media/app-insights-analytics-troubleshooting/020.png)

(Během ověřování nebo během generování tokenů přístupu) došlo k chybě související s ověřování. portál Hello může nijak příliš obnovit beze změny nastavení prohlížeče.

* Ověřte [jsou povolené soubory cookie třetích stran](#cookies) v prohlížeči hello. 

## <a name="authentication"></a>403... Ověřte zóny zabezpečení
![403.. .verify zóny zabezpečení](./media/app-insights-analytics-troubleshooting/030.png)

(Během ověřování nebo během generování tokenů přístupu) došlo k chybě související s ověřování. portál Hello může nijak příliš obnovit beze změny nastavení prohlížeče.

1. Ověřte [jsou povolené soubory cookie třetích stran](#cookies) v prohlížeči hello. 
2. Použijete Oblíbené položky, záložku nebo portálu analýza hello tooopen uložený odkaz? Jsou jste přihlášeni s použitím různých přihlašovacích údajů, než jste použili při uložení hello odkaz?
3. Zkuste použít okno prohlížeče v privátní nebo incognito (po zavření všechny takové windows). Tooprovide budete mít přihlašovací údaje. 
4. Otevře další okno (obyčejnou) prohlížeče a přejděte příliš[Azure](https://portal.azure.com). Odhlaste se. Potom otevřete odkaz na vaši a přihlaste se pomocí hello správné přihlašovací údaje.
5. Uživatelé okraj a prohlížeče Internet Explorer můžete také získat tuto chybu při nastavení zóny důvěryhodných serverů nejsou podporovány.
   
    Ověřte obě [portálu analýza](https://analytics.applicationinsights.io) a [portál Azure Active Directory](https://portal.azure.com) jsou v hello stejné zóny zabezpečení:
   
   * V Internet Exploreru otevřete **Možnosti Internetu**, **zabezpečení**, **Důvěryhodné servery**, **lokality**:
     
     ![Dialogové okno Možnosti Internetu, přidání webu tooTrusted lokalit](./media/app-insights-analytics-troubleshooting/033.png)
     
     V seznamu webů hello, pokud některá z následující adresy URL hello zahrnuty, ujistěte se, že hello jiné jsou zahrnuty také:
     
     https://Analytics.applicationinsights.IO<br/>
     https://login.microsoftonline.com<br/>
     https://login.windows.net

## <a name="e-d"></a>404 ... Prostředek se nenašel
![404... prostředek nebyl nalezen](./media/app-insights-analytics-troubleshooting/040.png)

Prostředek aplikace byla odstraněna z Application Insights a už není dostupný. To může dojít, pokud jste uložili hello URL toohello analýza stránky.

## <a name="e-e"></a>403 ... Žádné oprávnění
![403... neautorizovaných](./media/app-insights-analytics-troubleshooting/050.png)

Nemáte oprávnění tooopen tuto aplikaci v Analytics.

* Obdrželi jste od někoho jiného hello odkaz? Požádejte je, že jste v hello toomake [čtečky nebo přispěvatele pro tuto skupinu prostředků](app-insights-resources-roles-access-control.md).
* Uložíte hello odkaz použitím různých přihlašovacích údajů? Otevřete hello [portál Azure](https://portal.azure.com), odhlásit se a potom zkuste tohoto propojení znovu, poskytuje hello správné přihlašovací údaje.

## <a name="html-storage"></a>403 ... Úložiště HTML5
Náš portál používá HTML5 localStorage a sessionStorage.

* Chrome: Nastavení ochrany osobních údajů, nastavení obsahu.
* Internet Explorer: Možnosti Internetu, karta Upřesnit, zabezpečení, povolte úložiště DOM

![403... Zkuste tooenable HTML5 úložiště](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 ... Odběr nebyl nalezen
![404 ... Odběr nebyl nalezen](./media/app-insights-analytics-troubleshooting/070.png)

Adresa URL Hello je neplatná. 

* Otevřete prostředek aplikace hello v [portál Application Insights](https://portal.azure.com). Potom pomocí tlačítka Analytics hello.

## <a name="e-h"></a>404... stránka neexistuje
![404 ... Stránka neexistuje.](./media/app-insights-analytics-troubleshooting/080.png)

Adresa URL Hello je neplatná.

* Otevřete prostředek aplikace hello v [portál Application Insights](https://portal.azure.com). Potom pomocí tlačítka Analytics hello.

## <a name="cookies"></a>Povolit soubory cookie třetích stran
  V tématu [jak toodisable třetí strany soubory cookie](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), ale Všimněte si, potřebujeme příliš**povolit** je.


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

