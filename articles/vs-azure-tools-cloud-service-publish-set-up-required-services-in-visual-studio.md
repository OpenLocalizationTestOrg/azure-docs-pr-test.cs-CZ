---
title: "Příprava na publikování nebo nasadit aplikaci Azure ze sady Visual Studio | Microsoft Docs"
description: "Další postupy pro nastavení cloudu a úložiště účet služby a konfiguraci aplikace Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: cc4fb87e559f554634ae062a59bee31f0831da64
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a><span data-ttu-id="6c3d3-103">Příprava na publikování nebo nasadit aplikaci Azure ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c3d3-103">Prepare to Publish or Deploy an Azure Application from Visual Studio</span></span>
## <a name="overview"></a><span data-ttu-id="6c3d3-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6c3d3-104">Overview</span></span>
<span data-ttu-id="6c3d3-105">Před publikováním projekt cloudové služby, je potřeba nastavit následující služby:</span><span class="sxs-lookup"><span data-stu-id="6c3d3-105">Before you can publish a cloud service project, you must set up the following services:</span></span>

* <span data-ttu-id="6c3d3-106">A **Cloudová služba** ke spuštění vaší rolí v prostředí Azure</span><span class="sxs-lookup"><span data-stu-id="6c3d3-106">A **cloud service** to run your roles in the Azure environment</span></span>
* <span data-ttu-id="6c3d3-107">A **účet úložiště** , který poskytuje přístup ke službám Blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-107">A **storage account** that provides access to the Blob, Queue, and Table services.</span></span>

<span data-ttu-id="6c3d3-108">Pomocí následujících postupů nastavit tyto služby a konfiguraci vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="6c3d3-108">Use the following procedures to set up these services and configure your application</span></span>

