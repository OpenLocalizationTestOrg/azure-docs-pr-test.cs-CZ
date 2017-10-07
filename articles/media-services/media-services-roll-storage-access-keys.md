---
title: "aaaUpdate Media Services po vrácení úložiště přístupové klíče | Microsoft Docs"
description: "Tento článek získáte informace o tom, jak tooupdate Media Services po vrácení úložiště přístupové klíče."
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
ms.openlocfilehash: 26fa7a75a73397842aaebda59516a00f68ab97f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a><span data-ttu-id="3f34d-103">Po vrácení přístupových klíčů k úložišti aktualizuje Media Services</span><span class="sxs-lookup"><span data-stu-id="3f34d-103">Update Media Services after rolling storage access keys</span></span>

<span data-ttu-id="3f34d-104">Když vytvoříte nový účet Azure Media Services (AMS), zobrazí se výzva také, že tooselect Azure Storage účet, který je použit toostore mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="3f34d-104">When you create a new Azure Media Services (AMS) account, you are also asked tooselect an Azure Storage account that is used toostore your media content.</span></span> <span data-ttu-id="3f34d-105">Můžete přidat více než jeden tooyour účty úložiště účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="3f34d-105">You can add more than one storage accounts tooyour Media Services account.</span></span> <span data-ttu-id="3f34d-106">Toto téma ukazuje, jak toorotate úložiště klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f34d-106">This topic shows how toorotate storage keys.</span></span> <span data-ttu-id="3f34d-107">Také ukazuje, jak účtů úložiště tooadd účtu media tooa.</span><span class="sxs-lookup"><span data-stu-id="3f34d-107">It also shows how tooadd storage accounts tooa media account.</span></span> 

