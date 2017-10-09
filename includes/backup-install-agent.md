## <a name="download-install-and-register-hello-azure-backup-agent"></a>Stažení, instalace a registrace agenta Azure Backup hello
Po vytvoření trezoru zálohování Azure hello, měla nainstalovat agenta na všechny vaše Windows počítače (Windows Server, klient systému Windows, server System Center Data Protection Manager nebo počítače serveru Azure Backup), které povoluje zálohování dat a aplikací tooAzure.

1. Přihlaste se toohello [portálu pro správu](https://manage.windowsazure.com/)
2. Klikněte na tlačítko **služeb zotavení**, pak vyberte hello úložiště záloh, které chcete tooregister se serverem. Zobrazí se stránka Hello rychlý Start pro tento trezor záloh.
   
    ![Rychlý start](./media/backup-install-agent/quickstart.png)
3. Na stránce Rychlý Start hello klikněte hello **klienta pro systém Windows Server nebo System Center Data Protection Manager nebo Windows** možnost pod **stáhnout agenta**. Klikněte na tlačítko **Uložit** toocopy ho toohello místního počítače.
   
    ![Uložit agenta](./media/backup-install-agent/agent.png)
4. Po instalaci agenta hello dvakrát klikněte na tlačítko MARSAgentInstaller.exe toolaunch hello instalace agenta Azure Backup hello. Vyberte instalační složku hello a pomocné složky, které jsou potřebné pro agenta hello. Zadané umístění mezipaměti Hello musí mít volné místo, který je nejméně 5 % hello zálohovaná data.
5. Pokud používáte proxy serveru tooconnect toohello Internetu, v hello **konfiguraci proxy serveru** obrazovky, zadejte podrobnosti o serveru proxy hello. Pokud používáte ověřený server proxy, zadejte podrobnosti uživatelského jména a hesla hello na této obrazovce.
6. agent Azure Backup Hello nainstaluje rozhraní .NET Framework 4.5 a prostředí Windows PowerShell (Pokud není k dispozici již) toocomplete hello instalaci.
7. Jakmile je nainstalován hello agent, klikněte na možnost hello **pokračovat tooRegistration** toocontinue tlačítko s pracovním postupem hello.
   
   ![Registrace](./media/backup-install-agent/register.png)
8. V úvodní obrazovka přihlašovací údaje trezoru vyhledejte tooand vyberte hello trezoru pověření souboru, který byl dříve staženy.
   
    ![Přihlašovací údaje trezoru](./media/backup-install-agent/vc.png)
   
    soubor s přihlašovacími údaji trezoru Hello je platná pouze pro 48 hodin (po jeho stažení z portálu hello). Pokud narazíte na chyby na této obrazovce (například "přihlašovací údaje trezoru poskytnuté souboru vypršela platnost"), přihlášení toohello portál Azure a přihlašovací údaje trezoru hello stažení souboru znovu.
   
    Ujistěte se, že soubor přihlašovacích údajů trezoru hello je k dispozici v umístění, která je přístupná hello instalační program aplikace. Pokud narazíte na přístup k související chyby, přihlašovací údaje trezoru hello kopírování souboru tooa dočasného umístění v tomto počítači a opakujte operaci hello.
   
    Pokud dojde k chybě neplatný úložiště přihlašovacích údajů (např "Neplatné přihlašovací údaje úložiště") hello soubor je buď poškozený nebo nemá poslední přihlašovací údaje hello přidruženy službou obnovení hello. Opakujte operaci hello po stažení nový soubor s přihlašovacími údaji trezoru z portálu hello. Tato chyba je zpravidla se zobrazí, pokud hello uživatel klikne na hello **přihlašovací údaje trezoru Stáhnout** možnost v hello portál Azure, rychle po sobě. V takovém případě je platný pouze hello druhý soubor pověření pro úložiště.
9. V hello **nastavení šifrování** obrazovky, můžete buď vygenerovat přístupové heslo nebo zadat přístupové heslo (minimálně 16 znaků). Mějte na paměti, toosave hello heslo v zabezpečeném umístění.
   
    ![Šifrování](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Pokud hello přístupové heslo ztratíte nebo zapomenete; Microsoft vám nemůže pomoci obnovit zálohovaná data hello. koncový uživatel Hello vlastní hello šifrovací přístupové heslo a Microsoft nemá přehled hello přístupové heslo používané hello koncového uživatele. Uložte soubor hello v zabezpečeném umístění, jako je vyžadována během operace obnovení.
   > 
   > 
10. Po kliknutí na tlačítko hello **Dokončit** tlačítko, hello počítač je registrovaný úspěšně toohello trezoru a můžete je teď připravený toostart zálohování tooMicrosoft Azure.
11. Při použití samostatných Microsoft Azure Backup můžete upravit hello nastavení určené během pracovního postupu registrace hello kliknutím na hello **změnit vlastnosti** možnost v hello Azure Backup konzoly mmc snap in.
    
    ![Změnit vlastnosti](./media/backup-install-agent/change.png)
    
    Případně, pokud používáte Data Protection Manager, můžete upravit hello nastavení určené během pracovního postupu registrace hello kliknutím hello **konfigurace** možnost výběrem **Online** pod hello **Správy** kartě.
    
    ![Konfigurace služby Azure Backup](./media/backup-install-agent/configure.png)

