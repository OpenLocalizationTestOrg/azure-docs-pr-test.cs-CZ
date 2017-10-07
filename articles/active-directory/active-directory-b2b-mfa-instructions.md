---
title: "aaaConditional přístup pro uživatele spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B podporuje vícefaktorové ověřování (MFA) pro podnikové aplikace tooyour výběrový přístup"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 3a05be4393f74ff8e87f32432a222a5fbac9af62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>Podmíněný přístup pro uživatele spolupráce B2B

## <a name="multi-factor-authentication-for-b2b-users"></a>Víceúrovňové ověřování pro uživatele s B2B
S spolupráce Azure AD B2B organizace můžete vynutit zásady vícefaktorového ověřování (MFA) pro uživatele B2B. Tyto zásady je možné vynutit na hello klienta, aplikace nebo na úrovni jednotlivých uživatelských hello stejným způsobem, že jsou povoleny pro zaměstnance na plný úvazek a členy hello organizace. Zásady vícefaktorového ověřování se vynucují v organizaci poskytující prostředky hello.

Příklad:
1. Správce nebo informace pracovního procesu ve společnosti A vyzývá uživatele z aplikace společnosti B tooan *Foo* ve společnosti A.
2. Aplikace *Foo* ve společnosti A je nakonfigurovaný toorequire MFA na přístup.
3. Při pokusu uživatele hello od společnosti B tooaccess aplikace *Foo* ve společnosti hello klienta, budou vyzváni toocomplete výzvu vícefaktorového ověřování.
4. uživatel Hello můžete nastavit jejich MFA s společnosti A a vybere možnost jejich vícefaktorového ověřování.
5. Tento scénář se dá použít pro všechny identity (Azure AD nebo účet spravované služby, například, pokud uživatelé ve společnosti B ověření pomocí sociálních ID)
6. Společnost A musí mít dostatek licencí Azure AD Premium, které podporují vícefaktorového ověřování. Hello uživatel ze společnosti B spotřebuje tuto licenci od společnosti A.

Hello pozváním klientů je zodpovědná za vícefaktorového ověřování pro uživatele z hello partnerské organizaci, vždy i v případě, že organizace partnera hello má možnosti vícefaktorového ověřování.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>Nastavení vícefaktorového ověřování pro uživatele spolupráce B2B
toodiscover jak snadné je tooset vícefaktorového ověřování pro službu pro uživatele spolupráce B2B, najdete v části Jak v hello následující video:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>MFA prostředí pro uživatele B2B nabízejí uplatnění kódu.
Podívejte se na následující animace toosee hello uplatnění prostředí hello:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>MFA resetovat pro uživatele spolupráce B2B
V současné době Dobrý den, Správce může vyžadovat tooproof uživatelé spolupráce B2B až znovu pouze pomocí hello následující rutiny prostředí PowerShell:

1. Připojit tooAzure AD

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. Získání všech uživatelů s potvrzením až metody

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Zde naleznete příklad:

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Reset – metoda MFA hello konkrétního uživatele toorequire hello B2B spolupráce uživatele tooset výš metod znovu. Příklad:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-hello-resource-tenancy"></a>Proč jsme provedu MFA v hello prostředků klientů?

V aktuální verzi hello MFA je vždy v hello prostředků klientů, z důvodů předvídatelnost. Předpokládejme například že pozvané tooFabrikam je uživatel Contoso (Jan) a Fabrikam se povolit MFA pro uživatele B2B.

Pokud společnosti Contoso má povolenou zásadu vícefaktorového ověřování pro App1, ale není na počítači App2, pak pokud se podíváme na hello deklarace identity společnosti Contoso MFA v tokenu hello jsme může se zobrazit hello následující potíže:

* Den 1: Uživatel má MFA ve společnosti Contoso a přistupuje k počítači App1 a pak žádné další MFA řádku se zobrazí ve firmě Fabrikam.

* Den 2: hello přístup uživatelů k aplikaci 2 ve společnosti Contoso, takže teď při přístupu k Fabrikam, se musí v MFA zaregistrovat existuje.

Tento proces může být matoucí a může vést toodrop v dokončených přihlášení.

Kromě toho i v případě, že Contoso má schopnost MFA, jeho není vždy hello případu hello Fabrikam by důvěřovat hello zásad vícefaktorového ověřování Contoso.

Nakonec prostředků klienta MFA funguje taky pro účty spravované služby a sociálních ID a orgs partnera, které nemají MFA nastavit.

Hello doporučení pro MFA pro uživatele B2B je proto, že tooalways vyžadovat vícefaktorové ověřování v hello pozvání klienta. Tento požadavek, mohla způsobit toodouble vícefaktorového ověřování v některých případech, ale vždy, když přistupují k hello pozváním klienta, je předvídatelný hello koncovým uživatelům prostředí: Jan musí v MFA zaregistrovat s pozváním klienta hello.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>Na zařízení, na základě polohy a na základě riziko podmíněného přístupu pro uživatele s B2B

Pokud Contoso povolí zásady podmíněného přístupu na základě zařízení pro svoje firemní data, je přístup zabránila zařízení, která se nespravují ve společnosti Contoso a nejsou kompatibilní s zásady zařízení Contoso hello.

Pokud uživatel hello B2B zařízení nespravuje ve společnosti Contoso, zablokuje se přístup B2B uživatelů z partnerských organizací hello v jakémkoli kontextu tyto zásady se vynucují. Contoso však můžete vytvořit vyloučení seznamy obsahující konkrétní partnerskou uživatelé tooexclude je z hello zásady podmíněného přístupu podle zařízení.

#### <a name="location-based-conditional-access-for-b2b"></a>Na základě polohy podmíněný přístup pro B2B

Zásady podmíněného přístupu na základě umístění můžete vynutit pro B2B uživatele, pokud hello pozváním organizace je možné toocreate důvěryhodné rozsah IP adres, který definuje jejich organizace partnera.

#### <a name="risk-based-conditional-access-for-b2b"></a>Podmíněný přístup využívající riziko pro B2B

V současné době přihlášení zásad založených na riziko nemůže být použité tooB2B uživatele, protože vyhodnocení rizik hello se provádí na domovskou organizaci hello B2B uživatele.

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [elementy Hello hello e-mailová pozvánka pro spolupráci B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
