### <a name="app-service-plan"></a>Plán služby App Service
Vytvoří plán hello služby pro hostování hello webové aplikace. Zadejte název hello hello plánu prostřednictvím hello **hostingPlanName** parametr. umístění Hello hello plánu je hello používá stejné umístění pro skupinu prostředků hello. Hello cenovou úroveň a pracovní velikost jsou určené v hello **sku** a **workerSize** parametry

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

