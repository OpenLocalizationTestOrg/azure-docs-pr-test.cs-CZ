---
title: "Azure AD Domain Services: Řadiče domény tooDIY porovnat Azure AD Domain Services | Microsoft Docs"
description: "Porovnání řadiče domény tooDIY Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: maheshu
ms.openlocfilehash: 5e384f6a676e76e4f22ff62d4aeb578bc8481ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodecide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Jak toodecide Pokud služba Azure AD Domain Services je nejvhodnější pro váš případ použití
S Azure AD Domain Services můžete nasadit úlohy ve službách infrastruktury Azure, bez nutnosti tooworry o údržbu infrastruktury identity v Azure. Tato spravované služby se liší od typické nasazení systému Windows Server Active Directory, které můžete nasadit a spravovat sami. Služba Hello je snadno toodeploy a poskytuje automatizované stavu monitorování a nápravu. Jsme neustále vyvíjí hello služby tooadd podporu pro nejčastější scénáře nasazení.

toodecide zda toouse Azure AD Domain Services doporučujeme následující čtení materiálu hello:

* Zobrazit seznam hello [funkce nabízené službou Azure AD Domain Services](active-directory-ds-features.md).
* Zkontrolujte běžné [scénáře nasazení služby Azure AD Domain Services](active-directory-ds-scenarios.md).
* Nakonec [porovnání služby Azure AD Domain Services tooa samoobslužné AD možnost](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).

## <a name="compare-azure-ad-domain-services-toodiy-ad-domain-in-azure"></a>Porovnání služby Azure AD Domain Services tooDIY AD domény v Azure
Hello následující tabulka vám pomůže rozhodnout mezi pomocí Azure AD Domain Services a správu vlastní infrastrukturu AD v Azure.

