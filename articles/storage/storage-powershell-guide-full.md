---
title: "Použití Azure PowerShell s Azure Storage | Microsoft Docs"
description: "Další informace o použití rutin prostředí Azure PowerShell pro Azure Storage k vytváření a správě účtů úložiště; Práce s objekty BLOB, tabulky, fronty a soubory; Nakonfigurujte a dotaz analytika úložiště a vytvořte sdílených přístupových podpisů."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 51e3e93ebedd31370857e61a00139294bcee9237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a><span data-ttu-id="75a6a-103">Použití Azure Powershell s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="75a6a-103">Using Azure PowerShell with Azure Storage</span></span>
## <a name="overview"></a><span data-ttu-id="75a6a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="75a6a-104">Overview</span></span>
<span data-ttu-id="75a6a-105">Prostředí Azure PowerShell je modul, který poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-105">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="75a6a-106">Je to prostředí příkazového řádku založené na úlohách a skriptovací jazyk určený speciálně pro správu systému.</span><span class="sxs-lookup"><span data-stu-id="75a6a-106">It is a task-based command-line shell and scripting language designed especially for system administration.</span></span> <span data-ttu-id="75a6a-107">Pomocí prostředí PowerShell můžete snadno řídit a automatizovat správu systému Azure služby a aplikace.</span><span class="sxs-lookup"><span data-stu-id="75a6a-107">With PowerShell, you can easily control and automate the administration of your Azure services and applications.</span></span> <span data-ttu-id="75a6a-108">Například můžete použít rutiny k provádění stejných úloh, které můžete provádět prostřednictvím [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="75a6a-108">For example, you can use the cmdlets to perform the same tasks that you can perform through the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="75a6a-109">V této příručce, jsme budete prozkoumat postup použití [rutiny úložiště Azure](/powershell/module/azurerm.storage/#storage) k provádění různých úloh vývoj a správu s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-109">In this guide, we'll explore how to use the [Azure Storage Cmdlets](/powershell/module/azurerm.storage/#storage) to perform a variety of development and administration tasks with Azure Storage.</span></span>

