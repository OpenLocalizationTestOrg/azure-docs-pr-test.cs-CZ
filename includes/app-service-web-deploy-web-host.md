### <a name="app-service-plan"></a><span data-ttu-id="a64bf-101">Plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="a64bf-101">App Service plan</span></span>
<span data-ttu-id="a64bf-102">Vytvoří plán hello služby pro hostování hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a64bf-102">Creates hello service plan for hosting hello web app.</span></span> <span data-ttu-id="a64bf-103">Zadejte název hello hello plánu prostřednictvím hello **hostingPlanName** parametr.</span><span class="sxs-lookup"><span data-stu-id="a64bf-103">You provide hello name of hello plan through hello **hostingPlanName** parameter.</span></span> <span data-ttu-id="a64bf-104">umístění Hello hello plánu je hello používá stejné umístění pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="a64bf-104">hello location of hello plan is hello same location used for hello resource group.</span></span> <span data-ttu-id="a64bf-105">Hello cenovou úroveň a pracovní velikost jsou určené v hello **sku** a **workerSize** parametry</span><span class="sxs-lookup"><span data-stu-id="a64bf-105">hello pricing tier and worker size are specified in hello **sku** and **workerSize** parameters</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

