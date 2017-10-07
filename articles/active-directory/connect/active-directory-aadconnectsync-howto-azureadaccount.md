---
title: "Synchronizace Azure AD Connect: jak účet služby toomanage hello Azure AD | Microsoft Docs"
description: "Toto téma popisuje, jak účet služby toorestore hello Azure AD."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, jak tooreset hello heslo pro hello synchronizace Azure AD Connect účet služby konektoru"
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e563518eae173de42a1d40bb5a76e63f29f9da42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomanage-hello-azure-ad-service-account"></a>Synchronizace Azure AD Connect: jak účet služby toomanage hello Azure AD
účet služby Hello používá hello konektoru služby Azure AD by měla služba toobe volné. Pokud potřebujete tooreset svoje přihlašovací údaje, toto téma je pro vás. Například pokud má globálního správce tak, že resetovat hello heslo u účtu služby hello pomocí prostředí PowerShell.

## <a name="reset-hello-credentials"></a>Resetovat přihlašovací údaje hello
Pokud účet služby hello definované na hello Azure AD Connector nemůže kontaktovat Azure AD kvůli tooauthentication problémy, můžete resetovat heslo hello.

1. Přihlaste se toohello server synchronizace Azure AD Connect a spusťte prostředí PowerShell.
2. Spusťte `Add-ADSyncAADServiceAccount`.  
   ![Addadsyncaadserviceaccount rutiny prostředí PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Zadejte přihlašovací údaje Azure AD globálního správce.

Tato rutina vytvoří hello heslo pro účet služby hello a aktualizovat ji ve službě Azure AD i v hello synchronizační modul.

## <a name="known-issues-these-steps-can-solve"></a>Známé problémy těchto kroků můžete vyřešit.
Tato část se seznam chyb ohlášených zákazníky, které byly odstraněny přihlašovací údaje resetování na hello účet služby Azure AD.

- - -
Událost 6900  
Hello server došlo k neočekávané chybě během zpracování oznámení o změně hesla:  
AADSTS70002: Chyba ověřuje přihlašovací údaje. AADSTS50054: Staré heslo se používá k ověřování.

- - -
Událost 659  
Chyba při načítání konfiguraci synchronizace hesel zásad. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Chyba ověřuje přihlašovací údaje. AADSTS50054: Staré heslo se používá k ověřování.

## <a name="next-steps"></a>Další kroky
**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

