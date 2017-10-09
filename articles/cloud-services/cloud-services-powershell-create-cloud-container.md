---
title: "aaaCreate kontejner cloudové služby pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek vysvětluje, jak toocreate Cloudová služba kontejneru pomocí prostředí PowerShell. kontejner Hello hostuje webové a pracovní role."
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
ms.openlocfilehash: 4c47b10b5ba1f9cc39594a45cd869bf04fcaadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-azure-powershell-command-toocreate-an-empty-cloud-service-container"></a><span data-ttu-id="4cad3-104">Použijte prostředí Azure PowerShell příkaz toocreate kontejner prázdný cloudové služby</span><span class="sxs-lookup"><span data-stu-id="4cad3-104">Use an Azure PowerShell command toocreate an empty cloud service container</span></span>
<span data-ttu-id="4cad3-105">Tento článek vysvětluje, jak tooquickly vytvořit kontejner cloudové služby pomocí rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4cad3-105">This article explains how tooquickly create a Cloud Services container using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="4cad3-106">Postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="4cad3-106">Please follow hello steps below:</span></span>

1. <span data-ttu-id="4cad3-107">Nainstalovat rutiny Azure Powershellu Microsoft hello z hello [prostředí Azure PowerShell stáhne](http://aka.ms/webpi-azps) stránky.</span><span class="sxs-lookup"><span data-stu-id="4cad3-107">Install hello Microsoft Azure PowerShell cmdlet from hello [Azure PowerShell downloads](http://aka.ms/webpi-azps) page.</span></span>
2. <span data-ttu-id="4cad3-108">Otevřete příkazový řádek Powershellu hello.</span><span class="sxs-lookup"><span data-stu-id="4cad3-108">Open hello PowerShell command prompt.</span></span>
3. <span data-ttu-id="4cad3-109">Použití hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) toosign v.</span><span class="sxs-lookup"><span data-stu-id="4cad3-109">Use hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) toosign in.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4cad3-110">Další pokyny k instalaci rutiny Azure Powershellu hello a připojení tooyour předplatné Azure, najdete v části příliš[jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4cad3-110">For further instruction on installing hello Azure PowerShell cmdlet and connecting tooyour Azure subscription, refer too[How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
   >
   >
4. <span data-ttu-id="4cad3-111">Použití hello **New-AzureService** rutiny toocreate kontejner prázdný Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="4cad3-111">Use hello **New-AzureService** cmdlet toocreate an empty Azure cloud service container.</span></span>

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. <span data-ttu-id="4cad3-112">Použijte tuto ukázkovou rutinu tooinvoke hello:</span><span class="sxs-lookup"><span data-stu-id="4cad3-112">Follow this example tooinvoke hello cmdlet:</span></span>

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

<span data-ttu-id="4cad3-113">Další informace o vytváření hello cloudové služby Azure spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="4cad3-113">For more information about creating hello Azure cloud service, run:</span></span>

```
Get-help New-AzureService
```

### <a name="next-steps"></a><span data-ttu-id="4cad3-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cad3-114">Next steps</span></span>
* <span data-ttu-id="4cad3-115">toomanage hello cloudové nasazení služby, získáte toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), a [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) příkazy.</span><span class="sxs-lookup"><span data-stu-id="4cad3-115">toomanage hello cloud service deployment, refer toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), and [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) commands.</span></span> <span data-ttu-id="4cad3-116">Můžete také použít příliš[jak tooconfigure cloudových služeb](cloud-services-how-to-configure.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4cad3-116">You may also refer too[How tooconfigure cloud services](cloud-services-how-to-configure.md) for further information.</span></span>
* <span data-ttu-id="4cad3-117">toopublish cloudové služby projektu tooAzure získáte toohello **PublishCloudService.ps1** ukázka kódu z [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="4cad3-117">toopublish your cloud service project tooAzure, refer toohello  **PublishCloudService.ps1** code sample from [Continuous delivery for cloud service in Azure](cloud-services-dotnet-continuous-delivery.md).</span></span>
