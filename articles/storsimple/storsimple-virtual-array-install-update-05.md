---
title: "aaaInstall aktualizace 0,5 v poli virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse hello pole virtuální zařízení StorSimple webového uživatelského rozhraní tooapply aktualizací pomocí hello Azure portal a opravy hotfix – metoda"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: c38daa85daa0086e67cf0206d76cb19d9c8b21b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a>Nainstalujte aktualizace 0,5 na pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Tento článek popisuje hello kroky požadované tooinstall aktualizace 0,5 na pole virtuální zařízení StorSimple prostřednictvím hello místního webového uživatelského rozhraní a prostřednictvím hello portálu Azure. Je nutné tooapply softwarové aktualizace a opravy hotfix tookeep pole virtuální zařízení StorSimple aktuální.

Před instalací aktualizace, doporučujeme je provést hello svazky nebo sdílené složky offline na hello nejprve hostitele a pak hello zařízení. Tím se minimalizují možnost poškození dat. Jakmile hello svazky nebo sdílené složky jsou offline, můžete také provést ruční zálohování hello zařízení.

> [!IMPORTANT]
> - Aktualizace 0,5 odpovídá příliš**10.0.10290.0** verze softwaru ve vašem zařízení. Informace na to, co je nového v této aktualizaci najdete příliš[poznámky k verzi pro aktualizaci 0,5](storsimple-virtual-array-update-05-release-notes.md).
>
> - Pokud používáte verzi Update 0,2 nebo novější, doporučujeme, aby instalaci aktualizací hello prostřednictvím hello portálu Azure. Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít opravu hotfix metody hello přes hello místního webového uživatelského rozhraní tooinstall aktualizace 0,5.
>
> - Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení. Zadaná, hello pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku.

## <a name="use-hello-azure-portal"></a>Hello použití portálu Azure

Pokud aktualizace 0,2 a novější, doporučujeme instalovat aktualizace prostřednictvím hello portálu Azure. Postup portálu Hello vyžaduje tooscan uživatele hello, stáhněte a nainstalujte aktualizace hello. Tento postup trvá asi 7 minut toocomplete. Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Hello instalace po dokončení, přejděte tooyour služby StorSimple Manager zařízení. Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello k aktualizaci. Přejděte příliš**Nastavení > Správa > aktualizace zařízení**. verze softwaru Hello zobrazí musí být **10.0.10290.0**.

## <a name="use-hello-local-web-ui"></a>Použít místní hello webového uživatelského rozhraní

Existují dva kroky při použití hello místního webového uživatelského rozhraní:

* Stáhnout hello aktualizaci nebo opravu hotfix hello
* Nainstalujte hello aktualizaci nebo opravu hotfix hello

### <a name="download-hello-update-or-hello-hotfix"></a>Stáhnout hello aktualizaci nebo opravu hotfix hello

Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.

#### <a name="toodownload-hello-update-or-hello-hotfix"></a>toodownload hello aktualizace nebo hello oprav hotfix

1. Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.

3. Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello hello opravy hotfix, kterou chcete toodownload. Zadejte **4021576** pro aktualizace 0,5 a pak klikněte na tlačítko **vyhledávání**.
   
    Hello opravu hotfix seznamu se zobrazí, například **aktualizace pole virtuálního zařízení StorSimple 0,5**.
   
    ![Prohledávání katalogu](./media/storsimple-virtual-array-install-update-05/download1.png)

4. Klikněte na **Stáhnout**. 

5. Měli byste vidět dva soubory toodownload *.msu* a *.cab* souboru. Stáhněte si každý z těchto souborů tooa složky. Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.

6. Otevřete složku hello, kde jsou umístěny soubory hello.
    ![Soubory v balíčku hello](./media/storsimple-virtual-array-install-update-05/update05folder.png)

    Zobrazí:
    -  Soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`. Tento soubor je software, zařízení používaných tooupdate hello.
    - Soubor balíčku agenta monitorování Genevy `GenevaMonitoringAgentPackageInstaller`. Tento soubor je použité tooupdate hello monitorovací a diagnostické služby (MDS) agenta. Poklikejte na soubor cab hello. Zobrazí se .msi. Vyberte hello souboru, klikněte pravým tlačítkem a potom **extrahovat** hello souboru. Budete používat hello _.msi_ souboru tooupdate hello agenta.

        ![Extrahujte soubor aktualizace agenta služby MDS](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-hello-update-or-hello-hotfix"></a>Nainstalujte hello aktualizaci nebo opravu hotfix hello

Předchozí toohello instalace aktualizace nebo opravy hotfix, ujistěte se, že máte hello aktualizace nebo opravy hotfix hello stáhli buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky.

Tato metoda tooinstall aktualizace použít na zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru. Tento postup trvá méně než 2 minuty toocomplete. Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a>tooinstall hello aktualizace nebo hello oprav hotfix

1. Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **aktualizace softwaru**.
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. V **cesta k souboru aktualizace**, zadejte název souboru hello pro aktualizaci hello nebo hello opravu hotfix. Pokud je umístěn ve sdílené síťové složce, procházet toohello aktualizaci nebo opravu hotfix instalační soubor. Klikněte na tlačítko **Použít**.
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Zobrazí se upozornění. Zadaný to je jeden uzel zařízení, po instalaci aktualizace hello, hello zařízení restartuje a dojde k výpadku. Klikněte na ikonu zaškrtnutí hello.
   
   ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. Hello aktualizace spustí. Po úspěšné aktualizaci hello zařízení restartuje. Hello místního uživatelského rozhraní není dostupný v této hodnotě duration.
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Po dokončení restartování hello přejdete toohello **přihlášení** stránky. tooverify, která se aktualizovala hello zařízení software, hello místního webového uživatelského rozhraní, přejděte příliš**údržby** > **aktualizace softwaru**. verze softwaru Hello zobrazí musí být **10.0.0.0.0.10290.0** pro aktualizaci 0,5.
   
   > [!NOTE]
   > Jsme sestavy verze softwaru hello mírně jiným způsobem v hello místního webového uživatelského rozhraní a hello portálu Azure. Například hello místní webové sestavy nástroje uživatelského rozhraní **10.0.0.0.0.10290** a hello Azure portálu sestavy **10.0.10290.0** pro hello stejnou verzi.
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. dalším krokem Hello je tooupdate hello MDS agenta. V hello **aktualizace softwaru** stránky, přejděte toohello **cesta k souboru aktualizace** a procházet toohello `GenevaMonitoringAgentPackageInstaller.msi` souboru. Opakujte kroky 2 – 4. Po restartování hello virtuální pole, přihlaste se k hello místního webového uživatelského rozhraní.

Nyní po dokončení aktualizace Hello.

## <a name="next-steps"></a>Další kroky

Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

