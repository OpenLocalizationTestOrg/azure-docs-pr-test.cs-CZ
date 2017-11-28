---
title: "Aplikační server Java aaaRun na virtuálním počítači Azure classic | Microsoft Docs"
description: "Tento kurz používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a ukazuje, jak toocreate Windows virtuální počítač a nakonfigurujte ji toorun Apache Tomcat aplikačního serveru."
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
ms.openlocfilehash: 2d9f586c9f628d3738522b320996b95b078d7454
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-java-application-server-on-a-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="06eb4-103">Jak toorun aplikačního serveru Java na virtuálním počítači vytvořit pomocí modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="06eb4-103">How toorun a Java application server on a virtual machine created with hello classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="06eb4-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="06eb4-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="06eb4-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="06eb4-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="06eb4-107">Toodeploy webapp s Java 8 a Tomcat šablony Resource Manageru najdete v části [zde](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span><span class="sxs-lookup"><span data-stu-id="06eb4-107">For a Resource Manager template toodeploy a webapp with Java 8 and Tomcat, see [here](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span></span>

<span data-ttu-id="06eb4-108">S Azure můžete použít virtuální počítač tooprovide serveru možnosti.</span><span class="sxs-lookup"><span data-stu-id="06eb4-108">With Azure, you can use a virtual machine tooprovide server capabilities.</span></span> <span data-ttu-id="06eb4-109">Jako příklad může být virtuální počítač běží na Azure nakonfigurovaný toohost aplikačního serveru Java, jako je například Apache Tomcat.</span><span class="sxs-lookup"><span data-stu-id="06eb4-109">As an example, a virtual machine running on Azure can be configured toohost a Java application server, such as Apache Tomcat.</span></span>

<span data-ttu-id="06eb4-110">Po dokončení této příručce, bude mít pochopení toho, jak toocreate virtuální počítače spuštěné v Azure a nakonfigurujte ji toorun aplikační server Java.</span><span class="sxs-lookup"><span data-stu-id="06eb4-110">After completing this guide, you will have an understanding of how toocreate a virtual machine running on Azure and configure it toorun a Java application server.</span></span> <span data-ttu-id="06eb4-111">Bude informace a proveďte hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="06eb4-111">You will learn and perform hello following tasks:</span></span>

* <span data-ttu-id="06eb4-112">Jak toocreate virtuální počítač, který má Java Development Kit (JDK) už nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="06eb4-112">How toocreate a virtual machine that has a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="06eb4-113">Jak tooremotely přihlásit tooyour virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06eb4-113">How tooremotely sign in tooyour virtual machine.</span></span>
* <span data-ttu-id="06eb4-114">Jak tooinstall aplikačního serveru Java – Apache Tomcat – na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="06eb4-114">How tooinstall a Java application server--Apache Tomcat--on your virtual machine.</span></span>
* <span data-ttu-id="06eb4-115">Jak toocreate koncového bodu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="06eb4-115">How toocreate an endpoint for your virtual machine.</span></span>
* <span data-ttu-id="06eb4-116">Jak tooopen port v hello brány firewall pro aplikační server.</span><span class="sxs-lookup"><span data-stu-id="06eb4-116">How tooopen a port in hello firewall for your application server.</span></span>

<span data-ttu-id="06eb4-117">Hello dokončit výsledky instalace v Tomcat, které jsou spuštěny na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="06eb4-117">hello completed installation results in Tomcat running on a virtual machine.</span></span>

![Virtuální počítač se systémem Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a><span data-ttu-id="06eb4-119">toocreate virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="06eb4-119">toocreate a virtual machine</span></span>
1. <span data-ttu-id="06eb4-120">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="06eb4-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="06eb4-121">Klikněte na tlačítko **nový**, klikněte na tlačítko **výpočetní**, pak klikněte na tlačítko **zobrazit všechny** v hello **vybrané aplikace**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-121">Click **New**, click **Compute**, then click **See all** in hello **Featured apps**.</span></span>
3. <span data-ttu-id="06eb4-122">Klikněte na tlačítko **JDK**, klikněte na tlačítko **JDK 8** v hello **JDK** podokně.</span><span class="sxs-lookup"><span data-stu-id="06eb4-122">Click **JDK**, click **JDK 8** in hello **JDK** pane.</span></span>  
   <span data-ttu-id="06eb4-123">Virtuální počítač bitové kopie, které podporují **JDK 6** a **JDK 7** jsou k dispozici, pokud máte starší aplikace, které nejsou připravené toorun 8 JDK.</span><span class="sxs-lookup"><span data-stu-id="06eb4-123">Virtual machine images that support **JDK 6** and **JDK 7** are available if you have legacy applications that are not ready toorun in JDK 8.</span></span>
4. <span data-ttu-id="06eb4-124">V podokně hello JDK 8 vyberte **Classic**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-124">In hello JDK 8 pane, select **Classic**, then click **Create**.</span></span>
5. <span data-ttu-id="06eb4-125">V hello **Základy** okno:</span><span class="sxs-lookup"><span data-stu-id="06eb4-125">In hello **Basics** blade:</span></span>
   1. <span data-ttu-id="06eb4-126">Zadejte název pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-126">Specify a name for hello virtual machine.</span></span>
   2. <span data-ttu-id="06eb4-127">Zadejte název pro správce hello v hello **uživatelské jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="06eb4-127">Enter a name for hello administrator in hello **User Name** field.</span></span> <span data-ttu-id="06eb4-128">Mějte na paměti, tento název a přiřazené heslo, který následuje další pole hello hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-128">Remember this name and hello associated password that follows in hello next field.</span></span> <span data-ttu-id="06eb4-129">Musíte je při vzdálené přihlášení toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06eb4-129">You need them when you remotely sign in toohello virtual machine.</span></span>
   3. <span data-ttu-id="06eb4-130">Zadejte heslo do hello **nové heslo** pole a znovu ho zadejte do hello **potvrzení hesla** pole.</span><span class="sxs-lookup"><span data-stu-id="06eb4-130">Enter a password in hello **New password** field, and reenter it in hello **Confirm password** field.</span></span> <span data-ttu-id="06eb4-131">Je toto heslo pro účet správce hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-131">This password is for hello Administrator account.</span></span>
   4. <span data-ttu-id="06eb4-132">Vyberte odpovídající hello **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-132">Select hello appropriate **Subscription**.</span></span>
   5. <span data-ttu-id="06eb4-133">Pro hello **skupiny prostředků**, klikněte na tlačítko **vytvořit nový** a zadejte název hello novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="06eb4-133">For hello **Resource group**, click **Create new** and enter hello name of a new resource group.</span></span> <span data-ttu-id="06eb4-134">Nebo klikněte na tlačítko **použít existující** a vyberte jednu z hello dostupných skupin zdrojů.</span><span class="sxs-lookup"><span data-stu-id="06eb4-134">Or, click **Use existing** and select one of hello available resource groups.</span></span>
   6. <span data-ttu-id="06eb4-135">Vyberte umístění, kde hello virtuální počítač nachází, jako například **jihu USA**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-135">Select a location where hello virtual machine resides, such as **South Central US**.</span></span>
6. <span data-ttu-id="06eb4-136">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-136">Click **Next**.</span></span>
7. <span data-ttu-id="06eb4-137">V hello **velikost bitové kopie virtuálního počítače** vyberte **A1 standardní** nebo jiné příslušné bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="06eb4-137">In hello **Virtual machine image size** blade, select **A1 Standard** or another appropriate image.</span></span>
8. <span data-ttu-id="06eb4-138">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-138">Click **Select**.</span></span>

9. <span data-ttu-id="06eb4-139">V hello **nastavení** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-139">In hello **Settings** blade, click **OK**.</span></span> <span data-ttu-id="06eb4-140">Můžete použít výchozí hodnoty hello poskytovaný platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="06eb4-140">You can use hello default values provided by Azure.</span></span>  
10. <span data-ttu-id="06eb4-141">V hello **Souhrn** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-141">In hello **Summary** blade, click **OK**.</span></span>

## <a name="tooremotely-sign-in-tooyour-virtual-machine"></a><span data-ttu-id="06eb4-142">tooremotely přihlášení tooyour virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="06eb4-142">tooremotely sign in tooyour virtual machine</span></span>
1. <span data-ttu-id="06eb4-143">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="06eb4-143">Log on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="06eb4-144">Klikněte na tlačítko **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-144">Click **Virtual machines (classic)**.</span></span> <span data-ttu-id="06eb4-145">V případě potřeby klikněte na tlačítko **další služby** na hello levém dolním rohu pod kategorií služby hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-145">If needed, click **More services** at hello bottom left corner under hello service categories.</span></span> <span data-ttu-id="06eb4-146">Hello **virtuálních počítačů (klasické)** položka je uvedena v hello **výpočetní** skupiny.</span><span class="sxs-lookup"><span data-stu-id="06eb4-146">hello **Virtual machines (classic)** entry is listed in hello **Compute** group.</span></span>
3. <span data-ttu-id="06eb4-147">Klikněte na název hello hello virtuálního počítače, který chcete toosign v.</span><span class="sxs-lookup"><span data-stu-id="06eb4-147">Click hello name of hello virtual machine that you want toosign in to.</span></span>
4. <span data-ttu-id="06eb4-148">Po spuštění virtuálního počítače hello nabídky hello horní části podokna hello umožňuje připojení.</span><span class="sxs-lookup"><span data-stu-id="06eb4-148">After hello virtual machine has started, a menu at hello top of hello pane allows connections.</span></span>
5. <span data-ttu-id="06eb4-149">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-149">Click **Connect**.</span></span>
6. <span data-ttu-id="06eb4-150">Odpověď výzvy toohello jako potřebné tooconnect toohello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="06eb4-150">Respond toohello prompts as needed tooconnect toohello virtual machine.</span></span> <span data-ttu-id="06eb4-151">Obvykle můžete uložit nebo otevřete soubor RDP hello, který obsahuje podrobnosti připojení hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-151">Typically, you save or open hello .rdp file that contains hello connection details.</span></span> <span data-ttu-id="06eb4-152">Můžete mít toocopy hello adresy url: port jako hello poslední část hello první řádek souboru .rdp hello a vložte ji do vzdálené aplikace přihlášení.</span><span class="sxs-lookup"><span data-stu-id="06eb4-152">You might have toocopy hello url:port as hello last part of hello first line of hello .rdp file and paste it in a remote sign-in application.</span></span>

## <a name="tooinstall-a-java-application-server-on-your-virtual-machine"></a><span data-ttu-id="06eb4-153">tooinstall aplikačního serveru Java na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="06eb4-153">tooinstall a Java application server on your virtual machine</span></span>
<span data-ttu-id="06eb4-154">Můžete zkopírovat virtuální počítač Java aplikace server tooyour, nebo můžete nainstalovat aplikační server Java prostřednictvím Instalační program.</span><span class="sxs-lookup"><span data-stu-id="06eb4-154">You can copy a Java application server tooyour virtual machine, or you can install a Java application server through an installer.</span></span>

<span data-ttu-id="06eb4-155">Tento kurz používá jako hello Java aplikace server tooinstall Tomcat.</span><span class="sxs-lookup"><span data-stu-id="06eb4-155">This tutorial uses Tomcat as hello Java application server tooinstall.</span></span>

1. <span data-ttu-id="06eb4-156">Když jsou přihlášení tooyour virtuální počítač, otevřete relaci prohlížeče příliš[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span><span class="sxs-lookup"><span data-stu-id="06eb4-156">When you are signed in tooyour virtual machine, open a browser session too[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span></span>
2. <span data-ttu-id="06eb4-157">Dvakrát klikněte na odkaz hello **Instalační služby systému Windows 32-bit nebo 64-bit**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-157">Double-click hello link for **32-bit/64-bit Windows Service Installer**.</span></span> <span data-ttu-id="06eb4-158">Když použijete tento postup, Tomcat nainstaluje jako služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="06eb4-158">By using this technique, Tomcat installs as a Windows service.</span></span>
3. <span data-ttu-id="06eb4-159">Po zobrazení výzvy vyberte toorun hello Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="06eb4-159">When prompted, choose toorun hello installer.</span></span>
4. <span data-ttu-id="06eb4-160">V rámci hello **Apache Tomcat instalace** průvodce, postupujte podle hello vyzve k zadání tooinstall Tomcat.</span><span class="sxs-lookup"><span data-stu-id="06eb4-160">Within hello **Apache Tomcat Setup** wizard, follow hello prompts tooinstall Tomcat.</span></span> <span data-ttu-id="06eb4-161">Pro účely tohoto kurzu hello je v pořádku přijetí hello výchozího nastavení.</span><span class="sxs-lookup"><span data-stu-id="06eb4-161">For hello purposes of this tutorial, accepting hello defaults is fine.</span></span> <span data-ttu-id="06eb4-162">Když se dostanete hello **hello dokončení Průvodce instalací Apache Tomcat** dialogové okno, můžete volitelně zkontrolovat **spustit Apache Tomcat** toohave Tomcat spustit nyní.</span><span class="sxs-lookup"><span data-stu-id="06eb4-162">When you reach hello **Completing hello Apache Tomcat Setup Wizard** dialog box, you can optionally check **Run Apache Tomcat** toohave Tomcat start now.</span></span> <span data-ttu-id="06eb4-163">Klikněte na tlačítko **Dokončit** toocomplete hello Tomcat procesu instalace.</span><span class="sxs-lookup"><span data-stu-id="06eb4-163">Click **Finish** toocomplete hello Tomcat setup process.</span></span>

## <a name="toostart-tomcat"></a><span data-ttu-id="06eb4-164">toostart Tomcat</span><span class="sxs-lookup"><span data-stu-id="06eb4-164">toostart Tomcat</span></span>

<span data-ttu-id="06eb4-165">Tomcat můžete spustit ručně tak, že otevřete příkazový řádek na virtuální počítač a spuštěný příkaz hello **net&nbsp;spustit&nbsp;Tomcat8**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-165">You can manually start Tomcat by opening a command prompt on your virtual machine and running hello command **net&nbsp;start&nbsp;Tomcat8**.</span></span>

<span data-ttu-id="06eb4-166">Jakmile je spuštěna Tomcat, dostanete Tomcat zadáním adresy URL hello <adrese http://localhost: 8080> v prohlížeči hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06eb4-166">Once Tomcat is running, you can access Tomcat by entering hello URL <http://localhost:8080> in hello virtual machine's browser.</span></span>

<span data-ttu-id="06eb4-167">toosee Tomcat spuštěna z externích počítačů, třeba toocreate koncový bod a otevřít port.</span><span class="sxs-lookup"><span data-stu-id="06eb4-167">toosee Tomcat running from external machines, you need toocreate an endpoint and open a port.</span></span>

## <a name="toocreate-an-endpoint-for-your-virtual-machine"></a><span data-ttu-id="06eb4-168">toocreate koncového bodu pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="06eb4-168">toocreate an endpoint for your virtual machine</span></span>
1. <span data-ttu-id="06eb4-169">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="06eb4-169">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="06eb4-170">Klikněte na tlačítko **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-170">Click **Virtual machines (classic)**.</span></span>
3. <span data-ttu-id="06eb4-171">Klikněte na název hello hello virtuálního počítače, který je spuštěn aplikační server Java.</span><span class="sxs-lookup"><span data-stu-id="06eb4-171">Click hello name of hello virtual machine that is running your Java application server.</span></span>
4. <span data-ttu-id="06eb4-172">Klikněte na **Koncové body**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-172">Click **Endpoints**.</span></span>
5. <span data-ttu-id="06eb4-173">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-173">Click **Add**.</span></span>
6. <span data-ttu-id="06eb4-174">V hello **přidání koncového bodu** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="06eb4-174">In hello **Add endpoint** dialog box:</span></span>
   1. <span data-ttu-id="06eb4-175">Zadejte název pro koncový bod hello; například **HttpIn**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-175">Specify a name for hello endpoint; for example, **HttpIn**.</span></span>
   2. <span data-ttu-id="06eb4-176">Vyberte **TCP** pro protokol hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-176">Select **TCP** for hello protocol.</span></span>
   3. <span data-ttu-id="06eb4-177">Zadejte **80** pro veřejný port hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-177">Specify **80** for hello public port.</span></span>
   4. <span data-ttu-id="06eb4-178">Zadejte **8080** pro privátní port hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-178">Specify **8080** for hello private port.</span></span>
   5. <span data-ttu-id="06eb4-179">Vyberte **zakázané** pro hello plovoucí IP adresa.</span><span class="sxs-lookup"><span data-stu-id="06eb4-179">Select **Disabled** for hello floating IP address.</span></span>
   6. <span data-ttu-id="06eb4-180">Ponechte hello seznam řízení přístupu podle je.</span><span class="sxs-lookup"><span data-stu-id="06eb4-180">Leave hello access control list as is.</span></span>
   7. <span data-ttu-id="06eb4-181">Klikněte na tlačítko hello **OK** tlačítko dialogové okno hello tooclose a vytvořit koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-181">Click hello **OK** button tooclose hello dialog box and create hello endpoint.</span></span>

## <a name="tooopen-a-port-in-hello-firewall-for-your-virtual-machine"></a><span data-ttu-id="06eb4-182">tooopen port v bráně firewall hello pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="06eb4-182">tooopen a port in hello firewall for your virtual machine</span></span>
1. <span data-ttu-id="06eb4-183">Přihlaste se tooyour virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06eb4-183">Sign in tooyour virtual machine.</span></span>
2. <span data-ttu-id="06eb4-184">Klikněte na tlačítko **spuštění Windows**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-184">Click **Windows Start**.</span></span>
3. <span data-ttu-id="06eb4-185">Klikněte na tlačítko **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-185">Click **Control Panel**.</span></span>
4. <span data-ttu-id="06eb4-186">Klikněte na tlačítko **systém a zabezpečení**, klikněte na tlačítko **brány Windows Firewall**a potom klikněte na **Upřesnit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-186">Click **System and Security**, click **Windows Firewall**, and then click **Advanced Settings**.</span></span>
5. <span data-ttu-id="06eb4-187">Klikněte na tlačítko **příchozí pravidla**a potom klikněte na **nové pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-187">Click **Inbound Rules**, and then click **New Rule**.</span></span>  
   <span data-ttu-id="06eb4-188">![Nové příchozí pravidlo][NewIBRule]</span><span class="sxs-lookup"><span data-stu-id="06eb4-188">![New inbound rule][NewIBRule]</span></span>
6. <span data-ttu-id="06eb4-189">Pro hello **typ pravidla**, vyberte **Port**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-189">For hello **Rule Type**, select **Port**, and then click **Next**.</span></span>  
   <span data-ttu-id="06eb4-190">![Nový port příchozí pravidlo][NewRulePort]</span><span class="sxs-lookup"><span data-stu-id="06eb4-190">![New inbound rule port][NewRulePort]</span></span>
7. <span data-ttu-id="06eb4-191">Na hello **protokol a porty** obrazovku, vyberte **TCP**, zadejte **8080** jako hello **konkrétní místního portu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-191">On hello **Protocol and Ports** screen, select **TCP**, specify **8080** as hello **Specific local port**, and then click **Next**.</span></span>  
  <span data-ttu-id="06eb4-192">![Nové příchozí pravidlo][NewRuleProtocol]</span><span class="sxs-lookup"><span data-stu-id="06eb4-192">![New inbound rule ][NewRuleProtocol]</span></span>
8. <span data-ttu-id="06eb4-193">Na hello **akce** obrazovku, vyberte **povolit připojení hello**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-193">On hello **Action** screen, select **Allow hello connection**, and then click **Next**.</span></span>
   <span data-ttu-id="06eb4-194">![Nová akce příchozí pravidlo][NewRuleAction]</span><span class="sxs-lookup"><span data-stu-id="06eb4-194">![New inbound rule action][NewRuleAction]</span></span>
9. <span data-ttu-id="06eb4-195">Na hello **profil** obrazovky, ujistěte se, že **domény**, **privátní**, a **veřejné** jsou vybrané a pak klikněte na tlačítko **další** .</span><span class="sxs-lookup"><span data-stu-id="06eb4-195">On hello **Profile** screen, ensure that **Domain**, **Private**, and **Public** are selected, and then click **Next**.</span></span>
   <span data-ttu-id="06eb4-196">![Nový profil příchozí pravidlo][NewRuleProfile]</span><span class="sxs-lookup"><span data-stu-id="06eb4-196">![New inbound rule profile][NewRuleProfile]</span></span>
10. <span data-ttu-id="06eb4-197">Na hello **název** obrazovky, zadejte název pravidla hello, například **HttpIn** (název pravidla hello není název koncového bodu vyžaduje toomatch hello, ale) a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-197">On hello **Name** screen, specify a name for hello rule, such as **HttpIn** (hello rule name is not required toomatch hello endpoint name, however), and then click **Finish**.</span></span>  
    <span data-ttu-id="06eb4-198">![Nový název příchozí pravidlo][NewRuleName]</span><span class="sxs-lookup"><span data-stu-id="06eb4-198">![New inbound rule name][NewRuleName]</span></span>

<span data-ttu-id="06eb4-199">V tomto okamžiku by měla být váš web Tomcat viditelný z externího prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="06eb4-199">At this point, your Tomcat website should be viewable from an external browser.</span></span> <span data-ttu-id="06eb4-200">V okně prohlížeče hello adresu, zadejte adresu URL ve formátu hello  **http://*vaše\_DNS\_název*. cloudapp.net**, kde ***vaše\_DNS\_název*** je název DNS hello jste zadali při vytvoření hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06eb4-200">In hello browser's address window, type a URL of hello form **http://*your\_DNS\_name*.cloudapp.net**, where ***your\_DNS\_name*** is hello DNS name you specified when you created hello virtual machine.</span></span>

## <a name="application-lifecycle-considerations"></a><span data-ttu-id="06eb4-201">Aspekty životního cyklu aplikací</span><span class="sxs-lookup"><span data-stu-id="06eb4-201">Application lifecycle considerations</span></span>
* <span data-ttu-id="06eb4-202">Můžete vytvořit vlastní webový archiv aplikace (WAR) a přidejte ho toohello **webapps** složky.</span><span class="sxs-lookup"><span data-stu-id="06eb4-202">You could create your own web application archive (WAR) and add it toohello **webapps** folder.</span></span> <span data-ttu-id="06eb4-203">Například vytvoření základní dynamického webového projektu Java služby stránky (JSP) a exportovat jako soubor WAR.</span><span class="sxs-lookup"><span data-stu-id="06eb4-203">For example, create a basic Java Service Page (JSP) dynamic web project and export it as a WAR file.</span></span> <span data-ttu-id="06eb4-204">Zkopírujte hello WAR toohello Apache Tomcat **webapps** složky na hello virtuálního počítače, spusťte v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="06eb4-204">Next, copy hello WAR toohello Apache Tomcat **webapps** folder on hello virtual machine, then run it in a browser.</span></span>
* <span data-ttu-id="06eb4-205">Ve výchozím nastavení při instalaci hello Tomcat služby bude nastaveno toostart ručně.</span><span class="sxs-lookup"><span data-stu-id="06eb4-205">By default when hello Tomcat service is installed, it is set toostart manually.</span></span> <span data-ttu-id="06eb4-206">Můžete přepnout tak toostart automaticky pomocí modulu snap-in služby hello.</span><span class="sxs-lookup"><span data-stu-id="06eb4-206">You can switch it toostart automatically by using hello Services snap-in.</span></span> <span data-ttu-id="06eb4-207">Spusťte modul snap-in služby hello kliknutím **spustit Windows**, **nástroje pro správu**a potom **služby**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-207">Start hello Services snap-in by clicking **Windows Start**, **Administrative Tools**, and then **Services**.</span></span> <span data-ttu-id="06eb4-208">Klikněte dvakrát na hello **Apache Tomcat** služby a nastavte **typ spuštění** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="06eb4-208">Double-click hello **Apache Tomcat** service and set **Startup type** too**Automatic**.</span></span>

    ![Nastavení služby toostart automaticky][service_automatic_startup]

    <span data-ttu-id="06eb4-210">Hello výhodou s Tomcat automatické spuštění je, že je spuštěn při hello virtuální počítač po restartu (například po instalaci aktualizace softwaru, které vyžadují restartování).</span><span class="sxs-lookup"><span data-stu-id="06eb4-210">hello benefit of having Tomcat start automatically is that it starts running when hello virtual machine is rebooted (for example, after software updates that require a reboot are installed).</span></span>

## <a name="next-steps"></a><span data-ttu-id="06eb4-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06eb4-211">Next steps</span></span>
<span data-ttu-id="06eb4-212">Informace o jiných služeb (například Azure Storage, služby service bus a SQL Database), které může být vhodné najdete tooinclude s vaší aplikací v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="06eb4-212">You can learn about other services (such as Azure Storage, service bus, and SQL Database) that you may want tooinclude with your Java applications.</span></span> <span data-ttu-id="06eb4-213">Zobrazení informací o hello k dispozici v hello [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="06eb4-213">View hello information available at hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from hello "toocreate an ednpoint for your virtual mache" 3/17/2017,
     toouse hello new portal.
6. In hello **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In hello **New endpoint details** dialog box:
-->
