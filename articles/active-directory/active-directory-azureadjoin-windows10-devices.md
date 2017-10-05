---
title: "Použití zařízení s Windows 10 na vašem pracovišti | Microsoft Docs"
description: "Poskytuje snímek možnosti pro uživatele a IT, kontrastní různé způsoby zařízení můžete zřídit a použít v podniku se systémem Windows 10."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: 94ccc8fd-b17b-4fda-8d56-9d87aa37a9f9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 451842f764898af65dd7300e8b48442d256cea7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-windows-10-devices-in-your-workplace"></a>Použití zařízení s Windows 10 ve vaší síti na pracovišti
Platí pro: počítačů s Windows 10

Windows 10 nabízí tři modely pro organizace, které umožňují uživatelům přístup k pracovním prostředkům v zabezpečené a pohodlný způsob.

* **Azure Active Directory Join** (Azure AD Join), pro pracovníky, kteří přistupují k prostředkům, například Office 365 především v cloudu. Připojení k Azure AD je pracovní samoobslužné zřizování prostředí, které je nového ve Windows 10.
* **Připojení k doméně**, pro organizace, které mají investice do místní aplikace a prostředky. Připojení k doméně nabízí vylepšené prostředí ve Windows 10 při připojení k Azure AD.
* **Nový zjednodušený model BYOD možnosti**, uživatelé, kteří chtějí přidat pracovní nebo školní do Windows a snadno přístupem k prostředkům z osobních zařízení.

Následující tabulka představuje snímek možnosti pro uživatele a správce IT, kontrastní různé způsoby zařízení můžete zřídit a použít v podniku se systémem Windows 10:

|  | Připojení k doméně | Připojení k Azure AD | Osobní zařízení |
| --- | --- | --- | --- |
| Windows přihlašování zařízení pro pracovní nebo školní účty. |Ano |Ano |Ne |
| Uživatel jednotného přihlašování (SSO) k aplikacím Office 365 a Azure AD. Jednotné přihlašování je schopnost se přihlásit pouze jednou pro přístup k prostředkům organizace. |Ano |Ano |Ano |
| Uživatel jednotného přihlašování k aplikacím pomocí protokolu Kerberos nebo NTLM. |Ano |Omezená |Ano, prostřednictvím sítě VPN |
| Silné ověřování a pohodlný přihlášení pro pracovní nebo školní účty s Microsoft Passport a Windows Hello. |Ano |Ano |Ano |
| Přístup do firemní sítě Windows Store s pracovní nebo školní účet (ne účet Microsoft). |Ano |Ano |Ano |
| Kompatibilní se standardem Enterprise roaming uživatelská nastavení v zařízeních pomocí pracovní nebo školní účty. |Ano |Ano |Ano |
| Možnost omezení přístupu k organizační aplikace na zařízení, která jsou kompatibilní se zásadami organizace zařízení. |Ano |Ano |Ano |
| Uživatel samoobslužné služby zřizování zařízení pro "práce z libovolného místa." |Ne |Ano |Ano |
| Schopnost spravovat zařízení. |Ano, prostřednictvím zásad skupiny nebo SCCM |Ano |Ano |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Použití zařízení ve vlastnictví práce s Azure AD Join a domény připojení v systému Windows 10
Windows 10 nabízí dva způsoby, kterými zařízení ve vlastnictví pracovní pro přístup k pracovním prostředkům:

* Připojení k Azure AD
* Připojení k doméně
  
  Jak může být platné možnosti v závislosti na potřebách organizace a požadavcích. V některých případech mohou organizace využít povolení obě metody nasazení.

## <a name="when-to-use-azure-active-directory-join"></a>Kdy použít Azure Active Directory Join
Připojení k Azure AD je nové pracovní samoobslužné zřizování prostředí ve Windows 10.  Zaměřují se na pracovníky, kteří přístup k pracovním prostředkům, například Office 365 především v cloudu. Je lehký způsob, jak konfigurovat počítače, tablety a telefony pro podnik. Zařízení se spravují přes správu mobilních zařízení pomocí konzistentní ovládacích prvků napříč platformami systému Windows.

**Použít Azure AD Join pro některý z těchto důvodů**:

* Chcete povolit samoobslužné zřizování zařízení pro pracovníky na cestách.
* Můžete uživatelům poskytnout vlastněná pracovní mobilní zařízení, jako jsou tablety a telefony.
* Chcete spravovat sadu uživatelů ve službě Azure AD místo ve službě Active Directory, jako je například sezónních pracovníků, dodavatelů nebo studenty.
* Chcete poskytovat spojující možnosti pracovní procesy ve firemní pobočky ve vzdálených pobočkách s omezenou na místní infrastrukturu.
* Nemáte místní Active Directory.

Některé organizace používat Azure AD Join jako primární způsob, jak nasadit zařízení vlastněných pracovní, zejména migrovat většinu nebo všechny své prostředky do cloudu. Hybridní organizace pomocí služby Active Directory a Azure AD můžete také nasadit jedno z těchto metod nebo jiné v závislosti na uživatele nebo oddělení.

