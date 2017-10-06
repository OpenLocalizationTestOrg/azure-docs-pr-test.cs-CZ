---
title: "aaaUsing Azure AD Connect Health se synchronizací | Microsoft Docs"
description: "Toto je stránka hello Azure AD Connect Health, která bude popisují, jak synchronizovat toomonitor Azure AD Connect."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 1dfbeaba-bda2-4f68-ac89-1dbfaf5b4015
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56f0582be30e664026cedf15350bc23501998bfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a>Sledování synchronizace Azure AD Connect pomocí služby Azure AD Connect Health
Následující dokumentace Hello je konkrétní toomonitoring Azure AD Connect (Sync) s Azure AD Connect Health.  Informace o sledování služby AD FS pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md). Informace o sledování služby Active Directory Domain Services pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Upozornění služby Azure AD Connect Health pro synchronizaci
Hello Azure AD Connect stavu výstrahy pro část synchronizace poskytuje že Hello seznam aktivních výstrah. Každé upozornění obsahuje důležité informace, postup řešení a odkazy toorelated dokumentaci. Výběrem aktivního nebo vyřešeného upozornění se zobrazí nové okno s další informace, jakož i kroky, které můžete provést tooresolve hello výstrahy a odkazy tooadditional dokumentaci. Můžete také zobrazit historická data na výstrahy, které byly vyřešeny hello posledních.

Výběrem některého upozornění zobrazíte další informace a také kroky můžete využít tooresolve hello výstrahy a odkazy tooadditional dokumentaci.

![Chyba synchronizace služby Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Omezené vyhodnocení upozornění
Pokud Azure AD Connect není používá hello výchozí konfiguraci (například pokud je filtrování atributů změněné z hello výchozí konfigurace tooa vlastní konfigurace), pak agenta hello Azure AD Connect Health nebude odesílat hello chybové události související tooAzure AD Connect.

Toto nastavení omezuje hello vyhodnocení upozornění službou hello. Zobrazí se banner, který označuje tuto podmínku, která v hello portálu Azure v rámci služby.

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner.png)

Můžete změnit kliknutím na "Nastavení" a povolení tooupload agenta Azure AD Connect Health všechny protokoly chyb.

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Analýza synchronizace
Správci často mají tooknow o hello době potřebné změny tooAzure toosync AD a hello množství probíhající změny. Tato funkce poskytuje toovisualize snadný způsob, jak to pomocí hello níže grafy:   

* sledování latence operací synchronizace
* sledování trendu změn objektů

### <a name="sync-latency"></a>Latence synchronizace
Tato funkce poskytuje grafické zobrazení trendu latence operací hello synchronizace (import, export atd.) pro konektory.  To poskytuje rychlý a snadný způsob toounderstand nejen hello latence vaše operace (větší, pokud máte velké sady změn), ale také způsob toodetect anomálií hello latence, které můžou vyžadovat další šetření.

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Ve výchozím nastavení se zobrazí pouze hello latence operace "Export" hello pro konektor Azure AD hello.  toosee další operace na konektoru hello nebo tooview operace z jiných konektorů, klikněte pravým tlačítkem na graf hello vyberte upravit graf nebo klikněte na tlačítko "Upravit graf latence" hello a zvolte konkrétní operaci hello a konektory.

### <a name="sync-object-changes"></a>Změny objektů synchronizace
Tato funkce poskytuje grafické zobrazení trendu hello počet změn, které se vyhodnocují a exportují tooAzure AD.  Dnes, snažíme toogather tyto informace z protokolů synchronizace hello je obtížné.  Hello graf poskytuje nejen jednodušší způsob sledování hello počet změn, ke kterým dochází ve vašem prostředí, ale i vizuální zobrazení hello chyby, ke kterým dochází.

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Sestava chyb synchronizace na úrovni objektu (verze Preview)
Tato funkce poskytuje sestavu chyb synchronizace, ke kterým může dojít při synchronizaci dat identity mezi službou Windows Server AD a Azure AD pomocí služby Azure AD Connect.

* Sestava Hello pokrývá chyb zaznamenaných klientem synchronizace hello (Azure AD Connect verze 1.1.281.0 nebo vyšší)
* V poslední operaci synchronizace hello na hello synchronizační modul obsahuje hello chybách, ke kterým došlo k chybě. ("Export" na hello Azure AD Connector.)
* Agent Azure AD Connect Health pro synchronizaci musí mít odchozí připojení toohello požadované koncové body pro hello sestavy tooinclude hello nejnovější data.
* Sestava Hello je **aktualizované po každých 30 minut** pomocí dat hello odeslaný agenta Azure AD Connect Health pro synchronizaci. Poskytuje následující klíčové funkce hello

  * Kategorizace chyb
  * Seznam chybných objektů podle kategorie
  * Všechny hello data o chybách hello na jednom místě.
  * Souběžně straně porovnání objektů s chybou z důvodu konfliktu tooa
  * Stáhnout hello chybách jako systém CVS (už brzy)

### <a name="categorization-of-errors"></a>Kategorizace chyb
Sestava Hello rozděluje hello existující chyby synchronizace v hello následujících kategorií:

| Kategorie | Popis |
| --- | --- |
| Duplicitní atribut |Chyby vzniklé při pokusu služby Azure AD Connect o vytvoření nebo aktualizaci objektů s duplicitními hodnotami atributů ve službě Azure AD, které musí být v tenantovi jedinečné, například proxyAddresses, UserPrincipalName |
| Neshoda dat |Chyby při hello konfigurace soft-match selže toomatch objekty, které mít za následek chyby synchronizace. |
| Chyba ověřování dat |Chyby z důvodu tooinvalid dat, jako jsou nepodporované znaky v důležitých atributů, jako jsou třeba UserPrincipalName, formátu chyby, které nesplní ověření před zápisem ve službě Azure AD. |
| Rozsáhlý atribut |Chyby při jeden nebo více atributů jsou větší než hello povolená velikost, délka nebo count. |
| Ostatní |Všechny ostatní chyby, které nevyhovují hello výše uvedené skupiny. Na základě zpětné vazby rozdělíme tuto kategorii do podkategorií. |

![Souhrnná sestava chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Kategorie sestavy chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Seznam chybných objektů podle kategorie
Procházení do každé kategorie poskytne hello seznam objektů, které chybu hello do této kategorie spadají.
![Seznam sestav chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Podrobnosti o chybě
Následující data jsou k dispozici v hello podrobné zobrazení pro jednotlivé chyby

* Identifikátory pro hello *objekt AD* související se situací
* Identifikátory pro hello *objekt Azure AD* podílejí (podle vhodnosti)
* Popis chyby a jak toofix
* Související články

![Podrobnosti sestavy chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-hello-error-report-as-csv"></a>Stáhnout hello chybách jako sdílený svazek clusteru
Výběrem hello "Export" tlačítko si můžete stáhnout soubor CSV s všechny hello podrobnosti o všech chyb hello.

## <a name="related-links"></a>Související odkazy
* [Řešení chyb při synchronizaci](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Odolnost duplicitních atributů](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)