<span data-ttu-id="3f34d-108">tooperform hello akce popsané v tomto tématu, měli byste použít [rozhraní API ARM](https://docs.microsoft.com/rest/api/media/mediaservice) a [prostředí Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span><span class="sxs-lookup"><span data-stu-id="3f34d-108">tooperform hello actions described in this topic, you should be using [ARM APIs](https://docs.microsoft.com/rest/api/media/mediaservice) and [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span></span>  <span data-ttu-id="3f34d-109">Další informace najdete v tématu [jak toomanage Azure prostředků pomocí prostředí PowerShell a správce prostředků](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3f34d-109">For more information, see [How toomanage Azure resources with PowerShell and Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

## <a name="overview"></a><span data-ttu-id="3f34d-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="3f34d-110">Overview</span></span>

<span data-ttu-id="3f34d-111">Při vytvoření nového účtu úložiště vygeneruje Azure dva 512bitové přístupových klíčů k úložišti, které jsou používané tooauthenticate přístup k účtu úložiště tooyour.</span><span class="sxs-lookup"><span data-stu-id="3f34d-111">When a new storage account is created, Azure generates two 512-bit storage access keys, which are used tooauthenticate access tooyour storage account.</span></span> <span data-ttu-id="3f34d-112">tookeep připojení k úložišti bezpečnější, že se doporučuje tooperiodically znovu vygenerovat a otočit přístupový klíč k úložišti.</span><span class="sxs-lookup"><span data-stu-id="3f34d-112">tookeep your storage connections more secure, it is recommended tooperiodically regenerate and rotate your storage access key.</span></span> <span data-ttu-id="3f34d-113">Dva přístupové klíče (primární i sekundární) jsou k dispozici v pořadí tooenable jste toomaintain připojení toohello účet úložiště pomocí jednoho přístup klíč zatímco si znovu vygenerujete hello druhý přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3f34d-113">Two access keys (primary and secondary) are provided in order tooenable you toomaintain connections toohello storage account using one access key while you regenerate hello other access key.</span></span> <span data-ttu-id="3f34d-114">Tento postup je také označován "postupného přístupových klíčů".</span><span class="sxs-lookup"><span data-stu-id="3f34d-114">This procedure is also called "rolling access keys".</span></span>

<span data-ttu-id="3f34d-115">Služba Media Services, závisí na zadaný tooit klíč k úložišti.</span><span class="sxs-lookup"><span data-stu-id="3f34d-115">Media Services depends on a storage key provided tooit.</span></span> <span data-ttu-id="3f34d-116">Konkrétně hello lokátory, které jsou používané toostream nebo stáhnout vaše prostředky závisí na hello zadané úložiště přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3f34d-116">Specifically, hello locators that are used toostream or download your assets depend on hello specified storage access key.</span></span> <span data-ttu-id="3f34d-117">Při vytváření účtu AMS trvá závislost na hello primárního úložiště přístupový klíč ve výchozím nastavení ale jako uživatel, můžete aktualizovat klíč úložiště hello, který má AMS.</span><span class="sxs-lookup"><span data-stu-id="3f34d-117">When an AMS account is created it takes a dependency on hello primary storage access key by default but as a user you can update hello storage key that AMS has.</span></span> <span data-ttu-id="3f34d-118">Je nutné zkontrolujte, zda že toolet Media Services vědět, které klíče toouse podle následujících kroků popsaných v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="3f34d-118">You must make sure toolet Media Services know which key toouse by following steps described in this topic.</span></span>  

>[!NOTE]
> <span data-ttu-id="3f34d-119">Pokud máte více účtů úložiště, můžete provést tento postup se každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3f34d-119">If you have multiple storage accounts, you would perform this procedure with each storage account.</span></span> <span data-ttu-id="3f34d-120">Hello pořadí, ve kterém otočit klíčů k úložišti není pevný.</span><span class="sxs-lookup"><span data-stu-id="3f34d-120">hello order in which you rotate storage keys is not fixed.</span></span> <span data-ttu-id="3f34d-121">Můžete nejprve otočit hello sekundární klíč a pak hello primární klíč nebo naopak naopak.</span><span class="sxs-lookup"><span data-stu-id="3f34d-121">You can rotate hello secondary key first and then hello primary key or vice versa.</span></span>
>
> <span data-ttu-id="3f34d-122">Před provedením kroků popsaných v tomto tématu na produkční účtu, ujistěte se, že tootest je na předprodukční režim účtu.</span><span class="sxs-lookup"><span data-stu-id="3f34d-122">Before executing steps described in this topic on a production account, make sure tootest them on a pre-production account.</span></span>
>

## <a name="steps-toorotate-storage-keys"></a><span data-ttu-id="3f34d-123">Kroky toorotate úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="3f34d-123">Steps toorotate storage keys</span></span> 
 
 1. <span data-ttu-id="3f34d-124">Změna hello klíč účtu úložiště primární prostřednictvím rutiny prostředí powershell hello nebo [Azure](https://portal.azure.com/) portálu.</span><span class="sxs-lookup"><span data-stu-id="3f34d-124">Change hello storage account Primary key through hello powershell cmdlet or [Azure](https://portal.azure.com/) portal.</span></span>
 2. <span data-ttu-id="3f34d-125">Volání rutiny AzureRmMediaServiceStorageKeys synchronizace s odpovídající parametry tooforce média účet toopick až klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="3f34d-125">Call Sync-AzureRmMediaServiceStorageKeys cmdlet with appropriate params tooforce media account toopick up storage account keys</span></span>
 
    <span data-ttu-id="3f34d-126">Hello následující příklad ukazuje, jak toosync klíče toostorage účty.</span><span class="sxs-lookup"><span data-stu-id="3f34d-126">hello following example shows how toosync keys toostorage accounts.</span></span>
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. <span data-ttu-id="3f34d-127">Počkejte, než hodinu.</span><span class="sxs-lookup"><span data-stu-id="3f34d-127">Wait an hour or so.</span></span> <span data-ttu-id="3f34d-128">Ověřte, že hello streamování scénářů pracují.</span><span class="sxs-lookup"><span data-stu-id="3f34d-128">Verify hello streaming scenarios are working.</span></span>
 4. <span data-ttu-id="3f34d-129">Změnit sekundární klíč účtu úložiště prostřednictvím rutiny prostředí powershell hello nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f34d-129">Change storage account secondary key through hello powershell cmdlet or Azure portal.</span></span>
 5. <span data-ttu-id="3f34d-130">Volání synchronizace AzureRmMediaServiceStorageKeys prostředí powershell s odpovídající parametry tooforce média účet toopick si nových klíčů účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3f34d-130">Call Sync-AzureRmMediaServiceStorageKeys powershell with appropriate params tooforce media account toopick up new storage account keys.</span></span> 
 6. <span data-ttu-id="3f34d-131">Počkejte, než hodinu.</span><span class="sxs-lookup"><span data-stu-id="3f34d-131">Wait an hour or so.</span></span> <span data-ttu-id="3f34d-132">Ověřte, že hello streamování scénářů pracují.</span><span class="sxs-lookup"><span data-stu-id="3f34d-132">Verify hello streaming scenarios are working.</span></span>
 
### <a name="a-powershell-cmdlet-example"></a><span data-ttu-id="3f34d-133">V příkladu rutiny prostředí powershell</span><span class="sxs-lookup"><span data-stu-id="3f34d-133">A powershell cmdlet example</span></span> 

<span data-ttu-id="3f34d-134">Hello následující příklad ukazuje, jak tooget hello účtu úložiště a synchronizovat s účtem hello AMS.</span><span class="sxs-lookup"><span data-stu-id="3f34d-134">hello following example demonstrates how tooget hello storage account and sync it with hello AMS account.</span></span>

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-tooadd-storage-accounts-tooyour-ams-account"></a><span data-ttu-id="3f34d-135">Účet tooyour AMS účtů úložiště tooadd kroky</span><span class="sxs-lookup"><span data-stu-id="3f34d-135">Steps tooadd storage accounts tooyour AMS account</span></span>

<span data-ttu-id="3f34d-136">Hello následující téma ukazuje, jak účtů úložiště tooadd tooyour AMS účet: [připojit více tooa účty úložiště účtu Media Services](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="3f34d-136">hello following topic shows how tooadd storage accounts tooyour AMS account: [Attach multiple storage accounts tooa Media Services account](meda-services-managing-multiple-storage-accounts.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="3f34d-137">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="3f34d-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3f34d-138">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="3f34d-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="3f34d-139">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="3f34d-139">Acknowledgments</span></span>
<span data-ttu-id="3f34d-140">Rádi bychom znali tooacknowledge hello následující osoby podílí k vytvoření tohoto dokumentu: Cenk Dingiloglu, Gada Milán Seva Titov.</span><span class="sxs-lookup"><span data-stu-id="3f34d-140">We would like tooacknowledge hello following people who contributed towards creating this document: Cenk Dingiloglu, Milan Gada, Seva Titov.</span></span>
