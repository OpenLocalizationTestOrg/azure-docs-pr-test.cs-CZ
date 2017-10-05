---
title: "Řešení potíží s rolí, které se nepodařilo spustit | Microsoft Docs"
description: "Tady jsou některé běžné příčiny, proč nemusí podařit spustit roli cloudové služby. K dispozici jsou taky řešení těchto problémů."
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
ms.openlocfilehash: 7d956192e8b9c3688b8b6f0108bd9296f66fbd62
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a><span data-ttu-id="49c34-104">Řešení potíží s cloudové služby role, které se nepodařilo spustit</span><span class="sxs-lookup"><span data-stu-id="49c34-104">Troubleshoot Cloud Service roles that fail to start</span></span>
<span data-ttu-id="49c34-105">Tady jsou některé běžné problémy a řešení souvisejících s Azure Cloud Services rolí, které se nepodařilo spustit.</span><span class="sxs-lookup"><span data-stu-id="49c34-105">Here are some common problems and solutions related to Azure Cloud Services roles that fail to start.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a><span data-ttu-id="49c34-106">Chybějící knihovny DLL nebo závislosti</span><span class="sxs-lookup"><span data-stu-id="49c34-106">Missing DLLs or dependencies</span></span>
<span data-ttu-id="49c34-107">Reagovat role a role, které jsou prosté mezi **Probíhá inicializace**, **zaneprázdněn**, a **zastavení** stavy může být způsobeno chybějícím knihovny DLL nebo sestavení.</span><span class="sxs-lookup"><span data-stu-id="49c34-107">Unresponsive roles and roles that are cycling between **Initializing**, **Busy**, and **Stopping** states can be caused by missing DLLs or assemblies.</span></span>

<span data-ttu-id="49c34-108">Příznaky chybějící knihovny DLL nebo sestavení může být:</span><span class="sxs-lookup"><span data-stu-id="49c34-108">Symptoms of missing DLLs or assemblies can be:</span></span>

* <span data-ttu-id="49c34-109">Role instance je prosté prostřednictvím **Probíhá inicializace**, **zaneprázdněn**, a **zastavení** stavy.</span><span class="sxs-lookup"><span data-stu-id="49c34-109">Your role instance is cycling through **Initializing**, **Busy**, and **Stopping** states.</span></span>
* <span data-ttu-id="49c34-110">Role instance se přesunula do **připraven** , ale pokud přejdete k vaší webové aplikaci, stránky se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="49c34-110">Your role instance has moved to **Ready** but if you navigate to your web application, the page does not appear.</span></span>

<span data-ttu-id="49c34-111">Existuje několik doporučených metod pro příčin těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="49c34-111">There are several recommended methods for investigating these issues.</span></span>

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a><span data-ttu-id="49c34-112">Chybí knihovna DLL problémů ve webové roli diagnostiky</span><span class="sxs-lookup"><span data-stu-id="49c34-112">Diagnose missing DLL issues in a web role</span></span>
<span data-ttu-id="49c34-113">Když přejdete na web, který je nasazen ve webové role a prohlížeč zobrazí chybu serveru, který je podobný následujícímu, může to znamenat, že chybí knihovna DLL.</span><span class="sxs-lookup"><span data-stu-id="49c34-113">When you navigate to a website that is deployed in a web role, and the browser displays a server error similar to the following, it may indicate that a DLL is missing.</span></span>

