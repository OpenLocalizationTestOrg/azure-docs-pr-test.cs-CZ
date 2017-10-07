---
title: "aaaMonitor vaší místní infrastruktury identity v hello cloudu."
description: "Toto je stránka hello Azure AD Connect Health, která popisuje, co je a důvody, proč ji používat."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 84d0b00ec800ba98094343731aa4e7317dfb0c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-hello-cloud"></a>Monitorování vaší místní infrastruktury identit a synchronizace služeb v cloudu hello
Azure Active Directory (Azure AD) Connect Health pomáhá monitorovat a získáte přehled o vaší identity místní infrastruktury a hello synchronizační služby. Umožní vám toomaintain spolehlivé připojení tooOffice 365 a služeb Microsoft Online Services tím zajišťuje monitorování klíčových komponent identity jako jsou třeba servery služby Active Directory Federation Services (AD FS), (označované taky servery Azure AD Connect jako synchronizační stroj) řadiče domény služby Active Directory, atd. Také udržuje klíčové datové body hello o těchto součástí snadno přístupné, abyste měli využití a jiné důležité statistiky toomake informován rozhodnutí.

Hello informace jsou poskytovány hello [portálu Azure AD Connect Health](https://aka.ms/aadconnecthealth). Na portálu Azure AD Connect Health hello můžete zobrazit výstrahy, monitorování výkonu, analýzy využití a další informace. Azure AD Connect Health umožňuje hello jeden přehledu o stavu klíčových komponent identity na jednom místě.

![Co je Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Funkce hello v Azure AD Connect Health zvýšit, portál hello poskytuje jeden řídicího panelu pomocí hello přehledu identity. Můžete pracovat i více robustní, kvalitnější a propojenější prostředí pro vaše uživatele tooincrease jejich možnost tooget rychleji.

## <a name="why-use-azure-ad-connect-health"></a>Proč používat službu Azure AD Connect Health?
Při integraci místních adresářů se službou Azure AD, jsou vaši uživatelé zvýšit produktivitu, protože je společnou identitu tooaccess jak cloudových a místních prostředků. Tato integrace však vytvoří výzvu hello zajistit, aby toto prostředí bylo v pořádku, tak, aby uživatelé měli spolehlivý přístup k prostředkům místně i v hello cloudu z libovolného zařízení. Azure AD Connect Health pomáhá monitorovat a získáte přehled o vaší místní infrastruktury identity použít tooaccess Office 365 nebo jiné aplikace Azure AD. Stačí jednoduše nainstalovat agenta na každý z vašich místních serverů identity.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[Azure AD Connect Health pro službu AD FS](active-directory-aadconnect-health-adfs.md)
Azure AD Connect Health pro službu AD FS podporuje službu AD FS 2.0 v systémech Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2. Podporuje také monitorování proxy hello AD FS nebo proxy servery webových aplikací, které zajišťují ověřování podporu pro přístup z extranetu. Snadný a nízkonákladové instalaci hello agenta stavu poskytuje Azure AD Connect Health pro službu AD FS hello následující sadu klíčových funkcí:

* Monitorování s výstrahy tooknow proxy servery služby AD FS a AD FS nejsou v pořádku
* E-mailová oznámení pro kritické výstrahy
* Trendy v datech o výkonu, které se hodí pro plánování kapacity služby AD FS
* Funkce analýzy využití služby AD FS přihlášení s pivotů (aplikace, uživatelé, umístění v síti atd.), které jsou užitečné toounderstand jak služby AD FS využívána
* Sestavy pro službu AD FS, například 50 uživatelů s největším počtem chybně zadaných kombinací uživatelského jména a hesla z poslední IP adresy

Hello následující video obsahuje přehled služby Azure AD Connect Health pro službu AD FS.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
Azure AD Connect Health pro synchronizaci monitoruje a poskytuje informace o hello synchronizace, které se vyskytnou mezi vaší místní služby Active Directory a Azure AD. Azure AD Connect Health pro synchronizaci poskytuje hello následující sadu klíčových funkcí:

* Monitorování s tooknow výstrahy, když server služby Azure AD Connect, také známé jako hello synchronizační modul, není v pořádku
* E-mailová oznámení pro kritické výstrahy
* Synchronizace statistik provozu včetně přehledů latencí pro operace synchronizace a trendy v různých operacích, jako jsou přidání, aktualizace nebo odstranění
* Rychlého přehledu informace o vlastnostech synchronizace a posledním úspěšném exportu tooAzure AD
* Sestavy chyb synchronizace na úrovni objektu \(nevyžadují službu Azure AD Premium\)

Hello následující video obsahuje přehled služby Azure AD Connect Health pro synchronizaci.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-dsactive-directory-aadconnect-health-addsmd"></a>[Azure AD Connect Health pro službu AD DS](active-directory-aadconnect-health-adds.md)
Azure AD Connect Health pro službu Active Directory Domain Services (AD DS) podporuje monitorování řadičů domény nainstalovaných v systémech Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016. Hello instalace agenta stavu umožňuje toomonitor je místní prostředí služby AD DS z cloudu hello. Azure AD Connect Health pro službu AD DS poskytuje hello následující sadu klíčových funkcí:

* Monitorování výstrah toodetect řadiče domény jsou není v pořádku a e-mailová oznámení pro kritické výstrahy
* řídicí panel řadiče domény Hello, která poskytuje rychlý přehled o stavu hello a provozní stav řadičů domény
* řídicí panel Hello stav replikace, který má hello nejnovější informace o replikaci a propojuje tootroubleshooting příručky při zjištění chyb
* Rychlý přístup do tooperformance data grafy čítačů oblíbených výkonu, které jsou nezbytné pro monitorování účely a řešení potíží

Hello následující video obsahuje přehled služby Azure AD Connect Health pro službu AD DS.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Začínáme se službou Azure AD Connect Health
tooget začít s Azure AD Connect Health, hello použijte následující kroky:

1. [Získejte předplatné služby Azure AD Premium](../active-directory-get-started-premium.md) nebo [začněte se zkušební verzí](https://azure.microsoft.com/trial/get-started-active-directory/).
2. [Stáhněte a nainstalujte agenty služby Azure AD Connect Health](#download-and-install-azure-ad-connect-health-agent) na servery identity.
3. Zobrazení hello Azure AD Connect Health řídicí panel v [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).

> [!NOTE]
> Mějte na paměti, že před zobrazit data ve vašem řídicím panelu Azure AD Connect Health, je třeba tooinstall hello agenty Azure AD Connect Health na cílové servery.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Stažení a instalace agenta služby Azure AD Connect Health
* Ujistěte se, že jste [splňují požadavky hello](active-directory-aadconnect-health-agent-install.md#requirements) pro Azure AD Connect Health.
* Začínáme s využitím Azure AD Connect Health pro službu AD FS
    * [Stažení agenta Azure AD Connect Health pro službu AD FS](http://go.microsoft.com/fwlink/?LinkID=518973)
    * [Najdete pokyny k instalaci hello](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Začínáme s využitím Azure AD Connect Health pro synchronizaci
    * [Stáhněte a nainstalujte nejnovější verzi Azure AD Connect hello](http://go.microsoft.com/fwlink/?linkid=615771). Hello agenta Health pro synchronizaci se nainstaluje jako součást instalace hello Azure AD Connect (verze 1.0.9125.0 nebo vyšší).
* Začínáme s využitím Azure AD Connect Health pro službu AD DS
    * [Stažení agenta služby Azure AD Connect Health pro službu AD DS](http://go.microsoft.com/fwlink/?LinkID=820540)
    * [Najdete pokyny k instalaci hello](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="azure-ad-connect-health-portal"></a>Portál služby Azure AD Connect Health
portál Azure AD Connect Health Hello zobrazuje zobrazení výstrahy, monitorování výkonu a analýzy využití. Hello https://aka.ms/aadconnecthealth adresu URL přejdete toohello hlavního okno služby Azure AD Connect Health. Je to v podstatě hlavní obrazovka této služby. V hlavním okně hello, uvidíte **rychlý Start**, služby v rámci Azure AD Connect Health a další možnosti konfigurace. V tématu hello následující snímek obrazovky a stručné vysvětlení, které následují hello snímek. Poté, co nasadíte agenty hello, služba stavu hello automaticky identifikuje hello služby, které monitoruje Azure AD Connect Health.

> [!NOTE]
> Informace o licencování najdete v tématu hello [Azure AD Connect – nejčastější dotazy](active-directory-aadconnect-health-faq.md) nebo hello [stránky Azure AD cenová](https://aka.ms/aadpricing).
    
![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/portal4.png)

* **Rychlý Start**: Když vyberete tuto možnost, hello **rychlý Start** otevře se okno. Hello agenta Azure AD Connect Health můžete stáhnout tak, že vyberete **získat nástroje**. Máte také přístup k dokumentaci a můžete nám poskytnout zpětnou vazbu.
* **Active Directory Federation Services**: Tato možnost zobrazí všechny služby hello služby AD FS, Azure AD Connect Health je aktuálně monitorování. Když vyberete instanci, hello okno, které se otevře se zobrazují informace o této instance služby. Tyto informace zahrnují přehled, vlastnosti, výstrahy, monitorování a analýzu využití. Další informace o funkcích hello najdete v [používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md).
* **Azure Active Directory Connect (synchronizace):** Tato možnost ukazuje vaše servery Azure AD Connect, které služba Azure AD Connect Health aktuálně monitoruje. Když vyberete položku hello, hello okno, které se otevře se zobrazují informace o serverech Azure AD Connect. Další informace o funkcích hello najdete v [používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md).
* **Active Directory Domain Services**: Tato možnost zobrazí všechny doménové struktury služby AD DS hello aktuálně monitoruje Azure AD Connect Health. Když vyberete doménové struktuře, hello okno, které se otevře se zobrazují informace o této doménové struktuře. Tyto informace zahrnují přehled základní informace, řadiče domény hello řídicího panelu, řídicí panel hello stav replikace, výstrahy a monitorování. Další informace o funkcích hello najdete v [používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md).
* **Konfigurace**: Tato část obsahuje následující hello tooturn možnosti zapnout nebo vypnout:

  - Automatická aktualizace tooautomatically aktualizace hello Azure AD Connect Health toohello nejnovější verze agenta: Jakmile budou k dispozici bude automaticky aktualizovány toohello nejnovější verze hello agenta Azure AD Connect Health. Tato možnost je ve výchozím nastavení zapnutá.
  - Povolit Microsoft přístup tooyour Azure AD adresáře data o stavu pro řešení potíží s pouze pro účely: Pokud je tato možnost povolena, můžete zobrazit Microsoft hello stejná data, která se zobrazí. Tyto informace můžou pomoci při řešení potíží a problémů. Tato možnost je ve výchozím nastavení zakázána.

## <a name="related-links"></a>Související odkazy
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
