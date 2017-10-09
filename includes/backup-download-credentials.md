## <a name="using-vault-credentials-tooauthenticate-with-hello-azure-backup-service"></a>Pomocí tooauthenticate přihlašovací údaje trezoru hello služby zálohování Azure
místní server Hello (Windows klienta nebo serveru Windows Server nebo Data Protection Manager) musí toobe předtím, než můžete zálohovat data tooAzure k ověření pomocí úložiště záloh. ověřování Hello je dosaženo pomocí "přihlašovací údaje trezoru". Hello konceptu přihlašovací údaje trezoru je podobný koncept toohello "publikovat nastavení" souboru, který se používá v prostředí Azure PowerShell.

### <a name="what-is-hello-vault-credential-file"></a>Co je soubor s přihlašovacími údaji trezoru hello?
Hello soubor s přihlašovacími údaji trezoru je certifikát vytvořený portál hello pro každý trezor záloh. Hello portál poté odešle veřejný klíč toohello hello služby Řízení přístupu (ACS). privátní klíč certifikátu hello Hello přišla uživatele k dispozici toohello jako součást pracovního postupu hello, která je zadána jako vstup v pracovním postupu registrace počítače hello. To se ověřuje trezoru tooan identifikovat hello počítač toosend zálohování dat v hello služba Azure Backup.

přihlašovací údaje úložiště Hello se používají pouze během pracovního postupu registrace hello. Je tooensure odpovědnost hello uživatele, který hello pověření pro úložiště, které nejsou ohrožená souboru. Jestliže spadá do nesprávných rukou hello žádné neautorizovaný uživatel, soubor s přihlašovacími údaji trezoru hello lze použít tooregister dalších počítačů hello stejnému trezoru. Jako hello zálohovaná data šifrovaná pomocí hesla, která patří toohello zákazníka, však nemůže být ohrožena stávajících zálohovaných dat.. toomitigate tuto situaci přihlašovací údaje úložiště jsou nastaveny tooexpire v 48hrs. Si můžete stáhnout přihlašovací údaje trezoru hello úložiště záloh libovolný počet časy – ale během hello pracovního postupu registrace lze použít pouze hello nejnovější soubor pověření pro úložiště.

### <a name="download-hello-vault-credential-file"></a>Stáhněte si soubor s přihlašovacími údaji trezoru hello
soubor s přihlašovacími údaji trezoru Hello se stáhne prostřednictvím zabezpečeného kanálu z hello portálu Azure. Hello služby Azure Backup nezaznamená hello privátní klíč certifikátu hello, a v portálu hello nebo služby hello neuchovává jako trvalý hello privátní klíč. Pomocí následujících kroků toodownload hello úložiště přihlašovacích údajů souboru tooa místního počítače hello.

1. Přihlaste se toohello [portálu pro správu](https://manage.windowsazure.com/)
2. Klikněte na **služeb zotavení** v levém navigačním podokně hello a vyberte hello úložiště záloh, které jste vytvořili. Klikněte na hello cloudu ikonu tooget toohello rychlý Start zobrazení hello úložiště záloh.
   
   ![Rychlý přehled](./media/backup-download-credentials/quickview.png)
3. Na stránce hello rychlý Start, klikněte na **přihlašovací údaje trezoru Stáhnout**. portál Hello generuje soubor s přihlašovacími údaji trezoru hello, který je k dispozici ke stažení.
   
   ![Ke stažení](./media/backup-download-credentials/downloadvc.png)
4. Hello portálu vygeneruje přihlašovací údaje úložiště pomocí kombinace hello název trezoru a hello aktuální datum. Klikněte na tlačítko **Uložit** toodownload hello trezoru pověření toohello místního účtu stáhne složku nebo vyberte možnost Uložit jako z hello umístění pro přihlašovací údaje trezoru hello uložení toospecify nabídky.

### <a name="note"></a>Poznámka
* Ujistěte se, že přihlašovací údaje trezoru hello je uložen v umístění, které jsou přístupné z počítače. Pokud je uložený v souboru sdílené složky nebo SMB, zkontrolujte hello přístupová oprávnění.
* soubor s přihlašovacími údaji trezoru Hello se používají pouze během pracovního postupu registrace hello.
* Hello soubor s přihlašovacími údaji trezoru vyprší po 48hrs a lze stáhnout z portálu hello.
* Odkazovat toohello Azure Backup [– nejčastější dotazy](../articles/backup/backup-azure-backup-faq.md) pro všechny dotazy na pracovní postup hello.

