### <a name="determine-hello-dns-name-of-hello-virtual-machine"></a>Určení názvu DNS hello hello virtuálního počítače
tooconnect toohello databázového stroje SQL Server z jiného počítače, musíte znát hello systému DNS (Domain Name) název hello virtuálního počítače. (Toto je hello název hello Internetu používá tooidentify hello virtuálního počítače. Můžete použít hello IP adresu, ale hello IP adresy mohou změnit, pokud Azure přesune prostředky pro redundanci nebo údržby. název DNS Hello budou stabilní, protože může být přesměrován tooa novou IP adresu.)  

1. V portálu Azure hello (nebo z předchozího kroku hello), vyberte **virtuálních počítačů (klasické)**.
2. Vyberte virtuální počítač SQL.
3. Na hello **virtuálního počítače** okno, kopie hello **název DNS** hello virtuálního počítače.
   
    ![Název DNS](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a>Připojit toohello databázový stroj z jiného počítače
1. Na počítači připojené toohello Internetu, otevřete SQL Server Management Studio.
2. V hello **připojit tooServer** nebo **připojit tooDatabase modul** dialogové okno, v hello **název serveru** zadejte název DNS hello hello virtuálního počítače (určený v hello předchozí úlohy) a číslo portu veřejný koncový bod ve formátu hello *DNSName, ČísloPortu* například **mysqlvm.cloudapp.net,57500**.
   
    ![Připojení pomocí SSMS](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
   
    Pokud si nepamatujete číslo portu veřejný koncový bod hello jste dříve vytvořili, ho můžete najít v hello **koncové body** oblasti hello **virtuální počítač** okno.
   
    ![Veřejný port](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. V hello **ověřování** vyberte **ověřování systému SQL Server**.
4. V hello **přihlášení** pole, název typu hello přihlašovací jméno, které jste vytvořili v rámci předchozí úlohy.
5. V hello **heslo** pole, zadejte heslo hello hello přihlášení, který vytvoříte v rámci předchozí úlohy.
6. Klikněte na **Připojit**.

