---
title: "aaaConditional přístup v hello portál Azure classic | Microsoft Docs"
description: "Použití podmíněného řízení přístupu v hello Azure classic portálu toocheck za určitých podmínek při ověřování pro přístup k tooapplications."
services: active-directory
keywords: "podmíněný přístup tooapps, podmíněného přístupu s Azure AD, zabezpečení přístupu k prostředkům toocompany, zásady podmíněného přístupu"
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
ms.openlocfilehash: 049bd3f6ec9e35479319ee7608b007b9a1e16647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-in-hello-azure-classic-portal"></a>Podmíněný přístup v hello portál Azure classic

Toto téma se věnuje podmíněného přístupu v hello portál Azure classic. Hello nejnovější informace o podmíněném přístupu v hello Azure Active Directory najdete v tématu [podmíněného přístupu v Azure Active Directory](active-directory-conditional-access-azure-portal.md).


Hello funkcí řízení podmíněného přístupu Azure Active Directory (Azure AD) nabízejí jednoduchý způsoby toohelp zabezpečeným prostředkům v cloudu hello a místně. Zásady podmíněného přístupu, jako je vícefaktorové ověřování můžete ochránit proti hello riziko krádeže a phished pověření. Další zásady podmíněného přístupu můžete pomoct chránit data vaší organizace. Například kromě toorequiring pověření, bude pravděpodobně zásadu této jenom zařízení, která jsou registrovaná v systému pro správu mobilních zařízení, jako je Microsoft Intune můžete získat přístup ke službám citlivé vaší organizace.

