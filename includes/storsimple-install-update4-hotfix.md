<!--author=alkohli last changed: 02/10/17-->

#### <a name="toodownload-hotfixes"></a>toodownload opravy hotfix

Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.

1. Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.

    ![Instalace katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello opravy hotfix hello chcete toodownload, například **4011839**a potom klikněte na **vyhledávání**.
   
    Hello opravu hotfix seznamu se zobrazí, například **kumulativní 4.0 Update sady softwaru pro zařízení StorSimple řady 8000**.
   
    ![Prohledávání katalogu](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)

4. Klikněte na **Stáhnout**. Zadejte nebo **Procházet** tooa místního umístění, kam má hello stáhne tooappear. Klikněte na tlačítko hello soubory toodownload toohello zadané umístění a složky. Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.
5. Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce hello (**4011841**), a stažení hello odpovídající soubory toohello určitých složek, jak je uvedeno v předcházející tabulce hello.

> [!NOTE]
> opravy hotfix Hello musí být dostupný z obou řadičích toodetect všechny potenciální chybové zprávy z hello sdílené řadiče.
>
> Hello opravy hotfix je nutné zkopírovat v 3 samostatné složky. Například aktualizace softwaru, SNS/MDS agenta hello zařízení je možné zkopírovat v _FirstOrderUpdate_ složky, všechny hello jiné omezovaly aktualizace může být zkopírovali v hello _SecondOrderUpdate_ složku, a aktualizace režimu údržby zkopírovali v _ThirdOrderUpdate_ složky.

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall a ověřte regulární režimu opravy hotfix

Proveďte následující kroky tooinstall hello a ověřte regular režimu opravy hotfix. Pokud jste již nainstalovali pomocí hello portál Azure classic, přeskočit příliš[instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello opravy hotfix, které rozhraní Windows PowerShell hello přístupu v konzole sériového portu zařízení StorSimple. Postupujte podle hello podrobné pokyny v [použití klienta PuTTy tooconnect toohello konzoly sériového portu](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Na příkazovém řádku hello, stiskněte klávesu **Enter**.
2. Vyberte **možnost 1** toolog na toohello zařízení s úplným přístupem. Doporučujeme nejprve nainstalujte opravu hotfix hello na pasivní řadiči hello.
3. tooinstall hello oprav hotfix, hello příkazového řádku, zadejte:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Používejte IP místo DNS v cestě sdílené složky v hello výše příkaz. Parametr credential Hello se používá pouze v případě, že se připojujete ověřené sdílené složky.
   
    Doporučujeme použít hello sdílené složky tooaccess parametr přihlašovacích údajů. I sdílené složky, které jsou otevřené příliš "everyone" jsou obvykle otevřít toounauthenticated uživatele.
   
    Zadejte heslo hello po zobrazení výzvy.
   
    Ukázkový výstup pro instalaci první pořadí aktualizací hello je uveden níže. Hello první pořadí aktualizací musíte toopoint toohello konkrétní soubor.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. Typ **Y** při výzvami tooconfirm hello instalace opravy hotfix.
5. Monitorovat hello aktualizace pomocí hello `Get-HcsUpdateStatus` rutiny. aktualizace Hello nejprve dokončí na pasivní řadiči hello. Jakmile řadič pasivní hello je aktualizován, budou existovat převzetí služeb při selhání a aktualizace hello pak se použije na hello jiný řadič. Když jsou aktualizovány oba řadiče hello po dokončení aktualizace Hello.
   
    Hello následující ukázkový výstup ukazuje hello aktualizace v průběhu. Hello `RunInprogress` bude `True` při hello Probíhá aktualizace.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 02/03/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Následující ukázkový výstup Hello označuje, že po dokončení této aktualizace hello. Hello `RunInProgress` bude `False` po dokončení aktualizace hello.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 02/03/2017 9:15:55 AM
    LastUpdateTimestamp : 02/03/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > V některých případech hello rutiny sestavy `False` po hello aktualizaci stále probíhá. tooensure, který hello opravu hotfix je dokončena, počkejte několik minut, spusťte tento příkaz znovu a ověřte, že hello `RunInProgress` je `False`. Pokud se jedná, byla dokončena hello opravu hotfix.

6. Po dokončení aktualizace softwaru hello ověřte verzí softwaru systému hello. Zadejte:
   
    `Get-HcsSystem`
   
    Měli byste vidět hello následující verze:
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 4.0`
   *  `HcsSoftwareVersion: 6.3.9600.17820`
   
    Pokud hello číslo verze se nezmění po použití aktualizace hello, znamená to, že oprava hotfix hello se nezdařilo tooapply. Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).
     
    > [!IMPORTANT]
    > Je nutné restartovat řadič active hello prostřednictvím hello `Restart-HcsController` rutiny před použitím hello další aktualizace.
     
7. Opakujte kroky 3 až 5 tooinstall hello tooyour stáhnout agenta položek konfigurace nebo MDS _FirstOrderUpdate_ složky. 
8. Zopakujte kroky 3 až 5 tooinstall hello druhý pořadí aktualizací. **Druhý pořadí aktualizací, lze nainstalovat víc aktualizací právě spuštěním hello `Start-HcsHotfix cmdlet` a polohovací toohello složku, kde se nachází druhý pořadí aktualizací. hello rutiny, budou spuštěny všechny hello aktualizace k dispozici ve složce hello.** Pokud už je nainstalovaná aktualizace, logiku aktualizace hello rozpozná a tuto aktualizaci nelze použít. 

Po instalaci všech hello opravy hotfix, použijte hello `Get-HcsSystem` rutiny. verze Hello by měla být:

   * `CisAgentVersion:  1.0.9441.0`
   * `MdsAgentVersion: 35.2.2.0`
   * `Lsisas2Version: 2.0.78.00`


#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall a ověřte opravy hotfix režimu údržby
Aktualizace firmwaru disku tooinstall KB4011837 použijte. Tyto jsou rušivý aktualizace a trvat toocomplete přibližně 30 minut. Můžete zvolit tooinstall v plánované údržby pomocí připojování konzoly sériového portu zařízení toohello.

Všimněte si, že pokud firmware disku je již aktuální, nebudete potřebovat tooinstall tyto aktualizace. Spustit hello `Get-HcsUpdateAvailability` rutiny z toocheck konzoly sériového portu zařízení hello Pokud aktualizace jsou k dispozici a zda text hello aktualizace jsou rušivý (režim údržby) nebo omezovaly (regulární režim) aktualizace.

aktualizace firmwaru disku tooinstall hello, postupujte podle pokynů hello.

1. Umístěte hello zařízení v režimu údržby hello. **Všimněte si, že byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell, při připojování zařízení tooa v režimu údržby. Místo toho spusťte tuto rutinu v hello zařízení řadiče při připojení prostřednictvím sériové konzoly hello zařízení.** Zadejte:
   
    `Enter-HcsMaintenanceMode`
   
    Ukázkový výstup najdete níž.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Oba řadiče hello potom restartujte do režimu údržby.
2. tooinstall hello disku aktualizaci firmwaru, typ:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Ukázkový výstup najdete níž.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. Monitorování hello instalovat průběh pomocí `Get-HcsUpdateStatus` příkaz. Hello po dokončení aktualizace při hello `RunInProgress` změní příliš`False`.
4. Po dokončení instalace hello hello řadiče, na které hello byla nainstalována oprava hotfix režimu údržby se restartuje. Přihlaste se jako možnost 1 s úplným přístupem a ověřit verzi firmwaru hello disku. Zadejte:
   
   `Get-HcsFirmwareVersion`
   
   Hello očekává, že jsou verzí firmwaru disku:
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N002, 0106`
   
   Ukázkový výstup najdete níž.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17820
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    Spustit hello `Get-HcsFirmwareVersion` příkaz na hello druhý řadič tooverify který hello verze softwaru se aktualizovala. Potom můžete ukončit režim údržby hello. toodo Ano, zadejte následující příkaz pro každý řadič zařízení hello:
   
   `Exit-HcsMaintenanceMode`

5. Hello řadiče restartovat po ukončení režimu údržby. Po hello disk firmware se úspěšně aktualizace a hello zařízení ukončilo režim údržby, návratový toohello portál Azure classic. Všimněte si, že tohoto portálu hello nemusí zobrazit, že jste nainstalovali aktualizace režimu údržby hello 24 hodin.

