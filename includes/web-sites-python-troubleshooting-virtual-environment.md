skript nasazení Hello v Azure přeskočí vytvoření virtuálního prostředí hello, pokud zjistí, že kompatibilní virtuální prostředí již existuje.  To může nasazení značně urychlit.  Systém pip přeskočí balíčky, které jsou už nainstalované.

V některých situacích může být vhodné tooforce odstranění daného virtuálního prostředí.  Budete muset toodo to pokud se rozhodnete tooinclude virtuálním prostředí jako součást úložiště.  Můžete také toodo to potřebujete tooget zbavit určitých balíčků nebo testovat změny toorequirements.txt.

Existuje několik možností toomanage hello existující virtuální prostředí v Azure:

### <a name="option-1-use-ftp"></a>Možnost 1: Pomocí protokolu FTP
Pomocí klienta FTP připojte toohello serveru a budete moct toodelete hello env složky.  Všimněte si, že někteří klienti FTP (například webové prohlížeče) mohou být jen pro čtení a neumožňují toodelete složek, takže budete muset toomake zda toouse klient FTP má tuto funkci.  název hostitele Hello FTP a uživatele se zobrazuje v okně vaší webové aplikace na hello [portálu Azure](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Možnost 2: Přepnutí modulu runtime
Zde je alternativu, která využívá hello fakt, že hello skript nasazení odstraní složku env hello, pokud neodpovídá požadované verzi Pythonu hello.  To bude efektivně odstraňte hello stávajícího prostředí a vytvořte novou.

1. Přepínač tooa jinou verzi Pythonu (pomocí souboru runtime.txt nebo hello **nastavení aplikace** okno v hello portál Azure)
2. Pomocí příkazu git push proveďte nějaké změny (pokud dojde k chybám instalace systému pip, ignorujte je).
3. Přepnout zpět tooinitial verzi jazyka Python
4. Opět proveďte nějaké změny pomocí příkazu git push.

### <a name="option-3-customize-deployment-script"></a>Možnost 3: Přizpůsobení skriptu nasazení
Pokud jste přizpůsobili skript nasazení hello, můžete změnit hello kód v souboru deploy.cmd tooforce ho složku env toodelete hello.