## <a name="prerequisites"></a>Požadavky
Podmíněný přístup pro Azure AD je funkce [Azure Active Directory Premium](http://www.microsoft.com/identity). Každý uživatel, který přistupuje k aplikaci, která má uplatnit zásady podmíněného přístupu musí mít licenci na Azure AD Premium. Další informace o licenční požadavky v [zprávu nelicencovaná uživatele](https://aka.ms/utc5ix).

## <a name="how-is-conditional-access-control-enforced"></a>Jak se vynucuje řízení podmíněného přístupu?
Pomocí podmíněného řízení přístupu na místě Azure AD zjišťuje hello konkrétní podmínky, že nastavíte pro tooaccess uživatele aplikace. Po splnění požadavků na přístup, hello uživatel je ověřován a k hello aplikaci přístup.  

![Přehled podmíněného přístupu](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Podmínky
Toto jsou podmínky, které můžete zahrnout do zásad podmíněného přístupu:

* **Členství ve skupinách**. Řízení přístupu uživatele na základě členství ve skupině.
* **Umístění**. Použijte hello umístění hello uživatele tootrigger služby Multi-Factor authentication a použití ovládacích prvků bloku, pokud uživatel není v důvěryhodné síti.
* **Platforma**. Použijte hello platformu zařízení, jako je iOS, Android, Windows Mobile nebo Windows, jako podmínku pro použití zásad.
* **Povolit zařízení**. Stav zařízení, zda povolit nebo zakázat, je ověřen během hodnocení zásad zařízení. Pokud zakážete ztracené nebo odcizené zařízení v adresáři hello, můžete nadále nesplňují požadavky zásad.
* **Přihlášení a uživatele riziko**. Můžete použít [Azure AD Identity Protection](active-directory-identityprotection.md) pro zásady podmíněného přístupu riziko. Zásady podmíněného přístupu rizik pomůže poskytnout vaší organizaci zálohy ochrany na základě rizikových událostech a neobvyklé přihlašovací aktivity.

## <a name="controls"></a>Ovládací prvky
Toto jsou ovládací prvky, které můžete použít tooenforce zásady podmíněného přístupu:

* **Služba Multi-Factor authentication**. Může vyžadovat silné ověřování pomocí služby Multi-Factor authentication. Vícefaktorové ověřování můžete použít s Azure Multi-Factor Authentication nebo pomocí poskytovatele služby Multi-Factor authentication na místě v kombinaci s Active Directory Federation Services (AD FS). Pomocí služby Multi-Factor authentication pomáhá chránit prostředky z přistupuje neoprávněný uživatel, který může mít získal přístup toohello přihlašovací údaje platného uživatele.
* **Blok**. Můžete použít podmínky jako přístup uživatelů tooblock umístění uživatele. Například může blokovat přístup, když uživatel není v důvěryhodné síti.
* **Kompatibilní zařízení**. Můžete nastavit zásady podmíněného přístupu na úrovni zařízení hello. Zásady můžou nastavit tak, aby pouze počítače, které jsou připojené k doméně, nebo mobilní zařízení, která jsou zaregistrovaná pomocí aplikace pro správu mobilních zařízení, můžete přístup k prostředkům vaší organizace. Můžete například použít dodržování předpisů Intune toocheck zařízení a potom nahlásit ho tooAzure AD pro vynucení při pokusu uživatele hello tooaccess aplikace. Podrobné informace o tom, jak toouse Intune tooprotect aplikace a data, najdete v části [ochranu dat a aplikací pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Ochrana dat tooenforce Intune můžete použít také pro ztracené nebo odcizené zařízení. Další informace najdete v tématu [Chraňte svá data s využitím plného nebo selektivního vymazání pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Aplikace
Můžete vynutit zásady podmíněného přístupu na úrovni aplikace hello. Nastavení úrovně přístupu pro aplikace a služby v hello cloudu nebo místně. zásady Hello jsou použité přímo toohello webu nebo službě. Hello zásady se vynucují pro přístup k toohello prohlížeče a tooapplications, který přístup k službě hello.

## <a name="device-based-conditional-access"></a>Podmíněný přístup využívající zařízení
Můžete omezit přístup tooapplications ze zařízení, které jsou registrovány s Azure AD a které splňují určité podmínky. Podmíněného přístupu na základě zařízení chrání od uživatelů, kteří se pokusí tooaccess hello prostředky z prostředků v organizaci:

* Neznámá nebo nespravovaného zařízení.
* Zařízení, která nesplňují zásady zabezpečení hello vaše organizace nastavit.

Můžete nastavit zásady v závislosti na tyto požadavky:

* **Připojená k doméně**. Nastavte toodevices přístup zásad toorestrict, které jsou připojené k tooan místní služby Active Directory domény a která jsou zaregistrovaná taky s Azure AD. Tato zásada platí tooWindows stolních počítačů, laptopů a tablety enterprise.
  Další informace o tom, tooset automatické registrace zařízení připojených k doméně se službou Azure AD, najdete v části [nastavit automatické registrace zařízení připojených k doméně systému Windows s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
* **Kompatibilní zařízení**. Nastavení zásad přístupu toodevices toorestrict, které jsou označeny jako **kompatibilní** v adresáři systému správy hello. Tato zásada zajistí, že jsou přístup povolený jenom zařízení, které splňují zásady zabezpečení, jako je například vynucení šifrování souborů na zařízení. Můžete použít tento přístup toorestrict zásad z hello následující zařízení:
  
  * **Zařízení se systémem Windows připojených k doméně**. Spravované nástrojem System Center Configuration Manager (v aktuální větvi hello) nasazené v hybridní konfigurace.
  * **Windows 10 Mobile pracovní nebo osobní zařízení**. Spravovat pomocí Intune nebo systém správy podporovaných mobilních zařízení třetích stran.
  * **iOS a zařízení se systémem Android**. Spravovat přes Intune.

Uživatelé, kteří přístup k aplikacím, které jsou chráněny na zařízení, certifikační autorita zásad potřebuje přístup hello aplikace ze zařízení, které splňuje požadavky na tuto zásadu. Pokud pokus byl odepřen přístup na zařízení, který nesplňuje požadavky zásad.

Informace o tom, jak tooconfigure na zařízení, certifikační autorita zásad ve službě Azure AD, najdete v části [nastavit zásady podmíněného přístupu na základě zařízení připojená k Azure Active Directory aplikací](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Zdroje
V tématu hello následující prostředků kategorií a články toolearn informace o nastavení podmíněného přístupu pro vaši organizaci.

### <a name="multi-factor-authentication-and-location-policies"></a>Zásady vícefaktorového ověřování a umístění
* [Začínáme s aplikací AD připojení společnosti podmíněného přístupu tooAzure podle skupiny, umístění a zásady vícefaktorového ověřování](active-directory-conditional-access-azuread-connected-apps.md)
* [Aplikace a prohlížeče, které jsou podporovány](active-directory-conditional-access-supported-apps.md)

### <a name="device-based-conditional-access"></a>Podmíněný přístup využívající zařízení
* [Nastavit zásady podmíněného přístupu na základě zařízení pro přístup k řízení tooAzure připojené služby Active Directory aplikace](active-directory-conditional-access-policy-connected-applications.md)
* [Nastavit automatické registrace Windows připojená k doméně se službou Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
* [Řešení potíží se potíže s přístupem k Azure Active Directory](active-directory-conditional-access-device-remediation.md)
* [Pomoc při ochraně dat na ztracených nebo odcizených zařízeních pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### <a name="protect-resources-based-on-sign-in-risk"></a>Chránit prostředky v závislosti na riziko přihlášení
* [Azure AD identity protection](active-directory-identityprotection.md)

### <a name="next-steps"></a>Další kroky
* [Podmíněný přístup nejčastější dotazy](active-directory-conditional-faqs.md)
* [Technické referenční informace](active-directory-conditional-access-technical-reference.md)

