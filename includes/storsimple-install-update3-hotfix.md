<!--author=alkohli last changed: 09/01/16-->

#### <a name="toodownload-hotfixes"></a>toodownload opravy hotfix
Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.

1. Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.
    ![Nainstalujte katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello opravy hotfix hello chcete toodownload, například **3186843**a potom klikněte na **vyhledávání**.
   
    Hello opravu hotfix seznamu se zobrazí, například **kumulativní softwaru sady aktualizace 3.0 pro zařízení StorSimple řady 8000**.
   
    ![Prohledávání katalogu](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. Klikněte na tlačítko **Přidat**. aktualizace Hello je přidána toohello košík.
5. Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce hello (**3186859**) a přidejte každý toohello košík.
6. Klikněte na **Zobrazit košík**.
7. Klikněte na **Stáhnout**. Zadejte nebo **Procházet** tooa místního umístění, kam má hello stáhne tooappear. Hello aktualizace se stáhnou toohello zadané umístění a umístit do dílčí složky s hello stejný název jako hello aktualizace. Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.

> [!NOTE]
> opravy hotfix Hello musí být dostupný z obou řadičích toodetect všechny potenciální chybové zprávy z hello sdílené řadiče.
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall a ověřte regulární režimu opravy hotfix
Proveďte následující kroky tooinstall hello a ověřte regular režimu opravy hotfix. Pokud jste již nainstalovali pomocí hello portálu Azure, přeskočit příliš[instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello opravy hotfix, které rozhraní Windows PowerShell hello přístupu v konzole sériového portu zařízení StorSimple. Postupujte podle hello podrobné pokyny v [použití klienta PuTTy tooconnect toohello konzoly sériového portu](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Na příkazovém řádku hello, stiskněte klávesu **Enter**.
2. Vyberte **možnost 1** toolog na toohello zařízení s úplným přístupem. Doporučujeme nejprve nainstalujte opravu hotfix hello na pasivní řadiči hello.
3. tooinstall hello oprav hotfix, hello příkazového řádku, zadejte:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Používejte IP místo DNS v cestě sdílené složky v hello výše příkaz. Parametr credential Hello se používá pouze v případě, že se připojujete ověřené sdílené složky.
   
    Doporučujeme použít hello sdílené složky tooaccess parametr přihlašovacích údajů. I sdílené složky, které jsou otevřené příliš "everyone" jsou obvykle otevřít toounauthenticated uživatele.
   
    Zadejte heslo hello po zobrazení výzvy.
   
    Ukázkový výstup najdete níž.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
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
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Následující ukázkový výstup Hello označuje, že po dokončení této aktualizace hello. Hello `RunInProgress` bude `False` po dokončení aktualizace hello.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > V některých případech hello rutiny sestavy `False` po hello aktualizaci stále probíhá. tooensure, který hello opravu hotfix je dokončena, počkejte několik minut, spusťte tento příkaz znovu a ověřte, že hello `RunInProgress` je `False`. Pokud se jedná, byla dokončena hello opravu hotfix.

1. Po dokončení aktualizace softwaru hello ověřte verzí softwaru systému hello. Zadejte:
   
    `Get-HcsSystem`
   
    Měli byste vidět hello následující verze:
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     Pokud po použití aktualizace hello neměňte čísla verzí hello, znamená to, že oprava hotfix hello se nezdařilo tooapply. Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).
     
     > [!IMPORTANT]
     > Je nutné restartovat řadič active hello prostřednictvím hello `Restart-HcsController` rutiny před použitím hello zbývající aktualizace. 
     > 
     > 
2. Opakujte kroky 3 až 5 tooinstall hello LSI ovladače a firmware opravu hotfix **KB3186859**. Po instalaci opravy hotfix hello použít hello `Get-HcsSystem` rutiny. Hello LSI verze musí být:
   
   * `Lsisas2Version: 2.0.78.00`
3. Opakujte kroky 3 až 5 tooinstall hello Storport a aktualizace Spaceport **KB3121261**.
4. Při aktualizaci Update 2 nebo dřívější verzi, budete také potřebovat toodownload:
   
   * Oprava iSCSI pomocí KB3146621
   * Oprava rozhraní WMI pomocí KB3103616

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall a ověřte opravy hotfix režimu údržby
Aktualizace firmwaru disku tooinstall KB3121899 použijte. Tyto jsou rušivý aktualizace a trvat toocomplete přibližně 30 minut. Můžete zvolit tooinstall v plánované údržby pomocí připojování konzoly sériového portu zařízení toohello.

Všimněte si, že pokud firmware disku je již aktuální, nebudete potřebovat tooinstall tyto aktualizace. Spustit hello `Get-HcsUpdateAvailability` rutiny z toocheck konzoly sériového portu zařízení hello Pokud aktualizace jsou k dispozici a zda text hello aktualizace jsou rušivý (režim údržby) nebo omezovaly (regulární režim) aktualizace.

aktualizace firmwaru disku tooinstall hello, postupujte podle pokynů hello.

1. Umístěte hello zařízení v režimu údržby hello. Všimněte si, že byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell, při připojování zařízení tooa v režimu údržby. Místo toho spusťte tuto rutinu v hello zařízení řadiče při připojení prostřednictvím sériové konzoly hello zařízení. Zadejte:
   
    `Enter-HcsMaintenanceMode`
   
    Ukázkový výstup najdete níž.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
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
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
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
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Ukázkový výstup najdete níž.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    Spustit hello `Get-HcsFirmwareVersion` příkaz na hello druhý řadič tooverify který hello verze softwaru se aktualizovala. Potom můžete ukončit režim údržby hello. toodo Ano, zadejte následující příkaz pro každý řadič zařízení hello:
   
   `Exit-HcsMaintenanceMode`
5. Hello řadiče restartovat po ukončení režimu údržby. Po hello disk firmware se úspěšně aktualizace a hello zařízení ukončilo režim údržby, návratový toohello portál Azure classic. Všimněte si, že tohoto portálu hello nemusí zobrazit, že jste nainstalovali aktualizace režimu údržby hello 24 hodin.

