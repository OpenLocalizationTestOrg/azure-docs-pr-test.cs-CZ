1. Ve Správci clusteru převzetí služeb při selhání rozbalte **role**a pak zvýraznit vaší skupiny dostupnosti.  

2. Na hello **prostředky** kartě, klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na tlačítko **vlastnosti**.

3. Klikněte na tlačítko hello **závislosti** kartě. Pokud nejsou uvedeny více prostředků, ověřte, že hello IP adres nebo Ne a závislosti.  

4. Klikněte na **OK**.

5. Klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na **přepnout do režimu Online**.

6. Po hello listener je online na hello **prostředky** kartě, klikněte pravým tlačítkem na skupinu dostupnosti hello a pak klikněte na tlačítko **vlastnosti**.
   
    ![Konfigurace prostředek skupiny dostupnosti hello](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Závislost na prostředku názvu naslouchací proces hello (ne hello IP adresa zdroje názvem) a potom klikněte na **OK**.
   
    ![Přidat závislost na název naslouchacího procesu hello](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Spusťte SQL Server Management Studio a připojte toohello primární repliky.

9. Přejděte příliš**vysoké dostupnosti AlwaysOn** > **skupiny dostupnosti** > **\<AvailabilityGroupName\>**   >  **Naslouchací procesy skupiny dostupnosti**.  
    má být zobrazena Hello název naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání.

10. Klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na **vlastnosti**.

11. V hello **Port** zadejte číslo portu naslouchacího procesu skupiny dostupnosti hello hello pomocí hello $EndpointPort dříve používaného (v tomto kurzu se 1433 výchozí hello) a potom klikněte na **OK**.

