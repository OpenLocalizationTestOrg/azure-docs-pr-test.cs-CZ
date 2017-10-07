---
title: "zařízení aaaUsing Windows 10 ve vaší síti na pracovišti | Microsoft Docs"
description: "Poskytuje snímek možnosti pro uživatele a IT, kontrastní hello různé způsoby zařízení můžete zřídit a použít v podniku se systémem Windows 10."
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
ms.openlocfilehash: 8767e1649ced8737d20875f425c24198dcaa7f6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-10-devices-in-your-workplace"></a>Použití zařízení s Windows 10 ve vaší síti na pracovišti
Platí pro: počítačů s Windows 10

Windows 10 nabízí tři modely pro organizace, které umožňují uživatelům tooaccess pracovním prostředkům v zabezpečené a pohodlný způsob.

* **Azure Active Directory Join** (Azure AD Join), pro pracovníky, kteří přistupují k prostředkům, například Office 365 především v cloudu hello. Připojení k Azure AD je pracovní samoobslužné zřizování prostředí, které je nového ve Windows 10.
* **Připojení k doméně**, pro organizace, které mají investice do místní aplikace a prostředky. Připojení k doméně nabízí vylepšené prostředí ve Windows 10 při připojení tooAzure AD.
* **Nový zjednodušený model BYOD možnosti**pro účet uživatele, kteří chtějí tooadd pracovní nebo školní tooWindows a snadný přístup k prostředkům z osobních zařízení.

Hello následující tabulka představuje snímek možnosti pro uživatele a správce IT, kontrastní hello různé způsoby zařízení můžete zřídit a použít v podniku se systémem Windows 10:

|  | Připojení k doméně | Připojení k Azure AD | Osobní zařízení |
| --- | --- | --- | --- |
| Windows přihlašování zařízení pro pracovní nebo školní účty. |Ano |Ano |Ne |
| Uživatel jednotného přihlašování (SSO) tooOffice 365 a aplikace Azure AD. Jednotné přihlašování je hello možnost toosign v právě jednou tooaccess prostředkům organizace. |Ano |Ano |Ano |
| Aplikace tooKerberos/NTLM jednotné přihlašování uživatelů. |Ano |Omezená |Ano, prostřednictvím sítě VPN |
| Silné ověřování a pohodlný přihlášení pro pracovní nebo školní účty s Microsoft Passport a Windows Hello. |Ano |Ano |Ano |
| Přístup k úložišti Windows tooenterprise s pracovní nebo školní účet (ne účet Microsoft). |Ano |Ano |Ano |
| Kompatibilní se standardem Enterprise roaming uživatelská nastavení v zařízeních pomocí pracovní nebo školní účty. |Ano |Ano |Ano |
| Hello možnost toorestrict přístup tooorganizational aplikace toodevices, které jsou kompatibilní se zásadami organizace zařízení. |Ano |Ano |Ano |
| Uživatel samoobslužné služby zřizování zařízení pro "práce z libovolného místa." |Ne |Ano |Ano |
| Možnost toomanage zařízení. |Ano, prostřednictvím zásad skupiny nebo SCCM |Ano |Ano |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Použití zařízení ve vlastnictví práce s Azure AD Join a domény připojení v systému Windows 10
Windows 10 nabízí dva způsoby, kterými zařízení ve vlastnictví pracovní tooaccess pracovním prostředkům:

* Připojení k Azure AD
* Připojení k doméně
  
  Jak může být platné možnosti v závislosti na potřebách organizace a požadavcích. V některých případech mohou organizace využít povolení obě metody nasazení.

## <a name="when-toouse-azure-active-directory-join"></a>Když se toouse Azure Active Directory k
Připojení k Azure AD je nové pracovní samoobslužné zřizování prostředí ve Windows 10.  Zaměřují se na pracovníky, kteří přístup k pracovním prostředkům, například Office 365 především v cloudu hello. Je tooconfigure lightweight způsob, jak počítače, tablety a telefony pro podnik hello. Zařízení se spravují přes správu mobilních zařízení pomocí konzistentní ovládacích prvků napříč platformami systému Windows.

**Použít Azure AD Join pro některý z těchto důvodů**:

* Chcete tooenable hello samoobslužné zřizování zařízení naleznete pracovních procesů na hello.
* Můžete uživatelům poskytnout vlastněná pracovní mobilní zařízení, jako jsou tablety a telefony.
* Chcete toomanage sadu uživatelů ve službě Azure AD místo ve službě Active Directory, jako je například sezónních pracovníků, dodavatelů nebo studenty.
* Chcete tooprovide spojující možnosti tooworkers ve firemní pobočky ve vzdálených pobočkách s omezenou na místní infrastrukturu.
* Nemáte místní Active Directory.

Některé organizace používat Azure AD Join jako hello primární způsob toodeploy pracovní zařízení ve vlastnictví, zejména se migrovat většinu nebo všechny své toohello cloudové prostředky. Hybridní organizace pomocí služby Active Directory a Azure AD můžete také zvolit jednu metodu toodeploy nebo jiné v závislosti na hello uživatele nebo oddělení.

