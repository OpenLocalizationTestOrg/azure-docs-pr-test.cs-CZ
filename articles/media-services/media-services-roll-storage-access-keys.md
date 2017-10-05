---
title: "Po vrácení přístupových klíčů k úložišti aktualizuje Media Services | Microsoft Docs"
description: "Tento článek poskytují pokyny o tom, jak aktualizovat Media Services po vrácení přístupových klíčů k úložišti."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 304e72e0d2d4a7e95df513e6d5481def9eae3f68
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a><span data-ttu-id="07ddf-103">Po vrácení přístupových klíčů k úložišti aktualizuje Media Services</span><span class="sxs-lookup"><span data-stu-id="07ddf-103">Update Media Services after rolling storage access keys</span></span>

<span data-ttu-id="07ddf-104">Když vytvoříte nový účet Azure Media Services (AMS), zobrazí se výzva, vyberte účet úložiště Azure, který se používá k ukládání mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="07ddf-104">When you create a new Azure Media Services (AMS) account, you are also asked to select an Azure Storage account that is used to store your media content.</span></span> <span data-ttu-id="07ddf-105">Můžete přidat více než jeden účty úložiště pro váš účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="07ddf-105">You can add more than one storage accounts to your Media Services account.</span></span> <span data-ttu-id="07ddf-106">Toto téma ukazuje, jak otočit klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="07ddf-106">This topic shows how to rotate storage keys.</span></span> <span data-ttu-id="07ddf-107">Také ukazuje, jak přidat účty úložiště k účtu media.</span><span class="sxs-lookup"><span data-stu-id="07ddf-107">It also shows how to add storage accounts to a media account.</span></span> 

