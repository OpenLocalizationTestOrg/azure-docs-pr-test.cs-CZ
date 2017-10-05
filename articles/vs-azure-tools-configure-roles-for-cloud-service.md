---
title: "Konfigurace role pro cloudové služby Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak nastavit a konfigurovat role pro cloudové služby Azure pomocí sady Visual Studio."
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
ms.openlocfilehash: 17da71ac0c5ab9330b9244c0354e4d161d98229e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a><span data-ttu-id="7d5ec-103">Konfigurace role Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d5ec-103">Configure Azure cloud service roles with Visual Studio</span></span>
<span data-ttu-id="7d5ec-104">Cloudové služby Azure může mít jeden nebo více pracovních nebo webové role.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-104">An Azure cloud service can have one or more worker or web roles.</span></span> <span data-ttu-id="7d5ec-105">Pro každou roli musíte definovat nastavení dané role a taky nakonfigurovat, jak tato role běží.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-105">For each role, you need to define how that role is set up and also configure how that role runs.</span></span> <span data-ttu-id="7d5ec-106">Další informace o rolích v cloudové služby najdete v tématu video [Úvod do Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span><span class="sxs-lookup"><span data-stu-id="7d5ec-106">To learn more about roles in cloud services, see the video [Introduction to Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span></span> 

<span data-ttu-id="7d5ec-107">Informace pro cloudové služby jsou uloženy v následujících souborech:</span><span class="sxs-lookup"><span data-stu-id="7d5ec-107">The information for your cloud service is stored in the following files:</span></span>

- <span data-ttu-id="7d5ec-108">**ServiceDefinition.csdef** -definiční soubor služby definuje nastavení běhového prostředí pro cloudové služby, včetně toho, jaké role jsou požadovány, koncových bodů a velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-108">**ServiceDefinition.csdef** - The service definition file defines the runtime settings for your cloud service including what roles are required, endpoints, and virtual machine size.</span></span> <span data-ttu-id="7d5ec-109">Žádná data uložená v `ServiceDefinition.csdef` jde změnit, pokud vaše role běží.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-109">None of the data stored in `ServiceDefinition.csdef` can be changed when your role is running.</span></span>
- <span data-ttu-id="7d5ec-110">**Souboru ServiceConfiguration.cscfg** – konfigurační soubor služby konfiguruje, jak velký počet instancí role spouštějí a hodnoty nastavení definované pro roli.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-110">**ServiceConfiguration.cscfg** - The service configuration file configures how many instances of a role are run and the values of the settings defined for a role.</span></span> <span data-ttu-id="7d5ec-111">Data uložená v `ServiceConfiguration.cscfg` lze změnit, když vaše role běží.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-111">The data stored in `ServiceConfiguration.cscfg` can be changed while your role is running.</span></span>

<span data-ttu-id="7d5ec-112">Pokud chcete uložit různé hodnoty pro nastavení, které řídí, jak role běží, můžete definovat více konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-112">To store different values for the settings that control how a role runs, you can define multiple service configurations.</span></span> <span data-ttu-id="7d5ec-113">Konfigurace různé služby můžete použít pro jednotlivá prostředí nasazení.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-113">You can use a different service configuration for each deployment environment.</span></span> <span data-ttu-id="7d5ec-114">Například můžete nastavit připojovací řetězec účet úložiště můžete použít emulátor místního úložiště Azure v místní službě konfigurace a vytvořte jinou konfiguraci služby pro použití úložiště Azure v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-114">For example, you can set your storage account connection string to use the local Azure storage emulator in a local service configuration and create another service configuration to use Azure storage in the cloud.</span></span>

<span data-ttu-id="7d5ec-115">Při vytváření cloudové služby Azure v sadě Visual Studio, jsou automaticky vytvořen a přidán do projektu Azure dvě konfigurace služby:</span><span class="sxs-lookup"><span data-stu-id="7d5ec-115">When you create an Azure cloud service in Visual Studio, two service configurations are automatically created and added to your Azure project:</span></span>

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a><span data-ttu-id="7d5ec-116">Konfigurace Azure cloud service</span><span class="sxs-lookup"><span data-stu-id="7d5ec-116">Configure an Azure cloud service</span></span>
<span data-ttu-id="7d5ec-117">Cloudové služby Azure v Průzkumníku řešení v sadě Visual Studio, můžete nakonfigurovat, jak je znázorněno v následujícím postupu:</span><span class="sxs-lookup"><span data-stu-id="7d5ec-117">You can configure an Azure cloud service from Solution Explorer in Visual Studio, as shown in the following steps:</span></span>

1. <span data-ttu-id="7d5ec-118">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-118">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="7d5ec-119">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a v místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-119">In **Solution Explorer**, right-click the project, and, from the context menu, select **Properties**.</span></span>
   
    ![Místní nabídku projektu Průzkumník řešení](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. <span data-ttu-id="7d5ec-121">Na stránce vlastností projektu, vyberte **vývoj** kartě.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-121">In the project's properties page, select the **Development** tab.</span></span> 

    ![Stránka vlastností projektu - karta vývoj](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. <span data-ttu-id="7d5ec-123">V **konfigurace služby** seznamu, vyberte název konfigurace služby, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-123">In the **Service Configuration** list, select the name of the service configuration that you want to edit.</span></span> <span data-ttu-id="7d5ec-124">(Pokud chcete změnit konfiguraci služby pro tuto roli, vyberte **všechny konfigurace**.)</span><span class="sxs-lookup"><span data-stu-id="7d5ec-124">(If you want to make changes to all the service configurations for this role, select **All Configurations**.)</span></span>
   
    > [!IMPORTANT]
    > <span data-ttu-id="7d5ec-125">Pokud si zvolíte specifické služby konfigurace, některé vlastnosti jsou zakázané, protože jejich lze nastavit pouze pro všechny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-125">If you choose a specific service configuration, some properties are disabled because they can only be set for all configurations.</span></span> <span data-ttu-id="7d5ec-126">Chcete-li upravit tyto vlastnosti, je nutné vybrat **všechny konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-126">To edit these properties, you must select **All Configurations**.</span></span>
    > 
    > 
   
    ![Seznam konfigurace služby pro cloudové služby Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-the-number-of-role-instances"></a><span data-ttu-id="7d5ec-128">Změnit počet instancí role</span><span class="sxs-lookup"><span data-stu-id="7d5ec-128">Change the number of role instances</span></span>
<span data-ttu-id="7d5ec-129">Pokud chcete zlepšit výkon cloudové služby, můžete změnit počet instancí role, které běží na základě počtu uživatelů nebo zatížení pro konkrétní roli.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-129">To improve the performance of your cloud service, you can change the number of instances of a role that are running, based on the number of users or the load expected for a particular role.</span></span> <span data-ttu-id="7d5ec-130">Samostatný virtuální počítač se vytvoří pro každou instanci role, při spuštění cloudové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-130">A separate virtual machine is created for each instance of a role when the cloud service runs in Azure.</span></span> <span data-ttu-id="7d5ec-131">Tato akce ovlivní fakturace pro nasazení pro tuto cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-131">This affects the billing for the deployment of this cloud service.</span></span> <span data-ttu-id="7d5ec-132">Další informace o fakturaci, viz [porozumět vaší faktuře pro Microsoft Azure](billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="7d5ec-132">For more information about billing, see [Understand your bill for Microsoft Azure](billing/billing-understand-your-bill.md).</span></span>

1. <span data-ttu-id="7d5ec-133">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-133">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="7d5ec-134">V **Průzkumníku**, rozbalte uzel projektu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-134">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="7d5ec-135">V části **role** uzel, klikněte pravým tlačítkem na roli, kterou chcete aktualizovat a v místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-135">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="7d5ec-137">Vyberte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-137">Select the **Configuration** tab.</span></span>

    ![Na kartě Konfigurace](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. <span data-ttu-id="7d5ec-139">V **konfigurace služby** vyberte konfiguraci služby, který chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-139">In the **Service Configuration** list, select the service configuration that you want to update.</span></span>
   
    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. <span data-ttu-id="7d5ec-141">V **Instance počet** textové pole, zadejte počet instancí, které chcete spustit pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-141">In the **Instance count** text box, enter the number of instances that you want to start for this role.</span></span> <span data-ttu-id="7d5ec-142">Každá instance běží na samostatný virtuální počítač při publikování cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-142">Each instance runs on a separate virtual machine when you publish the cloud service to Azure.</span></span>

    ![Aktualizuje se počet instancí](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. <span data-ttu-id="7d5ec-144">Ze sady Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-144">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="manage-connection-strings-for-storage-accounts"></a><span data-ttu-id="7d5ec-145">Spravovat připojovací řetězce pro účty úložiště</span><span class="sxs-lookup"><span data-stu-id="7d5ec-145">Manage connection strings for storage accounts</span></span>
<span data-ttu-id="7d5ec-146">Můžete přidat, odebrat nebo upravit připojovací řetězce pro vaše konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-146">You can add, remove, or modify connection strings for your service configurations.</span></span> <span data-ttu-id="7d5ec-147">Například můžete místní připojovací řetězec pro konfiguraci místní služby, který má hodnotu `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-147">For example, you might want a local connection string for a local service configuration that has a value of `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="7d5ec-148">Můžete také nakonfigurovat služby konfigurace cloudu, který používá účet úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-148">You might also want to configure a cloud service configuration that uses a storage account in Azure.</span></span>

> [!WARNING]
> <span data-ttu-id="7d5ec-149">Když zadáte údaje klíče účtu úložiště Azure pro připojovací řetězce k účtu úložiště, tyto informace jsou uloženy místně v konfiguračním souboru služby.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-149">When you enter the Azure storage account key information for a storage account connection string, this information is stored locally in the service configuration file.</span></span> <span data-ttu-id="7d5ec-150">Tyto informace nejsou aktuálně uloženy jako šifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-150">However, this information is currently not stored as encrypted text.</span></span>
> 
> 

<span data-ttu-id="7d5ec-151">Pomocí jinou hodnotu pro každou konfiguraci služby není muset použít jiné připojovací řetězce v rámci cloudové služby nebo upravit kód při publikování cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-151">By using a different value for each service configuration, you do not have to use different connection strings in your cloud service or modify your code when you publish your cloud service to Azure.</span></span> <span data-ttu-id="7d5ec-152">Můžete použít stejný název pro připojovací řetězec v kódu a hodnota se liší v závislosti na konfiguraci služby, kterou vyberete při sestavování cloudové služby, nebo při jeho publikování.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-152">You can use the same name for the connection string in your code and the value is different, based on the service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="7d5ec-153">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-153">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="7d5ec-154">V **Průzkumníku**, rozbalte uzel projektu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-154">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="7d5ec-155">V části **role** uzel, klikněte pravým tlačítkem na roli, kterou chcete aktualizovat a v místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-155">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="7d5ec-157">Vyberte **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-157">Select the **Settings** tab.</span></span>

    ![Karta nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="7d5ec-159">V **konfigurace služby** vyberte konfiguraci služby, který chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-159">In the **Service Configuration** list, select the service configuration that you want to update.</span></span>

    ![Konfigurace služby](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="7d5ec-161">Chcete-li přidat připojovací řetězec, vyberte **přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-161">To add a connection string, select **Add Setting**.</span></span>

    ![Přidat připojovací řetězec](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="7d5ec-163">Jakmile se nové nastavení byl přidán do seznamu, aktualizujte řádek v seznamu nezbytné informace.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-163">Once the new setting has been added to the list, update the row in the list with the necessary information.</span></span>

    ![Nový připojovací řetězec](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="7d5ec-165">**Název** – zadejte název, který chcete použít pro připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-165">**Name** - Enter the name that you want to use for the connection string.</span></span>
    - <span data-ttu-id="7d5ec-166">**Typ** – vyberte **připojovací řetězec** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-166">**Type** - Select **Connection String** from the drop-down list.</span></span>
    - <span data-ttu-id="7d5ec-167">**Hodnota** – můžete buď zadat připojovací řetězec přímo do **hodnotu** buňky, nebo vyberte se třemi tečkami (...) pro práci v **vytvoření připojovacího řetězce úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-167">**Value** - You can either enter the connection string directly into the **Value** cell, or select the ellipsis (...) to work in the **Create Storage Connection String** dialog.</span></span>  

1. <span data-ttu-id="7d5ec-168">V **vytvoření připojovacího řetězce úložiště** dialogovém okně, vyberte možnost pro **připojit pomocí**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-168">In the **Create Storage Connection String** dialog, select an option for **Connect using**.</span></span> <span data-ttu-id="7d5ec-169">Postupujte podle pokynů pro možnosti, kterou jste vybrali:</span><span class="sxs-lookup"><span data-stu-id="7d5ec-169">Then follow the instructions for the option you select:</span></span>

    - <span data-ttu-id="7d5ec-170">**Emulátor úložiště Microsoft Azure** – Pokud vyberete tuto možnost, zbývající nastavení v dialogovém okně jsou zakázány, protože se vztahují pouze na Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-170">**Microsoft Azure storage emulator** - If you select this option, the remaining settings on the dialog are disabled as they apply only to Azure.</span></span> <span data-ttu-id="7d5ec-171">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-171">Select **OK**.</span></span>
    - <span data-ttu-id="7d5ec-172">**Vaše předplatné** – Pokud vyberete tuto možnost, pomocí rozevíracího seznamu vyberte a přihlaste se k účtu Microsoft nebo přidání účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-172">**Your subscription** - If you select this option, use the dropdown list to either select and sign into a Microsoft account, or add a Microsoft account.</span></span> <span data-ttu-id="7d5ec-173">Vyberte účet předplatného a úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-173">Select an Azure subscription and storage account.</span></span> <span data-ttu-id="7d5ec-174">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-174">Select **OK**.</span></span>
    - <span data-ttu-id="7d5ec-175">**Ručně zadali přihlašovací údaje** – zadejte název účtu úložiště a druhý nebo primární klíč.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-175">**Manually entered credentials** - Enter the storage account name, and either the primary or second key.</span></span> <span data-ttu-id="7d5ec-176">Vyberte možnost pro **připojení** (HTTPS se doporučuje pro většinu scénářů). Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-176">Select an option for **Connection** (HTTPS is recommended for most scenarios.) Select **OK**.</span></span>

1. <span data-ttu-id="7d5ec-177">Chcete-li odstranit připojovací řetězec, vyberte připojovací řetězec a pak vyberte **odebrat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-177">To delete a connection string, select the connection string, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="7d5ec-178">Ze sady Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-178">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-connection-string"></a><span data-ttu-id="7d5ec-179">Programový přístup připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="7d5ec-179">Programmatically access a connection string</span></span>

<span data-ttu-id="7d5ec-180">Následující kroky ukazují, jak k programovému přístupu ke připojovací řetězec pomocí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-180">The following steps show how to programmatically access a connection string using C#.</span></span>

1. <span data-ttu-id="7d5ec-181">Přidejte následující direktivy using do souboru C# kam používá nastavení:</span><span class="sxs-lookup"><span data-stu-id="7d5ec-181">Add the following using directives to a C# file where you are going to use the setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="7d5ec-182">Následující kód ukazuje příklad přístup připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-182">The following code illustrates an example of how to access a connection string.</span></span> <span data-ttu-id="7d5ec-183">Nahraďte &lt;ConnectionStringName > zástupný symbol na odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-183">Replace the &lt;ConnectionStringName> placeholder with the appropriate value.</span></span> 

    ```csharp
    // Setup the connection to Azure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a><span data-ttu-id="7d5ec-184">Přidat vlastní nastavení pro použití v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="7d5ec-184">Add custom settings to use in your Azure cloud service</span></span>
<span data-ttu-id="7d5ec-185">Vlastní nastavení v konfiguračním souboru služby můžete přidat název a hodnotu řetězce pro konfiguraci konkrétní služby.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-185">Custom settings in the service configuration file let you add a name and value for a string for a specific service configuration.</span></span> <span data-ttu-id="7d5ec-186">Můžete zvolit pomocí tohoto nastavení můžete nakonfigurovat funkce v rámci cloudové služby tak, že čtení hodnoty nastavení a použití této hodnoty pro řízení logiku v kódu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-186">You might choose to use this setting to configure a feature in your cloud service by reading the value of the setting and using this value to control the logic in your code.</span></span> <span data-ttu-id="7d5ec-187">Tyto hodnoty konfigurace služby můžete změnit bez nutnosti znovu vytvořit balíček vaší služby nebo při spuštění cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-187">You can change these service configuration values without having to rebuild your service package or when your cloud service is running.</span></span> <span data-ttu-id="7d5ec-188">Kódu můžete zkontrolovat pro oznámení, když se změní nastavení.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-188">Your code can check for notifications of when a setting changes.</span></span> <span data-ttu-id="7d5ec-189">V tématu [RoleEnvironment.Changing událostí](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d5ec-189">See [RoleEnvironment.Changing Event](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span></span>

<span data-ttu-id="7d5ec-190">Můžete přidat, odebrat nebo upravit vlastní nastavení pro vaše konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-190">You can add, remove, or modify custom settings for your service configurations.</span></span> <span data-ttu-id="7d5ec-191">Můžete chtít různé hodnoty pro tyto řetězce pro různé služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-191">You might want different values for these strings for different service configurations.</span></span>

<span data-ttu-id="7d5ec-192">Pomocí jinou hodnotu pro každou konfiguraci služby není muset použít jiné řetězce v rámci cloudové služby nebo upravit kód při publikování cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-192">By using a different value for each service configuration, you do not have to use different strings in your cloud service or modify your code when you publish your cloud service to Azure.</span></span> <span data-ttu-id="7d5ec-193">Můžete použít stejný název pro řetězec v kódu a hodnota se liší v závislosti na konfiguraci služby, kterou vyberete při sestavování cloudové služby, nebo při jeho publikování.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-193">You can use the same name for the string in your code and the value is different, based on the service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="7d5ec-194">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-194">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="7d5ec-195">V **Průzkumníku**, rozbalte uzel projektu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-195">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="7d5ec-196">V části **role** uzel, klikněte pravým tlačítkem na roli, kterou chcete aktualizovat a v místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-196">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="7d5ec-198">Vyberte **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-198">Select the **Settings** tab.</span></span>

    ![Karta nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="7d5ec-200">V **konfigurace služby** vyberte konfiguraci služby, který chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-200">In the **Service Configuration** list, select the service configuration that you want to update.</span></span>

    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="7d5ec-202">Chcete-li přidat vlastní nastavení, vyberte **přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-202">To add a custom setting, select **Add Setting**.</span></span>

    ![Přidat vlastní nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="7d5ec-204">Jakmile se nové nastavení byl přidán do seznamu, aktualizujte řádek v seznamu nezbytné informace.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-204">Once the new setting has been added to the list, update the row in the list with the necessary information.</span></span>

    ![Nové vlastní nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="7d5ec-206">**Název** – zadejte název nastavení.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-206">**Name** - Enter the name of the setting.</span></span>
    - <span data-ttu-id="7d5ec-207">**Typ** – vyberte **řetězec** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-207">**Type** - Select **String** from the drop-down list.</span></span>
    - <span data-ttu-id="7d5ec-208">**Hodnota** – zadejte hodnotu nastavení.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-208">**Value** - Enter the value of the setting.</span></span> <span data-ttu-id="7d5ec-209">Můžete buď zadat hodnotu přímo do **hodnotu** buňky, nebo vyberte se třemi tečkami (...) zadejte hodnotu v **Upravit řetězec** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-209">You can either enter the value directly into the **Value** cell, or select the ellipsis (...) to enter the value in the **Edit String** dialog.</span></span>  

1. <span data-ttu-id="7d5ec-210">Chcete-li odstranit vlastní nastavení, vyberte nastavení a pak vyberte **odebrat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-210">To delete a custom setting, select the setting, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="7d5ec-211">Ze sady Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-211">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-custom-settings-value"></a><span data-ttu-id="7d5ec-212">Programový přístup hodnoty vlastní nastavení</span><span class="sxs-lookup"><span data-stu-id="7d5ec-212">Programmatically access a custom setting's value</span></span>
 
<span data-ttu-id="7d5ec-213">Následující kroky ukazují, jak k programovému přístupu ke vlastního nastavení pomocí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-213">The following steps show how to programmatically access a custom setting using C#.</span></span>

1. <span data-ttu-id="7d5ec-214">Přidejte následující direktivy using do souboru C# kam používá nastavení:</span><span class="sxs-lookup"><span data-stu-id="7d5ec-214">Add the following using directives to a C# file where you are going to use the setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="7d5ec-215">Následující kód ukazuje příklad toho, jak pro přístup k vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-215">The following code illustrates an example of how to access a custom setting.</span></span> <span data-ttu-id="7d5ec-216">Nahraďte &lt;SettingName > zástupný symbol na odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-216">Replace the &lt;SettingName> placeholder with the appropriate value.</span></span> 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a><span data-ttu-id="7d5ec-217">Spravovat místní úložiště pro každou instanci role</span><span class="sxs-lookup"><span data-stu-id="7d5ec-217">Manage local storage for each role instance</span></span>
<span data-ttu-id="7d5ec-218">Můžete přidat úložiště systému místní soubor pro každou instanci role.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-218">You can add local file system storage for each instance of a role.</span></span> <span data-ttu-id="7d5ec-219">Data uložená na tomto úložiště není přístupné ostatní instance role jsou data uložena, nebo jiné role.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-219">The data stored in that storage is not accessible by other instances of the role for which the data is stored, or by other roles.</span></span>  

1. <span data-ttu-id="7d5ec-220">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-220">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="7d5ec-221">V **Průzkumníku**, rozbalte uzel projektu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-221">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="7d5ec-222">V části **role** uzel, klikněte pravým tlačítkem na roli, kterou chcete aktualizovat a v místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-222">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="7d5ec-224">Vyberte **místní úložiště** kartě.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-224">Select the **Local Storage** tab.</span></span>

    ![Karta místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. <span data-ttu-id="7d5ec-226">V **konfigurace služby** seznamu, ujistěte se, že **všechny konfigurace** je vybrán jako místní úložiště nastavení se použije pro všechny konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-226">In the **Service Configuration** list, ensure that **All Configurations** is selected as the local storage settings apply to all service configurations.</span></span> <span data-ttu-id="7d5ec-227">Jakákoli jiná hodnota výsledkem všech vstupních polí na stránce bude zakázán.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-227">Any other value results in all the input fields on the page being disabled.</span></span> 

    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. <span data-ttu-id="7d5ec-229">Přidání položky místní úložiště, vyberte **přidat místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-229">To add a local storage entry, select **Add Local Storage**.</span></span>

    ![Přidejte místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. <span data-ttu-id="7d5ec-231">Jakmile nový záznam místní úložiště byl přidán do seznamu, aktualizujte řádek v seznamu nezbytné informace.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-231">Once the new local storage entry has been added to the list, update the row in the list with the necessary information.</span></span>

    ![Nová položka místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - <span data-ttu-id="7d5ec-233">**Název** – zadejte název, který chcete použít pro nové místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-233">**Name** - Enter the name that you want to use for the new local storage.</span></span>
    - <span data-ttu-id="7d5ec-234">**Velikost (MB)** – zadejte velikost v MB, kterou potřebujete pro nové místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-234">**Size (MB)** - Enter the size in MB that you need for the new local storage.</span></span>
    - <span data-ttu-id="7d5ec-235">**Čištění na role recyklaci** – vyberte tuto možnost, chcete-li odstranit data v nové místní úložiště po recyklaci virtuálního počítače pro roli.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-235">**Clean on role recycle** - Select this option to remove the data in the new local storage when the virtual machine for the role is recycled.</span></span>

1. <span data-ttu-id="7d5ec-236">Chcete-li odstranit položku místní úložiště, vyberte položku a pak vyberte **odebrat místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-236">To delete a local storage entry, select the entry, and then select **Remove Local Storage**.</span></span>

1. <span data-ttu-id="7d5ec-237">Ze sady Visual Studio panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-237">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-accessing-local-storage"></a><span data-ttu-id="7d5ec-238">Prostřednictvím kódu programu přístup k místnímu úložišti</span><span class="sxs-lookup"><span data-stu-id="7d5ec-238">Programmatically accessing local storage</span></span>

<span data-ttu-id="7d5ec-239">V této části ukazuje, jak se prostřednictvím kódu programu přístup k místnímu úložišti pomocí jazyka C# vytvořením textového souboru testovací `MyLocalStorageTest.txt`.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-239">This section illustrates how to programmatically access local storage using C# by writing a test text file `MyLocalStorageTest.txt`.</span></span>  

### <a name="write-a-text-file-to-local-storage"></a><span data-ttu-id="7d5ec-240">Zápis do místního úložiště do textového souboru</span><span class="sxs-lookup"><span data-stu-id="7d5ec-240">Write a text file to local storage</span></span>

<span data-ttu-id="7d5ec-241">Následující kód ukazuje příklad zápis do textového souboru do místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-241">The following code shows an example of how to write a text file to local storage.</span></span> <span data-ttu-id="7d5ec-242">Nahraďte &lt;LocalStorageName > zástupný symbol na odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-242">Replace the &lt;LocalStorageName> placeholder with the appropriate value.</span></span> 

    ```csharp
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-to-local-storage"></a><span data-ttu-id="7d5ec-243">Vyhledat soubor zapisovat do místního úložiště</span><span class="sxs-lookup"><span data-stu-id="7d5ec-243">Find a file written to local storage</span></span>

<span data-ttu-id="7d5ec-244">Chcete-li zobrazit soubor vytvořený pomocí kódu v předchozí části, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7d5ec-244">To view the file created by the code in the previous section, follow these steps:</span></span>
    
1.  <span data-ttu-id="7d5ec-245">V oznamovací oblasti systému Windows klikněte pravým tlačítkem na ikonu Azure a, v místní nabídce vyberte **zobrazit uživatelské prostředí emulátoru výpočtů**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-245">In the Windows notification area, right-click the Azure icon, and, from the context menu, select **Show Compute Emulator UI**.</span></span> 

    ![Zobrazit emulátoru služby výpočty Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. <span data-ttu-id="7d5ec-247">Vyberte webovou roli.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-247">Select the web role.</span></span>

    ![Emulátoru služby výpočty Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. <span data-ttu-id="7d5ec-249">Na **Microsoft Azure výpočetní emulátor** nabídce vyberte možnost **nástroje** > **otevřete místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-249">On the **Microsoft Azure Compute Emulator** menu, select **Tools** > **Open local store**.</span></span>

    ![Položky nabídky otevřete místního úložiště.](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. <span data-ttu-id="7d5ec-251">Když se otevře okno Průzkumníka Windows, zadejte "MyLocalStorageTest.txt'' do **vyhledávání** textového pole a vyberte **Enter** má začít prohledávání.</span><span class="sxs-lookup"><span data-stu-id="7d5ec-251">When the Windows Explorer window opens, enter `MyLocalStorageTest.txt`\` into the **Search** text box, and select **Enter** to start the search.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7d5ec-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d5ec-252">Next steps</span></span>
<span data-ttu-id="7d5ec-253">Další informace o Azure projekty v sadě Visual Studio načtením [konfigurace projektu Azure](vs-azure-tools-configuring-an-azure-project.md).</span><span class="sxs-lookup"><span data-stu-id="7d5ec-253">Learn more about Azure projects in Visual Studio by reading [Configuring an Azure Project](vs-azure-tools-configuring-an-azure-project.md).</span></span> <span data-ttu-id="7d5ec-254">Další informace o schématu cloudové služby načtením [– odkaz schématu](https://msdn.microsoft.com/library/azure/dd179398).</span><span class="sxs-lookup"><span data-stu-id="7d5ec-254">Learn more about the cloud service schema by reading [Schema Reference](https://msdn.microsoft.com/library/azure/dd179398).</span></span>

