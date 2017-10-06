---
title: "aaaLog lístku podpory pro řady StorSimple 8000 | Microsoft Docs"
description: "Zjistěte, jak požádat o toocreate podporu a spustit relaci podpory zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e1a3aa3c56e036c782c4fb502c477dc0feaa0ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a>Obraťte se na podporu společnosti Microsoft pro vaše zařízení StorSimple
Pokud narazíte na potíže s vaším řešením Microsoft Azure StorSimple, můžete vytvořit žádost o službu pro technickou podporu. V online relaci s svého specialistu technické podpory můžete také potřebovat toostart relaci podpory zařízení StorSimple. Tento článek vás provede procesem:

* Jak požádat o toocreate podporu.
* Jak hello toostart relaci podpora v rozhraní Windows PowerShell zařízení StorSimple.

Zkontrolujte hello [StorSimple 8000 řady podporu SLA a informace o](https://msdn.microsoft.com/library/mt433077.aspx) předtím, než vytvoříte žádost o podporu.

## <a name="create-a-support-request"></a>Vytvořit žádost o podporu
Proveďte následující kroky toocreate žádosti o podporu hello:

#### <a name="toocreate-a-support-request"></a>toocreate žádosti o podporu
1. V hello [portál Azure classic](https://manage.windowsazure.com/), v horním pravém rohu hello, klikněte na název účtu a pak klikněte na tlačítko **obraťte se na podporu společnosti Microsoft**.
   
    ![MS obraťte se na podporu přes ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. Bude přesměrované toohello nový portál Azure (portal.azure.com). Klikněte na tlačítko hello **nová žádost o podporu** dlaždici.
   
    ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    Na pravé straně hello úvodní obrazovka, hello **nová žádost o podporu** podokně se zobrazí. 
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. V hello **Základy** dialogové okno, dokončení hello následující:                                
   
   1. Z hello **vydat typ** rozevíracího seznamu vyberte **technické**.
   2. Vyberte **předplatné** z rozevíracího seznamu hello.
   3. Z hello **služby** rozevíracího seznamu vyberte **StorSimple**. 
   4. Vyberte **plán podpory** z rozevíracího seznamu hello. Je nutné placené podporu plán tooenable technická podpora.
4. Klikněte na **Další**. Hello **problém** zobrazí se dialogové okno.
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. V hello **problém** dialogové okno, dokončení hello následující:
   
   1. Vyberte **závažnost** úrovně z rozevíracího seznamu hello.
   2. Vyberte **typ problému** z rozevíracího seznamu hello.
   3. Vyberte **kategorie** z rozevíracího seznamu hello. 
   4. V hello **podrobnosti** pole Krátce popište svůj problém.
   5. V hello **časového rámce** pole, zda text hello datum, čas a časové pásmo, které odpovídá toohello nejaktuálnějšího výskytu vašeho problému.
   6. V části **nahrávání souborů**, klikněte na tlačítko hello složky ikonu toobrowse tooyour podporu balíčku.
   7. Vyberte hello **sdílet diagnostické informace** zaškrtávací políčko.
6. Klikněte na **Další**. Hello **kontaktní informace** zobrazí se dialogové okno.
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. Zadejte své kontaktní informace a vyberte způsob kontaktu (telefon nebo e-mail). 
8. Vyberte hello **uložit změny kontaktů pro budoucí požadavky na podporu** zaškrtávací políčko.
9. Klikněte na možnost **Vytvořit**.

Po odeslání vaši žádost o pracovníka podpory vás bude kontaktovat co nejdříve tooproceed k žádosti.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Spusťte relaci podpory v prostředí Windows PowerShell pro StorSimple
tootroubleshoot všechny problémy, které setkat s hello zařízení StorSimple, budete potřebovat tooengage s týmem Microsoft Support hello. Microsoft Support může být nutné toouse toolog relace podporu na tooyour zařízení. 

Proveďte následující hello kroky toostart relaci podpory:

#### <a name="toostart-a-support-session"></a>toostart relaci podpory
1. Přístup hello zařízení přímo pomocí konzoly sériového portu hello nebo prostřednictvím relace Telnetu ze vzdáleného počítače. toodo tento, postupujte podle kroků hello v [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. V relaci hello, které se otevře, stiskněte klávesu hello **Enter** klíče tooget příkazového řádku.
3. V nabídce konzoly sériového portu hello, vyberte možnost 1, **přihlásit úplný přístup**.
4. Hello řádku zadejte následující heslo hello: 
   
    `Password1`
5. Hello řádku zadejte následující příkaz hello:
   
    `Enable-HcsSupportAccess`
6. Jako zašifrovaný řetězec zobrazí tooyou. Zkopírujte tento řetězec do textového editoru, například Poznámkový blok.
7. Uložte tento řetězec a poslat ho e-mailové zprávy tooMicrosoft podpory. 

> [!IMPORTANT]
> Podpora přístupu můžete vypnout spuštěním `Disable-HcsSupportAccess`. zařízení StorSimple Hello pokusí toodisable podporu přístup také 8 hodin po hello relace byl inicializován. Je nejlepší postup toochange zařízení StorSimple pověření po inicializaci relaci podpory.
> 
> 

