---
title: "aaaAzure Přehled monitorování a správu zabezpečení | Microsoft Docs"
description: " Azure poskytuje tooaid mechanismy zabezpečení v hello správy a monitorování cloudových služeb Azure a virtuálních počítačů.  Tento článek obsahuje přehled těchto klíčových funkcí zabezpečení a služeb. "
services: security
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: terrylan
ms.openlocfilehash: 0026fa97bab7e15c9f8de6856b5075abe2288f61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-management-and-monitoring-overview"></a>Správa zabezpečení Azure a Přehled monitorování
Azure poskytuje tooaid mechanismy zabezpečení v hello správy a monitorování cloudových služeb Azure a virtuálních počítačů. Tento článek obsahuje přehled těchto klíčových funkcí zabezpečení a služeb. Tooarticles, které poskytují podrobnosti o jednotlivých tak další informace jsou k dispozici odkazy.

zabezpečení vašich cloudových službách Microsoft Hello je spolupráci a sdílenou odpovědnost mezi vámi a Microsoft. Sdílené odpovědnost znamená, že Microsoft je odpovědná za hello Microsoft Azure a fyzického zabezpečení svá datového centra (pomocí ochranu zabezpečení, jako je například uzamčeném oznámení "BADGE" položku dveří, ochranné a chrání). Kromě toho Azure poskytuje silné úrovně zabezpečení cloudu ve vrstvě hello software, který splňuje hello zabezpečení, ochrany osobních údajů a dodržování předpisů potřebám jeho náročné zákazníků.

Vlastníte dat a identity, hello odpovědnost za ochranu jejich, hello zabezpečení místních prostředků a zabezpečení hello součástí cloudu, nad kterými nemáte kontrolu. Microsoft poskytuje s toohelp ovládací prvky a možnosti zabezpečení můžete chránit vaše data a aplikace. Vaše stupeň odpovědnost za zabezpečení je založený na typu hello cloudové služby.

Následující graf Hello shrnuje hello rovnováhu mezi počtem odpovědnost za zákazníků společnosti Microsoft a hello.

![Sdílená odpovědnost][1]

Podrobnější prohlídku do správy zabezpečení, najdete v části [Správa zabezpečení v Azure](azure-security-management.md).

Zde jsou hello základní funkce toobe popsaná v tomto článku:

* Řízení přístupu na základě rolí
* Antimalware
* Multi-factor Authentication
* ExpressRoute
* Brány virtuální sítě
* Správa privilegovaných identit
* Ochrana identit
* Security Center

## <a name="role-based-access-control"></a>Řízení přístupu na základě rolí
Řízení přístupu na základě rolí (RBAC) poskytuje přesnou správu přístupu k prostředkům Azure. Pomocí RBAC, můžete udělit osoby pouze hello úroveň přístupu, které potřebují tooperform svou práci.  RBAC také vám pomůže zkontrolovat, že při osoby nechte hello organizace se ztratit přístup tooresources v cloudu hello.

Další informace:

