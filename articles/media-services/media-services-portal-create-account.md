---
title: "aaaCreate účtu Azure Media Services s hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello k vytvoření účtu Azure Media Services s hello portálu Azure."
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
ms.openlocfilehash: fdad93d5d470fc08380670ec0f6c2d33468b1492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-media-services-account-using-hello-azure-portal"></a><span data-ttu-id="f3020-103">Vytvoření účtu Azure Media Services pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f3020-103">Create an Azure Media Services account using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f3020-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f3020-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="f3020-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f3020-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="f3020-106">REST</span><span class="sxs-lookup"><span data-stu-id="f3020-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="f3020-107">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f3020-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="f3020-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f3020-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="f3020-109">Hello portál Azure poskytuje způsob, jak tooquickly vytvoření účtu Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="f3020-109">hello Azure portal provides a way tooquickly create an Azure Media Services (AMS) account.</span></span> <span data-ttu-id="f3020-110">Můžete použít tooaccess váš účet Media Services, který toostore můžete povolit, šifrovat, kódovat, spravovat a Streamovat mediální obsah v Azure.</span><span class="sxs-lookup"><span data-stu-id="f3020-110">You can use your account tooaccess Media Services that enable you toostore, encrypt, encode, manage, and stream media content in Azure.</span></span> <span data-ttu-id="f3020-111">V době hello vytvořit účet Media Services, můžete také vytvořit přidružený účet úložiště (nebo použijte existující) v hello stejné zeměpisné oblasti jako hello účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3020-111">At hello time you create a Media Services account, you also create an associated storage account (or use an existing one) in hello same geographic region as hello Media Services account.</span></span>

<span data-ttu-id="f3020-112">Tento článek popisuje některé běžné koncepty a ukazuje, jak toocreate Media Services účet s hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f3020-112">This article explains some common concepts and shows how toocreate a Media Services account with hello Azure portal.</span></span>

## <a name="concepts"></a><span data-ttu-id="f3020-113">Koncepty</span><span class="sxs-lookup"><span data-stu-id="f3020-113">Concepts</span></span>
<span data-ttu-id="f3020-114">Přístup ke službě Media Services vyžaduje dva přidružené účty:</span><span class="sxs-lookup"><span data-stu-id="f3020-114">Accessing Media Services requires two associated accounts:</span></span>

* <span data-ttu-id="f3020-115">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3020-115">A Media Services account.</span></span> <span data-ttu-id="f3020-116">Váš účet poskytuje přístup tooa sadu cloudové služby Media Services, které jsou k dispozici v Azure.</span><span class="sxs-lookup"><span data-stu-id="f3020-116">Your account gives you access tooa set of cloud-based Media Services that are available in Azure.</span></span> <span data-ttu-id="f3020-117">Účet Media Services neukládá samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="f3020-117">A Media Services account does not store actual media content.</span></span> <span data-ttu-id="f3020-118">Místo toho ukládají metadata o hello média obsahu a úlohách zpracování médií v účtu.</span><span class="sxs-lookup"><span data-stu-id="f3020-118">Instead it stores metadata about hello media content and media processing jobs in your account.</span></span> <span data-ttu-id="f3020-119">V době hello vytvoříte účet hello vyberte oblast služby Media Services k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f3020-119">At hello time you create hello account, you select an available Media Services region.</span></span> <span data-ttu-id="f3020-120">datové centrum, které ukládá hello záznamů metadat pro váš účet, je Hello oblast, kterou vyberete.</span><span class="sxs-lookup"><span data-stu-id="f3020-120">hello region you select is a data center that stores hello metadata records for your account.</span></span>
  
* <span data-ttu-id="f3020-121">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f3020-121">An Azure storage account.</span></span> <span data-ttu-id="f3020-122">Účty úložiště se musí nacházet v hello stejné zeměpisné oblasti jako hello účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3020-122">Storage accounts must be located in hello same geographic region as hello Media Services account.</span></span> <span data-ttu-id="f3020-123">Při vytváření účtu Media Services můžete zvolit buď stávající účet úložiště v hello stejné oblasti, nebo můžete vytvořit nový účet úložiště v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="f3020-123">When you create a Media Services account, you can either choose an existing storage account in hello same region, or you can create a new storage account in hello same region.</span></span> <span data-ttu-id="f3020-124">Pokud odstraníte účet Media Services, nebudou odstraněny hello objektů BLOB v souvisejícím účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f3020-124">If you delete a Media Services account, hello blobs in your related storage account are not deleted.</span></span>

