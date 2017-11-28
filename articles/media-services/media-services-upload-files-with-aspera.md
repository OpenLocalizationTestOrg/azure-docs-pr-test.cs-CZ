---
title: "aaaUpload souborů do účtu Azure Media Services pomocí Aspera | Microsoft Docs"
description: "Tento kurz vás provede kroky hello nahrávání souborů do účtu úložiště, která je přidružena k účtu Media Services pomocí hello ** Aspera serveru na vyžádání ** služby v Azure."
services: media-services
documentationcenter: 
author: johndeu
manager: cfowler
editor: 
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 7213f016cc1b7f262b14db7b39b478a03970d1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-aspera-server-on-demand-service-on-azure"></a><span data-ttu-id="579d9-103">Nahrání souborů do účtu Media Services pomocí hello Aspera serveru na vyžádání služby v Azure</span><span class="sxs-lookup"><span data-stu-id="579d9-103">Upload files into a Media Services account using hello Aspera Server On Demand service on Azure</span></span>

## <a name="overview"></a><span data-ttu-id="579d9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="579d9-104">Overview</span></span>

<span data-ttu-id="579d9-105">**Aspera** je software pro vysokorychlostní přenos souborů.</span><span class="sxs-lookup"><span data-stu-id="579d9-105">**Aspera** is a high-speed file transfer software.</span></span> <span data-ttu-id="579d9-106">**Aspera Server On Demand** pro Azure umožňuje vysokorychlostní nahrávání a stahování velkých souborů přímo do úložiště objektů Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="579d9-106">**Aspera Server On Demand** for Azure enables high-speed upload and download of large files directly into Azure Blob object storage.</span></span> <span data-ttu-id="579d9-107">Informace o **Aspera na vyžádání**, najdete v části hello [Aspera cloudu](http://cloud.asperasoft.com/) lokality.</span><span class="sxs-lookup"><span data-stu-id="579d9-107">For information about **Aspera On Demand**, see hello [Aspera Cloud](http://cloud.asperasoft.com/) site.</span></span> 
  
<span data-ttu-id="579d9-108">**Aspera serveru na vyžádání** pro Azure je možné zakoupit z hello [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/).</span><span class="sxs-lookup"><span data-stu-id="579d9-108">**Aspera Server On Demand** for Azure is available for purchase from hello [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/).</span></span> <span data-ttu-id="579d9-109">V pořadí toocomplete nákupu **Aspera Server na vyžádání** pro Azure, prosím přihlaste se k Azure Marketplace pomocí účtu Windows Live ID.</span><span class="sxs-lookup"><span data-stu-id="579d9-109">In order toocomplete a purchase of **Aspera Server On Demand** for Azure, please log into Azure Marketplace with your Windows Live ID.</span></span>

<span data-ttu-id="579d9-110">Tento kurz vás provede kroky hello nahrávání souborů do účtu úložiště, která je přidružena k účtu Media Services pomocí hello **Aspera Server na vyžádání** služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="579d9-110">This tutorial walks you through hello steps of uploading files into a storage account that is associated with a Media Services account using hello **Aspera Server On Demand** service on Azure.</span></span> 

<span data-ttu-id="579d9-111">Příklad, který ukazuje, jak funguje toouse Azure s Aspera a služba Media Services můžete nalézt [zde](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span><span class="sxs-lookup"><span data-stu-id="579d9-111">You can find an example that shows how toouse Azure functions with Aspera and Media Services [here](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span></span>

>[!NOTE]
><span data-ttu-id="579d9-112">Existuje limit toohello maximální velikost souboru podporována pro zpracování pomocí služby Azure Media Services procesory médií (sad Management Pack).</span><span class="sxs-lookup"><span data-stu-id="579d9-112">There is a limit toohello maximum file size supported for processing with Azure Media Services media processors (MPs).</span></span> <span data-ttu-id="579d9-113">Najdete v tématu [to](media-services-quotas-and-limitations.md) téma podrobné informace o omezení velikosti souborů hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="579d9-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="579d9-114">Prerequisites</span></span> 

<span data-ttu-id="579d9-115">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="579d9-115">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="579d9-116">Windows Live ID</span><span class="sxs-lookup"><span data-stu-id="579d9-116">A Windows Live ID</span></span>
* <span data-ttu-id="579d9-117">[Účet Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="579d9-117">An [Azure account](https://azure.microsoft.com).</span></span> <span data-ttu-id="579d9-118">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="579d9-118">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="579d9-119">[Účet Azure Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="579d9-119">An [Azure Media Services account](media-services-portal-create-account.md).</span></span>

## <a name="purchase-aspera-on-demand-for-azure"></a><span data-ttu-id="579d9-120">Nákup služby Aspera On Demand pro Azure</span><span class="sxs-lookup"><span data-stu-id="579d9-120">Purchase Aspera On Demand for Azure</span></span>

<span data-ttu-id="579d9-121">Po přihlášení do Azure Marketplace postupujte podle těchto základních kroků toocomplete nákupu produktu Aspera na vyžádání pro Azure.</span><span class="sxs-lookup"><span data-stu-id="579d9-121">Once you have logged into Azure Marketplace,  follow these basic steps toocomplete your purchase of Aspera On Demand for Azure.</span></span>

1. <span data-ttu-id="579d9-122">Vyhledejte Asperu a vyberte „Server On Demand“.</span><span class="sxs-lookup"><span data-stu-id="579d9-122">Search for Aspera and select 'Server On Demand'.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. <span data-ttu-id="579d9-124">Zkontrolujte hello plány předplatného a klikněte na 'zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="579d9-124">Review hello subscription plans and click on 'Sign Up'</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. <span data-ttu-id="579d9-126">Vyplňte hello specifika pro váš Server na vyžádání předplatného.</span><span class="sxs-lookup"><span data-stu-id="579d9-126">Fill in hello specifics for your Server on Demand subscription.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. <span data-ttu-id="579d9-128">Klikněte na hello **cenová úroveň** a vyberte požadované měsíční svazku panelu dílčí hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-128">Click on hello **Pricing Tier** and select your desired monthly volume in hello sub panel.</span></span> <span data-ttu-id="579d9-129">V hello **plánování podrobnosti** panel, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="579d9-129">In hello **Plan details** panel, select **OK**.</span></span> <span data-ttu-id="579d9-130">Potom v hello **zvolte cenovou úroveň** panelu, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="579d9-130">Then, in hello **Choose your Pricing Tier** panel, click **Select**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. <span data-ttu-id="579d9-132">Klikněte na **právní podmínky** tooview a přijměte právní podmínky hello panelu dílčí hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-132">Click on **Legal terms** tooview and accept hello legal terms in hello sub panel.</span></span> <span data-ttu-id="579d9-133">Až si přečtete hello právní podmínky, klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="579d9-133">Once you have reviewed hello legal terms, click **Purchase**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. <span data-ttu-id="579d9-135">Dokončení nákupu hello kliknutím **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="579d9-135">Complete hello purchase by clicking **Create**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. <span data-ttu-id="579d9-137">Hello řídicí panel Azure bude oznamujeme, že ho se zřizováním služby hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-137">hello Azure dashboard will announce that it is provisioning hello service.</span></span>  <span data-ttu-id="579d9-138">Po dokončení se při zřizování, můžete najít nové předplatné hello prohledáním ve vašich prostředků pro název hello hello služby.</span><span class="sxs-lookup"><span data-stu-id="579d9-138">Once it is completed with provisioning, you can find hello new subscription by searching in your resources for hello name of hello service.</span></span> <span data-ttu-id="579d9-139">Jakmile naleznete hello služby, klikněte na něm portál pro správu služeb toolaunch hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-139">Once you have found hello service, double click on it toolaunch hello service management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. <span data-ttu-id="579d9-141">Spuštění portálu pro správu Aspera hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-141">Launch hello Aspera management portal.</span></span> <span data-ttu-id="579d9-142">Jakmile naleznete novou Aspera službu, můžete získat přístup k portálu pro správu toohello, kliknutím na hello služby.</span><span class="sxs-lookup"><span data-stu-id="579d9-142">Once you have found your new Aspera service, you can gain access toohello management portal, by clicking on hello service.</span></span>  <span data-ttu-id="579d9-143">Otevře se nový panel.</span><span class="sxs-lookup"><span data-stu-id="579d9-143">A new panel will be launched.</span></span> <span data-ttu-id="579d9-144">Z v rámci tento nový panel, musíte tooclick na hello **název prostředku** vaší nové služby.</span><span class="sxs-lookup"><span data-stu-id="579d9-144">From within that new panel, you need tooclick on hello **Resource Name** of your new service.</span></span>  <span data-ttu-id="579d9-145">V hello následující snímek obrazovky je název prostředku hello 'AsperaTransferDemo'.</span><span class="sxs-lookup"><span data-stu-id="579d9-145">In hello following screenshot, hello resource name is 'AsperaTransferDemo'.</span></span> <span data-ttu-id="579d9-146">Když kliknete na název prostředku hello, se spustí další panely.</span><span class="sxs-lookup"><span data-stu-id="579d9-146">Once you click on hello resource name, another panel is launched.</span></span> <span data-ttu-id="579d9-147">Na nově otevřeném panelu uvidíte odkaz Správa.</span><span class="sxs-lookup"><span data-stu-id="579d9-147">In that newly launched panel, you will see a 'Manage' link.</span></span> <span data-ttu-id="579d9-148">Klikněte na hello "Správa" odkaz toolaunch hello Aspera portálu pro správu služby.</span><span class="sxs-lookup"><span data-stu-id="579d9-148">Click on hello 'Manage' link toolaunch hello Aspera management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. <span data-ttu-id="579d9-150">Kliknutím na hello spravovat odkaz, zobrazí se stránka toohello registrace, který je vyžadován tooaccess hello služby.</span><span class="sxs-lookup"><span data-stu-id="579d9-150">By clicking on hello manage link, you will get toohello registration page, which is required tooaccess hello service.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. <span data-ttu-id="579d9-152">V tomto okamžiku byste měli mít přístup toohello Aspera portál pro správu služeb, kde můžete vytvořit přístupové klíče, stáhnout Aspera klientů a licence, zobrazení využití a další informace o hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="579d9-152">At this point, you should have access toohello Aspera service management portal, where you can create access keys, download Aspera clients and licenses, view usage and learn about hello APIs.</span></span>

    <span data-ttu-id="579d9-153">Hello následující snímek obrazovky ukazuje vytvoření hello přístup.</span><span class="sxs-lookup"><span data-stu-id="579d9-153">hello following screenshot shows hello access creation.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    <span data-ttu-id="579d9-155">Hello následující snímek obrazovky ukazuje použití hello reporting rozhraní portálu hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-155">hello following screenshot shows hello usage reporting interfaces in hello portal.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a><span data-ttu-id="579d9-157">Nahrávání souborů pomocí Aspery</span><span class="sxs-lookup"><span data-stu-id="579d9-157">Upload files with Aspera</span></span>

1. <span data-ttu-id="579d9-158">Stáhněte a nainstalujte software klienta Aspera hello:</span><span class="sxs-lookup"><span data-stu-id="579d9-158">Download and install hello Aspera client software:</span></span>
    
    * [<span data-ttu-id="579d9-159">Modul plug-in prohlížeče</span><span class="sxs-lookup"><span data-stu-id="579d9-159">Browser plugin</span></span>](http://downloads.asperasoft.com/connect2/)
    * [<span data-ttu-id="579d9-160">Plně funkční klient</span><span class="sxs-lookup"><span data-stu-id="579d9-160">Rich client</span></span>](http://downloads.asperasoft.com/en/downloads/2)

2. <span data-ttu-id="579d9-161">Proveďte svůj první přenos.</span><span class="sxs-lookup"><span data-stu-id="579d9-161">Make your first transfer.</span></span> <span data-ttu-id="579d9-162">Tootransfer klienta pořadí toouse hello Aspera s hello Aspera přenosu služby je třeba toocomplete hello následující:</span><span class="sxs-lookup"><span data-stu-id="579d9-162">In order toouse hello Aspera client tootransfer with hello Aspera transfer service, you need toocomplete hello following:</span></span> 

    1. <span data-ttu-id="579d9-163">Vytvořte přístupový klíč, pomocí portálu Aspera hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-163">Create an access key, using hello Aspera portal.</span></span>  
    2. <span data-ttu-id="579d9-164">Stažení, instalace a licencí hello Aspera klienta (softwaru naleznete v portálu Aspera hello).</span><span class="sxs-lookup"><span data-stu-id="579d9-164">Download, install, and license hello Aspera client (software can be found in hello Aspera portal).</span></span>  

    >[!NOTE]
    ><span data-ttu-id="579d9-165">Přečtěte si informace o konfiguraci příručce klienta Aspera hello.</span><span class="sxs-lookup"><span data-stu-id="579d9-165">Please read hello Aspera client guide for configuration information.</span></span>
    
    3. <span data-ttu-id="579d9-166">Načtení některých informací o účtu úložiště, která souvisí s vaším účtem média Azure pomocí hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="579d9-166">Retrieve some information of your storage account that is associated with your Azure Media Account using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="579d9-167">Konkrétně, název a klíč a název kontejneru objektu blob úložiště hello toowhich chcete tooplace svůj obsah.</span><span class="sxs-lookup"><span data-stu-id="579d9-167">Specifically, name and key, and hello storage blob container name in toowhich you want tooplace your content.</span></span> 

        * <span data-ttu-id="579d9-168">informace o úložiště hello tooget z portálu hello: najít váš účet úložiště, klikněte na hello přístupových klíčů a klíč název a hello hello kopie vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="579d9-168">tooget hello storage info from hello portal: find your storage account, click on hello Access keys and copy hello name and hello key of your account.</span></span>
        * <span data-ttu-id="579d9-169">název kontejneru hello tooget: najít váš účet úložiště, vyberte **objekty BLOB**vyberte hello název kontejneru hello chcete tooupload hello obsah do.</span><span class="sxs-lookup"><span data-stu-id="579d9-169">tooget hello container name: find your storage account, select **Blobs**, select hello name of hello container you want tooupload hello content into.</span></span> 

    <span data-ttu-id="579d9-170">Zde je snímek obrazovky hello hello Aspera klienta **Správce připojení** kde musíte zadat typ hello "Azure" úložiště a přihlašovací údaje, jakož i hello kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="579d9-170">Below is hello screenshot of hello Aspera client **Connection Manager** where you must specify hello 'Azure' storage type and credentials as well as hello blob container.</span></span>

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a><span data-ttu-id="579d9-172">Zdroje</span><span class="sxs-lookup"><span data-stu-id="579d9-172">Resources</span></span>

<span data-ttu-id="579d9-173">následující prostředky Hello byly uvedené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="579d9-173">hello following resources were mentioned in this article.</span></span> 

* [<span data-ttu-id="579d9-174">Modul plug-in prohlížeče Connect</span><span class="sxs-lookup"><span data-stu-id="579d9-174">Connect Browser Plugin</span></span>](http://downloads.asperasoft.com/connect2/)
* [<span data-ttu-id="579d9-175">Příručka modulu Connect</span><span class="sxs-lookup"><span data-stu-id="579d9-175">Connect Guide</span></span>](http://downloads.asperasoft.com/en/documentation/8)
* [<span data-ttu-id="579d9-176">Klient Aspera</span><span class="sxs-lookup"><span data-stu-id="579d9-176">Aspera Client</span></span>](http://downloads.asperasoft.com/en/downloads/2)
* [<span data-ttu-id="579d9-177">Příručka klienta</span><span class="sxs-lookup"><span data-stu-id="579d9-177">Client Guide</span></span>](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a><span data-ttu-id="579d9-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="579d9-178">Next steps</span></span>

<span data-ttu-id="579d9-179">Teď můžete [zkopírovat objekty blob z účtu úložiště do účtu AMS](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span><span class="sxs-lookup"><span data-stu-id="579d9-179">You can now [copy blobs from a storage account into an AMS account ](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="579d9-180">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="579d9-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="579d9-181">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="579d9-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

