---
title: "Vytvoření účtu Azure Media Services pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Tento kurz vás provede kroky pro vytvoření účtu služby Azure Media Services pomocí webu Azure Portal."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: juliako
ms.openlocfilehash: 919a0b2f390bab353d24423d7f216e7031581aba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a><span data-ttu-id="b438a-103">Vytvoření účtu Azure Media Services pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b438a-103">Create an Azure Media Services account using the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b438a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b438a-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="b438a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b438a-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="b438a-106">REST</span><span class="sxs-lookup"><span data-stu-id="b438a-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="b438a-107">K dokončení tohoto kurzu potřebujete mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="b438a-107">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="b438a-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b438a-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="b438a-109">Azure Portal nabízí rychlou možnost vytvoření účtu Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="b438a-109">The Azure portal provides a way to quickly create an Azure Media Services (AMS) account.</span></span> <span data-ttu-id="b438a-110">Účet můžete použít pro přístup ke službě Media Services, která vám umožní ukládat, šifrovat, kódovat, spravovat a streamovat mediální obsah v Azure.</span><span class="sxs-lookup"><span data-stu-id="b438a-110">You can use your account to access Media Services that enable you to store, encrypt, encode, manage, and stream media content in Azure.</span></span> <span data-ttu-id="b438a-111">V okamžiku vytvoření účtu Media Services si současně vytvoříte i přidružený účet úložiště (nebo použijete existující) ve stejné zeměpisné oblasti jako účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="b438a-111">At the time you create a Media Services account, you also create an associated storage account (or use an existing one) in the same geographic region as the Media Services account.</span></span>

<span data-ttu-id="b438a-112">Tento článek popisuje některé obvyklé koncepty a ukazuje jak vytvořit účet Media Services pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b438a-112">This article explains some common concepts and shows how to create a Media Services account with the Azure portal.</span></span>

## <a name="concepts"></a><span data-ttu-id="b438a-113">Koncepty</span><span class="sxs-lookup"><span data-stu-id="b438a-113">Concepts</span></span>
<span data-ttu-id="b438a-114">Přístup ke službě Media Services vyžaduje dva přidružené účty:</span><span class="sxs-lookup"><span data-stu-id="b438a-114">Accessing Media Services requires two associated accounts:</span></span>

* <span data-ttu-id="b438a-115">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="b438a-115">A Media Services account.</span></span> <span data-ttu-id="b438a-116">Váš účet umožňuje přístup k sadě cloudových služeb Media Services, které jsou dostupné v Azure.</span><span class="sxs-lookup"><span data-stu-id="b438a-116">Your account gives you access to a set of cloud-based Media Services that are available in Azure.</span></span> <span data-ttu-id="b438a-117">Účet Media Services neukládá samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="b438a-117">A Media Services account does not store actual media content.</span></span> <span data-ttu-id="b438a-118">Místo toho na vašem účtu ukládají metadata o mediálním obsahu a úlohách zpracování médií v účtu.</span><span class="sxs-lookup"><span data-stu-id="b438a-118">Instead it stores metadata about the media content and media processing jobs in your account.</span></span> <span data-ttu-id="b438a-119">Při vytváření účtu vybíráte dostupnou oblast služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="b438a-119">At the time you create the account, you select an available Media Services region.</span></span> <span data-ttu-id="b438a-120">Oblast, kterou vyberete, je datové centrum, které ukládá záznamy metadat vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="b438a-120">The region you select is a data center that stores the metadata records for your account.</span></span>
  
* <span data-ttu-id="b438a-121">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b438a-121">An Azure storage account.</span></span> <span data-ttu-id="b438a-122">Účty úložiště pro mediální soubory musí být umístěné ve stejné zeměpisné oblasti jako účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="b438a-122">Storage accounts must be located in the same geographic region as the Media Services account.</span></span> <span data-ttu-id="b438a-123">Při vytváření účtu Media Services můžete zvolit buď stávající účet úložiště ve stejné oblasti, nebo můžete vytvořit nový účet úložiště ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b438a-123">When you create a Media Services account, you can either choose an existing storage account in the same region, or you can create a new storage account in the same region.</span></span> <span data-ttu-id="b438a-124">Pokud odstraníte účet Media Services, objekty blob v souvisejícím účtu úložiště odstraněny nebudou.</span><span class="sxs-lookup"><span data-stu-id="b438a-124">If you delete a Media Services account, the blobs in your related storage account are not deleted.</span></span>

