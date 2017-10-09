<!--author=SharS last changed: 03/17/2016-->

#### <a name="toodownload-hotfixes"></a>toodownload opravy hotfix
Proveďte následující aktualizace softwaru hello toodownload kroky hello.

1. Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.
    ![Nainstalujte katalogu](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)
3. Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello opravy hotfix hello chcete toodownload, například **3063418**a potom klikněte na **vyhledávání**.
4. Zobrazí se hello **StorSimple aktualizace 1.2 zařízení aktualizace** sady. Klikněte na tlačítko **Přidat**. aktualizace Hello přidá toohello košík.
5. Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce hello (**3043005** a **3063416**) a přidejte každý košík hello.
6. Klikněte na **Zobrazit košík**.
   
    ![Zobrazit košík](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. Klikněte na **Stáhnout**. Zadejte nebo **Procházet** tooa místního umístění, kam má hello stáhne tooappear. Hello aktualizace se stáhnou toohello zadané umístění a umístěny v podsložce s hello stejný název jako hello aktualizace. Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.

> [!NOTE]
> opravy hotfix Hello musí být dostupný z obou řadičích toodetect všechny potenciální chybové zprávy z hello sdílené řadiče.
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall a ověřte regulární režimu opravy hotfix
Proveďte následující kroky tooinstall hello a ověřte hello regular režimu opravy hotfix. Pokud jste již nainstalovali pomocí hello portálu Azure, přeskočit příliš[instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).

1. aktualizace softwaru hello tooinstall, rozhraní Windows PowerShell hello přístupu v konzole sériového portu zařízení StorSimple. Postupujte podle hello podrobné pokyny v [použití klienta PuTTy tooconnect toohello konzoly sériového portu](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Na příkazovém řádku hello, stiskněte klávesu **Enter**.
2. Vyberte **možnost 1** toolog na toohello zařízení s úplným přístupem.
3. tooinstall hello balíček aktualizace, hello příkazového řádku, zadejte:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Používejte IP místo DNS v cestě sdílené složky v hello výše příkaz. Parametr credential Hello se používá pouze v případě, že se připojujete ověřené sdílené složky.
   
    Doporučujeme použít hello sdílené složky tooaccess parametr přihlašovacích údajů. I sdílené složky, které jsou otevřené příliš "everyone" jsou obvykle otevřít toounauthenticated uživatele.
   
    Ukázkový výstup najdete níž.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts hello hotfix installation and could reboot one or
    both of hello controllers. If hello device is serving I/Os, these will not
    be disrupted. Are you sure you want toocontinue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. Typ **Y** při výzvami tooconfirm hello instalace opravy hotfix.
5. Monitorovat hello aktualizace pomocí hello `Get-HcsUpdateStatus` rutiny.
   
    Hello následující ukázkový výstup ukazuje hello aktualizace v průběhu. Hello `RunInprogress` bude `True` při hello Probíhá aktualizace.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Následující ukázkový výstup Hello označuje, že po dokončení této aktualizace hello. Hello `RunInProgress` bude `False` po dokončení aktualizace hello.

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > V některých případech hello rutiny sestavy `False` po hello aktualizaci stále probíhá. tooensure, který hello opravu hotfix je dokončena, počkejte několik minut, spusťte tento příkaz znovu a ověřte, že hello `RunInProgress` je `False`. Pokud se jedná, byla dokončena hello opravu hotfix.
   > 
   > 
6. Po dokončení aktualizace softwaru hello ověřte verzí softwaru systému hello. Zadejte hello následující příkaz:
   
    `Get-HcsSystem`
   
    Měli byste vidět hello následující verze:
   
   * HcsSoftwareVersion: 6.3.9600.17584
   * CisAgentVersion: 1.0.9049.0
   * MdsAgentVersion: 26.0.4696.1433
     
     Pokud po použití aktualizace hello neměňte čísla verzí hello, znamená to, že oprava hotfix hello se nezdařilo tooapply. Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).
7. Opakujte kroky 3 až 5 tooinstall hello zbývající regular režimu oprav hotfix (KB3043005).

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall a ověřte opravy hotfix režimu údržby
Aktualizace firmwaru disku tooinstall KB3063416 použijte. Tyto jsou rušivý aktualizace a trvat přibližně toocomplete 30 – 45 minut. Můžete zvolit tooinstall v plánované údržby pomocí připojování konzoly sériového portu zařízení toohello.

aktualizace firmwaru disku tooinstall hello, postupujte podle pokynů hello.

1. Umístěte hello zařízení v režimu údržby. Všimněte si, že byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell, při připojování zařízení tooa v režimu údržby. Je nutné toorun Tato rutina v řadiči zařízení hello při připojení prostřednictvím sériové konzoly hello zařízení. Zadejte:
   
    `Enter-HcsMaintenanceMode`
   
    Ukázkový výstup najdete níž.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
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
        This operation starts a hotfix installation and could reboot one or both of hello controllers. Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. Monitorování hello instalovat průběh pomocí `Get-HcsUpdateStatus` příkaz. Hello po dokončení aktualizace při hello `RunInProgress` změní příliš`False`.
4. Po dokončení instalace hello hello řadiče, na kterém byla nainstalována oprava hotfix režimu údržby hello bude restartován. Přihlaste se jako možnost 1 s úplným přístupem a ověřit verzi firmwaru hello disku. Zadejte:
   
   `Get-HcsFirmwareVersion`
   
   Hello očekává, že jsou verzí firmwaru disku:
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   Spustit hello `Get-HcsFirmwareVersion` příkaz na hello druhý řadič tooverify který hello verze softwaru se aktualizovala. Potom můžete ukončit režim údržby hello. Zadejte následující příkaz pro každý řadič zařízení hello:
   
   `Exit-HcsMaintenanceMode`
5. Hello řadiče restartovat po ukončení režimu údržby. Po hello disk firmware se úspěšně aktualizace a hello zařízení ukončilo režim údržby, návratový toohello portál Azure classic. Všimněte si, že tohoto portálu hello nemusí zobrazit, že jste nainstalovali aktualizace režimu údržby hello 24 hodin.

