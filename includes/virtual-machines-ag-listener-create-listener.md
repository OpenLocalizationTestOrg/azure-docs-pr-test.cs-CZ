V tomto kroku vytvoříte ručně hello naslouchací proces skupiny dostupnosti ve Správci clusteru převzetí služeb při selhání a SQL Server Management Studio.

1. Otevřete Správce clusteru převzetí služeb při selhání z hello uzlu, který je hostitelem primární repliky hello.

2. Vyberte hello **sítě** uzel a potom název sítě s clustery Poznámka hello. Tento název se používá v proměnné hello $ClusterNetworkName v hello skript prostředí PowerShell.

3. Rozbalte název clusteru hello a pak klikněte na tlačítko **role**.

4. V hello **role** podokně, klikněte pravým tlačítkem na skupinu dostupnosti hello názvem a pak vyberte **přidat prostředek** > **klientský přístupový bod**.
   
    ![Přidat klientský přístupový bod pro skupinu dostupnosti](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. V hello **název** pole, vytvořte název pro tento nový naslouchací proces, klikněte na **Další** dvakrát a potom klikněte na **Dokončit**.  
    Nelze zobrazit naslouchací proces hello nebo prostředek do online režimu v tomto okamžiku.

6. Klikněte na tlačítko hello **prostředky** kartě a potom rozbalte hello klientský přístupový bod, kterou jste právě vytvořili. 
    Zobrazí se Hello IP adresu prostředku pro každou síť s clustery v clusteru. Pokud je to řešení Azure, se zobrazí pouze jeden prostředek IP adresy.

7. Proveďte jednu z následujících hello:
   
   * tooconfigure hybridní řešení:
     
        a. Klikněte pravým tlačítkem na prostředek hello IP adresy, která odpovídá tooyour místní podsítě a potom vyberte **vlastnosti**. Poznamenejte si název hello IP adresy a síťového názvu.
   
        b. Vyberte **statickou IP adresu**, přiřadit nepoužívané IP adresu a pak klikněte na tlačítko **OK**.
 
   * tooconfigure řešení služby Azure:

        a. Klikněte pravým tlačítkem na prostředek hello IP adresy, která odpovídá tooyour podsíti Azure a potom vyberte **vlastnosti**.
       
       > [!NOTE]
       > Pokud toocome online naslouchací proces hello později selže z důvodu konfliktních IP adresa vybraná protokolem DHCP, můžete nakonfigurovat platnou statickou IP adresu v tomto okně Vlastnosti.
       > 
       > 

       b. V hello stejné **IP adresu** vlastnosti – okno, změna hello **název IP adresy**.  
        Tento název se používá v hello $IPResourceName proměnná hello skript prostředí PowerShell. Pokud vaše řešení zahrnuje více virtuálních sítí Azure, můžete tento krok opakujte pro každý prostředek IP.