> [!NOTE]
> <span data-ttu-id="b438a-125">Informace o dostupnosti funkcí služby Azure Media Services v různých oblastech najdete v tématu popisujícím [dostupnost funkcí AMS v datových centrech](scenarios-and-availability.md#availability).</span><span class="sxs-lookup"><span data-stu-id="b438a-125">For information about availability of Azure Media Services features in different regions, see [availability of AMS features across datacenters](scenarios-and-availability.md#availability).</span></span>

## <a name="create-an-ams-account"></a><span data-ttu-id="b438a-126">Vytvoření účtu AMS</span><span class="sxs-lookup"><span data-stu-id="b438a-126">Create an AMS account</span></span>
<span data-ttu-id="b438a-127">Postup v této části ukazuje, jak vytvořit účet AMS.</span><span class="sxs-lookup"><span data-stu-id="b438a-127">The steps in this section show how to create an AMS account.</span></span>

1. <span data-ttu-id="b438a-128">Přihlaste se na [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b438a-128">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b438a-129">Klikněte na **+Nové** > **Web + mobilní zařízení** > **Media Services**.</span><span class="sxs-lookup"><span data-stu-id="b438a-129">Click **+New** > **Web + Mobile** > **Media Services**.</span></span>
   
    ![Media Services – vytvoření](./media/media-services-create-account/media-services-new1.png)
3. <span data-ttu-id="b438a-131">V okně **VYTVOŘIT ÚČET MEDIA SERVICES** zadejte požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b438a-131">In **CREATE MEDIA SERVICES ACCOUNT** enter required values.</span></span>
   
    ![Media Services – vytvoření](./media/media-services-create-account/media-services-new3.png)
   
   1. <span data-ttu-id="b438a-133">Do pole **Název účtu** zadejte název nového účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="b438a-133">In **Account Name**, enter the name of the new AMS account.</span></span> <span data-ttu-id="b438a-134">Název účtu Media Services musí obsahovat jenom malá písmena a číslice, nesmí obsahovat mezery a musí mít délku 3 až 24 znaků.</span><span class="sxs-lookup"><span data-stu-id="b438a-134">A Media Services account name is all lowercase letters or numbers with no spaces, and is 3 to 24 characters in length.</span></span>
   2. <span data-ttu-id="b438a-135">V poli Předplatné si vyberte z různých předplatných Azure, ke kterým máte přístup.</span><span class="sxs-lookup"><span data-stu-id="b438a-135">In Subscription, select among the different Azure subscriptions that you have access to.</span></span>
   3. <span data-ttu-id="b438a-136">V poli **Skupina prostředků** vyberte nový nebo existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="b438a-136">In **Resource Group**, select the new or existing resource.</span></span>  <span data-ttu-id="b438a-137">Skupina prostředků je kolekce prostředků, které sdílejí životní cyklus, oprávnění a zásady.</span><span class="sxs-lookup"><span data-stu-id="b438a-137">A resource group is a collection of resources that share lifecycle, permissions, and policies.</span></span> <span data-ttu-id="b438a-138">Další informace najdete [tady](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="b438a-138">Learn more [here](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
   4. <span data-ttu-id="b438a-139">V poli **Umístění** vyberte zeměpisnou oblast, která se bude používat k ukládání médií a záznamů metadat pro váš účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="b438a-139">In **Location**,  select the geographic region that will be used to store the media and metadata records for your Media Services account.</span></span> <span data-ttu-id="b438a-140">Tato oblast se bude používat ke zpracování a streamování vašeho média.</span><span class="sxs-lookup"><span data-stu-id="b438a-140">This  region will be used to process and stream your media.</span></span> <span data-ttu-id="b438a-141">V rozevíracím seznamu se vám zobrazí pouze ty oblasti Media Services, které jsou dostupné.</span><span class="sxs-lookup"><span data-stu-id="b438a-141">Only the available Media Services regions appear in the drop-down list box.</span></span> 
   5. <span data-ttu-id="b438a-142">V poli **Účet úložiště** vyberte účet úložiště, který bude sloužit jako úložiště objektů blob mediálního obsahu z vašeho účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="b438a-142">In **Storage Account**, select a storage account to provide blob storage of the media content from your Media Services account.</span></span> <span data-ttu-id="b438a-143">Můžete vybrat existující účet úložiště ve stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b438a-143">You can select an existing storage account in the same geographic region as your Media Services account, or you can create a storage account.</span></span> <span data-ttu-id="b438a-144">Nový účet úložiště bude vytvořen ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b438a-144">A new storage account is created in the same region.</span></span> <span data-ttu-id="b438a-145">Pro názvy účtů úložiště platí stejná pravidla jako pro názvy účtů Media Services.</span><span class="sxs-lookup"><span data-stu-id="b438a-145">The rules for storage account names are the same as for Media Services accounts.</span></span>
      
       <span data-ttu-id="b438a-146">Další informace o ukládání a úložištích najdete [tady](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b438a-146">Learn more about storage [here](../storage/common/storage-introduction.md).</span></span>
   6. <span data-ttu-id="b438a-147">Zaškrtněte **Připnout na řídicí panel**, abyste viděli průběh nasazení účtu.</span><span class="sxs-lookup"><span data-stu-id="b438a-147">Select **Pin to dashboard** to see the progress of the account deployment.</span></span>
4. <span data-ttu-id="b438a-148">Klikněte na tlačítko **Vytvořit** dole na formuláři.</span><span class="sxs-lookup"><span data-stu-id="b438a-148">Click **Create** at the bottom of the form.</span></span>
   
    <span data-ttu-id="b438a-149">Po úspěšném vytvoření účtu se načte stránka s přehledem.</span><span class="sxs-lookup"><span data-stu-id="b438a-149">Once the account is successfully created, overview page loads.</span></span> <span data-ttu-id="b438a-150">V tabulce koncových bodů streamování bude účet mít výchozí koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="b438a-150">In the streaming endpoint table the account will have a default streaming endpoint in the **Stopped** state.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="b438a-151">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="b438a-151">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="b438a-152">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="b438a-152">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
   
## <a name="to-manage-your-ams-account"></a><span data-ttu-id="b438a-153">Správa účtu AMS</span><span class="sxs-lookup"><span data-stu-id="b438a-153">To manage your AMS account</span></span>

<span data-ttu-id="b438a-154">Pokud chcete spravovat svůj účet AMS (například připojit se prostřednictvím kódu programu k rozhraní API pro AMS, nahrát videa, kódovat prostředky, konfigurovat ochranu obsahu nebo monitorovat průběh úloh), vyberte **Nastavení** na levé straně portálu.</span><span class="sxs-lookup"><span data-stu-id="b438a-154">To manage your AMS account (for example, connect to the AMS API programmatically, upload videos, encode assets, configure content protection, monitor job progress) select **Settings** on the left side of the portal.</span></span> <span data-ttu-id="b438a-155">V **Nastavení** přejděte do některého z dostupných oken (například: **Přístup k rozhraní API**, **Prostředky**, **Úlohy** nebo **Ochrana obsahu**).</span><span class="sxs-lookup"><span data-stu-id="b438a-155">From the **Settings**, navigate to one of the available blades (for example: **API access**, **Assets**, **Jobs**, **Content protection**).</span></span>


## <a name="next-steps"></a><span data-ttu-id="b438a-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b438a-156">Next steps</span></span>

<span data-ttu-id="b438a-157">Soubory teď můžete nahrát do účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="b438a-157">You can now upload files into your AMS account.</span></span> <span data-ttu-id="b438a-158">Další informace najdete v tématu [Nahrání souborů](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="b438a-158">For more information, see [Upload files](media-services-portal-upload-files.md).</span></span>

<span data-ttu-id="b438a-159">Pokud plánujete přistupovat k rozhraní API pro AMS prostřednictvím kódu programu, přečtěte si téma [Přístup k rozhraní API pro službu Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="b438a-159">If you plan to access AMS API programmatically, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="b438a-160">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="b438a-160">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b438a-161">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="b438a-161">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

