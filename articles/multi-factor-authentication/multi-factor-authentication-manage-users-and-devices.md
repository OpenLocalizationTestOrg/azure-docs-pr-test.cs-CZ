---
title: "aaaAdmins správu uživatelů a zařízení – Azure MFA | Microsoft Docs"
description: "Popisuje, jak toochange uživatelská nastavení, jako je například vynucení hello uživatelé toodo hello výš proces znovu."
documentationcenter: 
services: multi-factor-authentication
author: kgremban
manager: femila
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 8c35435e4f6504014d9a4f6f1b837639c3b53481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-hello-cloud"></a>Spravovat uživatelská nastavení s Azure Multi-Factor Authentication v cloudu hello
Jako správce můžete spravovat hello následující nastavení uživatelů a zařízení:

* Vyžadovat způsobů kontaktování uživatele tooprovide znovu
* Odstranit hesla aplikací
* Na všechny důvěryhodné zařízení vyžadovat vícefaktorové ověřování 

## <a name="require-users-tooprovide-contact-methods-again"></a>Vyžadovat způsobů kontaktování uživatele tooprovide znovu
Toto nastavení vynutí hello uživatele toocomplete hello registraci znovu. Neprohlížečové aplikace dál toowork, pokud má uživatel hello hesla aplikací pro ně.  Hesla aplikace hello uživatelů můžete odstranit také výběrem **odstranit všechna stávající hesla aplikací vytvořená hello vybrané uživateli**.

### <a name="how-toorequire-users-tooprovide-contact-methods-again"></a>Jak uživatelé tooprovide toorequire znovu kontaktovat metody
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na levé straně hello vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**. Otevře se stránka služby Multi-Factor authentication Hello. 
4. Zkontrolujte hello pole Další toohello uživatele nebo uživatele, že si přejete toomanage. Na pravém hello zobrazí seznam možností rychlý krok. 
5. Vyberte **spravovat uživatelská nastavení**.
6. Zaškrtněte políčko hello pro **vyžadují Vybraní uživatelé tooprovide způsobů kontaktu znovu**.
   ![Zadali způsoby kontaktování](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
7. Klikněte na **Uložit**.
8. Klikněte na tlačítko **zavřete**.

## <a name="delete-users-existing-app-passwords"></a>Odstranit uživatele existující hesla aplikací
Tato nastavení odstraní všechna hesla aplikací hello, která uživatel vytvoří. Neprohlížečové aplikace, které byly přidruženy tato hesla aplikace přestat pracovat teprve po vytvoření nové heslo aplikace.

### <a name="how-toodelete-users-existing-app-passwords"></a>Jak uživatelé toodelete existující hesla aplikací
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na levé straně hello vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**. Otevře se stránka služby Multi-Factor authentication Hello. 
6. Zkontrolujte hello pole Další toohello uživatele nebo uživatele, že si přejete toomanage. Na pravém hello zobrazí seznam možností rychlý krok. 
7. Vyberte **spravovat uživatelská nastavení**.
8. Zaškrtněte políčko hello pro **odstranit všechna stávající hesla aplikací vytvořená hello vybrané uživateli**.
   ![Odstranit hesla aplikací](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. Klikněte na **Uložit**.
10. Klikněte na tlačítko **zavřete**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Obnovit vícefaktorové ověřování u všech zapamatovaných zařízení pro uživatele
Jedna z funkcí konfigurovat hello služby Azure Multi-Factor Authentication je udělení uživatelům hello možnost toomark zařízení jako důvěryhodný. Další informace najdete v tématu [nastavení konfigurace Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Uživatelé mohou vyjádření výslovného nesouhlasu dvoustupňové ověřování pro konfigurovat počet dní na svých zařízeních regulární. Pokud dojde k ohrožení bezpečnosti účtu nebo o důvěryhodné zařízení dojde ke ztrátě, třeba toobe možné tooremove hello důvěryhodného stavu a vyžadují dvoustupňové ověření znovu.

Hello **obnovení služby Multi-Factor authentication u všech zapamatovaných zařízení** nastavení znamená, že hello uživatel se bude němuž tooperform dvoustupňové ověření hello příštím přihlášení, bez ohledu na to, jestli zvolili toomark jejich zařízení jako důvěryhodný. 

### <a name="how-toorestore-mfa-on-all-suspended-devices-for-a-user"></a>Jak toorestore MFA na všechny pozastavené zařízení pro uživatele
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na levé straně hello vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**. Otevře se stránka služby Multi-Factor authentication Hello. 
6. Zkontrolujte hello pole Další toohello uživatele nebo uživatele, že si přejete toomanage. Na pravém hello zobrazí seznam možností rychlý krok. 
7. Vyberte **spravovat uživatelská nastavení**.
8. Zaškrtněte políčko hello pro **obnovení služby Multi-Factor authentication u všech zapamatovaných zařízení**
   ![odstranit hesla aplikací](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klikněte na **Uložit**.
10. Klikněte na tlačítko **zavřete**.

## <a name="next-steps"></a>Další kroky

- Získat další informace o příliš[nastavení konfigurace Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md)

- Pokud budou uživatelé potřebovat pomoc, je nasměrujte směrem hello [uživatelská příručka pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user.md)
