---
title: "role hello aaaConfigure Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooset až a konfigurovat role pro cloudové služby Azure pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: d3c62eb57040ebe987787e73b17b468bb82122bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a><span data-ttu-id="4948b-103">Konfigurace role Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4948b-103">Configure Azure cloud service roles with Visual Studio</span></span>
<span data-ttu-id="4948b-104">Cloudové služby Azure může mít jeden nebo více pracovních nebo webové role.</span><span class="sxs-lookup"><span data-stu-id="4948b-104">An Azure cloud service can have one or more worker or web roles.</span></span> <span data-ttu-id="4948b-105">Pro každou roli potřebovat toodefine nastavení dané role a taky nakonfigurovat, jak tato role běží.</span><span class="sxs-lookup"><span data-stu-id="4948b-105">For each role, you need toodefine how that role is set up and also configure how that role runs.</span></span> <span data-ttu-id="4948b-106">toolearn Další informace o rolí v cloudových služeb, najdete v části hello video [tooAzure Úvod cloudové služby](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span><span class="sxs-lookup"><span data-stu-id="4948b-106">toolearn more about roles in cloud services, see hello video [Introduction tooAzure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span></span> 

<span data-ttu-id="4948b-107">Hello informace pro cloudové služby jsou uloženy v hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="4948b-107">hello information for your cloud service is stored in hello following files:</span></span>

- <span data-ttu-id="4948b-108">**ServiceDefinition.csdef** -souboru definice služby hello definuje hello nastavení běhového prostředí pro cloudové služby, včetně toho, jaké role jsou požadovány, koncových bodů a velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4948b-108">**ServiceDefinition.csdef** - hello service definition file defines hello runtime settings for your cloud service including what roles are required, endpoints, and virtual machine size.</span></span> <span data-ttu-id="4948b-109">Žádná z hello data uložená v `ServiceDefinition.csdef` jde změnit, pokud vaše role běží.</span><span class="sxs-lookup"><span data-stu-id="4948b-109">None of hello data stored in `ServiceDefinition.csdef` can be changed when your role is running.</span></span>
- <span data-ttu-id="4948b-110">**Souboru ServiceConfiguration.cscfg** – konfigurační soubor služby hello konfiguruje, jak velký počet instancí role spouštějí a hello hodnoty hello nastavení definované pro roli.</span><span class="sxs-lookup"><span data-stu-id="4948b-110">**ServiceConfiguration.cscfg** - hello service configuration file configures how many instances of a role are run and hello values of hello settings defined for a role.</span></span> <span data-ttu-id="4948b-111">Hello data uložená v `ServiceConfiguration.cscfg` lze změnit, když vaše role běží.</span><span class="sxs-lookup"><span data-stu-id="4948b-111">hello data stored in `ServiceConfiguration.cscfg` can be changed while your role is running.</span></span>

<span data-ttu-id="4948b-112">toostore různé hodnoty pro hello nastavení, které určují běh roli, můžete definovat více konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="4948b-112">toostore different values for hello settings that control how a role runs, you can define multiple service configurations.</span></span> <span data-ttu-id="4948b-113">Konfigurace různé služby můžete použít pro jednotlivá prostředí nasazení.</span><span class="sxs-lookup"><span data-stu-id="4948b-113">You can use a different service configuration for each deployment environment.</span></span> <span data-ttu-id="4948b-114">Můžete například nastavit vašeho úložiště účet připojovací řetězec toouse hello místní emulátoru úložiště Azure v konfiguraci místní služby a vytvořit další toouse konfigurace služby Azure storage v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-114">For example, you can set your storage account connection string toouse hello local Azure storage emulator in a local service configuration and create another service configuration toouse Azure storage in hello cloud.</span></span>

<span data-ttu-id="4948b-115">Při vytváření cloudové služby Azure v sadě Visual Studio, dvě konfigurace služby se automaticky vytvoří a přidány tooyour projektu Azure:</span><span class="sxs-lookup"><span data-stu-id="4948b-115">When you create an Azure cloud service in Visual Studio, two service configurations are automatically created and added tooyour Azure project:</span></span>

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a><span data-ttu-id="4948b-116">Konfigurace Azure cloud service</span><span class="sxs-lookup"><span data-stu-id="4948b-116">Configure an Azure cloud service</span></span>
<span data-ttu-id="4948b-117">Cloudové služby Azure v Průzkumníku řešení v sadě Visual Studio, můžete nakonfigurovat, jak je znázorněno v hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4948b-117">You can configure an Azure cloud service from Solution Explorer in Visual Studio, as shown in hello following steps:</span></span>

1. <span data-ttu-id="4948b-118">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4948b-118">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="4948b-119">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4948b-119">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>
   
    ![Místní nabídku projektu Průzkumník řešení](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. <span data-ttu-id="4948b-121">Na stránce vlastností projektu hello vyberte hello **vývoj** kartě.</span><span class="sxs-lookup"><span data-stu-id="4948b-121">In hello project's properties page, select hello **Development** tab.</span></span> 

    ![Stránka vlastností projektu - karta vývoj](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. <span data-ttu-id="4948b-123">V hello **konfigurace služby** seznamu, vyberte hello název hello konfigurace služby, které chcete tooedit.</span><span class="sxs-lookup"><span data-stu-id="4948b-123">In hello **Service Configuration** list, select hello name of hello service configuration that you want tooedit.</span></span> <span data-ttu-id="4948b-124">(Pokud chcete toomake změny konfigurace služby hello tooall pro tuto roli, vyberte **všechny konfigurace**.)</span><span class="sxs-lookup"><span data-stu-id="4948b-124">(If you want toomake changes tooall hello service configurations for this role, select **All Configurations**.)</span></span>
   
    > [!IMPORTANT]
    > <span data-ttu-id="4948b-125">Pokud si zvolíte specifické služby konfigurace, některé vlastnosti jsou zakázané, protože jejich lze nastavit pouze pro všechny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4948b-125">If you choose a specific service configuration, some properties are disabled because they can only be set for all configurations.</span></span> <span data-ttu-id="4948b-126">tooedit tyto vlastnosti, je nutné vybrat **všechny konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="4948b-126">tooedit these properties, you must select **All Configurations**.</span></span>
    > 
    > 
   
    ![Seznam konfigurace služby pro cloudové služby Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-hello-number-of-role-instances"></a><span data-ttu-id="4948b-128">Změnit hello počet instancí role</span><span class="sxs-lookup"><span data-stu-id="4948b-128">Change hello number of role instances</span></span>
<span data-ttu-id="4948b-129">výkon hello tooimprove cloudové služby, můžete změnit hello počet instancí role, které běží na základě hello počtu uživatelů nebo hello zatížení pro konkrétní roli.</span><span class="sxs-lookup"><span data-stu-id="4948b-129">tooimprove hello performance of your cloud service, you can change hello number of instances of a role that are running, based on hello number of users or hello load expected for a particular role.</span></span> <span data-ttu-id="4948b-130">Samostatný virtuální počítač se vytvoří pro každou instanci role, když hello Cloudová služba běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="4948b-130">A separate virtual machine is created for each instance of a role when hello cloud service runs in Azure.</span></span> <span data-ttu-id="4948b-131">Tato akce ovlivní hello fakturace pro hello nasazení pro tuto cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="4948b-131">This affects hello billing for hello deployment of this cloud service.</span></span> <span data-ttu-id="4948b-132">Další informace o fakturaci, viz [porozumět vaší faktuře pro Microsoft Azure](billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="4948b-132">For more information about billing, see [Understand your bill for Microsoft Azure](billing/billing-understand-your-bill.md).</span></span>

1. <span data-ttu-id="4948b-133">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4948b-133">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="4948b-134">V **Průzkumníku**, rozbalte uzel projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-134">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="4948b-135">V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4948b-135">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="4948b-137">Vyberte hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="4948b-137">Select hello **Configuration** tab.</span></span>

    ![Na kartě Konfigurace](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. <span data-ttu-id="4948b-139">V hello **konfigurace služby** seznamu, vyberte hello konfigurace služby, které chcete tooupdate.</span><span class="sxs-lookup"><span data-stu-id="4948b-139">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>
   
    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. <span data-ttu-id="4948b-141">V hello **Instance počet** textové pole, zadejte hello počet instancí, který má toostart pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="4948b-141">In hello **Instance count** text box, enter hello number of instances that you want toostart for this role.</span></span> <span data-ttu-id="4948b-142">Každá instance běží na samostatný virtuální počítač při publikování hello cloudové služby tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4948b-142">Each instance runs on a separate virtual machine when you publish hello cloud service tooAzure.</span></span>

    ![Aktualizace hello počet instancí](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. <span data-ttu-id="4948b-144">Z hello Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4948b-144">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="manage-connection-strings-for-storage-accounts"></a><span data-ttu-id="4948b-145">Spravovat připojovací řetězce pro účty úložiště</span><span class="sxs-lookup"><span data-stu-id="4948b-145">Manage connection strings for storage accounts</span></span>
<span data-ttu-id="4948b-146">Můžete přidat, odebrat nebo upravit připojovací řetězce pro vaše konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="4948b-146">You can add, remove, or modify connection strings for your service configurations.</span></span> <span data-ttu-id="4948b-147">Například můžete místní připojovací řetězec pro konfiguraci místní služby, který má hodnotu `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="4948b-147">For example, you might want a local connection string for a local service configuration that has a value of `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="4948b-148">Také můžete chtít tooconfigure konfigurace služby cloud, který používá účet úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="4948b-148">You might also want tooconfigure a cloud service configuration that uses a storage account in Azure.</span></span>

> [!WARNING]
> <span data-ttu-id="4948b-149">Když zadáte hello Azure klíčové informace o účtu úložiště pro připojovací řetězec, účet úložiště, tyto informace jsou uloženy místně v konfiguračním souboru služby hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-149">When you enter hello Azure storage account key information for a storage account connection string, this information is stored locally in hello service configuration file.</span></span> <span data-ttu-id="4948b-150">Tyto informace nejsou aktuálně uloženy jako šifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="4948b-150">However, this information is currently not stored as encrypted text.</span></span>
> 
> 

<span data-ttu-id="4948b-151">Pomocí jinou hodnotu pro každou konfiguraci služby nemáte mít toouse různých připojovací řetězce v rámci cloudové služby nebo upravit kód při publikování tooAzure vaše cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="4948b-151">By using a different value for each service configuration, you do not have toouse different connection strings in your cloud service or modify your code when you publish your cloud service tooAzure.</span></span> <span data-ttu-id="4948b-152">Můžete použít stejný název pro hello připojovací řetězec v kódu a hello hodnota se liší podle konfigurace služby hello, kterou jste vybrali při sestavování cloudové služby, nebo pokud ji publikujete hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-152">You can use hello same name for hello connection string in your code and hello value is different, based on hello service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="4948b-153">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4948b-153">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="4948b-154">V **Průzkumníku**, rozbalte uzel projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-154">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="4948b-155">V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4948b-155">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="4948b-157">Vyberte hello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="4948b-157">Select hello **Settings** tab.</span></span>

    ![Karta nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="4948b-159">V hello **konfigurace služby** seznamu, vyberte hello konfigurace služby, které chcete tooupdate.</span><span class="sxs-lookup"><span data-stu-id="4948b-159">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>

    ![Konfigurace služby](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="4948b-161">Vyberte tooadd připojovací řetězec, **přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4948b-161">tooadd a connection string, select **Add Setting**.</span></span>

    ![Přidat připojovací řetězec](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="4948b-163">Po přidání seznamu toohello nové nastavení hello aktualizujte hello řádek v seznamu hello hello potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="4948b-163">Once hello new setting has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Nový připojovací řetězec](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="4948b-165">**Název** – zadejte název hello chcete toouse pro hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4948b-165">**Name** - Enter hello name that you want toouse for hello connection string.</span></span>
    - <span data-ttu-id="4948b-166">**Typ** – vyberte **připojovací řetězec** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-166">**Type** - Select **Connection String** from hello drop-down list.</span></span>
    - <span data-ttu-id="4948b-167">**Hodnota** -hello připojovací řetězec můžete zadat přímo do hello **hodnotu** buňku nebo toowork třemi tečkami (...) vyberte hello v hello **vytvoření připojovacího řetězce úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4948b-167">**Value** - You can either enter hello connection string directly into hello **Value** cell, or select hello ellipsis (...) toowork in hello **Create Storage Connection String** dialog.</span></span>  

1. <span data-ttu-id="4948b-168">V hello **vytvoření připojovacího řetězce úložiště** dialogovém okně, vyberte možnost pro **připojit pomocí**.</span><span class="sxs-lookup"><span data-stu-id="4948b-168">In hello **Create Storage Connection String** dialog, select an option for **Connect using**.</span></span> <span data-ttu-id="4948b-169">Postupujte podle pokynů hello hello možnost, kterou vyberete:</span><span class="sxs-lookup"><span data-stu-id="4948b-169">Then follow hello instructions for hello option you select:</span></span>

    - <span data-ttu-id="4948b-170">**Emulátor úložiště Microsoft Azure** – Pokud vyberete tuto možnost, hello zbývající nastavení v dialogovém okně hello jsou zakázána, protože se vztahují pouze tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4948b-170">**Microsoft Azure storage emulator** - If you select this option, hello remaining settings on hello dialog are disabled as they apply only tooAzure.</span></span> <span data-ttu-id="4948b-171">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="4948b-171">Select **OK**.</span></span>
    - <span data-ttu-id="4948b-172">**Vaše předplatné** – Pokud vyberte tuto možnost, použít hello rozevíracího seznamu tooeither vybrat a přihlášení do účtu Microsoft, nebo přidat účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4948b-172">**Your subscription** - If you select this option, use hello dropdown list tooeither select and sign into a Microsoft account, or add a Microsoft account.</span></span> <span data-ttu-id="4948b-173">Vyberte účet předplatného a úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4948b-173">Select an Azure subscription and storage account.</span></span> <span data-ttu-id="4948b-174">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="4948b-174">Select **OK**.</span></span>
    - <span data-ttu-id="4948b-175">**Ručně zadali přihlašovací údaje** – zadejte název účtu úložiště hello a buď hello primární a druhý klíč.</span><span class="sxs-lookup"><span data-stu-id="4948b-175">**Manually entered credentials** - Enter hello storage account name, and either hello primary or second key.</span></span> <span data-ttu-id="4948b-176">Vyberte možnost pro **připojení** (HTTPS se doporučuje pro většinu scénářů). Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="4948b-176">Select an option for **Connection** (HTTPS is recommended for most scenarios.) Select **OK**.</span></span>

1. <span data-ttu-id="4948b-177">toodelete připojovací řetězec, vyberte hello připojovací řetězec a pak vyberte **odebrat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4948b-177">toodelete a connection string, select hello connection string, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="4948b-178">Z hello Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4948b-178">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-connection-string"></a><span data-ttu-id="4948b-179">Programový přístup připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="4948b-179">Programmatically access a connection string</span></span>

<span data-ttu-id="4948b-180">Hello následující kroky ukazují, jak tooprogrammatically přístup připojovací řetězec pomocí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="4948b-180">hello following steps show how tooprogrammatically access a connection string using C#.</span></span>

1. <span data-ttu-id="4948b-181">Přidejte následující hello pomocí souboru tooa C# direktivy kam toouse hello nastavení:</span><span class="sxs-lookup"><span data-stu-id="4948b-181">Add hello following using directives tooa C# file where you are going toouse hello setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="4948b-182">Hello následující kód ukazuje příklad tooaccess připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4948b-182">hello following code illustrates an example of how tooaccess a connection string.</span></span> <span data-ttu-id="4948b-183">Nahraďte hello &lt;ConnectionStringName > zástupný symbol hello odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4948b-183">Replace hello &lt;ConnectionStringName> placeholder with hello appropriate value.</span></span> 

    ```csharp
    // Setup hello connection tooAzure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-toouse-in-your-azure-cloud-service"></a><span data-ttu-id="4948b-184">Přidat vlastní nastavení toouse ve službě Azure cloud</span><span class="sxs-lookup"><span data-stu-id="4948b-184">Add custom settings toouse in your Azure cloud service</span></span>
<span data-ttu-id="4948b-185">Vlastní nastavení v konfiguračním souboru služby hello umožňuje přidat název a hodnotu řetězce pro konfiguraci konkrétní služby.</span><span class="sxs-lookup"><span data-stu-id="4948b-185">Custom settings in hello service configuration file let you add a name and value for a string for a specific service configuration.</span></span> <span data-ttu-id="4948b-186">Můžete zvolit, toouse tato nastavení tooconfigure funkce v cloudové službě načtením hello hodnotu hello nastavení a použití této hodnoty toocontrol hello logiky v kódu.</span><span class="sxs-lookup"><span data-stu-id="4948b-186">You might choose toouse this setting tooconfigure a feature in your cloud service by reading hello value of hello setting and using this value toocontrol hello logic in your code.</span></span> <span data-ttu-id="4948b-187">Tyto hodnoty konfigurace služby můžete změnit bez nutnosti toorebuild vašeho balíčku služby, nebo když cloudové služby běží.</span><span class="sxs-lookup"><span data-stu-id="4948b-187">You can change these service configuration values without having toorebuild your service package or when your cloud service is running.</span></span> <span data-ttu-id="4948b-188">Kódu můžete zkontrolovat pro oznámení, když se změní nastavení.</span><span class="sxs-lookup"><span data-stu-id="4948b-188">Your code can check for notifications of when a setting changes.</span></span> <span data-ttu-id="4948b-189">V tématu [RoleEnvironment.Changing událostí](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span><span class="sxs-lookup"><span data-stu-id="4948b-189">See [RoleEnvironment.Changing Event](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span></span>

<span data-ttu-id="4948b-190">Můžete přidat, odebrat nebo upravit vlastní nastavení pro vaše konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="4948b-190">You can add, remove, or modify custom settings for your service configurations.</span></span> <span data-ttu-id="4948b-191">Můžete chtít různé hodnoty pro tyto řetězce pro různé služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4948b-191">You might want different values for these strings for different service configurations.</span></span>

<span data-ttu-id="4948b-192">Pomocí jinou hodnotu pro každou konfiguraci služby nemáte máte toouse jiné řetězce v rámci cloudové služby nebo upravit kód při publikování tooAzure vaše cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="4948b-192">By using a different value for each service configuration, you do not have toouse different strings in your cloud service or modify your code when you publish your cloud service tooAzure.</span></span> <span data-ttu-id="4948b-193">Můžete použít stejný název pro hello řetězec v kódu a hello hodnota se liší podle konfigurace služby hello, kterou jste vybrali při sestavování cloudové služby, nebo pokud ji publikujete hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-193">You can use hello same name for hello string in your code and hello value is different, based on hello service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="4948b-194">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4948b-194">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="4948b-195">V **Průzkumníku**, rozbalte uzel projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-195">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="4948b-196">V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4948b-196">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="4948b-198">Vyberte hello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="4948b-198">Select hello **Settings** tab.</span></span>

    ![Karta nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="4948b-200">V hello **konfigurace služby** seznamu, vyberte hello konfigurace služby, které chcete tooupdate.</span><span class="sxs-lookup"><span data-stu-id="4948b-200">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>

    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="4948b-202">tooadd vlastního nastavení, vyberte **přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4948b-202">tooadd a custom setting, select **Add Setting**.</span></span>

    ![Přidat vlastní nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="4948b-204">Po přidání seznamu toohello nové nastavení hello aktualizujte hello řádek v seznamu hello hello potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="4948b-204">Once hello new setting has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Nové vlastní nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="4948b-206">**Název** – zadejte název hello hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="4948b-206">**Name** - Enter hello name of hello setting.</span></span>
    - <span data-ttu-id="4948b-207">**Typ** – vyberte **řetězec** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-207">**Type** - Select **String** from hello drop-down list.</span></span>
    - <span data-ttu-id="4948b-208">**Hodnota** – zadejte hodnotu hello hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="4948b-208">**Value** - Enter hello value of hello setting.</span></span> <span data-ttu-id="4948b-209">Můžete buď zadat hello hodnotu přímo do hello **hodnotu** buňky, nebo vyberte hello třemi tečkami (...) tooenter hello hodnota ve hello **Upravit řetězec** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4948b-209">You can either enter hello value directly into hello **Value** cell, or select hello ellipsis (...) tooenter hello value in hello **Edit String** dialog.</span></span>  

1. <span data-ttu-id="4948b-210">Vyberte nastavení hello toodelete vlastního nastavení a pak vyberte **odebrat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4948b-210">toodelete a custom setting, select hello setting, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="4948b-211">Z hello Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4948b-211">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-custom-settings-value"></a><span data-ttu-id="4948b-212">Programový přístup hodnoty vlastní nastavení</span><span class="sxs-lookup"><span data-stu-id="4948b-212">Programmatically access a custom setting's value</span></span>
 
<span data-ttu-id="4948b-213">Hello následující kroky ukazují, jak tooprogrammatically přístup vlastního nastavení pomocí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="4948b-213">hello following steps show how tooprogrammatically access a custom setting using C#.</span></span>

1. <span data-ttu-id="4948b-214">Přidejte následující hello pomocí souboru tooa C# direktivy kam toouse hello nastavení:</span><span class="sxs-lookup"><span data-stu-id="4948b-214">Add hello following using directives tooa C# file where you are going toouse hello setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="4948b-215">Hello následující kód ukazuje příklad tooaccess vlastního nastavení.</span><span class="sxs-lookup"><span data-stu-id="4948b-215">hello following code illustrates an example of how tooaccess a custom setting.</span></span> <span data-ttu-id="4948b-216">Nahraďte hello &lt;SettingName > zástupný symbol hello odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4948b-216">Replace hello &lt;SettingName> placeholder with hello appropriate value.</span></span> 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a><span data-ttu-id="4948b-217">Spravovat místní úložiště pro každou instanci role</span><span class="sxs-lookup"><span data-stu-id="4948b-217">Manage local storage for each role instance</span></span>
<span data-ttu-id="4948b-218">Můžete přidat úložiště systému místní soubor pro každou instanci role.</span><span class="sxs-lookup"><span data-stu-id="4948b-218">You can add local file system storage for each instance of a role.</span></span> <span data-ttu-id="4948b-219">Hello datům uloženým v úložiště není přístupné ostatní instance hello role, pro které hello data uložená nebo jiné role.</span><span class="sxs-lookup"><span data-stu-id="4948b-219">hello data stored in that storage is not accessible by other instances of hello role for which hello data is stored, or by other roles.</span></span>  

1. <span data-ttu-id="4948b-220">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4948b-220">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="4948b-221">V **Průzkumníku**, rozbalte uzel projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-221">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="4948b-222">V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4948b-222">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="4948b-224">Vyberte hello **místní úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="4948b-224">Select hello **Local Storage** tab.</span></span>

    ![Karta místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. <span data-ttu-id="4948b-226">V hello **konfigurace služby** seznamu, ujistěte se, že **všechny konfigurace** je vybrán jako nastavení místní úložiště hello použít tooall služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4948b-226">In hello **Service Configuration** list, ensure that **All Configurations** is selected as hello local storage settings apply tooall service configurations.</span></span> <span data-ttu-id="4948b-227">Jakákoli jiná hodnota výsledkem všechny hello vstupních polí na stránce hello zakázaná.</span><span class="sxs-lookup"><span data-stu-id="4948b-227">Any other value results in all hello input fields on hello page being disabled.</span></span> 

    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. <span data-ttu-id="4948b-229">Vyberte položku místní úložiště tooadd **přidat místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4948b-229">tooadd a local storage entry, select **Add Local Storage**.</span></span>

    ![Přidejte místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. <span data-ttu-id="4948b-231">Po přidání seznamu toohello hello nové místní úložiště položku aktualizujte hello řádek v seznamu hello hello potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="4948b-231">Once hello new local storage entry has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Nová položka místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - <span data-ttu-id="4948b-233">**Název** – zadejte název hello má toouse pro hello nové místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="4948b-233">**Name** - Enter hello name that you want toouse for hello new local storage.</span></span>
    - <span data-ttu-id="4948b-234">**Velikost (MB)** – zadejte velikost hello v MB, kterou potřebujete pro hello nové místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="4948b-234">**Size (MB)** - Enter hello size in MB that you need for hello new local storage.</span></span>
    - <span data-ttu-id="4948b-235">**Čištění na role recyklaci** – vyberte tuto možnost tooremove hello data v nové místní úložiště hello po recyklaci hello virtuálního počítače pro roli hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-235">**Clean on role recycle** - Select this option tooremove hello data in hello new local storage when hello virtual machine for hello role is recycled.</span></span>

1. <span data-ttu-id="4948b-236">Vyberte položku hello toodelete položku místní úložiště a potom vyberte **odebrat místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4948b-236">toodelete a local storage entry, select hello entry, and then select **Remove Local Storage**.</span></span>

1. <span data-ttu-id="4948b-237">Z hello Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4948b-237">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-accessing-local-storage"></a><span data-ttu-id="4948b-238">Prostřednictvím kódu programu přístup k místnímu úložišti</span><span class="sxs-lookup"><span data-stu-id="4948b-238">Programmatically accessing local storage</span></span>

<span data-ttu-id="4948b-239">Tato část ukazuje, jak tooprogrammatically přistupovat k místnímu úložišti pomocí jazyka C# vytvořením textového souboru testovací `MyLocalStorageTest.txt`.</span><span class="sxs-lookup"><span data-stu-id="4948b-239">This section illustrates how tooprogrammatically access local storage using C# by writing a test text file `MyLocalStorageTest.txt`.</span></span>  

### <a name="write-a-text-file-toolocal-storage"></a><span data-ttu-id="4948b-240">Zápis textu souboru toolocal úložiště</span><span class="sxs-lookup"><span data-stu-id="4948b-240">Write a text file toolocal storage</span></span>

<span data-ttu-id="4948b-241">Hello následující kód ukazuje příklad jak toowrite textový toolocal úložiště souborů.</span><span class="sxs-lookup"><span data-stu-id="4948b-241">hello following code shows an example of how toowrite a text file toolocal storage.</span></span> <span data-ttu-id="4948b-242">Nahraďte hello &lt;LocalStorageName > zástupný symbol hello odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4948b-242">Replace hello &lt;LocalStorageName> placeholder with hello appropriate value.</span></span> 

    ```csharp
    // Retrieve an object that points toohello local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define hello file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-toolocal-storage"></a><span data-ttu-id="4948b-243">Vyhledat soubor zapsána toolocal úložiště</span><span class="sxs-lookup"><span data-stu-id="4948b-243">Find a file written toolocal storage</span></span>

<span data-ttu-id="4948b-244">soubor hello tooview vytvořené hello kód v předchozí části hello, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4948b-244">tooview hello file created by hello code in hello previous section, follow these steps:</span></span>
    
1.  <span data-ttu-id="4948b-245">V oznamovací oblasti systému Windows hello, klikněte pravým tlačítkem na hello Azure – ikona a, hello místní nabídce vyberte **zobrazit uživatelské prostředí emulátoru výpočtů**.</span><span class="sxs-lookup"><span data-stu-id="4948b-245">In hello Windows notification area, right-click hello Azure icon, and, from hello context menu, select **Show Compute Emulator UI**.</span></span> 

    ![Zobrazit emulátoru služby výpočty Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. <span data-ttu-id="4948b-247">Vyberte roli webový hello.</span><span class="sxs-lookup"><span data-stu-id="4948b-247">Select hello web role.</span></span>

    ![Emulátoru služby výpočty Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. <span data-ttu-id="4948b-249">Na hello **Microsoft Azure výpočetní emulátor** nabídce vyberte možnost **nástroje** > **otevřete místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4948b-249">On hello **Microsoft Azure Compute Emulator** menu, select **Tools** > **Open local store**.</span></span>

    ![Položky nabídky otevřete místního úložiště.](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. <span data-ttu-id="4948b-251">Když se otevře okno Průzkumníka Windows hello, zadejte ' MyLocalStorageTest.txt'' do hello **vyhledávání** textového pole a vyberte **Enter** toostart hello vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4948b-251">When hello Windows Explorer window opens, enter `MyLocalStorageTest.txt`\` into hello **Search** text box, and select **Enter** toostart hello search.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4948b-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4948b-252">Next steps</span></span>
<span data-ttu-id="4948b-253">Další informace o Azure projekty v sadě Visual Studio načtením [konfigurace projektu Azure](vs-azure-tools-configuring-an-azure-project.md).</span><span class="sxs-lookup"><span data-stu-id="4948b-253">Learn more about Azure projects in Visual Studio by reading [Configuring an Azure Project](vs-azure-tools-configuring-an-azure-project.md).</span></span> <span data-ttu-id="4948b-254">Další informace o hello cloudové služby schéma načtením [– odkaz schématu](https://msdn.microsoft.com/library/azure/dd179398).</span><span class="sxs-lookup"><span data-stu-id="4948b-254">Learn more about hello cloud service schema by reading [Schema Reference](https://msdn.microsoft.com/library/azure/dd179398).</span></span>