> [!NOTE]
> <span data-ttu-id="f3020-125">Informace o dostupnosti funkcí služby Azure Media Services v různých oblastech najdete v tématu popisujícím [dostupnost funkcí AMS v datových centrech](scenarios-and-availability.md#availability).</span><span class="sxs-lookup"><span data-stu-id="f3020-125">For information about availability of Azure Media Services features in different regions, see [availability of AMS features across datacenters](scenarios-and-availability.md#availability).</span></span>

## <a name="create-an-ams-account"></a><span data-ttu-id="f3020-126">Vytvoření účtu AMS</span><span class="sxs-lookup"><span data-stu-id="f3020-126">Create an AMS account</span></span>
<span data-ttu-id="f3020-127">Hello kroků v tomto tématu zobrazit jak toocreate AMS účtu.</span><span class="sxs-lookup"><span data-stu-id="f3020-127">hello steps in this section show how toocreate an AMS account.</span></span>

1. <span data-ttu-id="f3020-128">Přihlaste se na hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f3020-128">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f3020-129">Klikněte na **+Nové** > **Web + mobilní zařízení** > **Media Services**.</span><span class="sxs-lookup"><span data-stu-id="f3020-129">Click **+New** > **Web + Mobile** > **Media Services**.</span></span>
   
    ![Media Services – vytvoření](./media/media-services-create-account/media-services-new1.png)
3. <span data-ttu-id="f3020-131">V okně **VYTVOŘIT ÚČET MEDIA SERVICES** zadejte požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f3020-131">In **CREATE MEDIA SERVICES ACCOUNT** enter required values.</span></span>
   
    ![Media Services – vytvoření](./media/media-services-create-account/media-services-new3.png)
   
   1. <span data-ttu-id="f3020-133">V **název účtu**, zadejte název nového účtu AMS hello hello.</span><span class="sxs-lookup"><span data-stu-id="f3020-133">In **Account Name**, enter hello name of hello new AMS account.</span></span> <span data-ttu-id="f3020-134">Název účtu Media Services je všechny malých písmen nebo číslic bez mezer a je 3 too24 znaků.</span><span class="sxs-lookup"><span data-stu-id="f3020-134">A Media Services account name is all lowercase letters or numbers with no spaces, and is 3 too24 characters in length.</span></span>
   2. <span data-ttu-id="f3020-135">V odběru vyberte mezi hello různých předplatných Azure, které máte přístup.</span><span class="sxs-lookup"><span data-stu-id="f3020-135">In Subscription, select among hello different Azure subscriptions that you have access to.</span></span>
   3. <span data-ttu-id="f3020-136">V **skupiny prostředků**, vyberte hello nový nebo existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="f3020-136">In **Resource Group**, select hello new or existing resource.</span></span>  <span data-ttu-id="f3020-137">Skupina prostředků je kolekce prostředků, které sdílejí životní cyklus, oprávnění a zásady.</span><span class="sxs-lookup"><span data-stu-id="f3020-137">A resource group is a collection of resources that share lifecycle, permissions, and policies.</span></span> <span data-ttu-id="f3020-138">Další informace najdete [tady](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="f3020-138">Learn more [here](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
   4. <span data-ttu-id="f3020-139">V **umístění**, hello vyberte zeměpisnou oblast, která bude použité toostore hello médií a záznamů metadat pro váš účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3020-139">In **Location**,  select hello geographic region that will be used toostore hello media and metadata records for your Media Services account.</span></span> <span data-ttu-id="f3020-140">Tato oblast bude použité tooprocess a vysílání datových proudů.</span><span class="sxs-lookup"><span data-stu-id="f3020-140">This  region will be used tooprocess and stream your media.</span></span> <span data-ttu-id="f3020-141">V rozevíracím seznamu hello se zobrazí pouze hello dostupné oblasti Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3020-141">Only hello available Media Services regions appear in hello drop-down list box.</span></span> 
   5. <span data-ttu-id="f3020-142">V **účet úložiště**, vyberte úložiště objektů blob tooprovide účet úložiště hello mediálního obsahu z vašeho účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3020-142">In **Storage Account**, select a storage account tooprovide blob storage of hello media content from your Media Services account.</span></span> <span data-ttu-id="f3020-143">Můžete vybrat existující účet úložiště v hello stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f3020-143">You can select an existing storage account in hello same geographic region as your Media Services account, or you can create a storage account.</span></span> <span data-ttu-id="f3020-144">Nový účet úložiště je vytvořen v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="f3020-144">A new storage account is created in hello same region.</span></span> <span data-ttu-id="f3020-145">Hello pravidla pro účet úložiště, že jsou názvy hello stejné jako u účtů Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3020-145">hello rules for storage account names are hello same as for Media Services accounts.</span></span>
      
       <span data-ttu-id="f3020-146">Další informace o ukládání a úložištích najdete [tady](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f3020-146">Learn more about storage [here](../storage/common/storage-introduction.md).</span></span>
   6. <span data-ttu-id="f3020-147">Vyberte **Pin toodashboard** toosee hello průběh nasazení účtu hello.</span><span class="sxs-lookup"><span data-stu-id="f3020-147">Select **Pin toodashboard** toosee hello progress of hello account deployment.</span></span>
4. <span data-ttu-id="f3020-148">Klikněte na tlačítko **vytvořit** v hello dolní části formuláře hello.</span><span class="sxs-lookup"><span data-stu-id="f3020-148">Click **Create** at hello bottom of hello form.</span></span>
   
    <span data-ttu-id="f3020-149">Po úspěšném vytvoření účtu hello načtení stránky přehled.</span><span class="sxs-lookup"><span data-stu-id="f3020-149">Once hello account is successfully created, overview page loads.</span></span> <span data-ttu-id="f3020-150">V hello streamování koncový bod tabulky hello účet bude mít výchozí koncový bod v hello streamování **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="f3020-150">In hello streaming endpoint table hello account will have a default streaming endpoint in hello **Stopped** state.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="f3020-151">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="f3020-151">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="f3020-152">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="f3020-152">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
   
## <a name="toomanage-your-ams-account"></a><span data-ttu-id="f3020-153">toomanage vašeho účtu AMS</span><span class="sxs-lookup"><span data-stu-id="f3020-153">toomanage your AMS account</span></span>

<span data-ttu-id="f3020-154">toomanage vašeho účtu AMS (například připojení toohello AMS rozhraní API prostřednictvím kódu programu, nahrávání videí, kódování assetů, konfigurace ochrany obsahu a sledovat průběh úlohy) vyberte **nastavení** na levé straně hello portálu hello.</span><span class="sxs-lookup"><span data-stu-id="f3020-154">toomanage your AMS account (for example, connect toohello AMS API programmatically, upload videos, encode assets, configure content protection, monitor job progress) select **Settings** on hello left side of hello portal.</span></span> <span data-ttu-id="f3020-155">Z hello **nastavení**, přejděte tooone hello k dispozici oken (například: **přístup pomocí rozhraní API**, **prostředky**, **úlohy**, **Obsahu ochrany**).</span><span class="sxs-lookup"><span data-stu-id="f3020-155">From hello **Settings**, navigate tooone of hello available blades (for example: **API access**, **Assets**, **Jobs**, **Content protection**).</span></span>


## <a name="next-steps"></a><span data-ttu-id="f3020-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3020-156">Next steps</span></span>

<span data-ttu-id="f3020-157">Soubory teď můžete nahrát do účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="f3020-157">You can now upload files into your AMS account.</span></span> <span data-ttu-id="f3020-158">Další informace najdete v tématu [Nahrání souborů](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="f3020-158">For more information, see [Upload files](media-services-portal-upload-files.md).</span></span>

<span data-ttu-id="f3020-159">Pokud máte v plánu prostřednictvím kódu programu tooaccess AMS rozhraní API, přečtěte si téma [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="f3020-159">If you plan tooaccess AMS API programmatically, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="f3020-160">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="f3020-160">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f3020-161">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f3020-161">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

