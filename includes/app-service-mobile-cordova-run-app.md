
1. Navštivte hello [portál Azure].
2. Klikněte na tlačítko **App Services** > hello back-end, který jste vytvořili.
3. V nastavení mobilní aplikace hello, klikněte na tlačítko **rychlý Start** > **Cordova**.
![Azure Portal se zvýrazněnou možností Rychlý start Mobile Apps][quickstart]
4. V části **Configure your client application** (Konfigurace klientské aplikace) vyberte možnost **Vytvořit novou aplikaci** a klikněte na **Stáhnout**.
2. Rozbalte hello stáhnout ZIP soubor tooa adresáři na pevném disku, přejděte toohello soubor řešení (.sln) a otevřete jej pomocí sady Visual Studio.
3. V sadě Visual Studio vyberte hello další toothe počáteční šipku platforma řešení hello (Android, iOS nebo Windows). Vyberte konkrétní nasazení zařízení nebo emulátoru kliknutím hello na hello zelenou šipku rozevíracího seznamu. Můžete použít platformu Android výchozí hello a Ripple emulatoru. Pokročilejší kurzy (například nabízená oznámení) vyžadují tooselect podporovaných zařízení nebo emulátor.
4. toobuild a spuštění aplikace Cordova, stisknutím klávesy F5 nebo klikněte na tlačítko hello zelenou šipku. Pokud se zobrazí dialogové okno zabezpečení v síti žádajícího přístup toohello hello emulátoru, přijměte.
5. Po spuštění aplikace hello na emulátoru nebo zařízení hello zadejte smysluplný text v **zadejte nový text**, jako například *hello dokončení kurzu* a pak klikněte na tlačítko hello **přidat** tlačítko.

Hello back-end vloží data z požadavku hello do tabulky TodoItem hello v hello databáze SQL a vrátí informace o hello nově uložených položkách back toohello mobilní aplikace. mobilní aplikace Hello zobrazí tato data v seznamu hello.

Kroky 3 až 5 můžete opakovat i pro jiné platformy.

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png

<!-- URLs -->
[portál Azure]: https://portal.azure.com/
