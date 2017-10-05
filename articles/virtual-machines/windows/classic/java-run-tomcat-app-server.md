---
title: "Aplikační server Java spustit na virtuálním počítači Azure classic | Microsoft Docs"
description: "Tento kurz používá prostředky, které jsou vytvořené pomocí modelu nasazení classic a ukazuje, jak vytvořit Windows virtuální počítač a nakonfigurovat jej pro spuštění aplikačního serveru Apache Tomcat."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
ms.openlocfilehash: 6e02f42613808bcb13c0057e9f8fcc1c02273e77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a><span data-ttu-id="08eb1-103">Jak spouštět javový aplikační server na virtuálním počítači vytvořeném pomocí modelu klasického nasazení</span><span class="sxs-lookup"><span data-stu-id="08eb1-103">How to run a Java application server on a virtual machine created with the classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="08eb1-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="08eb1-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="08eb1-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="08eb1-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="08eb1-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="08eb1-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="08eb1-107">Šablony Resource Manageru k nasazení webové aplikace Java 8 a Tomcat, najdete v části [zde](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span><span class="sxs-lookup"><span data-stu-id="08eb1-107">For a Resource Manager template to deploy a webapp with Java 8 and Tomcat, see [here](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span></span>

<span data-ttu-id="08eb1-108">S Azure můžete použít virtuální počítač k poskytování funkcí serveru.</span><span class="sxs-lookup"><span data-stu-id="08eb1-108">With Azure, you can use a virtual machine to provide server capabilities.</span></span> <span data-ttu-id="08eb1-109">Jako příklad lze nakonfigurovat virtuální počítač spuštěný v Azure k hostování aplikačního serveru Java, jako je například Apache Tomcat.</span><span class="sxs-lookup"><span data-stu-id="08eb1-109">As an example, a virtual machine running on Azure can be configured to host a Java application server, such as Apache Tomcat.</span></span>

<span data-ttu-id="08eb1-110">Po dokončení tohoto průvodce, budete mít představu o tom, jak vytvořit virtuální počítač běží na Azure a nakonfigurovat jej pro spuštění aplikační server Java.</span><span class="sxs-lookup"><span data-stu-id="08eb1-110">After completing this guide, you will have an understanding of how to create a virtual machine running on Azure and configure it to run a Java application server.</span></span> <span data-ttu-id="08eb1-111">Bude další a provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="08eb1-111">You will learn and perform the following tasks:</span></span>

* <span data-ttu-id="08eb1-112">Jak vytvořit virtuální počítač, který má Java Development Kit (JDK) už nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="08eb1-112">How to create a virtual machine that has a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="08eb1-113">Jak vzdáleně Přihlaste se k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="08eb1-113">How to remotely sign in to your virtual machine.</span></span>
* <span data-ttu-id="08eb1-114">Postup instalace aplikačního serveru Java – Apache Tomcat – na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="08eb1-114">How to install a Java application server--Apache Tomcat--on your virtual machine.</span></span>
* <span data-ttu-id="08eb1-115">Postup vytvoření koncového bodu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="08eb1-115">How to create an endpoint for your virtual machine.</span></span>
* <span data-ttu-id="08eb1-116">Postup otevření portu v bráně firewall pro aplikační server.</span><span class="sxs-lookup"><span data-stu-id="08eb1-116">How to open a port in the firewall for your application server.</span></span>

<span data-ttu-id="08eb1-117">Instalace byla dokončena. výsledkem Tomcat, které jsou spuštěny na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="08eb1-117">The completed installation results in Tomcat running on a virtual machine.</span></span>

![Virtuální počítač se systémem Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a><span data-ttu-id="08eb1-119">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="08eb1-119">To create a virtual machine</span></span>
1. <span data-ttu-id="08eb1-120">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08eb1-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="08eb1-121">Klikněte na tlačítko **nový**, klikněte na tlačítko **výpočetní**, pak klikněte na tlačítko **zobrazit všechny** v **vybrané aplikace**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-121">Click **New**, click **Compute**, then click **See all** in the **Featured apps**.</span></span>
3. <span data-ttu-id="08eb1-122">Klikněte na tlačítko **JDK**, klikněte na tlačítko **JDK 8** v **JDK** podokně.</span><span class="sxs-lookup"><span data-stu-id="08eb1-122">Click **JDK**, click **JDK 8** in the **JDK** pane.</span></span>  
   <span data-ttu-id="08eb1-123">Virtuální počítač bitové kopie, které podporují **JDK 6** a **JDK 7** jsou k dispozici, pokud máte starší aplikace, které nejsou připravené ke spuštění v JDK 8.</span><span class="sxs-lookup"><span data-stu-id="08eb1-123">Virtual machine images that support **JDK 6** and **JDK 7** are available if you have legacy applications that are not ready to run in JDK 8.</span></span>
4. <span data-ttu-id="08eb1-124">V podokně JDK 8 vyberte **Classic**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-124">In the JDK 8 pane, select **Classic**, then click **Create**.</span></span>
5. <span data-ttu-id="08eb1-125">V **Základy** okno:</span><span class="sxs-lookup"><span data-stu-id="08eb1-125">In the **Basics** blade:</span></span>
   1. <span data-ttu-id="08eb1-126">Zadejte název pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="08eb1-126">Specify a name for the virtual machine.</span></span>
   2. <span data-ttu-id="08eb1-127">Zadejte název pro správce v **uživatelské jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="08eb1-127">Enter a name for the administrator in the **User Name** field.</span></span> <span data-ttu-id="08eb1-128">Mějte na paměti, tento název a přiřazené heslo, který následuje v poli Další.</span><span class="sxs-lookup"><span data-stu-id="08eb1-128">Remember this name and the associated password that follows in the next field.</span></span> <span data-ttu-id="08eb1-129">Musíte je při vzdálené přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="08eb1-129">You need them when you remotely sign in to the virtual machine.</span></span>
   3. <span data-ttu-id="08eb1-130">Zadejte heslo do **nové heslo** pole a znovu ho zadat **potvrzení hesla** pole.</span><span class="sxs-lookup"><span data-stu-id="08eb1-130">Enter a password in the **New password** field, and reenter it in the **Confirm password** field.</span></span> <span data-ttu-id="08eb1-131">Je toto heslo pro účet správce.</span><span class="sxs-lookup"><span data-stu-id="08eb1-131">This password is for the Administrator account.</span></span>
   4. <span data-ttu-id="08eb1-132">Vyberte odpovídající **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-132">Select the appropriate **Subscription**.</span></span>
   5. <span data-ttu-id="08eb1-133">Pro **skupiny prostředků**, klikněte na tlačítko **vytvořit nový** a zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="08eb1-133">For the **Resource group**, click **Create new** and enter the name of a new resource group.</span></span> <span data-ttu-id="08eb1-134">Nebo klikněte na tlačítko **použít existující** a vyberte jednu z dostupných skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="08eb1-134">Or, click **Use existing** and select one of the available resource groups.</span></span>
   6. <span data-ttu-id="08eb1-135">Vyberte umístění, kde virtuální počítač je umístěn, jako například **jihu USA**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-135">Select a location where the virtual machine resides, such as **South Central US**.</span></span>
6. <span data-ttu-id="08eb1-136">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-136">Click **Next**.</span></span>
7. <span data-ttu-id="08eb1-137">V **velikost bitové kopie virtuálního počítače** vyberte **A1 standardní** nebo jiné příslušné bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="08eb1-137">In the **Virtual machine image size** blade, select **A1 Standard** or another appropriate image.</span></span>
8. <span data-ttu-id="08eb1-138">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-138">Click **Select**.</span></span>

9. <span data-ttu-id="08eb1-139">V **nastavení** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-139">In the **Settings** blade, click **OK**.</span></span> <span data-ttu-id="08eb1-140">Můžete použít výchozí hodnoty, které poskytuje Azure.</span><span class="sxs-lookup"><span data-stu-id="08eb1-140">You can use the default values provided by Azure.</span></span>  
10. <span data-ttu-id="08eb1-141">V **Souhrn** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-141">In the **Summary** blade, click **OK**.</span></span>

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a><span data-ttu-id="08eb1-142">Chcete-li vzdáleně Přihlaste se k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="08eb1-142">To remotely sign in to your virtual machine</span></span>
1. <span data-ttu-id="08eb1-143">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08eb1-143">Log on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="08eb1-144">Klikněte na tlačítko **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-144">Click **Virtual machines (classic)**.</span></span> <span data-ttu-id="08eb1-145">V případě potřeby klikněte na tlačítko **další služby** v levém dolním rohu pod kategorií služby.</span><span class="sxs-lookup"><span data-stu-id="08eb1-145">If needed, click **More services** at the bottom left corner under the service categories.</span></span> <span data-ttu-id="08eb1-146">**Virtuálních počítačů (klasické)** položka je uvedena ve **výpočetní** skupiny.</span><span class="sxs-lookup"><span data-stu-id="08eb1-146">The **Virtual machines (classic)** entry is listed in the **Compute** group.</span></span>
3. <span data-ttu-id="08eb1-147">Klikněte na název virtuálního počítače, který chcete použít pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="08eb1-147">Click the name of the virtual machine that you want to sign in to.</span></span>
4. <span data-ttu-id="08eb1-148">Po spuštění virtuálního počítače, nabídce v horní části podokna umožňuje připojení.</span><span class="sxs-lookup"><span data-stu-id="08eb1-148">After the virtual machine has started, a menu at the top of the pane allows connections.</span></span>
5. <span data-ttu-id="08eb1-149">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-149">Click **Connect**.</span></span>
6. <span data-ttu-id="08eb1-150">Reagujete na výzvy podle potřeby pro připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="08eb1-150">Respond to the prompts as needed to connect to the virtual machine.</span></span> <span data-ttu-id="08eb1-151">Obvykle můžete uložit nebo otevření souboru .rdp, který obsahuje podrobnosti o připojení.</span><span class="sxs-lookup"><span data-stu-id="08eb1-151">Typically, you save or open the .rdp file that contains the connection details.</span></span> <span data-ttu-id="08eb1-152">Můžete chtít zkopírujte adresu url: port jako poslední část první řádek souboru RDP a vložte ho v aplikaci vzdálené přihlášení.</span><span class="sxs-lookup"><span data-stu-id="08eb1-152">You might have to copy the url:port as the last part of the first line of the .rdp file and paste it in a remote sign-in application.</span></span>

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a><span data-ttu-id="08eb1-153">Chcete-li nainstalovat aplikační server Java na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="08eb1-153">To install a Java application server on your virtual machine</span></span>
<span data-ttu-id="08eb1-154">Aplikační server Java můžete zkopírovat do virtuálního počítače, nebo můžete nainstalovat aplikační server Java prostřednictvím Instalační program.</span><span class="sxs-lookup"><span data-stu-id="08eb1-154">You can copy a Java application server to your virtual machine, or you can install a Java application server through an installer.</span></span>

<span data-ttu-id="08eb1-155">Tento kurz používá k instalaci Tomcat jako aplikační server Java.</span><span class="sxs-lookup"><span data-stu-id="08eb1-155">This tutorial uses Tomcat as the Java application server to install.</span></span>

1. <span data-ttu-id="08eb1-156">Když jsou přihlášení k virtuálnímu počítači, otevřete relaci prohlížeče na [Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span><span class="sxs-lookup"><span data-stu-id="08eb1-156">When you are signed in to your virtual machine, open a browser session to [Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span></span>
2. <span data-ttu-id="08eb1-157">Dvakrát klikněte na odkaz pro **Instalační služby systému Windows 32-bit nebo 64-bit**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-157">Double-click the link for **32-bit/64-bit Windows Service Installer**.</span></span> <span data-ttu-id="08eb1-158">Když použijete tento postup, Tomcat nainstaluje jako služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="08eb1-158">By using this technique, Tomcat installs as a Windows service.</span></span>
3. <span data-ttu-id="08eb1-159">Po zobrazení výzvy vyberte spusťte instalační program.</span><span class="sxs-lookup"><span data-stu-id="08eb1-159">When prompted, choose to run the installer.</span></span>
4. <span data-ttu-id="08eb1-160">V rámci **Apache Tomcat instalace** průvodce, postupujte podle pokynů k instalaci Tomcat.</span><span class="sxs-lookup"><span data-stu-id="08eb1-160">Within the **Apache Tomcat Setup** wizard, follow the prompts to install Tomcat.</span></span> <span data-ttu-id="08eb1-161">Pro účely tohoto kurzu je v pořádku přijměte výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08eb1-161">For the purposes of this tutorial, accepting the defaults is fine.</span></span> <span data-ttu-id="08eb1-162">Když se dostanete **dokončování Průvodce instalací Apache Tomcat** dialogové okno, můžete volitelně zkontrolovat **spustit Apache Tomcat** tak, aby měl Tomcat nyní spustit.</span><span class="sxs-lookup"><span data-stu-id="08eb1-162">When you reach the **Completing the Apache Tomcat Setup Wizard** dialog box, you can optionally check **Run Apache Tomcat** to have Tomcat start now.</span></span> <span data-ttu-id="08eb1-163">Klikněte na tlačítko **Dokončit** k dokončení procesu instalace Tomcat.</span><span class="sxs-lookup"><span data-stu-id="08eb1-163">Click **Finish** to complete the Tomcat setup process.</span></span>

## <a name="to-start-tomcat"></a><span data-ttu-id="08eb1-164">Chcete-li spustit Tomcat</span><span class="sxs-lookup"><span data-stu-id="08eb1-164">To start Tomcat</span></span>

<span data-ttu-id="08eb1-165">Otevřete příkazový řádek na virtuálním počítači a spuštění příkazu můžete spustit ručně Tomcat **net&nbsp;spustit&nbsp;Tomcat8**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-165">You can manually start Tomcat by opening a command prompt on your virtual machine and running the command **net&nbsp;start&nbsp;Tomcat8**.</span></span>

<span data-ttu-id="08eb1-166">Jakmile je spuštěna Tomcat, dostanete Tomcat zadáním adresy URL <adrese http://localhost: 8080> v prohlížeči virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="08eb1-166">Once Tomcat is running, you can access Tomcat by entering the URL <http://localhost:8080> in the virtual machine's browser.</span></span>

<span data-ttu-id="08eb1-167">Tomcat spuštěna z externích počítačů najdete potřebujete vytvořit koncový bod a otevřít port.</span><span class="sxs-lookup"><span data-stu-id="08eb1-167">To see Tomcat running from external machines, you need to create an endpoint and open a port.</span></span>

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a><span data-ttu-id="08eb1-168">Chcete-li vytvořit koncový bod pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="08eb1-168">To create an endpoint for your virtual machine</span></span>
1. <span data-ttu-id="08eb1-169">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08eb1-169">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="08eb1-170">Klikněte na tlačítko **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-170">Click **Virtual machines (classic)**.</span></span>
3. <span data-ttu-id="08eb1-171">Klikněte na název virtuálního počítače, který je spuštěn aplikační server Java.</span><span class="sxs-lookup"><span data-stu-id="08eb1-171">Click the name of the virtual machine that is running your Java application server.</span></span>
4. <span data-ttu-id="08eb1-172">Klikněte na **Koncové body**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-172">Click **Endpoints**.</span></span>
5. <span data-ttu-id="08eb1-173">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-173">Click **Add**.</span></span>
6. <span data-ttu-id="08eb1-174">V **přidání koncového bodu** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="08eb1-174">In the **Add endpoint** dialog box:</span></span>
   1. <span data-ttu-id="08eb1-175">Zadejte název pro koncový bod; například **HttpIn**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-175">Specify a name for the endpoint; for example, **HttpIn**.</span></span>
   2. <span data-ttu-id="08eb1-176">Vyberte **TCP** protokolu.</span><span class="sxs-lookup"><span data-stu-id="08eb1-176">Select **TCP** for the protocol.</span></span>
   3. <span data-ttu-id="08eb1-177">Zadejte **80** pro veřejný port.</span><span class="sxs-lookup"><span data-stu-id="08eb1-177">Specify **80** for the public port.</span></span>
   4. <span data-ttu-id="08eb1-178">Zadejte **8080** pro privátní port.</span><span class="sxs-lookup"><span data-stu-id="08eb1-178">Specify **8080** for the private port.</span></span>
   5. <span data-ttu-id="08eb1-179">Vyberte **zakázané** pro plovoucí IP adresa.</span><span class="sxs-lookup"><span data-stu-id="08eb1-179">Select **Disabled** for the floating IP address.</span></span>
   6. <span data-ttu-id="08eb1-180">Ponechte seznamu řízení přístupu, jako je.</span><span class="sxs-lookup"><span data-stu-id="08eb1-180">Leave the access control list as is.</span></span>
   7. <span data-ttu-id="08eb1-181">Klikněte **OK** tlačítko a zavřete dialogové okno a vytvořte koncový bod.</span><span class="sxs-lookup"><span data-stu-id="08eb1-181">Click the **OK** button to close the dialog box and create the endpoint.</span></span>

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a><span data-ttu-id="08eb1-182">Otevření portu v bráně firewall pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="08eb1-182">To open a port in the firewall for your virtual machine</span></span>
1. <span data-ttu-id="08eb1-183">Přihlaste se k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="08eb1-183">Sign in to your virtual machine.</span></span>
2. <span data-ttu-id="08eb1-184">Klikněte na tlačítko **spuštění Windows**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-184">Click **Windows Start**.</span></span>
3. <span data-ttu-id="08eb1-185">Klikněte na tlačítko **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-185">Click **Control Panel**.</span></span>
4. <span data-ttu-id="08eb1-186">Klikněte na tlačítko **systém a zabezpečení**, klikněte na tlačítko **brány Windows Firewall**a potom klikněte na **Upřesnit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-186">Click **System and Security**, click **Windows Firewall**, and then click **Advanced Settings**.</span></span>
5. <span data-ttu-id="08eb1-187">Klikněte na tlačítko **příchozí pravidla**a potom klikněte na **nové pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-187">Click **Inbound Rules**, and then click **New Rule**.</span></span>  
   <span data-ttu-id="08eb1-188">![Nové příchozí pravidlo][NewIBRule]</span><span class="sxs-lookup"><span data-stu-id="08eb1-188">![New inbound rule][NewIBRule]</span></span>
6. <span data-ttu-id="08eb1-189">Pro **typ pravidla**, vyberte **Port**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-189">For the **Rule Type**, select **Port**, and then click **Next**.</span></span>  
   <span data-ttu-id="08eb1-190">![Nový port příchozí pravidlo][NewRulePort]</span><span class="sxs-lookup"><span data-stu-id="08eb1-190">![New inbound rule port][NewRulePort]</span></span>
7. <span data-ttu-id="08eb1-191">Na **protokol a porty** obrazovku, vyberte **TCP**, zadejte **8080** jako **konkrétní místního portu**a pak klikněte na tlačítko  **Další**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-191">On the **Protocol and Ports** screen, select **TCP**, specify **8080** as the **Specific local port**, and then click **Next**.</span></span>  
  <span data-ttu-id="08eb1-192">![Nové příchozí pravidlo][NewRuleProtocol]</span><span class="sxs-lookup"><span data-stu-id="08eb1-192">![New inbound rule ][NewRuleProtocol]</span></span>
8. <span data-ttu-id="08eb1-193">Na **akce** obrazovku, vyberte **povolit připojení**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-193">On the **Action** screen, select **Allow the connection**, and then click **Next**.</span></span>
   <span data-ttu-id="08eb1-194">![Nová akce příchozí pravidlo][NewRuleAction]</span><span class="sxs-lookup"><span data-stu-id="08eb1-194">![New inbound rule action][NewRuleAction]</span></span>
9. <span data-ttu-id="08eb1-195">Na **profil** obrazovky, ujistěte se, že **domény**, **privátní**, a **veřejné** jsou vybrané a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-195">On the **Profile** screen, ensure that **Domain**, **Private**, and **Public** are selected, and then click **Next**.</span></span>
   <span data-ttu-id="08eb1-196">![Nový profil příchozí pravidlo][NewRuleProfile]</span><span class="sxs-lookup"><span data-stu-id="08eb1-196">![New inbound rule profile][NewRuleProfile]</span></span>
10. <span data-ttu-id="08eb1-197">Na **název** obrazovky, zadejte název pravidla, jako například **HttpIn** (název pravidla nemusí odpovídat názvu koncového bodu, ale) a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-197">On the **Name** screen, specify a name for the rule, such as **HttpIn** (the rule name is not required to match the endpoint name, however), and then click **Finish**.</span></span>  
    <span data-ttu-id="08eb1-198">![Nový název příchozí pravidlo][NewRuleName]</span><span class="sxs-lookup"><span data-stu-id="08eb1-198">![New inbound rule name][NewRuleName]</span></span>

<span data-ttu-id="08eb1-199">V tomto okamžiku by měla být váš web Tomcat viditelný z externího prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="08eb1-199">At this point, your Tomcat website should be viewable from an external browser.</span></span> <span data-ttu-id="08eb1-200">V okně adresa v prohlížeči zadejte adresu URL ve formátu  **http://*vaše\_DNS\_název*. cloudapp.net**, kde ***vaše\_DNS\_název*** je název DNS, který jste zadali při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="08eb1-200">In the browser's address window, type a URL of the form **http://*your\_DNS\_name*.cloudapp.net**, where ***your\_DNS\_name*** is the DNS name you specified when you created the virtual machine.</span></span>

## <a name="application-lifecycle-considerations"></a><span data-ttu-id="08eb1-201">Aspekty životního cyklu aplikací</span><span class="sxs-lookup"><span data-stu-id="08eb1-201">Application lifecycle considerations</span></span>
* <span data-ttu-id="08eb1-202">Můžete vytvořit vlastní webový archiv aplikace (WAR) a přidejte ho do **webapps** složky.</span><span class="sxs-lookup"><span data-stu-id="08eb1-202">You could create your own web application archive (WAR) and add it to the **webapps** folder.</span></span> <span data-ttu-id="08eb1-203">Například vytvoření základní dynamického webového projektu Java služby stránky (JSP) a exportovat jako soubor WAR.</span><span class="sxs-lookup"><span data-stu-id="08eb1-203">For example, create a basic Java Service Page (JSP) dynamic web project and export it as a WAR file.</span></span> <span data-ttu-id="08eb1-204">Zkopírujte WAR Apache Tomcat **webapps** složky na virtuálním počítači, spusťte v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="08eb1-204">Next, copy the WAR to the Apache Tomcat **webapps** folder on the virtual machine, then run it in a browser.</span></span>
* <span data-ttu-id="08eb1-205">Ve výchozím nastavení při instalaci služby Tomcat, jinak je nastavená na spustit ručně.</span><span class="sxs-lookup"><span data-stu-id="08eb1-205">By default when the Tomcat service is installed, it is set to start manually.</span></span> <span data-ttu-id="08eb1-206">Můžete přepnout ji na automatické spouštění pomocí modulu snap-in služby.</span><span class="sxs-lookup"><span data-stu-id="08eb1-206">You can switch it to start automatically by using the Services snap-in.</span></span> <span data-ttu-id="08eb1-207">Spusťte modul snap-in služby kliknutím **spustit Windows**, **nástroje pro správu**a potom **služby**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-207">Start the Services snap-in by clicking **Windows Start**, **Administrative Tools**, and then **Services**.</span></span> <span data-ttu-id="08eb1-208">Dvakrát klikněte **Apache Tomcat** služby a nastavte **typ spuštění** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="08eb1-208">Double-click the **Apache Tomcat** service and set **Startup type** to **Automatic**.</span></span>

    ![Nastavení služby na automatické spouštění.][service_automatic_startup]

    <span data-ttu-id="08eb1-210">Výhodou vytvoření Tomcat automatické spuštění je, že je spuštěn při restartování virtuálního počítače (například po instalaci aktualizace softwaru, které vyžadují restartování).</span><span class="sxs-lookup"><span data-stu-id="08eb1-210">The benefit of having Tomcat start automatically is that it starts running when the virtual machine is rebooted (for example, after software updates that require a reboot are installed).</span></span>

## <a name="next-steps"></a><span data-ttu-id="08eb1-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08eb1-211">Next steps</span></span>
<span data-ttu-id="08eb1-212">Můžete informace o jiných služeb (například Azure Storage, služby service bus a SQL Database), které chcete zahrnout s vaší aplikací v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="08eb1-212">You can learn about other services (such as Azure Storage, service bus, and SQL Database) that you may want to include with your Java applications.</span></span> <span data-ttu-id="08eb1-213">Zobrazit informace, které jsou k dispozici na [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="08eb1-213">View the information available at the [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from the "To create an ednpoint for your virtual mache" 3/17/2017,
     to use the new portal.
6. In the **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In the **New endpoint details** dialog box:
-->
