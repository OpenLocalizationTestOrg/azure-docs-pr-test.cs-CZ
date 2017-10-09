### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Příprava nabízené instalace na serveru s Linuxem

1. Zkontrolujte, zda je síťové připojení mezi hello počítače se systémem Linux a hello procesového serveru.
2. Vytvoření účtu, že tento proces server hello můžete použít tooaccess hello počítače. Hello účet by měl být **kořenové** uživatele na server Linux hello zdroje. (Jenom pro hello nabízené instalace a aktualizace, použijte tento účet.)
3. Zkontrolujte tento soubor/etc/hosts hello na zdroji hello Linux server má položky, které mapují hello názvem místního hostitele tooIP adresy přidružené k všechny síťové adaptéry.
4. Nainstalujte nejnovější openssh, openssh server a openssl balíčky hello hello počítače, které chcete tooreplicate.
5. Ujistěte se, že je povolený Secure Shell (SSH) a že běží na portu 22.
6. Povolte ověřování pomocí protokolu SFTP subsystém a heslo v souboru sshd_config hello:
  1.  Přihlaste se jako uživatel **root**.
  2.  V hello soubor/etc/ssh/sshd_config souboru najít hello řádek, který začíná **PasswordAuthentication**.
  3.  Zrušením komentáře u hello řádku a změňte hodnotu hello příliš**Ano**.
  4.  Najít hello řádek, který začíná **subsystému** a zrušte komentář u hello řádku.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. Restartujte hello **sshd** služby.

7. Přidejte hello účet, který jste vytvořili v CSPSConfigtool.
    1.  Přihlaste se tooyour konfigurační server.
    2.  Otevřete **cspsconfigtool.exe**. (Je k dispozici jako zástupce na ploše hello a ve složce %ProgramData%\home\svsystems\bin hello.)
    3.  Na hello **Správa účtů** , klikněte na **přidat účet**.
    4.  Přidejte hello účet, který jste vytvořili. 
    5.  Zadejte přihlašovací údaje hello, kterou použijete při povolíte replikaci pro počítač.