Školní okresech a vysoké školy, například může spravovat zaměstnanci ve službě Active Directory a studenti ve službě Azure AD. Některé společnosti chtít spravovat vzdálených pobočkách nebo prodejní oddělení ve službě Azure AD. V rámci organizace hybridní můžete použít Azure AD Join i metody připojení k doméně. Připojení k Azure AD může být skvělým doplněk k připojení k doméně pro nasazení zařízení v pracovním prostředí.

**Pokud nejběžnější sestavy přístup k podnikovým zdrojům se přes cloud, může vaše organizace Pokud Získejte další výhody**:

* Odebráním závislosti na místní infrastrukturu identity.
* Nasazení modelu zařízení, můžete zjednodušit získávání z vytváření bitové kopie řešení tím, že se konfigurace samoobslužné služby.
* Správa mobilních zařízení můžete spravovat všechna zařízení v rámci různých platformách.

Další informace o připojení ke službě Azure AD najdete v tématu [rozšíření možností cloudu k zařízení s Windows 10 prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Kdy použít připojení k doméně (nebo ji používat dál)
V posledních 15 letech řada organizací použili připojení k doméně připojit pracovní zařízení. Umožňuje uživatelům přihlašovat do jejich zařízení v práci služby Active Directory nebo školní účty. Připojení k doméně také umožňuje IT centrálně a plně spravovat tato zařízení. Organizace se zpravidla spoléhají na vytváření bitové kopie metody pro zřizování zařízení a často používají k jejich správě System Center Configuration Manager (SCCM) nebo zásad skupiny (zásady skupiny).

**Podniku by měl použít připojení k domény (nebo ji používat dál) z těchto důvodů**:

* Máte Win32 aplikace nasazené do těchto zařízení, které používají protokol NTLM nebo Kerberos.
* Vyžadujete, aby zásady skupiny nebo SCCM nebo DCM ke správě zařízení.
* Chcete nadále používat pro vytváření bitových kopií řešení pro konfiguraci zařízení pro vaši zaměstnanci.

**Připojení k doméně v systému Windows 10 také poskytuje následující výhody při připojení k Azure AD**:

* Silné ověřování vázané na zařízení a pohodlný přihlášení pro pracovní nebo školní účty s Microsoft Passport a Windows Hello.
* Přístup k podnikové síti Windows Store pro zařízení, která používáte pracovní nebo školní účty (bez účtu Microsoft vyžaduje).
* Kompatibilní se standardem Enterprise roaming uživatelských nastavení mezi zařízeními, které používají pracovní nebo školní účty (bez účtu Microsoft vyžaduje).
* Možnost omezení přístupu k organizační aplikace na zařízení, která jsou kompatibilní se zásadami organizace zařízení.

Další informace o připojení ke službě Azure AD najdete v tématu [rozšíření možností cloudu k zařízení s Windows 10 prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Povolit připojení zařízení patřících uživatelům práci nebo škole
Na podporu modelu BYOD v podnikové síti, Windows 10 dává uživateli možnost Přidat pracovní nebo školní účet do jejich počítače, tabletu nebo telefonu. Uživatel přidá pracovní nebo školní účet, je zařízení zaregistrované s Azure AD a volitelně zaregistrované v systému pro správu mobilních zařízení, který byl nakonfigurován v organizaci. Adresář se zobrazí tato zařízení jako "Registrovaná" vs. Azure AD připojený. IT mohou správci používat různé zásady na základě této informace poskytuje odlehčený touch na zařízení patřících uživatelům než na zařízení ve vlastnictví práce v případě potřeby.

Uživatele můžete přidat pracovní nebo školní účet osobního zařízení velmi pohodlně. Dělají to při přístupu k pracovní aplikace poprvé, nebo ho mohou provádět ručně pomocí nabídky nastavení. Tento účet bude poskytovat jednotného přihlašování k prostředkům organizace.

Další informace o připojení ke službě Azure AD najdete v tématu [připojení zařízení připojených k doméně ke službě Azure AD pro Windows 10 vyskytne](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Povolit připojení k doméně nebo připojení k Azure AD
Všechny metody popsané výše (připojení k doméně, připojení k Azure AD a přidat pracovní nebo školní účet) k dispozici vstupních bodů v uživatelské prostředí Windows 10. Všechny ale vyžadují správcem IT k povolení funkcí v infrastruktuře, než bude pracovat na prostředí.

## <a name="requirements-for-deploying-azure-ad-join"></a>Požadavky pro nasazení Azure AD Join
K nasazení Azure AD Join pro všechny sadu uživatelů, budete potřebovat následující:

* Předplatné služby Azure AD.
* Předplatné Azure AD Premium, jako je mobilní zařízení správy automatického zápisu, pokud budete potřebovat další možnosti.
* Správa mobilních zařízení – například předplatné Microsoft Intune, Správa mobilních zařízení pro Office 365, nebo partnera dodavatele správy mobilních zařízení, které se integrují s Azure AD. (Viz [v části Nejčastější dotazy](#frequently-asked-questions) téměř na konci tohoto článku informace).

Pokud vaše zařízení hybridní, důrazně doporučujeme nasadit Azure AD Connect místní adresář rozšířit do Azure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Požadavky pro připojení k doméně pomocí Azure AD
Připojení k doméně bude pořád fungovat, protože má vždy. Však získat výhody Azure AD budete potřebovat následující:

* Předplatné služby Azure AD.
* Nasazení služby Azure AD Connect místní adresář rozšířit do Azure AD.
* Zásada, která umožňuje zařízení připojených k doméně, se podmíněný přístup do služby Azure AD.
* Zásada, která umožňuje přístup k "" připojená k doméně, pokud chcete mít možnost omezit přístup pro některá zařízení.
* System Center Configuration Manager verze 1509 pro Technical Preview, a povolte tak pravidla pro vyžádání kompatibilní zařízení. (Viz dokumentace TechNet a blogu).

Další informace o připojení k doméně v systému Windows 10 najdete v tématu [připojení zařízení připojených k doméně ke službě Azure AD pro Windows 10 vyskytne](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Požadavky na použití vlastní zařízení a "Přidat pracovní nebo školní účet"
Chcete-li povolit "přineste si vlastní zařízení" (BYOD) s pracovní nebo školní účty, potřebujete následující:

* Předplatné služby Azure AD.
* Předplatné Azure AD Premium, jako je mobilní zařízení správy automatického zápisu, pokud budete potřebovat další možnosti.

## <a name="requirements-for-using-microsoft-passport"></a>Požadavky pro používání Microsoft Passport
Pokud chcete povolit Microsoft Passport, budete potřebovat následující:

* Infrastrukturu veřejných klíčů (PKI) pro podporu ověřování pomocí certifikátů, který používá Microsoft Passport.
* Předplatné služby Intune pro podporu ověřování pomocí certifikátů, který používá Microsoft Passport pro připojení ke službě Azure AD a pracovní nebo školní účty.
* System Center Configuration Manager verze 1509 pro Technical Preview (viz dokumentace TechNet a blogu) pro podporu ověřování pomocí certifikátů, který používá Microsoft Passport pro připojení k doméně.
* Zásady pro povolení Microsoft Passport v organizaci.

Jako alternativu k použití infrastruktury veřejných KLÍČŮ můžete povolit na základě klíčů Microsoft Passport následujícím způsobem:

* Nasazení řadiče domény systému Windows Server 2016 "Produkční Preview 1" (není nutné mít funkční úrovně domény nebo doménové struktury; několik řadičů domény pro slouží redundance, které postačí každou lokalitu služby Active Directory).
* Nastavte zásady Povolit Microsoft Passport v organizaci.

Další informace o Microsoft Passport a Windows Hello ve Windows 10 najdete v < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Které produkty partnera pro správu mobilních zařízení integraci s Azure AD?
Následující produkty dodavatele integraci se službou Azure AD pro jednotné registrace a podmíněného přístupu v systému Windows 10:

* AirWatch VMware
* Citrix Xenmobile
* Lightspeed Mobile Manager
* SOTI místní Správa mobilních zařízení
* MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Co o síti na pracovišti připojit ve Windows 10?
Připojení k pracovní ploše bylo používané ve Windows 8.1 pro povolit model BYOD. Ve Windows 10 je povoleno BYOD prostřednictvím "Přidat pracovní nebo školní účet", jak je popsáno výše v tomto dokumentu. Pro organizace, které není jejich správu mobilních zařízení integraci se službou Azure AD, uživatelé mohou registrovat zařízení ke správě ručně prostřednictvím **nastavení** > **účty** > **přístup do práce**.

### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Mohou uživatelé připojit svůj účet Microsoft k účtu domény ve Windows 10?
Není v systému Windows 10. Ve Windows 8.1 uživatelé zařízení připojených k doméně může "připojit" svůj účet Microsoft (například Hotmail, živé, Outlook, Xbox, atd.) k jejich účet domény, aby měla určité prostředí jako jednotné přihlašování do služby Live, použití Windows Store a roaming uživatelských nastavení mezi zařízeními. Ve Windows 10 byl vyřazen "připojit" funkce účtu Microsoft. Uživatele můžete přidat jeden nebo více účtů Microsoft jako další účty umožňující jednotného přihlašování k příjemce služby, jako je například úložiště systému Windows. To se provádí v **nastavení** > **účty** > **účtu**.

Uživatelé, kteří upgradují ze zařízení s Windows 8.1 připojený k doméně a kdo měl svůj účet Microsoft připojen, bude mít automaticky přidán do seznamu další účty, které používají jejich připojeného účtu Microsoft.

## <a name="additional-information"></a>Další informace
* [Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