![Chyba serveru v aplikaci '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a><span data-ttu-id="49c34-115">Diagnostikovat problémy vypnutím vlastní chyby</span><span class="sxs-lookup"><span data-stu-id="49c34-115">Diagnose issues by turning off custom errors</span></span>
<span data-ttu-id="49c34-116">Konfigurace souboru web.config pro webovou roli na nastavení režimu vlastní chyby na vypnutém a opětovného nasazení služby lze zobrazit podrobnější informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="49c34-116">More complete error information can be viewed by configuring the web.config for the web role to set the custom error mode to Off and redeploying the service.</span></span>

<span data-ttu-id="49c34-117">Chcete-li zobrazit podrobnější chyby bez pomocí vzdálené plochy:</span><span class="sxs-lookup"><span data-stu-id="49c34-117">To view more complete errors without using Remote Desktop:</span></span>

1. <span data-ttu-id="49c34-118">Otevřete řešení v sadě Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49c34-118">Open the solution in Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="49c34-119">V **Průzkumníku**, vyhledejte soubor web.config a otevřete ji.</span><span class="sxs-lookup"><span data-stu-id="49c34-119">In the **Solution Explorer**, locate the web.config file and open it.</span></span>
3. <span data-ttu-id="49c34-120">V souboru web.config najděte část system.web a přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="49c34-120">In the web.config file, locate the system.web section and add the following line:</span></span>

    ```xml
    <customErrors mode="Off" />
    ```
4. <span data-ttu-id="49c34-121">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="49c34-121">Save the file.</span></span>
5. <span data-ttu-id="49c34-122">Znovu zabalte a znovu nasaďte službu.</span><span class="sxs-lookup"><span data-stu-id="49c34-122">Repackage and redeploy the service.</span></span>

<span data-ttu-id="49c34-123">Jakmile služba je znovu nasazena, zobrazí se chybová zpráva s názvem chybí sestavení nebo DLL.</span><span class="sxs-lookup"><span data-stu-id="49c34-123">Once the service is redeployed, you will see an error message with the name of the missing assembly or DLL.</span></span>

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a><span data-ttu-id="49c34-124">Vyřešení problémů se zobrazením chyba vzdáleně</span><span class="sxs-lookup"><span data-stu-id="49c34-124">Diagnose issues by viewing the error remotely</span></span>
<span data-ttu-id="49c34-125">Vzdálená plocha můžete použít pro přístup k roli a vzdáleně zobrazit podrobnější informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="49c34-125">You can use Remote Desktop to access the role and view more complete error information remotely.</span></span> <span data-ttu-id="49c34-126">Chcete-li zobrazit chyby pomocí vzdálené plochy pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="49c34-126">Use the following steps to view the errors by using Remote Desktop:</span></span>

1. <span data-ttu-id="49c34-127">Zkontrolujte, zda je nainstalovaný Azure SDK 1.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="49c34-127">Ensure that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="49c34-128">Při nasazení řešení pomocí sady Visual Studio vyberte "Konfigurace připojení ke vzdálené ploše...".</span><span class="sxs-lookup"><span data-stu-id="49c34-128">During the deployment of the solution by using Visual Studio, choose to “Configure Remote Desktop connections…”.</span></span> <span data-ttu-id="49c34-129">Další informace o konfiguraci připojení vzdálené plochy najdete v tématu [pomocí vzdálené plochy s rolemi Azure](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="49c34-129">For more information on configuring the Remote Desktop connection, see [Using Remote Desktop with Azure Roles](../vs-azure-tools-remote-desktop-roles.md).</span></span>
3. <span data-ttu-id="49c34-130">V portálu Microsoft Azure classic, jakmile se zobrazí stav instance **připraven**, klikněte na jednu z instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="49c34-130">In the Microsoft Azure classic portal, once the instance shows a status of **Ready**, click one of the role instances.</span></span>
4. <span data-ttu-id="49c34-131">Klikněte na tlačítko **připojit** ikonu v **vzdáleného přístupu** oblasti pásu karet.</span><span class="sxs-lookup"><span data-stu-id="49c34-131">Click the **Connect** icon in the **Remote Access** area of the ribbon.</span></span>
5. <span data-ttu-id="49c34-132">Přihlaste se k virtuálnímu počítači pomocí přihlašovacích údajů, které se zadaly během konfigurace vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="49c34-132">Sign in to the virtual machine by using the credentials that were specified during the Remote Desktop configuration.</span></span>
6. <span data-ttu-id="49c34-133">Otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="49c34-133">Open a command window.</span></span>
7. <span data-ttu-id="49c34-134">Zadejte `IPconfig`.</span><span class="sxs-lookup"><span data-stu-id="49c34-134">Type `IPconfig`.</span></span>
8. <span data-ttu-id="49c34-135">Poznamenejte si hodnotu adresu IPV4.</span><span class="sxs-lookup"><span data-stu-id="49c34-135">Note the IPV4 Address value.</span></span>
9. <span data-ttu-id="49c34-136">Otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="49c34-136">Open Internet Explorer.</span></span>
10. <span data-ttu-id="49c34-137">Zadejte adresu a název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="49c34-137">Type the address and the name of the web application.</span></span> <span data-ttu-id="49c34-138">Například, `http://<IPV4 Address>/default.aspx`.</span><span class="sxs-lookup"><span data-stu-id="49c34-138">For example, `http://<IPV4 Address>/default.aspx`.</span></span>

<span data-ttu-id="49c34-139">Přejdete na web se teď vrátit podrobnější chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="49c34-139">Navigating to the website will now return more explicit error messages:</span></span>

* <span data-ttu-id="49c34-140">Chyba serveru v aplikaci '/'.</span><span class="sxs-lookup"><span data-stu-id="49c34-140">Server Error in '/' Application.</span></span>
* <span data-ttu-id="49c34-141">Popis: Při provádění aktuální webové žádosti došlo k neošetřené výjimce.</span><span class="sxs-lookup"><span data-stu-id="49c34-141">Description: An unhandled exception occurred during the execution of the current web request.</span></span> <span data-ttu-id="49c34-142">Přečtěte si další informace o této chybě, a místo původu v kódu trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="49c34-142">Please review the stack trace for more information about the error and where it originated in the code.</span></span>
* <span data-ttu-id="49c34-143">Podrobnosti o výjimce: System.IO.FIleNotFoundException: Nelze načíst soubor nebo sestavení ' Microsoft.WindowsAzure.StorageClient, verze = 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35, nebo jeden z jeho závislých.</span><span class="sxs-lookup"><span data-stu-id="49c34-143">Exception Details: System.IO.FIleNotFoundException: Could not load file or assembly ‘Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35’ or one of its dependencies.</span></span> <span data-ttu-id="49c34-144">Systém nemůže najít zadaný soubor.</span><span class="sxs-lookup"><span data-stu-id="49c34-144">The system cannot find the file specified.</span></span>

<span data-ttu-id="49c34-145">Například:</span><span class="sxs-lookup"><span data-stu-id="49c34-145">For example:</span></span>

![Chyba explicitní serveru v aplikaci '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a><span data-ttu-id="49c34-147">Vyřešení problémů pomocí emulátoru služby výpočty v</span><span class="sxs-lookup"><span data-stu-id="49c34-147">Diagnose issues by using the compute emulator</span></span>
<span data-ttu-id="49c34-148">Emulátoru služby výpočty v Microsoft Azure můžete použít při diagnostice a řešení potíží se chybějící závislosti a chyby v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="49c34-148">You can use the Microsoft Azure compute emulator to diagnose and troubleshoot issues of missing dependencies and web.config errors.</span></span>

<span data-ttu-id="49c34-149">Nejlepších výsledků dosáhnete v pomocí této metody diagnostiky měli byste použít počítač nebo virtuální počítač, který má čistou instalaci systému Windows.</span><span class="sxs-lookup"><span data-stu-id="49c34-149">For best results in using this method of diagnosis, you should use a computer or virtual machine that has a clean installation of Windows.</span></span> <span data-ttu-id="49c34-150">Chcete-li nejlépe simulaci prostředí Azure, použijte Windows Server 2008 R2 x64.</span><span class="sxs-lookup"><span data-stu-id="49c34-150">To best simulate the Azure environment, use Windows Server 2008 R2 x64.</span></span>

1. <span data-ttu-id="49c34-151">Nainstalujte samostatnou verzi [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="49c34-151">Install the standalone version of the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="49c34-152">Na vývojovém počítači sestavení projektu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="49c34-152">On the development machine, build the cloud service project.</span></span>
3. <span data-ttu-id="49c34-153">V Průzkumníku Windows přejděte do složky bin\debug projektu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="49c34-153">In Windows Explorer, navigate to the bin\debug folder of the cloud service project.</span></span>
4. <span data-ttu-id="49c34-154">Zkopírujte soubor složky a .cscfg .csx k počítači, který používáte k ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="49c34-154">Copy the .csx folder and .cscfg file to the computer that you are using to debug the issues.</span></span>
5. <span data-ttu-id="49c34-155">Na vyčištění počítači otevřete okno příkazového řádku Azure SDK a zadejte `csrun.exe /devstore:start`.</span><span class="sxs-lookup"><span data-stu-id="49c34-155">On the clean machine, open an Azure SDK Command Prompt window and type `csrun.exe /devstore:start`.</span></span>
6. <span data-ttu-id="49c34-156">Na příkazovém řádku zadejte `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.</span><span class="sxs-lookup"><span data-stu-id="49c34-156">At the command prompt, type `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.</span></span>
7. <span data-ttu-id="49c34-157">Když se spustí roli, zobrazí se podrobné informace o chybě v aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="49c34-157">When the role starts, you will see detailed error information in Internet Explorer.</span></span> <span data-ttu-id="49c34-158">Můžete také standardní Windows nástroje pro řešení potíží při další diagnostice problému.</span><span class="sxs-lookup"><span data-stu-id="49c34-158">You can also use standard Windows troubleshooting tools to further diagnose the problem.</span></span>

## <a name="diagnose-issues-by-using-intellitrace"></a><span data-ttu-id="49c34-159">Diagnostikovat problémy s použitím technologie IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="49c34-159">Diagnose issues by using IntelliTrace</span></span>
<span data-ttu-id="49c34-160">Pro pracovní a webové role, které používají rozhraní .NET Framework 4, můžete použít [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), která je dostupná v aplikaci Microsoft Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="49c34-160">For worker and web roles that use .NET Framework 4, you can use [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), which is available in Microsoft Visual Studio Enterprise.</span></span>

<span data-ttu-id="49c34-161">Postupujte podle těchto kroků nasadíte službu s použitím technologie IntelliTrace povoleno:</span><span class="sxs-lookup"><span data-stu-id="49c34-161">Follow these steps to deploy the service with IntelliTrace enabled:</span></span>

1. <span data-ttu-id="49c34-162">Zkontrolujte, že je nainstalovaná sada Azure SDK 1.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="49c34-162">Confirm that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="49c34-163">Nasazení řešení pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49c34-163">Deploy the solution by using Visual Studio.</span></span> <span data-ttu-id="49c34-164">Během nasazení, zkontrolujte **povolit IntelliTrace pro role rozhraní .NET 4** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="49c34-164">During deployment, check the **Enable IntelliTrace for .NET 4 roles** check box.</span></span>
3. <span data-ttu-id="49c34-165">Jakmile se spustí instanci, otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="49c34-165">Once the instance starts, open the **Server Explorer**.</span></span>
4. <span data-ttu-id="49c34-166">Rozbalte **Azure\\cloudové služby** uzlu a najděte nasazení.</span><span class="sxs-lookup"><span data-stu-id="49c34-166">Expand the **Azure\\Cloud Services** node and locate the deployment.</span></span>
5. <span data-ttu-id="49c34-167">Rozbalte možnost nasazení, dokud neuvidíte instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="49c34-167">Expand the deployment until you see the role instances.</span></span> <span data-ttu-id="49c34-168">Klikněte pravým tlačítkem myši na jednu z instancí.</span><span class="sxs-lookup"><span data-stu-id="49c34-168">Right-click on one of the instances.</span></span>
6. <span data-ttu-id="49c34-169">Zvolte **protokoly IntelliTrace zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="49c34-169">Choose **View IntelliTrace logs**.</span></span> <span data-ttu-id="49c34-170">**IntelliTrace Souhrn** se otevře.</span><span class="sxs-lookup"><span data-stu-id="49c34-170">The **IntelliTrace Summary** will open.</span></span>
7. <span data-ttu-id="49c34-171">Vyhledejte část výjimky souhrn.</span><span class="sxs-lookup"><span data-stu-id="49c34-171">Locate the exceptions section of the summary.</span></span> <span data-ttu-id="49c34-172">Pokud jsou výjimky, bude v části s názvem bez přípony **Data výjimky**.</span><span class="sxs-lookup"><span data-stu-id="49c34-172">If there are exceptions, the section will be labeled **Exception Data**.</span></span>
8. <span data-ttu-id="49c34-173">Rozbalte **Data výjimky** a vyhledejte **System.IO.FileNotFoundException** chyby podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="49c34-173">Expand the **Exception Data** and look for **System.IO.FileNotFoundException** errors similar to the following:</span></span>

![Data výjimky, chybí soubor nebo sestavení](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a><span data-ttu-id="49c34-175">Adresa chybějící knihovny DLL a sestavení</span><span class="sxs-lookup"><span data-stu-id="49c34-175">Address missing DLLs and assemblies</span></span>
<span data-ttu-id="49c34-176">Chcete-li vyřešit chybějící DLL a chyby sestavení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="49c34-176">To address missing DLL and assembly errors, follow these steps:</span></span>

1. <span data-ttu-id="49c34-177">Otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49c34-177">Open the solution in Visual Studio.</span></span>
2. <span data-ttu-id="49c34-178">V **Průzkumníku řešení**, otevřete **odkazy** složky.</span><span class="sxs-lookup"><span data-stu-id="49c34-178">In **Solution Explorer**, open the **References** folder.</span></span>
3. <span data-ttu-id="49c34-179">Klikněte na tlačítko sestavení identifikované v chybě.</span><span class="sxs-lookup"><span data-stu-id="49c34-179">Click the assembly identified in the error.</span></span>
4. <span data-ttu-id="49c34-180">V **vlastnosti** podokně vyhledejte **místní kopie vlastnosti** a nastavte hodnotu na **True**.</span><span class="sxs-lookup"><span data-stu-id="49c34-180">In the **Properties** pane, locate **Copy Local property** and set the value to **True**.</span></span>
5. <span data-ttu-id="49c34-181">Znovu nasaďte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="49c34-181">Redeploy the cloud service.</span></span>

<span data-ttu-id="49c34-182">Jakmile si ověříte, že byly odstraněny všechny chyby, můžete nasadit službu bez kontroly, zda **povolit IntelliTrace pro role rozhraní .NET 4** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="49c34-182">Once you have verified that all errors have been corrected, you can deploy the service without checking the **Enable IntelliTrace for .NET 4 roles** check box.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49c34-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49c34-183">Next steps</span></span>
<span data-ttu-id="49c34-184">Zobrazení [řešení potíží s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="49c34-184">View more [troubleshooting articles](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="49c34-185">Informace o řešení problémů role služby cloudu pomocí Azure PaaS dat diagnostiky počítače, v tématu [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="49c34-185">To learn how to troubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, see [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
