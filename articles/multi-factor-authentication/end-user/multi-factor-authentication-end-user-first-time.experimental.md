---
title: "aaaSet si dvoustupňové ověřování pro pracovní nebo školní účet | Microsoft Docs"
description: "Pokud vaše společnost konfiguruje Azure Multi-Factor Authentication, bude výzvami toosign pro dvoustupňové ověření. Zjistěte, jak tooset ho nahoru. "
services: multi-factor-authentication
keywords: "jak toouse azure, active directory v cloudu hello kurz služby active directory"
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2d0348081eefa42c23de2047044688879dcb5966
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-my-account-for-two-step-verification"></a>Nastavit účtu pro dvoustupňové ověření
Dvoustupňové ověření je na další bezpečnostní krok, který pomáhá chránit váš účet tak, že pro jiné osoby toobreak v složitější. Pokud přečtení tohoto článku pravděpodobně tu e-mailu ze svého pracovního nebo školního správce o službě Multi-Factor Authentication. Nebo možná pokusil toosign v a zobrazí chybové hlášení s dotazem, tooset až dalšího ověření zabezpečení. Pokud je případ hello **nemůžete se přihlásit před dokončením procesu automatické registrace hello**.

Tento článek vám pomůže nastavit vaše **pracovní nebo školní účet**. Pokud chcete tooenable dvoustupňové ověřování pro vlastní, osobní účet Microsoft, najdete v části [o dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>Nastavení účtu

Pokud vaše IT oddělení vyžaduje, abyste toostart pomocí dvoustupňového ověření na obrazovce o tom, že **váš správce vyžaduje, abyste nastavili tento účet další bezpečnostní ověření** se zobrazí:

![Nastavení](./media/multi-factor-authentication-end-user-first-time/first.png)

Vyberte tooget spustili, **nyní nastavit.**

Pokud nevidíte obrazovky takto při přihlášení, postupujte podle pokynů hello v [spravovat nastavení pro dvoustupňové ověření](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) toofind hello nastavení stránky, kde můžete spravovat vaše možnosti ověření. 

## <a name="decide-how-you-want-tooverify-your-sign-ins"></a>Určuje, jakým tooverify vaše přihlášení

první otázku Hello v procesu registrace hello je, jak chcete, abychom toocontact můžete. Podívejte se na možnosti hello v tabulce hello a hello odkazy toogo toohello instalační kroky použijte pro každou metodu.

| Způsob kontaktu | Popis |
| --- | --- |
| [Mobilní aplikace](#use-a-mobile-app-as-the-contact-method) |- **Přijímejte oznámení pro ověření.** Tato možnost nabízených oznámení oznámení toohello ověřovací aplikaci na tablet nebo smartphone. Zobrazit oznámení hello a, pokud je oprávněné, vyberte **ověřit** v aplikaci hello. Vaše škola nebo pracoviště může vyžadovat zadání kódu PIN, než ověřování.<br>- **Použijte ověřovací kód.** V tomto režimu hello ověřovací aplikace generuje ověřovací kód, který aktualizuje každých 30 sekund. Zadejte aktuální ověřovací kód hello v hello přihlašovací rozhraní.<br>je k dispozici pro aplikaci Microsoft Authenticator Hello [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| [Volání na mobilní telefon nebo text.](#use-your-mobile-phone-as-the-contact-method) |- **Telefonní hovor** umístí automatický hlasový hovor toohello telefonní číslo je zadat. Odpovězte hello volání a stisknutím klávesy # v tooauthenticate klávesnici telefonu hello.<br>- **Textová zpráva** ukončí textovou zprávu s ověřovacím kódem. Následující hello řádku v textu hello odpovědět toohello textová zpráva nebo zadejte ověřovací kód hello zadané do hello přihlašovací rozhraní. |
| [Office telefonního hovoru](#use-your-office-phone-as-the-contact-method) |Umístí automatický hlasový hovor toohello telefonní číslo, které zadáte. Odpověď hello hovor a stiskem tlačítka # na tooauthenticate klávesnici telefonu hello. |

## <a name="use-a-mobile-app-as-hello-contact-method"></a>Použití mobilní aplikace jako způsob kontaktu hello
Pomocí této metody vyžaduje instalaci ověřovací aplikaci na telefonu nebo tabletu. Hello kroky v tomto článku jsou založeny na aplikaci Microsoft Authenticator hello, která je k dispozici pro [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Vyberte **mobilní aplikace** z rozevíracího seznamu hello.
2. Vyberte buď **dostávat oznámení pro ověření** nebo **použít ověřovací kód**, pak vyberte **nastavit**.

   ![Další bezpečnostní ověření obrazovky](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Na telefonu nebo tabletu, otevřete aplikaci hello a vyberte  **+**  tooadd účet. (Na zařízeních s Androidem, vyberte hello tři tečky, pak **přidejte účet**.)
4. Zadejte, že chcete tooadd pracovní nebo školní účet. Otevře se skener kód QR Hello na váš telefon. Pokud svůj fotoaparát nepracuje správně, můžete vybrat tooenter informací o společnosti ručně. Další informace najdete v tématu [ručně přidat účet](#add-an-account-manually).  
5. Naskenujte obrázek kódu QR hello, který se objevil s úvodní obrazovka pro konfiguraci mobilní aplikace hello.  Vyberte **provádí** tooclose hello QR kód obrazovky.  

   ![Obrazovka kód QR](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Po dokončení aktivace na telefonu hello vyberte **kontaktujte mi**.  Tento krok odešle oznámení nebo telefon tooyour ověřovací kód. Vyberte **ověřte**.  
7. Pokud vaše společnost vyžaduje kód PIN pro schválení ověření přihlášení, zadejte její název.

   ![Pole pro zadání kódu PIN](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. Po dokončení zadání kódu PIN, vyberte **Zavřít**. Ověření v tomto okamžiku by měla být úspěšné.
9. Doporučujeme, abyste zadali číslo svého mobilního telefonu v případě ztráty přístupu tooyour mobilní aplikace. Ve vaší zemi hello rozevíracího seznamu a zadejte své mobilní telefonní číslo v názvu další toohello země hello pole. Vyberte **Další**.
10. V tomto okamžiku jste výzvami tooset si hesla aplikací pro neprohlížečové aplikace, jako je například Outlook 2010 nebo starší nebo hello nativní e-mailové aplikace na zařízení Apple. Tato akce je vzhledem k tomu, že některé aplikace nepodporují dvoustupňové ověřování. Pokud nepoužijete těchto aplikací, klikněte na tlačítko **provádí** a přeskočit hello zbytek tyto kroky.
11. Pokud používáte tyto aplikace, heslo aplikace hello kopie zadaný a vložte jej do vaší aplikace místo regulární heslo. Můžete použít hello stejné heslo aplikace pro víc aplikací. Další informace najdete [pomoci s hesly aplikací].
12. Klikněte na **Done** (Hotovo).

### <a name="add-an-account-manually"></a>Ručně přidat účet
Pokud chcete tooadd k účtu toohello mobilní aplikaci ručně, místo použití čtečky hello QR, postupujte podle těchto kroků.

1. Vyberte hello **ručně zadejte účet** tlačítko.  
2. Zadejte hello kód a adresu URL hello, které jsou k dispozici na hello stejné stránce, která vám zobrazí hello čárový kód. Tyto informace je třeba do hello **kód** a **URL** políček na mobilní aplikace hello.

    ![Nastavení](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Po dokončení aktivace hello vyberte **kontaktujte mi**. Tento krok odešle oznámení nebo telefon tooyour ověřovací kód. Vyberte **ověřte**.

## <a name="use-your-mobile-phone-as-hello-contact-method"></a>Pomocí mobilního telefonu jako způsob kontaktu hello
1. Vyberte **telefon pro ověření** z rozevíracího seznamu hello.  

    ![Nastavení](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Zvolte vaší země z rozevíracího seznamu hello a zadejte své mobilní telefonní číslo.
3. Vyberte metodu hello si přejete toouse s mobilním telefonem - text nebo volání.
4. Vyberte **kontaktujte mi** tooverify vaše telefonní číslo. V závislosti na režimu hello, který jste vybrali nemůžeme vám pošleme text nebo zavolat. Postupujte podle pokynů hello na úvodní obrazovka a pak vyberte **ověřte**.
5. V tomto okamžiku jste výzvami tooset si hesla aplikací pro neprohlížečové aplikace, jako je například Outlook 2010 nebo starší nebo hello nativní e-mailové aplikace na zařízení Apple. Tato akce je vzhledem k tomu, že některé aplikace nepodporují dvoustupňové ověřování. Pokud nepoužijete těchto aplikací, klikněte na tlačítko **provádí** a přeskočit hello zbytek hello kroky.
6. Pokud používáte tyto aplikace, heslo aplikace hello kopie zadaný a vložte jej do vaší aplikace místo regulární heslo. Můžete použít hello stejné heslo aplikace pro víc aplikací. Další informace najdete [pomoci s hesly aplikací].
7. Klikněte na **Done** (Hotovo).

## <a name="use-your-office-phone-as-hello-contact-method"></a>Použijte váš telefon do kanceláře jako způsob kontaktu hello
1. Vyberte **Telefon do kanceláře** z rozevíracího seznamu hello  

    ![Nastavení](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Hello telefonní číslo pole je automaticky vyplněno své kontaktní informace společnosti. Pokud je číslo hello nesprávnou nebo chybějící, požádejte správce toomake změny.
3. Vyberte **kontaktujte mi** tooverify vaše telefonní číslo a my vám zavoláme vaše číslo. Postupujte podle pokynů hello na úvodní obrazovka a pak vyberte **ověřte**.
4. V tomto okamžiku jste výzvami tooset si hesla aplikací pro neprohlížečové aplikace, jako je například Outlook 2010 nebo starší nebo hello nativní e-mailové aplikace na zařízení Apple. Tato akce je vzhledem k tomu, že některé aplikace nepodporují dvoustupňové ověřování. Pokud nepoužijete těchto aplikací, klikněte na tlačítko **provádí** a přeskočit hello zbytek hello kroky.
5. Pokud používáte tyto aplikace, heslo aplikace hello kopie zadaný a vložte jej do vaší aplikace místo regulární heslo. Můžete použít hello stejné heslo aplikace pro víc aplikací. Další informace najdete v tématu [co jsou hesla aplikací](multi-factor-authentication-end-user-app-passwords.md).
6. Klikněte na **Done** (Hotovo).

## <a name="next-steps"></a>Další kroky
* Změňte upřednostňované možnosti a [spravovat nastavení pro dvoustupňové ověření](multi-factor-authentication-end-user-manage-settings.md)
* Nastavit [hesla aplikací](multi-factor-authentication-end-user-app-passwords.md) pro nativní zařízení s aplikací, které nepodporují dvoustupňové ověřování.
* Podívejte se na hello [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) fast, zabezpečení ověřování i v případě, že nemáte buňky služby.

