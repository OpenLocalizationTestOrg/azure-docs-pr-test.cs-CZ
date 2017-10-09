---
title: "aaaUsing prostředí Azure PowerShell s Azure Storage | Microsoft Docs"
description: "Zjistěte, jak toouse hello rutin Azure Powershellu pro Azure Storage toocreate a spravovat účty úložiště; Práce s objekty BLOB, tabulky, fronty a soubory; Nakonfigurujte a dotaz analytika úložiště a vytvořte sdílených přístupových podpisů."
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
ms.openlocfilehash: befe7adda2384f8bcdb8b9f1a063e4eafc158271
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a><span data-ttu-id="61829-103">Použití Azure Powershell s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="61829-103">Using Azure PowerShell with Azure Storage</span></span>
## <a name="overview"></a><span data-ttu-id="61829-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="61829-104">Overview</span></span>
<span data-ttu-id="61829-105">Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-105">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="61829-106">Je to prostředí příkazového řádku založené na úlohách a skriptovací jazyk určený speciálně pro správu systému.</span><span class="sxs-lookup"><span data-stu-id="61829-106">It is a task-based command-line shell and scripting language designed especially for system administration.</span></span> <span data-ttu-id="61829-107">Pomocí prostředí PowerShell můžete snadno řídit a automatizovat správu hello Azure služby a aplikace.</span><span class="sxs-lookup"><span data-stu-id="61829-107">With PowerShell, you can easily control and automate hello administration of your Azure services and applications.</span></span> <span data-ttu-id="61829-108">Například můžete použít hello rutiny tooperform hello stejné úlohy, že můžete provádět prostřednictvím hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="61829-108">For example, you can use hello cmdlets tooperform hello same tasks that you can perform through hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="61829-109">V této příručce, jsme budete prozkoumat jak toouse hello [rutiny úložiště Azure](/powershell/module/azurerm.storage/#storage) tooperform řadu vývoj a správu úloh s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-109">In this guide, we'll explore how toouse hello [Azure Storage Cmdlets](/powershell/module/azurerm.storage/#storage) tooperform a variety of development and administration tasks with Azure Storage.</span></span>