| **Funkce** | **Služba Azure AD Domain Services** | **'Samoobslužné' AD ve virtuálních počítačích Azure** |
| --- |:---:|:---:|
| [**Spravované služby**](active-directory-ds-comparison.md#managed-service) |**&amp;#x2713;** |**& #x 2715;** |
| [**Bezpečné nasazení**](active-directory-ds-comparison.md#secure-deployments) |**&amp;#x2713;** |Správce musí toosecure hello nasazení. |
| [**DNS server**](active-directory-ds-comparison.md#dns-server) |**& #x 2713;**  (spravované služby) |**&amp;#x2713;** |
| [**Oprávnění správce domény nebo Enterprise**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges) |**& #x 2715;** |**&amp;#x2713;** |
| [**Připojení k doméně**](active-directory-ds-comparison.md#domain-join) |**&amp;#x2713;** |**&amp;#x2713;** |
| [**Ověřování v doméně pomocí protokolu NTLM a Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&amp;#x2713;** |**&amp;#x2713;** |
| [**Omezené delegování protokolu Kerberos**](active-directory-ds-comparison.md#kerberos-constrained-delegation)|na základě prostředků|na základě prostředků & založené na klientech|
| [**Vlastní struktury organizační jednotky**](active-directory-ds-comparison.md#custom-ou-structure) |**&amp;#x2713;** |**&amp;#x2713;** |
| [**Rozšíření schématu**](active-directory-ds-comparison.md#schema-extensions) |**& #x 2715;** |**&amp;#x2713;** |
| [**Vztahy důvěryhodnosti domény nebo doménové struktuře AD**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts) |**& #x 2715;** |**&amp;#x2713;** |
| [**Čtení protokolu LDAP**](active-directory-ds-comparison.md#ldap-read) |**&amp;#x2713;** |**&amp;#x2713;** |
| [**Zabezpečený LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap) |**&amp;#x2713;** |**&amp;#x2713;** |
| [**Zápis LDAP**](active-directory-ds-comparison.md#ldap-write) |**& #x 2715;** |**&amp;#x2713;** |
| [**Zásady skupiny**](active-directory-ds-comparison.md#group-policy) |**&amp;#x2713;** |**&amp;#x2713;** |
| [**Geograficky distribuovaná nasazení**](active-directory-ds-comparison.md#geo-dispersed-deployments) |**& #x 2715;** |**&amp;#x2713;** |

#### <a name="managed-service"></a>Spravované služby
Microsoft spravuje domén Azure AD Domain Services. Nemáte tooworry o opravy, aktualizace, monitorování, zálohování a zajištění dostupnosti vaší domény. Tyto úlohy správy jsou nabízena jako služba Microsoft Azure pro vaše spravované domény.

#### <a name="secure-deployments"></a>Bezpečné nasazení
spravované domény Hello je bezpečně uzamčené dle doporučení zabezpečení společnosti Microsoft pro nasazení AD. Tato doporučení kmen z produktového týmu hello AD desetiletí prostředí technici a podpůrné AD nasazení. Pro samoobslužné nasazení, budete potřebovat konkrétní nasazení tootake kroky toolock nižší nebo zabezpečit vaše nasazení.

#### <a name="dns-server"></a>DNS server
Spravované doméně služby Azure AD Domain Services zahrnuje spravované služby DNS. Členům skupiny hello 'Administrators AAD řadič domény, můžete spravovat DNS na hello spravované domény. Členové této skupiny mají úplná oprávnění správy služby DNS pro hello spravované domény. Správa služby DNS je možné provést pomocí hello 'DNS konzole pro správu' součástí balíčku hello vzdálenou správu serveru (RSAT).
[Další informace](active-directory-ds-admin-guide-administer-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Oprávnění domény nebo správce podnikové sítě
Na spravované doméně služby AAD-DS nejsou nabízí tyto zvýšená oprávnění. Aplikace, které vyžadují tyto zvýšená oprávnění nemůže být nasazena ve službě AAD DS spravované domény. Menší podmnožinu oprávnění pro správu je k dispozici toomembers hello delegovanou správu skupiny s názvem 'AAD řadič domény správci'. Tato oprávnění jsou oprávnění tooconfigure DNS, nakonfigurovat zásady skupiny, získání oprávnění správce na počítačích připojených k doméně atd.

#### <a name="domain-join"></a>Připojení k doméně
Můžete připojit virtuální počítače toohello spravované domény podobné toohow připojíte počítače tooan AD domény.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Ověřování v doméně pomocí protokolu NTLM a Kerberos
S Azure AD Domain Services můžete použít tooauthenticate vaše podnikové přihlašovací údaje s hello spravované domény. Přihlašovací údaje jsou synchronizovány s klientovi Azure AD. U synchronizovaného klientů Azure AD Connect zajišťuje, aby změny provedené toocredentials místní synchronizované tooAzure AD. Při instalaci samoobslužné domény může být nutné tooset až domény AD vztah důvěryhodnosti s místní AD pro tooauthenticate uživatelé se svými podnikovými přihlašovacími údaji. Alternativně může být nutné tooset až tooensure replikace AD, synchronizaci hesel uživatelů virtuální počítače řadičů domény Azure tooyour.

#### <a name="kerberos-constrained-delegation"></a>Omezené delegování protokolu Kerberos
Nemáte oprávnění správce domény na spravované doméně služby Active Directory Domain Services. Proto se nedají konfigurovat na základě účtu (tradiční) omezené delegování protokolu Kerberos. Můžete ale nakonfigurovat hello bezpečnější omezeného delegování na základě prostředků.
[Další informace](active-directory-ds-enable-kcd.md)

#### <a name="custom-ou-structure"></a>Vlastní struktury organizační jednotky
Členům skupiny hello 'Administrators AAD řadič domény, můžete vytvořit vlastní organizační jednotky v rámci hello spravované domény. Uživatelé, kteří vytváří vlastní organizační jednotky jsou udělena úplná oprávnění správce přes hello organizační jednotky.
[Další informace](active-directory-ds-admin-guide-create-ou.md)

#### <a name="schema-extensions"></a>Rozšíření schématu
Nelze rozšířit hello základní schéma spravované doméně služby Azure AD Domain Services. Aplikace, které závisí na rozšíření schématu tooAD (například nové atributy v rámci objektu uživatele hello) proto nemůže být odvolány a zapuštěno tooAAD DS domény.

#### <a name="ad-domain-or-forest-trusts"></a>Domény služby AD nebo vztahy důvěryhodnosti doménové struktury
Spravované domény nemůže být nakonfigurované tooset až vztahy důvěryhodnosti (příchozí nebo odchozí) s ostatními doménami. Scénáře nasazení doménové struktury prostředků proto nelze použít Azure AD Domain Services. Podobně nasazení, kde dáváte přednost není toosynchronize tooAzure hesla AD nelze použít Azure AD Domain Services.

#### <a name="ldap-read"></a>LDAP pro čtení
Hello spravované domény podporuje LDAP čtení úlohy. Proto můžete nasadit aplikace, které provádějí operace proti spravované doméně hello čtení protokolu LDAP.

#### <a name="secure-ldap"></a>Zabezpečený LDAP
Můžete konfigurovat Azure AD Domain Services tooprovide zabezpečený LDAP přístup tooyour spravované hello domény, včetně přes internet.
[Další informace](active-directory-ds-admin-guide-configure-secure-ldap.md)

#### <a name="ldap-write"></a>Zápis LDAP
spravované domény Hello je jen pro čtení pro uživatelské objekty. Aplikace, které provádějí LDAP operace zápisu u atributů objektu uživatele hello tedy nefungují v spravované domény. Kromě toho uživatelská hesla nelze změnit z v rámci hello spravované domény. Dalším příkladem může být úprava členství ve skupině nebo skupinu atributů v rámci spravované domény hello, což není povoleno. Ale některé změny toouser atributy nebo hesla provedené ve službě Azure AD (prostřednictvím portálu prostředí PowerShell nebo Azure) nebo místní AD se synchronizují toohello AAD DS spravované domény.

#### <a name="group-policy"></a>Zásady skupiny
Je integrované objektu zásad skupiny každý hello "AADDC počítače" a "Uživatelé AADDC" kontejnerů. Tyto integrované zásad skupiny tooconfigure objekty zásad skupiny můžete přizpůsobit. Členům skupiny hello 'Administrators AAD řadič domény, můžete také vytvořit vlastní objekty zásad skupiny a připojte je tooexisting organizační jednotky (včetně vlastní organizačních jednotek).
[Další informace](active-directory-ds-admin-guide-administer-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>Rozptýlená v geograficky nasazení
Spravované domény služby Azure AD Domain Services jsou k dispozici v jedné virtuální sítě v Azure. Pro scénáře, které vyžadují domény řadičů toobe k dispozici v několika oblastech Azure v rámci hello, world nastavení řadiče domény ve virtuálních počítačích Azure IaaS může být lepší alternativou hello.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>Možnosti nasazení 'Samoobslužné' (DIY) AD
Může mít nasazení případy použití, kde je nutné některé z možností hello nabízená instalace systému Windows Server AD. V těchto případech zvažte mezi hello následující možnosti samoobslužné (DIY):

* **Samostatné doméně cloudu:** můžete nastavit samostatnou cloudovou doménu pomocí Azure virtuální počítače, které jsou nakonfigurované jako řadiče domény. Tato infrastruktura Neintegruje s místní prostředí AD. Tato možnost by vyžadovaly jinou sadu 'cloudu přihlašovací údaje, toologin nebo správě virtuálních počítačů v cloudu hello.
* **Nasazení doménové struktury prostředků:** můžete nastavit domény v doménové struktuře topologii hello prostředků pomocí Azure virtuální počítače nakonfigurované jako řadiče domény. Dále je nutné nakonfigurovat vztah důvěryhodnosti služby AD s místními prostředí AD. Můžete připojení k doméně doménové struktury prostředku toothis počítačů (virtuálních počítačích Azure) v cloudu hello. Ověření uživatele se stane přes buď místní adresář tooyour připojení sítě VPN nebo ExpressRoute.
* **Rozšířit vaše místní domény tooAzure:** se můžete připojit virtuální síti Azure tooyour do místní sítě pomocí připojení VPN nebo ExpressRoute. Toto nastavení umožňuje virtuální počítače Azure toobe tooyour připojené k místní službě AD. Další alternativou je řadičů domény repliky toopromote vaší místní domény v Azure jako virtuální počítač. Vám může potom ho nastavit tooreplicate přes VPN nebo ExpressRoute připojení tooyour místní adresář. V tomto režimu nasazení efektivně rozšiřuje vaše místní domény tooAzure.

> [!NOTE]
> Může určit, že je DIY možnost lépe vhodné pro vaše nasazení případy použití. Vezměte v úvahu [sdílení zpětné vazby](active-directory-ds-contact-us.md) toohelp nám pochopit, co by funkce nápovědy jste zvolili v hello budoucí Azure AD Domain Services. Tato zpětná vazba pomůže nám momentální hello služby toobetter barvy, které potřebuje vaše nasazení a případy použití.
>
>

Jsme publikovali [pokyny pro nasazení systému Windows Server Active Directory ve virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx) toohelp snadněji DIY instalace.

## <a name="related-content"></a>Související obsah
* [Funkce – Azure AD Domain Services](active-directory-ds-features.md)
* [Scénáře nasazení – Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Pokyny pro nasazení systému Windows Server Active Directory na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
