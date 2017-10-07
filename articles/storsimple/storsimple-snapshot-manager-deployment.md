---
title: "aaaDeploy Snapshot Manager zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak toodownload a nainstalujte hello Snapshot Manager zařízení StorSimple, modul snap-in konzoly MMC pro správu funkce pro ochranu a zálohování dat StorSimple."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: dd90cca2bbb3410bb8cd459fb1a3ff3fb5f2997b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-snapshot-manager-mmc-snap-in"></a>Nasazení hello modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple

## <a name="overview"></a>Přehled
Hello Snapshot Manager zařízení StorSimple je modul snap-in konzoly Microsoft Management Console (MMC), který zjednodušuje ochrany dat a správu záloh v prostředí Microsoft Azure StorSimple. S StorSimple Snapshot Manager můžete spravovat Microsoft Azure StorSimple místního a cloudového úložiště, jako by šlo plně integrované úložiště systému, proto kvůli zjednodušení procesů zálohování a obnovení a snižuje tak náklady. 

Tento kurz popisuje požadavky na konfiguraci, také s postupy pro instalaci, odebrání a upgrade StorSimple Snapshot Manager.

> [!NOTE]
> * Nelze použít StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuální pole (také označované jako StorSimple místní virtuální zařízení).
> * Pokud máte v plánu tooinstall StorSimple Update 2 v zařízení StorSimple, že toodownload hello nejnovější verzi služby StorSimple Snapshot Manager a nainstalujte ji **před instalací StorSimple Update 2**. Hello nejnovější verzi služby StorSimple Snapshot Manager je zpětně kompatibilní a spolupracuje s všechny vydané verze Microsoft Azure StorSimple. Pokud používáte hello předchozí verze služby StorSimple Snapshot Manager, budete potřebovat tooupdate it (není nutné toouninstall hello předchozí verzi před instalací nové verze hello).


## <a name="storsimple-snapshot-manager-installation"></a>Instalace Snapshot Manager zařízení StorSimple
StorSimple Snapshot Manager lze nainstalovat do počítače, které je spuštěn operační systém Windows Server 2008 R2 SP1, Windows Server 2012 nebo Windows Server 2012 R2 hello. Na serverech se systémem Windows 2008 R2 musíte také nainstalovat systém Windows Server 2008 SP1 a Windows Management Framework 3.0.

Před instalací nebo upgradem hello StorSimple Snapshot Manager modul snap-in pro hello Microsoft Management Console (MMC), ujistěte se, že hello Microsoft Azure StorSimple zařízení a hostitele serveru jsou správně nakonfigurované.

