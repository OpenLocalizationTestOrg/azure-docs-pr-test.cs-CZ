---
title: "aaaManage zařízení s Snapshot Manager zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse hello tooconnect modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple a spravovat zařízení StorSimple."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 7a2a2ca830e4ea6eb4b01f2542958df3871c1700
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooconnect-and-manage-storsimple-devices"></a>Použití tooconnect Snapshot Manager zařízení StorSimple a správě zařízení StorSimple
## <a name="overview"></a>Přehled
Uzly můžete použít v hello StorSimple Snapshot Manager **oboru** podokně tooverify importovat data ze zařízení StorSimple a aktualizujte připojené paměťové zařízení. Kromě toho, když kliknete na tlačítko hello **zařízení** uzlu, zobrazí se seznam připojených zařízení a odpovídající informace o stavu v hello **výsledky** podokně.

![Připojená zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Obrázek 1: StorSimple Snapshot Manager připojeného zařízení** 

V závislosti na vaší **zobrazení** výběry, hello **výsledky** podokně se zobrazí následující informace o každé zařízení hello. (Další informace o konfiguraci zobrazení, přejděte příliš[nabídka Zobrazit](storsimple-use-snapshot-manager.md#view-menu).

| Sloupec výsledků | Popis |
|:--- |:--- |
| Name (Název) |Název Hello hello zařízení jako nakonfigurovaný v hello portál Azure classic |
| Model |číslo modelu Hello hello zařízení |
| Verze |Hello verzi hello software nainstalovaný v zařízení hello |
| Status |Jestli je k dispozici hello zařízení |
| Poslední synchronizace proběhla |Datum a čas, kdy zařízení hello poslední synchronizace |
| Sériové číslo. |Hello sériové číslo zařízení hello |

Pokud kliknete pravým tlačítkem na hello **zařízení** uzel v hello **oboru** podokně, můžete vybrat z hello následující akce:

* Přidat nebo nahradit zařízení
* Připojte zařízení a ověřte importy
* Aktualizujte připojená zařízení

Pokud kliknete na tlačítko hello **zařízení** uzel a potom klikněte pravým tlačítkem na zařízení názvu hello **výsledky** podokně, můžete vybrat z hello následující akce:

* Ověřování zařízení
* Zobrazení podrobností o zařízeních
* Aktualizace zařízení
* Odstranění konfigurace zařízení
* Změnit heslo k zařízení

> [!NOTE]
> Všechny tyto akce jsou také k dispozici v hello **akce** podokně.


Tento kurz vysvětluje, jak toouse tooconnect Snapshot Manager zařízení StorSimple a spravovat zařízení a provádět hello následující úlohy:

* Přidat nebo nahradit zařízení
* Připojte zařízení a ověřte importy
* Aktualizujte připojená zařízení
* Ověřování zařízení
* Zobrazení podrobností o zařízeních
* Aktualizace k jednotlivým zařízením
* Odstranění konfigurace zařízení
* Změnit heslo k zařízení s vypršenou platností
* Nahraďte zařízení se nezdařilo

> [!NOTE]
> Obecné informace o používání rozhraní StorSimple Snapshot Manager hello přejděte příliš[StorSimple Snapshot Manager uživatelské rozhraní](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Přidat nebo nahradit zařízení
Použijte následující postup tooadd hello nebo nahradit zařízení StorSimple.

#### <a name="tooadd-or-replace-a-device"></a>tooadd nebo nahradit zařízení
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně, klikněte pravým tlačítkem na hello **zařízení** uzel a pak klikněte na tlačítko **nakonfigurovat zařízení**. Hello **nakonfigurovat zařízení** zobrazí se dialogové okno.
   
    ![Konfigurace zařízení StorSimple](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. V hello **zařízení** rozevíracího seznamu, vyberte hello IP adresu zařízení hello nebo virtuální zařízení. 
4. V hello **heslo** textového pole, zadejte heslo Snapshot Manager zařízení StorSimple hello, kterou jste vytvořili pro zařízení hello v hello portál Azure classic. Klikněte na **OK**. StorSimple Snapshot Manager hledá hello zařízení, které jste určili. 
   
   * Pokud je k dispozici hello zařízení, StorSimple Snapshot Manager přidá připojení.
   * Pokud z nějakého důvodu není k dispozici hello zařízení, StorSimple Snapshot Manager vrátí chybovou zprávu. Klikněte na tlačítko **OK** tooclose hello chybovou zprávu a potom klikněte na **zrušit** tooclose hello **nakonfigurovat zařízení** dialogové okno.

## <a name="connect-a-device-and-verify-imports"></a>Připojte zařízení a ověřte importy
Použijte následující postup tooconnect zařízení StorSimple hello a ověřit, zda jsou naimportovány jakékoli existující svazek skupiny, které mají přidružené zálohy.

#### <a name="tooconnect-a-device-and-verify-imports"></a>tooconnect zařízení a ověřte importy
1. tooconnect zařízení tooStorSimple Snapshot Manager, postupujte podle pokynů hello v přidat nebo nahradit zařízení. Po připojení tooa zařízení StorSimple Snapshot Manager zareaguje takto:
   
   * Pokud z nějakého důvodu není k dispozici hello zařízení, StorSimple Snapshot Manager vrátí chybovou zprávu. 
   
   * Pokud je k dispozici hello zařízení, StorSimple Snapshot Manager přidá připojení. Když vyberete hello zařízení, zobrazí se v hello **výsledky** podokně a hello stav pole označuje, že hello zařízení **dostupné**. StorSimple Snapshot Manager importuje všechny svazku skupiny nakonfigurované pro hello zařízení za předpokladu, že mít hello svazku skupiny přidružené zálohy. Zásady zálohování nebyly naimportovány. Svazek skupin, které nemají přidružených záloh nebyly naimportovány.
2. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
3. Klikněte pravým tlačítkem na nejvyšší uzel hello v hello **oboru** podokně a pak klikněte na tlačítko **přepnout importů zobrazení**.
   
    ![Vyberte možnost přepnout importů zobrazení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. Hello **přepnout importů zobrazení** se zobrazí dialogové okno, zobrazuje stav hello hello importovat skupiny svazku a zálohování. Klikněte na **OK**.

Jakmile skupiny hello svazku a zálohování jsou úspěšně importovány, můžete použít toomanage Snapshot Manager zařízení StorSimple je, jenom jako by spravovat skupiny svazku a zálohování, které vytvoříte a nakonfigurujete s StorSimple Snapshot Manager. 

## <a name="refresh-connected-devices"></a>Aktualizujte připojená zařízení
Hello použijte následující postup toosynchronize hello připojení s StorSimple Snapshot Manager zařízení StorSimple.

#### <a name="toorefresh-connected-devices"></a>toorefresh připojená zařízení
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte pravým tlačítkem na **zařízení**a potom klikněte na **aktualizace zařízení**. Provádí synchronizaci hello připojené zařízení s StorSimple Snapshot Manager tak, aby můžete zobrazit skupiny hello svazku a zálohování, včetně všech nedávno přidány. 
   
    ![Aktualizace zařízení StorSimple hello](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

Hello **aktualizace zařízení** akce načte všechny nové skupiny svazek a všechny přidružené zálohy z připojených zařízení. Na rozdíl od hello **Prohledat znovu svazky** akce, které jsou k dispozici pro hello **svazky** uzlu **aktualizace zařízení** neobnoví hello zálohování registru.

## <a name="authenticate-a-device"></a>Ověřování zařízení
Použijte následující postup tooauthenticate s StorSimple Snapshot Manager zařízení StorSimple hello.

#### <a name="tooauthenticate-a-device"></a>tooauthenticate zařízení
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte na tlačítko **zařízení**.
3. V hello **výsledky** podokně klikněte pravým tlačítkem na název hello hello zařízení a pak klikněte na tlačítko **ověřit**.
4. Hello **ověřit** zobrazí se dialogové okno. Zadejte heslo hello zařízení a pak klikněte na tlačítko **OK**.
   
    ![Ověření dialogové okno](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Zobrazení podrobností o zařízeních
Použijte následující postup tooview hello podrobnosti o zařízení StorSimple hello a v případě potřeby znovu synchronizovat zařízení hello s StorSimple Snapshot Manager.

#### <a name="tooview-and-resynchronize-device-details"></a>Podrobnosti o zařízení tooview a resynchronizovat
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte na tlačítko **zařízení**.
3. V hello **výsledky** podokně klikněte pravým tlačítkem na název hello hello zařízení a pak klikněte na tlačítko **podrobnosti**.

4 hello **podrobnosti o zařízení** zobrazí se dialogové okno. Toto pole ukazuje název hello, modelu, verzi, sériové číslo, stav, cíl iSCSI kvalifikovaný název IQN () a poslední synchronizace datum a čas.

* Klikněte na tlačítko **nové synchronizace** toosynchronize hello zařízení.
* Klikněte na tlačítko **OK** nebo **zrušit** dialogové okno tooclose hello.
  
  ![Podrobnosti o zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Aktualizace k jednotlivým zařízením
Použijte následující postup tooresynchronize jednotlivých s StorSimple Snapshot Manager zařízení StorSimple hello.

#### <a name="toorefresh-a-device"></a>toorefresh zařízení
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager. 
2. V hello **oboru** podokně klikněte na tlačítko **zařízení**. 
3. V hello **výsledky** podokně klikněte pravým tlačítkem na název hello hello zařízení a pak klikněte na tlačítko **aktualizace zařízení**. To synchronizuje s StorSimple Snapshot Manager zařízení hello.

## <a name="delete-a-device-configuration"></a>Odstranění konfigurace zařízení
Použijte následující postup toodelete individuální konfigurace zařízení StorSimple z StorSimple Snapshot Manager hello.

#### <a name="toodelete-a-device-configuration"></a>toodelete konfigurace zařízení
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte na tlačítko **zařízení**. 
3. V hello **výsledky** podokně klikněte pravým tlačítkem na název hello hello zařízení a pak klikněte na tlačítko **odstranit**. 
4. Zobrazí se následující zpráva Hello. Klikněte na tlačítko **Ano** toodelete hello konfigurace nebo klikněte na tlačítko **ne** toocancel hello odstranění.
   
    ![Odstranit konfigurace zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Změnit heslo k zařízení s vypršenou platností
Je nutné zadat heslo tooauthenticate s StorSimple Snapshot Manager zařízení StorSimple. Při použití hello prostředí Windows PowerShell rozhraní tooset až hello zařízení nakonfigurujete toto heslo. Však můžete vypršení platnosti hesla hello. Pokud k tomu dojde, můžete použít hello Azure classic portálu toochange hello heslo. Pak, protože zařízení hello byl nakonfigurován v Snapshot Manager zařízení StorSimple, než vyprší platnost hesla hello, je třeba znovu ověřit hello zařízení ve Snapshot Manageru zařízení StorSimple.

#### <a name="toochange-hello-expired-password"></a>platnost jeho hesla toochange hello
1. V hello portál Azure classic spusťte službu StorSimple Manager hello.
2. Klikněte na tlačítko **zařízení** > **konfigurace** hello zařízení.
3. Posuňte se dolů toohello části StorSimple Snapshot Manager. Zadejte heslo, které je 14 až 15 znaků. Zajistěte, aby že toto heslo hello obsahuje kombinaci velká písmena, malá písmena, číselné a speciální znaky.
4. Zadejte znovu heslo tooconfirm hello ho.
5. Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.

#### <a name="toore-authenticate-hello-device"></a>toore-ověření hello zařízení
1. Spusťte Snapshot Manager zařízení StorSimple.
2. V hello **oboru** podokně klikněte na tlačítko **zařízení**. Seznam zařízení, nakonfigurovaného se zobrazí v hello **výsledky** podokně.
3. Vyberte hello zařízení, klikněte pravým tlačítkem a pak klikněte na tlačítko **ověřit**.
4. V hello **ověřit** okno, zadejte nové heslo hello.
5. Vyberte hello zařízení, klikněte pravým tlačítkem a vyberte **aktualizace zařízení**. To synchronizuje s StorSimple Snapshot Manager zařízení hello.

## <a name="replace-a-failed-device"></a>Nahraďte zařízení se nezdařilo
Pokud zařízení StorSimple selže a je nahrazena zařízení úsporném režimu (převzetí služeb při selhání), hello použijte následující postup tooconnect toohello nové zařízení a zobrazení hello přidružených záloh.

#### <a name="tooconnect-tooa-new-device-after-failover"></a>tooconnect tooa nové zařízení po převzetí služeb při selhání
1. Překonfigurujte hello iSCSI připojení toohello nové zařízení. Pokyny najdete příliš "krok 7: připojení, inicializace a formát svazek" v [nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).

> [!NOTE]
> Pokud nové zařízení StorSimple hello má stejnou IP adresu jako hello hello ten starý, je možné tooconnect hello původní konfigurace.


1. Zastavte službu StorSimple Management společnosti Microsoft hello:
   
   1. Spusťte správce serveru.
   2. Na hello řídicí panel Správce serveru, na hello **nástroje** nabídce vyberte možnost **služby**.
   3. Na hello **služby** okno, vyberte hello **služba správy zařízení StorSimple Microsoft**.
   4. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **zastavit službu hello**.
2. Odebrání hello konfigurační informace související toohello staré zařízení:
   
   1. V Průzkumníku souborů přejděte tooC:\ProgramData\Microsoft\StorSimple\BACatalog.
   2. Odstraníte hello soubory ve složce BACatalog hello.
3. Restartujte službu StorSimple Management společnosti Microsoft hello:
   
   1. Na hello řídicí panel Správce serveru, na hello **nástroje** nabídce vyberte možnost **služby**.
   2. Na hello **služby** okno, vyberte hello **služba správy zařízení StorSimple Microsoft**.
   3. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **restartujte službu hello**.
4. Spusťte Snapshot Manager zařízení StorSimple.
5. tooconfigure hello nové zařízení StorSimple, dokončení hello kroky v kroku 2: připojení zařízení StorSimple v [nasazení zařízení StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
6. Klikněte pravým tlačítkem na uzel nejvyšší úrovně hello v hello **oboru** panelu (Snapshot Manager zařízení StorSimple v příkladu hello) a pak klikněte na tlačítko **přepnout importů zobrazení**. 
7. Zpráva se zobrazí, když hello importovat skupiny svazku a zálohování jsou viditelné v zařízení StorSimple Snapshot Manager. Klikněte na **OK**.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Zjistěte, jak příliš[použijte tooview Snapshot Manager zařízení StorSimple a správu svazků](storsimple-snapshot-manager-manage-volumes.md).

