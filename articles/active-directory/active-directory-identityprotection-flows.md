---
title: "aaaSign v prostředí s Azure AD Identity Protection | Microsoft Docs"
description: "Obsahuje přehled hello uživatelské prostředí, když má omezeny nebo opraven uživatele nebo když je službu Multi-Factor authentication požadované zásady ochrany identit."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: fbdca5b86ed93d0a2f2b6df1dd0150da9c0c85c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Možnosti přihlášení s Azure AD Identity Protection
S Azure Active Directory Identity Protection můžete:

* vyžadovat tooregister uživatele u služby Multi-Factor authentication
* zpracování rizikové přihlášení a ohrožení zabezpečení uživatelů

vzhledem k tomu, že právě přímo přihlášení zadáním uživatelského jména a hesla nebude možné už Hello odpověď problémy toothese hello systému má vliv na uživatele přihlašování uživatelů. Další kroky jsou požadované tooget uživatele bezpečně zpět do firmy.

Toto téma poskytuje přehled možností přihlašování uživatele pro všechny případy, které se můžou vyskytnout.

**Multi-Factor Authentication**

* Registrace služby Multi-Factor authentication

**Přihlášení v ohrožení**

* Obnovení rizikové přihlášení
* Rizikové přihlášení blokován
* Registrace služby Multi-Factor authentication během rizikové přihlášení

**Uživatel v ohrožení**

* Obnovení ohrožení bezpečnosti účtu
* Ohrožení bezpečnosti účtu blokován

## <a name="multi-factor-authentication-registration"></a>Registrace služby Multi-Factor authentication
Dobrý den nejlepší uživatelské prostředí pro obě, hello tok obnovení ohrožení bezpečnosti účtu a hello rizikové toku přihlášení, je, když uživatel hello můžete samoobslužné obnovení. Pokud jsou uživatelé zaregistrovaní pro službu Multi-Factor authentication, už mají telefonní číslo přidružené ke svému účtu, který lze použít toopass výzvy zabezpečení. Žádné pomoc HelpDesk nebo správce zapojení je potřebné toorecover před ohrožením účet. Proto má vysokou doporučuje tooget uživatelům zaregistrovat u služby Multi-Factor authentication. 

Správci můžou:

* Nastavte zásady, které vyžaduje další bezpečnostní ověření uživatelé tooset svých účtů. 
* Povolit přeskočení registrace služby Multi-Factor authentication pro až too30 dnů v případě, že chtějí uživatelé toogive období odkladu před registrací.

**registrace služby Multi-Factor authentication Hello má tři kroky:**

1. V prvním kroku hello hello uživatel získá oznámení o hello požadavek tooset hello účtu službu Multi-Factor authentication. 
   
    ![Náprava](./media/active-directory-identityprotection-flows/140.png "nápravy")
2. Služba Multi-Factor authentication tooset nahoru, je nutné toolet hello systému vědět, jak chcete toobe kontaktovat.
   
    ![Náprava](./media/active-directory-identityprotection-flows/141.png "nápravy")
3. Hello systému odesílá výzva tooyou a je třeba toorespond.
   
    ![Náprava](./media/active-directory-identityprotection-flows/142.png "nápravy")

## <a name="risky-sign-in-recovery"></a>Obnovení rizikové přihlášení
Jestliže správce konfiguroval zásady pro přihlášení rizika, hello vliv na uživatele upozorněni snaží toosign-in. 

**Hello rizikové přihlášení toku má dva kroky:** 

1. uživatel Hello je informován něco neobvyklého zjistilo o jejich přihlášení, jako je například přihlašujete z nového místa, zařízení nebo aplikace. 
   
    ![Náprava](./media/active-directory-identityprotection-flows/120.png "nápravy")
2. uživatel Hello je požadovaná tooprove svou identitu řešení zabezpečení výzvu. Pokud je uživatel hello zaregistrován u služby Multi-Factor authentication potřebují tooround cestě zabezpečení kód tootheir telefonní číslo. Vzhledem k tomu, že toto je jenom rizikové přihlášení a ohrožení bezpečnosti účtu, hello uživatel nebude mít toochange hello heslo v tomto toku. 
   
    ![Náprava](./media/active-directory-identityprotection-flows/121.png "nápravy")

## <a name="risky-sign-in-blocked"></a>Rizikové přihlášení blokován
Můžete také správci tooset riziko přihlášení uživatelů tooblock zásady při přihlašování v závislosti na úroveň rizika hello. tooget odblokováno, koncoví uživatelé musí obraťte se na správce nebo HelpDesk, nebo se mohou zkuste se přihlásit ze známé umístění nebo zařízení. Samoobslužné obnovení pomocí řešení služby Multi-Factor authentication není možné v tomto případě.

![Náprava](./media/active-directory-identityprotection-flows/200.png "nápravy")

## <a name="compromised-account-recovery"></a>Obnovení ohrožení bezpečnosti účtu
Když je nakonfigurován uživatelských zásad zabezpečení riziko, uživatele, kteří splní hello uživatele riziko úroveň určená v zásadách hello (a proto se předpokládá, že dojde k ohrožení) musí projít tok obnovení ohrožení hello uživatele předtím, než se můžete přihlásit. 

**tok obnovení ohrožení Hello uživatel má tři kroky:**

1. uživatel Hello je informován, že jejich zabezpečení účtu je ohrožena kvůli podezřelé aktivity nebo úniku přihlašovacích údajů.
   
    ![Náprava](./media/active-directory-identityprotection-flows/101.png "nápravy")
2. uživatel Hello je požadovaná tooprove svou identitu řešení zabezpečení výzvu. Pokud je uživatel hello zaregistrován u služby Multi-Factor authentication může samoobslužné obnovení před ohrožením.. Potřebují tooround cestě zabezpečení kód tootheir telefonní číslo. 
   
   ![Náprava](./media/active-directory-identityprotection-flows/110.png "nápravy")
3. Nakonec hello uživatele je vynucené toochange své heslo, protože někdo jiný měl účet pro přístup k tootheir. 
   Níže jsou snímky obrazovky toto prostředí.
   
   ![Náprava](./media/active-directory-identityprotection-flows/111.png "nápravy")

## <a name="compromised-account-blocked"></a>Ohrožení bezpečnosti účtu blokován
tooget uživatel, který byl zablokován pomocí zásad zabezpečení riziko uživatele odblokováno, hello uživatele obraťte se na správce nebo odbornou pomoc. Samoobslužné obnovení pomocí řešení služby Multi-Factor authentication není možné v tomto případě.

![Náprava](./media/active-directory-identityprotection-flows/104.png "nápravy")

## <a name="reset-password"></a>Resetování hesla
Pokud jsou ohrožené uživatelé blokováni v přihlášení, Správce může generovat dočasné heslo pro ně. Hello uživatelé budou mít toochange své heslo při příštím přihlášení.

![Náprava](./media/active-directory-identityprotection-flows/160.png "nápravy")

## <a name="see-also"></a>Viz také
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md) 

