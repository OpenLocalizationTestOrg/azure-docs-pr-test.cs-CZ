## <a name="overview-of-azure-resource-manager-templates"></a><span data-ttu-id="a4420-101">Přehled šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="a4420-101">Overview of Azure Resource Manager templates</span></span>
<span data-ttu-id="a4420-102">Šablony Azure Resource Manageru umožní deklarativně zadejte infrastruktury Azure IaaS v jazyce Json definováním závislosti mezi prostředky.</span><span class="sxs-lookup"><span data-stu-id="a4420-102">Azure Resource Manager templates allow you to declaratively specify the Azure IaaS infrastructure in Json language by defining the dependencies between resources.</span></span> <span data-ttu-id="a4420-103">Podrobný přehled o šablon Azure Resource Manageru naleznete v článku níže:</span><span class="sxs-lookup"><span data-stu-id="a4420-103">For a detailed overview of Azure Resource Manager Templates, please refer to the article below:</span></span>

[<span data-ttu-id="a4420-104">Skupina prostředků – přehled</span><span class="sxs-lookup"><span data-stu-id="a4420-104">Resource Group Overview</span></span>](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a><span data-ttu-id="a4420-105">Ukázka šablony fragment kódu pro rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a4420-105">Sample template snippet for VM extensions</span></span>
<span data-ttu-id="a4420-106">Nasazení rozšíření virtuálního počítače v rámci služby Správce prostředků Azure vyžaduje šablony vám deklarativně určit konfigurace rozšíření v šabloně.</span><span class="sxs-lookup"><span data-stu-id="a4420-106">Deploying VM extensions as part of an Azure Resource Manager template requires you to declaratively specify the extension configuration in the template.</span></span>
<span data-ttu-id="a4420-107">Zde je formát pro zadání konfigurace rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a4420-107">Here is the format for specifying the extension configuration.</span></span>

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

<span data-ttu-id="a4420-108">Jak vidíte z výše uvedeného, šablona rozšíření obsahuje dvě hlavní části:</span><span class="sxs-lookup"><span data-stu-id="a4420-108">As you can see from the above, the extension template contains two main parts:</span></span>

1. <span data-ttu-id="a4420-109">Rozšíření název, vydavatel a verze</span><span class="sxs-lookup"><span data-stu-id="a4420-109">Extension name, publisher and version</span></span>
2. <span data-ttu-id="a4420-110">Konfigurace rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a4420-110">Extension Configuration.</span></span>

## <a name="identifying-the-publisher-type-and-typehandlerversion-for-any-extension"></a><span data-ttu-id="a4420-111">Identifikace vydavatele, typ a typeHandlerVersion pro jakoukoli příponu</span><span class="sxs-lookup"><span data-stu-id="a4420-111">Identifying the publisher, type, and typeHandlerVersion for any extension</span></span>
<span data-ttu-id="a4420-112">Rozšíření virtuálního počítače Azure jsou publikovány společností Microsoft a důvěryhodných vydavatelů 3. stran a každé rozšíření je jedinečně identifikovaný vydavatele, typ a typeHandlerVersion.</span><span class="sxs-lookup"><span data-stu-id="a4420-112">Azure VM extensions are published by Microsoft and trusted 3rd party publishers and each extension is uniquely identified by its publisher,type and the typeHandlerVersion.</span></span> <span data-ttu-id="a4420-113">To se dá určit jako následující:</span><span class="sxs-lookup"><span data-stu-id="a4420-113">These can be determined as following:</span></span>  

