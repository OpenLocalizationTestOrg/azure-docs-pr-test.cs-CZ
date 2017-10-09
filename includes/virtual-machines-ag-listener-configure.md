naslouchací proces skupiny dostupnosti Hello je IP adresy a síťového názvu objektu který hello skupiny dostupnosti naslouchá na serveru SQL Server. toocreate hello naslouchací proces skupiny dostupnosti, hello následující:

1. <a name="getnet"></a>Získáte název hello hello clusteru síťovému prostředku.

    a. Pomocí protokolu RDP tooconnect toohello Azure virtuální počítač, který je hostitelem primární repliky hello. 

    b. Otevřete Správce clusteru převzetí služeb při selhání.

    c. Vyberte hello **sítě** uzel a název sítě s clustery Poznámka hello. Tento název použít v hello `$ClusterNetworkName` proměnné v hello skript prostředí PowerShell. Následující obrázek hello clusteru síťový název je v hello **clusteru sítě 1**:

   ![Název sítě s clustery](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <a name="addcap"></a>Přidejte hello klientský přístupový bod.  
    Hello klientský přístupový bod je název sítě hello, že aplikace používat tooconnect toohello databází ve skupině dostupnosti. Vytvoření hello klientský přístupový bod, ve Správci clusteru převzetí služeb při selhání.

    a. Rozbalte název clusteru hello a pak klikněte na tlačítko **role**.

    b. V hello **role** podokně, klikněte pravým tlačítkem na skupinu dostupnosti hello názvem a pak vyberte **přidat prostředek** > **klientský přístupový bod**.

   ![Klientský přístupový bod](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. V hello **název** pole, vytvořte název pro tento nový naslouchací proces. 
   Hello název nové naslouchací proces hello je název sítě hello, že aplikace používat tooconnect toodatabases ve skupině dostupnosti systému SQL Server hello.
   
    d. Klikněte na tlačítko toofinish vytváření hello naslouchací proces, **Další** dvakrát a potom klikněte na **Dokončit**. Nelze zobrazit naslouchací proces hello nebo prostředek do online režimu v tomto okamžiku.

3. <a name="congroup"></a>Nakonfigurujte hello IP prostředku pro skupinu dostupnosti hello.

    a. Klikněte na tlačítko hello **prostředky** kartě a potom rozbalte hello klientský přístupový bod jste vytvořili.  
    Hello klientský přístupový bod je offline.

   ![Klientský přístupový bod](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. Klikněte pravým tlačítkem na prostředek hello IP a potom klikněte na položku Vlastnosti. Poznamenejte si název hello hello IP adresy a použít ho v hello `$IPResourceName` proměnné v hello skript prostředí PowerShell.

    c. V části **IP adresu**, klikněte na tlačítko **statickou IP adresu**. Nastavit hello IP adresu jako hello stejná adresa jste použili při nastavování hello adresa služby Vyrovnávání zatížení pro hello portálu Azure.

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <a name = "dependencyGroup"></a>Vytvořte prostředek skupiny dostupnosti systému SQL Server hello závislost na hello klientský přístupový bod.

    a. Ve Správci clusteru převzetí služeb při selhání, klikněte na tlačítko **role**a potom klikněte na vaší skupiny dostupnosti.

    b. Na hello **prostředky** v části **další prostředky**, klikněte pravým tlačítkem na skupinu prostředků hello dostupnosti a pak klikněte na tlačítko **vlastnosti**. 

    c. Na kartě hello závislosti přidejte název hello hello klientský přístup k bodu (hello naslouchací proces) prostředku.

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. Klikněte na **OK**.

5. <a name="listname"></a>Usnadnění přístupu klienta hello bodu prostředků, které jsou závislé na hello IP adresu.

    a. Ve Správci clusteru převzetí služeb při selhání, klikněte na tlačítko **role**a potom klikněte na vaší skupiny dostupnosti. 

    b. Na hello **prostředky** kartě, klikněte pravým tlačítkem na hello klientský přístup k bodu prostředek pod **název serveru**a potom klikněte na **vlastnosti**. 

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Klikněte na tlačítko hello **závislosti** kartě. Ověřte, zda je adresa IP hello závislost. Pokud není, nastavte závislost na IP adresu hello. Pokud uvedený více prostředků, ověřte, že hello IP adres nebo Ne a závislosti. Klikněte na **OK**. 

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    d. Klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na **přepnout do režimu Online**. 

    >[!TIP]
    >Můžete ověřit, že hello, které jsou správně nakonfigurovány závislosti. Ve Správci clusteru převzetí služeb při selhání přejděte tooRoles, klikněte pravým tlačítkem na skupinu dostupnosti hello, klikněte na **další akce**a potom klikněte na **zobrazit sestavu závislostí**. Pokud jsou správně nakonfigurovány závislosti hello, skupina dostupnosti hello je závislá na název sítě hello a hello síťový název je závislá na IP adresu hello. 


6. <a name="setparam"></a>Nastavení parametrů clusteru hello v prostředí PowerShell.
    
    a. Zkopírujte hello následující tooone skript prostředí PowerShell vaše instancí systému SQL Server. Aktualizujte hello proměnné prostředí.     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
    $IPResourceName = "<IPResourceName>" # hello IP Address resource name
    $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    b. Nastavení parametrů clusteru hello spuštěním hello skript prostředí PowerShell na jednom z uzlů clusteru hello.  

    > [!NOTE]
    > Pokud vaše instance systému SQL Server v oblastech, musíte skript prostředí PowerShell hello toorun dvakrát. Hello poprvé, použijte hello `$ILBIP` a `$ProbePort` z první oblasti hello. Hello podruhé, použijte hello `$ILBIP` a `$ProbePort` z druhé oblasti hello. Název sítě Hello clusteru a název prostředku IP clusteru hello hello jsou stejné. 
