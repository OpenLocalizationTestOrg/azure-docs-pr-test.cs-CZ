---
title: "aaaTroubleshoot rolí, které nesplní toostart | Microsoft Docs"
description: "Tady jsou některé běžné příčiny, proč roli cloudové služby může selhat toostart. K dispozici jsou taky řešení toothese problémy."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: e2fbecb08a10984add79dfc74e73de6869bb314f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-toostart"></a><span data-ttu-id="73281-104">Řešení potíží s cloudové služby rolí, které nesplní toostart</span><span class="sxs-lookup"><span data-stu-id="73281-104">Troubleshoot Cloud Service roles that fail toostart</span></span>
<span data-ttu-id="73281-105">Tady jsou některé běžné problémy a řešení související tooAzure cloudové služby rolí, které nesplní toostart.</span><span class="sxs-lookup"><span data-stu-id="73281-105">Here are some common problems and solutions related tooAzure Cloud Services roles that fail toostart.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a><span data-ttu-id="73281-106">Chybějící knihovny DLL nebo závislosti</span><span class="sxs-lookup"><span data-stu-id="73281-106">Missing DLLs or dependencies</span></span>
<span data-ttu-id="73281-107">Reagovat role a role, které jsou prosté mezi **Probíhá inicializace**, **zaneprázdněn**, a **zastavení** stavy může být způsobeno chybějícím knihovny DLL nebo sestavení.</span><span class="sxs-lookup"><span data-stu-id="73281-107">Unresponsive roles and roles that are cycling between **Initializing**, **Busy**, and **Stopping** states can be caused by missing DLLs or assemblies.</span></span>

<span data-ttu-id="73281-108">Příznaky chybějící knihovny DLL nebo sestavení může být:</span><span class="sxs-lookup"><span data-stu-id="73281-108">Symptoms of missing DLLs or assemblies can be:</span></span>

* <span data-ttu-id="73281-109">Role instance je prosté prostřednictvím **Probíhá inicializace**, **zaneprázdněn**, a **zastavení** stavy.</span><span class="sxs-lookup"><span data-stu-id="73281-109">Your role instance is cycling through **Initializing**, **Busy**, and **Stopping** states.</span></span>
* <span data-ttu-id="73281-110">Role instance přesunula příliš**připraven** , ale pokud přejdete tooyour webové aplikace, se nezobrazí stránka hello.</span><span class="sxs-lookup"><span data-stu-id="73281-110">Your role instance has moved too**Ready** but if you navigate tooyour web application, hello page does not appear.</span></span>

<span data-ttu-id="73281-111">Existuje několik doporučených metod pro příčin těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="73281-111">There are several recommended methods for investigating these issues.</span></span>

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a><span data-ttu-id="73281-112">Chybí knihovna DLL problémů ve webové roli diagnostiky</span><span class="sxs-lookup"><span data-stu-id="73281-112">Diagnose missing DLL issues in a web role</span></span>
<span data-ttu-id="73281-113">Když přejdete tooa webu, který je nasazen ve webové roli a hello prohlížeč zobrazí podobné toohello následující chyba serveru, může to znamenat, že chybí knihovna DLL.</span><span class="sxs-lookup"><span data-stu-id="73281-113">When you navigate tooa website that is deployed in a web role, and hello browser displays a server error similar toohello following, it may indicate that a DLL is missing.</span></span>

