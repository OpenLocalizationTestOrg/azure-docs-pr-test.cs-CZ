<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-regular-hotfixes-via-windows-powershell-for-storsimple"></a>regulární opravy hotfix tooinstall prostřednictvím Windows Powershellu pro StorSimple
1. Připojte toohello konzoly sériového portu zařízení. Další informace najdete v tématu [krok 1: připojení konzoly sériového portu toohello](../articles/storsimple/storsimple-update-device.md#step1).
2. V nabídce konzoly sériového portu hello, vyberte možnost 1, **přihlásit úplný přístup**. Zadejte heslo hello. výchozí heslo Hello je **Heslo1**.
3. Hello příkazového řádku zadejte:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Tento příkaz se vztahuje pouze tooregular opravy hotfix. Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje.
    > Může dojít k selhání řadiče během procesu aktualizace hello; převzetí služeb při selhání hello však nebude mít vliv dostupnosti systému nebo operace.

4. Po zobrazení výzvy zadejte hello cesta toohello síťové sdílené složky, která obsahuje soubory oprav hotfix hello.
5. Zobrazí se výzva k potvrzení. Typ **Y** tooproceed s hello instalace opravy hotfix.

