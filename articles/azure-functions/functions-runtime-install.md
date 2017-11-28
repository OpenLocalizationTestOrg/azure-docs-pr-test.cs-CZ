---
title: Instalace funkce modulu CLR aaaAzure | Microsoft Docs
description: Jak tooInstall hello Azure Functions Runtime
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
ms.openlocfilehash: 67c6d10b5c0ac43e880d29cff0ae7b099f82bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-functions-runtime-preview"></a><span data-ttu-id="ffdf7-103">Nainstalujte hello Azure funkce Runtime Preview</span><span class="sxs-lookup"><span data-stu-id="ffdf7-103">Install hello Azure Functions Runtime Preview</span></span>

<span data-ttu-id="ffdf7-104">Pokud chcete tooinstall hello Azure Functions Runtime preview, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="ffdf7-104">If you would like tooinstall hello Azure Functions Runtime preview, you must follow these steps:</span></span>

1. <span data-ttu-id="ffdf7-105">Zajistěte, aby byl že váš počítač úspěšně projde hello minimální požadavky</span><span class="sxs-lookup"><span data-stu-id="ffdf7-105">Ensure your machine passes hello minimum requirements</span></span>
1. <span data-ttu-id="ffdf7-106">Stáhnout hello [Azure funkce Runtime Preview instalační](https://aka.ms/azafr).</span><span class="sxs-lookup"><span data-stu-id="ffdf7-106">Download hello [Azure Functions Runtime Preview Installer](https://aka.ms/azafr).</span></span> 
1. <span data-ttu-id="ffdf7-107">Nainstalujte hello Runtime funkce Azure preview</span><span class="sxs-lookup"><span data-stu-id="ffdf7-107">Install hello Azure Functions Runtime preview</span></span>
1. <span data-ttu-id="ffdf7-108">Konfigurace dokončení hello hello Runtime funkce Azure Preview</span><span class="sxs-lookup"><span data-stu-id="ffdf7-108">Complete hello configuration of hello Azure Functions Runtime preview</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ffdf7-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ffdf7-109">Prerequisites</span></span>

<span data-ttu-id="ffdf7-110">Než nainstalujete hello Azure Functions Runtime preview, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="ffdf7-110">Before you install hello Azure Functions Runtime preview, you must have hello following:</span></span>

1. <span data-ttu-id="ffdf7-111">Počítač se systémem Microsoft Windows Server 2016 nebo Microsoft Windows 10 Creators Update (Professional nebo Enterprise Edition).</span><span class="sxs-lookup"><span data-stu-id="ffdf7-111">A machine running Microsoft Windows Server 2016 or Microsoft Windows 10 Creators Update (Professional or Enterprise Edition).</span></span>
1. <span data-ttu-id="ffdf7-112">Instance SQL serveru spuštěna v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-112">A SQL Server instance running within your network.</span></span>  <span data-ttu-id="ffdf7-113">Požadavek na minimální verzi je SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-113">Minimum edition requirement is SQL Server Express.</span></span>

## <a name="install-hello-azure-functions-runtime-preview"></a><span data-ttu-id="ffdf7-114">Nainstalujte hello Azure funkce Runtime Preview</span><span class="sxs-lookup"><span data-stu-id="ffdf7-114">Install hello Azure Functions Runtime Preview</span></span>

<span data-ttu-id="ffdf7-115">Instalační program preview Azure Functions Runtime Hello vás provede instalaci hello hello Azure Functions Runtime preview správy a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-115">hello Azure Functions Runtime preview installer guides you through hello installation of hello Azure Functions Runtime preview Management and Worker Roles.</span></span>  <span data-ttu-id="ffdf7-116">Je možné tooinstall hello správy a roli pracovního procesu na hello stejný počítač.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-116">It is possible tooinstall hello Management and Worker role on hello same machine.</span></span>  <span data-ttu-id="ffdf7-117">Však jako přidáte další funkce, je nutné nasadit více rolí pracovního procesu na další počítače toobe možné tooscale funkcí do více pracovníků.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-117">However, as you add more Functions, you must deploy more worker roles on additional machines toobe able tooscale your functions onto multiple workers.</span></span>

## <a name="install-hello-management-and-worker-role-on-hello-same-machine"></a><span data-ttu-id="ffdf7-118">Nainstalujte hello správy a roli pracovního procesu na hello stejný počítač</span><span class="sxs-lookup"><span data-stu-id="ffdf7-118">Install hello Management and Worker Role on hello same machine</span></span>

1. <span data-ttu-id="ffdf7-119">Spusťte instalační program Azure funkce Runtime Preview hello.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-119">Run hello Azure Functions Runtime Preview Installer.</span></span>

    ![Instalační program Preview Runtime Azure Functions][1]

1. <span data-ttu-id="ffdf7-121">**Klikněte na tlačítko Další** záloh za hello první fázi instalačního programu hello</span><span class="sxs-lookup"><span data-stu-id="ffdf7-121">**Click Next** advance past hello first stage of hello installer</span></span>
1. <span data-ttu-id="ffdf7-122">Jakmile jste četli hello podmínky hello **smlouvy EULA**, **políčko hello** tooaccept hello podmínky a **klikněte na tlačítko Další** tooadvance.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-122">Once you have read hello terms of hello **EULA**, **check hello box** tooaccept hello terms and **click Next** tooadvance.</span></span>
1. <span data-ttu-id="ffdf7-123">Nyní vyberte hello role chcete tooinstall na tomto počítači **Role správy funkce** nebo **Role pracovního procesu funkce** a **kliknutím na tlačítko Další**</span><span class="sxs-lookup"><span data-stu-id="ffdf7-123">Now select hello roles you want tooinstall on this machine **Functions Management Role** and/or **Functions Worker Role** and **Click Next**</span></span>

    ![Instalační program Preview Runtime Azure Functions - výběr Role][3]

    > [!NOTE]
    > <span data-ttu-id="ffdf7-125">Můžete nainstalovat hello **Role pracovního procesu funkce** na mnoha dalších počítačů toodo Ano, postupujte podle těchto pokynů a vybrat pouze **Role pracovního procesu funkce** v hello Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-125">You can install hello **Functions Worker Role** on many other machines toodo so, follow these instructions, and only select **Functions Worker Role** in hello installer.</span></span>

1. <span data-ttu-id="ffdf7-126">**Klikněte na tlačítko Další** toohave hello **instalační program modulu Runtime Azure funkce** nainstalovat na svůj počítač.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-126">**Click Next** toohave hello **Azure Functions Runtime Installer** install on your machine.</span></span>
1. <span data-ttu-id="ffdf7-127">Po dokončení hello instalační program se spustí hello **nástroj Konfigurace modulu Runtime Azure funkce**.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-127">Once complete hello installer will launch hello **Azure Functions Runtime Configuration tool**.</span></span>

    ![Dokončení instalační Preview Runtime Azure Functions][5]

    > [!NOTE]
    > <span data-ttu-id="ffdf7-129">Pokud instalujete na **Windows 10** a hello **kontejneru** funkce není povolená dříve, hello **Azure Functions Runtime** instalační program zobrazí výzvu tooreboot váš počítač toocomplete hello nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-129">If you are installing on **Windows 10** and hello **Container** feature has not been previously enabled, hello **Azure Functions Runtime** Installer prompts you tooreboot your machine toocomplete hello install.</span></span>

## <a name="configure-hello-azure-functions-runtime"></a><span data-ttu-id="ffdf7-130">Konfigurace hello Azure Functions Runtime</span><span class="sxs-lookup"><span data-stu-id="ffdf7-130">Configure hello Azure Functions Runtime</span></span>

<span data-ttu-id="ffdf7-131">toocomplete hello Azure Functions Runtime instalace musí dokončit konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-131">toocomplete hello Azure Functions Runtime installation you must complete hello configuration.</span></span>

1. <span data-ttu-id="ffdf7-132">Hello **nástroj Konfigurace Runtime funkce Azure** ukazuje, jaké role jsou nainstalovány v počítači.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-132">hello **Azure Functions Runtime Configuration Tool** shows which roles are installed on your machine.</span></span>

    ![Nástroj konfigurace Preview Runtime Azure Functions][6]

1. <span data-ttu-id="ffdf7-134">Klikněte na tlačítko hello **databáze** zadejte hello **podrobnosti připojení pro vaše Instance systému SQL Server** a **kliknutím na tlačítko použít**.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-134">Click hello **Database** tab, enter hello **connection details for your SQL Server Instance** and **click Apply**.</span></span>  <span data-ttu-id="ffdf7-135">To je nutné v pořadí toohello toocreate Azure Functions Runtime databáze toosupport hello modulu Runtime.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-135">This is required in order toohello Azure Functions Runtime toocreate a database toosupport hello Runtime.</span></span>
    
    ![Konfigurace databáze Preview modulu Runtime Azure Functions][7]

1. <span data-ttu-id="ffdf7-137">Klikněte na tlačítko hello **pověření** kartě.  Na této obrazovce je nutné vytvořit dva nové přihlašovací údaje pro použití s sdílení souborů pro hostování všech Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-137">Click hello **Credentials** tab.  On this screen you must create two new credentials for use with a FileShare for hosting all your Azure Functions.</span></span>  <span data-ttu-id="ffdf7-138">**Zadejte uživatelské jméno a heslo** kombinací pro hello **vlastníka sdílené složky souboru** a hello **uživatele sdílené složky souboru** a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-138">**Specify Username and Password** combinations for hello **File Share Owner** and for hello **File Share User** and click **Apply**.</span></span>

    ![Přihlašovací údaje Preview Runtime Azure Functions][8]

1. <span data-ttu-id="ffdf7-140">Klikněte na tlačítko hello **sdílené složky** kartě.  V této obrazovce je nutné zadat podrobnosti o hello hello **umístění sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-140">Click hello **File Share** tab.  In this screen you must specify hello details of hello **File Share location**.</span></span>  <span data-ttu-id="ffdf7-141">To je možné vytvořit za vás, nebo můžete použít existující sdílené složky a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-141">This can be created for you or you can use an existing File Share and click **Apply**.</span></span>  <span data-ttu-id="ffdf7-142">Pokud vyberete nové umístění sdílené složky, musíte zadat adresář pro použití ve hello Azure Functions Runtime.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-142">If you select a new File Share location, you must specify a directory for use by hello Azure Functions Runtime.</span></span>
    
    ![Azure Functions Runtime Preview sdílené složky][9]

1. <span data-ttu-id="ffdf7-144">Klikněte na tlačítko hello **IIS** kartě.  Tato karta obsahuje podrobnosti hello hello webů ve službě IIS tento hello instalace modulu Runtime funkce Azure vytvoří.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-144">Click hello **IIS** tab.  This tab shows hello details of hello websites in IIS that hello Azure Functions Runtime Installation will create.</span></span>  <span data-ttu-id="ffdf7-145">**Klikněte na tlačítko použít** toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-145">**Click Apply** toocomplete.</span></span>

    ![Azure Functions Preview Runtime služby IIS][10]

1. <span data-ttu-id="ffdf7-147">Klikněte na tlačítko hello **služby** kartě.  Tato karta zobrazuje stav hello hello služeb v instalaci Azure Functions Runtime.</span><span class="sxs-lookup"><span data-stu-id="ffdf7-147">Click hello **Services** tab.  This tab shows hello status of hello services in your Azure Functions Runtime installation.</span></span>  <span data-ttu-id="ffdf7-148">Pokud po počáteční konfiguraci hello **služba Aktivace hostitele funkce Azure** neběží klikněte na tlačítko **spustit službu**</span><span class="sxs-lookup"><span data-stu-id="ffdf7-148">If after initial configuration hello **Azure Functions Host Activation Service** is not running click **Start Service**</span></span>

    ![Dokončení Configruation Preview Runtime Azure Functions][11]

1. <span data-ttu-id="ffdf7-150">Nakonec procházet toohello **modulu Runtime Azure Functions na portálu** jako`https://<machinename>/`</span><span class="sxs-lookup"><span data-stu-id="ffdf7-150">Finally browse toohello **Azure Functions Runtime Portal** as `https://<machinename>/`</span></span>

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