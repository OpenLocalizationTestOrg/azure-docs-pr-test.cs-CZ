---
title: "Podmíněný přístup na portálu Azure classic | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu na portálu Azure classic zkontrolujte za určitých podmínek při ověřování pro přístup k aplikacím."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: da3f0a44-1399-4e0b-aefb-03a826ae4ead
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.custom: oldportal
ms.openlocfilehash: d96eeb07c4bf3944be82d9c54aea93d52b54a37a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="conditional-access-in-the-azure-classic-portal"></a>Podmíněný přístup na portálu Azure classic

Toto téma se věnuje podmíněného přístupu na portálu Azure classic. Nejnovější informace o podmíněném přístupu ve službě Azure Active Directory najdete v tématu [podmíněného přístupu v Azure Active Directory](active-directory-conditional-access-azure-portal.md).


Možnosti řízení podmíněného přístupu Azure Active Directory (Azure AD) nabízejí jednoduchý způsoby, jak zabezpečeným prostředkům v cloudu a místně. Zásady podmíněného přístupu, jako je vícefaktorové ověřování můžete ochránit proti riziko krádeže a phished pověření. Další zásady podmíněného přístupu můžete pomoct chránit data vaší organizace. Například kromě nutnosti přihlašovací údaje, bude pravděpodobně zásadu této jenom zařízení, která jsou registrovaná v systému pro správu mobilních zařízení, jako je Microsoft Intune můžete získat přístup ke službám citlivé vaší organizace.

