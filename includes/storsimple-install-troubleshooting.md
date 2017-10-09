<!--author=alkohli last changed: 03/17/16-->

## <a name="troubleshooting-update-failures"></a>Řešení potíží se selháním aktualizací
**Co dělat, když se zobrazí oznámení, že hello před upgradem kontroly se nezdařilo?**

Pokud předběžná kontrola selže, ujistěte se, že jste prohlédli hello podrobné oznamovací pruh v hello dolní části stránky hello. To obsahuje pokyny, jak předběžná kontrola toowhich se nezdařilo. Hello následující obrázek znázorňuje instanci, ve kterém se zobrazí toto oznámení. V takovém případě se nezdařily Kontrola stavu řadiče hello a kontrola stavu součásti hardwaru. V části hello **stavu hardwaru** části, můžete zobrazit, obě **řadič 0** a **řadič 1** součásti vyžadují pozornost.

  ![Selhání předběžné kontroly](./media/storsimple-install-troubleshooting/HCS_PreUpdateCheckFailed-include.png)

Budete potřebovat toomake se, že jsou oba řadiče v pořádku a online. Budete také potřebovat toomake jistotu, že všechny hello hardwarové součásti v zařízení StorSimple hello jsou zobrazeny toobe v pořádku na stránce údržby hello. Potom můžete zkusit tooinstall aktualizace. Pokud si nejste možné toofix hello hardwarové součásti problémy, bude nutné toocontact Microsoft Support pro další kroky.

**Co dělat, když se zobrazí chybová zpráva "Nelze instalovat aktualizace" a hello doporučení je aktualizace toohello toorefer řešení potíží s Průvodce toodetermine hello příčina selhání hello?**

Jeden pravděpodobně příčinou může být nemají servery Microsoft Update toohello připojení. Toto je ruční kontrolu vyžadující toobe provést. Pokud ztratíte připojení k serveru aktualizací toohello, by vaše úloha aktualizace selhala. Hello připojení můžete zkontrolovat spuštěním následující rutiny z rozhraní Windows PowerShell hello zařízení StorSimple hello:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Spusťte rutinu hello na oba řadiče.

Pokud jste ověřili hello připojení existuje, a budete pokračovat toosee tento problém, kontaktujte prosím Microsoft Support pro další kroky.

**Co v případě selhání aktualizace zobrazí při aktualizaci vašeho zařízení tooUpdate 4 a oba řadiče hello běží aktualizace 4?**

Spuštění aktualizace 4, pokud jsou oba řadiče hello hello spuštěna stejná verze softwaru, a pokud dojde selhání aktualizace, hello řadiče nepřecházejí do režimu obnovení. Tato situace může dojít, pokud hello opravu hotfix softwaru zařízení (update 1. pořadí) je použité tooboth hello řadiče úspěšně ale jiné opravy hotfix (pořadí 2 a 3. pořadí) jsou ještě toobe použít. Spuštění aktualizace 4, hello řadiče přejde do režimu obnovení pouze v případě, že hello dva řadiče se používají jiný software verze. 

Pokud selhání aktualizace hello uživateli se zobrazí, když oba řadiče běží aktualizace 4, doporučujeme, abyste Počkejte několik minut a opakujte aktualizaci. Pokud neproběhne úspěšně hello opakování, jejich měli požádat Microsoft Support.
