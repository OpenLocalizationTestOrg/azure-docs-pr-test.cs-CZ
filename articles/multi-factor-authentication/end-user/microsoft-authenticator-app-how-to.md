---
title: "aaaMicrosoft ověřovací aplikací pro mobilní telefony | Microsoft Docs"
description: "Zjistěte, jak tooupgrade toohello nejnovější verzi Azure Authenticator."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: d895d92d89613cbafd9fc09d4ff4810cbf25652e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-microsoft-authenticator-app"></a>Začínáme s aplikací Microsoft Authenticator hello
Hello aplikaci Microsoft Authenticator poskytuje další úroveň zabezpečení v pracovní nebo školní účet (například bsimon@contoso.com) nebo účtu Microsoft (například bsimon@outlook.com).

aplikace Hello funguje v jednom ze dvou způsobů:

* **Oznámení**. aplikace Hello můžou pomoct zabránit neoprávněnému přístupu tooaccounts a zastavit podvodné transakce vynucením tablet nebo smartphone tooyour oznámení. Jednoduše zobrazení hello oznámení a pokud je oprávněné, vyberte **ověřte**. Jinak, můžete vybrat **Odepřít**. 
* **Ověřovací kód**. Hello aplikace můžete použít jako softwaru token toogenerate OAuth ověřovací kód. Po zadání uživatelského jména a hesla, jste zadali kód hello poskytované aplikace hello do přihlašovací obrazovky hello. ověřovací kód Hello poskytuje druhou podobu ověřování.

aplikace Microsoft Authenticator Hello nahrazuje aplikaci Azure Authenticator hello. aplikaci Azure Authenticator Hello pořád funguje, ale pokud se rozhodnete nové aplikace Microsoft Authenticator toomove toohello, tento článek vám může pomoci.  

## <a name="opt-in-for-two-step-verification"></a>Vyjádřit výslovný souhlas pro dvoustupňové ověření

aplikace Microsoft Authenticator Hello nefunguje samostatně. Konfigurovat tooprompt vaše účty pro druhý metoda ověření po můžete přihlásit pomocí uživatelského jména a hesla. 

Pro pracovní nebo školní účet neobdržíte obvykle toochoose této funkce si sami. Místo toho správce zabezpečení vyjádřit výslovný souhlas vaším jménem a upozorní vás tooregister metody ověření pro váš účet. Pokud tento scénář se vztahuje tooyou, přečtěte si informace v [co Azure Multi-Factor Authentication znamená pro mě nejlepší](multi-factor-authentication-end-user.md).

Pro osobní účet budete potřebovat tooset si dvoustupňové ověřování pro sami. Pokud máte účet Microsoft, tyto kroky jsou k dispozici v [o dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification). 

Můžete taky hello Microsoft Authenticator s účty jiných společností než Microsoft. Funkce hello se volání něco jiného než dvoustupňové ověření, ale musí být schopný toofind ji v části Nastavení zabezpečení nebo přihlášení. 

