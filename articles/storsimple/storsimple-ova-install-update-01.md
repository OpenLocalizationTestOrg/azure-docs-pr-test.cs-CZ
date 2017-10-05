---
title: "Instalaci aktualizací v poli virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje způsob použití pole virtuální zařízení StorSimple webového uživatelského rozhraní na aktualizace pomocí metody portál a opravy hotfix"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 7f1b1e7d-04d0-4ca2-9dbb-77077ff19bb9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/07/2016
ms.author: alkohli
ms.openlocfilehash: bccb0d49c1959a690d513961c32d946763385a87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array"></a>Instalaci aktualizací do pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek popisuje kroky potřebné k instalaci aktualizací do pole virtuální zařízení StorSimple prostřednictvím místního webového uživatelského rozhraní a prostřednictvím portálu Azure classic. Budete muset použít aktualizace softwaru nebo opravy hotfix, které k zachování aktualizovaného stavu pole virtuální zařízení StorSimple. 

Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení. Vzhledem k tomu, že pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku. 

Než použijete aktualizaci, doporučujeme vám věnovat svazky nebo sdílené složky do offline režimu na hostiteli první a pak zařízení. Tím se minimalizují možnost poškození dat.

> [!IMPORTANT]
> Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít metodu opravu hotfix prostřednictvím místního webového uživatelského rozhraní pro instalaci aktualizace 0.3. Pokud používáte aktualizace 0,2, doporučujeme vám, že instalujete aktualizace prostřednictvím portálu Azure classic.
> 
> 

## <a name="use-the-local-web-ui"></a>Použití místního webového uživatelského rozhraní
Existují dva kroky při použití místního webového uživatelského rozhraní:

* Stáhnout aktualizace nebo opravy hotfix
* Instalovat aktualizace nebo opravy hotfix

### <a name="download-the-update-or-the-hotfix"></a>Stáhnout aktualizace nebo opravy hotfix
Provedením následujících kroků si stáhněte aktualizace softwaru z Katalogu služby Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Chcete-li stáhnout aktualizace nebo opravy hotfix
1. Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.
3. Do vyhledávacího pole katalogu služby Microsoft Update zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout. Zadejte **3182061** pro aktualizace 0.3 a pak klikněte na tlačítko **vyhledávání**.
   
    Zobrazí se seznam oprav hotfix, například **StorSimple virtuální pole Update 0.3**.
   
    ![Prohledávání katalogu](./media/storsimple-ova-install-update-01/download1.png)
4. Klikněte na tlačítko **Přidat**. Aktualizace se přidá do košíku.
5. Klikněte na **Zobrazit košík**.
6. Klikněte na **Stáhnout**. Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**. Aktualizace se stáhnou do zadaného umístění do podsložky se stejným názvem, jako má aktualizace. Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.
7. Otevřete složku, zkopírovaný, měli byste vidět soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`. Tento soubor se používá k instalaci, aktualizaci nebo opravu hotfix.

### <a name="install-the-update-or-the-hotfix"></a>Instalovat aktualizace nebo opravy hotfix
Před instalací aktualizace nebo opravy hotfix ověřte, že aktualizaci nebo opravu hotfix stažené buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky. 

Tuto metodu použijte k instalaci aktualizací do zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru. Tento postup trvá méně než 2 minut. Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.

#### <a name="to-install-the-update-or-the-hotfix"></a>K instalaci aktualizace nebo opravy hotfix
1. V místní webového uživatelského rozhraní, přejděte do **údržby** > **aktualizace softwaru**.
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update1m.png)
2. V **cesta k souboru aktualizace**, zadejte název souboru pro aktualizaci nebo opravu hotfix. Můžete také procházet k instalačnímu souboru aktualizaci nebo opravu hotfix Pokud umístěn ve sdílené síťové složce. Klikněte na tlačítko **Použít**.
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update2m.png)
3. Zobrazí se upozornění. Zadaný to je jeden uzel zařízení, když tuto aktualizaci nenainstalujete, restartování zařízení a dojde k výpadku. Klikněte na ikonu zaškrtnutí.
   
   ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update3m.png)
4. Aktualizace spustí. Po úspěšné aktualizaci zařízení restartuje. Místní uživatelské rozhraní není dostupný v této hodnotě duration.
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update5m.png)
5. Po dokončení restartování se otevře **přihlášení** stránky. Chcete-li ověřit, že zařízení software byl aktualizován, v místní webového uživatelského rozhraní, přejděte na **údržby** > **aktualizace softwaru**. Zobrazit software verze musí být **10.0.0.0.0.10288.0** pro Update 0.3.
   
   > [!NOTE]
   > Verze softwaru jsme sestavu mírně odlišné způsobem místního webového uživatelského rozhraní a portálu Azure classic. Například místní webového uživatelského rozhraní sestavy **10.0.0.0.0.10288** a sestavy portálu Azure classic **10.0.10288.0** pro stejnou verzi. 
   > 
   > 
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update6m.png)

## <a name="use-the-azure-classic-portal"></a>Použití portálu Azure classic
Pokud aktualizace 0,2, doporučujeme instalovat aktualizace prostřednictvím portálu Azure classic. Portál postup vyžaduje, aby uživatel ke kontrole, stáhněte a nainstalujte aktualizace. Tento postup trvá asi 7 minut. Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.

[!INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Po dokončení instalace (jako je označené podle stavu úlohy na 100 %), přejděte na **zařízení > údržby > aktualizací softwaru**. Verze zobrazených softwaru by měl být 10.0.10288.0.

![aktualizace zařízení](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Další kroky
Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

