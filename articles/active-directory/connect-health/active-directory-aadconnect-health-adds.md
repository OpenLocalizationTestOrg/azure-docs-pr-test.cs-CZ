---
title: "aaaUsing Azure AD Connect Health se službou AD DS | Microsoft Docs"
description: "Toto je stránka hello Azure AD Connect Health, která popisuje jak toomonitor služby AD DS."
services: active-directory
documentationcenter: 
author: arluca
manager: femila
editor: curtand
ms.assetid: 19e3cf15-f150-46a3-a10c-2990702cd700
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: e2fb6be65407d02c214dcab385b85d6cb54f48de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Používání služby Azure AD Connect Health se službou AD DS
Následující dokumentace Hello je konkrétní toomonitoring Active Directory Domain Services s Azure AD Connect Health. Hello podporované verze služby AD DS jsou: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016.

Další informace o sledování služby AD FS pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md). Kromě toho informace o monitorování služby Azure AD Connect (synchronizace) pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md).

![Azure AD Connect Health pro službu AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Upozornění služby Azure AD Connect Health pro službu AD DS
Hello výstrahy části v rámci Azure AD Connect Health pro službu AD DS, poskytuje seznam aktivní a vyřešené výstrahy, související tooyour řadiče domény. Výběrem aktivního nebo vyřešeného upozornění otevře nové okno s doplňujícími informacemi, společně s kroky řešení a odkazy toosupporting dokumentaci. Každý typ výstrahy může mít jednu nebo více instancí, které odpovídají tooeach řadičů domény hello této konkrétní výstraha týká. Téměř hello dolní části okna výstrah hello poklepáním dotčené doméně řadič tooopen další okno s dalšími podrobnostmi o dané výstrahy instance.

V tomto okně můžete povolit e-mailová oznámení pro výstrahy a změnit časový rozsah hello v zobrazení. Rozšiřování hello časový rozsah můžete toosee předchozí vyřešené výstrahy.

![Chyba synchronizace služby Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Řídicí panel Řadiče domény
Tento řídicí panel nabízí topologické zobrazení prostředí společně s klíčovými provozními metrikami a stavem jednotlivých monitorovaných řadičů domény. metriky Hello zobrazí nápovědu tooquickly identifikovat, všechny řadiče domény, které mohou vyžadovat další šetření. Ve výchozím nastavení je zobrazena pouze podmnožina hello sloupců. Však můžete najít hello celou sadu dostupných sloupců dvojitým kliknutím na příkaz sloupce hello. Výběr hello sloupce, které vám nejvíc záleží, zapne tento řídicí panel do jednoho a snadno umístit tooview hello stav prostředí služby AD DS.

![Řadiče domény](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Řadiče domény můžete seskupovat podle jejich příslušné domény nebo webu, což je užitečné pro pochopení hello prostředí topologie. Nakonec pokud dvakrát kliknete na záhlaví okna hello řídicí panel hello maximalizuje tooutilize hello k dispozici obrazovky-nemovitosti. Toto větší zobrazení je užitečné, když se zobrazuje více sloupců.

## <a name="replication-status-dashboard"></a>Řídicí panel Stav replikace
Tento řídicí panel poskytuje přehled replikace hello stavu a replikační topologie vaší monitorovaných řadičů domény. Stav Hello hello poslední pokus o replikaci je uveden spolu s užitečné dokumentace pro všechny chyby, která se nachází. Poklepáním na řadič domény s touto chybou tooopen nové okno s informacemi, jako: Podrobnosti o chybě hello, doporučené kroky řešení a propojí tootroubleshooting dokumentaci.

![Stav replikace](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Monitorování
Tato funkce poskytuje grafické trendy různých výkonnosti, které se shromažďují průběžně ze všech řadičů domény hello monitorovány. Výkon řadiče domény můžete snadno porovnat se všemi ostatními monitorovanými řadiči domény v doménové struktuře. Kromě toho můžete různé čítače výkonu zobrazit vedle sebe, což je užitečné při řešení problémů ve vašem prostředí.

![Monitorování](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Ve výchozím nastavení jsme jste předem vybrali čtyři čítače výkonu; však můžete zahrnout ostatní kliknutím na příkaz filter hello a výběrem nebo zrušením výběru všechny požadované čítače. Kromě toho poklepáním na tooopen grafu čítače výkonu nové okno, včetně datových bodů pro každou hello monitorované řadiče domény.

## <a name="related-links"></a>Související odkazy
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)

