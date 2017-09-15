<span data-ttu-id="459f6-101">V tomto kroku vytvoříte pravidlo brány firewall otevřít port testu pro koncový bod Vyrovnávání zatížení sítě (59999, jak je uvedeno výše) a jiné pravidlo otevřít port naslouchacího procesu skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="459f6-101">In this step, you create a firewall rule to open the probe port for the load-balanced endpoint (59999, as specified earlier) and another rule to open the availability group listener port.</span></span> <span data-ttu-id="459f6-102">Vzhledem k tomu, že jste vytvořili koncový bod Vyrovnávání zatížení na virtuálních počítačích, které obsahují replik skupin dostupnosti, budete muset otevřít port testu a port naslouchacího procesu na příslušných virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="459f6-102">Because you created the load-balanced endpoint on the VMs that contain availability group replicas, you need to open the probe port and the listener port on the respective VMs.</span></span>

1. <span data-ttu-id="459f6-103">Na virtuálních počítačích, které jsou hostiteli repliky, spusťte **brány Windows Firewall s pokročilým zabezpečením**.</span><span class="sxs-lookup"><span data-stu-id="459f6-103">On VMs that host replicas, start **Windows Firewall with Advanced Security**.</span></span>

2. <span data-ttu-id="459f6-104">Klikněte pravým tlačítkem na **příchozí pravidla**a potom klikněte na **nové pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="459f6-104">Right-click **Inbound Rules**, and then click **New Rule**.</span></span>

3. <span data-ttu-id="459f6-105">Na **typ pravidla** vyberte **Port**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="459f6-105">On the **Rule Type** page, select **Port**, and then click **Next**.</span></span>

4. <span data-ttu-id="459f6-106">Na **protokol a porty** vyberte **TCP**, typ **59999** v **určité místní porty** pole a pak klikněte na tlačítko  **Další**.</span><span class="sxs-lookup"><span data-stu-id="459f6-106">On the **Protocol and Ports** page, select **TCP**, type **59999** in the **Specific local ports** box, and then click **Next**.</span></span>

5. <span data-ttu-id="459f6-107">Na **akce** ponechte **povolit připojení** vybrané a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="459f6-107">On the **Action** page, keep **Allow the connection** selected, and then click **Next**.</span></span>

6. <span data-ttu-id="459f6-108">Na **profil** stránky, přijměte výchozí nastavení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="459f6-108">On the **Profile** page, accept the default settings, and then click **Next**.</span></span>

7. <span data-ttu-id="459f6-109">Na **název** stránky v **název** text zadejte název pravidla, jako například **vždy na Port naslouchacího procesu testu**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="459f6-109">On the **Name** page, in the **Name** text box, specify a rule name, such as **Always On Listener Probe Port**, and then click **Finish**.</span></span>

8. <span data-ttu-id="459f6-110">Opakujte předchozí kroky pro port naslouchacího procesu skupiny dostupnosti (uvedený dříve v parametru $EndpointPort skriptu) a pak zadejte název příslušné pravidlo, jako například **vždy na Port naslouchacího procesu**.</span><span class="sxs-lookup"><span data-stu-id="459f6-110">Repeat the preceding steps for the availability group listener port (as specified earlier in the $EndpointPort parameter of the script), and then specify an appropriate rule name, such as **Always On Listener Port**.</span></span>