![Chyba serveru v aplikaci '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a><span data-ttu-id="73281-115">Diagnostikovat problémy vypnutím vlastní chyby</span><span class="sxs-lookup"><span data-stu-id="73281-115">Diagnose issues by turning off custom errors</span></span>
<span data-ttu-id="73281-116">Nakonfigurováním hello web.config pro režim tooOff vlastní chybové tooset hello hello webové role a opětovného nasazení služby hello lze zobrazit podrobnější informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="73281-116">More complete error information can be viewed by configuring hello web.config for hello web role tooset hello custom error mode tooOff and redeploying hello service.</span></span>

<span data-ttu-id="73281-117">tooview více dokončení chyby bez pomocí vzdálené plochy:</span><span class="sxs-lookup"><span data-stu-id="73281-117">tooview more complete errors without using Remote Desktop:</span></span>

1. <span data-ttu-id="73281-118">Otevřete hello řešení v sadě Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73281-118">Open hello solution in Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="73281-119">V hello **Průzkumníku**, vyhledejte soubor web.config hello a otevřete jej.</span><span class="sxs-lookup"><span data-stu-id="73281-119">In hello **Solution Explorer**, locate hello web.config file and open it.</span></span>
3. <span data-ttu-id="73281-120">V souboru web.config hello najděte část system.web hello a přidejte následující řádek hello:</span><span class="sxs-lookup"><span data-stu-id="73281-120">In hello web.config file, locate hello system.web section and add hello following line:</span></span>

    ```xml
    <customErrors mode="Off" />
    ```
4. <span data-ttu-id="73281-121">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="73281-121">Save hello file.</span></span>
5. <span data-ttu-id="73281-122">Znovu zabalte a znovu nasaďte hello služby.</span><span class="sxs-lookup"><span data-stu-id="73281-122">Repackage and redeploy hello service.</span></span>

<span data-ttu-id="73281-123">Jakmile hello služba je znovu nasazena, zobrazí se chybová zpráva s názvem hello hello chybí sestavení nebo DLL.</span><span class="sxs-lookup"><span data-stu-id="73281-123">Once hello service is redeployed, you will see an error message with hello name of hello missing assembly or DLL.</span></span>

## <a name="diagnose-issues-by-viewing-hello-error-remotely"></a><span data-ttu-id="73281-124">Vyřešení problémů se zobrazením hello chyba vzdáleně</span><span class="sxs-lookup"><span data-stu-id="73281-124">Diagnose issues by viewing hello error remotely</span></span>
<span data-ttu-id="73281-125">Můžete používat roli hello tooaccess Vzdálená plocha a vzdáleně zobrazit podrobnější informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="73281-125">You can use Remote Desktop tooaccess hello role and view more complete error information remotely.</span></span> <span data-ttu-id="73281-126">Použijte následující postup tooview hello chyby pomocí vzdálené plochy hello:</span><span class="sxs-lookup"><span data-stu-id="73281-126">Use hello following steps tooview hello errors by using Remote Desktop:</span></span>

1. <span data-ttu-id="73281-127">Zkontrolujte, zda je nainstalovaný Azure SDK 1.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="73281-127">Ensure that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="73281-128">Během nasazování hello hello řešení pomocí sady Visual Studio, vyberte příliš "... Konfigurace vzdálené plochy připojení".</span><span class="sxs-lookup"><span data-stu-id="73281-128">During hello deployment of hello solution by using Visual Studio, choose too“Configure Remote Desktop connections…”.</span></span> <span data-ttu-id="73281-129">Další informace o konfiguraci připojení ke vzdálené ploše hello najdete v tématu [pomocí vzdálené plochy s rolemi Azure](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="73281-129">For more information on configuring hello Remote Desktop connection, see [Using Remote Desktop with Azure Roles](../vs-azure-tools-remote-desktop-roles.md).</span></span>
3. <span data-ttu-id="73281-130">V hello portálu Microsoft Azure classic, jakmile hello instance zobrazuje stav **připraven**, klikněte na jednu z instancí rolí hello.</span><span class="sxs-lookup"><span data-stu-id="73281-130">In hello Microsoft Azure classic portal, once hello instance shows a status of **Ready**, click one of hello role instances.</span></span>
4. <span data-ttu-id="73281-131">Klikněte na tlačítko hello **připojit** ikonu v hello **vzdáleného přístupu** oblasti hello pásu karet.</span><span class="sxs-lookup"><span data-stu-id="73281-131">Click hello **Connect** icon in hello **Remote Access** area of hello ribbon.</span></span>
5. <span data-ttu-id="73281-132">Přihlaste se toohello virtuálního počítače pomocí hello přihlašovacích údajů, které se zadaly během konfigurace vzdálené plochy hello.</span><span class="sxs-lookup"><span data-stu-id="73281-132">Sign in toohello virtual machine by using hello credentials that were specified during hello Remote Desktop configuration.</span></span>
6. <span data-ttu-id="73281-133">Otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="73281-133">Open a command window.</span></span>
7. <span data-ttu-id="73281-134">Zadejte `IPconfig`.</span><span class="sxs-lookup"><span data-stu-id="73281-134">Type `IPconfig`.</span></span>
8. <span data-ttu-id="73281-135">Poznamenejte si hodnotu hello adresu IPV4.</span><span class="sxs-lookup"><span data-stu-id="73281-135">Note hello IPV4 Address value.</span></span>
9. <span data-ttu-id="73281-136">Otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="73281-136">Open Internet Explorer.</span></span>
10. <span data-ttu-id="73281-137">Zadejte adresu hello a název hello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="73281-137">Type hello address and hello name of hello web application.</span></span> <span data-ttu-id="73281-138">Například, `http://<IPV4 Address>/default.aspx`.</span><span class="sxs-lookup"><span data-stu-id="73281-138">For example, `http://<IPV4 Address>/default.aspx`.</span></span>

<span data-ttu-id="73281-139">Navigace toohello webu nyní vrátí podrobnější chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="73281-139">Navigating toohello website will now return more explicit error messages:</span></span>

* <span data-ttu-id="73281-140">Chyba serveru v aplikaci '/'.</span><span class="sxs-lookup"><span data-stu-id="73281-140">Server Error in '/' Application.</span></span>
* <span data-ttu-id="73281-141">Popis: Při provádění hello hello aktuální webové žádosti došlo k neošetřené výjimce.</span><span class="sxs-lookup"><span data-stu-id="73281-141">Description: An unhandled exception occurred during hello execution of hello current web request.</span></span> <span data-ttu-id="73281-142">Přečtěte si hello trasování zásobníku pro další informace o chybě hello a kde vytvořena v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="73281-142">Please review hello stack trace for more information about hello error and where it originated in hello code.</span></span>
* <span data-ttu-id="73281-143">Podrobnosti o výjimce: System.IO.FIleNotFoundException: Nelze načíst soubor nebo sestavení ' Microsoft.WindowsAzure.StorageClient, verze = 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35, nebo jeden z jeho závislých.</span><span class="sxs-lookup"><span data-stu-id="73281-143">Exception Details: System.IO.FIleNotFoundException: Could not load file or assembly ‘Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35’ or one of its dependencies.</span></span> <span data-ttu-id="73281-144">Hello systém nemůže najít zadaný soubor hello.</span><span class="sxs-lookup"><span data-stu-id="73281-144">hello system cannot find hello file specified.</span></span>

<span data-ttu-id="73281-145">Například:</span><span class="sxs-lookup"><span data-stu-id="73281-145">For example:</span></span>

![Chyba explicitní serveru v aplikaci '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-hello-compute-emulator"></a><span data-ttu-id="73281-147">Vyřešení problémů pomocí emulátoru služby výpočty hello</span><span class="sxs-lookup"><span data-stu-id="73281-147">Diagnose issues by using hello compute emulator</span></span>
<span data-ttu-id="73281-148">Můžete použít hello Microsoft Azure výpočetní emulátor toodiagnose a řešení problémů chybějících závislostí a chyby v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="73281-148">You can use hello Microsoft Azure compute emulator toodiagnose and troubleshoot issues of missing dependencies and web.config errors.</span></span>

<span data-ttu-id="73281-149">Nejlepších výsledků dosáhnete v pomocí této metody diagnostiky měli byste použít počítač nebo virtuální počítač, který má čistou instalaci systému Windows.</span><span class="sxs-lookup"><span data-stu-id="73281-149">For best results in using this method of diagnosis, you should use a computer or virtual machine that has a clean installation of Windows.</span></span> <span data-ttu-id="73281-150">toobest simulovat hello prostředí Azure, použijte Windows Server 2008 R2 x64.</span><span class="sxs-lookup"><span data-stu-id="73281-150">toobest simulate hello Azure environment, use Windows Server 2008 R2 x64.</span></span>

1. <span data-ttu-id="73281-151">Nainstalujte hello samostatnou verzi hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="73281-151">Install hello standalone version of hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="73281-152">Na vývojovém počítači hello sestavení projektu hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="73281-152">On hello development machine, build hello cloud service project.</span></span>
3. <span data-ttu-id="73281-153">V Průzkumníku Windows přejděte toohello bin\debug složky hello projekt cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="73281-153">In Windows Explorer, navigate toohello bin\debug folder of hello cloud service project.</span></span>
4. <span data-ttu-id="73281-154">Zkopírujte složku .csx hello a počítač toohello souboru .cscfg je, že používáte toodebug hello problémy.</span><span class="sxs-lookup"><span data-stu-id="73281-154">Copy hello .csx folder and .cscfg file toohello computer that you are using toodebug hello issues.</span></span>
5. <span data-ttu-id="73281-155">Na hello čištění počítače, otevřete okno příkazového řádku Azure SDK a zadejte `csrun.exe /devstore:start`.</span><span class="sxs-lookup"><span data-stu-id="73281-155">On hello clean machine, open an Azure SDK Command Prompt window and type `csrun.exe /devstore:start`.</span></span>
6. <span data-ttu-id="73281-156">Hello příkazového řádku, zadejte `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.</span><span class="sxs-lookup"><span data-stu-id="73281-156">At hello command prompt, type `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.</span></span>
7. <span data-ttu-id="73281-157">Když se spustí hello role, zobrazí se podrobné informace o chybě v aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="73281-157">When hello role starts, you will see detailed error information in Internet Explorer.</span></span> <span data-ttu-id="73281-158">Můžete také použít standardní Windows nástroje pro řešení potíží toofurther diagnostikovat problém hello.</span><span class="sxs-lookup"><span data-stu-id="73281-158">You can also use standard Windows troubleshooting tools toofurther diagnose hello problem.</span></span>

## <a name="diagnose-issues-by-using-intellitrace"></a><span data-ttu-id="73281-159">Diagnostikovat problémy s použitím technologie IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="73281-159">Diagnose issues by using IntelliTrace</span></span>
<span data-ttu-id="73281-160">Pro pracovní a webové role, které používají rozhraní .NET Framework 4, můžete použít [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), která je dostupná v aplikaci Microsoft Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="73281-160">For worker and web roles that use .NET Framework 4, you can use [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), which is available in Microsoft Visual Studio Enterprise.</span></span>

<span data-ttu-id="73281-161">Postupujte podle těchto kroků toodeploy hello služby s použitím technologie IntelliTrace povoleno:</span><span class="sxs-lookup"><span data-stu-id="73281-161">Follow these steps toodeploy hello service with IntelliTrace enabled:</span></span>

1. <span data-ttu-id="73281-162">Zkontrolujte, že je nainstalovaná sada Azure SDK 1.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="73281-162">Confirm that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="73281-163">Nasaďte řešení hello pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73281-163">Deploy hello solution by using Visual Studio.</span></span> <span data-ttu-id="73281-164">Během nasazení, zkontrolujte hello **povolit IntelliTrace pro role rozhraní .NET 4** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="73281-164">During deployment, check hello **Enable IntelliTrace for .NET 4 roles** check box.</span></span>
3. <span data-ttu-id="73281-165">Jakmile se spustí hello instance, otevřete hello **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="73281-165">Once hello instance starts, open hello **Server Explorer**.</span></span>
4. <span data-ttu-id="73281-166">Rozbalte hello **Azure\\cloudové služby** uzlu a najděte hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="73281-166">Expand hello **Azure\\Cloud Services** node and locate hello deployment.</span></span>
5. <span data-ttu-id="73281-167">Rozbalte hello nasazení, dokud neuvidíte hello instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="73281-167">Expand hello deployment until you see hello role instances.</span></span> <span data-ttu-id="73281-168">Klikněte pravým tlačítkem myši na jednu z instancí hello.</span><span class="sxs-lookup"><span data-stu-id="73281-168">Right-click on one of hello instances.</span></span>
6. <span data-ttu-id="73281-169">Zvolte **protokoly IntelliTrace zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="73281-169">Choose **View IntelliTrace logs**.</span></span> <span data-ttu-id="73281-170">Hello **IntelliTrace Souhrn** se otevře.</span><span class="sxs-lookup"><span data-stu-id="73281-170">hello **IntelliTrace Summary** will open.</span></span>
7. <span data-ttu-id="73281-171">Vyhledejte hello výjimky část hello souhrnu.</span><span class="sxs-lookup"><span data-stu-id="73281-171">Locate hello exceptions section of hello summary.</span></span> <span data-ttu-id="73281-172">Pokud jsou výjimky, bude hello části s názvem bez přípony **Data výjimky**.</span><span class="sxs-lookup"><span data-stu-id="73281-172">If there are exceptions, hello section will be labeled **Exception Data**.</span></span>
8. <span data-ttu-id="73281-173">Rozbalte hello **Data výjimky** a vyhledejte **System.IO.FileNotFoundException** podobné toohello následující chyby:</span><span class="sxs-lookup"><span data-stu-id="73281-173">Expand hello **Exception Data** and look for **System.IO.FileNotFoundException** errors similar toohello following:</span></span>

![Data výjimky, chybí soubor nebo sestavení](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a><span data-ttu-id="73281-175">Adresa chybějící knihovny DLL a sestavení</span><span class="sxs-lookup"><span data-stu-id="73281-175">Address missing DLLs and assemblies</span></span>
<span data-ttu-id="73281-176">tooaddress chybí chyby knihoven DLL a sestavení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="73281-176">tooaddress missing DLL and assembly errors, follow these steps:</span></span>

1. <span data-ttu-id="73281-177">Otevřete hello řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73281-177">Open hello solution in Visual Studio.</span></span>
2. <span data-ttu-id="73281-178">V **Průzkumníku řešení**, otevřete hello **odkazy** složky.</span><span class="sxs-lookup"><span data-stu-id="73281-178">In **Solution Explorer**, open hello **References** folder.</span></span>
3. <span data-ttu-id="73281-179">Klikněte na určené v hello chyby sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="73281-179">Click hello assembly identified in hello error.</span></span>
4. <span data-ttu-id="73281-180">V hello **vlastnosti** podokně vyhledejte **místní kopie vlastnosti** a nastavte hodnotu hello příliš**True**.</span><span class="sxs-lookup"><span data-stu-id="73281-180">In hello **Properties** pane, locate **Copy Local property** and set hello value too**True**.</span></span>
5. <span data-ttu-id="73281-181">Znovu nasaďte hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="73281-181">Redeploy hello cloud service.</span></span>

<span data-ttu-id="73281-182">Jakmile si ověříte, že byly odstraněny všechny chyby, můžete nasadit hello služby bez kontroly, zda text hello **povolit IntelliTrace pro role rozhraní .NET 4** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="73281-182">Once you have verified that all errors have been corrected, you can deploy hello service without checking hello **Enable IntelliTrace for .NET 4 roles** check box.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73281-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73281-183">Next steps</span></span>
<span data-ttu-id="73281-184">Zobrazení [řešení potíží s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="73281-184">View more [troubleshooting articles](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="73281-185">v tématu jak problémy tootroubleshoot cloudové služby role pomocí Azure PaaS počítače diagnostická data toolearn [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="73281-185">toolearn how tootroubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, see [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
