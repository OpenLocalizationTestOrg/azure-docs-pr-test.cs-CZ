1. <span data-ttu-id="a1a59-101">Ve Správci clusteru převzetí služeb při selhání rozbalte **role**a pak zvýraznit vaší skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a1a59-101">In Failover Cluster Manager, expand **Roles**, and then highlight your availability group.</span></span>  

2. <span data-ttu-id="a1a59-102">Na hello **prostředky** kartě, klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-102">On hello **Resources** tab, right-click hello listener name, and then click **Properties**.</span></span>

3. <span data-ttu-id="a1a59-103">Klikněte na tlačítko hello **závislosti** kartě. Pokud nejsou uvedeny více prostředků, ověřte, že hello IP adres nebo Ne a závislosti.</span><span class="sxs-lookup"><span data-stu-id="a1a59-103">Click hello **Dependencies** tab. If multiple resources are listed, verify that hello IP addresses have OR, not AND, dependencies.</span></span>  

4. <span data-ttu-id="a1a59-104">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-104">Click **OK**.</span></span>

5. <span data-ttu-id="a1a59-105">Klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-105">Right-click hello listener name, and then click **Bring Online**.</span></span>

6. <span data-ttu-id="a1a59-106">Po hello listener je online na hello **prostředky** kartě, klikněte pravým tlačítkem na skupinu dostupnosti hello a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-106">After hello listener is online, on hello **Resources** tab, right-click hello availability group, and then click **Properties**.</span></span>
   
    ![Konfigurace prostředek skupiny dostupnosti hello](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. <span data-ttu-id="a1a59-108">Závislost na prostředku názvu naslouchací proces hello (ne hello IP adresa zdroje názvem) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-108">Create a dependency on hello listener name resource (not hello IP address resources name), and then click **OK**.</span></span>
   
    ![Přidat závislost na název naslouchacího procesu hello](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. <span data-ttu-id="a1a59-110">Spusťte SQL Server Management Studio a připojte toohello primární repliky.</span><span class="sxs-lookup"><span data-stu-id="a1a59-110">Start SQL Server Management Studio, and then connect toohello primary replica.</span></span>

9. <span data-ttu-id="a1a59-111">Přejděte příliš**vysoké dostupnosti AlwaysOn** > **skupiny dostupnosti** > **\<AvailabilityGroupName\>**   >  **Naslouchací procesy skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-111">Go too**AlwaysOn High Availability** > **Availability Groups** > **\<AvailabilityGroupName\>** > **Availability Group Listeners**.</span></span>  
    <span data-ttu-id="a1a59-112">má být zobrazena Hello název naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="a1a59-112">hello listener name that you created in Failover Cluster Manager should be displayed.</span></span>

10. <span data-ttu-id="a1a59-113">Klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-113">Right-click hello listener name, and then click **Properties**.</span></span>

11. <span data-ttu-id="a1a59-114">V hello **Port** zadejte číslo portu naslouchacího procesu skupiny dostupnosti hello hello pomocí hello $EndpointPort dříve používaného (v tomto kurzu se 1433 výchozí hello) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1a59-114">In hello **Port** box, specify hello port number for hello availability group listener by using hello $EndpointPort that you used earlier (in this tutorial, 1433 was hello default), and then click **OK**.</span></span>