* [Blog týmu Active Directory na RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
* [Řízení přístupu Azure na základě rolí](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware
S Azure můžete použít antimalwarový software od dodavatelů hlavní zabezpečení, jako je Microsoft, Symantec, Trend Micro, McAfee a Kaspersky toohelp chránit virtuální počítače ze škodlivých souborů, adwaru a dalšími hrozbami.

Antimalware od Microsoftu nabízí že Hello možnost tooinstall antimalwarových agenta pro rolí PaaS a virtuální počítače. Podle toho, System Center Endpoint Protection, tato funkce přináší Principy místní při zabezpečení technologie toohello cloudu.

Nabízíme těsná integrace pro je Trend [hloubkové zabezpečení](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ a [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ produkty ve hello platformy Azure. DeepSecurity je antivirový řešení a SecureCloud je řešení pro šifrování. DeepSecurity je nasadit do virtuálních počítačů pomocí model rozšíření. Pomocí portálu hello uživatelského rozhraní a prostředí PowerShell, můžete zvolit toouse DeepSecurity uvnitř nové virtuální počítače, které jsou právě spuštěné, nebo existující virtuální počítače, které jsou už nasazené.

Symantec koncového bodu ochrany (září) je podporováno také v Azure. Prostřednictvím portálu integraci můžete zadat zákazníky, že chcete toouse září ve virtuálním počítači. ZÁŘÍ se dá nainstalovat na zbrusu novou virtuálního počítače prostřednictvím hello portál Azure nebo se dá nainstalovat na existující virtuální počítač pomocí prostředí PowerShell.

Další informace:

* [Nasazování antimalwarových řešení na virtuálních počítačích Azure](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Antimalware od Microsoftu pro cloudové služby Azure a virtuální počítače](azure-security-antimalware.md)
* [Jak tooinstall a nakonfigurujte Trend Micro hluboké Security jako služba na virtuální počítač s Windows](../virtual-machines/windows/classic/install-trend.md)
* [Jak tooinstall a konfigurace Symantec Endpoint Protection na virtuální počítač s Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Nové možnosti Antimalwarové ochrany virtuálních počítačů Azure – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-factor Authentication
Azure Multi-Factor authentication (MFA) je metoda ověřování, který vyžaduje hello použití více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení toouser přihlášení a transakce. MFA pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování přes celou řadu možností ověření – telefonní hovor, textová zpráva nebo mobilní aplikace oznámení nebo ověřovací kód a třetích stran tokeny OATH.

Další informace:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Jak funguje Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute
Microsoft Azure ExpressRoute umožňuje rozšířit vaše místní sítě do hello cloudu Microsoftu přes vyhrazené soukromé připojení zajišťované poskytovatelem připojení. Prostřednictvím ExpressRoute můžete vytvořit připojení tooMicrosoft cloudové služby, jako je například Microsoft Azure, Office 365 a CRM Online. Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), síť Ethernet typu point-to-point nebo virtuální křížové připojení prostřednictvím poskytovatele připojení ve společném umístění. Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. To umožňuje toooffer připojení ExpressRoute Další spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než Typická připojení přes hello Internet.

Další informace:

* [Technický přehled ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Brány virtuální sítě
Brány sítě VPN, označované taky jako Azure brány virtuální sítě, se používají toosend síťový provoz mezi virtuálními sítěmi a místními umístění. Jsou to taky použít toosend provoz mezi virtuálními sítěmi v rámci Azure (VNet-to-VNet).  Brány sítě VPN poskytují zabezpečení mezi různými místy připojení mezi Azure a infrastruktury.

Další informace:

* [O branách VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Přehled zabezpečení sítě Azure](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Uživatelé někdy potřebují toocarry out privilegované operace v prostředků Azure nebo jiné aplikace SaaS. Často to znamená organizace mají toogive je trvalé privilegovaný přístup v Azure Active Directory (Azure AD). Je to rostoucí bezpečnostní riziko pro hostované cloudové prostředky, proto organizace nelze monitorovat dostatečně co tito uživatelé pracují s jejich privilegovaný přístup.
Navíc pokud uživatelský účet s privilegovaného přístupu je ohrožen, že jeden porušení zabezpečení by mohlo mít vliv celkového zabezpečení cloudu. Azure AD Privileged Identity Management pomáhá tooresolve toto riziko podle snížení času ohrožení hello oprávnění a zvýšení získat přehled o využití.  

Správa privilegovaných identit zavádí koncepci hello k dočasné oprávnění správce k roli nebo "pouze v čase" přístup správce, který je uživatel, který potřebuje toocomplete aktivaci, pro který přiřazenou roli. změny proces aktivace Hello hello přiřazení role tooa hello uživatele ve službě Azure AD z neaktivní tooactive pro zadané časové období, jako je 8 hodin.

Další informace:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Začínáme s Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identity Protection
Ochrany identit Azure Active Directory (AD) poskytuje ucelený přehled podezřelé aktivity přihlášení a potenciální ohrožení zabezpečení toohelp chránit vaši firmu. Ochrana identity zjištění podezřelých aktivit pro uživatele a identity oprávněními (správce), podle signály jako útoku hrubou silou úniku přihlašovacích údajů a přihlášení z neznámých míst a nakažených zařízení.

Zadáním oznámení a doporučené nápravy Identity Protection pomáhá toomitigate rizika v reálném čase. Počítá závažnost riziko uživatelů a můžete nakonfigurovat zásady založené na riziko tooautomatically nápovědy chránit aplikaci přístup z budoucích hrozeb.

Další informace:

* [Ochrany identit Azure Active Directory](../active-directory/active-directory-identityprotection.md)
* [Kanál 9: Azure AD a Identity zobrazení: Identity Protection verze Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Security Center
Azure Security Center pomáhá zabránit, zjistit a reagovat toothreats a poskytuje že vyšší přehled a kontrolu nad, hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Security Center pomáhá optimalizovat a monitorovat hello zabezpečení vašich prostředků Azure pomocí:

* Umožňuje toodefine zásady pro vaše předplatné Azure prostředky podle tooyour zabezpečení společnosti musí a hello typu aplikací nebo citlivosti dat hello v každém předplatném.
* Monitorování stavu hello virtuální počítače Azure, sítě a aplikace.
* Poskytuje seznam nastavovat výstrahy zabezpečení, včetně výstrahy z integrovaných partnerských řešení, společně s hello informace, které potřebujete tooquickly prozkoumat a doporučeními týkajícími tooremediate útoku.

Další informace:

* [Úvod tooAzure Security Center](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
