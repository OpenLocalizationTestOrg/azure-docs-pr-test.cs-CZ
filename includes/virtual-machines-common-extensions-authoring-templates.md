## <a name="overview-of-azure-resource-manager-templates"></a><span data-ttu-id="ae1b3-101">Přehled šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ae1b3-101">Overview of Azure Resource Manager templates</span></span>
<span data-ttu-id="ae1b3-102">Šablony Azure Resource Manager umožňují toodeclaratively zadejte hello infrastruktury Azure IaaS v jazyce Json definováním hello závislosti mezi prostředky.</span><span class="sxs-lookup"><span data-stu-id="ae1b3-102">Azure Resource Manager templates allow you toodeclaratively specify hello Azure IaaS infrastructure in Json language by defining hello dependencies between resources.</span></span> <span data-ttu-id="ae1b3-103">Podrobný přehled o šablon Azure Resource Manageru najdete v článku toohello níže:</span><span class="sxs-lookup"><span data-stu-id="ae1b3-103">For a detailed overview of Azure Resource Manager Templates, please refer toohello article below:</span></span>

[<span data-ttu-id="ae1b3-104">Skupina prostředků – přehled</span><span class="sxs-lookup"><span data-stu-id="ae1b3-104">Resource Group Overview</span></span>](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a><span data-ttu-id="ae1b3-105">Ukázka šablony fragment kódu pro rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ae1b3-105">Sample template snippet for VM extensions</span></span>
<span data-ttu-id="ae1b3-106">Nasazení rozšíření virtuálního počítače jako součást šablonu Azure Resource Manager, musíte zadat toodeclaratively konfigurace rozšíření hello v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="ae1b3-106">Deploying VM extensions as part of an Azure Resource Manager template requires you toodeclaratively specify hello extension configuration in hello template.</span></span>
<span data-ttu-id="ae1b3-107">Zde je hello formát pro zadání konfigurace rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="ae1b3-107">Here is hello format for specifying hello extension configuration.</span></span>

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

<span data-ttu-id="ae1b3-108">Jak je vidět z výše uvedených hello hello rozšíření šablona obsahuje dvě hlavní části:</span><span class="sxs-lookup"><span data-stu-id="ae1b3-108">As you can see from hello above, hello extension template contains two main parts:</span></span>

1. <span data-ttu-id="ae1b3-109">Rozšíření název, vydavatel a verze</span><span class="sxs-lookup"><span data-stu-id="ae1b3-109">Extension name, publisher and version</span></span>
2. <span data-ttu-id="ae1b3-110">Konfigurace rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ae1b3-110">Extension Configuration.</span></span>

## <a name="identifying-hello-publisher-type-and-typehandlerversion-for-any-extension"></a><span data-ttu-id="ae1b3-111">Identifikace hello vydavatele, typ a typeHandlerVersion pro jakoukoli příponu</span><span class="sxs-lookup"><span data-stu-id="ae1b3-111">Identifying hello publisher, type, and typeHandlerVersion for any extension</span></span>
<span data-ttu-id="ae1b3-112">Rozšíření virtuálního počítače Azure jsou publikovány společností Microsoft a důvěryhodných vydavatelů 3. stran a každé rozšíření je jedinečně identifikovaný typeHandlerVersion jeho vydavatele, typ a hello.</span><span class="sxs-lookup"><span data-stu-id="ae1b3-112">Azure VM extensions are published by Microsoft and trusted 3rd party publishers and each extension is uniquely identified by its publisher,type and hello typeHandlerVersion.</span></span> <span data-ttu-id="ae1b3-113">To se dá určit jako následující:</span><span class="sxs-lookup"><span data-stu-id="ae1b3-113">These can be determined as following:</span></span>  

