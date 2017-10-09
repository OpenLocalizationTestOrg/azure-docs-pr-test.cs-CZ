---
title: "aaaIIS přihlásí Log Analytics | Microsoft Docs"
description: "Internetové informační služby (IIS) ukládá aktivity uživatelů v souborech protokolů, které může shromáždit analýzy protokolů.  Tento článek popisuje, jak tooconfigure shromažďování protokolů služby IIS a podrobnosti záznamů hello vytvoří v úložišti OMS hello."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: bwren
ms.openlocfilehash: c5575351090cdabaf651bcb49867794ee3a4b6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="iis-logs-in-log-analytics"></a>Služba IIS přihlásí analýzy protokolů
Internetové informační služby (IIS) ukládá aktivity uživatelů v souborech protokolů, které může shromáždit analýzy protokolů.  

![Protokoly IIS](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurace služby IIS protokoly
Analýzy protokolů shromáždí položky z soubory protokolů vytvořené službou IIS, takže musíte [konfigurace IIS pro protokolování](https://technet.microsoft.com/library/hh831775.aspx).

Analýzy protokolů pouze podporuje uložený ve formátu W3C souborech protokolu služby IIS a nepodporuje vlastní pole nebo rozšířené protokolování internetové informační služby.  
Analýzy protokolů neshromažďuje protokoly ve formátu NCSA nebo IIS nativní.

Konfigurace služby IIS protokolů v analýzy protokolů z hello [nabídce Data v nastavení analýzy protokolů](log-analytics-data-sources.md#configuring-data-sources).  Není nutná žádná konfigurace, jiné než vyberete **souborů protokolu W3C shromažďovat formátu služby IIS**.

Doporučujeme vám, že pokud povolíte kolekce protokolu služby IIS, byste měli na každém serveru nakonfigurovat nastavení výměny protokolu služby IIS hello.

## <a name="data-collection"></a>Shromažďování dat
Analýzy protokolů shromažďuje položky protokolu služby IIS z každého připojeného zdroje přibližně každých 15 minut.  Hello agent zaznamenává jeho místo v každém protokolu událostí, shromažďující z.  Pokud agenta hello přejde do režimu offline, pak analýzy protokolů shromažďuje události od poslední místa vypnout, i v případě, že tyto události byly vytvořeny v době, kdy hello agent offline.

## <a name="iis-log-record-properties"></a>Vlastnosti záznamu protokolu služby IIS
Záznamy protokolu služby IIS mít typ **W3CIISLog** a mít hello vlastnosti v hello následující tabulka:

| Vlastnost | Popis |
|:--- |:--- |
| Počítač |Název hello počítače, který hello událost nebyla shromážděna z. |
| cIP |IP adresa klienta hello. |
| csMethod |Metoda požadavku hello například GET nebo POST. |
| csReferer |Lokality této hello uživatele použili odkaz z aktuální lokality toohello. |
| csUserAgent |Typ prohlížeče klienta hello. |
| csUserName |Název hello ověřeného uživatele, který se připojil hello serveru. Anonymní uživatelé se označují pomlčkou. |
| csUriStem |Cíl hello požadavku, jako je například na webové stránce. |
| csUriQuery |Dotaz, pokud existuje, že klient hello pokoušel tooperform. |
| ManagementGroupName |Název skupiny pro správu hello agenty nástroje Operations Manager.  Pro jiné agenty jde AOI -\<ID pracovního prostoru\> |
| RemoteIPCountry |Země hello IP adresy klienta hello. |
| RemoteIPLatitude |Zeměpisná šířka hello klienta IP adresy. |
| RemoteIPLongitude |Zeměpisná délka hello klienta IP adresy. |
| scStatus |Stavový kód HTTP. |
| scSubStatus |Druhotný stavový kód chyby. |
| scWin32Status |Stavový kód Windows. |
| sIP |IP adresa hello webového serveru. |
| SourceSystem |Nástroj OpsMgr |
| sPort |Port na hello server hello klient připojen. |
| sSiteName |Název webu IIS hello. |
| TimeGenerated |Datum a čas hello položka byla zaprotokolována. |
| timeTaken |Požádat o délce hello tooprocess čas v milisekundách. |

## <a name="log-searches-with-iis-logs"></a>Protokol hledání s protokoly služby IIS
Hello následující tabulka obsahuje různé příklady dotazů protokolu, které načtení záznamů protokolu služby IIS.

| Dotaz | Popis |
|:--- |:--- |
| Typ = W3CIISLog |Všechny záznamy protokolu služby IIS. |
| Typ = W3CIISLog scStatus = 500 |Všechny záznamy protokolu služby IIS s návratový stav 500. |
| Typ = W3CIISLog &#124; Míra count() podle cIP |Počet služby IIS protokolu položky podle IP adresy klienta. |
| Typ = W3CIISLog csHost = "www.contoso.com" &#124; Míra count() podle csUriStem |Počet z protokolu služby IIS položky podle adresy URL pro www.contoso.com hello hostitele. |
| Typ = W3CIISLog &#124; Míra Sum(csBytes) podle počítače &#124; horní 500000 |Celkový počet přijatých bajtů na každém počítači služby IIS. |

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak změní následující toohello hello výše dotazy.

> | Dotaz | Popis |
|:--- |:--- |
| W3CIISLog |Všechny záznamy protokolu služby IIS. |
| W3CIISLog &#124; kde scStatus == 500 |Všechny záznamy protokolu služby IIS s návratový stav 500. |
| W3CIISLog &#124; shrnout count() podle cIP |Počet služby IIS protokolu položky podle IP adresy klienta. |
| W3CIISLog &#124; kde csHost == "www.contoso.com" &#124; shrnout count() podle csUriStem |Počet z protokolu služby IIS položky podle adresy URL pro www.contoso.com hello hostitele. |
| W3CIISLog &#124; shrnout sum(csBytes) podle počítače &#124; trvat 500000 |Celkový počet přijatých bajtů na každém počítači služby IIS. |

## <a name="next-steps"></a>Další kroky
* Konfigurace analýzy protokolů toocollect jiných [zdroje dat](log-analytics-data-sources.md) pro analýzu.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.
* Konfigurace výstrah v analýzy protokolů tooproactively upozorňují na důležité podmínky najít v protokolech služby IIS.
