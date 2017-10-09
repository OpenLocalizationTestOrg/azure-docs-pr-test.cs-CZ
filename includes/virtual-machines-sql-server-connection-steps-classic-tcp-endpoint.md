### <a name="create-a-tcp-endpoint-for-hello-virtual-machine"></a>Vytvoření koncového bodu protokolu TCP pro virtuální počítač hello
V pořadí tooaccess systému SQL Server z hello Internetu, hello virtuální počítač musí mít toolisten koncový bod pro příchozí komunikaci TCP. Tento krok konfigurace Azure bude směrovat příchozí TCP port provoz tooa port TCP, který je přístupný toohello virtuální počítač.

> [!NOTE]
> Pokud se připojujete v rámci hello stejné cloudové služby nebo virtuální sítě, není nutné toocreate veřejně přístupném koncovém bodu. V takovém případě může pokračovat dalším krokem toohello. Další informace najdete v tématu věnovaném [scénářům připojení](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. Na hello portálu Azure, vyberte **virtuálních počítačů (klasické)**.
2. Potom vyberte virtuální počítač SQL Server.
3. Vyberte **koncové body**a potom klikněte na hello **přidat** tlačítko hello horní části okna hello koncové body.
   
    ![Kroky pro vytvoření koncového bodu na portálu](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. Na hello **přidat koncový bod** okno, zadejte **název** například SQLEndpoint.
5. Vyberte **TCP** pro hello **protokol**.
6. V poli **Veřejný port** zadejte číslo portu, například **57500**.
7. Pro **privátní port**, zadejte naslouchající port serveru SQL Server, výchozí nastavení je příliš**1433**.
8. Klikněte na tlačítko **Ok** toocreate hello koncový bod.