<span data-ttu-id="07ddf-108">K provedení akce popsané v tomto tématu, měli byste použít [rozhraní API ARM](https://docs.microsoft.com/rest/api/media/mediaservice) a [prostředí Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span><span class="sxs-lookup"><span data-stu-id="07ddf-108">To perform the actions described in this topic, you should be using [ARM APIs](https://docs.microsoft.com/rest/api/media/mediaservice) and [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span></span>  <span data-ttu-id="07ddf-109">Další informace najdete v tématu [jak ke správě prostředků Azure pomocí prostředí PowerShell a správce prostředků](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="07ddf-109">For more information, see [How to manage Azure resources with PowerShell and Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

## <a name="overview"></a><span data-ttu-id="07ddf-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="07ddf-110">Overview</span></span>

<span data-ttu-id="07ddf-111">Při vytvoření nového účtu úložiště vygeneruje Azure dva 512bitové přístupové klíče k úložišti, které se používají k ověření přístupu k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="07ddf-111">When a new storage account is created, Azure generates two 512-bit storage access keys, which are used to authenticate access to your storage account.</span></span> <span data-ttu-id="07ddf-112">K lepšímu zabezpečení připojení k úložišti, doporučujeme pravidelně znovu vygenerovat a otočit přístupový klíč k úložišti.</span><span class="sxs-lookup"><span data-stu-id="07ddf-112">To keep your storage connections more secure, it is recommended to periodically regenerate and rotate your storage access key.</span></span> <span data-ttu-id="07ddf-113">Chcete-li povolit, abyste mohli udržovat připojení k účtu úložiště používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč jsou k dispozici dva přístupové klíče (primární i sekundární).</span><span class="sxs-lookup"><span data-stu-id="07ddf-113">Two access keys (primary and secondary) are provided in order to enable you to maintain connections to the storage account using one access key while you regenerate the other access key.</span></span> <span data-ttu-id="07ddf-114">Tento postup je také označován "postupného přístupových klíčů".</span><span class="sxs-lookup"><span data-stu-id="07ddf-114">This procedure is also called "rolling access keys".</span></span>

<span data-ttu-id="07ddf-115">Služba Media Services, závisí na klíč úložiště, který mu je poskytnut.</span><span class="sxs-lookup"><span data-stu-id="07ddf-115">Media Services depends on a storage key provided to it.</span></span> <span data-ttu-id="07ddf-116">Konkrétně lokátory, které se používají ke streamování nebo stažení vaše prostředky závisí na přístupový klíč zadaný úložiště.</span><span class="sxs-lookup"><span data-stu-id="07ddf-116">Specifically, the locators that are used to stream or download your assets depend on the specified storage access key.</span></span> <span data-ttu-id="07ddf-117">Při vytváření účtu AMS trvá závislost na přístupový klíč primárního úložiště ve výchozím nastavení ale jako uživatel, můžete aktualizovat klíč úložiště, který má AMS.</span><span class="sxs-lookup"><span data-stu-id="07ddf-117">When an AMS account is created it takes a dependency on the primary storage access key by default but as a user you can update the storage key that AMS has.</span></span> <span data-ttu-id="07ddf-118">Musí se ujistěte, že se chcete, aby služba Media Services vědět, které klíč pro použití podle následujících kroků popsaných v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="07ddf-118">You must make sure to let Media Services know which key to use by following steps described in this topic.</span></span>  

>[!NOTE]
> <span data-ttu-id="07ddf-119">Pokud máte více účtů úložiště, můžete provést tento postup se každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="07ddf-119">If you have multiple storage accounts, you would perform this procedure with each storage account.</span></span> <span data-ttu-id="07ddf-120">Pořadí, ve kterém otočit klíčů k úložišti není pevný.</span><span class="sxs-lookup"><span data-stu-id="07ddf-120">The order in which you rotate storage keys is not fixed.</span></span> <span data-ttu-id="07ddf-121">Otočit sekundární klíče první a pak primární klíč nebo naopak naopak.</span><span class="sxs-lookup"><span data-stu-id="07ddf-121">You can rotate the secondary key first and then the primary key or vice versa.</span></span>
>
> <span data-ttu-id="07ddf-122">Před provedením kroků popsaných v tomto tématu na produkční účet, nezapomeňte otestovat v předprodukční účtu.</span><span class="sxs-lookup"><span data-stu-id="07ddf-122">Before executing steps described in this topic on a production account, make sure to test them on a pre-production account.</span></span>
>

## <a name="steps-to-rotate-storage-keys"></a><span data-ttu-id="07ddf-123">Postup střídání úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="07ddf-123">Steps to rotate storage keys</span></span> 
 
 1. <span data-ttu-id="07ddf-124">Změnit primární klíč účtu úložiště prostřednictvím rutiny prostředí powershell nebo [Azure](https://portal.azure.com/) portálu.</span><span class="sxs-lookup"><span data-stu-id="07ddf-124">Change the storage account Primary key through the powershell cmdlet or [Azure](https://portal.azure.com/) portal.</span></span>
 2. <span data-ttu-id="07ddf-125">Volání rutiny AzureRmMediaServiceStorageKeys synchronizace s odpovídající parametry vynutit účtu media a pokračovat tam, klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="07ddf-125">Call Sync-AzureRmMediaServiceStorageKeys cmdlet with appropriate params to force media account to pick up storage account keys</span></span>
 
    <span data-ttu-id="07ddf-126">Následující příklad ukazuje, jak synchronizovat klíčů na účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="07ddf-126">The following example shows how to sync keys to storage accounts.</span></span>
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. <span data-ttu-id="07ddf-127">Počkejte, než hodinu.</span><span class="sxs-lookup"><span data-stu-id="07ddf-127">Wait an hour or so.</span></span> <span data-ttu-id="07ddf-128">Ověřte, že streamování scénářů pracují.</span><span class="sxs-lookup"><span data-stu-id="07ddf-128">Verify the streaming scenarios are working.</span></span>
 4. <span data-ttu-id="07ddf-129">Změnit sekundární klíč účtu úložiště prostřednictvím rutiny prostředí powershell nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="07ddf-129">Change storage account secondary key through the powershell cmdlet or Azure portal.</span></span>
 5. <span data-ttu-id="07ddf-130">Volání synchronizace AzureRmMediaServiceStorageKeys prostředí powershell s odpovídající parametry vynutit účtu media zachycení nových klíčů účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="07ddf-130">Call Sync-AzureRmMediaServiceStorageKeys powershell with appropriate params to force media account to pick up new storage account keys.</span></span> 
 6. <span data-ttu-id="07ddf-131">Počkejte, než hodinu.</span><span class="sxs-lookup"><span data-stu-id="07ddf-131">Wait an hour or so.</span></span> <span data-ttu-id="07ddf-132">Ověřte, že streamování scénářů pracují.</span><span class="sxs-lookup"><span data-stu-id="07ddf-132">Verify the streaming scenarios are working.</span></span>
 
### <a name="a-powershell-cmdlet-example"></a><span data-ttu-id="07ddf-133">V příkladu rutiny prostředí powershell</span><span class="sxs-lookup"><span data-stu-id="07ddf-133">A powershell cmdlet example</span></span> 

<span data-ttu-id="07ddf-134">Následující příklad ukazuje, jak získat účet úložiště a synchronizovat s účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="07ddf-134">The following example demonstrates how to get the storage account and sync it with the AMS account.</span></span>

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a><span data-ttu-id="07ddf-135">Postup pro přidání do vašeho účtu AMS účty úložiště</span><span class="sxs-lookup"><span data-stu-id="07ddf-135">Steps to add storage accounts to your AMS account</span></span>

<span data-ttu-id="07ddf-136">Následující téma ukazuje, jak přidat účty úložiště do vašeho účtu AMS: [k účtu Media Services připojit více účtů úložiště](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="07ddf-136">The following topic shows how to add storage accounts to your AMS account: [Attach multiple storage accounts to a Media Services account](meda-services-managing-multiple-storage-accounts.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="07ddf-137">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="07ddf-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="07ddf-138">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="07ddf-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="07ddf-139">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="07ddf-139">Acknowledgments</span></span>
<span data-ttu-id="07ddf-140">Rádi bychom se na vědomí následující osob, které podílí k vytvoření tohoto dokumentu: Cenk Dingiloglu, Gada Milán Seva Titov.</span><span class="sxs-lookup"><span data-stu-id="07ddf-140">We would like to acknowledge the following people who contributed towards creating this document: Cenk Dingiloglu, Milan Gada, Seva Titov.</span></span>