Školní okresech a vysoké školy, například může spravovat zaměstnanci ve službě Active Directory a studenti ve službě Azure AD. Některé společnosti může být vhodné toomanage vzdálených pobočkách nebo prodejní oddělení ve službě Azure AD. V rámci organizace hybridní můžete použít Azure AD Join i metody připojení k doméně. Připojení k Azure AD může být skvělým doplňku toodomain spojení pro nasazení zařízení v pracovním prostředí.

**Pokud hello nejběžnější sestavy přístup k prostředkům tooenterprise prostřednictvím hello cloudu, může vaše organizace Pokud Získejte další výhody**:

* Odebráním závislosti na místní infrastrukturu identity.
* Nasazení modelu zařízení, můžete zjednodušit získávání z vytváření bitové kopie řešení tím, že se konfigurace samoobslužné služby.
* Všechna zařízení toomanage správy mobilních zařízení můžete použít v různých platformách.

Další informace o připojení ke službě Azure AD najdete v tématu [rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="when-toouse-domain-join-or-keep-using-it"></a>Při připojení k doméně toouse (nebo ji používat)
Pro hello posledních 15 let, řada organizací použili zařízení pracovní tooconnect připojení k doméně. Umožňuje uživatelům toosign v tootheir zařízení s jejich služby Active Directory pracovní nebo školní účty. Připojení k doméně také umožňuje IT toocentrally a plně spravovat tato zařízení. Organizace se zpravidla spoléhají na zařízení tooprovision metody pro zpracování obrázků a často používají toomanage System Center Configuration Manager (SCCM) nebo zásad skupiny (GP) je.

**Podniku by měl použít připojení k domény (nebo ji používat dál) z těchto důvodů**:

* Máte Win32 aplikace nasazené toothese zařízení, které používají protokol NTLM nebo Kerberos.
* Vyžadujete, aby zásady skupiny nebo SCCM nebo DCM toomanage zařízení.
* Chcete toocontinue toouse vytváření bitové kopie řešení tooconfigure zařízení pro vaši zaměstnanci.

**Připojení k doméně v systému Windows 10 také poskytuje následující hello výhody při připojení tooAzure AD**:

* Silné ověřování vázané na zařízení a pohodlný přihlášení pro pracovní nebo školní účty s Microsoft Passport a Windows Hello.
* Přístup k podnikovým toohello Windows Store pro zařízení, která používáte pracovní nebo školní účty (bez účtu Microsoft vyžaduje).
* Kompatibilní se standardem Enterprise roaming uživatelských nastavení mezi zařízeními, které používají pracovní nebo školní účty (bez účtu Microsoft vyžaduje).
* Hello možnost toorestrict přístup tooorganizational aplikace toodevices, které jsou kompatibilní se zásadami organizace zařízení.

Další informace o připojení ke službě Azure AD najdete v tématu [rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Povolit připojení zařízení patřících uživatelům práci nebo škole
toosupport BYOD v hello enterprise, Windows 10 dává hello uživatele hello možnost tooadd pracovní nebo školní účet tootheir počítače, tabletu nebo telefonu. Po hello uživatel přidá pracovní nebo školní účet, zařízení hello se zaregistrovat ve službě Azure AD a volitelně zaregistrované v systému pro správu mobilních zařízení hello nakonfigurovaný hello organizace. Hello directory se zobrazí tato zařízení jako "Registrovaná" vs. Azure AD připojený. IT mohou správci používat různé zásady na základě této informace poskytuje odlehčený touch na zařízení patřících uživatelům než na zařízení ve vlastnictví práce v případě potřeby.

Uživatele můžete přidat pracovní nebo školní účet tootheir osobní zařízení velmi pohodlně. Dělají to při přístupu k aplikace pracovní pro hello poprvé, nebo ji mohou provádět ručně pomocí nabídky nastavení hello. Tento účet bude poskytovat prostředky tooorganizational jednotné přihlašování.

Další informace o připojení ke službě Azure AD najdete v tématu [připojení zařízení připojených k doméně tooAzure AD pro Windows 10 vyskytne](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Povolit připojení k doméně nebo připojení k Azure AD
Všechny metody popsané výše (připojení k doméně, připojení k Azure AD a přidat pracovní nebo školní účet) k dispozici vstupních bodů v činnost koncového uživatele hello Windows 10. Ale všechny IT správce tooenable hello funkce v infrastruktuře hello vyžadovat předtím, než bude fungovat hello prostředí.

## <a name="requirements-for-deploying-azure-ad-join"></a>Požadavky pro nasazení Azure AD Join
toodeploy Azure AD Join pro všechny skupiny uživatelů, je třeba hello následující:

* Předplatné služby Azure AD.
* Předplatné Azure AD Premium, jako je mobilní zařízení správy automatického zápisu, pokud budete potřebovat další možnosti.
* Správa mobilních zařízení – například předplatné Microsoft Intune, Správa mobilních zařízení pro Office 365, nebo žádný hello partnera mobilních zařízení správu dodavatelů, které se integrují s Azure AD. (Viz hello [v části Nejčastější dotazy](#frequently-asked-questions) téměř hello konci tohoto článku informace).

Pokud vaše zařízení hybridní, důrazně doporučujeme nasadit Azure AD Connect tooextend hello místní adresář tooAzure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Požadavky pro připojení k doméně pomocí Azure AD
Připojení k doméně pokračuje toowork, jako má vždy. Výhody hello Azure AD tooget však musíte hello následující:

* Předplatné služby Azure AD.
* Nasazení Azure AD Connect tooextend hello místní adresář tooAzure AD.
* Zásada, která umožňuje tooAzure podmíněného přístupu toohave připojená k doméně AD.
* Zásada, která umožňuje přístup příliš "" připojená k doméně, pokud chcete přístup mít toorestrict toobe pro některá zařízení.
* System Center Configuration Manager verze 1509 pro Technical Preview, tooenable pravidla pro vyžádání kompatibilní zařízení. (Viz hello dokumentaci TechNet a příspěvku na blogu).

Další informace o připojení k doméně v systému Windows 10 najdete v tématu [připojení zařízení připojených k doméně tooAzure AD pro Windows 10 vyskytne](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Požadavky na použití vlastní zařízení a "Přidat pracovní nebo školní účet"
tooenable "přineste si vlastní zařízení" (BYOD) s pracovní nebo školní účty, musíte hello následující:

* Předplatné služby Azure AD.
* Předplatné Azure AD Premium, jako je mobilní zařízení správy automatického zápisu, pokud budete potřebovat další možnosti.

## <a name="requirements-for-using-microsoft-passport"></a>Požadavky pro používání Microsoft Passport
tooenable Microsoft Passport, budete potřebovat následující hello:

* Infrastrukturu veřejných klíčů (PKI) pro podporu ověřování pomocí certifikátů, který používá Microsoft Passport.
* Předplatné služby Intune pro podporu ověřování pomocí certifikátů, který používá Microsoft Passport pro připojení ke službě Azure AD a pracovní nebo školní účty.
* System Center Configuration Manager verze 1509 pro Technical Preview (viz hello dokumentaci TechNet a příspěvku na blogu) pro podporu ověřování pomocí certifikátů, který používá Microsoft Passport pro připojení k doméně.
* Zásady pro povolení Microsoft Passport v organizaci hello.

Jako alternativní toousing infrastruktury veřejných KLÍČŮ můžete povolit na základě klíčů Microsoft Passport díky hello následující:

* Nasazení řadiče domény systému Windows Server 2016 "Produkční Preview 1" (není nutné mít funkční úrovně domény nebo doménové struktury; několik řadičů domény pro slouží redundance, které postačí každou lokalitu služby Active Directory).
* Nastavte zásady tooenable Microsoft Passport v organizaci hello.

Další informace o Microsoft Passport a Windows Hello ve Windows 10 najdete v < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Které produkty partnera pro správu mobilních zařízení integraci s Azure AD?
Hello následující produkty dodavatele integraci se službou Azure AD pro jednotné registrace a podmíněného přístupu v systému Windows 10:

* AirWatch VMware
* Citrix Xenmobile
* Lightspeed Mobile Manager
* SOTI místní Správa mobilních zařízení
* MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Co o síti na pracovišti připojit ve Windows 10?
Připojení k síti na pracovišti byla použita ve Windows 8.1 tooenable BYOD. Ve Windows 10 je povoleno BYOD prostřednictvím "Přidat pracovní nebo školní účet", jak je popsáno výše v tomto dokumentu. Pro organizace, které není jejich správu mobilních zařízení integraci se službou Azure AD, uživatelé můžou hello zařízení zaregistrovat do správy ručně prostřednictvím **nastavení** > **účty**  >  **Přístup do práce**.

### <a name="can-users-connect-their-microsoft-account-tootheir-domain-account-in-windows-10"></a>Mohou uživatelé připojit svůj účet tootheir domény účet Microsoft v systému Windows 10?
Není v systému Windows 10. Ve Windows 8.1 uživatelé zařízení připojených k doméně může "připojit" jejich Microsoft účtu (například Hotmail, živé, Outlook, Xbox, atd.) tootheir domény účtu tooenable určité prostředí jako jednotné přihlašování tooLive služby, použijte hello Windows Store a roaming uživatelská nastavení mezi zařízeními. Ve Windows 10 byl vyřazen hello "připojit" funkce účtu Microsoft. Hello uživatele můžete přidat jeden nebo více účtů Microsoft jako další účty tooenable jednotného přihlašování k tooconsumer službám, jako je hello Windows Store. To se provádí v **nastavení** > **účty** > **účtu**.

Uživatelé, kteří upgradují ze zařízení s Windows 8.1 připojený k doméně a kdo měl svůj účet Microsoft připojen, bude automaticky jejich připojeného účtu Microsoft přidali toohello seznam dalších účtů, které používají.

## <a name="additional-information"></a>Další informace
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

