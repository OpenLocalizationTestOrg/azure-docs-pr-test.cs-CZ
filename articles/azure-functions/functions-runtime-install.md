---
title: Azure Functions instalace modulu Runtime | Microsoft Docs
description: Postup instalace modulu Runtime Azure Functions
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 1e4188313a87d07f396e5f8edc8969dd5da2c436
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-the-azure-functions-runtime-preview"></a><span data-ttu-id="f56d8-103">Nainstalujte Preview Runtime Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f56d8-103">Install the Azure Functions Runtime Preview</span></span>

<span data-ttu-id="f56d8-104">Pokud chcete nainstalovat ve verzi preview Azure Functions Runtime, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="f56d8-104">If you would like to install the Azure Functions Runtime preview, you must follow these steps:</span></span>

1. <span data-ttu-id="f56d8-105">Zajistěte, aby byl že váš počítač úspěšně projde minimální požadavky</span><span class="sxs-lookup"><span data-stu-id="f56d8-105">Ensure your machine passes the minimum requirements</span></span>
1. <span data-ttu-id="f56d8-106">Stažení [Azure Functions instalačního programu Preview Runtime](https://aka.ms/azafr).</span><span class="sxs-lookup"><span data-stu-id="f56d8-106">Download the [Azure Functions Runtime Preview Installer](https://aka.ms/azafr).</span></span> 
1. <span data-ttu-id="f56d8-107">Instalace modulu Runtime funkce Azure preview</span><span class="sxs-lookup"><span data-stu-id="f56d8-107">Install the Azure Functions Runtime preview</span></span>
1. <span data-ttu-id="f56d8-108">Dokončení konfigurace preview Azure Functions Runtime</span><span class="sxs-lookup"><span data-stu-id="f56d8-108">Complete the configuration of the Azure Functions Runtime preview</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f56d8-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f56d8-109">Prerequisites</span></span>

<span data-ttu-id="f56d8-110">Než nainstalujete Azure Functions Runtime preview, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="f56d8-110">Before you install the Azure Functions Runtime preview, you must have the following:</span></span>

1. <span data-ttu-id="f56d8-111">Počítač se systémem Microsoft Windows Server 2016 nebo Microsoft Windows 10 Creators Update (Professional nebo Enterprise Edition).</span><span class="sxs-lookup"><span data-stu-id="f56d8-111">A machine running Microsoft Windows Server 2016 or Microsoft Windows 10 Creators Update (Professional or Enterprise Edition).</span></span>
1. <span data-ttu-id="f56d8-112">Instance SQL serveru spuštěna v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="f56d8-112">A SQL Server instance running within your network.</span></span>  <span data-ttu-id="f56d8-113">Požadavek na minimální verzi je SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="f56d8-113">Minimum edition requirement is SQL Server Express.</span></span>

## <a name="install-the-azure-functions-runtime-preview"></a><span data-ttu-id="f56d8-114">Nainstalujte Preview Runtime Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f56d8-114">Install the Azure Functions Runtime Preview</span></span>

<span data-ttu-id="f56d8-115">Instalační program preview Azure Functions Runtime vás provede instalaci preview Azure Functions Runtime správy a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="f56d8-115">The Azure Functions Runtime preview installer guides you through the installation of the Azure Functions Runtime preview Management and Worker Roles.</span></span>  <span data-ttu-id="f56d8-116">Je možné nainstalovat roli správy a pracovního procesu na stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="f56d8-116">It is possible to install the Management and Worker role on the same machine.</span></span>  <span data-ttu-id="f56d8-117">Jako přidáte další funkce, ale musíte nasadit více rolí pracovního procesu na další počítače. abyste mohli škálovat funkcí do více pracovníků.</span><span class="sxs-lookup"><span data-stu-id="f56d8-117">However, as you add more Functions, you must deploy more worker roles on additional machines to be able to scale your functions onto multiple workers.</span></span>

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a><span data-ttu-id="f56d8-118">Instalace správy a roli pracovního procesu na stejném počítači</span><span class="sxs-lookup"><span data-stu-id="f56d8-118">Install the Management and Worker Role on the same machine</span></span>

1. <span data-ttu-id="f56d8-119">Spusťte instalační program Preview Runtime Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="f56d8-119">Run the Azure Functions Runtime Preview Installer.</span></span>

    ![Instalační program Preview Runtime Azure Functions][1]

1. <span data-ttu-id="f56d8-121">**Klikněte na tlačítko Další** záloh po první fázi instalačního programu</span><span class="sxs-lookup"><span data-stu-id="f56d8-121">**Click Next** advance past the first stage of the installer</span></span>
1. <span data-ttu-id="f56d8-122">Jakmile budete mít přečtěte si podmínky **smlouvy EULA**, **zaškrtněte políčko** přijmout podmínky a **klikněte na tlačítko Další** posunut.</span><span class="sxs-lookup"><span data-stu-id="f56d8-122">Once you have read the terms of the **EULA**, **check the box** to accept the terms and **click Next** to advance.</span></span>
1. <span data-ttu-id="f56d8-123">Nyní vyberte role, kterou chcete nainstalovat na tomto počítači **Role správy funkce** nebo **Role pracovního procesu funkce** a **kliknutím na tlačítko Další**</span><span class="sxs-lookup"><span data-stu-id="f56d8-123">Now select the roles you want to install on this machine **Functions Management Role** and/or **Functions Worker Role** and **Click Next**</span></span>

    ![Instalační program Preview Runtime Azure Functions - výběr Role][3]

    > [!NOTE]
    > <span data-ttu-id="f56d8-125">Můžete nainstalovat **Role pracovního procesu funkce** na mnoho dalších počítačů to udělat, postupujte podle těchto pokynů a vybrat pouze **Role pracovního procesu funkce** v instalačním programu.</span><span class="sxs-lookup"><span data-stu-id="f56d8-125">You can install the **Functions Worker Role** on many other machines to do so, follow these instructions, and only select **Functions Worker Role** in the installer.</span></span>

1. <span data-ttu-id="f56d8-126">**Klikněte na tlačítko Další** tak, aby měl **instalační program modulu Runtime Azure funkce** nainstalovat na svůj počítač.</span><span class="sxs-lookup"><span data-stu-id="f56d8-126">**Click Next** to have the **Azure Functions Runtime Installer** install on your machine.</span></span>
1. <span data-ttu-id="f56d8-127">Po dokončení se spustí instalační program **nástroj Konfigurace modulu Runtime Azure funkce**.</span><span class="sxs-lookup"><span data-stu-id="f56d8-127">Once complete the installer will launch the **Azure Functions Runtime Configuration tool**.</span></span>

    ![Dokončení instalační Preview Runtime Azure Functions][5]

    > [!NOTE]
    > <span data-ttu-id="f56d8-129">Pokud instalujete na **Windows 10** a **kontejneru** funkce není povolená dříve, **Azure Functions Runtime** instalační program zobrazí výzvu k restartování vaší počítač pro dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="f56d8-129">If you are installing on **Windows 10** and the **Container** feature has not been previously enabled, the **Azure Functions Runtime** Installer prompts you to reboot your machine to complete the install.</span></span>

## <a name="configure-the-azure-functions-runtime"></a><span data-ttu-id="f56d8-130">Konfigurace modulu Runtime Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f56d8-130">Configure the Azure Functions Runtime</span></span>

<span data-ttu-id="f56d8-131">K dokončení instalace Azure Functions Runtime je třeba provést konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="f56d8-131">To complete the Azure Functions Runtime installation you must complete the configuration.</span></span>

1. <span data-ttu-id="f56d8-132">**Nástroj Konfigurace Runtime funkce Azure** ukazuje, jaké role jsou nainstalovány v počítači.</span><span class="sxs-lookup"><span data-stu-id="f56d8-132">The **Azure Functions Runtime Configuration Tool** shows which roles are installed on your machine.</span></span>

    ![Nástroj konfigurace Preview Runtime Azure Functions][6]

1. <span data-ttu-id="f56d8-134">Klikněte na tlačítko **databáze** , zadejte **podrobnosti připojení pro vaše Instance systému SQL Server** a **kliknutím na tlačítko použít**.</span><span class="sxs-lookup"><span data-stu-id="f56d8-134">Click the **Database** tab, enter the **connection details for your SQL Server Instance** and **click Apply**.</span></span>  <span data-ttu-id="f56d8-135">To je nutné, aby Azure Functions Runtime k vytvoření databáze pro podporu modulu Runtime.</span><span class="sxs-lookup"><span data-stu-id="f56d8-135">This is required in order to the Azure Functions Runtime to create a database to support the Runtime.</span></span>
    
    ![Konfigurace databáze Preview modulu Runtime Azure Functions][7]

1. <span data-ttu-id="f56d8-137">Klikněte **pověření** kartě.</span><span class="sxs-lookup"><span data-stu-id="f56d8-137">Click the **Credentials** tab.</span></span>  <span data-ttu-id="f56d8-138">Na této obrazovce je nutné vytvořit dva nové přihlašovací údaje pro použití s sdílení souborů pro hostování všech Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="f56d8-138">On this screen you must create two new credentials for use with a FileShare for hosting all your Azure Functions.</span></span>  <span data-ttu-id="f56d8-139">**Zadejte uživatelské jméno a heslo** kombinací pro **vlastníka sdílené složky souboru** a **uživatele sdílené složky souboru** a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="f56d8-139">**Specify Username and Password** combinations for the **File Share Owner** and for the **File Share User** and click **Apply**.</span></span>

    ![Přihlašovací údaje Preview Runtime Azure Functions][8]

1. <span data-ttu-id="f56d8-141">Klikněte **sdílené složky** kartě.</span><span class="sxs-lookup"><span data-stu-id="f56d8-141">Click the **File Share** tab.</span></span>  <span data-ttu-id="f56d8-142">V této obrazovce je nutné zadat podrobnosti o **umístění sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="f56d8-142">In this screen you must specify the details of the **File Share location**.</span></span>  <span data-ttu-id="f56d8-143">To je možné vytvořit za vás, nebo můžete použít existující sdílené složky a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="f56d8-143">This can be created for you or you can use an existing File Share and click **Apply**.</span></span>  <span data-ttu-id="f56d8-144">Pokud vyberete nové umístění sdílené složky, musíte zadat adresář pro použití Azure Functions Runtime.</span><span class="sxs-lookup"><span data-stu-id="f56d8-144">If you select a new File Share location, you must specify a directory for use by the Azure Functions Runtime.</span></span>
    
    ![Azure Functions Runtime Preview sdílené složky][9]

1. <span data-ttu-id="f56d8-146">Klikněte **IIS** kartě.</span><span class="sxs-lookup"><span data-stu-id="f56d8-146">Click the **IIS** tab.</span></span>  <span data-ttu-id="f56d8-147">Tato karta zobrazuje podrobnosti o weby ve službě IIS, tím se vytvoří instalace Azure funkce modulu CLR.</span><span class="sxs-lookup"><span data-stu-id="f56d8-147">This tab shows the details of the websites in IIS that the Azure Functions Runtime Installation will create.</span></span>  <span data-ttu-id="f56d8-148">**Klikněte na tlačítko použít** k dokončení.</span><span class="sxs-lookup"><span data-stu-id="f56d8-148">**Click Apply** to complete.</span></span>

    ![Azure Functions Preview Runtime služby IIS][10]

1. <span data-ttu-id="f56d8-150">Klikněte **služby** kartě.</span><span class="sxs-lookup"><span data-stu-id="f56d8-150">Click the **Services** tab.</span></span>  <span data-ttu-id="f56d8-151">Tato karta zobrazuje stav služby v instalaci Azure Functions Runtime.</span><span class="sxs-lookup"><span data-stu-id="f56d8-151">This tab shows the status of the services in your Azure Functions Runtime installation.</span></span>  <span data-ttu-id="f56d8-152">Pokud po počáteční konfiguraci **služba Aktivace hostitele funkce Azure** neběží klikněte na tlačítko **spustit službu**</span><span class="sxs-lookup"><span data-stu-id="f56d8-152">If after initial configuration the **Azure Functions Host Activation Service** is not running click **Start Service**</span></span>

    ![Dokončení Configruation Preview Runtime Azure Functions][11]

1. <span data-ttu-id="f56d8-154">Nakonec vyhledejte **modulu Runtime Azure Functions na portálu** jako`https://<machinename>/`</span><span class="sxs-lookup"><span data-stu-id="f56d8-154">Finally browse to the **Azure Functions Runtime Portal** as `https://<machinename>/`</span></span>

    ![Modul Runtime Azure Functions na portálu Preview][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png