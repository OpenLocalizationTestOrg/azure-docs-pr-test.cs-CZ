---
title: "Azure AD Connect: Ověřování průchozí – inteligentní uzamčení | Microsoft Docs"
description: "Tento článek popisuje, jak předávací ověřování Azure Active Directory (Azure AD) chrání před útoky hrubou silou hesla v cloudu hello místních účtů."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: b02e315c3cc3eae00ca6408d735a416f34c2cdc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Předávací ověřování Azure Active Directory: Inteligentní uzamčení

## <a name="overview"></a>Přehled

Azure AD chrání před útoky hrubou silou heslo a originální uživatelům bránit v zamykají mimo jejich aplikace Office 365 a SaaS. Tato funkce volána **inteligentní uzamčení**, je podporováno, pokud používáte ověřování průchozí jako způsob přihlášení. Inteligentní uzamčení je ve výchozím nastavení povolena pro všechny klienty a chráníte uživatelské účty všech hello čas; neexistuje žádné tooturn nutné ho na.

Inteligentní uzamčení funguje udržovat přehled o neúspěšných pokusů o přihlášení a po určitou **prahové hodnoty počtu uzamčení**, počáteční **doba trvání uzamčení**. Všechny pokusů o přihlášení před útočníkem hello během hello doba trvání uzamčení odmítají. Pokud pokračuje hello útoku, hello následné neúspěšné pokusy o přihlášení po hello doba trvání uzamčení končí způsobí delší doby uzamčení.

>[!NOTE]
>Hello výchozí prahové hodnoty počtu uzamčení je 10 neúspěšných pokusů o přihlášení a hello výchozí doba trvání uzamčení je 60 sekund.

Inteligentní uzamčení také rozlišuje přihlášení z originálního uživatelů a útočníci a pouze zámky se útočníci hello ve většině případů. Tato funkce zabrání útočníkům neoprávněnému uzamčení uživatele originálního. Používáme po přihlášení chování uživatelů zařízení & prohlížečů a dalších toodistinguish signály mezi originální uživateli a útočníkům. Jsme jsou neustále zlepšování našich algoritmy.

