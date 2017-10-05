---
title: "Vytvořte kontejner cloudové služby pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek vysvětluje, jak vytvořit kontejner cloudové služby pomocí prostředí PowerShell. Kontejner hostuje webové a pracovní role."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 2023fa7b318f9f76ce1e1ea0a46110297be9a001
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a><span data-ttu-id="b1104-104">Použít příkaz prostředí Azure PowerShell k vytvoření kontejner prázdný cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b1104-104">Use an Azure PowerShell command to create an empty cloud service container</span></span>
<span data-ttu-id="b1104-105">Tento článek vysvětluje, jak rychle vytvořit kontejner cloudové služby pomocí rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b1104-105">This article explains how to quickly create a Cloud Services container using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="b1104-106">Postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b1104-106">Please follow the steps below:</span></span>

1. <span data-ttu-id="b1104-107">Nainstalovat rutiny Powershellu pro Microsoft Azure z [prostředí Azure PowerShell stáhne](http://aka.ms/webpi-azps) stránky.</span><span class="sxs-lookup"><span data-stu-id="b1104-107">Install the Microsoft Azure PowerShell cmdlet from the [Azure PowerShell downloads](http://aka.ms/webpi-azps) page.</span></span>
2. <span data-ttu-id="b1104-108">Otevřete příkazový řádek prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b1104-108">Open the PowerShell command prompt.</span></span>
3. <span data-ttu-id="b1104-109">Použití [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b1104-109">Use the [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) to sign in.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b1104-110">Další pokyny k instalaci rutiny Azure Powershellu a připojení k předplatnému Azure, najdete v části [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b1104-110">For further instruction on installing the Azure PowerShell cmdlet and connecting to your Azure subscription, refer to [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
   >
   >
4. <span data-ttu-id="b1104-111">Použití **New-AzureService** vytvořte kontejner prázdný Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="b1104-111">Use the **New-AzureService** cmdlet to create an empty Azure cloud service container.</span></span>

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. <span data-ttu-id="b1104-112">Použijte tento příklad k vyvolání rutiny:</span><span class="sxs-lookup"><span data-stu-id="b1104-112">Follow this example to invoke the cmdlet:</span></span>

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

<span data-ttu-id="b1104-113">Další informace o vytváření Azure cloudové služby spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="b1104-113">For more information about creating the Azure cloud service, run:</span></span>

```
Get-help New-AzureService
```

### <a name="next-steps"></a><span data-ttu-id="b1104-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1104-114">Next steps</span></span>
* <span data-ttu-id="b1104-115">Ke správě nasazení cloudové služby, naleznete [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), a [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) příkazy.</span><span class="sxs-lookup"><span data-stu-id="b1104-115">To manage the cloud service deployment, refer to the [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), and [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) commands.</span></span> <span data-ttu-id="b1104-116">Můžete také použít odkaz na [postup konfigurace cloudové služby](cloud-services-how-to-configure.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b1104-116">You may also refer to [How to configure cloud services](cloud-services-how-to-configure.md) for further information.</span></span>
* <span data-ttu-id="b1104-117">Publikovat projekt cloudové služby Azure, najdete v tématu **PublishCloudService.ps1** ukázka kódu z [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="b1104-117">To publish your cloud service project to Azure, refer to the  **PublishCloudService.ps1** code sample from [Continuous delivery for cloud service in Azure](cloud-services-dotnet-continuous-delivery.md).</span></span>
