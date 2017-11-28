---
title: "Expresní emulátor toorun aaaUsing a ladění Azure cloudové služby v místním počítači | Microsoft Docs"
description: "Pomocí emulátoru Express toorun a ladění cloudové služby v místním počítači"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d89a0fc2dce42b4a2d2f93f9c4ff0482af924ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-emulator-express-toorun-and-debug-an-azure-cloud-service-on-a-local-machine"></a><span data-ttu-id="6995e-103">Pomocí emulátoru Express toorun a ladění Azure cloudové služby v místním počítači</span><span class="sxs-lookup"><span data-stu-id="6995e-103">Using Emulator Express toorun and debug an Azure cloud service on a local machine</span></span>
<span data-ttu-id="6995e-104">Pomocí emulátoru Express, můžete si otestovat a ladění cloudové služby bez spuštění sady Visual Studio jako správce.</span><span class="sxs-lookup"><span data-stu-id="6995e-104">By using Emulator Express, you can test and debug a cloud service without running Visual Studio as an administrator.</span></span> <span data-ttu-id="6995e-105">Můžete nastavit toouse nastavení vašeho projektu buď Express emulátoru nebo hello úplný emulátor, v závislosti na požadavcích hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6995e-105">You can set your project settings toouse either Emulator Express or hello full emulator, depending on hello requirements of your cloud service.</span></span> <span data-ttu-id="6995e-106">Další informace o úplný emulátor hello najdete v tématu [spustit aplikaci Azure v hello výpočetní emulátor](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="6995e-106">For more information about hello full emulator, see [Run an Azure Application in hello Compute Emulator](storage/common/storage-use-emulator.md).</span></span>

## <a name="using-emulator-express-in-visual-studio"></a><span data-ttu-id="6995e-107">Pomocí expresní emulátor služby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6995e-107">Using Emulator Express in Visual Studio</span></span>
<span data-ttu-id="6995e-108">Při vytváření projektu Azure v Azure SDK 2.3 nebo novější, expresní emulátor automaticky použita.</span><span class="sxs-lookup"><span data-stu-id="6995e-108">When you create an Azure project in Azure SDK 2.3 or later, Emulator Express is automatically used.</span></span> <span data-ttu-id="6995e-109">Pro existující projekty, které byly vytvořeny z předchozích verzí hello Azure SDK použijte následující kroky tooselect expresní emulátor hello:</span><span class="sxs-lookup"><span data-stu-id="6995e-109">For existing projects that were created with an earlier version of hello Azure SDK, use hello following steps tooselect Emulator Express:</span></span>

1. <span data-ttu-id="6995e-110">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6995e-110">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="6995e-111">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6995e-111">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>

1. <span data-ttu-id="6995e-112">Na stránkách vlastností projekty hello vyberte hello **webové** kartě.</span><span class="sxs-lookup"><span data-stu-id="6995e-112">In hello projects properties pages, select hello **Web** tab.</span></span>

    ![Vlastnosti projektu Azure cloud service](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. <span data-ttu-id="6995e-114">V části **místní vývojový Server**, vyberte **možnost použít službu IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="6995e-114">Under **Local Development Server**, select **Use IIS Express option**.</span></span>

1. <span data-ttu-id="6995e-115">V části **emulátoru**, vyberte **použít expresní emulátor**.</span><span class="sxs-lookup"><span data-stu-id="6995e-115">Under **Emulator**, select **Use Emulator Express**.</span></span>
   
1. <span data-ttu-id="6995e-116">toolaunch hello emulátoru Express, spusťte následující příkaz na příkazovém řádku hello:</span><span class="sxs-lookup"><span data-stu-id="6995e-116">toolaunch hello Emulator Express, run hello following command at a command prompt:</span></span> 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a><span data-ttu-id="6995e-117">Omezení expresní emulátor</span><span class="sxs-lookup"><span data-stu-id="6995e-117">Emulator Express limitations</span></span>
<span data-ttu-id="6995e-118">omezení expresní emulátor známé Hello následující problémy:</span><span class="sxs-lookup"><span data-stu-id="6995e-118">hello following issues are known limitations of Emulator Express:</span></span> 

- <span data-ttu-id="6995e-119">Expresní emulátor není kompatibilní s webového serveru IIS.</span><span class="sxs-lookup"><span data-stu-id="6995e-119">Emulator Express is not compatible with IIS Web Server.</span></span>
- <span data-ttu-id="6995e-120">Cloudové služby může obsahovat víc rolí, ale každá role je omezená tooone instance.</span><span class="sxs-lookup"><span data-stu-id="6995e-120">Your cloud service can contain multiple roles, but each role is limited tooone instance.</span></span>
- <span data-ttu-id="6995e-121">Nelze získat přístup čísla portů pod 1 000.</span><span class="sxs-lookup"><span data-stu-id="6995e-121">You can't access port numbers below 1000.</span></span> <span data-ttu-id="6995e-122">Pokud používáte zprostředkovatele ověřování, který obvykle využívá port pod 1 000, bude pravděpodobně nutné toochange toto číslo portu tooa hodnotu, která je vyšší než 1 000.</span><span class="sxs-lookup"><span data-stu-id="6995e-122">If you use an authentication provider that normally uses a port below 1000, you might need toochange this value tooa port number that's above 1000.</span></span>
- <span data-ttu-id="6995e-123">Jakákoli omezení, které se vztahují toohello výpočetní emulátor Azure platí také tooEmulator Express.</span><span class="sxs-lookup"><span data-stu-id="6995e-123">Any limitations that apply toohello Azure Compute Emulator also apply tooEmulator Express.</span></span> <span data-ttu-id="6995e-124">Například nemůže mít více než 50 instancí role na nasazení.</span><span class="sxs-lookup"><span data-stu-id="6995e-124">For example, you can't have more than 50 role instances per deployment.</span></span> <span data-ttu-id="6995e-125">Další informace o hello výpočetní emulátor Azure najdete v tématu [spustit aplikaci Azure v hello výpočetní emulátor](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span><span class="sxs-lookup"><span data-stu-id="6995e-125">For more information about hello Azure Compute Emulator, see [Run an Azure Application in hello Compute Emulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6995e-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6995e-126">Next steps</span></span>
[<span data-ttu-id="6995e-127">Ladění cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="6995e-127">Debugging Azure cloud services</span></span>](https://msdn.microsoft.com/library/azure/ee405479.aspx)