## <a name="create-a-cloud-service"></a><span data-ttu-id="6c3d3-109">Vytvoření cloudové služby</span><span class="sxs-lookup"><span data-stu-id="6c3d3-109">Create a cloud service</span></span>
<span data-ttu-id="6c3d3-110">Chcete-li publikovat Cloudová služba Azure, musí nejdřív vytvořit cloudové služby, který se spouští vaše role v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-110">To publish a cloud service to Azure, you must first create a cloud service, which runs your roles in the Azure environment.</span></span> <span data-ttu-id="6c3d3-111">Můžete vytvořit cloudové služby v rámci [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), jak je popsáno v části **k vytvoření cloudové služby pomocí portálu Azure classic**dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-111">You can create a cloud service in the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), as described in the section **To create a cloud service by using the Azure classic portal**, later in this topic.</span></span> <span data-ttu-id="6c3d3-112">Cloudové služby v sadě Visual Studio můžete také vytvořit pomocí Průvodce přidáním publikování.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-112">You can also create a cloud service in Visual Studio by using the publishing wizard.</span></span>

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a><span data-ttu-id="6c3d3-113">Vytvoření cloudové služby, pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c3d3-113">To create a cloud service by using Visual Studio</span></span>
1. <span data-ttu-id="6c3d3-114">Otevřete místní nabídky pro Azure projekt a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-114">Open the shortcut menu for the Azure project, and choose **Publish**.</span></span>

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. <span data-ttu-id="6c3d3-116">Pokud nejste přihlášeni, přihlaste se pomocí uživatelského jména a hesla pro účet Microsoft nebo účtu organizace, který je spojen s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-116">If you haven't signed in, sign in with your username and password for the Microsoft account or organizational account that's associated with your Azure subscription.</span></span>
3. <span data-ttu-id="6c3d3-117">Vyberte **Další** tlačítko pro přechod **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-117">Choose the **Next** button to advance to the **Settings** page.</span></span>

    ![Běžné nastavení Průvodce publikování](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. <span data-ttu-id="6c3d3-119">V **cloudové služby** vyberte **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-119">In the **Cloud Services** list, choose **Create New**.</span></span> <span data-ttu-id="6c3d3-120">**Vytvoření služby Azure** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-120">The **Create Azure Services** dialog appears.</span></span>
5. <span data-ttu-id="6c3d3-121">Zadejte název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-121">Enter the name of your cloud service.</span></span> <span data-ttu-id="6c3d3-122">Název je součástí adresy URL služby a proto musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-122">The name forms part of the URL for your service and therefore must be globally unique.</span></span> <span data-ttu-id="6c3d3-123">Název není malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-123">The name is not case-sensitive.</span></span>

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a><span data-ttu-id="6c3d3-124">K vytvoření cloudové služby pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="6c3d3-124">To create a cloud service by using the Azure classic portal</span></span>
1. <span data-ttu-id="6c3d3-125">Přihlaste se k [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-125">Sign in to the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkId=253103) on the Microsoft website.</span></span>
2. <span data-ttu-id="6c3d3-126">(volitelné) Chcete-li zobrazit seznam cloudových služeb, které jste už vytvořili, zvolte odkaz cloudové služby na levé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-126">(optional) To display a list of cloud services that you've already created, choose the Cloud Services link on the left side of the page.</span></span>
3. <span data-ttu-id="6c3d3-127">Vyberte  **+**  ikonu v levém dolním rohu a potom vyberte **Cloudová služba** v zobrazené nabídce.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-127">Choose the **+** icon in the lower-left corner, and then choose **Cloud Service** on the menu that appears.</span></span> <span data-ttu-id="6c3d3-128">Jiné obrazovky s dvě možnosti, **rychle vytvořit** a **vytvořit vlastní**, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-128">Another screen with two options, **Quick Create** and **Custom Create**, appears.</span></span> <span data-ttu-id="6c3d3-129">Pokud se rozhodnete **rychle vytvořit**, můžete vytvořit cloudovou službu právě zadáním jeho adresa URL a oblasti, kde se bude fyzicky hostován.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-129">If you choose **Quick Create**, you can create a cloud service just by specifying its URL and the region where it will be physically hosted.</span></span> <span data-ttu-id="6c3d3-130">Pokud se rozhodnete **vytvořit vlastní**, můžete publikovat Cloudová služba okamžitě zadáním balíček (soubor .cspkg), souboru (.csdef) a certifikát.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-130">If you choose **Custom Create**, you can immediately publish a cloud service by specifying a package (.cspkg file), a configuration (.cscfg) file, and a certificate.</span></span> <span data-ttu-id="6c3d3-131">Vytvořit vlastní není povinné, pokud chcete publikovat pomocí cloudové služby **publikovat** v projektu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-131">Custom Create isn’t required if you intend to publish your cloud service by using the **Publish** command in an Azure project.</span></span> <span data-ttu-id="6c3d3-132">**Publikovat** příkaz je k dispozici v místní nabídce pro projektu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-132">The **Publish** command is available on the shortcut menu for an Azure project.</span></span>
4. <span data-ttu-id="6c3d3-133">Zvolte **rychle vytvořit** později publikovat cloudové služby pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-133">Choose **Quick Create** to later publish your cloud service by using Visual Studio.</span></span>
5. <span data-ttu-id="6c3d3-134">Zadejte název pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-134">Specify a name for your cloud service.</span></span> <span data-ttu-id="6c3d3-135">Úplnou adresu URL se zobrazí vedle názvu.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-135">The complete URL appears next to the name.</span></span>
6. <span data-ttu-id="6c3d3-136">V seznamu vyberte oblast, kde se nachází většina uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-136">In the list, choose the region where most of your users are located.</span></span>
7. <span data-ttu-id="6c3d3-137">V dolní části okna, vyberte **vytvoření cloudové služby** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-137">At the bottom of the window, choose the **Create Cloud Service** link.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="6c3d3-138">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="6c3d3-138">Create a storage account</span></span>
<span data-ttu-id="6c3d3-139">Účet úložiště poskytuje přístup ke službám Blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-139">A storage account provides access to the Blob, Queue, and Table services.</span></span> <span data-ttu-id="6c3d3-140">Účet úložiště můžete vytvořit pomocí sady Visual Studio nebo [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-140">You can create a storage account by using Visual Studio or the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkId=253103).</span></span>

### <a name="to-create-a-storage-account-by-using-visual-studio"></a><span data-ttu-id="6c3d3-141">Chcete-li vytvořit účet úložiště pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c3d3-141">To create a storage account by using Visual Studio</span></span>
1. <span data-ttu-id="6c3d3-142">V **Průzkumníku řešení**, otevřete místní nabídku pro **úložiště** uzel a potom zvolte **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-142">In **Solution Explorer**, open the shortcut menu for the **Storage** node, and then choose **Create Storage Account**.</span></span>

    ![Vytvořit nový účet úložiště Azure](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. <span data-ttu-id="6c3d3-144">Vyberte nebo zadejte následující informace pro nový účet úložiště **vytvořit účet úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-144">Select or enter the following information for the new storage account in the **Create Storage Account** dialog box.</span></span>

   * <span data-ttu-id="6c3d3-145">Předplatné Azure, ke které chcete přidat účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-145">The Azure subscription to which you want to add the storage account.</span></span>
   * <span data-ttu-id="6c3d3-146">Název, který chcete použít pro nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-146">The name you want to use for the new storage account.</span></span>
   * <span data-ttu-id="6c3d3-147">Oblast nebo skupinu vztahů (například západní USA nebo jihovýchodní Asie).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-147">The region or affinity group (such as West US or East Asia).</span></span>
   * <span data-ttu-id="6c3d3-148">Typ replikace, kterou chcete použít pro účet úložiště, jako je například geograficky redundantní.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-148">The type of replication you want to use for the storage account, such as Geo-Redundant.</span></span>
3. <span data-ttu-id="6c3d3-149">Až budete hotoví, zvolte **vytvořit**. Nový účet úložiště zobrazí v **úložiště** seznamu v **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-149">When you’re done, choose **Create**.The new storage account appears in the **Storage** list in **Server Explorer**.</span></span>

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a><span data-ttu-id="6c3d3-150">Chcete-li vytvořit účet úložiště pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="6c3d3-150">To create a storage account by using the Azure classic portal</span></span>
1. <span data-ttu-id="6c3d3-151">Přihlaste se k [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-151">Sign in to the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkId=253103) on the Microsoft website.</span></span>
2. <span data-ttu-id="6c3d3-152">(Volitelné) Chcete-li zobrazit účty úložiště, zvolte **úložiště** odkaz v panelu na levé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-152">(Optional) To view your storage accounts, choose the **Storage** link in the panel on the left side of the page.</span></span>
3. <span data-ttu-id="6c3d3-153">V levém horním rohu stránky, vyberte  **+**  ikonu.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-153">In the lower-left corner of the page, choose the **+** icon.</span></span>
4. <span data-ttu-id="6c3d3-154">V nabídce, která se zobrazí, zvolte **úložiště**a potom zvolte **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-154">In the menu that appears, choose **Storage**, and then choose **Quick Create**.</span></span>
5. <span data-ttu-id="6c3d3-155">Zadejte název, který bude mít za následek jedinečnou adresou url účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-155">Give the storage account a name that will result in a unique url.</span></span>
6. <span data-ttu-id="6c3d3-156">Zadejte název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-156">Give your cloud service a name.</span></span> <span data-ttu-id="6c3d3-157">Úplnou adresu URL se zobrazí vedle názvu.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-157">The complete URL appears next to the name.</span></span>
7. <span data-ttu-id="6c3d3-158">V seznamu oblastí vyberte v oblasti, kde se nachází většina uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-158">In the list of regions, choose a region where most of your users are located.</span></span>
8. <span data-ttu-id="6c3d3-159">Zadejte, zda chcete aktivovat geografickou replikaci.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-159">Specify whether you want to enable geo-replication.</span></span> <span data-ttu-id="6c3d3-160">Pokud povolíte geografická replikace, data se uloží do více fyzických umístění, abyste snížili riziko ztráty.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-160">If you enable geo-replication, your data will be saved in multiple physical locations to reduce the chance of loss.</span></span> <span data-ttu-id="6c3d3-161">Díky této funkci úložiště dražší, ale může snížit náklady na povolením geografického umístění, při vytváření účtu úložiště místo přidávání funkci později.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-161">This feature makes storage more expensive, but you can reduce the cost by enabling geo-location when you create the storage account instead of adding the feature later.</span></span> <span data-ttu-id="6c3d3-162">Další informace najdete v tématu [geografická replikace](http://go.microsoft.com/fwlink/?LinkId=253108).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-162">For more information, see [Geo-replication](http://go.microsoft.com/fwlink/?LinkId=253108).</span></span>
9. <span data-ttu-id="6c3d3-163">V dolní části okna, vyberte **vytvořit účet úložiště** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-163">At the bottom of the window, choose the **Create Storage Account** link.</span></span>

<span data-ttu-id="6c3d3-164">Po vytvoření účtu úložiště, zobrazí se adresy URL, které můžete použít pro přístup k prostředkům v každé služby Azure storage a primární a sekundární přístupové klíče pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-164">After you create your storage account, you will see the URLs that you can use to access resources in each of the Azure storage services, and also the primary and secondary access keys for your account.</span></span> <span data-ttu-id="6c3d3-165">Tyto klíče se používají k ověření požadavků na služby úložiště přístup.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-165">You use these keys to authenticate requests made against the storage services.</span></span>

> [!NOTE]
> <span data-ttu-id="6c3d3-166">Sekundární přístupový klíč poskytuje stejný přístup k účtu úložiště jako primární přístupový klíč a je generována jako záložní ohrožené primární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-166">The secondary access key provides the same access to your storage account as the primary access key and is generated as a backup should your primary access key be compromised.</span></span> <span data-ttu-id="6c3d3-167">Kromě toho se doporučuje znovu vygenerovat klíče pro přístup k v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-167">Additionally, it is recommended that you regenerate your access keys on a regular basis.</span></span> <span data-ttu-id="6c3d3-168">Můžete upravit nastavení připojovacího řetězce používat sekundární klíč obnovit primární klíč, pak můžete upravit ji používat se znova vygeneroval primární klíč obnovit sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-168">You can modify a connection string setting to use the secondary key while you regenerate the primary key, then you can modify it to use the regenerated primary key while you regenerate the secondary key.</span></span>
>
>

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a><span data-ttu-id="6c3d3-169">Konfigurace aplikace pro službu pro zadaný účet úložiště</span><span class="sxs-lookup"><span data-stu-id="6c3d3-169">Configure your app to use services provided by the storage account</span></span>
<span data-ttu-id="6c3d3-170">Je nutné nakonfigurovat všechny role, který přistupuje k služeb pro úložiště na použití služby Azure storage, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-170">You must configure any role that accesses storage services to use the Azure storage services that you have created.</span></span> <span data-ttu-id="6c3d3-171">K tomuto účelu můžete použít více konfigurace služby pro svůj projekt Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-171">To do this, you can use multiple service configurations for your Azure project.</span></span> <span data-ttu-id="6c3d3-172">Ve výchozím nastavení dva jsou vytvořené v projektu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-172">By default, two are created in your Azure project.</span></span> <span data-ttu-id="6c3d3-173">Pomocí několika konfigurace služby, můžete použít stejný připojovací řetězec v kódu, ale mít jinou hodnotu pro připojovací řetězec v každé konfiguraci služby.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-173">By using multiple service configurations, you can use the same connection string in your code, but have a different value for a connection string in each service configuration.</span></span> <span data-ttu-id="6c3d3-174">Například můžete použít jednu konfiguraci služby pro spouštění a ladění aplikace místně pomocí emulátoru úložiště Azure a konfiguraci jinou službu, kterou chcete publikovat svoji aplikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-174">For example, you can use one service configuration to run and debug your application locally using the Azure storage emulator and a different service configuration to publish your application to Azure.</span></span> <span data-ttu-id="6c3d3-175">Další informace o konfiguracích služby najdete v tématu [konfigurace si Azure pomocí více služby konfigurace projektu](vs-azure-tools-multiple-services-project-configurations.md).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-175">For more information about service configurations, see [Configuring Your Azure Project Using Multiple Service Configurations](vs-azure-tools-multiple-services-project-configurations.md).</span></span>

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a><span data-ttu-id="6c3d3-176">Konfigurace aplikace k používání služby, které poskytuje účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="6c3d3-176">To configure your application to use services that the storage account provides</span></span>
1. <span data-ttu-id="6c3d3-177">V sadě Visual Studio otevřete řešení Azure.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-177">In Visual Studio open your Azure solution.</span></span> <span data-ttu-id="6c3d3-178">V Průzkumníku řešení otevřete místní nabídku pro každou roli v projektu Azure, který přistupuje k služby storage a zvolte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-178">In Solution Explorer, open the shortcut menu for each role in your Azure project that accesses the storage services and choose **Properties**.</span></span> <span data-ttu-id="6c3d3-179">Na stránce s názvem role se zobrazí v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-179">A page with the name of the role is displayed in the Visual Studio editor.</span></span> <span data-ttu-id="6c3d3-180">Na stránce se zobrazí pole pro **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-180">The page displays the fields for the **Configuration** tab.</span></span>
2. <span data-ttu-id="6c3d3-181">Na stránkách vlastností pro roli, zvolte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-181">In the property pages for the role, choose **Settings**.</span></span>
3. <span data-ttu-id="6c3d3-182">V **konfigurace služby** seznamu, vyberte název konfigurace služby, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-182">In the **Service Configuration** list, choose the name of the service configuration that you want to edit.</span></span> <span data-ttu-id="6c3d3-183">Pokud chcete změnit všechny konfigurace služby pro tuto roli, můžete vybrat **všechny konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-183">If you want to make changes to all of the service configurations for this role, you can choose **All Configurations**.</span></span>  <span data-ttu-id="6c3d3-184">Další informace o aktualizaci konfigurace služby, najdete v části **spravovat připojovací řetězce pro účty úložiště** v tématu [konfiguraci rolí pro cloudové služby Azure pomocí sady Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-184">For more information about how to update service configurations, see the section **Manage Connection Strings for Storage Accounts** in the topic [Configure the Roles for an Azure Cloud Service with Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>
4. <span data-ttu-id="6c3d3-185">Chcete-li změnit jakékoli nastavení připojovacího řetězce, vyberte **...**</span><span class="sxs-lookup"><span data-stu-id="6c3d3-185">To modify any connection string settings, choose the **…**</span></span> <span data-ttu-id="6c3d3-186">tlačítko vedle nastavení.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-186">button next to the setting.</span></span> <span data-ttu-id="6c3d3-187">**Vytvoření připojovacího řetězce úložiště** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-187">The **Create Storage Connection String** dialog box appears.</span></span>
5. <span data-ttu-id="6c3d3-188">V části **připojit pomocí**, vyberte **vaše předplatné** možnost.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-188">Under **Connect using**, choose the **Your subscription** option.</span></span>
6. <span data-ttu-id="6c3d3-189">V **předplatné** vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-189">In the **Subscription** list, choose your subscription.</span></span> <span data-ttu-id="6c3d3-190">Pokud seznam předplatných neobsahuje ten, který chcete, vyberte **stáhnout nastavení publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-190">If the list of subscriptions doesn't include the one that you want, choose the **Download Publish Settings** link.</span></span>
7. <span data-ttu-id="6c3d3-191">V **název účtu** seznamu, vyberte název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-191">In the **Account name** list, choose your storage account name.</span></span> <span data-ttu-id="6c3d3-192">Přihlašovací údaje účtu úložiště Azure nástroje automaticky získá pomocí souboru .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-192">Azure Tools obtains storage account credentials automatically by using the .publishsettings file.</span></span> <span data-ttu-id="6c3d3-193">Chcete-li ručně zadat přihlašovací údaje účtu úložiště, zvolte **ručně zadali přihlašovací údaje** možnost a potom pokračujte v tomto postupu.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-193">To specify your storage account credentials manually, choose the **Manually entered credentials** option, and then continue with this procedure.</span></span> <span data-ttu-id="6c3d3-194">Název účtu úložiště a primární klíč z můžete získat [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-194">You can get your storage account name and primary key from the [Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span> <span data-ttu-id="6c3d3-195">Pokud nechcete zadat účtu úložiště ručně, vyberte nastavení **OK** tlačítko dialogové okno zavřete.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-195">If you don’t want to specify your storage account settings manually, choose the **OK** button to close the dialog box.</span></span>
8. <span data-ttu-id="6c3d3-196">Vyberte **zadejte účet úložiště** odkaz přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-196">Choose the **Enter storage account** credentials link.</span></span>
9. <span data-ttu-id="6c3d3-197">V **název účtu** pole, zadejte název svého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-197">In the **Account name** box, enter the name of your storage account.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6c3d3-198">Přihlaste se [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885)a potom zvolte **úložiště** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-198">Log into the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), and then choose the **Storage** button.</span></span> <span data-ttu-id="6c3d3-199">Portál zobrazuje seznam účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-199">The portal shows a list of storage accounts.</span></span> <span data-ttu-id="6c3d3-200">Pokud si zvolíte účet, otevře se stránka pro ni.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-200">If you choose an account, a page for it opens.</span></span> <span data-ttu-id="6c3d3-201">Název účtu úložiště můžete zkopírovat z této stránky.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-201">You can copy the name of the storage account from this page.</span></span> <span data-ttu-id="6c3d3-202">Pokud používáte předchozí verzi portálu classic, název účtu úložiště se zobrazí v **účty úložiště** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-202">If you are using a previous version of the classic portal, the name of your storage account appears in the **Storage Accounts** view.</span></span> <span data-ttu-id="6c3d3-203">Zkopírujte tento název, zvýrazněte ho v **vlastnosti** okno tohoto zobrazení a potom vyberte Ctrl-C klíče.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-203">To copy this name, highlight it in the **Properties** window of this view, and then choose the Ctrl-C keys.</span></span> <span data-ttu-id="6c3d3-204">Pokud chcete vložit název do sady Visual Studio, vyberte **název účtu** textové pole a potom vyberte klávesy Ctrl + V.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-204">To paste the name into Visual Studio, choose the **Account name** text box, and then choose the Ctrl+V keys.</span></span>
   >
   >
10. <span data-ttu-id="6c3d3-205">V **klíč účtu** pole, zadejte primární klíč, nebo zkopírujte a vložte ho [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-205">In the **Account key** box, enter your primary key, or copy and paste it from the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>
     <span data-ttu-id="6c3d3-206">Zkopírujte tento klíč:</span><span class="sxs-lookup"><span data-stu-id="6c3d3-206">To copy this key:</span></span>

    1. <span data-ttu-id="6c3d3-207">V dolní části stránky pro účet odpovídající úložiště, vyberte **Správa klíčů** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-207">At the bottom of the page for the appropriate storage account, choose the **Manage Keys** button.</span></span>
    2. <span data-ttu-id="6c3d3-208">Na **spravovat přístup klíče** vyberte text primární přístupový klíč a potom vyberte klávesy Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-208">On the **Manage Keys Access** page, select the text of the primary access key, and then choose the Ctrl+C keys.</span></span>
    3. <span data-ttu-id="6c3d3-209">Do nástroje Azure, vložte klíč do **klíč účtu** pole.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-209">In Azure Tools, paste the key into the **Account key** box.</span></span>
    4. <span data-ttu-id="6c3d3-210">Musíte vybrat jednu z následujících možností služby budou pro přístup k účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="6c3d3-210">You must select one of the following options to determine how the service will access the storage account:</span></span>

       * <span data-ttu-id="6c3d3-211">**Prostřednictvím protokolu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-211">**Use HTTP**.</span></span> <span data-ttu-id="6c3d3-212">Toto je možnost Standardní.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-212">This is the standard option.</span></span> <span data-ttu-id="6c3d3-213">Například, `http://<account name>.blob.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-213">For example, `http://<account name>.blob.core.windows.net`.</span></span>
       * <span data-ttu-id="6c3d3-214">**Použití protokolu HTTPS** pro zabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-214">**Use HTTPS** for a secure connection.</span></span> <span data-ttu-id="6c3d3-215">Například, `https://<accountname>.blob.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-215">For example, `https://<accountname>.blob.core.windows.net`.</span></span>
       * <span data-ttu-id="6c3d3-216">**Zadejte vlastní koncové body** pro všechny tři služby.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-216">**Specify custom endpoints** for each of the three services.</span></span> <span data-ttu-id="6c3d3-217">Potom můžete zadat těchto koncových bodů do pole pro konkrétní službu.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-217">You can then type these endpoints into the field for the specific service.</span></span>

         > [!NOTE]
         > <span data-ttu-id="6c3d3-218">Pokud vytvoříte vlastní koncové body, můžete vytvořit složitější připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-218">If you create custom endpoints, you can create a more complex connection string.</span></span> <span data-ttu-id="6c3d3-219">Pokud použijete tento formát řetězce, můžete zadat koncové body služby úložiště, které obsahují název vlastní domény, který je zaregistrovaný pro váš účet úložiště se službou objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-219">When you use this string format, you can specify storage service endpoints that include a custom domain name that you have registered for your storage account with the Blob service.</span></span> <span data-ttu-id="6c3d3-220">Také můžete udělit přístup jen k prostředkům blob v jediný kontejner prostřednictvím sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-220">Also you can grant access only to blob resources in a single container through a shared access signature.</span></span> <span data-ttu-id="6c3d3-221">Další informace o tom, jak vytvořit vlastní koncové body najdete v tématu [nakonfigurování připojovacích řetězců úložiště Azure](storage/common/storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-221">For more information about how to create custom endpoints, see [Configure Azure Storage Connection Strings](storage/common/storage-configure-connection-string.md).</span></span>
         >
         >
11. <span data-ttu-id="6c3d3-222">Pokud chcete uložit tyto změny řetězec připojení, vyberte **OK** tlačítko a potom zvolte **Uložit** tlačítka na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-222">To save these connection string changes, choose the **OK** button and then choose the **Save** button on the toolbar.</span></span> <span data-ttu-id="6c3d3-223">Po uložení těchto změn, můžete získat hodnotu tento připojovací řetězec v kódu pomocí [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-223">After you save these changes, you can get the value of this connection string in your code by using [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx).</span></span> <span data-ttu-id="6c3d3-224">Když publikujete aplikaci do Azure, zvolte konfigurace služby, který obsahuje účet úložiště Azure pro připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="6c3d3-224">When you publish your application to Azure, choose the service configuration that contains the Azure storage account for the connection string.</span></span> <span data-ttu-id="6c3d3-225">Po publikování aplikace, ověřte, že aplikace funguje podle očekávání proti služby Azure storage</span><span class="sxs-lookup"><span data-stu-id="6c3d3-225">After your application is published, verify that the application works as expected against the Azure storage services</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c3d3-226">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c3d3-226">Next steps</span></span>
<span data-ttu-id="6c3d3-227">Další informace o publikování aplikace do Azure ze sady Visual Studio, najdete v části [publikování cloudové služby pomocí nástroje Azure](vs-azure-tools-publishing-a-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="6c3d3-227">To learn more about publishing apps to Azure from Visual Studio, see [Publishing a Cloud Service using the Azure Tools](vs-azure-tools-publishing-a-cloud-service.md).</span></span>