## <a name="install-hello-app"></a>Instalace aplikace hello
je k dispozici pro aplikaci Microsoft Authenticator Hello [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-toohello-app"></a>Přidat aplikaci toohello účty
U každého účtu, které chcete aplikaci Microsoft Authenticator toohello tooadd použijte jednu z následujících postupů hello:

### <a name="add-a-personal-microsoft-account-toohello-app"></a>Přidat aplikaci toohello osobní účet Microsoft

Pro osobní účet Microsoft (jeden použít toosign v tooOutlook.com, Xbox, Skype, apod), všechny máte toodo je přihlášení tooyour účtu v aplikaci Microsoft Authenticator hello.

### <a name="add-a-work-or-school-account-toohello-app-using-hello-qr-code-scanner"></a>Přidat pracovní nebo školní účet toohello aplikace pomocí skener kód QR hello
1. Obrazovka nastavení ověření zabezpečení toohello přejděte.  Informace o tom tooget toothis obrazovky, najdete v části [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Zaškrtněte políčko hello vedle příliš**ověřovací aplikaci** vyberte **konfigurace**.

    ![tlačítko Konfigurovat Hello na obrazovku nastavení ověření zabezpečení hello](./media/authenticator-app-how-to/azureauthe.png)

    Otevře obrazovky s kód QR na něm.

    ![Obrazovka, která poskytuje kód QR hello](./media/authenticator-app-how-to/barcode2.png)
3. Aplikace Microsoft Authenticator otevřete hello. Na hello **účty** obrazovku, vyberte  **+** a pak určete, zda má tooadd pracovní nebo školní účet.
4. Použít hello fotoaparát tooscan hello QR kód a potom vyberte **provádí** tooclose hello QR kód obrazovky.

    Pokud svůj fotoaparát nepracuje správně, můžete [hello QR kód a adresu URL zadat ručně](#add-an-account-to-the-app-manually).

5. Pokud aplikace hello ukazuje název účtu s šestimístný kód pod ním, jste hotovi. 

    ![Účty obrazovky](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-manually"></a>Ručně přidejte účet toohello aplikace
1. Obrazovka nastavení ověření zabezpečení toohello přejděte.  Informace o tom tooget toothis obrazovky, najdete v části [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md).
2. Vyberte **konfigurace**.

    ![tlačítko Konfigurovat Hello na obrazovku nastavení ověření zabezpečení hello](./media/authenticator-app-how-to/azureauthe.png)

    Otevře obrazovky s kód QR na něm.  Všimněte si hello kód a adresu URL.

    ![Obrazovka, která poskytuje hello QR kód a adresu URL](./media/authenticator-app-how-to/barcode2.png)
3. Aplikace Microsoft Authenticator otevřete hello. Na hello **účty** obrazovku, vyberte  **+** a pak určete, zda má tooadd pracovní nebo školní účet.

4. V hello skener, vyberte **ručně zadejte kód**.

    ![Obrazovka pro skenování kód QR](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. Zadejte hello kód a adresu URL hello hello příslušných polí v aplikaci hello a potom vyberte **Dokončit**.

    ![Obrazovka pro zadání kód a adresu URL](./media/authenticator-app-how-to/manual.png)

6. Pokud aplikace hello ukazuje název účtu s šestimístný kód pod ním, jste hotovi.

    ![Účty obrazovky](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-using-touch-id"></a>Přidat účet toohello aplikaci pomocí Touch ID
aplikace Microsoft Authenticator Hello v systému iOS podporuje Touch ID.  Azure Multi-Factor Authentication umožňuje organizacím toorequire PIN kódu pro zařízení. Uživatelé systému iOS se Touch ID, nepotřebujete tooenter kód PIN. Místo toho můžete kontrolovat jejich otisk prstu a vybrat **schválit**.

Nastavení Touch ID s Microsoft Authenticator je jednoduché. Dokončení výzvy normální ověření pomocí kódu PIN. Pokud vaše zařízení podporuje Touch ID, Microsoft Authenticator nastaví ho automaticky pro tento účet.

![Ověření instalace Touch ID](./media/authenticator-app-how-to/touchid1.png)

Z tohoto bodu předání, když jste požadované tooverify vaše přihlášení, vyberte hello přijatých nabízených oznámení a kontrolovat otisk prstu místo zadávání svůj PIN kód.

![Nabízená oznámení](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-hello-app-when-you-sign-in"></a>Použití aplikace hello při přihlášení

Jakmile je váš účet je přidán toohello aplikace, může být výzvami toodo toomake ověřovací test, která je opravdu že všechno, co byla nakonfigurována správně. Potom je vše. Nepotřebujete toodo cokoliv jiného dokud hello příštím přihlášení.

Pokud jste zvolili toouse ověřovací kódy v hello aplikaci, můžete spustit toosee je na domovskou stránku hello. Se změní každých 30 sekund, takže máte vždy nový kód, když budete potřebovat. Ale nepotřebujete toodo něco s nimi dokud přihlásit a jsou výzvami tooenter ověřovací kód.  