## <a name="configure-prerequisites"></a>Konfigurace požadavků
Hello následující kroky poskytují souhrnné informace o úlohy konfigurace, je třeba provést před instalací hello StorSimple Snapshot Manager. Kompletní konfigurace Microsoft Azure StorSimple a informace o instalaci, včetně požadavky na systém a podrobné pokyny najdete v tématu [nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Než začnete, zkontrolujte hello [kontrolní seznam konfigurace nasazení](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) a a [požadavky nasazení](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) v [nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>Před instalací Snapshot Manager zařízení StorSimple
1. – Vybalení, připojení a připojení zařízení hello Microsoft Azure StorSimple, jak je popsáno v [instalace zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [instalace zařízení StorSimple 8600](storsimple-8600-hardware-installation.md).
2. Ověřte, že hostitelský počítač je spuštěný jeden z následujících operačních systémů hello:
   
   * Windows Server 2008 R2 (na servery se systémem Windows 2008 R2, musíte také nainstalovat systém Windows Server 2008 SP1 a Windows Management Framework 3.0)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     Pro virtuální zařízení StorSimple hello hostitele musí být virtuální počítač Microsoft Azure.
3. Ujistěte se, že jsou splněny všechny požadavky na konfiguraci Microsoft Azure StorSimple hello. Podrobnosti najdete příliš[požadavky nasazení](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites).
4. Připojit hostitele toohello hello zařízení a provedení počáteční konfigurace hello. Podrobnosti najdete příliš[kroky nasazení](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps).
5. Vytvářet svazky na hello zařízení a přiřaďte jim toohello hostitele a ověřte, že hello hostitele můžete připojit a používat je. StorSimple Snapshot Manager podporuje následující typy svazků hello:
   
   * Základní svazky
   * Jednoduché svazky
   * Dynamické svazky
   * Zrcadlené dynamické svazky (RAID 1)
   * Sdílených svazků clusteru
     
     Informace o vytváření svazků v zařízení StorSimple hello nebo virtuální zařízení StorSimple, přejděte příliš[krok 6: vytvoření svazku](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume)v [nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Instalace nové Snapshot Manager zařízení StorSimple
Před instalací Snapshot Manager zařízení StorSimple, ujistěte se, že hello svazky, které jste vytvořili v zařízení StorSimple hello nebo virtuální zařízení StorSimple připojit, inicializovat a formátu, jak je popsáno v [konfigurace požadavků](#configure-prerequisites).

> [!IMPORTANT]
> * Pro virtuální zařízení StorSimple hello hostitele musí být virtuální počítač Microsoft Azure. 
> * Hello hostitele musí používat Windows 2008 R2, Windows Server 2012 nebo Windows Server 2012 R2. Pokud váš server se systémem Windows Server 2008 R2, musíte také nainstalovat Windows Server 2008 SP1 a Windows Management Framework 3.0.
> * Před připojením hello zařízení tooStorSimple Snapshot Manager, je nutné nakonfigurovat připojení k iSCSI ze zařízení StorSimple toohello hello hostitele.

Postupujte podle těchto kroků toocomplete novou instalací služby StorSimple Snapshot Manager. Pokud instalujete upgrade, přejděte příliš[upgradovat nebo přeinstalovat StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

* Krok 1: Instalace zařízení StorSimple Snapshot Manager 
* Krok 2: Připojení zařízení StorSimple Snapshot Manager tooa 
* Krok 3: Ověření hello připojení toohello zařízení 

### <a name="step-1-install-storsimple-snapshot-manager"></a>Krok 1: Instalace zařízení StorSimple Snapshot Manager
Pomocí následujících kroků tooinstall StorSimple Snapshot Manager hello.

#### <a name="tooinstall-storsimple-snapshot-manager"></a>tooinstall Snapshot Manager zařízení StorSimple
1. Stáhnout software Snapshot Manager zařízení StorSimple hello (přejděte příliš[StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) v hello Microsoft Download Center) a uložte místně na hostiteli hello.
2. V Průzkumníku souborů klikněte pravým tlačítkem na hello komprimovanou složku a pak klikněte na tlačítko **extrahujte všechny**.
3. V hello **extrahujte komprimované (Zipped) složky** okno, v hello **vyberte cíl a extrahujte soubory** pole zadejte nebo vyhledejte toohello cestu, kam chcete toofile toobe extrahovat.
   
    > [!IMPORTANT]
    > Snapshot Manager zařízení StorSimple je nutné nainstalovat na jednotku C: hello.
    
4. Vyberte hello **zobrazit rozbalené soubory po dokončení** zaškrtněte políčko a potom klikněte na **extrahovat**.
   
    ![Extrahujte soubory](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Po dokončení hello extrakce se otevře hello cílovou složku. Dvakrát klikněte na ikonu instalační program aplikace hello, která se zobrazí v cílové složce hello.
6. Když hello **instalační program úspěšně** se zobrazí zpráva, klikněte na tlačítko **Zavřít**. Měli byste vidět hello StorSimple Snapshot Manager ikony na ploše.
   
    ![ikony na ploše](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-tooa-device"></a>Krok 2: Připojení zařízení StorSimple Snapshot Manager tooa
Pomocí následujících kroků tooconnect zařízení StorSimple Snapshot Manager zařízení StorSimple tooa hello.

#### <a name="tooconnect-storsimple-snapshot-manager-tooa-device"></a>tooconnect tooa StorSimple Snapshot Manager zařízení
1. Klikněte na ikonu StorSimple Snapshot Manager hello na ploše. Zobrazí se okno Hello StorSimple Snapshot Manager. okno Hello obsahuje **oboru** podokně **výsledky** podokně a **akce** podokně. 
   
    ![Uživatelské rozhraní Snapshot Manager zařízení StorSimple](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * Hello **oboru** podokně (levé podokno hello) obsahuje seznam uzlů, které jsou uspořádány do stromové struktury. Můžete rozbalit některé uzly tooselect zobrazení nebo konkrétní data související toothat uzlu. Klikněte na tlačítko tooexpand ikonu šipky hello nebo Sbalit uzel. Klikněte pravým tlačítkem na položku v hello **oboru** podokně toosee seznam dostupné akce pro tuto položku.
   * Hello **výsledky** hello center (podokno) obsahuje podrobné informace o hello uzlu, zobrazení nebo data, která jste vybrali v hello stavu **oboru** podokně.
   * Hello **akce** podokně seznam hello operací, které lze provést na uzlu hello, zobrazení nebo data, která jste vybrali v hello **oboru** podokně.
     
     Úplný popis hello StorSimple Snapshot Manager uživatelské rozhraní, najdete v části [StorSimple Snapshot Manager uživatelské rozhraní](storsimple-use-snapshot-manager.md).
2. V hello **oboru** podokně, klikněte pravým tlačítkem na hello **zařízení** uzel a pak klikněte na tlačítko **nakonfigurovat zařízení**. Hello **nakonfigurovat zařízení** zobrazí se dialogové okno.
   
    ![Konfigurace zařízení](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. V hello **zařízení** seznam pole, vyberte hello IP adresu zařízení Microsoft Azure StorSimple hello nebo virtuální zařízení. V hello **heslo** textového pole, zadejte heslo Snapshot Manager zařízení StorSimple hello, kterou jste vytvořili pro zařízení hello v hello portálu Azure. Klikněte na **OK**.
4. StorSimple Snapshot Manager hledá hello zařízení, které jste určili. Pokud je k dispozici hello zařízení, StorSimple Snapshot Manager přidá připojení. Můžete [ověřte hello připojení toohello zařízení](#to-verify-the-connection) tooconfirm, který hello připojení byl úspěšně přidán.
   
    Pokud z nějakého důvodu není k dispozici hello zařízení, StorSimple Snapshot Manager vrátí chybovou zprávu. Klikněte na tlačítko **OK** tooclose hello chybovou zprávu a potom klikněte na **zrušit** tooclose hello **nakonfigurovat zařízení** dialogové okno.
5. Při připojení zařízení tooa StorSimple Snapshot Manager importuje každý svazek skupiny nakonfigurované pro dané zařízení za předpokladu, že hello svazek má přidružené skupiny zálohy. Svazek skupin, které nemají přidružených záloh nebyly naimportovány. Kromě toho zásady zálohování, které byly vytvořeny pro skupinu svazku nebyly naimportovány. toosee hello importovaných skupin, klikněte pravým tlačítkem na hello nejvyšší **svazku skupiny** uzel v hello **oboru** panelu a klikněte na tlačítko **přepnutí importovaných skupin**.

### <a name="step-3-verify-hello-connection-toohello-device"></a>Krok 3: Ověření hello připojení toohello zařízení
Pomocí následujících kroků tooverify, že je StorSimple Snapshot Manager zařízení StorSimple připojené toohello hello.

#### <a name="tooverify-hello-connection"></a>tooverify hello připojení
1. V hello **oboru** podokně klikněte na tlačítko hello **zařízení** uzlu.
   
    ![Stav zařízení StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. Zkontrolujte hello **výsledky** podokně: 
   
   * Pokud se zobrazí na zelenou ukazatel na zařízení ikonu hello a **dostupné** se zobrazí v hello **stav** sloupce, pak zařízení hello je připojen. 
   * Pokud se zobrazí červený ukazatele na zařízení ikonu hello a není k dispozici v hello **stav** sloupce, pak zařízení hello není připojen. 
   * Pokud **aktualizace** se zobrazí v hello **stav** sloupec a potom StorSimple Snapshot Manager načítá skupiny svazku a přidružených záloh pro připojené zařízení.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Upgradovat nebo přeinstalovat Snapshot Manager zařízení StorSimple
StorSimple Snapshot Manager byste měli odinstalovat úplně upgradovat nebo přeinstalovat hello software. 

Před opětovnou instalací Snapshot Manager zařízení StorSimple, zálohujte hello existující databázi Snapshot Manager zařízení StorSimple v hostitelském počítači hello. To umožňuje ušetřit hello zálohování zásady a informace o konfiguraci tak, aby tato data můžete snadno obnovit ze zálohy.

Pokud se upgrade nebo opětovné instalaci Snapshot Manager zařízení StorSimple, postupujte takto:

* Krok 1: Odinstalujte Snapshot Manager zařízení StorSimple 
* Krok 2: Vytvořte zálohu databáze hello Snapshot Manager zařízení StorSimple 
* Krok 3: Přeinstalujte Snapshot Manager zařízení StorSimple a obnovení databáze hello 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Krok 1: Odinstalujte Snapshot Manager zařízení StorSimple
Pomocí následujících kroků toouninstall StorSimple Snapshot Manager hello.

#### <a name="toouninstall-storsimple-snapshot-manager"></a>toouninstall Snapshot Manager zařízení StorSimple
1. Na počítači hostitele hello otevřete hello **ovládací panely**, klikněte na tlačítko **programy**a potom klikněte na **programy a funkce**.
2. V levém podokně hello, klikněte na **odinstalovat nebo změnit program**.
3. Klikněte pravým tlačítkem na **StorSimple Snapshot Manager**a potom klikněte na **odinstalovat**.
4. Tím se spustí hello StorSimple Snapshot Manager instalační program. Klikněte na tlačítko **upravit nastavení**a potom klikněte na **odinstalovat**.
   
   > [!NOTE]
   > Pokud nejsou žádné procesy konzoly MMC v pozadí hello, například Snapshot Manager zařízení StorSimple nebo Správa disků, odinstalace hello se nezdaří a zobrazí se zpráva tooclose všechny instance konzoly MMC před pokus toouninstall hello programu. Vyberte **automaticky ukončete aplikace a pokusit se je po jejím dokončení toorestart**a potom klikněte na **OK**.
   > 
   > 
5. Při odinstalaci hello dokončení procesu **instalační program úspěšně** se zobrazí zpráva. Klikněte na **Zavřít**.

### <a name="step-2-back-up-hello-storsimple-snapshot-manager-database"></a>Krok 2: Vytvořte zálohu databáze hello Snapshot Manager zařízení StorSimple
Použijte následující postup toocreate hello a uložte kopii databáze hello StorSimple Snapshot Manager.

#### <a name="tooback-up-hello-database"></a>tooback zálohu databáze hello
1. Zastavte službu StorSimple Management společnosti Microsoft hello:
   
   1. Spusťte správce serveru.
   2. Na hello řídicí panel Správce serveru, na hello **nástroje** nabídce vyberte možnost **služby**.
   3. Na hello **služby** vyberte **služba správy zařízení StorSimple Microsoft**.
   4. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **zastavit službu hello**.
      
        ![Zastavit službu StorSimple Manager zařízení hello](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. Procházejte tooC:\ProgramData\Microsoft\StorSimple\BACatalog. 
   
   > [!NOTE]
   > ProgramData je skrytá složka.
  
3. Najít soubor XML hello katalogu, kopírovat hello soubor a zkopírujte hello úložiště do bezpečného umístění nebo v cloudu hello.
   
    ![Soubor zálohy katalogu StorSimple](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. Restartujte službu StorSimple Management společnosti Microsoft hello: 
   
   1. Na hello řídicí panel Správce serveru, na hello **nástroje** nabídce vyberte možnost **služby**.
   2. Na hello **služby** stránky, vyberte hello **Microsoft StorSimple správu Statistika**e.
   3. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **restartujte službu hello**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-hello-database"></a>Krok 3: Přeinstalujte Snapshot Manager zařízení StorSimple a obnovení databáze hello
tooreinstall Snapshot Manager zařízení StorSimple, postupujte podle kroků hello v [nainstalovat nový StorSimple Snapshot Manager](#install-a-new-storsimple-snapshot-manager). Potom použijte následující postup toorestore hello StorSimple Snapshot Manager databáze hello.

#### <a name="toorestore-hello-database"></a>toorestore hello databáze
1. Zastavte službu StorSimple Management společnosti Microsoft hello:
   
   1. Spusťte správce serveru.
   2. Na hello řídicí panel Správce serveru, na hello **nástroje** nabídce vyberte možnost **služby**.
   3. Na hello **služby** vyberte **služba správy zařízení StorSimple Microsoft**.
   4. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **zastavit službu hello**.
2. Procházejte tooC:\ProgramData\Microsoft\StorSimple\BACatalog.
   
   > [!NOTE]
   > ProgramData je skrytá složka.
   > 
   > 
3. Odstranit soubor XML hello katalogu a nahraďte ji metodou hello verze, který jste předtím uložili.
4. Restartujte službu StorSimple Management společnosti Microsoft hello: 
   
   1. Na hello řídicí panel Správce serveru, na hello **nástroje** nabídce vyberte možnost **služby**.
   2. Na hello **služby** vyberte **služba správy zařízení StorSimple Microsoft**.
   3. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **restartujte službu hello**.

## <a name="next-steps"></a>Další kroky
* toolearn více o Snapshot Manager zařízení StorSimple, přejděte příliš[co je StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md).
* toolearn Další informace o hello StorSimple Snapshot Manager uživatelské rozhraní, přejděte příliš[StorSimple Snapshot Manager uživatelské rozhraní](storsimple-use-snapshot-manager.md).
* toolearn Další informace o použití Snapshot Manager zařízení StorSimple, přejděte příliš[tooadminister Snapshot Manager zařízení StorSimple pomocí vašeho řešení StorSimple](storsimple-snapshot-manager-admin.md).