Protože předávací ověřování předává žádosti o ověření hesla do vaší místní služby Active Directory (AD), je třeba tooprevent útočníci z uzamykání účty AD vašich uživatelů. Vzhledem k tomu, že máte vlastní zásady AD uzamčení účtu (konkrétně [ **prahovou hodnotu uzamknutí účtu** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) a [ **zásady resetování čítač účet uzamčení po** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), budete muset prahové hodnoty počtu uzamčení tooconfigure Azure AD a doba trvání uzamčení hodnoty správně toofilter out útoky v cloudu hello než dosáhnou místní AD.

>[!NOTE]
>Funkce uzamčení inteligentní Hello je zdarma a je _na_ ve výchozím nastavení pro všechny zákazníky. Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD však musí toohave alespoň jeden Azure AD Premium P2 licenci klienta. Nepotřebujete licenci Azure AD Premium P2 _na uživatele_ tooget hello inteligentní uzamčení funkce pomocí předávacího ověřování.

tooensure, aby vaši uživatelé místní účty AD se dobře chráněný, musíte tooensure který:

1.  Prahová hodnota pro uzamčení Azure AD je _méně_ než prahovou hodnotu uzamknutí účtu na AD. Doporučujeme nastavit hello hodnoty tak, aby na AD prahovou hodnotu uzamknutí účtu alespoň dvě nebo tři krát prahové hodnoty počtu uzamčení Azure AD.
2.  Doba trvání uzamčení Azure AD (vyjádřený v sekundách) je _delší_ než AD na resetovat účet uzamčení čítač po (vyjádřený v minutách).

## <a name="verify-your-ad-account-lockout-policies"></a>Zkontrolujte vaše zásady uzamčení účtu služby AD

Použijte následující pokyny tooverify hello zásad uzamčení účtu AD:

1.  Spusťte nástroj pro správu zásad skupiny hello.
2.  Upravte hello zásad skupiny, které je použité tooall uživatele, například hello výchozí zásada domény.
3.  Přejděte tooComputer Konfigurace uživatele\Zásady\Nastavení systému Windows\Nastavení zabezpečení\Zásady zamknutí uzamčení zásady.
4.  Ověřte hodnoty prahovou hodnotu uzamknutí účtu a resetování čítač účet uzamčení po.

![Zásady uzamčení účtu AD](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-hello-graph-api-toomanage-your-tenants-smart-lockout-values"></a>Použít hello rozhraní Graph API toomanage hodnoty inteligentní uzamčení vašeho klienta

>[!IMPORTANT]
>Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD je funkce Azure AD Premium P2. Je také potřebuje toobe jako globální správce v klientovi.

Můžete použít [grafu Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, nastavte a aktualizujte hodnoty inteligentní uzamčení Azure AD. Ale můžete také provést tyto operace prostřednictvím kódu programu.

### <a name="read-smart-lockout-values"></a>Inteligentní uzamčení pro čtení hodnoty

Postupujte podle těchto kroků tooread hodnoty inteligentní uzamčení vašeho klienta:

1. Průzkumník grafu se přihlaste jako globální správce tenanta. Pokud se zobrazí výzva, udělení přístupu pro hello požadovaná oprávnění.
2. Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All" hello.
3. Žádost o rozhraní Graph API hello nakonfigurovat následujícím způsobem: nastavit verzi příliš "BETA" typ požadavku příliš "GET" a adresa URL příliš`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Klikněte na tlačítko "Spustit dotaz" toosee hodnoty inteligentní uzamčení vašeho klienta. Pokud jste nenastavili před hodnoty vašeho klienta, zobrazí prázdnou sadou.

### <a name="set-smart-lockout-values"></a>Nastavení hodnot inteligentní uzamčení

Postupujte podle těchto kroků tooset hodnoty inteligentní uzamčení vašeho klienta (pro hello pouze poprvé):

1. Průzkumník grafu se přihlaste jako globální správce tenanta. Pokud se zobrazí výzva, udělení přístupu pro hello požadovaná oprávnění.
2. Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All" hello.
3. Žádost o rozhraní Graph API hello nakonfigurovat následujícím způsobem: nastavit verzi příliš "BETA" typ požadavku příliš "POST" a adresa URL příliš`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Zkopírujte a vložte následující požadavku JSON do pole "Žádost text" hello hello. Hello inteligentní uzamčení hodnoty podle potřeby změnit a použít náhodný identifikátor GUID pro `templateId`.
5. Klikněte na tlačítko "Spustit dotaz" tooset hodnoty inteligentní uzamčení vašeho klienta.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Pokud je nepoužíváte, můžete nechat hello **BannedPasswordList** a **EnableBannedPasswordCheck** hodnoty jako prázdný ("") a "false" v uvedeném pořadí.

Ověřte, že jste si nastavili hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Aktualizujte hodnoty inteligentní uzamčení

Postupujte podle těchto kroků tooupdate hodnoty inteligentní uzamčení vašeho klienta (Pokud jste již zadali, je před):

1. Průzkumník grafu se přihlaste jako globální správce tenanta. Pokud se zobrazí výzva, udělení přístupu pro hello požadovaná oprávnění.
2. Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All" hello.
3. [Postupujte podle těchto kroků tooread vašeho klienta inteligentní uzamčení hodnoty](#read-smart-lockout-values). Kopírování hello `id` hodnota (GUID) položky hello se zobrazovaným názvem"" jako "PasswordRuleSettings".
4. Hello rozhraní Graph API žádosti nakonfigurovat následujícím způsobem: nastavte verzi příliš "BETA" typ žádosti příliš "OPRAVIT" a adresy URL příliš`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -použít hello identifikátoru GUID z kroku 3 pro `<id>`.
5. Zkopírujte a vložte následující požadavku JSON do pole "Žádost text" hello hello. Hello inteligentní uzamčení hodnoty podle potřeby změnit.
6. Klikněte na tlačítko "Spustit dotaz" tooupdate hodnoty inteligentní uzamčení vašeho klienta.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Ověřte, že jste aktualizovali hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).

## <a name="next-steps"></a>Další kroky
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