<span data-ttu-id="61829-110">Tato příručka předpokládá, že máte zkušenosti s použitím [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) a [prostředí Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-110">This guide assumes that you have prior experience using [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) and [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span></span> <span data-ttu-id="61829-111">Příručka Hello obsahuje řadu skriptů toodemonstrate hello použití prostředí PowerShell s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-111">hello guide provides a number of scripts toodemonstrate hello usage of PowerShell with Azure Storage.</span></span> <span data-ttu-id="61829-112">Proměnné skriptu hello na základě vaší konfigurace před spuštěním každý skript by měl aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="61829-112">You should update hello script variables based on your configuration before running each script.</span></span>

<span data-ttu-id="61829-113">první část Hello v této příručce poskytuje stručný přehled Azure Storage a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-113">hello first section in this guide provides a quick glance at Azure Storage and PowerShell.</span></span> <span data-ttu-id="61829-114">Podrobné informace a pokyny, spusťte z hello [požadavky pro použití prostředí Azure PowerShell s Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="61829-114">For detailed information and instructions, start from hello [Prerequisites for using Azure PowerShell with Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span></span>

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a><span data-ttu-id="61829-115">Začínáme s Azure Storage a prostředí PowerShell během 5 minut</span><span class="sxs-lookup"><span data-stu-id="61829-115">Getting started with Azure Storage and PowerShell in 5 minutes</span></span>
<span data-ttu-id="61829-116">V této části se dozvíte, jak tooaccess Azure Storage pomocí prostředí PowerShell během 5 minut.</span><span class="sxs-lookup"><span data-stu-id="61829-116">This section shows you how tooaccess Azure Storage via PowerShell in 5 minutes.</span></span>

<span data-ttu-id="61829-117">**Nové tooAzure:** získat předplatné Microsoft Azure a účet Microsoft přidružené k tomuto předplatnému.</span><span class="sxs-lookup"><span data-stu-id="61829-117">**New tooAzure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="61829-118">Informace o možnostech nákupu Azure najdete v tématu [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [zakoupit možnosti](https://azure.microsoft.com/pricing/purchase-options/), a [člen nabízí](https://azure.microsoft.com/pricing/member-offers/) (pro členy MSDN, BizSpark, Microsoft Partner Network, a další programy společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="61829-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="61829-119">V tématu [přiřazení rolí správce v Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Další informace o předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="61829-120">**Po vytvoření odběru služby Microsoft Azure a účet:**</span><span class="sxs-lookup"><span data-stu-id="61829-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="61829-121">Stáhněte a nainstalujte nejnovější hello [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span><span class="sxs-lookup"><span data-stu-id="61829-121">Download and install hello latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span></span>
2. <span data-ttu-id="61829-122">Spuštění Windows PowerShell integrované skriptování prostředí (ISE): V místním počítači, přejděte toohello **spustit** nabídky.</span><span class="sxs-lookup"><span data-stu-id="61829-122">Start Windows PowerShell Integrated Scripting Environment (ISE): In your local computer, go toohello **Start** menu.</span></span> <span data-ttu-id="61829-123">Typ **nástroje pro správu** a klikněte na tlačítko toorun ho.</span><span class="sxs-lookup"><span data-stu-id="61829-123">Type **Administrative Tools** and click toorun it.</span></span> <span data-ttu-id="61829-124">V hello **nástroje pro správu** okna, klikněte pravým tlačítkem na **Windows PowerShell ISE**, klikněte na tlačítko **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="61829-124">In hello **Administrative Tools** window, right-click **Windows PowerShell ISE**, click **Run as Administrator**.</span></span>
3. <span data-ttu-id="61829-125">V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** toocreate nový soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="61829-125">In **Windows PowerShell ISE**, click **File** > **New** toocreate a new script file.</span></span>
4. <span data-ttu-id="61829-126">Nyní sdělíme vám jednoduchý skript, který zobrazuje základní tooaccess příkazy prostředí PowerShell Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-126">Now, we'll give you a simple script that shows basic PowerShell commands tooaccess Azure Storage.</span></span> <span data-ttu-id="61829-127">skript Hello požádá vaší tooadd přihlašovací údaje účtu Azure nejprve váš účet Azure toohello místní prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-127">hello script will first ask your Azure account credentials tooadd your Azure account toohello local PowerShell environment.</span></span> <span data-ttu-id="61829-128">Potom hello skript nastavit výchozí hello předplatného Azure a vytvořte nový účet úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-128">Then, hello script will set hello default Azure subscription and create a new storage account in Azure.</span></span> <span data-ttu-id="61829-129">V dalším kroku hello skript vytvořit nový kontejner v rámci tohoto nového účtu úložiště a nahrajte existující kontejner toothat bitové kopie souboru (binární rozsáhlý objekt).</span><span class="sxs-lookup"><span data-stu-id="61829-129">Next, hello script will create a new container in this new storage account and upload an existing image file (blob) toothat container.</span></span> <span data-ttu-id="61829-130">Po hello skript obsahuje seznam všech objektů BLOB v kontejneru, vytvoří nový cílový adresář v místním počítači a stáhnout soubor bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="61829-130">After hello script lists all blobs in that container, it will create a new destination directory in your local computer and download hello image file.</span></span>
5. <span data-ttu-id="61829-131">V následující části kódu hello, vyberte skript hello mezi hello poznámky **#begin** a **#end**.</span><span class="sxs-lookup"><span data-stu-id="61829-131">In hello following code section, select hello script between hello remarks **#begin** and **#end**.</span></span> <span data-ttu-id="61829-132">Stiskněte kombinaci kláves CTRL + C toocopy ho toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="61829-132">Press CTRL+C toocopy it toohello clipboard.</span></span>

    ```powershell
    # begin
    # Update with hello name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name tooyour new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name tooyour new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account toohello local PowerShell environment.
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
       
    # Download blobs from hello container:
    # Get a reference tooa list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create hello destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into hello local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. <span data-ttu-id="61829-133">V **Windows PowerShell ISE**, stiskněte kombinaci kláves CTRL + V toocopy hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="61829-133">In **Windows PowerShell ISE**, press CTRL+V toocopy hello script.</span></span> <span data-ttu-id="61829-134">Klikněte na tlačítko **soubor** > **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="61829-134">Click **File** > **Save**.</span></span> <span data-ttu-id="61829-135">V hello **uložit jako** dialogového okna, název typu hello hello souboru skriptu, jako je například "mystoragescript."</span><span class="sxs-lookup"><span data-stu-id="61829-135">In hello **Save As** dialog window, type hello name of hello script file, such as "mystoragescript."</span></span> <span data-ttu-id="61829-136">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="61829-136">Click **Save**.</span></span>
7. <span data-ttu-id="61829-137">Nyní musíte tooupdate hello skriptu proměnné na základě svého nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="61829-137">Now, you need tooupdate hello script variables based on your configuration settings.</span></span> <span data-ttu-id="61829-138">Je nutné aktualizovat hello **$SubscriptionName** proměnné pomocí svého vlastního předplatného.</span><span class="sxs-lookup"><span data-stu-id="61829-138">You must update hello **$SubscriptionName** variable with your own subscription.</span></span> <span data-ttu-id="61829-139">Můžete ponechat hello jiné proměnné zadané v hello skriptu nebo je aktualizovat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="61829-139">You can keep hello other variables as specified in hello script or update them as you wish.</span></span>
   
   * <span data-ttu-id="61829-140">**$SubscriptionName:** musíte aktualizovat tuto proměnnou s vlastní název odběru.</span><span class="sxs-lookup"><span data-stu-id="61829-140">**$SubscriptionName:** You must update this variable with your own subscription name.</span></span> <span data-ttu-id="61829-141">Postupujte podle jednoho z hello následující tři způsoby toolocate hello název předplatného:</span><span class="sxs-lookup"><span data-stu-id="61829-141">Follow one of hello following three ways toolocate hello name of your subscription:</span></span>
     
    <span data-ttu-id="61829-142">a.</span><span class="sxs-lookup"><span data-stu-id="61829-142">a.</span></span> <span data-ttu-id="61829-143">V **Windows PowerShell ISE**, klikněte na tlačítko **soubor** > **nový** toocreate nový soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="61829-143">In **Windows PowerShell ISE**, click **File** > **New** toocreate a new script file.</span></span> <span data-ttu-id="61829-144">Kopírování hello následující skript toohello nový soubor skriptu a klikněte na tlačítko **ladění** > **spustit**.</span><span class="sxs-lookup"><span data-stu-id="61829-144">Copy hello following script toohello new script file and click **Debug** > **Run**.</span></span> <span data-ttu-id="61829-145">Hello následující skript se nejdřív požádat vašeho tooadd přihlašovací údaje účtu Azure váš účet Azure toohello místní prostředí PowerShell a pak zobrazíte všechny hello odběry, které jsou připojené toohello místní relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-145">hello following script will first ask your Azure account credentials tooadd your Azure account toohello local PowerShell environment and then show all hello subscriptions that are connected toohello local PowerShell session.</span></span> <span data-ttu-id="61829-146">Poznamenejte si název hello hello předplatného, které chcete toouse během provádění v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="61829-146">Take a note of hello name of hello subscription that you want toouse while following this tutorial:</span></span>
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    <span data-ttu-id="61829-147">b.</span><span class="sxs-lookup"><span data-stu-id="61829-147">b.</span></span> <span data-ttu-id="61829-148">toolocate a zkopírujte vaše předplatné názvu hello [portál Azure](https://portal.azure.com), v nabídce centra v levé hello hello, klikněte na **odběry**.</span><span class="sxs-lookup"><span data-stu-id="61829-148">toolocate and copy your subscription name in hello [Azure portal](https://portal.azure.com), in hello Hub menu on hello left, click **Subscriptions**.</span></span> <span data-ttu-id="61829-149">Zkopírujte hello název odběru, které chcete toouse při spouštění skriptů hello v této příručce.</span><span class="sxs-lookup"><span data-stu-id="61829-149">Copy hello name of subscription that you want toouse while running hello scripts in this guide.</span></span>
     
     ![portál Azure](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    <span data-ttu-id="61829-151">c.</span><span class="sxs-lookup"><span data-stu-id="61829-151">c.</span></span> <span data-ttu-id="61829-152">toolocate a zkopírujte vaše předplatné názvu hello [portálu Azure Classic](https://manage.windowsazure.com/), posuňte se dolů a klikněte na **nastavení** na levé straně hello portálu hello.</span><span class="sxs-lookup"><span data-stu-id="61829-152">toolocate and copy your subscription name in hello [Azure Classic Portal](https://manage.windowsazure.com/), scroll down and click **Settings** on hello left side of hello portal.</span></span> <span data-ttu-id="61829-153">Klikněte na tlačítko **odběry** toosee seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="61829-153">Click **Subscriptions** toosee a list of your subscriptions.</span></span> <span data-ttu-id="61829-154">Zkopírujte hello název odběru, který chcete toouse při spouštění skriptů hello zadané v této příručce.</span><span class="sxs-lookup"><span data-stu-id="61829-154">Copy hello name of subscription that you want toouse while running hello scripts given in this guide.</span></span>
     
     ![portál Azure Classic](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * <span data-ttu-id="61829-156">**$StorageAccountName:** použít hello zadané ve skriptu hello název nebo zadejte nový název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-156">**$StorageAccountName:** Use hello given name in hello script or enter a new name for your storage account.</span></span> <span data-ttu-id="61829-157">**Důležité:** hello název účtu úložiště hello musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-157">**Important:** hello name of hello storage account must be unique in Azure.</span></span> <span data-ttu-id="61829-158">Musí být malými písmeny, příliš!</span><span class="sxs-lookup"><span data-stu-id="61829-158">It must be lowercase, too!</span></span>
   * <span data-ttu-id="61829-159">**$Location:** použít zadané "Západní USA" hello ve skriptu hello nebo zvolte jiné umístění Azure, jako například Východ USA, Severní Evropa a tak dále.</span><span class="sxs-lookup"><span data-stu-id="61829-159">**$Location:** Use hello given "West US" in hello script or choose other Azure locations, such as East US, North Europe, and so on.</span></span>
   * <span data-ttu-id="61829-160">**$ContainerName:** použít hello zadané ve skriptu hello název nebo zadejte nový název pro váš kontejner.</span><span class="sxs-lookup"><span data-stu-id="61829-160">**$ContainerName:** Use hello given name in hello script or enter a new name for your container.</span></span>
   * <span data-ttu-id="61829-161">**$ImageToUpload:** zadejte obrázku tooa cestu v místním počítači, jako například: "C:\Images\HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="61829-161">**$ImageToUpload:** Enter a path tooa picture on your local computer, such as: "C:\Images\HelloWorld.png".</span></span>
   * <span data-ttu-id="61829-162">**$DestinationFolder:** Enter toostore souborů cesta tooa místního adresáře stáhli ze služby Azure Storage, například: "C:\DownloadImages".</span><span class="sxs-lookup"><span data-stu-id="61829-162">**$DestinationFolder:** Enter a path tooa local directory toostore files downloaded from Azure Storage, such as: "C:\DownloadImages".</span></span>
8. <span data-ttu-id="61829-163">Po aktualizaci proměnné hello skript v souboru "mystoragescript.ps1" hello, klikněte na tlačítko **soubor** > **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="61829-163">After updating hello script variables in hello "mystoragescript.ps1" file, click **File** > **Save**.</span></span> <span data-ttu-id="61829-164">Potom klikněte na **ladění** > **spustit** nebo stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="61829-164">Then, click **Debug** > **Run** or press **F5** toorun hello script.</span></span>  

<span data-ttu-id="61829-165">Po spuštění skriptu hello byste měli mít místní cílovou složku, která zahrnuje hello stáhnout soubor bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="61829-165">After hello script runs, you should have a local destination folder that includes hello downloaded image file.</span></span> <span data-ttu-id="61829-166">Hello následující snímek obrazovky ukazuje příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="61829-166">hello following screenshot shows an example output:</span></span>

![Stáhnout objekty BLOB](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> <span data-ttu-id="61829-168">Hello "Začínáme s Azure Storage a prostředí PowerShell během 5 minut" části poskytuje rychlý úvod o toouse prostředí Azure PowerShell s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-168">hello "Getting started with Azure Storage and PowerShell in 5 minutes" section provided a quick introduction on how toouse Azure PowerShell with Azure Storage.</span></span> <span data-ttu-id="61829-169">Podrobné informace a pokyny doporučujeme vám tooread hello následující části.</span><span class="sxs-lookup"><span data-stu-id="61829-169">For detailed information and instructions, we encourage you tooread hello following sections.</span></span>
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a><span data-ttu-id="61829-170">Požadavky pro použití prostředí Azure PowerShell s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="61829-170">Prerequisites for using Azure PowerShell with Azure Storage</span></span>
<span data-ttu-id="61829-171">Je nutné předplatné a účet toorun hello rutiny Azure PowerShell uvedených v této příručce, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="61829-171">You need an Azure subscription and account toorun hello PowerShell cmdlets given in this guide, as described above.</span></span>

<span data-ttu-id="61829-172">Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-172">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="61829-173">Informace o instalaci a nastavení prostředí Azure PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="61829-173">For information on installing and setting up Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="61829-174">Doporučujeme stáhnout a nainstalovat nebo upgradovat nejnovější modul Azure PowerShell toohello před použitím tohoto průvodce.</span><span class="sxs-lookup"><span data-stu-id="61829-174">We recommend that you download and install or upgrade toohello latest Azure PowerShell module before using this guide.</span></span>

<span data-ttu-id="61829-175">Spuštěním rutiny hello v konzole Windows PowerShell standardní hello nebo hello Windows PowerShell Integrované skriptovací prostředí (ISE).</span><span class="sxs-lookup"><span data-stu-id="61829-175">You can run hello cmdlets in hello standard Windows PowerShell console or hello Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="61829-176">Například tooopen **Windows PowerShell ISE**přejděte toohello nabídky Start, nástroje pro správu a klikněte na toorun ho.</span><span class="sxs-lookup"><span data-stu-id="61829-176">For example, tooopen **Windows PowerShell ISE**, go toohello Start menu, type Administrative Tools and click toorun it.</span></span> <span data-ttu-id="61829-177">V okně hello nástroje pro správu klikněte pravým tlačítkem na Windows PowerShell ISE, klikněte na tlačítko Spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="61829-177">In hello Administrative Tools window, right-click Windows PowerShell ISE, click Run as Administrator.</span></span>

## <a name="how-toomanage-storage-accounts-in-azure"></a><span data-ttu-id="61829-178">Jak účtů toomanage úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="61829-178">How toomanage storage accounts in Azure</span></span>

<span data-ttu-id="61829-179">Podívejme se na správu účty úložiště v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="61829-179">Let's take a look at managing storage accounts in Azure with PowerShell</span></span>

### <a name="how-tooset-a-default-azure-subscription"></a><span data-ttu-id="61829-180">Jak tooset výchozí předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="61829-180">How tooset a default Azure subscription</span></span>
<span data-ttu-id="61829-181">toomanage Azure Storage pomocí Azure PowerShell, je nutné tooauthenticate prostředí klienta s nástrojem Azure přes Azure Active Directory ověřování nebo ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="61829-181">toomanage Azure Storage using Azure PowerShell, you need tooauthenticate your client environment with Azure via Azure Active Directory authentication or certificate-based authentication.</span></span> <span data-ttu-id="61829-182">Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) kurzu.</span><span class="sxs-lookup"><span data-stu-id="61829-182">For detailed information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tutorial.</span></span> <span data-ttu-id="61829-183">Tento průvodce pomocí ověřování Azure Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="61829-183">This guide uses hello Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="61829-184">V systému Windows PowerShell ISE zadejte následující příkaz tooadd hello váš účet Azure toohello místní prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="61829-184">In Windows PowerShell ISE, type hello following command tooadd your Azure account toohello local PowerShell environment:</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="61829-185">V okně "Přihlásit tooMicrosoft Azure" hello, typ hello e-mailovou adresu a heslo spojené s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="61829-185">In hello "Sign in tooMicrosoft Azure" window, type hello email address and password associated with your account.</span></span> <span data-ttu-id="61829-186">Azure ověřuje a uloží hello přihlašovací údaje a potom zavře okno hello.</span><span class="sxs-lookup"><span data-stu-id="61829-186">Azure authenticates and saves hello credential information, and then closes hello window.</span></span>

3. <span data-ttu-id="61829-187">Potom spustíte následující příkaz tooview hello Azure účty ve vašem místním prostředí PowerShell a zkontrolujte, zda váš účet hello:</span><span class="sxs-lookup"><span data-stu-id="61829-187">Next, run hello following command tooview hello Azure accounts in your local PowerShell environment, and verify that your account is listed:</span></span>
   
    ```powershell
    Get-AzureAccount
    ```
4. <span data-ttu-id="61829-188">Potom spusťte následující rutiny tooview hello všechny hello odběry, které jsou připojené toohello místní relaci prostředí PowerShell a zkontrolujte, zda vaše předplatné:</span><span class="sxs-lookup"><span data-stu-id="61829-188">Then, run hello following cmdlet tooview all hello subscriptions that are connected toohello local PowerShell session, and verify that your subscription is listed:</span></span>

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. <span data-ttu-id="61829-189">tooset výchozí předplatné Azure, spusťte rutinu hello Select-AzureSubscription:</span><span class="sxs-lookup"><span data-stu-id="61829-189">tooset a default Azure subscription, run hello Select-AzureSubscription cmdlet:</span></span>

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. <span data-ttu-id="61829-190">Ověřte název hello hello výchozí předplatné spuštěním rutiny Get-AzureSubscription hello:</span><span class="sxs-lookup"><span data-stu-id="61829-190">Verify hello name of hello default subscription by running hello Get-AzureSubscription cmdlet:</span></span>

    ```powershell
    Get-AzureSubscription -Default
    ```

7. <span data-ttu-id="61829-191">toosee všechny hello k dispozici rutiny prostředí PowerShell pro Azure Storage, spusťte:</span><span class="sxs-lookup"><span data-stu-id="61829-191">toosee all hello available PowerShell cmdlets for Azure Storage, run:</span></span>
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-toocreate-a-new-azure-storage-account"></a><span data-ttu-id="61829-192">Jak toocreate nový účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="61829-192">How toocreate a new Azure storage account</span></span>
<span data-ttu-id="61829-193">toouse úložiště Azure, budete potřebovat účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-193">toouse Azure storage, you will need a storage account.</span></span> <span data-ttu-id="61829-194">Po nakonfigurování vaše počítače tooconnect tooyour předplatné, můžete vytvořit nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-194">You can create a new Azure storage account after you have configured your computer tooconnect tooyour subscription.</span></span>

1. <span data-ttu-id="61829-195">Všechny dostupné datacenter umístění hello spouštění toofind rutiny Get-AzureLocation hello:</span><span class="sxs-lookup"><span data-stu-id="61829-195">Run hello Get-AzureLocation cmdlet toofind all hello available datacenter locations:</span></span>

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. <span data-ttu-id="61829-196">Dále proveďte toocreate rutiny New-AzureStorageAccount hello nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-196">Next, run hello New-AzureStorageAccount cmdlet toocreate a new storage account.</span></span> <span data-ttu-id="61829-197">Hello následující příklad vytvoří nový účet úložiště v datovém centru hello "Západní USA".</span><span class="sxs-lookup"><span data-stu-id="61829-197">hello following example creates a new storage account in hello "West US" datacenter.</span></span>
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> <span data-ttu-id="61829-198">Hello název svého účtu úložiště musí být jedinečný v rámci Azure a musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="61829-198">hello name of your storage account must be unique within Azure and must be lowercase.</span></span> <span data-ttu-id="61829-199">Zásady vytváření názvů a omezení najdete v tématu [o účtech úložiště Azure](storage-create-storage-account.md) a [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-199">For naming conventions and restrictions, see [About Azure Storage Accounts](storage-create-storage-account.md) and [Naming and Referencing Containers, Blobs, and Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span></span>
> 
> 

### <a name="how-tooset-a-default-azure-storage-account"></a><span data-ttu-id="61829-200">Jak tooset výchozí účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="61829-200">How tooset a default Azure storage account</span></span>
<span data-ttu-id="61829-201">Můžete mít více účtů úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="61829-201">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="61829-202">Můžete zvolit jeden z nich a nastavte ji jako hello výchozí účet úložiště pro všechny hello úložiště příkazů v hello stejné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-202">You can choose one of them and set it as hello default storage account for all hello storage commands in hello same PowerShell session.</span></span> <span data-ttu-id="61829-203">To umožňuje toorun hello prostředí Azure PowerShell úložiště příkazy bez zadání kontext úložiště hello explicitně.</span><span class="sxs-lookup"><span data-stu-id="61829-203">This enables you toorun hello Azure PowerShell storage commands without specifying hello storage context explicitly.</span></span>

1. <span data-ttu-id="61829-204">tooset výchozí účet úložiště pro vaše předplatné, můžete spustit rutinu Set-AzureSubscription hello.</span><span class="sxs-lookup"><span data-stu-id="61829-204">tooset a default storage account for your subscription, you can run hello Set-AzureSubscription cmdlet.</span></span>

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. <span data-ttu-id="61829-205">Dále proveďte tooverify rutiny Get-AzureSubscription hello, je přiřazená k vašemu účtu předplatného výchozí účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="61829-205">Next, run hello Get-AzureSubscription cmdlet tooverify that hello storage account is associated with your default subscription account.</span></span> <span data-ttu-id="61829-206">Tento příkaz vrátí hello vlastnosti odběru na hello aktuální předplatného, včetně jeho aktuální účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-206">This command returns hello subscription properties on hello current subscription including its current storage account.</span></span>

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-toolist-all-azure-storage-accounts-in-a-subscription"></a><span data-ttu-id="61829-207">Jak toolist všechny Azure účtů úložiště v předplatném.</span><span class="sxs-lookup"><span data-stu-id="61829-207">How toolist all Azure storage accounts in a subscription</span></span>
<span data-ttu-id="61829-208">Každé předplatné Azure může mít too100 úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="61829-208">Each Azure subscription can have up too100 storage accounts.</span></span> <span data-ttu-id="61829-209">Hello nejaktuálnější informace o limitech najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="61829-209">For hello most up-to-date information on limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="61829-210">Spusťte následující rutinu toofind hello název a stav hello účty úložiště v aktuálním předplatném hello hello:</span><span class="sxs-lookup"><span data-stu-id="61829-210">Run hello following cmdlet toofind out hello name and status of hello storage accounts in hello current subscription:</span></span>

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-toocreate-an-azure-storage-context"></a><span data-ttu-id="61829-211">Jak toocreate kontextu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="61829-211">How toocreate an Azure storage context</span></span>
<span data-ttu-id="61829-212">Kontext úložiště Azure je objekt v prostředí PowerShell tooencapsulate hello úložiště pověření.</span><span class="sxs-lookup"><span data-stu-id="61829-212">Azure storage context is an object in PowerShell tooencapsulate hello storage credentials.</span></span> <span data-ttu-id="61829-213">Použití úložiště kontextu při spuštění libovolné další rutiny umožňuje vám tooauthenticate vaši žádost o bez zadání hello účet úložiště a jeho přístupový klíč explicitně.</span><span class="sxs-lookup"><span data-stu-id="61829-213">Using a storage context while running any subsequent cmdlet enables you tooauthenticate your request without specifying hello storage account and its access key explicitly.</span></span> <span data-ttu-id="61829-214">Kontext úložiště můžete vytvořit v mnoha způsoby, například pomocí názvu a přístupový klíč účtu úložiště, sdílený přístupový podpis (SAS) token, připojovací řetězec, nebo anonymní.</span><span class="sxs-lookup"><span data-stu-id="61829-214">You can create a storage context in many ways, such as using storage account name and access key, shared access signature (SAS) token, connection string, or anonymous.</span></span> <span data-ttu-id="61829-215">Další informace najdete v tématu [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span><span class="sxs-lookup"><span data-stu-id="61829-215">For more information, see [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span></span>  

<span data-ttu-id="61829-216">Použijte jednu z následujících tří způsobů toocreate hello kontext úložiště:</span><span class="sxs-lookup"><span data-stu-id="61829-216">Use one of hello following three ways toocreate a storage context:</span></span>

* <span data-ttu-id="61829-217">Spustit hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) toofind rutiny se hello primárního úložiště přístupový klíč pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-217">Run hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet toofind out hello primary storage access key for your Azure storage account.</span></span> <span data-ttu-id="61829-218">Pak zavolejte hello [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) toocreate rutiny kontext úložiště:</span><span class="sxs-lookup"><span data-stu-id="61829-218">Next, call hello [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet toocreate a storage context:</span></span>

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* <span data-ttu-id="61829-219">Vygenerování tokenu sdíleného přístupového podpisu pro kontejner úložiště Azure a použít ho toocreate kontext úložiště:</span><span class="sxs-lookup"><span data-stu-id="61829-219">Generate a shared access signature token for an Azure storage container and use it toocreate a storage context:</span></span>

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    <span data-ttu-id="61829-220">Další informace najdete v tématu [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) a [pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="61829-220">For more information, see [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) and [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span></span>

* <span data-ttu-id="61829-221">V některých případech můžete koncové body služby hello toospecify, když vytvoříte nový kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-221">In some cases, you may want toospecify hello service endpoints when you create a new storage context.</span></span> <span data-ttu-id="61829-222">To může být nezbytné, pokud jste registrovali vlastního názvu domény pro váš účet úložiště s hello služby objektů Blob nebo chcete toouse sdílený přístupový podpis pro přístup k prostředkům úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-222">This might be necessary when you have registered a custom domain name for your storage account with hello Blob service or you want toouse a shared access signature for accessing storage resources.</span></span> <span data-ttu-id="61829-223">Nastavit koncové body služby hello v připojovacím řetězci a použít ho toocreate nový kontext úložiště, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="61829-223">Set hello service endpoints in a connection string and use it toocreate a new storage context as shown below:</span></span>

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

<span data-ttu-id="61829-224">Další informace o tom, najdete v části tooconfigure připojovacího řetězce úložiště [konfiguraci připojovacích řetězců](storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="61829-224">For more information on how tooconfigure a storage connection string, see [Configuring Connection Strings](storage-configure-connection-string.md).</span></span>

<span data-ttu-id="61829-225">Teď, když máte v počítači a se dozvěděli, jak toomanage předplatných a účtů úložiště pomocí prostředí Azure PowerShell přejděte toohello další části toolearn jak objekty BLOB toomanage Azure a objekt blob snímky.</span><span class="sxs-lookup"><span data-stu-id="61829-225">Now that you have set up your computer and learned how toomanage subscriptions and storage accounts using Azure PowerShell, go toohello next section toolearn how toomanage Azure blobs and blob snapshots.</span></span>

### <a name="how-tooretrieve-and-regenerate-azure-storage-keys"></a><span data-ttu-id="61829-226">Jak tooretrieve a znovu vygenerovat klíče úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="61829-226">How tooretrieve and regenerate Azure storage keys</span></span>
<span data-ttu-id="61829-227">Účet úložiště Azure obsahuje dva klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="61829-227">An Azure Storage account comes with two account keys.</span></span> <span data-ttu-id="61829-228">Můžete použít následující rutiny tooretrieve hello klíče.</span><span class="sxs-lookup"><span data-stu-id="61829-228">You can use hello following cmdlet tooretrieve your keys.</span></span>

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

<span data-ttu-id="61829-229">Pomocí následující rutiny tooretrieve konkrétního klíče hello.</span><span class="sxs-lookup"><span data-stu-id="61829-229">Use hello following cmdlet tooretrieve a specific key.</span></span> <span data-ttu-id="61829-230">Platné hodnoty jsou primární i sekundární.</span><span class="sxs-lookup"><span data-stu-id="61829-230">Valid values are Primary and Secondary.</span></span>  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

<span data-ttu-id="61829-231">Pokud chcete tooregenerate klíče, použijte následující rutinu hello.</span><span class="sxs-lookup"><span data-stu-id="61829-231">If you would like tooregenerate your keys, use hello following cmdlet.</span></span> <span data-ttu-id="61829-232">Typ_klíče - platné hodnoty jsou "Primární" a "Sekundární"</span><span class="sxs-lookup"><span data-stu-id="61829-232">Valid values for -KeyType are "Primary" and "Secondary"</span></span>

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-toomanage-azure-blobs"></a><span data-ttu-id="61829-233">Jak objekty BLOB toomanage Azure</span><span class="sxs-lookup"><span data-stu-id="61829-233">How toomanage Azure blobs</span></span>
<span data-ttu-id="61829-234">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61829-234">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="61829-235">V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště objektů Blob Azure hello.</span><span class="sxs-lookup"><span data-stu-id="61829-235">This section assumes that you are already familiar with hello Azure Blob Storage Service concepts.</span></span> <span data-ttu-id="61829-236">Podrobné informace najdete v tématu [Začínáme s úložištěm Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-236">For detailed information, see [Get started with Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="how-toocreate-a-container"></a><span data-ttu-id="61829-237">Jak toocreate kontejner</span><span class="sxs-lookup"><span data-stu-id="61829-237">How toocreate a container</span></span>
<span data-ttu-id="61829-238">Každý objekt blob v úložišti Azure musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="61829-238">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="61829-239">Můžete vytvořit kontejner privátní, pomocí rutiny New-AzureStorageContainer hello:</span><span class="sxs-lookup"><span data-stu-id="61829-239">You can create a private container using hello New-AzureStorageContainer cmdlet:</span></span>

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> <span data-ttu-id="61829-240">Existují tři úrovně anonymní přístup pro čtení: **vypnout**, **Blob**, a **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="61829-240">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="61829-241">tooprevent anonymní přístup k tooblobs parametru oprávnění sady hello příliš**vypnout**.</span><span class="sxs-lookup"><span data-stu-id="61829-241">tooprevent anonymous access tooblobs, set hello Permission parameter too**Off**.</span></span> <span data-ttu-id="61829-242">Ve výchozím nastavení nový kontejner hello je privátní a je přístupný jenom vlastník účtu hello.</span><span class="sxs-lookup"><span data-stu-id="61829-242">By default, hello new container is private and can be accessed only by hello account owner.</span></span> <span data-ttu-id="61829-243">čtení tooallow anonymní veřejný přístup k prostředkům tooblob, ale není toocontainer metadata nebo toohello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**Blob**.</span><span class="sxs-lookup"><span data-stu-id="61829-243">tooallow anonymous public read access tooblob resources, but not toocontainer metadata or toohello list of blobs in hello container, set hello Permission parameter too**Blob**.</span></span> <span data-ttu-id="61829-244">čtení tooallow úplné veřejný přístup k prostředkům tooblob, metadata kontejneru a hello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="61829-244">tooallow full public read access tooblob resources, container metadata, and hello list of blobs in hello container, set hello Permission parameter too**Container**.</span></span> <span data-ttu-id="61829-245">Další informace najdete v tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="61829-245">For more information, see [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md).</span></span>
> 
> 

### <a name="how-tooupload-a-blob-into-a-container"></a><span data-ttu-id="61829-246">Jak tooupload objekt blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="61829-246">How tooupload a blob into a container</span></span>
<span data-ttu-id="61829-247">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="61829-247">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="61829-248">Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-248">For more information, see [Understanding Block Blobs, Append BLobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="61829-249">tooupload objekty BLOB v kontejneru tooa, můžete použít hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-249">tooupload blobs in tooa container, you can use hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span></span> <span data-ttu-id="61829-250">Ve výchozím nastavení odešle tento příkaz hello objekt blob bloku tooa místní soubory.</span><span class="sxs-lookup"><span data-stu-id="61829-250">By default, this command uploads hello local files tooa block blob.</span></span> <span data-ttu-id="61829-251">Typ hello toospecify pro hello blob, můžete pomocí parametru - BlobType hello.</span><span class="sxs-lookup"><span data-stu-id="61829-251">toospecify hello type for hello blob, you can use hello -BlobType parameter.</span></span>

<span data-ttu-id="61829-252">Vítejte v následujícím příkladu spustí hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) tooget rutiny hello všechny soubory v zadané složce hello a pak předá je další rutiny toohello pomocí operátor kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="61829-252">hello following example runs hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet tooget all hello files in hello specified folder, and then passes them toohello next cmdlet by using hello pipeline operator.</span></span> <span data-ttu-id="61829-253">Hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) rutiny odešle hello místních souborů tooyour kontejneru:</span><span class="sxs-lookup"><span data-stu-id="61829-253">hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet uploads hello local files tooyour container:</span></span>

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-toodownload-blobs-from-a-container"></a><span data-ttu-id="61829-254">Jak toodownload objektů blob z kontejneru</span><span class="sxs-lookup"><span data-stu-id="61829-254">How toodownload blobs from a container</span></span>
<span data-ttu-id="61829-255">Hello následující příklad ukazuje, jak z kontejneru objektů BLOB toodownload.</span><span class="sxs-lookup"><span data-stu-id="61829-255">hello following example demonstrates how toodownload blobs from a container.</span></span> <span data-ttu-id="61829-256">Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho primární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="61829-256">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its primary access key.</span></span> <span data-ttu-id="61829-257">Potom hello příklad načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-257">Then, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span></span> <span data-ttu-id="61829-258">V dalším kroku hello příklad používá hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) objekty BLOB toodownload rutiny do hello místní cílovou složku.</span><span class="sxs-lookup"><span data-stu-id="61829-258">Next, hello example uses hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet toodownload blobs into hello local destination folder.</span></span>

```powershell
#Define hello variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-toocopy-blobs-from-one-storage-container-tooanother"></a><span data-ttu-id="61829-259">Jak toocopy objektů blob z jednoho tooanother kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="61829-259">How toocopy blobs from one storage container tooanother</span></span>
<span data-ttu-id="61829-260">Mezi účty úložiště a oblastí, můžete zkopírovat objekty BLOB asynchronně.</span><span class="sxs-lookup"><span data-stu-id="61829-260">You can copy blobs across storage accounts and regions asynchronously.</span></span> <span data-ttu-id="61829-261">Hello následující příklad ukazuje, jak toocopy objektů blob z jednoho úložiště kontejneru tooanother ve dvou jiným účtům úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-261">hello following example demonstrates how toocopy blobs from one storage container tooanother in two different storage accounts.</span></span> <span data-ttu-id="61829-262">Příklad Hello nejprve nastaví proměnné pro zdrojové a cílové účty úložiště a potom vytvoří kontext úložiště pro každý účet.</span><span class="sxs-lookup"><span data-stu-id="61829-262">hello example first sets variables for source and destination storage accounts, and then creates a storage context for each account.</span></span> <span data-ttu-id="61829-263">V dalším kroku hello příkladu se zkopíruje objekty BLOB z hello zdrojový kontejner toohello cílový kontejner pomocí hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-263">Next, hello example copies blobs from hello source container toohello destination container using hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span></span> <span data-ttu-id="61829-264">Hello příklad předpokládá, že účty hello zdrojového a cílového úložiště a kontejnerů, již existuje.</span><span class="sxs-lookup"><span data-stu-id="61829-264">hello example assumes that hello source and destination storage accounts and containers already exist.</span></span>

```powershell
#Define hello source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define hello destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference tooblobs in hello source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container tooanother.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

<span data-ttu-id="61829-265">Pamatujte, že v tomto příkladu asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="61829-265">Note that this example performs an asynchronous copy.</span></span> <span data-ttu-id="61829-266">Můžete sledovat stav hello každé kopie spuštěním hello [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-266">You can monitor hello status of each copy by running hello [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span></span>

### <a name="how-toocopy-blobs-from-a-secondary-location"></a><span data-ttu-id="61829-267">Jak toocopy objekty BLOB ze sekundární lokality</span><span class="sxs-lookup"><span data-stu-id="61829-267">How toocopy blobs from a secondary location</span></span>
<span data-ttu-id="61829-268">Objekty BLOB můžete zkopírovat z hello sekundárního umístění účtu povoleno RA-GRS.</span><span class="sxs-lookup"><span data-stu-id="61829-268">You can copy blobs from hello secondary location of a RA-GRS-enabled account.</span></span>

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-toodelete-a-blob"></a><span data-ttu-id="61829-269">Jak toodelete objektu blob</span><span class="sxs-lookup"><span data-stu-id="61829-269">How toodelete a blob</span></span>
<span data-ttu-id="61829-270">toodelete objekt blob, nejdřív získejte odkaz na objekt blob a potom na něm volat rutinu Remove-AzureStorageBlob hello.</span><span class="sxs-lookup"><span data-stu-id="61829-270">toodelete a blob, first get a blob reference and then call hello Remove-AzureStorageBlob cmdlet on it.</span></span> <span data-ttu-id="61829-271">Následující ukázka Hello odstraní všechny objekty BLOB hello v daném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="61829-271">hello following example deletes all hello blobs in a given container.</span></span> <span data-ttu-id="61829-272">Příklad Hello nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-272">hello example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="61829-273">V dalším kroku hello příklad načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí hello [odebrat AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) objekty BLOB tooremove rutiny z kontejneru v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-273">Next, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet tooremove blobs from a container in Azure storage.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooall hello blobs in hello container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-toomanage-azure-blob-snapshots"></a><span data-ttu-id="61829-274">Jak toomanage Azure blob snímky</span><span class="sxs-lookup"><span data-stu-id="61829-274">How toomanage Azure blob snapshots</span></span>
<span data-ttu-id="61829-275">Azure umožňuje vytvoření snímku objektu blob.</span><span class="sxs-lookup"><span data-stu-id="61829-275">Azure lets you create a snapshot of a blob.</span></span> <span data-ttu-id="61829-276">Snímek je jen pro čtení verze objektu blob, který se pořídí na bod v čase.</span><span class="sxs-lookup"><span data-stu-id="61829-276">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="61829-277">Po vytvoření snímku, ho můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně.</span><span class="sxs-lookup"><span data-stu-id="61829-277">Once a snapshot has been created, it can be read, copied, or deleted, but not modified.</span></span> <span data-ttu-id="61829-278">Snímky zadejte tooback způsob, jak se objekt blob, jak se objevuje v časovém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="61829-278">Snapshots provide a way tooback up a blob as it appears at a moment in time.</span></span> <span data-ttu-id="61829-279">Další informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-279">For more information, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span>

### <a name="how-toocreate-a-blob-snapshot"></a><span data-ttu-id="61829-280">Jak toocreate snímku objektů blob</span><span class="sxs-lookup"><span data-stu-id="61829-280">How toocreate a blob snapshot</span></span>
<span data-ttu-id="61829-281">toocreate snímek objektu blob, nejdřív získejte odkaz na objekt blob a pak zavolají hello `ICloudBlob.CreateSnapshot` metoda na něm.</span><span class="sxs-lookup"><span data-stu-id="61829-281">toocreate a snaphot of a blob, first get a blob reference and then call hello `ICloudBlob.CreateSnapshot` method on it.</span></span> <span data-ttu-id="61829-282">Hello následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-282">hello following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="61829-283">V dalším kroku hello příklad načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoda toocreate snímku.</span><span class="sxs-lookup"><span data-stu-id="61829-283">Next, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method toocreate a snapshot.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of hello blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-toolist-a-blobs-snapshots"></a><span data-ttu-id="61829-284">Jak toolist objekt blob pro snímky</span><span class="sxs-lookup"><span data-stu-id="61829-284">How toolist a blob's snapshots</span></span>
<span data-ttu-id="61829-285">Můžete vytvořit libovolný počet snímků, jak chcete použít pro objekt blob.</span><span class="sxs-lookup"><span data-stu-id="61829-285">You can create as many snapshots as you want for a blob.</span></span> <span data-ttu-id="61829-286">Můžete vytvořit seznam hello snímky přidružené k vaší blob tootrack vaše aktuální snímky.</span><span class="sxs-lookup"><span data-stu-id="61829-286">You can list hello snapshots associated with your blob tootrack your current snapshots.</span></span> <span data-ttu-id="61829-287">Hello následující příklad používá předdefinovaných objektů blob a volání hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny toolist hello snímky tomuto objektu blob.</span><span class="sxs-lookup"><span data-stu-id="61829-287">hello following example uses a predefined blob and calls hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet toolist hello snapshots of that blob.</span></span>  

```powershell
#Define hello blob name.
$BlobName = "yourblobname"

#List hello snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-toocopy-a-snapshot-of-a-blob"></a><span data-ttu-id="61829-288">Jak toocopy snímek objektu blob</span><span class="sxs-lookup"><span data-stu-id="61829-288">How toocopy a snapshot of a blob</span></span>
<span data-ttu-id="61829-289">Můžete zkopírovat snímek snímku hello toorestore objektů blob.</span><span class="sxs-lookup"><span data-stu-id="61829-289">You can copy a snapshot of a blob toorestore hello snapshot.</span></span> <span data-ttu-id="61829-290">Omezení a podrobné informace najdete v tématu [vytvoření snímku objektu Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-290">For detailed information and restrictions, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span> <span data-ttu-id="61829-291">Hello následující příklad nejprve nastaví proměnných pro účet úložiště a poté vytvoří kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-291">hello following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="61829-292">V dalším kroku hello příklad definuje název proměnné hello kontejnerů a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="61829-292">Next, hello example defines hello container and blob name variables.</span></span> <span data-ttu-id="61829-293">Příklad Hello načte odkaz na objekt blob pomocí hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) rutiny a spustí hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoda toocreate snímku.</span><span class="sxs-lookup"><span data-stu-id="61829-293">hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method toocreate a snapshot.</span></span> <span data-ttu-id="61829-294">Potom hello příklad spustí hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) rutiny toocopy hello snímku objektu blob pomocí hello ICloudBlob objektu pro objekt hello zdroje blob.</span><span class="sxs-lookup"><span data-stu-id="61829-294">Then, hello example runs hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet toocopy hello snapshot of a blob using hello ICloudBlob object for hello source blob.</span></span> <span data-ttu-id="61829-295">Být opravdu tooupdate hello proměnné na základě vaší konfigurace před spuštěné příklad hello.</span><span class="sxs-lookup"><span data-stu-id="61829-295">Be sure tooupdate hello variables based on your configuration before running hello example.</span></span> <span data-ttu-id="61829-296">Všimněte si, že hello následující příklad předpokládá, že hello zdrojové a cílové kontejnery a hello zdrojový objekt blob již existují.</span><span class="sxs-lookup"><span data-stu-id="61829-296">Note that hello following example assumes that hello source and destination containers, and hello source blob already exist.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define hello variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy hello snapshot tooanother container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

<span data-ttu-id="61829-297">Teď, když jste se naučili, jak objekty BLOB toomanage Azure a objektů blob snímky s prostředím Azure PowerShell, přejděte toohello další části toolearn jak toomanage tabulky, fronty a soubory.</span><span class="sxs-lookup"><span data-stu-id="61829-297">Now that you have learned how toomanage Azure blobs and blob snapshots with Azure PowerShell, go toohello next section toolearn how toomanage tables, queues, and files.</span></span>

## <a name="how-toomanage-azure-tables-and-table-entities"></a><span data-ttu-id="61829-298">Jak toomanage Azure tabulky a tabulka entity</span><span class="sxs-lookup"><span data-stu-id="61829-298">How toomanage Azure tables and table entities</span></span>
<span data-ttu-id="61829-299">Služby úložiště Azure Table je úložiště dat typu NoSQL, které můžete použít toostore a dotazování obrovských sad strukturovaných, nerelačních data.</span><span class="sxs-lookup"><span data-stu-id="61829-299">Azure Table storage service is a NoSQL datastore, which you can use toostore and query huge sets of structured, non-relational data.</span></span> <span data-ttu-id="61829-300">hlavní součásti Hello hello služby jsou tabulky, entit a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="61829-300">hello main components of hello service are tables, entities, and properties.</span></span> <span data-ttu-id="61829-301">Tabulka je kolekce entit.</span><span class="sxs-lookup"><span data-stu-id="61829-301">A table is a collection of entities.</span></span> <span data-ttu-id="61829-302">Entita je sada vlastností.</span><span class="sxs-lookup"><span data-stu-id="61829-302">An entity is a set of properties.</span></span> <span data-ttu-id="61829-303">Každá entita může mít too252 vlastností, které jsou všechny páry název hodnota.</span><span class="sxs-lookup"><span data-stu-id="61829-303">Each entity can have up too252 properties, which are all name-value pairs.</span></span> <span data-ttu-id="61829-304">V této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště Azure Table hello.</span><span class="sxs-lookup"><span data-stu-id="61829-304">This section assumes that you are already familiar with hello Azure Table Storage Service concepts.</span></span> <span data-ttu-id="61829-305">Podrobné informace najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx) a [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="61829-305">For detailed information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx) and [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="61829-306">V hello následující témata se dozvíte, jak služba toomanage Azure Table storage pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-306">In hello following subsections, you'll learn how toomanage Azure Table storage service using Azure PowerShell.</span></span> <span data-ttu-id="61829-307">Hello pokryté scénáře zahrnují **vytváření**, **odstraňování**, a **načítání** **tabulky**, a také **přidání**, **dotazování**, a **odstranění entity tabulky**.</span><span class="sxs-lookup"><span data-stu-id="61829-307">hello scenarios covered include **creating**, **deleting**, and **retrieving** **tables**, as well as **adding**, **querying**, and **deleting table entities**.</span></span>

### <a name="how-toocreate-a-table"></a><span data-ttu-id="61829-308">Jak toocreate tabulku</span><span class="sxs-lookup"><span data-stu-id="61829-308">How toocreate a table</span></span>
<span data-ttu-id="61829-309">Každá tabulka musí nacházet v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-309">Every table must reside in an Azure storage account.</span></span> <span data-ttu-id="61829-310">Hello následující příklad ukazuje, jak toocreate tabulku ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-310">hello following example demonstrates how toocreate a table in Azure Storage.</span></span> <span data-ttu-id="61829-311">Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="61829-311">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="61829-312">Dále je použita hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutiny toocreate tabulku ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-312">Next, it uses hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet toocreate a table in Azure Storage.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-tooretrieve-a-table"></a><span data-ttu-id="61829-313">Jak tooretrieve tabulku</span><span class="sxs-lookup"><span data-stu-id="61829-313">How tooretrieve a table</span></span>
<span data-ttu-id="61829-314">Můžete zadat dotaz a načtení jedné nebo všech tabulek v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-314">You can query and retrieve one or all tables in a Storage account.</span></span> <span data-ttu-id="61829-315">Hello následující příklad ukazuje, jak tooretrieve dané tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-315">hello following example demonstrates how tooretrieve a given table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span>

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

<span data-ttu-id="61829-316">Při volání rutiny Get-AzureStorageTable hello bez parametrů, získá všechny tabulky úložiště pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-316">If you call hello Get-AzureStorageTable cmdlet without any parameters, it gets all storage tables for a Storage account.</span></span>

### <a name="how-toodelete-a-table"></a><span data-ttu-id="61829-317">Jak toodelete tabulku</span><span class="sxs-lookup"><span data-stu-id="61829-317">How toodelete a table</span></span>
<span data-ttu-id="61829-318">Můžete odstranit tabulku z účtu úložiště pomocí hello [odebrat AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-318">You can delete a table from a storage account by using hello [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span></span>  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-toomanage-table-entities"></a><span data-ttu-id="61829-319">Jak toomanage tabulka entity</span><span class="sxs-lookup"><span data-stu-id="61829-319">How toomanage table entities</span></span>
<span data-ttu-id="61829-320">V současné době prostředí Azure PowerShell neposkytuje entity tabulky toomanage rutiny přímo.</span><span class="sxs-lookup"><span data-stu-id="61829-320">Currently, Azure PowerShell does not provide cmdlets toomanage table entities directly.</span></span> <span data-ttu-id="61829-321">operace tooperform na entity tabulky, můžete použít hello třídy zadaná v hello [Klientská knihovna pro úložiště Azure pro .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-321">tooperform operations on table entities, you can use hello classes given in hello [Azure Storage Client Library for .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span></span>

#### <a name="how-tooadd-table-entities"></a><span data-ttu-id="61829-322">Jak tooadd tabulka entity</span><span class="sxs-lookup"><span data-stu-id="61829-322">How tooadd table entities</span></span>
<span data-ttu-id="61829-323">tooadd tooa tabulka entity, nejprve vytvořit objekt, který definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="61829-323">tooadd an entity tooa table, first create an object that defines your entity properties.</span></span> <span data-ttu-id="61829-324">Entita může mít too255 vlastností, včetně 3 Vlastnosti systému: **PartitionKey**, **RowKey**, a **časové razítko**.</span><span class="sxs-lookup"><span data-stu-id="61829-324">An entity can have up too255 properties, including 3 system properties: **PartitionKey**, **RowKey**, and **Timestamp**.</span></span> <span data-ttu-id="61829-325">Jste zodpovědní za vložení a aktualizace hodnoty hello **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="61829-325">You are responsible for inserting and updating hello values of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="61829-326">Hello server spravuje hello hodnotu **časové razítko**, který nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="61829-326">hello server manages hello value of **Timestamp**, which cannot be modified.</span></span> <span data-ttu-id="61829-327">Společně hello **PartitionKey** a **RowKey** jednoznačně identifikovat každou entitu v tabulce.</span><span class="sxs-lookup"><span data-stu-id="61829-327">Together hello **PartitionKey** and **RowKey** uniquely identify every entity within a table.</span></span>

* <span data-ttu-id="61829-328">**PartitionKey**: Určuje hello oddíl, který hello entity je uložen v.</span><span class="sxs-lookup"><span data-stu-id="61829-328">**PartitionKey**: Determines hello partition that hello entity is stored in.</span></span>
* <span data-ttu-id="61829-329">**RowKey**: jednoznačně identifikuje hello entity v rámci oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="61829-329">**RowKey**: Uniquely identifies hello entity within hello partition.</span></span>

<span data-ttu-id="61829-330">Můžete definovat vlastní vlastností too252 pro entitu.</span><span class="sxs-lookup"><span data-stu-id="61829-330">You may define up too252 custom properties for an entity.</span></span> <span data-ttu-id="61829-331">Další informace najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-331">For more information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="61829-332">Hello následující příklad ukazuje, jak tooadd entity tooa tabulky.</span><span class="sxs-lookup"><span data-stu-id="61829-332">hello following example demonstrates how tooadd entities tooa table.</span></span> <span data-ttu-id="61829-333">Hello příklad ukazuje, jak tooretrieve hello tabulky zaměstnanců a přidejte do ní několik entit.</span><span class="sxs-lookup"><span data-stu-id="61829-333">hello example shows how tooretrieve hello employee table and add several entities into it.</span></span> <span data-ttu-id="61829-334">Nejprve navazuje připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="61829-334">First, it establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="61829-335">V dalším kroku se načte hello zadané tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-335">Next, it retrieves hello given table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="61829-336">Pokud hello tabulka neexistuje, hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) rutina je použité toocreate tabulku ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-336">If hello table does not exist, hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet is used toocreate a table in Azure Storage.</span></span> <span data-ttu-id="61829-337">V dalším kroku hello příklad definuje vlastní funkci Přidat Entity tooadd entity toohello tabulku zadáním každé entity oddílu a klíč řádku.</span><span class="sxs-lookup"><span data-stu-id="61829-337">Next, hello example defines a custom function Add-Entity tooadd entities toohello table by specifying each entity's partition and row key.</span></span> <span data-ttu-id="61829-338">hello volání funkce Hello přidat Entity [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na hello rutinu [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) toocreate třída objektu entity.</span><span class="sxs-lookup"><span data-stu-id="61829-338">hello Add-Entity function calls hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on hello [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) class toocreate an entity object.</span></span> <span data-ttu-id="61829-339">Později, hello ukázka volá hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) metoda na tento objekt tooadd entity ho tooa tabulky.</span><span class="sxs-lookup"><span data-stu-id="61829-339">Later, hello example calls hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) method on this entity object tooadd it tooa table.</span></span>

```powershell
#Function Add-Entity: Adds an employee entity tooa table.
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

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve hello table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities tooa table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-tooquery-table-entities"></a><span data-ttu-id="61829-340">Jak tooquery tabulka entity</span><span class="sxs-lookup"><span data-stu-id="61829-340">How tooquery table entities</span></span>
<span data-ttu-id="61829-341">tooquery tabulky, použijte hello [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="61829-341">tooquery a table, use hello [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) class.</span></span> <span data-ttu-id="61829-342">Hello následující příklad předpokládá, že jste již spuštění skriptu hello uvedenému v hello jak tooadd entity části této příručky.</span><span class="sxs-lookup"><span data-stu-id="61829-342">hello following example assumes that you've already run hello script given in hello How tooadd entities section of this guide.</span></span> <span data-ttu-id="61829-343">Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí hello úložiště kontext, který obsahuje název účtu úložiště hello a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="61829-343">hello example first establishes a connection tooAzure Storage using hello storage context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="61829-344">V dalším kroku se pokusí o tooretrieve hello vytvořili "zaměstnanci" tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-344">Next, it tries tooretrieve hello previously created "Employees" table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="61829-345">Volání hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) na hello Microsoft.WindowsAzure.Storage.Table.TableQuery třída rutinu vytvoří nový objekt dotazu.</span><span class="sxs-lookup"><span data-stu-id="61829-345">Calling hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on hello Microsoft.WindowsAzure.Storage.Table.TableQuery class creates a new query object.</span></span> <span data-ttu-id="61829-346">Příklad Hello hledá hello entity, které mají na sloupec 'ID', jehož hodnota je 1 zadané v řetězec filtru.</span><span class="sxs-lookup"><span data-stu-id="61829-346">hello example looks for hello entities that have an 'ID' column whose value is 1 as specified in a string filter.</span></span> <span data-ttu-id="61829-347">Podrobné informace najdete v tématu [dotazování tabulky a entity](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-347">For detailed information, see [Querying Tables and Entities](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span></span> <span data-ttu-id="61829-348">Při spuštění tohoto dotazu vrátí všechny entity, které odpovídají kritériím filtru hello.</span><span class="sxs-lookup"><span data-stu-id="61829-348">When you run this query, it returns all entities that match hello filter criteria.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference tooa table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns tooselect.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute hello query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with hello table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-toodelete-table-entities"></a><span data-ttu-id="61829-349">Jak toodelete tabulka entity</span><span class="sxs-lookup"><span data-stu-id="61829-349">How toodelete table entities</span></span>
<span data-ttu-id="61829-350">Můžete odstranit pomocí jeho klíče oddílu a řádku entity.</span><span class="sxs-lookup"><span data-stu-id="61829-350">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="61829-351">Hello následující příklad předpokládá, že jste již spuštění skriptu hello uvedenému v hello jak tooadd entity části této příručky.</span><span class="sxs-lookup"><span data-stu-id="61829-351">hello following example assumes that you've already run hello script given in hello How tooadd entities section of this guide.</span></span> <span data-ttu-id="61829-352">Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí hello úložiště kontext, který obsahuje název účtu úložiště hello a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="61829-352">hello example first establishes a connection tooAzure Storage using hello storage context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="61829-353">V dalším kroku se pokusí o tooretrieve hello vytvořili "zaměstnanci" tabulky pomocí hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-353">Next, it tries tooretrieve hello previously created "Employees" table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="61829-354">Pokud tabulka hello existuje, zavolá hello příklad hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) tooretrieve metoda entity podle jeho oddílu a řádku klíčové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="61829-354">If hello table exists, hello example calls hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) method tooretrieve an entity based on its partition and row key values.</span></span> <span data-ttu-id="61829-355">Pak předejte hello entity toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) toodelete metoda.</span><span class="sxs-lookup"><span data-stu-id="61829-355">Then, pass hello entity toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) method toodelete.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If hello table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together hello PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete hello entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-toomanage-azure-queues-and-queue-messages"></a><span data-ttu-id="61829-356">Jak toomanage Azure fronty a zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="61829-356">How toomanage Azure queues and queue messages</span></span>
<span data-ttu-id="61829-357">Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61829-357">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="61829-358">Této části se předpokládá, že jste již obeznámeni s koncepty služby úložiště fronty Azure hello.</span><span class="sxs-lookup"><span data-stu-id="61829-358">This section assumes that you are already familiar with hello Azure Queue Storage Service concepts.</span></span> <span data-ttu-id="61829-359">Podrobné informace najdete v tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="61829-359">For detailed information, see [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md).</span></span>

<span data-ttu-id="61829-360">V této části se dozvíte, jak služba toomanage Azure Queue storage pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-360">This section will show you how toomanage Azure Queue storage service using Azure PowerShell.</span></span> <span data-ttu-id="61829-361">Hello pokryté scénáře zahrnují **vkládání** a **odstraňování** fronty zpráv, a také **vytváření**, **odstraňování**a **načítání fronty**.</span><span class="sxs-lookup"><span data-stu-id="61829-361">hello scenarios covered include **inserting** and **deleting** queue messages, as well as **creating**, **deleting**, and **retrieving queues**.</span></span>

### <a name="how-toocreate-a-queue"></a><span data-ttu-id="61829-362">Jak toocreate fronty</span><span class="sxs-lookup"><span data-stu-id="61829-362">How toocreate a queue</span></span>
<span data-ttu-id="61829-363">Hello následující příklad nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="61829-363">hello following example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="61829-364">Dále volá [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) toocreate rutiny frontu s názvem 'queuename'.</span><span class="sxs-lookup"><span data-stu-id="61829-364">Next, it calls [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet toocreate a queue named 'queuename'.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

<span data-ttu-id="61829-365">Informace o vytváření názvů pro službu front Azure, najdete v části [pojmenování front a Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-365">For information on naming conventions for Azure Queue Service, see [Naming Queues and Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

### <a name="how-tooretrieve-a-queue"></a><span data-ttu-id="61829-366">Jak tooretrieve fronty</span><span class="sxs-lookup"><span data-stu-id="61829-366">How tooretrieve a queue</span></span>
<span data-ttu-id="61829-367">Můžete zadat dotaz a načíst konkrétní fronty nebo seznam všech front hello v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-367">You can query and retrieve a specific queue or a list of all hello queues in a Storage account.</span></span> <span data-ttu-id="61829-368">Hello následující příklad ukazuje, jak tooretrieve zadané frontě pomocí hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-368">hello following example demonstrates how tooretrieve a specified queue using hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span></span>

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

<span data-ttu-id="61829-369">Když zavoláte hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) rutiny bez parametrů, získá seznam všech front hello.</span><span class="sxs-lookup"><span data-stu-id="61829-369">If you call hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet without any parameters, it gets a list of all hello queues.</span></span>

### <a name="how-toodelete-a-queue"></a><span data-ttu-id="61829-370">Jak toodelete fronty</span><span class="sxs-lookup"><span data-stu-id="61829-370">How toodelete a queue</span></span>
<span data-ttu-id="61829-371">toodelete frontu a všechny zprávy hello obsažené v něm volání rutiny hello AzureStorageQueue odebrat.</span><span class="sxs-lookup"><span data-stu-id="61829-371">toodelete a queue and all hello messages contained in it, call hello Remove-AzureStorageQueue cmdlet.</span></span> <span data-ttu-id="61829-372">Hello následující příklad ukazuje, jak hello toodelete zadané frontě pomocí rutiny Remove-AzureStorageQueue.</span><span class="sxs-lookup"><span data-stu-id="61829-372">hello following example shows how toodelete a specified queue using hello Remove-AzureStorageQueue cmdlet.</span></span>

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-tooinsert-a-message-into-a-queue"></a><span data-ttu-id="61829-373">Jak tooinsert zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="61829-373">How tooinsert a message into a queue</span></span>
<span data-ttu-id="61829-374">tooinsert zprávu do existující fronty, nejprve vytvořit novou instanci třídy hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="61829-374">tooinsert a message into an existing queue, first create a new instance of hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="61829-375">Pak zavolejte hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="61829-375">Next, call hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method.</span></span> <span data-ttu-id="61829-376">CloudQueueMessage můžete vytvořit z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="61829-376">A CloudQueueMessage can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="61829-377">Hello následující příklad ukazuje, jak tooadd zprávy tooa fronty.</span><span class="sxs-lookup"><span data-stu-id="61829-377">hello following example demonstrates how tooadd message tooa queue.</span></span> <span data-ttu-id="61829-378">Příklad Hello nejprve vytvoří připojení tooAzure úložiště pomocí kontextu účtu úložiště hello, která zahrnuje název účtu úložiště hello a jeho přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="61829-378">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="61829-379">V dalším kroku se načte hello zadané frontě pomocí hello [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="61829-379">Next, it retrieves hello specified queue using hello [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span></span> <span data-ttu-id="61829-380">Pokud hello fronta existuje, hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) rutina je použité toocreate instanci hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="61829-380">If hello queue exists, hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet is used toocreate an instance of hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="61829-381">Později, hello ukázka volá hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metoda na tuto zprávu objekt tooadd ho tooa fronty.</span><span class="sxs-lookup"><span data-stu-id="61829-381">Later, hello example calls hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method on this message object tooadd it tooa queue.</span></span> <span data-ttu-id="61829-382">Zde je kód, který načte fronty a vloží zprávu hello 'MessageInfo':</span><span class="sxs-lookup"><span data-stu-id="61829-382">Here is code which retrieves a queue and inserts hello message 'MessageInfo':</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If hello queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of hello CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message toohello queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-toode-queue-at-hello-next-message"></a><span data-ttu-id="61829-383">Jak toode fronty v hello další zprávy</span><span class="sxs-lookup"><span data-stu-id="61829-383">How toode-queue at hello next message</span></span>
<span data-ttu-id="61829-384">Váš kód vyřazuje zprávy z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="61829-384">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="61829-385">Při volání hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metody get hello další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="61829-385">When you call hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) method, you get hello next message in a queue.</span></span> <span data-ttu-id="61829-386">Zpráva vrácená metodou **GetMessage** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="61829-386">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="61829-387">toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="61829-387">toofinish removing hello message from hello queue, you must also call hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) method.</span></span> <span data-ttu-id="61829-388">Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="61829-388">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="61829-389">Váš kód zavolá metodu **DeleteMessage** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="61829-389">Your code calls **DeleteMessage** right after hello message has been processed.</span></span>

```powershell
# Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get hello message object from hello queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete hello message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-toomanage-azure-file-shares-and-files"></a><span data-ttu-id="61829-390">Jak toomanage Azure file sdílené složky a soubory</span><span class="sxs-lookup"><span data-stu-id="61829-390">How toomanage Azure file shares and files</span></span>
<span data-ttu-id="61829-391">Azure File storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB hello.</span><span class="sxs-lookup"><span data-stu-id="61829-391">Azure File storage offers shared storage for applications using hello standard SMB protocol.</span></span> <span data-ttu-id="61829-392">Virtuální počítače Microsoft Azure a cloudových služeb můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím rozhraní API úložiště souboru hello nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-392">Microsoft Azure virtual machines and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share via hello File storage API or Azure PowerShell.</span></span>

<span data-ttu-id="61829-393">Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](storage-dotnet-how-to-use-files.md) a [rozhraní API REST služby soubor](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span><span class="sxs-lookup"><span data-stu-id="61829-393">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) and [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span></span>

## <a name="how-tooset-and-query-storage-analytics"></a><span data-ttu-id="61829-394">Jak tooset a dotaz analytika úložiště</span><span class="sxs-lookup"><span data-stu-id="61829-394">How tooset and query storage analytics</span></span>
<span data-ttu-id="61829-395">Můžete použít [Azure Storage Analytics](storage-analytics.md) toocollect metriky pro účty úložiště Azure a data protokolu o žádosti odeslané tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-395">You can use [Azure Storage Analytics](storage-analytics.md) toocollect metrics for your Azure storage accounts and log data about requests sent tooyour storage account.</span></span> <span data-ttu-id="61829-396">Můžete použít stav hello toomonitor metriky úložiště z účtu úložiště a toodiagnose protokolování úložiště a řešení problémů s vaším účtem úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-396">You can use storage metrics toomonitor hello health of a storage account, and storage logging toodiagnose and troubleshoot issues with your storage account.</span></span> <span data-ttu-id="61829-397">Můžete nakonfigurovat monitorování pomocí hello portálu Azure nebo prostředí Windows PowerShell nebo programově pomocí klientské knihovny pro úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="61829-397">You can configure monitoring using hello Azure portal or Windows PowerShell, or programmatically using hello storage client library.</span></span> <span data-ttu-id="61829-398">Protokolování úložiště se stane, serverový a umožní vám toorecord podrobnosti úspěšných i neúspěšných požadavků ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="61829-398">Storage logging happens server-side and enables you toorecord details for both successful and failed requests in your storage account.</span></span> <span data-ttu-id="61829-399">Tyto protokoly umožňují toosee podrobnosti o čtení, zápisu a operace odstranění proti vaší tabulky, fronty a objekty BLOB a také hello důvody pro chybné žádosti.</span><span class="sxs-lookup"><span data-stu-id="61829-399">These logs enable you toosee details of read, write, and delete operations against your tables, queues, and blobs as well as hello reasons for failed requests.</span></span>

<span data-ttu-id="61829-400">jak zjistit, tooenable a zobrazení data metriky úložiště pomocí prostředí PowerShell, toolearn [jak tooenable metriky úložiště pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span><span class="sxs-lookup"><span data-stu-id="61829-400">toolearn how tooenable and view Storage Metrics data using PowerShell, see [How tooenable Storage Metrics using PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span></span>

<span data-ttu-id="61829-401">toolearn jak zjistit, tooenable a načtení protokolování úložiště dat pomocí prostředí PowerShell, [jak tooenable úložiště protokolování pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) a [hledání data protokolu protokolování úložiště](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span><span class="sxs-lookup"><span data-stu-id="61829-401">toolearn how tooenable and retrieve Storage Logging data using PowerShell, see [How tooenable Storage Logging using PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) and [Finding your Storage Logging log data](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span></span>
<span data-ttu-id="61829-402">Podrobné informace o použití úložiště metrik a protokolování problémů s úložištěm tootroubleshoot úložiště najdete v tématu [monitorování, diagnostikování a řešení potíží s Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="61829-402">For detailed information on using Storage Metrics and Storage Logging tootroubleshoot storage issues, see [Monitoring, Diagnosing, and Troubleshooting Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span></span>

## <a name="how-toomanage-shared-access-signature-sas-and-stored-access-policy"></a><span data-ttu-id="61829-403">Jak toomanage sdílený přístupový podpis (SAS) a uložené zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="61829-403">How toomanage Shared Access Signature (SAS) and Stored Access Policy</span></span>
<span data-ttu-id="61829-404">Sdílené přístupové podpisy jsou důležitou součástí hello model zabezpečení pro všechny aplikace pomocí Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-404">Shared access signatures are an important part of hello security model for any application using Azure Storage.</span></span> <span data-ttu-id="61829-405">Jsou užitečné pro zajištění omezenými oprávněními tooyour úložiště účet tooclients, který by neměl mít hello klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="61829-405">They are useful for providing limited permissions tooyour storage account tooclients that should not have hello account key.</span></span> <span data-ttu-id="61829-406">Ve výchozím nastavení může přístup pouze vlastník hello hello účtu úložiště objektů BLOB, tabulek a v rámci tohoto účtu.</span><span class="sxs-lookup"><span data-stu-id="61829-406">By default, only hello owner of hello storage account may access blobs, tables, and queues within that account.</span></span> <span data-ttu-id="61829-407">Pokud služba nebo aplikace potřebuje toomake těchto klientů k dispozici tooother prostředky bez sdílení přístupový klíč, máte tři možnosti:</span><span class="sxs-lookup"><span data-stu-id="61829-407">If your service or application needs toomake these resources available tooother clients without sharing your access key, you have three options:</span></span>

* <span data-ttu-id="61829-408">Nastavte kontejneru oprávnění toopermit anonymní přístup pro čtení toohello kontejner a jeho objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="61829-408">Set a container's permissions toopermit anonymous read access toohello container and its blobs.</span></span> <span data-ttu-id="61829-409">To není povolené pro tabulky a fronty.</span><span class="sxs-lookup"><span data-stu-id="61829-409">This is not allowed for tables or queues.</span></span>
* <span data-ttu-id="61829-410">Použijte sdílený přístupový podpis, která uděluje práva toocontainers s omezeným přístupem, objekty BLOB, fronty a tabulky pro určitého časového intervalu.</span><span class="sxs-lookup"><span data-stu-id="61829-410">Use a shared access signature that grants restricted access rights toocontainers, blobs, queues, and tables for a specific time interval.</span></span>
* <span data-ttu-id="61829-411">Použijte zásady tooobtain uložené přístup na další úroveň kontroly nad sdílené přístupové podpisy pro kontejner nebo jeho objekty BLOB, fronty nebo pro tabulku.</span><span class="sxs-lookup"><span data-stu-id="61829-411">Use a stored access policy tooobtain an additional level of control over shared access signatures for a container or its blobs, for a queue, or for a table.</span></span> <span data-ttu-id="61829-412">Hello zásady uložené přístupu vám umožní toochange hello počáteční čas, čas vypršení platnosti nebo oprávnění pro podpis, nebo toorevoke po jeho vydání.</span><span class="sxs-lookup"><span data-stu-id="61829-412">hello stored access policy allows you toochange hello start time, expiry time, or permissions for a signature, or toorevoke it after it has been issued.</span></span>

<span data-ttu-id="61829-413">Sdílený přístupový podpis může být v jednom ze dvou formách:</span><span class="sxs-lookup"><span data-stu-id="61829-413">A shared access signature can be in one of two forms:</span></span>

* <span data-ttu-id="61829-414">**Ad hoc SAS**: Když vytvoříte ad hoc SAS, čas spuštění hello, čas vypršení platnosti a oprávnění pro hello SAS nejsou zadány na hello identifikátor URI pro SAS.</span><span class="sxs-lookup"><span data-stu-id="61829-414">**Ad hoc SAS**: When you create an ad hoc SAS, hello start time, expiry time, and permissions for hello SAS are all specified on hello SAS URI.</span></span> <span data-ttu-id="61829-415">Tento typ SAS mohou být vytvořeny na kontejner, objektů blob, tabulka nebo fronty a je jiný odvolatelné.</span><span class="sxs-lookup"><span data-stu-id="61829-415">This type of SAS may be created on a container, blob, table, or queue and it is non-revocable.</span></span>
* <span data-ttu-id="61829-416">**SAS se zásadami přístupu uložené**: zásadu uložené přístupu je definovaný na kontejner prostředků kontejner objektů blob, tabulka nebo fronta – a můžete ji použít toomanage omezení pro jeden nebo více sdílených přístupových podpisů.</span><span class="sxs-lookup"><span data-stu-id="61829-416">**SAS with stored access policy**: A stored access policy is defined on a resource container a blob container, table, or queue - and you can use it toomanage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="61829-417">Pokud přidružíte SAS se zásadami přístupu uložené, hello SAS dědí omezení hello – hello času spuštění, čas vypršení platnosti a - definována pro zásady přístupu hello uložené oprávnění.</span><span class="sxs-lookup"><span data-stu-id="61829-417">When you associate a SAS with a stored access policy, hello SAS inherits hello constraints - hello start time, expiry time, and permissions - defined for hello stored access policy.</span></span> <span data-ttu-id="61829-418">Tento typ SAS se odvolatelné.</span><span class="sxs-lookup"><span data-stu-id="61829-418">This type of SAS is revocable.</span></span>

<span data-ttu-id="61829-419">Další informace najdete v tématu [pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md) a [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="61829-419">For more information, see [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) and [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md).</span></span>

<span data-ttu-id="61829-420">V dalších částech hello, se dozvíte, jak toocreate zásadu sdílený přístupový podpis tokenu a uložené přístup pro tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="61829-420">In hello next sections, you will learn how toocreate a shared access signature token and stored access policy for Azure tables.</span></span> <span data-ttu-id="61829-421">Prostředí Azure PowerShell poskytuje podobné rutiny pro kontejnery objektů BLOB a fronty i.</span><span class="sxs-lookup"><span data-stu-id="61829-421">Azure PowerShell provides similar cmdlets for containers, blobs, and queues as well.</span></span> <span data-ttu-id="61829-422">toorun hello skriptů v této části stáhněte hello [prostředí Azure PowerShell verze 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="61829-422">toorun hello scripts in this section, download hello [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) or later.</span></span>

### <a name="how-toocreate-a-policy-based-shared-access-signature-token"></a><span data-ttu-id="61829-423">Jak tokenu sdíleného přístupového podpisu toocreate na základě zásad</span><span class="sxs-lookup"><span data-stu-id="61829-423">How toocreate a policy-based Shared Access Signature token</span></span>
<span data-ttu-id="61829-424">Použijte toocreate rutiny New-AzureStorageTableStoredAccessPolicy hello nové zásady uložené přístup.</span><span class="sxs-lookup"><span data-stu-id="61829-424">Use hello New-AzureStorageTableStoredAccessPolicy cmdlet toocreate a new stored access policy.</span></span> <span data-ttu-id="61829-425">Potom zavolejte hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) rutiny toocreate nový token podpis sdíleného přístupu na základě zásad pro tabulku Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61829-425">Then, call hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate a new policy-based shared access signature token for an Azure Storage table.</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-toocreate-an-ad-hoc-non-revocable-shared-access-signature-token"></a><span data-ttu-id="61829-426">Jak toocreate tokenu ad hoc sdíleného přístupového podpisu (bez odvolatelné)</span><span class="sxs-lookup"><span data-stu-id="61829-426">How toocreate an ad hoc (non-revocable) Shared Access Signature token</span></span>
<span data-ttu-id="61829-427">Použití hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) rutiny toocreate nový ad hoc (bez odvolatelné) sdíleného přístupového podpisu token pro tabulku Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="61829-427">Use hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate a new ad hoc (non-revocable) Shared Access Signature token for an Azure Storage table:</span></span>

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-toocreate-a-stored-access-policy"></a><span data-ttu-id="61829-428">Jak toocreate zásadu uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="61829-428">How toocreate a stored access policy</span></span>
<span data-ttu-id="61829-429">Použijte hello AzureStorageTableStoredAccessPolicy nové rutiny toocreate nové zásady uložené přístup tabulce Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="61829-429">Use hello New-AzureStorageTableStoredAccessPolicy cmdlet toocreate a new stored access policy for an Azure Storage table:</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-tooupdate-a-stored-access-policy"></a><span data-ttu-id="61829-430">Jak tooupdate zásadu uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="61829-430">How tooupdate a stored access policy</span></span>
<span data-ttu-id="61829-431">Použijte tooupdate rutiny Set-AzureStorageTableStoredAccessPolicy hello existující zásady uložené přístup tabulce Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="61829-431">Use hello Set-AzureStorageTableStoredAccessPolicy cmdlet tooupdate an existing stored access policy for an Azure Storage table:</span></span>

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-toodelete-a-stored-access-policy"></a><span data-ttu-id="61829-432">Jak toodelete zásadu uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="61829-432">How toodelete a stored access policy</span></span>
<span data-ttu-id="61829-433">Použijte rutiny toodelete hello odebrat AzureStorageTableStoredAccessPolicy zásadu přístupu uložené na tabulky Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="61829-433">Use hello Remove-AzureStorageTableStoredAccessPolicy cmdlet toodelete a stored access policy on an Azure Storage table:</span></span>

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-toouse-azure-storage-for-us-government-and-azure-china"></a><span data-ttu-id="61829-434">Jak toouse Azure Storage pro Spojených státech amerických a Azure China</span><span class="sxs-lookup"><span data-stu-id="61829-434">How toouse Azure Storage for U.S. government and Azure China</span></span>
<span data-ttu-id="61829-435">Prostředí Azure je nezávislé nasazení ve službě Microsoft Azure, jako například [Azure Government pro US government](https://azure.microsoft.com/features/gov/), [AzureCloud pro globální Azure](https://portal.azure.com), a [AzureChinaCloud pro Azure provozované v Číně společností 21Vianet](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="61829-435">An Azure environment is an independent deployment of Microsoft Azure, such as [Azure Government for U.S. government](https://azure.microsoft.com/features/gov/), [AzureCloud for global Azure](https://portal.azure.com), and [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span> <span data-ttu-id="61829-436">Můžete nasadit nové prostředí Azure government ministerstvo a Azure China.</span><span class="sxs-lookup"><span data-stu-id="61829-436">You can deploy new Azure environments for U.S government and Azure China.</span></span>

<span data-ttu-id="61829-437">toouse Azure Storage s AzureChinaCloud, musíte toocreate kontext úložiště, který je přidružen AzureChinaCloud.</span><span class="sxs-lookup"><span data-stu-id="61829-437">toouse Azure Storage with AzureChinaCloud, you need toocreate a storage context that is associated with AzureChinaCloud.</span></span> <span data-ttu-id="61829-438">Postupujte podle těchto kroků tooget, kterou jste zahájili:</span><span class="sxs-lookup"><span data-stu-id="61829-438">Follow these steps tooget you started:</span></span>

1. <span data-ttu-id="61829-439">Spustit hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) toosee rutiny hello k dispozici prostředí Azure:</span><span class="sxs-lookup"><span data-stu-id="61829-439">Run hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello available Azure environments:</span></span>
   
    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="61829-440">Přidejte Azure China účet tooWindows prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="61829-440">Add an Azure China account tooWindows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. <span data-ttu-id="61829-441">Vytvoření kontextu úložiště pro účet AzureChinaCloud:</span><span class="sxs-lookup"><span data-stu-id="61829-441">Create a storage context for an AzureChinaCloud account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

<span data-ttu-id="61829-442">toouse Azure Storage s [USA Azure Government](https://azure.microsoft.com/features/gov/), měli byste definovat nové prostředí a pak vytvořte nový kontext úložiště s prostředím tohoto nástroje:</span><span class="sxs-lookup"><span data-stu-id="61829-442">toouse Azure Storage with [U.S. Azure Government](https://azure.microsoft.com/features/gov/), you should define a new environment and then create a new storage context with this environment:</span></span>

1. <span data-ttu-id="61829-443">Spustit hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) toosee rutiny hello k dispozici prostředí Azure:</span><span class="sxs-lookup"><span data-stu-id="61829-443">Run hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello available Azure environments:</span></span>

    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="61829-444">Přidejte Azure US Government účet tooWindows prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="61829-444">Add an Azure US Government account tooWindows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. <span data-ttu-id="61829-445">Vytvoření kontextu úložiště pro účet AzureUSGovernment:</span><span class="sxs-lookup"><span data-stu-id="61829-445">Create a storage context for an AzureUSGovernment account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
<span data-ttu-id="61829-446">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="61829-446">For more information, see:</span></span>

* <span data-ttu-id="61829-447">[Příručka vývojáře pro službu Microsoft Azure Government](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="61829-447">[Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>
* [<span data-ttu-id="61829-448">Přehled rozdíly při vytváření aplikace v Číně služby</span><span class="sxs-lookup"><span data-stu-id="61829-448">Overview of Differences When Creating an Application on China Service</span></span>](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a><span data-ttu-id="61829-449">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61829-449">Next Steps</span></span>
<span data-ttu-id="61829-450">V této příručce, když jste se naučili jak toomanage Azure Storage pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61829-450">In this guide, you've learned how toomanage Azure Storage with Azure PowerShell.</span></span> <span data-ttu-id="61829-451">Tady jsou některé související články a zdroje informací o nich víc.</span><span class="sxs-lookup"><span data-stu-id="61829-451">Here are some related articles and resources for learning more about them.</span></span>

* [<span data-ttu-id="61829-452">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="61829-452">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="61829-453">Rutiny prostředí PowerShell úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="61829-453">Azure Storage PowerShell Cmdlets</span></span>](/powershell/module/azurerm.storage/#storage)
* [<span data-ttu-id="61829-454">Referenční informace prostředí PowerShell systému Windows</span><span class="sxs-lookup"><span data-stu-id="61829-454">Windows PowerShell Reference</span></span>](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How toomanage storage accounts in Azure]: #manageaccount
[How tooset a default Azure subscription]: #setdefsub
[How toocreate a new Azure storage account]: #createaccount
[How tooset a default Azure storage account]: #defaultaccount
[How toolist all Azure storage accounts in a subscription]: #listaccounts
[How toocreate an Azure storage context]: #createctx
[How toomanage Azure blobs and blob snapshots]: #manageblobs
[How toocreate a container]: #container
[How tooupload a blob into a container]: #uploadblob
[How toodownload blobs from a container]: #downblob
[How toocopy blobs from one storage container tooanother]: #copyblob
[How toodelete a blob]: #deleteblob
[How toomanage Azure blob snapshots]: #manageshots
[How toocreate a blob snapshot]: #createshot
[How toolist snapshots of a blob]: #listshot
[How toocopy a snapshot of a blob]: #copyshot
[How toomanage Azure tables and table entities]: #managetables
[How toocreate a table]: #createtable
[How tooretrieve a table]: #gettable
[How toodelete a table]: #remtable
[How toomanage table entities]: #mngentity
[How tooadd table entities]: #addentity
[How tooquery table entities]: #queryentity
[How toodelete table entities]: #deleteentity
[How toomanage Azure queues and queue messages]: #managequeues
[How toocreate a queue]: #createqueue
[How tooretrieve a queue]: #getqueue
[How toodelete a queue]: #remqueue
[How toomanage queue messages]: #mngqueuemsg
[How tooinsert a message into a queue]: #addqueuemsg
[How toode-queue at hello next message]: #dequeuemsg
[How toomanage Azure file shares and files]: #files
[How tooset and query storage analytics]: #stganalytics
[How toomanage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How toouse Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