## <a name="prerequisites"></a>Požadavky
Podmíněný přístup pro Azure AD je funkce [Azure Active Directory Premium](http://www.microsoft.com/identity). Každý uživatel, který přistupuje k aplikaci, která má uplatnit zásady podmíněného přístupu musí mít licenci na Azure AD Premium. Další informace o licenční požadavky v [zprávu nelicencovaná uživatele](https://aka.ms/utc5ix).

## <a name="how-is-conditional-access-control-enforced"></a>Jak se vynucuje řízení podmíněného přístupu?
Pomocí podmíněného řízení přístupu na místě, Azure AD ověří pro konkrétní podmínky můžete nastavit pro uživatele o přístup k aplikaci. Po splnění požadavků na přístup, uživatel je ověřen a přístup k aplikaci.  

![Přehled podmíněného přístupu](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Podmínky
Toto jsou podmínky, které můžete zahrnout do zásad podmíněného přístupu:

* **Členství ve skupinách**. Řízení přístupu uživatele na základě členství ve skupině.
* **Umístění**. Použijte umístění uživatele k aktivační události služby Multi-Factor authentication a použití ovládacích prvků bloku, pokud uživatel není v důvěryhodné síti.
* **Platforma**. Použijte platformu zařízení, jako je iOS, Android, Windows Mobile nebo Windows, jako podmínku pro použití zásad.
* **Povolit zařízení**. Stav zařízení, zda povolit nebo zakázat, je ověřen během hodnocení zásad zařízení. Pokud zakážete ztracené nebo odcizené zařízení v adresáři, můžete nadále nesplňují požadavky zásad.
* **Přihlášení a uživatele riziko**. Můžete použít [Azure AD Identity Protection](active-directory-identityprotection.md) pro zásady podmíněného přístupu riziko. Zásady podmíněného přístupu rizik pomůže poskytnout vaší organizaci zálohy ochrany na základě rizikových událostech a neobvyklé přihlašovací aktivity.

## <a name="controls"></a>Ovládací prvky
Toto jsou ovládací prvky, které můžete použít k vynucení zásad podmíněného přístupu:

* **Služba Multi-Factor authentication**. Může vyžadovat silné ověřování pomocí služby Multi-Factor authentication. Vícefaktorové ověřování můžete použít s Azure Multi-Factor Authentication nebo pomocí poskytovatele služby Multi-Factor authentication na místě v kombinaci s Active Directory Federation Services (AD FS). Pomocí služby Multi-Factor authentication pomáhá chránit prostředky z přistupuje neoprávněný uživatel, který může mít získal přístup k přihlašovacím údajům platného uživatele.
* **Blok**. Můžete použít podmínky jako umístění uživatele na blokování přístupu uživatele. Například může blokovat přístup, když uživatel není v důvěryhodné síti.
* **Kompatibilní zařízení**. Můžete nastavit zásady podmíněného přístupu na úrovni zařízení. Zásady můžou nastavit tak, aby pouze počítače, které jsou připojené k doméně, nebo mobilní zařízení, která jsou zaregistrovaná pomocí aplikace pro správu mobilních zařízení, můžete přístup k prostředkům vaší organizace. Můžete například Intune používat ke kontrole dodržování předpisů pro zařízení a pak zprávy do služby Azure AD pro vynucení když se uživatel pokusí o přístup k aplikaci. Podrobné pokyny o tom, jak Intune používat k ochraně dat a aplikací najdete v tématu [ochranu dat a aplikací pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Intune taky můžete použít k vynucení ochrana dat pro ztracené nebo odcizené zařízení. Další informace najdete v tématu [Chraňte svá data s využitím plného nebo selektivního vymazání pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Aplikace
Můžete vynutit zásady podmíněného přístupu na úrovni aplikace. Nastavení úrovně přístupu pro aplikace a služby v cloudu nebo místně. Zásady platí přímo k webu nebo službě. Zásady se vynucují pro přístup do prohlížeče a k aplikacím, které přístup ke službě.

## <a name="device-based-conditional-access"></a>Podmíněný přístup využívající zařízení
Můžete omezit přístup k aplikacím ze zařízení, které jsou registrovány s Azure AD a které splňují určité podmínky. Podmíněného přístupu na základě zařízení chrání od uživatelů, kteří se pokusí o přístup k prostředkům z prostředků v organizaci:

* Neznámá nebo nespravovaného zařízení.
* Zařízení, která nesplňují zásady zabezpečení vaší organizace nastavit.

Můžete nastavit zásady v závislosti na tyto požadavky:

* **Připojená k doméně**. Nastavte zásady omezení přístupu k zařízení, které jsou připojeny k doméně služby Active Directory v místě a která jsou zaregistrovaná taky s Azure AD. Tato zásada platí pro stolní počítače, přenosné počítače a tablety enterprise systému Windows.
  Další informace o tom, jak nastavit automatické registrace zařízení připojených k doméně se službou Azure AD najdete v tématu [nastavit automatické registrace zařízení připojených k doméně systému Windows s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
* **Kompatibilní zařízení**. Nastavit zásady omezení přístupu k zařízení, které jsou označené **kompatibilní** v adresáři systému správy. Tato zásada zajistí, že jsou přístup povolený jenom zařízení, které splňují zásady zabezpečení, jako je například vynucení šifrování souborů na zařízení. Tyto zásady můžete omezit přístup z následujícího zařízení:
  
  * **Zařízení se systémem Windows připojených k doméně**. Spravované nástrojem System Center Configuration Manager (v aktuální větev) nasazené v hybridní konfigurace.
  * **Windows 10 Mobile pracovní nebo osobní zařízení**. Spravovat pomocí Intune nebo systém správy podporovaných mobilních zařízení třetích stran.
  * **iOS a zařízení se systémem Android**. Spravovat přes Intune.

Uživatelé, kteří přístup k aplikacím, které jsou chráněny na zařízení, certifikační autorita zásad musí přístup k aplikaci ze zařízení, které splňuje požadavky na tuto zásadu. Pokud pokus byl odepřen přístup na zařízení, který nesplňuje požadavky zásad.

Informace o tom, jak nakonfigurovat zásady certifikační autority založené na zařízení ve službě Azure AD najdete v tématu [nastavit zásady podmíněného přístupu na základě zařízení připojená k Azure Active Directory aplikací](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Zdroje
Naleznete v následujících kategorií prostředků a články pro další informace o nastavení podmíněného přístupu pro vaši organizaci.

### <a name="multi-factor-authentication-and-location-policies"></a>Zásady vícefaktorového ověřování a umístění
* [Začínáme s podmíněným přístupem k Azure AD připojené aplikace založené na skupině, umístění a zásady vícefaktorového ověřování](active-directory-conditional-access-azuread-connected-apps.md)
* [Aplikace a prohlížeče, které jsou podporovány](active-directory-conditional-access-supported-apps.md)

### <a name="device-based-conditional-access"></a>Podmíněný přístup využívající zařízení
* [Nastavit zásady podmíněného přístupu na základě zařízení pro řízení přístupu k aplikacím připojená k Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md)
* [Nastavit automatické registrace Windows připojená k doméně se službou Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
* [Řešení potíží se potíže s přístupem k Azure Active Directory](active-directory-conditional-access-device-remediation.md)
* [Pomoc při ochraně dat na ztracených nebo odcizených zařízeních pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### <a name="protect-resources-based-on-sign-in-risk"></a>Chránit prostředky v závislosti na riziko přihlášení
* [Azure AD identity protection](active-directory-identityprotection.md)

### <a name="next-steps"></a>Další kroky
* [Podmíněný přístup nejčastější dotazy](active-directory-conditional-faqs.md)
* [Technické referenční informace](active-directory-conditional-access-technical-reference.md)

