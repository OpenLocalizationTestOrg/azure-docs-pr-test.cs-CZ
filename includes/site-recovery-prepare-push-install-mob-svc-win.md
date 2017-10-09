### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Příprava pro nabízenou instalaci v počítači se systémem Windows

1. Zkontrolujte, zda je síťové připojení mezi počítači Windows hello a hello procesového serveru.
2. Vytvoření účtu, že tento proces server hello můžete použít tooaccess hello počítače. Hello účet by měl mít oprávnění správce (místní nebo doménový). (Jenom pro hello nabízené instalace a aktualizace agenta, použijte tento účet.)

   > [!NOTE]
   > Pokud nepoužíváte účet domény, zakažte řízení vzdáleného přístupu uživatele na místním počítači hello. toodisable řízení vzdáleného přístupu uživatele, v klíči registru HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System hello, přidejte novou hodnotu DWORD: **LocalAccountTokenFilterPolicy**. Nastavte hodnotu hello příliš**1**. toodo to do příkazového řádku, spusťte následující příkaz hello:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. V bráně Windows Firewall v počítači hello chcete tooprotect, vyberte **povolit aplikace nebo funkci průchod bránou Firewall**. Povolit **sdílení souborů a tiskáren** a **Windows Management Instrumentation (WMI)**. Pro počítače, které patří tooa domény můžete nakonfigurovat nastavení brány firewall hello pomocí objektu zásad skupiny (GPO).

   ![Nastavení brány firewall](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. Přidejte hello účet, který jste vytvořili v CSPSConfigtool.
    1.  Přihlaste se tooyour konfigurační server.
    2.  Otevřete **cspsconfigtool.exe**. (Je k dispozici jako zástupce na ploše hello a ve složce %ProgramData%\home\svsystems\bin hello.)
    3.  Na hello **Správa účtů** vyberte **přidat účet**.
    4.  Přidejte hello účet, který jste vytvořili.
    5.  Zadejte přihlašovací údaje hello, kterou použijete při povolíte replikaci pro počítač.
