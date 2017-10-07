---
title: "aaaIntegrate účet úložiště Azure s Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure sítě pro doručování obsahu (CDN) toodeliver širokopásmového obsahu ukládání do mezipaměti objektů blob z úložiště Azure."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e44716969d6a784265cc4b1da34f0d021a17b38d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a><span data-ttu-id="645d1-103">Účet úložiště Azure integrovat Azure CDN</span><span class="sxs-lookup"><span data-stu-id="645d1-103">Integrate an Azure storage account with Azure CDN</span></span>
<span data-ttu-id="645d1-104">CDN může být povoleno toocache obsahu z úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="645d1-104">CDN can be enabled toocache content from your Azure storage.</span></span> <span data-ttu-id="645d1-105">Nabízí vývojářům globální řešení pro doručování širokopásmového obsahu pomocí ukládání do mezipaměti objektů BLOB a statického obsahu výpočetní instance na fyzických uzlech hello USA, Evropa, Asie, Austrálie a Jižní Ameriky.</span><span class="sxs-lookup"><span data-stu-id="645d1-105">It offers developers a global solution for delivering high-bandwidth content by caching blobs and static content of compute instances at physical nodes in hello United States, Europe, Asia, Australia and South America.</span></span>

## <a name="step-1-create-a-storage-account"></a><span data-ttu-id="645d1-106">Krok 1: Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="645d1-106">Step 1: Create a storage account</span></span>
<span data-ttu-id="645d1-107">Použijte následující postup toocreate nový účet úložiště pro předplatné Azure hello.</span><span class="sxs-lookup"><span data-stu-id="645d1-107">Use hello following procedure toocreate a new storage account for a Azure subscription.</span></span> <span data-ttu-id="645d1-108">Účet úložiště poskytuje přístup ke službám úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="645d1-108">A storage account gives access to Azure storage services.</span></span> <span data-ttu-id="645d1-109">účet úložiště Hello představuje nejvyšší úroveň hello hello oboru názvů pro přístup k jednotlivých součástí služby úložiště Azure hello: objektu Blob služby, služba fronty a tabulky služby.</span><span class="sxs-lookup"><span data-stu-id="645d1-109">hello storage account represents hello highest level of hello namespace for accessing each of hello Azure storage service components: Blob services, Queue services, and Table services.</span></span> <span data-ttu-id="645d1-110">Další informace najdete v části toohello [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="645d1-110">For more information, refer toohello [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span></span>

<span data-ttu-id="645d1-111">toocreate účet úložiště, musí být buď správce služby hello správce nebo spolusprávce pro hello přidružené předplatné.</span><span class="sxs-lookup"><span data-stu-id="645d1-111">toocreate a storage account, you must be either hello service administrator or a co-administrator for hello associated subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="645d1-112">Existuje několik metod, které můžete použít toocreate účet úložiště, včetně hello portálu Azure a prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="645d1-112">There are several methods you can use toocreate a storage account, including hello Azure Portal and Powershell.</span></span>  <span data-ttu-id="645d1-113">V tomto kurzu budeme používat hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="645d1-113">For this tutorial, we'll be using hello Azure Portal.</span></span>  
> 
> 

<span data-ttu-id="645d1-114">**toocreate účet úložiště pro předplatné Azure**</span><span class="sxs-lookup"><span data-stu-id="645d1-114">**toocreate a storage account for an Azure subscription**</span></span>

1. <span data-ttu-id="645d1-115">Přihlaste se toohello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="645d1-115">Sign in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="645d1-116">V hello levém horním rohu, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="645d1-116">In hello upper left corner, select **New**.</span></span> <span data-ttu-id="645d1-117">V hello **nový** vyberte **Data + úložiště**, pak klikněte na tlačítko **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="645d1-117">In hello **New** Dialog, select **Data  + Storage**, then click **Storage account**.</span></span>
    
    <span data-ttu-id="645d1-118">Hello **vytvořit účet úložiště** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="645d1-118">hello **Create storage account** blade appears.</span></span>   

    ![Vytvořit účet úložiště][create-new-storage-account]  

3. <span data-ttu-id="645d1-120">V hello **název** pole, zadejte název subdomény.</span><span class="sxs-lookup"><span data-stu-id="645d1-120">In hello **Name** field, type a subdomain name.</span></span> <span data-ttu-id="645d1-121">Tato položka může obsahovat 3 až 24 malých písmen a číslic.</span><span class="sxs-lookup"><span data-stu-id="645d1-121">This entry can contain 3-24 lowercase letters and numbers.</span></span>
   
    <span data-ttu-id="645d1-122">Tato hodnota se změní na název hostitele hello v rámci hello identifikátor URI, který se používá k adresování objektů Blob, fronty nebo tabulky prostředky pro předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="645d1-122">This value becomes hello host name within hello URI that is used to address Blob, Queue, or Table resources for hello subscription.</span></span> <span data-ttu-id="645d1-123">Chcete-li vyřešit kontejner prostředku v hello služby objektů Blob, použijte identifikátor URI v hello formátu, kde  *&lt;StorageAccountLabel&gt;*  odkazuje toohello hodnoty, které jste zadali v **zadejte adresu URL**:</span><span class="sxs-lookup"><span data-stu-id="645d1-123">To address a container resource in hello Blob service, you would use a URI in hello following format, where *&lt;StorageAccountLabel&gt;* refers toohello value you typed in **Enter a URL**:</span></span>
   
    <span data-ttu-id="645d1-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;můj_kontejner&gt;*</span><span class="sxs-lookup"><span data-stu-id="645d1-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*</span></span>
   
    <span data-ttu-id="645d1-125">**Důležité:** hello adresy URL popisku forms hello subdomény účtu úložiště hello URI a musí být jedinečný mezi všechny hostované služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="645d1-125">**Important:** hello URL label forms hello subdomain of hello storage  account URI and must be unique among all hosted services in  Azure.</span></span>
   
    <span data-ttu-id="645d1-126">Tato hodnota se používá jako hello název tohoto účtu úložiště hello portálu nebo při přístupu k tomuto účtu prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="645d1-126">This value is also used as hello name of this storage account in hello portal, or when accessing this account programmatically.</span></span>
4. <span data-ttu-id="645d1-127">Ponechte výchozí hodnoty hello **modelu nasazení**, **účet druhu**, **výkonu**, a **replikace**.</span><span class="sxs-lookup"><span data-stu-id="645d1-127">Leave hello defaults for **Deployment model**, **Account kind**, **Performance**, and **Replication**.</span></span> 
5. <span data-ttu-id="645d1-128">Vyberte hello **předplatné** že hello účet úložiště se použije s.</span><span class="sxs-lookup"><span data-stu-id="645d1-128">Select hello **Subscription** that hello storage account will be used with.</span></span>
6. <span data-ttu-id="645d1-129">Vyberte nebo vytvořte **skupinu prostředků**.</span><span class="sxs-lookup"><span data-stu-id="645d1-129">Select or create a **Resource Group**.</span></span>  <span data-ttu-id="645d1-130">Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="645d1-130">For more information on Resource Groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
7. <span data-ttu-id="645d1-131">Vyberte umístění vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="645d1-131">Select a location for your storage account.</span></span>
8. <span data-ttu-id="645d1-132">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="645d1-132">Click **Create**.</span></span> <span data-ttu-id="645d1-133">Vytvoření účtu úložiště hello Hello proces může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="645d1-133">hello process of creating hello storage account might take several minutes toocomplete.</span></span>

## <a name="step-2-enable-cdn-for-hello-storage-account"></a><span data-ttu-id="645d1-134">Krok 2: Povolení CDN pro účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="645d1-134">Step 2: Enable CDN for hello storage account</span></span>

<span data-ttu-id="645d1-135">Díky integraci nejnovější hello můžete teď povolit CDN pro váš účet úložiště bez opuštění portálu rozšíření úložiště.</span><span class="sxs-lookup"><span data-stu-id="645d1-135">With hello newest integration, you can now enable CDN for your storage account without leaving your storage portal extension.</span></span> 

1. <span data-ttu-id="645d1-136">Vyberte účet úložiště hello, hledání "CDN" nebo se posouvají dolů z hello levé navigační nabídku a pak klikněte na "Azure CDN".</span><span class="sxs-lookup"><span data-stu-id="645d1-136">Select hello storage account, search "CDN" or scroll down from hello left navigation menu, then click "Azure CDN".</span></span>
    
    <span data-ttu-id="645d1-137">Hello **Azure CDN** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="645d1-137">hello **Azure CDN** blade appears.</span></span>

    ![povolení navigace CDN][cdn-enable-navigation]
    
2. <span data-ttu-id="645d1-139">Vytvořte nový koncový bod zadáním hello požadované informace</span><span class="sxs-lookup"><span data-stu-id="645d1-139">Create a new endpoint by entering hello required information</span></span>
    - <span data-ttu-id="645d1-140">**Profil CDN**: můžete vytvořit novou nebo použít stávající profil.</span><span class="sxs-lookup"><span data-stu-id="645d1-140">**CDN Profile**: You can create a new or use an existing profile.</span></span>
    - <span data-ttu-id="645d1-141">**Cenová úroveň**: stačí tooselect cenovou úroveň, pokud vytvoříte nový profil CDN.</span><span class="sxs-lookup"><span data-stu-id="645d1-141">**Pricing tier**: You only need tooselect a pricing tier if you create a new CDN profile.</span></span>
    - <span data-ttu-id="645d1-142">**Název koncového bodu CDN**: Zadejte název koncového bodu podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="645d1-142">**CDN endpoint name**: Enter an endpoint name per your choice.</span></span>

    > [!TIP]
    > <span data-ttu-id="645d1-143">koncový bod CDN Hello vytvořili používá název hostitele hello svého účtu úložiště jako původní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="645d1-143">hello created CDN endpoint uses hello hostname of your storage account as origin by default.</span></span>

    <span data-ttu-id="645d1-144">! [cdn nové vytvoření koncového bodu] [cdn--koncový bod vytvoření nové]</span><span class="sxs-lookup"><span data-stu-id="645d1-144">![cdn new endpoint creation][cdn-new-endpoint-creation]</span></span>

3. <span data-ttu-id="645d1-145">Po vytvoření hello nový koncový bod se zobrazí v seznamu koncový bod hello výše.</span><span class="sxs-lookup"><span data-stu-id="645d1-145">After creation, hello new endpoint will show up in hello endpoint list above.</span></span>

    ![Nový koncový bod CDN úložiště][cdn-storage-new-endpoint]

> [!NOTE]
> <span data-ttu-id="645d1-147">Můžete také přejít tooAzure CDN rozšíření tooenable CDN. [Kurzu](#Tutorial-cdn-create-profile).</span><span class="sxs-lookup"><span data-stu-id="645d1-147">You can also go tooAzure CDN extension tooenable CDN.[Tutorial](#Tutorial-cdn-create-profile).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a><span data-ttu-id="645d1-148">Krok 3: Povolení dalších funkcí CDN</span><span class="sxs-lookup"><span data-stu-id="645d1-148">Step 3: Enable additional CDN features</span></span>

<span data-ttu-id="645d1-149">V okně "Azure CDN" účet úložiště klikněte na koncový bod CDN hello z okno Konfigurace hello seznamu tooopen CDN.</span><span class="sxs-lookup"><span data-stu-id="645d1-149">From storage account "Azure CDN" blade, click hello CDN endpoint from hello list tooopen CDN configuration blade.</span></span> <span data-ttu-id="645d1-150">Můžete povolit další funkce CDN pro doručení, jako je například kompresi, řetězce dotazu, geograficky filtrování.</span><span class="sxs-lookup"><span data-stu-id="645d1-150">You can enable additional CDN features for your delivery, such as compression, query string, geo filtering.</span></span> <span data-ttu-id="645d1-151">Můžete také přidat vlastní doménu koncový bod CDN tooyour mapování a povolit HTTPS pro vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="645d1-151">You can also add custom domain mapping tooyour CDN endpoint and enable custom domain HTTPS.</span></span>
    
![Konfigurace cdn úložiště CDN][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a><span data-ttu-id="645d1-153">Krok 4: Přístup k obsahu CDN</span><span class="sxs-lookup"><span data-stu-id="645d1-153">Step 4: Access CDN content</span></span>
<span data-ttu-id="645d1-154">tooaccess v mezipaměti obsahu na hello CDN, použijte hello CDN adresy URL uvedené v hello portálu.</span><span class="sxs-lookup"><span data-stu-id="645d1-154">tooaccess cached content on hello CDN, use hello CDN URL provided in hello portal.</span></span> <span data-ttu-id="645d1-155">Hello adresy pro v mezipaměti objektů blob budou podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="645d1-155">hello address for a cached blob will be similar toohello following:</span></span>

<span data-ttu-id="645d1-156">http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\></span><span class="sxs-lookup"><span data-stu-id="645d1-156">http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\></span></span>

> [!NOTE]
> <span data-ttu-id="645d1-157">Jakmile povolíte účet úložiště tooa CDN přístup, jsou způsobilé pro ukládání do mezipaměti CDN edge všechny veřejně dostupné objekty.</span><span class="sxs-lookup"><span data-stu-id="645d1-157">Once you enable CDN access tooa storage account, all publicly available objects are eligible for CDN edge caching.</span></span> <span data-ttu-id="645d1-158">Můžete upravit objekt, který je aktuálně uložené do mezipaměti v hello CDN, nový obsah hello nebudete mít k dispozici prostřednictvím hello CDN, dokud hello CDN aktualizuje jeho obsah, když vyprší platnost hello uložené v mezipaměti obsahu time to live období.</span><span class="sxs-lookup"><span data-stu-id="645d1-158">If you modify an object that is currently cached in hello CDN, hello new content will not be available via hello CDN until hello CDN refreshes its content when hello cached content time-to-live period expires.</span></span>
> 
> 

## <a name="step-5-remove-content-from-hello-cdn"></a><span data-ttu-id="645d1-159">Krok 5: Odeberte obsah z CDN hello</span><span class="sxs-lookup"><span data-stu-id="645d1-159">Step 5: Remove content from hello CDN</span></span>
<span data-ttu-id="645d1-160">Nechcete-li již toocache objekt v hello Azure Content Delivery Network (CDN), můžete provést jednu z následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="645d1-160">If you no longer wish toocache an object in hello Azure Content Delivery Network (CDN), you can take one of hello following steps:</span></span>

* <span data-ttu-id="645d1-161">Hello můžete nastavit kontejner privátní místo veřejné.</span><span class="sxs-lookup"><span data-stu-id="645d1-161">You can make hello container private instead of public.</span></span> <span data-ttu-id="645d1-162">V tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../storage/blobs/storage-manage-access-to-resources.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="645d1-162">See [Manage anonymous read access toocontainers and blobs](../storage/blobs/storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="645d1-163">Můžete zakázat nebo odstranit koncový bod CDN hello pomocí hello portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="645d1-163">You can disable or delete hello CDN endpoint using hello Management Portal.</span></span>
* <span data-ttu-id="645d1-164">Můžete upravit vaší hostované služby toono delší reakce toorequests hello objektu.</span><span class="sxs-lookup"><span data-stu-id="645d1-164">You can modify your hosted service toono longer respond toorequests for hello object.</span></span>

<span data-ttu-id="645d1-165">Objekt již uložené v mezipaměti v hello CDN zůstane v mezipaměti, dokud neuplyne období time to live hello hello objektu, nebo dokud se vyprazdňují hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="645d1-165">An object already cached in hello CDN will remain cached until hello time-to-live period for hello object expires or until hello endpoint is purged.</span></span> <span data-ttu-id="645d1-166">Když hello time to live období vyprší, bude hello CDN toosee zkontrolujte, jestli koncový bod CDN hello je stále platný a hello objekt anonymně přístupné.</span><span class="sxs-lookup"><span data-stu-id="645d1-166">When hello time-to-live period expires, hello CDN will check toosee whether hello CDN endpoint is still valid and hello object still anonymously accessible.</span></span> <span data-ttu-id="645d1-167">Pokud není, pak hello objektu bude už do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="645d1-167">If it is not, then hello object will no longer be cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="645d1-168">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="645d1-168">Additional resources</span></span>
* [<span data-ttu-id="645d1-169">Jak tooMap obsahu CDN tooa vlastní domény</span><span class="sxs-lookup"><span data-stu-id="645d1-169">How tooMap CDN Content tooa Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="645d1-170">Povolit HTTPS pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="645d1-170">Enable HTTPS for your custom domain</span></span>](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
