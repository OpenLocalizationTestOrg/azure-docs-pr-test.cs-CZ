---
title: "Synchronizace Azure AD Connect: Změna hesla účtu hello AD DS | Microsoft Docs"
description: "Tento dokument téma popisuje, jak změnit tooupdate Azure AD Connect po hello heslo účtu hello služby AD DS."
services: active-directory
keywords: "AD DS účet, heslo a účet služby Active Directory"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2707c9246446612f8d083ecde876b3b4fb2435ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-ad-ds-account-password"></a>Změna hesla účtu hello AD DS
Hello AD DS účtu odkazuje toohello uživatel účet používaný službou Azure AD Connect toocommunicate s místní služby Active Directory. Pokud změníte heslo hello hello účet služby AD DS, je třeba aktualizovat službu Azure AD Connect synchronizace s hello nové heslo. Jinak hello synchronizace mohou už správně synchronizovat s hello místní služby Active Directory a můžete narazit hello následujícím chybám:

* V hello Synchronization Service Manager, všechny import nebo export operaci s místní AD selže s **bez start pověření** chyby.

* V prohlížeči událostí systému Windows, protokolu událostí aplikace hello obsahuje chybu s **6000 ID události** a zpráva **'agenta pro správu hello "contoso.com" neúspěšné toorun, protože nebyly platné přihlašovací údaje hello'** .


## <a name="how-tooupdate-hello-synchronization-service-with-new-password-for-ad-ds-account"></a>Jak tooupdate hello synchronizační služba s nové heslo pro účet služby AD DS
tooupdate hello synchronizační služba s hello nové heslo:

1. Spusťte hello Synchronization Service Manager (START → synchronizace Service).
</br>![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Přejděte toohello **konektory** kartě.

3. Vyberte hello **AD Connector.** odpovídající účet toohello AD DS, pro kterou byl změněn jeho heslo.

4. V části **akce**, vyberte **vlastnosti**.

5. V místním dialogovém okně hello, vyberte **připojit doménové struktury Directory tooActive**:

6. Zadejte nové heslo účtu hello služby AD DS hello v hello **heslo** textové pole.

7. Klikněte na tlačítko **OK** toosave hello nové heslo a automaticky otevíraný dialog zavřít hello.

8. Restartování hello službu Azure AD Connect synchronizace v modulu snap-in Správce řízení služeb systému Windows. Toto je tooensure která žádné odkaz toohello staré heslo bude odebrána z mezipaměti paměti hello.

## <a name="next-steps"></a>Další kroky
**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)

* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