<span data-ttu-id="75a6a-110">Tato příručka předpokládá, že máte zkušenosti s použitím [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) a [prostředí Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-110">This guide assumes that you have prior experience using [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) and [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span></span> <span data-ttu-id="75a6a-111">Průvodce poskytuje řadu skripty, které ukazují použití prostředí PowerShell s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-111">The guide provides a number of scripts to demonstrate the usage of PowerShell with Azure Storage.</span></span> <span data-ttu-id="75a6a-112">Skript proměnné na základě vaší konfigurace před spuštěním každý skript by měl aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="75a6a-112">You should update the script variables based on your configuration before running each script.</span></span>

<span data-ttu-id="75a6a-113">V první části v tomto průvodci poskytuje stručný přehled Azure Storage a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-113">The first section in this guide provides a quick glance at Azure Storage and PowerShell.</span></span> <span data-ttu-id="75a6a-114">Podrobné informace a pokyny, spusťte z [požadavky pro použití prostředí Azure PowerShell s Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="75a6a-114">For detailed information and instructions, start from the [Prerequisites for using Azure PowerShell with Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span></span>

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a><span data-ttu-id="75a6a-115">Začínáme s Azure Storage a prostředí PowerShell během 5 minut</span><span class="sxs-lookup"><span data-stu-id="75a6a-115">Getting started with Azure Storage and PowerShell in 5 minutes</span></span>
<span data-ttu-id="75a6a-116">V této části se dozvíte, jak pro přístup k úložišti Azure pomocí prostředí PowerShell během 5 minut.</span><span class="sxs-lookup"><span data-stu-id="75a6a-116">This section shows you how to access Azure Storage via PowerShell in 5 minutes.</span></span>

<span data-ttu-id="75a6a-117">**Nové do Azure:** získat předplatné Microsoft Azure a účet Microsoft přidružené k tomuto předplatnému.</span><span class="sxs-lookup"><span data-stu-id="75a6a-117">**New to Azure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="75a6a-118">Informace o možnostech nákupu Azure najdete v tématu [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [zakoupit možnosti](https://azure.microsoft.com/pricing/purchase-options/), a [člen nabízí](https://azure.microsoft.com/pricing/member-offers/) (pro členy MSDN, BizSpark, Microsoft Partner Network, a další programy společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="75a6a-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="75a6a-119">V tématu [přiřazení rolí správce v Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Další informace o předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="75a6a-120">**Po vytvoření odběru služby Microsoft Azure a účet:**</span><span class="sxs-lookup"><span data-stu-id="75a6a-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="75a6a-121">Stáhněte a nainstalujte nejnovější [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span><span class="sxs-lookup"><span data-stu-id="75a6a-121">Download and install the latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span></span>
2. <span data-ttu-id="75a6a-122">Spuštění Windows PowerShell integrované skriptování prostředí (ISE): V místním počítači, přejděte do **spustit** nabídky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-122">Start Windows PowerShell Integrated Scripting Environment (ISE): In your local computer, go to the **Start** menu.</span></span> <span data-ttu-id="75a6a-123">Typ **nástroje pro správu** a klikněte na tlačítko ji spustit.</span><span class="sxs-lookup"><span data-stu-id="75a6a-123">Type **Administrative Tools** and click to run it.</span></span> <span data-ttu-id="75a6a-124">V **nástroje pro správu** okna, klikněte pravým tlačítkem na **Windows PowerShell ISE**, klikněte na tlačítko **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-124">In the **Administrative Tools** window, right-click **Windows PowerShell ISE**, click **Run as Administrator**.</span></span>
3. <span data-ttu-id="75a6a-125">V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** k vytvoření nového souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-125">In **Windows PowerShell ISE**, click **File** > **New** to create a new script file.</span></span>
4. <span data-ttu-id="75a6a-126">Nyní sdělíme vám jednoduchý skript, který zobrazuje základní příkazy prostředí PowerShell pro přístup k úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-126">Now, we'll give you a simple script that shows basic PowerShell commands to access Azure Storage.</span></span> <span data-ttu-id="75a6a-127">Skript se nejdřív požádat vašeho Azure přihlašovací údaje účtu pro vaši Azure přidat účet místní prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-127">The script will first ask your Azure account credentials to add your Azure account to the local PowerShell environment.</span></span> <span data-ttu-id="75a6a-128">Skript bude poté nastavit výchozí předplatného Azure a vytvořte nový účet úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-128">Then, the script will set the default Azure subscription and create a new storage account in Azure.</span></span> <span data-ttu-id="75a6a-129">V dalším kroku skript vytvoříte nový kontejner v rámci tohoto nového účtu úložiště a nahrajte existující soubor bitové kopie (binární rozsáhlý objekt) do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="75a6a-129">Next, the script will create a new container in this new storage account and upload an existing image file (blob) to that container.</span></span> <span data-ttu-id="75a6a-130">Po skript obsahuje seznam všech objektů BLOB v kontejneru, vytvoří nový cílový adresář v místním počítači a stáhnout soubor bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="75a6a-130">After the script lists all blobs in that container, it will create a new destination directory in your local computer and download the image file.</span></span>
5. <span data-ttu-id="75a6a-131">V následující části kódu, vyberte skript mezi poznámky **#begin** a **#end**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-131">In the following code section, select the script between the remarks **#begin** and **#end**.</span></span> <span data-ttu-id="75a6a-132">Stisknutím kláves CTRL + C zkopírujte jej do schránky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-132">Press CTRL+C to copy it to the clipboard.</span></span>

    ```powershell
    # begin
    # Update with the name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name to your new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name to your new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account to the local PowerShell environment.
    Add-AzureAccount
       
    # Set a default Azure subscription.
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
       
    # Create a new storage account.
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location
       
    # Set a default storage account.
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
       
    # Create a new container.
    New-AzureStorageContainer -Name $ContainerName -Permission Off
       
    # Upload a blob into a container.
    Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload
       
    # List all blobs in a container.
    Get-AzureStorageBlob -Container $ContainerName
       
    # Download blobs from the container:
    # Get a reference to a list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create the destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into the local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. <span data-ttu-id="75a6a-133">V **Windows PowerShell ISE**, stiskněte klávesy CTRL + V zkopírujte skript.</span><span class="sxs-lookup"><span data-stu-id="75a6a-133">In **Windows PowerShell ISE**, press CTRL+V to copy the script.</span></span> <span data-ttu-id="75a6a-134">Klikněte na tlačítko **soubor** > **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-134">Click **File** > **Save**.</span></span> <span data-ttu-id="75a6a-135">V **uložit jako** dialogového okna zadejte název souboru skriptu, jako je například "mystoragescript."</span><span class="sxs-lookup"><span data-stu-id="75a6a-135">In the **Save As** dialog window, type the name of the script file, such as "mystoragescript."</span></span> <span data-ttu-id="75a6a-136">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-136">Click **Save**.</span></span>
7. <span data-ttu-id="75a6a-137">Teď je potřeba aktualizovat proměnné skriptu na základě svého nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="75a6a-137">Now, you need to update the script variables based on your configuration settings.</span></span> <span data-ttu-id="75a6a-138">Je nutné aktualizovat **$SubscriptionName** proměnné pomocí svého vlastního předplatného.</span><span class="sxs-lookup"><span data-stu-id="75a6a-138">You must update the **$SubscriptionName** variable with your own subscription.</span></span> <span data-ttu-id="75a6a-139">Můžete ponechat jiné proměnné uvedeného ve skriptu nebo je aktualizovat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="75a6a-139">You can keep the other variables as specified in the script or update them as you wish.</span></span>
   
   * <span data-ttu-id="75a6a-140">**$SubscriptionName:** musíte aktualizovat tuto proměnnou s vlastní název odběru.</span><span class="sxs-lookup"><span data-stu-id="75a6a-140">**$SubscriptionName:** You must update this variable with your own subscription name.</span></span> <span data-ttu-id="75a6a-141">Proveďte jeden z následujících tří způsobů vyhledejte název předplatného:</span><span class="sxs-lookup"><span data-stu-id="75a6a-141">Follow one of the following three ways to locate the name of your subscription:</span></span>
     
    <span data-ttu-id="75a6a-142">a.</span><span class="sxs-lookup"><span data-stu-id="75a6a-142">a.</span></span> <span data-ttu-id="75a6a-143">V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** k vytvoření nového souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-143">In **Windows PowerShell ISE**, click **File** > **New** to create a new script file.</span></span> <span data-ttu-id="75a6a-144">Zkopírujte následující skript na nový soubor skriptu a klikněte na tlačítko **ladění** > **spustit**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-144">Copy the following script to the new script file and click **Debug** > **Run**.</span></span> <span data-ttu-id="75a6a-145">Následující skript se nejdřív požádat vašeho Azure přihlašovací údaje účtu pro vaši Azure přidat účet do místního prostředí PowerShell a pak zobrazíte všechny odběry, které jsou připojené k místní relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-145">The following script will first ask your Azure account credentials to add your Azure account to the local PowerShell environment and then show all the subscriptions that are connected to the local PowerShell session.</span></span> <span data-ttu-id="75a6a-146">Poznamenejte si název odběru, který chcete použít během provádění v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="75a6a-146">Take a note of the name of the subscription that you want to use while following this tutorial:</span></span>
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    <span data-ttu-id="75a6a-147">b.</span><span class="sxs-lookup"><span data-stu-id="75a6a-147">b.</span></span> <span data-ttu-id="75a6a-148">Vyhledejte a zkopírujte název odběru v [portál Azure](https://portal.azure.com), v nabídce centra na levé straně klikněte na **odběry**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-148">To locate and copy your subscription name in the [Azure portal](https://portal.azure.com), in the Hub menu on the left, click **Subscriptions**.</span></span> <span data-ttu-id="75a6a-149">Zkopírujte název odběru, který chcete použít při spouštění skriptů v této příručce.</span><span class="sxs-lookup"><span data-stu-id="75a6a-149">Copy the name of subscription that you want to use while running the scripts in this guide.</span></span>
     
     ![portál Azure](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    <span data-ttu-id="75a6a-151">c.</span><span class="sxs-lookup"><span data-stu-id="75a6a-151">c.</span></span> <span data-ttu-id="75a6a-152">Vyhledejte a zkopírujte název odběru v [portálu Azure Classic](https://manage.windowsazure.com/), posuňte se dolů a klikněte na **nastavení** na levé straně na portálu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-152">To locate and copy your subscription name in the [Azure Classic Portal](https://manage.windowsazure.com/), scroll down and click **Settings** on the left side of the portal.</span></span> <span data-ttu-id="75a6a-153">Klikněte na tlačítko **odběry** zobrazíte seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="75a6a-153">Click **Subscriptions** to see a list of your subscriptions.</span></span> <span data-ttu-id="75a6a-154">Zkopírujte název odběru, který chcete použít při spouštění skriptů zadané v této příručce.</span><span class="sxs-lookup"><span data-stu-id="75a6a-154">Copy the name of subscription that you want to use while running the scripts given in this guide.</span></span>
     
     ![portál Azure Classic](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * <span data-ttu-id="75a6a-156">**$StorageAccountName:** použít daným názvem ve skriptu nebo zadejte nový název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-156">**$StorageAccountName:** Use the given name in the script or enter a new name for your storage account.</span></span> <span data-ttu-id="75a6a-157">**Důležité:** název účtu úložiště musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-157">**Important:** The name of the storage account must be unique in Azure.</span></span> <span data-ttu-id="75a6a-158">Musí být malými písmeny, příliš!</span><span class="sxs-lookup"><span data-stu-id="75a6a-158">It must be lowercase, too!</span></span>
   * <span data-ttu-id="75a6a-159">**$Location:** použít daný "západní USA" ve skriptu nebo zvolte jiné umístění Azure, jako například Východ USA, Severní Evropa a tak dále.</span><span class="sxs-lookup"><span data-stu-id="75a6a-159">**$Location:** Use the given "West US" in the script or choose other Azure locations, such as East US, North Europe, and so on.</span></span>
   * <span data-ttu-id="75a6a-160">**$ContainerName:** použít daným názvem ve skriptu nebo zadejte nový název pro váš kontejner.</span><span class="sxs-lookup"><span data-stu-id="75a6a-160">**$ContainerName:** Use the given name in the script or enter a new name for your container.</span></span>
   * <span data-ttu-id="75a6a-161">**$ImageToUpload:** zadejte cestu k obrázku v místním počítači, jako například: "C:\Images\HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="75a6a-161">**$ImageToUpload:** Enter a path to a picture on your local computer, such as: "C:\Images\HelloWorld.png".</span></span>
   * <span data-ttu-id="75a6a-162">**$DestinationFolder:** zadejte cestu k místnímu adresáři ukládat soubory stahované z Azure Storage, například: "C:\DownloadImages".</span><span class="sxs-lookup"><span data-stu-id="75a6a-162">**$DestinationFolder:** Enter a path to a local directory to store files downloaded from Azure Storage, such as: "C:\DownloadImages".</span></span>
8. <span data-ttu-id="75a6a-163">Po aktualizaci proměnné skript v souboru "mystoragescript.ps1", klikněte na tlačítko **soubor** > **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-163">After updating the script variables in the "mystoragescript.ps1" file, click **File** > **Save**.</span></span> <span data-ttu-id="75a6a-164">Potom klikněte na **ladění** > **spustit** nebo stiskněte klávesu **F5** pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-164">Then, click **Debug** > **Run** or press **F5** to run the script.</span></span>  

<span data-ttu-id="75a6a-165">Po spuštění skriptu, měli byste mít místní cílovou složku, která obsahuje soubor stažený bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="75a6a-165">After the script runs, you should have a local destination folder that includes the downloaded image file.</span></span> <span data-ttu-id="75a6a-166">Následující snímek obrazovky ukazuje příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="75a6a-166">The following screenshot shows an example output:</span></span>

![Stáhnout objekty BLOB](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> <span data-ttu-id="75a6a-168">V části "Začínáme s Azure Storage a prostředí PowerShell během 5 minut" poskytuje rychlý úvod o tom, jak pomocí prostředí Azure PowerShell s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-168">The "Getting started with Azure Storage and PowerShell in 5 minutes" section provided a quick introduction on how to use Azure PowerShell with Azure Storage.</span></span> <span data-ttu-id="75a6a-169">Podrobné informace a pokyny doporučujeme vám přečíst v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="75a6a-169">For detailed information and instructions, we encourage you to read the following sections.</span></span>
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a><span data-ttu-id="75a6a-170">Požadavky pro použití prostředí Azure PowerShell s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="75a6a-170">Prerequisites for using Azure PowerShell with Azure Storage</span></span>
<span data-ttu-id="75a6a-171">Potřebujete předplatné Azure a účet, který chcete spustit rutiny prostředí PowerShell uvedených v této příručce, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="75a6a-171">You need an Azure subscription and account to run the PowerShell cmdlets given in this guide, as described above.</span></span>

<span data-ttu-id="75a6a-172">Prostředí Azure PowerShell je modul, který poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-172">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="75a6a-173">Informace o instalaci a nastavení prostředí Azure PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="75a6a-173">For information on installing and setting up Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="75a6a-174">Doporučujeme stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure PowerShell před použitím tohoto průvodce.</span><span class="sxs-lookup"><span data-stu-id="75a6a-174">We recommend that you download and install or upgrade to the latest Azure PowerShell module before using this guide.</span></span>

<span data-ttu-id="75a6a-175">Spuštěním rutiny v konzole pro standardní prostředí Windows PowerShell nebo Windows PowerShell Integrované skriptovací prostředí (ISE).</span><span class="sxs-lookup"><span data-stu-id="75a6a-175">You can run the cmdlets in the standard Windows PowerShell console or the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="75a6a-176">Chcete-li například otevřete **Windows PowerShell ISE**, přejděte do nabídky Start, typ nástroje pro správu a klikněte na tlačítko ji spustit.</span><span class="sxs-lookup"><span data-stu-id="75a6a-176">For example, to open **Windows PowerShell ISE**, go to the Start menu, type Administrative Tools and click to run it.</span></span> <span data-ttu-id="75a6a-177">V okně nástroje pro správu klikněte pravým tlačítkem na Windows PowerShell ISE, klikněte na tlačítko Spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="75a6a-177">In the Administrative Tools window, right-click Windows PowerShell ISE, click Run as Administrator.</span></span>

## <a name="how-to-manage-storage-accounts-in-azure"></a><span data-ttu-id="75a6a-178">Jak spravovat účty úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-178">How to manage storage accounts in Azure</span></span>

<span data-ttu-id="75a6a-179">Podívejme se na správu účty úložiště v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="75a6a-179">Let's take a look at managing storage accounts in Azure with PowerShell</span></span>

### <a name="how-to-set-a-default-azure-subscription"></a><span data-ttu-id="75a6a-180">Jak nastavit výchozí předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-180">How to set a default Azure subscription</span></span>
<span data-ttu-id="75a6a-181">Ke správě úložiště Azure pomocí Azure PowerShell, budete muset ověřit prostředí klienta s nástrojem Azure přes Azure Active Directory ověřování nebo ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="75a6a-181">To manage Azure Storage using Azure PowerShell, you need to authenticate your client environment with Azure via Azure Active Directory authentication or certificate-based authentication.</span></span> <span data-ttu-id="75a6a-182">Podrobné informace najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) kurzu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-182">For detailed information, see [How to install and configure Azure PowerShell](/powershell/azure/overview) tutorial.</span></span> <span data-ttu-id="75a6a-183">Tento průvodce pomocí ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="75a6a-183">This guide uses the Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="75a6a-184">V systému Windows PowerShell ISE, zadejte následující příkaz pro přidání vaší Azure účet místní prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="75a6a-184">In Windows PowerShell ISE, type the following command to add your Azure account to the local PowerShell environment:</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="75a6a-185">V okně "Přihlášení v k Microsoft Azure" Zadejte e-mailovou adresu a heslo spojené s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="75a6a-185">In the "Sign in to Microsoft Azure" window, type the email address and password associated with your account.</span></span> <span data-ttu-id="75a6a-186">Azure přihlašovací údaje ověří, uloží je a pak zavře okno.</span><span class="sxs-lookup"><span data-stu-id="75a6a-186">Azure authenticates and saves the credential information, and then closes the window.</span></span>

3. <span data-ttu-id="75a6a-187">Potom spusťte následující příkaz k zobrazení účtů Azure ve vašem místním prostředí PowerShell a zkontrolujte, zda váš účet:</span><span class="sxs-lookup"><span data-stu-id="75a6a-187">Next, run the following command to view the Azure accounts in your local PowerShell environment, and verify that your account is listed:</span></span>
   
    ```powershell
    Get-AzureAccount
    ```
4. <span data-ttu-id="75a6a-188">Potom spusťte následující rutinu zobrazíte všechny odběry, které jsou připojené k místní relaci prostředí PowerShell a zkontrolujte, zda vaše předplatné:</span><span class="sxs-lookup"><span data-stu-id="75a6a-188">Then, run the following cmdlet to view all the subscriptions that are connected to the local PowerShell session, and verify that your subscription is listed:</span></span>

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. <span data-ttu-id="75a6a-189">Pokud chcete nastavit výchozí předplatné Azure, spusťte rutinu Select-AzureSubscription:</span><span class="sxs-lookup"><span data-stu-id="75a6a-189">To set a default Azure subscription, run the Select-AzureSubscription cmdlet:</span></span>

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. <span data-ttu-id="75a6a-190">Ověřte název výchozí předplatné spuštěním rutiny Get-AzureSubscription:</span><span class="sxs-lookup"><span data-stu-id="75a6a-190">Verify the name of the default subscription by running the Get-AzureSubscription cmdlet:</span></span>

    ```powershell
    Get-AzureSubscription -Default
    ```

7. <span data-ttu-id="75a6a-191">Chcete-li zobrazit všechny dostupné rutiny prostředí PowerShell pro Azure Storage, spusťte:</span><span class="sxs-lookup"><span data-stu-id="75a6a-191">To see all the available PowerShell cmdlets for Azure Storage, run:</span></span>
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-to-create-a-new-azure-storage-account"></a><span data-ttu-id="75a6a-192">Jak vytvořit nový účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-192">How to create a new Azure storage account</span></span>
<span data-ttu-id="75a6a-193">Pokud chcete používat úložiště Azure, potřebujete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-193">To use Azure storage, you will need a storage account.</span></span> <span data-ttu-id="75a6a-194">Po nakonfigurování počítače pro připojení k vašemu předplatnému, můžete vytvořit nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-194">You can create a new Azure storage account after you have configured your computer to connect to your subscription.</span></span>

1. <span data-ttu-id="75a6a-195">Spusťte rutinu Get-AzureLocation pro vyhledání všech dostupných datacenter umístění:</span><span class="sxs-lookup"><span data-stu-id="75a6a-195">Run the Get-AzureLocation cmdlet to find all the available datacenter locations:</span></span>

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. <span data-ttu-id="75a6a-196">Potom spusťte rutinu New-AzureStorageAccount k vytvoření nového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-196">Next, run the New-AzureStorageAccount cmdlet to create a new storage account.</span></span> <span data-ttu-id="75a6a-197">Následující příklad vytvoří nový účet úložiště v datovém centru "Západní USA".</span><span class="sxs-lookup"><span data-stu-id="75a6a-197">The following example creates a new storage account in the "West US" datacenter.</span></span>
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> <span data-ttu-id="75a6a-198">Název účtu úložiště musí být jedinečný v rámci Azure a musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-198">The name of your storage account must be unique within Azure and must be lowercase.</span></span> <span data-ttu-id="75a6a-199">Zásady vytváření názvů a omezení najdete v tématu [o účtech úložiště Azure](storage-create-storage-account.md) a [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-199">For naming conventions and restrictions, see [About Azure Storage Accounts](storage-create-storage-account.md) and [Naming and Referencing Containers, Blobs, and Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span></span>
> 
> 

### <a name="how-to-set-a-default-azure-storage-account"></a><span data-ttu-id="75a6a-200">Jak nastavit výchozí účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-200">How to set a default Azure storage account</span></span>
<span data-ttu-id="75a6a-201">Můžete mít více účtů úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="75a6a-201">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="75a6a-202">Můžete zvolit jeden z nich a nastavte jej jako výchozí účet úložiště pro všechny příkazy úložiště ve stejné relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-202">You can choose one of them and set it as the default storage account for all the storage commands in the same PowerShell session.</span></span> <span data-ttu-id="75a6a-203">Díky tomu můžete ke spuštění příkazů prostředí Azure PowerShell úložiště bez zadání explicitně kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-203">This enables you to run the Azure PowerShell storage commands without specifying the storage context explicitly.</span></span>

1. <span data-ttu-id="75a6a-204">Pokud chcete nastavit výchozí účet úložiště pro vaše předplatné, můžete spustit rutinu Set-AzureSubscription.</span><span class="sxs-lookup"><span data-stu-id="75a6a-204">To set a default storage account for your subscription, you can run the Set-AzureSubscription cmdlet.</span></span>

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. <span data-ttu-id="75a6a-205">Potom spusťte rutinu Get-AzureSubscription k ověřte, zda je přiřazená k vašemu účtu předplatného výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-205">Next, run the Get-AzureSubscription cmdlet to verify that the storage account is associated with your default subscription account.</span></span> <span data-ttu-id="75a6a-206">Tento příkaz vrátí vlastnosti odběru v aktuálním předplatném, včetně jeho aktuální účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-206">This command returns the subscription properties on the current subscription including its current storage account.</span></span>

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a><span data-ttu-id="75a6a-207">Postup seznamu všech účtů úložiště Azure v předplatném.</span><span class="sxs-lookup"><span data-stu-id="75a6a-207">How to list all Azure storage accounts in a subscription</span></span>
<span data-ttu-id="75a6a-208">Každé předplatné Azure může mít až 100 účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-208">Each Azure subscription can have up to 100 storage accounts.</span></span> <span data-ttu-id="75a6a-209">Nejaktuálnější informace o limitech najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-209">For the most up-to-date information on limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="75a6a-210">Spusťte následující rutiny můžete zjistit název a stav účtů úložiště v aktuálním předplatném:</span><span class="sxs-lookup"><span data-stu-id="75a6a-210">Run the following cmdlet to find out the name and status of the storage accounts in the current subscription:</span></span>

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-to-create-an-azure-storage-context"></a><span data-ttu-id="75a6a-211">Postup vytvoření kontextu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-211">How to create an Azure storage context</span></span>
<span data-ttu-id="75a6a-212">Kontext úložiště Azure je objekt v prostředí PowerShell pro zapouzdření přihlašovacích údajů úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-212">Azure storage context is an object in PowerShell to encapsulate the storage credentials.</span></span> <span data-ttu-id="75a6a-213">Použitím odlišného kontextu, úložiště, při spuštění libovolné další rutiny umožňuje vaši žádost o ověření bez zadání účtu úložiště a jeho přístupový klíč explicitně.</span><span class="sxs-lookup"><span data-stu-id="75a6a-213">Using a storage context while running any subsequent cmdlet enables you to authenticate your request without specifying the storage account and its access key explicitly.</span></span> <span data-ttu-id="75a6a-214">Kontext úložiště můžete vytvořit v mnoha způsoby, například pomocí názvu a přístupový klíč účtu úložiště, sdílený přístupový podpis (SAS) token, připojovací řetězec, nebo anonymní.</span><span class="sxs-lookup"><span data-stu-id="75a6a-214">You can create a storage context in many ways, such as using storage account name and access key, shared access signature (SAS) token, connection string, or anonymous.</span></span> <span data-ttu-id="75a6a-215">Další informace najdete v tématu [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span><span class="sxs-lookup"><span data-stu-id="75a6a-215">For more information, see [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span></span>  

<span data-ttu-id="75a6a-216">Použijte jednu z následujících tří způsobů vytvořit kontext úložiště:</span><span class="sxs-lookup"><span data-stu-id="75a6a-216">Use one of the following three ways to create a storage context:</span></span>

* <span data-ttu-id="75a6a-217">Spustit [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) rutiny získat přístupový klíč primárního úložiště pro váš účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-217">Run the [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet to find out the primary storage access key for your Azure storage account.</span></span> <span data-ttu-id="75a6a-218">Pak zavolejte [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) vytvořte kontext úložiště:</span><span class="sxs-lookup"><span data-stu-id="75a6a-218">Next, call the [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet to create a storage context:</span></span>

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* <span data-ttu-id="75a6a-219">Vygenerování tokenu sdíleného přístupového podpisu pro kontejner úložiště Azure a použít ho k vytvoření kontextu úložiště:</span><span class="sxs-lookup"><span data-stu-id="75a6a-219">Generate a shared access signature token for an Azure storage container and use it to create a storage context:</span></span>

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    <span data-ttu-id="75a6a-220">Další informace najdete v tématu [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) a [pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-220">For more information, see [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) and [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span></span>

* <span data-ttu-id="75a6a-221">V některých případech můžete určovat koncové body služby, když vytvoříte nový kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-221">In some cases, you may want to specify the service endpoints when you create a new storage context.</span></span> <span data-ttu-id="75a6a-222">To může být nezbytné, pokud je zaregistrovaný vlastního názvu domény pro váš účet úložiště se službou objektů Blob nebo chcete použít sdílený přístupový podpis pro přístup k prostředkům úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-222">This might be necessary when you have registered a custom domain name for your storage account with the Blob service or you want to use a shared access signature for accessing storage resources.</span></span> <span data-ttu-id="75a6a-223">Nastavte koncové body služby v připojovacím řetězci a použít k vytvoření nový kontext úložiště, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="75a6a-223">Set the service endpoints in a connection string and use it to create a new storage context as shown below:</span></span>

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

<span data-ttu-id="75a6a-224">Další informace o tom, jak nakonfigurovat připojovací řetězec úložiště najdete v tématu [konfiguraci připojovacích řetězců](storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-224">For more information on how to configure a storage connection string, see [Configuring Connection Strings](storage-configure-connection-string.md).</span></span>

<span data-ttu-id="75a6a-225">Teď, když máte v počítači a zjistili, jak Správa předplatných a účtů úložiště pomocí prostředí Azure PowerShell, přejděte k další části se dozvíte, jak spravovat Azure objekty BLOB a objektů blob snímky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-225">Now that you have set up your computer and learned how to manage subscriptions and storage accounts using Azure PowerShell, go to the next section to learn how to manage Azure blobs and blob snapshots.</span></span>

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a><span data-ttu-id="75a6a-226">Jak načíst a znovu vygenerovat klíče úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-226">How to retrieve and regenerate Azure storage keys</span></span>
<span data-ttu-id="75a6a-227">Účet úložiště Azure obsahuje dva klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-227">An Azure Storage account comes with two account keys.</span></span> <span data-ttu-id="75a6a-228">Následující rutiny můžete načíst vaše klíče.</span><span class="sxs-lookup"><span data-stu-id="75a6a-228">You can use the following cmdlet to retrieve your keys.</span></span>

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

<span data-ttu-id="75a6a-229">Použijte následující rutinu k načtení konkrétního klíče.</span><span class="sxs-lookup"><span data-stu-id="75a6a-229">Use the following cmdlet to retrieve a specific key.</span></span> <span data-ttu-id="75a6a-230">Platné hodnoty jsou primární i sekundární.</span><span class="sxs-lookup"><span data-stu-id="75a6a-230">Valid values are Primary and Secondary.</span></span>  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

<span data-ttu-id="75a6a-231">Pokud jste chtěli znovu vygenerovat klíče, použijte následující rutinu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-231">If you would like to regenerate your keys, use the following cmdlet.</span></span> <span data-ttu-id="75a6a-232">Typ_klíče - platné hodnoty jsou "Primární" a "Sekundární"</span><span class="sxs-lookup"><span data-stu-id="75a6a-232">Valid values for -KeyType are "Primary" and "Secondary"</span></span>

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-to-manage-azure-blobs"></a><span data-ttu-id="75a6a-233">Správa objektů Azure BLOB</span><span class="sxs-lookup"><span data-stu-id="75a6a-233">How to manage Azure blobs</span></span>
<span data-ttu-id="75a6a-234">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="75a6a-234">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="75a6a-235">V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-235">This section assumes that you are already familiar with the Azure Blob Storage Service concepts.</span></span> <span data-ttu-id="75a6a-236">Podrobné informace najdete v tématu [Začínáme s úložištěm Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-236">For detailed information, see [Get started with Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="how-to-create-a-container"></a><span data-ttu-id="75a6a-237">Postup vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="75a6a-237">How to create a container</span></span>
<span data-ttu-id="75a6a-238">Každý objekt blob v úložišti Azure musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="75a6a-238">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="75a6a-239">Můžete vytvořit kontejner privátní, pomocí rutiny New-AzureStorageContainer:</span><span class="sxs-lookup"><span data-stu-id="75a6a-239">You can create a private container using the New-AzureStorageContainer cmdlet:</span></span>

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> <span data-ttu-id="75a6a-240">Existují tři úrovně anonymní přístup pro čtení: **vypnout**, **Blob**, a **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-240">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="75a6a-241">Chcete-li zabránit anonymní přístup k objektům BLOB, nastavte parametr oprávnění na **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-241">To prevent anonymous access to blobs, set the Permission parameter to **Off**.</span></span> <span data-ttu-id="75a6a-242">Ve výchozím nastavení je nový kontejner je privátní a můžete přistupovat pouze pomocí vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-242">By default, the new container is private and can be accessed only by the account owner.</span></span> <span data-ttu-id="75a6a-243">Chcete-li povolit anonymní veřejný přístup pro čtení k prostředkům blob, ale ne pro metadata kontejneru nebo do seznamu objektů BLOB v kontejneru, nastavte parametr oprávnění na **Blob**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-243">To allow anonymous public read access to blob resources, but not to container metadata or to the list of blobs in the container, set the Permission parameter to **Blob**.</span></span> <span data-ttu-id="75a6a-244">Povolit úplné veřejný přístup pro čtení do objektu blob prostředků, metadata kontejneru a seznamu objektů BLOB v kontejneru, nastavte parametr oprávnění na **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-244">To allow full public read access to blob resources, container metadata, and the list of blobs in the container, set the Permission parameter to **Container**.</span></span> <span data-ttu-id="75a6a-245">Další informace najdete v tématu [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-245">For more information, see [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>
> 
> 

### <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="75a6a-246">Jak nahrát objekt blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="75a6a-246">How to upload a blob into a container</span></span>
<span data-ttu-id="75a6a-247">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-247">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="75a6a-248">Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-248">For more information, see [Understanding Block Blobs, Append BLobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="75a6a-249">Pokud chcete nahrát do kontejneru objektů BLOB v, můžete použít [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-249">To upload blobs in to a container, you can use the [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span></span> <span data-ttu-id="75a6a-250">Ve výchozím nastavení odešle tento příkaz místních souborů do objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="75a6a-250">By default, this command uploads the local files to a block blob.</span></span> <span data-ttu-id="75a6a-251">Chcete-li určit typ pro tento objekt blob, můžete použít parametr - BlobType.</span><span class="sxs-lookup"><span data-stu-id="75a6a-251">To specify the type for the blob, you can use the -BlobType parameter.</span></span>

<span data-ttu-id="75a6a-252">Následující příklad spustí [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) rutiny všechny soubory v zadané složce a předává je do další rutiny pomocí operátor kanálu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-252">The following example runs the [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet to get all the files in the specified folder, and then passes them to the next cmdlet by using the pipeline operator.</span></span> <span data-ttu-id="75a6a-253">[Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny odešle místních souborů do vašeho kontejneru:</span><span class="sxs-lookup"><span data-stu-id="75a6a-253">The [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet uploads the local files to your container:</span></span>

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-to-download-blobs-from-a-container"></a><span data-ttu-id="75a6a-254">Tom, jak stáhnout objekty BLOB z kontejneru</span><span class="sxs-lookup"><span data-stu-id="75a6a-254">How to download blobs from a container</span></span>
<span data-ttu-id="75a6a-255">Následující příklad ukazuje, jak stáhnout objekty BLOB kontejneru.</span><span class="sxs-lookup"><span data-stu-id="75a6a-255">The following example demonstrates how to download blobs from a container.</span></span> <span data-ttu-id="75a6a-256">V příkladu nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho primární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="75a6a-256">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its primary access key.</span></span> <span data-ttu-id="75a6a-257">Potom příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-257">Then, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span></span> <span data-ttu-id="75a6a-258">V dalším kroku v příkladu se používá [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) rutiny stáhnout objekty BLOB do místní cílovou složku.</span><span class="sxs-lookup"><span data-stu-id="75a6a-258">Next, the example uses the [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet to download blobs into the local destination folder.</span></span>

```powershell
#Define the variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a><span data-ttu-id="75a6a-259">Jak kopírovat z jednoho kontejneru úložiště objektů BLOB do jiného</span><span class="sxs-lookup"><span data-stu-id="75a6a-259">How to copy blobs from one storage container to another</span></span>
<span data-ttu-id="75a6a-260">Mezi účty úložiště a oblastí, můžete zkopírovat objekty BLOB asynchronně.</span><span class="sxs-lookup"><span data-stu-id="75a6a-260">You can copy blobs across storage accounts and regions asynchronously.</span></span> <span data-ttu-id="75a6a-261">Následující příklad ukazuje, jak zkopírovat objekty BLOB z jednoho kontejneru úložiště do druhého v dva účty jiného úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-261">The following example demonstrates how to copy blobs from one storage container to another in two different storage accounts.</span></span> <span data-ttu-id="75a6a-262">V příkladu nejprve nastaví proměnné pro zdrojové a cílové účty úložiště a poté vytvoří kontext úložiště pro každý účet.</span><span class="sxs-lookup"><span data-stu-id="75a6a-262">The example first sets variables for source and destination storage accounts, and then creates a storage context for each account.</span></span> <span data-ttu-id="75a6a-263">V dalším kroku v příkladu kopíruje objekty BLOB z kontejneru zdrojového na cílový kontejner pomocí [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-263">Next, the example copies blobs from the source container to the destination container using the [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span></span> <span data-ttu-id="75a6a-264">Příklad předpokládá, že zdrojový a cílový účty úložiště a kontejnery již existuje.</span><span class="sxs-lookup"><span data-stu-id="75a6a-264">The example assumes that the source and destination storage accounts and containers already exist.</span></span>

```powershell
#Define the source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define the destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference to blobs in the source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container to another.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

<span data-ttu-id="75a6a-265">Pamatujte, že v tomto příkladu asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="75a6a-265">Note that this example performs an asynchronous copy.</span></span> <span data-ttu-id="75a6a-266">Můžete monitorovat stav každé kopie spuštěním [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-266">You can monitor the status of each copy by running the [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span></span>

### <a name="how-to-copy-blobs-from-a-secondary-location"></a><span data-ttu-id="75a6a-267">Postup kopírování objekty BLOB ze sekundárního umístění</span><span class="sxs-lookup"><span data-stu-id="75a6a-267">How to copy blobs from a secondary location</span></span>
<span data-ttu-id="75a6a-268">Objekty BLOB můžete zkopírovat ze sekundárního umístění účtu povoleno RA-GRS.</span><span class="sxs-lookup"><span data-stu-id="75a6a-268">You can copy blobs from the secondary location of a RA-GRS-enabled account.</span></span>

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-to-delete-a-blob"></a><span data-ttu-id="75a6a-269">Jak odstranit objekt blob</span><span class="sxs-lookup"><span data-stu-id="75a6a-269">How to delete a blob</span></span>
<span data-ttu-id="75a6a-270">Pokud chcete odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte rutinu Remove-AzureStorageBlob na něm.</span><span class="sxs-lookup"><span data-stu-id="75a6a-270">To delete a blob, first get a blob reference and then call the Remove-AzureStorageBlob cmdlet on it.</span></span> <span data-ttu-id="75a6a-271">Následující příklad odstraní všechny objekty BLOB v daném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="75a6a-271">The following example deletes all the blobs in a given container.</span></span> <span data-ttu-id="75a6a-272">V příkladu nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-272">The example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="75a6a-273">V dalším kroku příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí [odebrat AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) pro odstranění objektů blob z kontejneru v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-273">Next, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet to remove blobs from a container in Azure storage.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to all the blobs in the container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-to-manage-azure-blob-snapshots"></a><span data-ttu-id="75a6a-274">Jak spravovat snímky objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-274">How to manage Azure blob snapshots</span></span>
<span data-ttu-id="75a6a-275">Azure umožňuje vytvoření snímku objektu blob.</span><span class="sxs-lookup"><span data-stu-id="75a6a-275">Azure lets you create a snapshot of a blob.</span></span> <span data-ttu-id="75a6a-276">Snímek je jen pro čtení verze objektu blob, který se pořídí na bod v čase.</span><span class="sxs-lookup"><span data-stu-id="75a6a-276">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="75a6a-277">Po vytvoření snímku, ho můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně.</span><span class="sxs-lookup"><span data-stu-id="75a6a-277">Once a snapshot has been created, it can be read, copied, or deleted, but not modified.</span></span> <span data-ttu-id="75a6a-278">Snímky poskytují způsob, jak zálohovat objekt blob, jak se objevuje v časovém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="75a6a-278">Snapshots provide a way to back up a blob as it appears at a moment in time.</span></span> <span data-ttu-id="75a6a-279">Další informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-279">For more information, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span>

### <a name="how-to-create-a-blob-snapshot"></a><span data-ttu-id="75a6a-280">Postup vytvoření snímku objektů blob</span><span class="sxs-lookup"><span data-stu-id="75a6a-280">How to create a blob snapshot</span></span>
<span data-ttu-id="75a6a-281">Chcete-li vytvořit snímek objektu blob, nejdřív získejte odkaz na objekt blob a potom volejte `ICloudBlob.CreateSnapshot` metoda na něm.</span><span class="sxs-lookup"><span data-stu-id="75a6a-281">To create a snaphot of a blob, first get a blob reference and then call the `ICloudBlob.CreateSnapshot` method on it.</span></span> <span data-ttu-id="75a6a-282">Následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-282">The following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="75a6a-283">V dalším kroku příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metodu pro vytvoření snímku.</span><span class="sxs-lookup"><span data-stu-id="75a6a-283">Next, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method to create a snapshot.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of the blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-to-list-a-blobs-snapshots"></a><span data-ttu-id="75a6a-284">Jak zobrazit objekt blob snímky</span><span class="sxs-lookup"><span data-stu-id="75a6a-284">How to list a blob's snapshots</span></span>
<span data-ttu-id="75a6a-285">Můžete vytvořit libovolný počet snímků, jak chcete použít pro objekt blob.</span><span class="sxs-lookup"><span data-stu-id="75a6a-285">You can create as many snapshots as you want for a blob.</span></span> <span data-ttu-id="75a6a-286">Můžete vytvořit seznam snímků přidružených objektu blob služby sledovat vaše aktuální snímky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-286">You can list the snapshots associated with your blob to track your current snapshots.</span></span> <span data-ttu-id="75a6a-287">Následující příklad používá předdefinovaných objektů blob a volání [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny seznamu snímky tomuto objektu blob.</span><span class="sxs-lookup"><span data-stu-id="75a6a-287">The following example uses a predefined blob and calls the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet to list the snapshots of that blob.</span></span>  

```powershell
#Define the blob name.
$BlobName = "yourblobname"

#List the snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-to-copy-a-snapshot-of-a-blob"></a><span data-ttu-id="75a6a-288">Postup kopírování snímku objektu blob</span><span class="sxs-lookup"><span data-stu-id="75a6a-288">How to copy a snapshot of a blob</span></span>
<span data-ttu-id="75a6a-289">Můžete zkopírovat snímek objektu blob obnovení snímku.</span><span class="sxs-lookup"><span data-stu-id="75a6a-289">You can copy a snapshot of a blob to restore the snapshot.</span></span> <span data-ttu-id="75a6a-290">Omezení a podrobné informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-290">For detailed information and restrictions, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span> <span data-ttu-id="75a6a-291">Následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-291">The following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="75a6a-292">V příkladu dále definuje název proměnné kontejnerů a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="75a6a-292">Next, the example defines the container and blob name variables.</span></span> <span data-ttu-id="75a6a-293">Příklad načte pomocí odkazu objektu blob [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metodu pro vytvoření snímku.</span><span class="sxs-lookup"><span data-stu-id="75a6a-293">The example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method to create a snapshot.</span></span> <span data-ttu-id="75a6a-294">Poté, v příkladu spustí [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny se kopírování snímku objektu blob pomocí objektu ICloudBlob pro zdrojový objekt blob.</span><span class="sxs-lookup"><span data-stu-id="75a6a-294">Then, the example runs the [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet to copy the snapshot of a blob using the ICloudBlob object for the source blob.</span></span> <span data-ttu-id="75a6a-295">Nezapomeňte aktualizovat proměnné na základě vaší konfigurace před spuštěním v příkladu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-295">Be sure to update the variables based on your configuration before running the example.</span></span> <span data-ttu-id="75a6a-296">Všimněte si, že v následujícím příkladu se předpokládá, že zdrojové a cílové kontejnery a zdrojový objekt blob již existují.</span><span class="sxs-lookup"><span data-stu-id="75a6a-296">Note that the following example assumes that the source and destination containers, and the source blob already exist.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define the variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy the snapshot to another container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

<span data-ttu-id="75a6a-297">Teď, když jste se naučili jak spravovat Azure objekty BLOB a objektů blob snímky s prostředím Azure PowerShell, přejděte k další části se dozvíte, jak spravovat tabulky, fronty a soubory.</span><span class="sxs-lookup"><span data-stu-id="75a6a-297">Now that you have learned how to manage Azure blobs and blob snapshots with Azure PowerShell, go to the next section to learn how to manage tables, queues, and files.</span></span>

## <a name="how-to-manage-azure-tables-and-table-entities"></a><span data-ttu-id="75a6a-298">Správa Azure tabulky a entity tabulky</span><span class="sxs-lookup"><span data-stu-id="75a6a-298">How to manage Azure tables and table entities</span></span>
<span data-ttu-id="75a6a-299">Služby úložiště Azure Table je úložiště dat typu NoSQL, který můžete použít k ukládání a dotazování obrovských sad strukturovaných, nerelačních data.</span><span class="sxs-lookup"><span data-stu-id="75a6a-299">Azure Table storage service is a NoSQL datastore, which you can use to store and query huge sets of structured, non-relational data.</span></span> <span data-ttu-id="75a6a-300">Hlavní komponenty služby jsou tabulky, entit a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="75a6a-300">The main components of the service are tables, entities, and properties.</span></span> <span data-ttu-id="75a6a-301">Tabulka je kolekce entit.</span><span class="sxs-lookup"><span data-stu-id="75a6a-301">A table is a collection of entities.</span></span> <span data-ttu-id="75a6a-302">Entita je sada vlastností.</span><span class="sxs-lookup"><span data-stu-id="75a6a-302">An entity is a set of properties.</span></span> <span data-ttu-id="75a6a-303">Každá entita může mít až 252 vlastností, které jsou všechny páry název hodnota.</span><span class="sxs-lookup"><span data-stu-id="75a6a-303">Each entity can have up to 252 properties, which are all name-value pairs.</span></span> <span data-ttu-id="75a6a-304">V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště Azure Table.</span><span class="sxs-lookup"><span data-stu-id="75a6a-304">This section assumes that you are already familiar with the Azure Table Storage Service concepts.</span></span> <span data-ttu-id="75a6a-305">Podrobné informace najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx) a [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-305">For detailed information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx) and [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="75a6a-306">V následující témata dozvíte, jak spravovat službu úložiště Azure Table pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-306">In the following subsections, you'll learn how to manage Azure Table storage service using Azure PowerShell.</span></span> <span data-ttu-id="75a6a-307">Pokryté scénáře zahrnují **vytváření**, **odstraňování**, a **načítání** **tabulky**, a také **přidání**, **dotazování**, a **odstranění entity tabulky**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-307">The scenarios covered include **creating**, **deleting**, and **retrieving** **tables**, as well as **adding**, **querying**, and **deleting table entities**.</span></span>

### <a name="how-to-create-a-table"></a><span data-ttu-id="75a6a-308">Postup vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="75a6a-308">How to create a table</span></span>
<span data-ttu-id="75a6a-309">Každá tabulka musí nacházet v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-309">Every table must reside in an Azure storage account.</span></span> <span data-ttu-id="75a6a-310">Následující příklad ukazuje, jak vytvořit tabulku ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-310">The following example demonstrates how to create a table in Azure Storage.</span></span> <span data-ttu-id="75a6a-311">V příkladu nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="75a6a-311">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="75a6a-312">Dále je použita [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutiny k vytvoření tabulky ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-312">Next, it uses the [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet to create a table in Azure Storage.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-retrieve-a-table"></a><span data-ttu-id="75a6a-313">Jak načíst tabulku</span><span class="sxs-lookup"><span data-stu-id="75a6a-313">How to retrieve a table</span></span>
<span data-ttu-id="75a6a-314">Můžete zadat dotaz a načtení jedné nebo všech tabulek v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-314">You can query and retrieve one or all tables in a Storage account.</span></span> <span data-ttu-id="75a6a-315">Následující příklad ukazuje, jak načíst dané tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-315">The following example demonstrates how to retrieve a given table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span>

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

<span data-ttu-id="75a6a-316">Při volání rutiny Get-AzureStorageTable bez parametrů, získá všechny tabulky úložiště pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-316">If you call the Get-AzureStorageTable cmdlet without any parameters, it gets all storage tables for a Storage account.</span></span>

### <a name="how-to-delete-a-table"></a><span data-ttu-id="75a6a-317">Postup odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="75a6a-317">How to delete a table</span></span>
<span data-ttu-id="75a6a-318">Můžete odstranit tabulku z účtu úložiště pomocí [odebrat AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-318">You can delete a table from a storage account by using the [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span></span>  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-manage-table-entities"></a><span data-ttu-id="75a6a-319">Jak spravovat entity tabulky</span><span class="sxs-lookup"><span data-stu-id="75a6a-319">How to manage table entities</span></span>
<span data-ttu-id="75a6a-320">V současné době prostředí Azure PowerShell neposkytuje rutin určených ke správě entity tabulky přímo.</span><span class="sxs-lookup"><span data-stu-id="75a6a-320">Currently, Azure PowerShell does not provide cmdlets to manage table entities directly.</span></span> <span data-ttu-id="75a6a-321">K provádění operací na entity tabulky, můžete použít třídy zadaná v [Klientská knihovna pro úložiště Azure pro .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-321">To perform operations on table entities, you can use the classes given in the [Azure Storage Client Library for .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span></span>

#### <a name="how-to-add-table-entities"></a><span data-ttu-id="75a6a-322">Postup přidání entity tabulky</span><span class="sxs-lookup"><span data-stu-id="75a6a-322">How to add table entities</span></span>
<span data-ttu-id="75a6a-323">Chcete-li do tabulky přidat entitu, nejprve vytvořte objekt, který definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="75a6a-323">To add an entity to a table, first create an object that defines your entity properties.</span></span> <span data-ttu-id="75a6a-324">Entita může mít maximálně 255 vlastnosti, včetně 3 Vlastnosti systému: **PartitionKey**, **RowKey**, a **časové razítko**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-324">An entity can have up to 255 properties, including 3 system properties: **PartitionKey**, **RowKey**, and **Timestamp**.</span></span> <span data-ttu-id="75a6a-325">Jste zodpovědní za vložení a aktualizace hodnoty **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-325">You are responsible for inserting and updating the values of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="75a6a-326">Server spravuje hodnotu **časové razítko**, který nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="75a6a-326">The server manages the value of **Timestamp**, which cannot be modified.</span></span> <span data-ttu-id="75a6a-327">Společně **PartitionKey** a **RowKey** jednoznačně identifikovat každou entitu v tabulce.</span><span class="sxs-lookup"><span data-stu-id="75a6a-327">Together the **PartitionKey** and **RowKey** uniquely identify every entity within a table.</span></span>

* <span data-ttu-id="75a6a-328">**PartitionKey**: Určuje uložených entity v oddílu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-328">**PartitionKey**: Determines the partition that the entity is stored in.</span></span>
* <span data-ttu-id="75a6a-329">**RowKey**: jednoznačně identifikuje entity v oddílu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-329">**RowKey**: Uniquely identifies the entity within the partition.</span></span>

<span data-ttu-id="75a6a-330">Můžete definovat vlastní až 252 vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="75a6a-330">You may define up to 252 custom properties for an entity.</span></span> <span data-ttu-id="75a6a-331">Další informace najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-331">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="75a6a-332">Následující příklad ukazuje, jak přidat entity do tabulky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-332">The following example demonstrates how to add entities to a table.</span></span> <span data-ttu-id="75a6a-333">Tento příklad ukazuje, jak načíst tabulky zaměstnanců a přidejte do ní několik entit.</span><span class="sxs-lookup"><span data-stu-id="75a6a-333">The example shows how to retrieve the employee table and add several entities into it.</span></span> <span data-ttu-id="75a6a-334">Nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="75a6a-334">First, it establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="75a6a-335">V dalším kroku načte dané tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-335">Next, it retrieves the given table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="75a6a-336">Pokud tabulka neexistuje, [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutina se používá k vytvoření tabulky ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-336">If the table does not exist, the [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet is used to create a table in Azure Storage.</span></span> <span data-ttu-id="75a6a-337">V příkladu dále definuje vlastní funkci Přidat Entity k přidání entity do tabulky se zadáním každé entity oddílu a klíč řádku.</span><span class="sxs-lookup"><span data-stu-id="75a6a-337">Next, the example defines a custom function Add-Entity to add entities to the table by specifying each entity's partition and row key.</span></span> <span data-ttu-id="75a6a-338">Volání funkcí přidat Entity [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na rutinu [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) třídy za účelem vytvoření objektu entity.</span><span class="sxs-lookup"><span data-stu-id="75a6a-338">The Add-Entity function calls the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on the [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) class to create an entity object.</span></span> <span data-ttu-id="75a6a-339">Později v příkladu volá [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) metoda na tento objekt entity přidat do tabulky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-339">Later, the example calls the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) method on this entity object to add it to a table.</span></span>

```powershell
#Function Add-Entity: Adds an employee entity to a table.
function Add-Entity() {
    [CmdletBinding()]
    param(
       $table,
       [String]$partitionKey,
       [String]$rowKey,
       [String]$name,
       [Int]$id
    )

  $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
  $entity.Properties.Add("Name", $name)
  $entity.Properties.Add("ID", $id)

  $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
}

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve the table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities to a table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-to-query-table-entities"></a><span data-ttu-id="75a6a-340">Postup dotazování entity tabulky</span><span class="sxs-lookup"><span data-stu-id="75a6a-340">How to query table entities</span></span>
<span data-ttu-id="75a6a-341">Dotaz na tabulku, použijte [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="75a6a-341">To query a table, use the [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) class.</span></span> <span data-ttu-id="75a6a-342">V následujícím příkladu se předpokládá, že jste již spusťte skript v pokynů pro přidání entity části této příručky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-342">The following example assumes that you've already run the script given in the How to add entities section of this guide.</span></span> <span data-ttu-id="75a6a-343">V příkladu nejprve vytvoří připojení k Azure Storage pomocí storage kontext, který obsahuje název účtu úložiště a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="75a6a-343">The example first establishes a connection to Azure Storage using the storage context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="75a6a-344">Další, pokusí se načíst dříve vytvořenou "zaměstnanci" tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-344">Next, it tries to retrieve the previously created "Employees" table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="75a6a-345">Volání [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na třídě Microsoft.WindowsAzure.Storage.Table.TableQuery rutinu vytvoří nový objekt dotazu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-345">Calling the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on the Microsoft.WindowsAzure.Storage.Table.TableQuery class creates a new query object.</span></span> <span data-ttu-id="75a6a-346">V příkladu vypadá pro entity, které mají na sloupec 'ID', jehož hodnota je 1 zadané v řetězec filtru.</span><span class="sxs-lookup"><span data-stu-id="75a6a-346">The example looks for the entities that have an 'ID' column whose value is 1 as specified in a string filter.</span></span> <span data-ttu-id="75a6a-347">Podrobné informace najdete v tématu [dotazování tabulky a entity](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-347">For detailed information, see [Querying Tables and Entities](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span></span> <span data-ttu-id="75a6a-348">Při spuštění tohoto dotazu vrátí všechny entity, které odpovídají kritériím filtru.</span><span class="sxs-lookup"><span data-stu-id="75a6a-348">When you run this query, it returns all entities that match the filter criteria.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference to a table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns to select.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute the query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with the table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-to-delete-table-entities"></a><span data-ttu-id="75a6a-349">Postup odstranění entity tabulky</span><span class="sxs-lookup"><span data-stu-id="75a6a-349">How to delete table entities</span></span>
<span data-ttu-id="75a6a-350">Můžete odstranit pomocí jeho klíče oddílu a řádku entity.</span><span class="sxs-lookup"><span data-stu-id="75a6a-350">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="75a6a-351">V následujícím příkladu se předpokládá, že jste již spusťte skript v pokynů pro přidání entity části této příručky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-351">The following example assumes that you've already run the script given in the How to add entities section of this guide.</span></span> <span data-ttu-id="75a6a-352">V příkladu nejprve vytvoří připojení k Azure Storage pomocí storage kontext, který obsahuje název účtu úložiště a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="75a6a-352">The example first establishes a connection to Azure Storage using the storage context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="75a6a-353">Další, pokusí se načíst dříve vytvořenou "zaměstnanci" tabulky pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-353">Next, it tries to retrieve the previously created "Employees" table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="75a6a-354">Pokud v tabulce existuje, zavolá na příklad [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metoda pro načtení entity podle jeho oddílu a řádku klíčové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="75a6a-354">If the table exists, the example calls the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) method to retrieve an entity based on its partition and row key values.</span></span> <span data-ttu-id="75a6a-355">Pak předejte entity, která má [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metoda odstranit.</span><span class="sxs-lookup"><span data-stu-id="75a6a-355">Then, pass the entity to the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) method to delete.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If the table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together the PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete the entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-to-manage-azure-queues-and-queue-messages"></a><span data-ttu-id="75a6a-356">Správa Azure fronty a zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="75a6a-356">How to manage Azure queues and queue messages</span></span>
<span data-ttu-id="75a6a-357">Azure Queue Storage je služba pro ukládání velkého počtu zpráv, ke které můžete získat přístup z jakéhokoli místa na světě prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="75a6a-357">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="75a6a-358">V této části se předpokládá, že jste již obeznámeni s koncepty Azure fronty úložiště služby.</span><span class="sxs-lookup"><span data-stu-id="75a6a-358">This section assumes that you are already familiar with the Azure Queue Storage Service concepts.</span></span> <span data-ttu-id="75a6a-359">Podrobné informace najdete v tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-359">For detailed information, see [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md).</span></span>

<span data-ttu-id="75a6a-360">V této části vám ukáže, jak spravovat službu úložiště Azure Queue pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-360">This section will show you how to manage Azure Queue storage service using Azure PowerShell.</span></span> <span data-ttu-id="75a6a-361">Pokryté scénáře zahrnují **vkládání** a **odstraňování** fronty zpráv, a také **vytváření**, **odstraňování**, a **načítání fronty**.</span><span class="sxs-lookup"><span data-stu-id="75a6a-361">The scenarios covered include **inserting** and **deleting** queue messages, as well as **creating**, **deleting**, and **retrieving queues**.</span></span>

### <a name="how-to-create-a-queue"></a><span data-ttu-id="75a6a-362">Postup vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="75a6a-362">How to create a queue</span></span>
<span data-ttu-id="75a6a-363">Následující příklad nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="75a6a-363">The following example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="75a6a-364">Dále volá [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) rutiny vytvořit frontu s názvem 'queuename'.</span><span class="sxs-lookup"><span data-stu-id="75a6a-364">Next, it calls [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet to create a queue named 'queuename'.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

<span data-ttu-id="75a6a-365">Informace o vytváření názvů pro službu front Azure, najdete v části [pojmenování front a Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-365">For information on naming conventions for Azure Queue Service, see [Naming Queues and Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

### <a name="how-to-retrieve-a-queue"></a><span data-ttu-id="75a6a-366">Jak načíst fronty</span><span class="sxs-lookup"><span data-stu-id="75a6a-366">How to retrieve a queue</span></span>
<span data-ttu-id="75a6a-367">Můžete zadat dotaz a načíst konkrétní fronty nebo seznam všech front, které jsou v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-367">You can query and retrieve a specific queue or a list of all the queues in a Storage account.</span></span> <span data-ttu-id="75a6a-368">Následující příklad ukazuje, jak načíst zadané frontě pomocí [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-368">The following example demonstrates how to retrieve a specified queue using the [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span></span>

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

<span data-ttu-id="75a6a-369">Když zavoláte [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny bez parametrů, získá seznam všech front.</span><span class="sxs-lookup"><span data-stu-id="75a6a-369">If you call the [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet without any parameters, it gets a list of all the queues.</span></span>

### <a name="how-to-delete-a-queue"></a><span data-ttu-id="75a6a-370">Postup odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="75a6a-370">How to delete a queue</span></span>
<span data-ttu-id="75a6a-371">Chcete-li odstranit frontu se všemi zprávami, které v ní, zavolejte rutinu Remove-AzureStorageQueue.</span><span class="sxs-lookup"><span data-stu-id="75a6a-371">To delete a queue and all the messages contained in it, call the Remove-AzureStorageQueue cmdlet.</span></span> <span data-ttu-id="75a6a-372">Následující příklad ukazuje, jak odstranit pomocí rutiny Remove-AzureStorageQueue zadanou frontu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-372">The following example shows how to delete a specified queue using the Remove-AzureStorageQueue cmdlet.</span></span>

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="75a6a-373">Postup vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="75a6a-373">How to insert a message into a queue</span></span>
<span data-ttu-id="75a6a-374">Pokud chcete vložit zprávu do existující fronty, nejprve vytvořit novou instanci třídy [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="75a6a-374">To insert a message into an existing queue, first create a new instance of the [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="75a6a-375">Pak zavolejte metodu [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-375">Next, call the [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method.</span></span> <span data-ttu-id="75a6a-376">CloudQueueMessage můžete vytvořit z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="75a6a-376">A CloudQueueMessage can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="75a6a-377">Následující příklad ukazuje, jak přidat zprávu do fronty.</span><span class="sxs-lookup"><span data-stu-id="75a6a-377">The following example demonstrates how to add message to a queue.</span></span> <span data-ttu-id="75a6a-378">V příkladu nejprve vytvoří připojení k Azure Storage pomocí kontext účtu úložiště, která zahrnuje název účtu úložiště a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="75a6a-378">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="75a6a-379">V dalším kroku načte zadané frontě pomocí [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="75a6a-379">Next, it retrieves the specified queue using the [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span></span> <span data-ttu-id="75a6a-380">Pokud fronta existuje, [New-Object](http://technet.microsoft.com/library/hh849885.aspx) rutina se používá k vytvoření instance [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="75a6a-380">If the queue exists, the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet is used to create an instance of the [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="75a6a-381">Později v příkladu volá [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metoda na tento objekt zpráva tím ho přidáte do fronty.</span><span class="sxs-lookup"><span data-stu-id="75a6a-381">Later, the example calls the [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method on this message object to add it to a queue.</span></span> <span data-ttu-id="75a6a-382">Zde je kód, který načte fronty a vloží zprávu "MessageInfo":</span><span class="sxs-lookup"><span data-stu-id="75a6a-382">Here is code which retrieves a queue and inserts the message 'MessageInfo':</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If the queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of the CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message to the queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-to-de-queue-at-the-next-message"></a><span data-ttu-id="75a6a-383">Postup zrušte další zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="75a6a-383">How to de-queue at the next message</span></span>
<span data-ttu-id="75a6a-384">Váš kód vyřazuje zprávy z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="75a6a-384">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="75a6a-385">Při volání [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metoda, získáte další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="75a6a-385">When you call the [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) method, you get the next message in a queue.</span></span> <span data-ttu-id="75a6a-386">Zpráva vrácená metodou **GetMessage** se stane neviditelnou pro jakýkoli jiný kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="75a6a-386">A message returned from **GetMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="75a6a-387">K dokončení odebrání zprávy z fronty, musíte také zavolat [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="75a6a-387">To finish removing the message from the queue, you must also call the [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) method.</span></span> <span data-ttu-id="75a6a-388">Tento dvoukrokový proces odebrání zprávy zaručuje, aby v případě, že se vašemu kódu nepodaří zprávu zpracovat z důvodu selhání hardwaru nebo softwaru, mohla stejnou zprávu získat jiná instance vašeho kódu a bylo možné to zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-388">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="75a6a-389">Váš kód zavolá metodu **DeleteMessage** hned po zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="75a6a-389">Your code calls **DeleteMessage** right after the message has been processed.</span></span>

```powershell
# Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get the message object from the queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete the message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-to-manage-azure-file-shares-and-files"></a><span data-ttu-id="75a6a-390">Správa Azure sdílených složek a souborů</span><span class="sxs-lookup"><span data-stu-id="75a6a-390">How to manage Azure file shares and files</span></span>
<span data-ttu-id="75a6a-391">Azure File storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="75a6a-391">Azure File storage offers shared storage for applications using the standard SMB protocol.</span></span> <span data-ttu-id="75a6a-392">Virtuální počítače Microsoft Azure a cloudových služeb můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím rozhraní API úložiště souboru nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-392">Microsoft Azure virtual machines and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share via the File storage API or Azure PowerShell.</span></span>

<span data-ttu-id="75a6a-393">Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](storage-dotnet-how-to-use-files.md) a [rozhraní API REST služby soubor](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span><span class="sxs-lookup"><span data-stu-id="75a6a-393">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) and [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span></span>

## <a name="how-to-set-and-query-storage-analytics"></a><span data-ttu-id="75a6a-394">Jak nastavit a analytika úložiště dotazů</span><span class="sxs-lookup"><span data-stu-id="75a6a-394">How to set and query storage analytics</span></span>
<span data-ttu-id="75a6a-395">Můžete použít [Azure Storage Analytics](storage-analytics.md) ke shromažďování metrik pro účty úložiště Azure a protokolovat data o požadavky odeslané na účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-395">You can use [Azure Storage Analytics](storage-analytics.md) to collect metrics for your Azure storage accounts and log data about requests sent to your storage account.</span></span> <span data-ttu-id="75a6a-396">Úložiště metriky můžete monitorovat stav účtu úložiště a úložiště protokolování pro diagnostiku a řešení problémů s vaším účtem úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-396">You can use storage metrics to monitor the health of a storage account, and storage logging to diagnose and troubleshoot issues with your storage account.</span></span> <span data-ttu-id="75a6a-397">Můžete nakonfigurovat monitorování pomocí portálu Azure nebo prostředí Windows PowerShell nebo programově pomocí klientské knihovny pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-397">You can configure monitoring using the Azure portal or Windows PowerShell, or programmatically using the storage client library.</span></span> <span data-ttu-id="75a6a-398">Protokolování úložiště se stane, serverový a umožní vám si zaznamenejte podrobnosti pro úspěšné i neúspěšné požadavky ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="75a6a-398">Storage logging happens server-side and enables you to record details for both successful and failed requests in your storage account.</span></span> <span data-ttu-id="75a6a-399">Tyto protokoly umožňují najdete v části Podrobnosti o čtení, zápisu a odstraňování operace u tabulky, fronty a objekty BLOB, jakož i důvody pro chybné žádosti.</span><span class="sxs-lookup"><span data-stu-id="75a6a-399">These logs enable you to see details of read, write, and delete operations against your tables, queues, and blobs as well as the reasons for failed requests.</span></span>

<span data-ttu-id="75a6a-400">Informace o povolení a zobrazovat data metriky úložiště pomocí prostředí PowerShell najdete v tématu [povolení metrik úložiště pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span><span class="sxs-lookup"><span data-stu-id="75a6a-400">To learn how to enable and view Storage Metrics data using PowerShell, see [How to enable Storage Metrics using PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span></span>

<span data-ttu-id="75a6a-401">Informace o povolení a načtení dat protokolování úložiště pomocí prostředí PowerShell najdete v tématu [povolení protokolování úložiště pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) a [hledání data protokolu protokolování úložiště](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span><span class="sxs-lookup"><span data-stu-id="75a6a-401">To learn how to enable and retrieve Storage Logging data using PowerShell, see [How to enable Storage Logging using PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) and [Finding your Storage Logging log data](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span></span>
<span data-ttu-id="75a6a-402">Podrobné informace o použití úložiště metrik a protokolování úložiště k řešení problémů s úložištěm najdete v tématu [monitorování, diagnostikování a řešení potíží s Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-402">For detailed information on using Storage Metrics and Storage Logging to troubleshoot storage issues, see [Monitoring, Diagnosing, and Troubleshooting Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span></span>

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a><span data-ttu-id="75a6a-403">Správa sdíleného přístupového podpisu (SAS) a uložené zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="75a6a-403">How to manage Shared Access Signature (SAS) and Stored Access Policy</span></span>
<span data-ttu-id="75a6a-404">Sdílené přístupové podpisy jsou důležitou součástí model zabezpečení pro všechny aplikace pomocí Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-404">Shared access signatures are an important part of the security model for any application using Azure Storage.</span></span> <span data-ttu-id="75a6a-405">Jsou užitečné pro zajištění omezené oprávnění k účtu úložiště na klienty, kteří by neměl mít klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-405">They are useful for providing limited permissions to your storage account to clients that should not have the account key.</span></span> <span data-ttu-id="75a6a-406">Ve výchozím nastavení může přístup pouze vlastník účtu úložiště objektů BLOB, tabulek a v rámci tohoto účtu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-406">By default, only the owner of the storage account may access blobs, tables, and queues within that account.</span></span> <span data-ttu-id="75a6a-407">Pokud je potřeba zpřístupnit tyto prostředky pro ostatní klienty bez sdílení přístupový klíč služby nebo aplikace, máte tři možnosti:</span><span class="sxs-lookup"><span data-stu-id="75a6a-407">If your service or application needs to make these resources available to other clients without sharing your access key, you have three options:</span></span>

* <span data-ttu-id="75a6a-408">Nastavte oprávnění kontejner pro povolení anonymního přístupu pro čtení ke kontejneru a jeho objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="75a6a-408">Set a container's permissions to permit anonymous read access to the container and its blobs.</span></span> <span data-ttu-id="75a6a-409">To není povolené pro tabulky a fronty.</span><span class="sxs-lookup"><span data-stu-id="75a6a-409">This is not allowed for tables or queues.</span></span>
* <span data-ttu-id="75a6a-410">Použijte sdílený přístupový podpis, udělí omezený přístupová práva k kontejnerů, objektů BLOB, fronty a tabulky pro určitého časového intervalu.</span><span class="sxs-lookup"><span data-stu-id="75a6a-410">Use a shared access signature that grants restricted access rights to containers, blobs, queues, and tables for a specific time interval.</span></span>
* <span data-ttu-id="75a6a-411">Pomocí zásad uložené přístup získat další úroveň kontroly nad sdílené přístupové podpisy pro kontejner nebo jeho objekty BLOB, fronty nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="75a6a-411">Use a stored access policy to obtain an additional level of control over shared access signatures for a container or its blobs, for a queue, or for a table.</span></span> <span data-ttu-id="75a6a-412">Zásady uložené přístupu vám umožní změnit čas zahájení, čas vypršení platnosti nebo oprávnění pro podpis, nebo ho zablokujte po jeho bylo vydáno.</span><span class="sxs-lookup"><span data-stu-id="75a6a-412">The stored access policy allows you to change the start time, expiry time, or permissions for a signature, or to revoke it after it has been issued.</span></span>

<span data-ttu-id="75a6a-413">Sdílený přístupový podpis může být v jednom ze dvou formách:</span><span class="sxs-lookup"><span data-stu-id="75a6a-413">A shared access signature can be in one of two forms:</span></span>

* <span data-ttu-id="75a6a-414">**Ad hoc SAS**: Když vytvoříte ad hoc SAS, čas zahájení, čas vypršení platnosti a oprávnění pro SAS se zadaný v identifikátoru URI SAS.</span><span class="sxs-lookup"><span data-stu-id="75a6a-414">**Ad hoc SAS**: When you create an ad hoc SAS, the start time, expiry time, and permissions for the SAS are all specified on the SAS URI.</span></span> <span data-ttu-id="75a6a-415">Tento typ SAS mohou být vytvořeny na kontejner, objektů blob, tabulka nebo fronty a je jiný odvolatelné.</span><span class="sxs-lookup"><span data-stu-id="75a6a-415">This type of SAS may be created on a container, blob, table, or queue and it is non-revocable.</span></span>
* <span data-ttu-id="75a6a-416">**SAS se zásadami přístupu uložené**: zásadu uložené přístupu je definován na kontejner prostředků kontejner objektů blob, tabulka nebo fronta – a můžete ji použít ke správě omezení pro jeden nebo více sdílených přístupových podpisů.</span><span class="sxs-lookup"><span data-stu-id="75a6a-416">**SAS with stored access policy**: A stored access policy is defined on a resource container a blob container, table, or queue - and you can use it to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="75a6a-417">Pokud přidružíte SAS se zásadami přístupu uložené, zdědí SAS omezení – čas spuštění, čas vypršení platnosti a - definována pro zásady uložené přístupu oprávnění.</span><span class="sxs-lookup"><span data-stu-id="75a6a-417">When you associate a SAS with a stored access policy, the SAS inherits the constraints - the start time, expiry time, and permissions - defined for the stored access policy.</span></span> <span data-ttu-id="75a6a-418">Tento typ SAS se odvolatelné.</span><span class="sxs-lookup"><span data-stu-id="75a6a-418">This type of SAS is revocable.</span></span>

<span data-ttu-id="75a6a-419">Další informace najdete v tématu [pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md) a [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-419">For more information, see [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) and [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>

<span data-ttu-id="75a6a-420">V následujících částech se dozvíte, jak vytvořit zásady sdíleného přístupu podpis tokenu a uložené přístup pro tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="75a6a-420">In the next sections, you will learn how to create a shared access signature token and stored access policy for Azure tables.</span></span> <span data-ttu-id="75a6a-421">Prostředí Azure PowerShell poskytuje podobné rutiny pro kontejnery objektů BLOB a fronty i.</span><span class="sxs-lookup"><span data-stu-id="75a6a-421">Azure PowerShell provides similar cmdlets for containers, blobs, and queues as well.</span></span> <span data-ttu-id="75a6a-422">Chcete-li spustit skripty v této části, stáhněte [prostředí Azure PowerShell verze 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="75a6a-422">To run the scripts in this section, download the [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) or later.</span></span>

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a><span data-ttu-id="75a6a-423">Jak vytvořit token podpis sdíleného přístupu na základě zásad</span><span class="sxs-lookup"><span data-stu-id="75a6a-423">How to create a policy-based Shared Access Signature token</span></span>
<span data-ttu-id="75a6a-424">Pomocí rutiny New-AzureStorageTableStoredAccessPolicy vytvořit novou zásadu uložené přístup.</span><span class="sxs-lookup"><span data-stu-id="75a6a-424">Use the New-AzureStorageTableStoredAccessPolicy cmdlet to create a new stored access policy.</span></span> <span data-ttu-id="75a6a-425">Potom zavolejte [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) vytvořte nový token podpis sdíleného přístupu na základě zásad pro tabulku Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75a6a-425">Then, call the [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet to create a new policy-based shared access signature token for an Azure Storage table.</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-to-create-an-ad-hoc-non-revocable-shared-access-signature-token"></a><span data-ttu-id="75a6a-426">Postup vytvoření tokenu ad hoc sdíleného přístupového podpisu (bez odvolatelné)</span><span class="sxs-lookup"><span data-stu-id="75a6a-426">How to create an ad hoc (non-revocable) Shared Access Signature token</span></span>
<span data-ttu-id="75a6a-427">Použití [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) vytvořte nový ad hoc (bez odvolatelné) sdíleného přístupového podpisu token pro tabulku Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="75a6a-427">Use the [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet to create a new ad hoc (non-revocable) Shared Access Signature token for an Azure Storage table:</span></span>

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-to-create-a-stored-access-policy"></a><span data-ttu-id="75a6a-428">Postup vytvoření zásady uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="75a6a-428">How to create a stored access policy</span></span>
<span data-ttu-id="75a6a-429">Pomocí rutiny New-AzureStorageTableStoredAccessPolicy vytvořit novou zásadu uložené přístup tabulce Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="75a6a-429">Use the New-AzureStorageTableStoredAccessPolicy cmdlet to create a new stored access policy for an Azure Storage table:</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-to-update-a-stored-access-policy"></a><span data-ttu-id="75a6a-430">Postup aktualizace zásad uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="75a6a-430">How to update a stored access policy</span></span>
<span data-ttu-id="75a6a-431">Použijte rutinu Set-AzureStorageTableStoredAccessPolicy aktualizovat existující zásady uložené přístup tabulce Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="75a6a-431">Use the Set-AzureStorageTableStoredAccessPolicy cmdlet to update an existing stored access policy for an Azure Storage table:</span></span>

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-to-delete-a-stored-access-policy"></a><span data-ttu-id="75a6a-432">Postup odstranění zásady uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="75a6a-432">How to delete a stored access policy</span></span>
<span data-ttu-id="75a6a-433">Použijte rutinu Remove-AzureStorageTableStoredAccessPolicy odstranění zásady přístupu uložené v tabulce Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="75a6a-433">Use the Remove-AzureStorageTableStoredAccessPolicy cmdlet to delete a stored access policy on an Azure Storage table:</span></span>

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a><span data-ttu-id="75a6a-434">Jak používat Azure Storage pro vládou USA a Azure China</span><span class="sxs-lookup"><span data-stu-id="75a6a-434">How to use Azure Storage for U.S. government and Azure China</span></span>
<span data-ttu-id="75a6a-435">Prostředí Azure je nezávislé nasazení ve službě Microsoft Azure, jako například [Azure Government pro US government](https://azure.microsoft.com/features/gov/), [AzureCloud pro globální Azure](https://portal.azure.com), a [AzureChinaCloud pro Azure provozované v Číně společností 21Vianet](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="75a6a-435">An Azure environment is an independent deployment of Microsoft Azure, such as [Azure Government for U.S. government](https://azure.microsoft.com/features/gov/), [AzureCloud for global Azure](https://portal.azure.com), and [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span> <span data-ttu-id="75a6a-436">Můžete nasadit nové prostředí Azure government ministerstvo a Azure China.</span><span class="sxs-lookup"><span data-stu-id="75a6a-436">You can deploy new Azure environments for U.S government and Azure China.</span></span>

<span data-ttu-id="75a6a-437">Používání Azure Storage s AzureChinaCloud, musíte vytvořit kontext úložiště, který je přidružen AzureChinaCloud.</span><span class="sxs-lookup"><span data-stu-id="75a6a-437">To use Azure Storage with AzureChinaCloud, you need to create a storage context that is associated with AzureChinaCloud.</span></span> <span data-ttu-id="75a6a-438">Postupujte podle těchto kroků, které vám pomůžou začít:</span><span class="sxs-lookup"><span data-stu-id="75a6a-438">Follow these steps to get you started:</span></span>

1. <span data-ttu-id="75a6a-439">Spustit [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) rutiny zobrazíte dostupné prostředí Azure:</span><span class="sxs-lookup"><span data-stu-id="75a6a-439">Run the [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet to see the available Azure environments:</span></span>
   
    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="75a6a-440">Přidání účtu Azure China do prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="75a6a-440">Add an Azure China account to Windows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. <span data-ttu-id="75a6a-441">Vytvoření kontextu úložiště pro účet AzureChinaCloud:</span><span class="sxs-lookup"><span data-stu-id="75a6a-441">Create a storage context for an AzureChinaCloud account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

<span data-ttu-id="75a6a-442">Používání Azure Storage s [USA Azure Government](https://azure.microsoft.com/features/gov/), měli byste definovat nové prostředí a pak vytvořte nový kontext úložiště s prostředím tohoto nástroje:</span><span class="sxs-lookup"><span data-stu-id="75a6a-442">To use Azure Storage with [U.S. Azure Government](https://azure.microsoft.com/features/gov/), you should define a new environment and then create a new storage context with this environment:</span></span>

1. <span data-ttu-id="75a6a-443">Spustit [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) rutiny zobrazíte dostupné prostředí Azure:</span><span class="sxs-lookup"><span data-stu-id="75a6a-443">Run the [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet to see the available Azure environments:</span></span>

    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="75a6a-444">Přidání účtu Azure US Government do prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="75a6a-444">Add an Azure US Government account to Windows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. <span data-ttu-id="75a6a-445">Vytvoření kontextu úložiště pro účet AzureUSGovernment:</span><span class="sxs-lookup"><span data-stu-id="75a6a-445">Create a storage context for an AzureUSGovernment account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
<span data-ttu-id="75a6a-446">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="75a6a-446">For more information, see:</span></span>

* <span data-ttu-id="75a6a-447">[Příručka vývojáře pro službu Microsoft Azure Government](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="75a6a-447">[Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>
* [<span data-ttu-id="75a6a-448">Přehled rozdíly při vytváření aplikace v Číně služby</span><span class="sxs-lookup"><span data-stu-id="75a6a-448">Overview of Differences When Creating an Application on China Service</span></span>](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a><span data-ttu-id="75a6a-449">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75a6a-449">Next Steps</span></span>
<span data-ttu-id="75a6a-450">V této příručce když jste se naučili jak spravovat úložiště Azure pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a6a-450">In this guide, you've learned how to manage Azure Storage with Azure PowerShell.</span></span> <span data-ttu-id="75a6a-451">Tady jsou některé související články a zdroje informací o nich víc.</span><span class="sxs-lookup"><span data-stu-id="75a6a-451">Here are some related articles and resources for learning more about them.</span></span>

* [<span data-ttu-id="75a6a-452">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="75a6a-452">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="75a6a-453">Rutiny prostředí PowerShell úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="75a6a-453">Azure Storage PowerShell Cmdlets</span></span>](/powershell/module/azurerm.storage/#storage)
* [<span data-ttu-id="75a6a-454">Referenční informace prostředí PowerShell systému Windows</span><span class="sxs-lookup"><span data-stu-id="75a6a-454">Windows PowerShell Reference</span></span>](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
