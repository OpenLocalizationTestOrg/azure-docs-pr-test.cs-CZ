---
title: "aplikace Java aaaCompute náročných na virtuálním počítači | Microsoft Docs"
description: "Zjistěte, jak se dá sledovat toocreate Azure virtuální počítač, který spouští aplikaci Java náročné, který ho jiná aplikace Java."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 02a198802a8d78bd444cd5a9197a78cb94f48e3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-compute-intensive-task-in-java-on-a-virtual-machine"></a><span data-ttu-id="fa493-103">Jak toorun výpočetně náročných úloh v jazyce Java ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="fa493-103">How toorun a compute-intensive task in Java on a virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="fa493-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fa493-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fa493-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="fa493-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="fa493-107">S Azure můžete použít virtuální počítač toohandle náročné úlohy.</span><span class="sxs-lookup"><span data-stu-id="fa493-107">With Azure, you can use a virtual machine toohandle compute-intensive tasks.</span></span> <span data-ttu-id="fa493-108">Virtuální počítač můžete například zpracování úlohy a poskytovat výsledky tooclient počítače nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa493-108">For example, a virtual machine can handle tasks and deliver results tooclient machines or mobile applications.</span></span> <span data-ttu-id="fa493-109">Po přečtení tohoto článku, budete mít představu o tom, jak toocreate virtuální počítač, který spouští aplikaci Java náročné, který se dá sledovat jinou aplikací Java.</span><span class="sxs-lookup"><span data-stu-id="fa493-109">After reading this article, you will have an understanding of how toocreate a virtual machine that runs a compute-intensive Java application that can be monitored by another Java application.</span></span>

<span data-ttu-id="fa493-110">Tento kurz předpokládá, že víte, jak toocreate konzolové aplikace Java, můžete importovat aplikaci Java tooyour knihovny a může generovat Java archivu (JAR).</span><span class="sxs-lookup"><span data-stu-id="fa493-110">This tutorial assumes you know how toocreate Java console applications, can import libraries tooyour Java application, and can generate a Java archive (JAR).</span></span> <span data-ttu-id="fa493-111">Předpokládají se žádné znalosti ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="fa493-111">No knowledge of Microsoft Azure is assumed.</span></span>

<span data-ttu-id="fa493-112">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="fa493-112">You will learn:</span></span>

* <span data-ttu-id="fa493-113">Jak toocreate virtuální počítač s Java Development Kit (JDK) už nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="fa493-113">How toocreate a virtual machine with a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="fa493-114">Jak tooremotely přihlásit tooyour virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa493-114">How tooremotely log in tooyour virtual machine.</span></span>
* <span data-ttu-id="fa493-115">Jak toocreate služby sběrnici oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fa493-115">How toocreate a service bus namespace.</span></span>
* <span data-ttu-id="fa493-116">Jak toocreate aplikaci Java, která provede náročné úlohy.</span><span class="sxs-lookup"><span data-stu-id="fa493-116">How toocreate a Java application that performs a compute-intensive task.</span></span>
* <span data-ttu-id="fa493-117">Jak toocreate aplikaci Java, která monitoruje hello průběh úlohy náročné hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-117">How toocreate a Java application that monitors hello progress of hello compute-intensive task.</span></span>
* <span data-ttu-id="fa493-118">Jak toorun hello aplikací v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="fa493-118">How toorun hello Java applications.</span></span>
* <span data-ttu-id="fa493-119">Jak toostop hello aplikací v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="fa493-119">How toostop hello Java applications.</span></span>

<span data-ttu-id="fa493-120">V tomto kurzu použije hello Traveling prodejce problém pro hello výpočetně náročných úloh.</span><span class="sxs-lookup"><span data-stu-id="fa493-120">This tutorial will use hello Traveling Salesman Problem for hello compute-intensive task.</span></span> <span data-ttu-id="fa493-121">Hello tady je příklad hello Java aplikace spuštěné hello náročné úlohy.</span><span class="sxs-lookup"><span data-stu-id="fa493-121">hello following is an example of hello Java application running hello compute-intensive task.</span></span>

![Řešitel problém Traveling prodejce][solver_output]

<span data-ttu-id="fa493-123">Hello následuje příklad hello Java aplikace monitorování hello výpočetně náročných úloh.</span><span class="sxs-lookup"><span data-stu-id="fa493-123">hello following is an example of hello Java application monitoring hello compute-intensive task.</span></span>

![Problém Traveling prodejce klienta][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a><span data-ttu-id="fa493-125">toocreate virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fa493-125">toocreate a virtual machine</span></span>
1. <span data-ttu-id="fa493-126">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fa493-126">Log in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="fa493-127">Klikněte na tlačítko **nový**, klikněte na tlačítko **výpočetní**, klikněte na tlačítko **virtuálního počítače**a potom klikněte na **z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="fa493-127">Click **New**, click **Compute**, click **Virtual machine**, and then click **From Gallery**.</span></span>
3. <span data-ttu-id="fa493-128">V hello **vyberte bitovou kopii virtuálního počítače** dialogové okno, vyberte **JDK 7 Windows serveru 2012**.</span><span class="sxs-lookup"><span data-stu-id="fa493-128">In hello **Virtual machine image select** dialog box, select **JDK 7 Windows Server 2012**.</span></span>
   <span data-ttu-id="fa493-129">Všimněte si, že **JDK 6 systému Windows Server 2012** je k dispozici v případě, že máte starší aplikace, které ještě nejsou připraveny toorun v JDK 7.</span><span class="sxs-lookup"><span data-stu-id="fa493-129">Note that **JDK 6 Windows Server 2012** is available in case you have legacy applications that are not yet ready toorun in JDK 7.</span></span>
4. <span data-ttu-id="fa493-130">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fa493-130">Click **Next**.</span></span>
5. <span data-ttu-id="fa493-131">V hello **konfigurace virtuálního počítače** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="fa493-131">In hello **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="fa493-132">Zadejte název pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-132">Specify a name for hello virtual machine.</span></span>
   2. <span data-ttu-id="fa493-133">Zadejte velikost toouse hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa493-133">Specify hello size toouse for hello virtual machine.</span></span>
   3. <span data-ttu-id="fa493-134">Zadejte název pro správce hello v hello **uživatelské jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="fa493-134">Enter a name for hello administrator in hello **User Name** field.</span></span> <span data-ttu-id="fa493-135">Název a hello heslo, které budou vedle zadejte si zapamatujte, je budou používat při vzdálené přihlášení toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa493-135">Remember this name and hello password you will enter next, you will use them when you remotely log in toohello virtual machine.</span></span>
   4. <span data-ttu-id="fa493-136">Zadejte heslo do hello **nové heslo** pole a zadejte jej znovu v hello **potvrdit** pole.</span><span class="sxs-lookup"><span data-stu-id="fa493-136">Enter a password in hello **New password** field, and re-enter it in hello **Confirm** field.</span></span> <span data-ttu-id="fa493-137">Toto je heslo účtu správce hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-137">This is hello Administrator account password.</span></span>
   5. <span data-ttu-id="fa493-138">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fa493-138">Click **Next**.</span></span>
6. <span data-ttu-id="fa493-139">V hello Další **konfigurace virtuálního počítače** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="fa493-139">In hello next **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="fa493-140">Pro **Cloudová služba**, použít výchozí hello **vytvořte novou cloudovou službu**.</span><span class="sxs-lookup"><span data-stu-id="fa493-140">For **Cloud service**, use hello default **Create a new cloud service**.</span></span>
   2. <span data-ttu-id="fa493-141">Hello hodnotu **název cloudové služby DNS** musí být jedinečný v rámci cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="fa493-141">hello value for **Cloud service DNS name** must be unique across cloudapp.net.</span></span> <span data-ttu-id="fa493-142">V případě potřeby upravte tuto hodnotu tak, že Azure označuje, že je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fa493-142">If needed, modify this value so that Azure indicates it is unique.</span></span>
   3. <span data-ttu-id="fa493-143">Zadejte oblast, skupiny vztahů nebo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fa493-143">Specify a region, affinity group, or virtual network.</span></span> <span data-ttu-id="fa493-144">Pro účely tohoto kurzu, zadejte v oblasti **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="fa493-144">For purposes of this tutorial, specify a region such as **West US**.</span></span>
   4. <span data-ttu-id="fa493-145">Pro **účet úložiště**, vyberte **použít účet úložiště automaticky generované**.</span><span class="sxs-lookup"><span data-stu-id="fa493-145">For **Storage Account**, select **Use an automatically generated storage account**.</span></span>
   5. <span data-ttu-id="fa493-146">Pro **sadu dostupnosti**, vyberte **(None)**.</span><span class="sxs-lookup"><span data-stu-id="fa493-146">For **Availability Set**, select **(None)**.</span></span>
   6. <span data-ttu-id="fa493-147">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fa493-147">Click **Next**.</span></span>
7. <span data-ttu-id="fa493-148">V posledním hello **konfigurace virtuálního počítače** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="fa493-148">In hello final **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="fa493-149">Přijměte hello výchozí koncový bod položky.</span><span class="sxs-lookup"><span data-stu-id="fa493-149">Accept hello default endpoint entries.</span></span>
   2. <span data-ttu-id="fa493-150">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="fa493-150">Click **Complete**.</span></span>

## <a name="tooremotely-log-in-tooyour-virtual-machine"></a><span data-ttu-id="fa493-151">tooremotely přihlášení tooyour virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fa493-151">tooremotely log in tooyour virtual machine</span></span>
1. <span data-ttu-id="fa493-152">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fa493-152">Log on toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="fa493-153">Klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="fa493-153">Click **Virtual machines**.</span></span>
3. <span data-ttu-id="fa493-154">Klikněte na název hello hello virtuálního počítače, který chcete toolog v.</span><span class="sxs-lookup"><span data-stu-id="fa493-154">Click hello name of hello virtual machine that you want toolog in to.</span></span>
4. <span data-ttu-id="fa493-155">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="fa493-155">Click **Connect**.</span></span>
5. <span data-ttu-id="fa493-156">Odpověď výzvy toohello jako potřebné tooconnect toohello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fa493-156">Respond toohello prompts as needed tooconnect toohello virtual machine.</span></span> <span data-ttu-id="fa493-157">Po zobrazení výzvy k hello správce jména a hesla, použijte hello hodnoty, které jste zadali při vytváření hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa493-157">When prompted for hello administrator name and password, use hello values that you provided when you created hello virtual machine.</span></span>

<span data-ttu-id="fa493-158">Všimněte si, že hello funkce Azure Service Bus vyžaduje toobe certifikát Baltimore CyberTrust Root hello nainstalované v rámci vašeho prostředí JRE **cacerts** uložit.</span><span class="sxs-lookup"><span data-stu-id="fa493-158">Note that hello Azure Service Bus functionality requires hello Baltimore CyberTrust Root certificate toobe installed as part of your JRE's **cacerts** store.</span></span> <span data-ttu-id="fa493-159">Tento certifikát je automaticky součástí hello Java Runtime Environment (JRE) používané v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fa493-159">This certificate is automatically included in hello Java Runtime Environment (JRE) used by this tutorial.</span></span> <span data-ttu-id="fa493-160">Pokud jste tento certifikát ve vašem prostředí JRE **cacerts** úložiště najdete v tématu [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java] [ add_ca_cert] informace o přidání (a také informace o zobrazení hello certifikáty v úložišti cacerts).</span><span class="sxs-lookup"><span data-stu-id="fa493-160">If you do not have this certificate in your JRE **cacerts** store, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert] for information on adding it (as well as information on viewing hello certificates in your cacerts store).</span></span>

## <a name="how-toocreate-a-service-bus-namespace"></a><span data-ttu-id="fa493-161">Jak toocreate služby sběrnici obor názvů</span><span class="sxs-lookup"><span data-stu-id="fa493-161">How toocreate a service bus namespace</span></span>
<span data-ttu-id="fa493-162">fronty toobegin pomocí služby Service Bus v Azure, musíte nejdřív vytvořit obor názvů služby.</span><span class="sxs-lookup"><span data-stu-id="fa493-162">toobegin using Service Bus queues in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="fa493-163">Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa493-163">A service namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="fa493-164">toocreate oboru názvů služby:</span><span class="sxs-lookup"><span data-stu-id="fa493-164">toocreate a service namespace:</span></span>

1. <span data-ttu-id="fa493-165">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fa493-165">Log on toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="fa493-166">V levém navigačním podokně hello hello portál Azure classic, klikněte na tlačítko **Service Bus, řízení přístupu a ukládání do mezipaměti**.</span><span class="sxs-lookup"><span data-stu-id="fa493-166">In hello lower-left navigation pane of hello Azure classic portal, click **Service Bus, Access Control & Caching**.</span></span>
3. <span data-ttu-id="fa493-167">V levém podokně hello hello portál Azure classic, klikněte na hello **Service Bus** uzel a potom klikněte na hello **nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fa493-167">In hello upper-left pane of hello Azure classic portal, click hello **Service Bus** node, and then click hello **New** button.</span></span>  
   <span data-ttu-id="fa493-168">![Snímek obrazovky uzel Service Bus][svc_bus_node]</span><span class="sxs-lookup"><span data-stu-id="fa493-168">![Service Bus Node screenshot][svc_bus_node]</span></span>
4. <span data-ttu-id="fa493-169">V hello **vytvoření nové služby Namespace** dialogovém okně zadejte **Namespace**, a potom klikněte na toomake se, že je jedinečný, **zkontrolovat dostupnost** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fa493-169">In hello **Create a new Service Namespace** dialog box, enter a **Namespace**, and then toomake sure that it is unique, click the **Check Availability** button.</span></span>  
   <span data-ttu-id="fa493-170">![Vytvořit nový Namespace snímek][create_namespace]</span><span class="sxs-lookup"><span data-stu-id="fa493-170">![Create a New Namespace screenshot][create_namespace]</span></span>
5. <span data-ttu-id="fa493-171">Ověřte název oboru názvů hello je k dispozici, vyberte zemi nebo oblast, ve kterém by měl být hostován vašeho oboru názvů a klikněte na položku hello **vytvořit Namespace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fa493-171">After making sure hello namespace name is available, choose the country or region in which your namespace should be hosted, and then click hello **Create Namespace** button.</span></span>  
   
   <span data-ttu-id="fa493-172">vytvořený obor názvů Hello se potom zobrazí v hello portál Azure classic a tooactivate chvíli trvá.</span><span class="sxs-lookup"><span data-stu-id="fa493-172">hello namespace you created will then appear in hello Azure classic portal and takes a moment tooactivate.</span></span> <span data-ttu-id="fa493-173">Počkejte, dokud je stav hello **Active** než budete pokračovat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-173">Wait until hello status is **Active** before continuing with hello next step.</span></span>

## <a name="obtain-hello-default-management-credentials-for-hello-namespace"></a><span data-ttu-id="fa493-174">Získání hello výchozí pověření pro správu oboru názvů hello</span><span class="sxs-lookup"><span data-stu-id="fa493-174">Obtain hello Default Management Credentials for hello namespace</span></span>
<span data-ttu-id="fa493-175">Operace správy tooperform pořadí, jako je například vytváření fronty, na hello nový obor názvů je nutné tooobtain hello správy pověření pro obor názvů.</span><span class="sxs-lookup"><span data-stu-id="fa493-175">In order tooperform management operations, such as creating a queue, on hello new namespace, you need tooobtain hello management credentials for the namespace.</span></span>

1. <span data-ttu-id="fa493-176">V levém navigačním podokně hello, klikněte na tlačítko hello **Service Bus** uzel k zobrazení hello seznam dostupných oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="fa493-176">In hello left navigation pane, click hello **Service Bus** node to display hello list of available namespaces.</span></span>
   <span data-ttu-id="fa493-177">![K dispozici snímek obory názvů][avail_namespaces]</span><span class="sxs-lookup"><span data-stu-id="fa493-177">![Available Namespaces screenshot][avail_namespaces]</span></span>
2. <span data-ttu-id="fa493-178">Vyberte obor názvů hello, který jste právě vytvořili, z v zobrazeném seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-178">Select hello namespace you just created from hello list shown.</span></span>
   <span data-ttu-id="fa493-179">![Snímek obrazovky seznamu Namespace][namespace_list]</span><span class="sxs-lookup"><span data-stu-id="fa493-179">![Namespace List screenshot][namespace_list]</span></span>
3. <span data-ttu-id="fa493-180">Hello pravém **vlastnosti** podokně zobrazí hello vlastnosti pro nový obor názvů.</span><span class="sxs-lookup"><span data-stu-id="fa493-180">hello right-hand **Properties** pane lists hello properties for the new namespace.</span></span>
   <span data-ttu-id="fa493-181">![Snímek obrazovky podokna Vlastnosti][properties_pane]</span><span class="sxs-lookup"><span data-stu-id="fa493-181">![Properties Pane screenshot][properties_pane]</span></span>
4. <span data-ttu-id="fa493-182">Hello **výchozí klíč** skryt.</span><span class="sxs-lookup"><span data-stu-id="fa493-182">hello **Default Key** is hidden.</span></span> <span data-ttu-id="fa493-183">Klikněte na tlačítko hello **zobrazení** tlačítko toodisplay hello zabezpečovací pověření.</span><span class="sxs-lookup"><span data-stu-id="fa493-183">Click hello **View** button toodisplay hello security credentials.</span></span>
   <span data-ttu-id="fa493-184">![Výchozí klíč – snímek obrazovky][default_key]</span><span class="sxs-lookup"><span data-stu-id="fa493-184">![Default Key screenshot][default_key]</span></span>
5. <span data-ttu-id="fa493-185">Poznamenejte si hello **výchozí vystavitele** a hello **výchozí klíč** jako použije tyto informace níže tooperform operací s oborem názvů.</span><span class="sxs-lookup"><span data-stu-id="fa493-185">Make a note of hello **Default Issuer** and hello **Default Key** as you will use this information below tooperform operations with the namespace.</span></span>

## <a name="how-toocreate-a-java-application-that-performs-a-compute-intensive-task"></a><span data-ttu-id="fa493-186">Jak toocreate aplikaci Java, která provede náročné úlohy</span><span class="sxs-lookup"><span data-stu-id="fa493-186">How toocreate a Java application that performs a compute-intensive task</span></span>
1. <span data-ttu-id="fa493-187">Na počítači pro vývoj (který nemá toobe hello virtuální počítač, který jste vytvořili), stažení hello [Azure SDK pro jazyk Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="fa493-187">On your development machine (which does not have toobe hello virtual machine that you created), download hello [Azure SDK for Java](https://azure.microsoft.com/develop/java/).</span></span>
2. <span data-ttu-id="fa493-188">Vytvořte konzolovou aplikaci Java pomocí hello ukázkový kód na konci hello v této části.</span><span class="sxs-lookup"><span data-stu-id="fa493-188">Create a Java console application using hello example code at hello end of this section.</span></span> <span data-ttu-id="fa493-189">V tomto kurzu použijeme **TSPSolver.java** jako název souboru hello Java.</span><span class="sxs-lookup"><span data-stu-id="fa493-189">In this tutorial, we'll use **TSPSolver.java** as hello Java file name.</span></span> <span data-ttu-id="fa493-190">Upravit hello **vaše\_služby\_sběrnice\_obor názvů**, **vaše\_služby\_sběrnice\_vlastníka**a **vaše\_služby\_sběrnice\_klíč** toouse zástupné symboly služby service bus **obor názvů**, **výchozí vystavitele** a  **Výchozí klíč** hodnoty v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="fa493-190">Modify hello **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders toouse your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
3. <span data-ttu-id="fa493-191">Po kódování, export hello aplikace tooa spustitelného Java archivu (JAR) a balíček hello požadované knihovny do hello generované JAR.</span><span class="sxs-lookup"><span data-stu-id="fa493-191">After coding, export hello application tooa runnable Java archive (JAR), and package hello required libraries into hello generated JAR.</span></span> <span data-ttu-id="fa493-192">V tomto kurzu použijeme **TSPSolver.jar** jako název JAR hello vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="fa493-192">In this tutorial, we'll use **TSPSolver.jar** as hello generated JAR name.</span></span>

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often tooprovide an update toohello console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as hello default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing toooccur other than creating hello queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing toooccur other than deleting hello queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume hello value passed in is hello number of cities toosolve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-toocreate-a-java-application-that-monitors-hello-progress-of-hello-compute-intensive-task"></a><span data-ttu-id="fa493-193">Jak toocreate aplikaci Java, která monitoruje hello průběh úlohy náročné hello</span><span class="sxs-lookup"><span data-stu-id="fa493-193">How toocreate a Java application that monitors hello progress of hello compute-intensive task</span></span>
1. <span data-ttu-id="fa493-194">Na vývojovém počítači vytvořte konzolovou aplikaci Java pomocí hello ukázkový kód na konci hello v této části.</span><span class="sxs-lookup"><span data-stu-id="fa493-194">On your development machine, create a Java console application using hello example code at hello end of this section.</span></span> <span data-ttu-id="fa493-195">V tomto kurzu použijeme **TSPClient.java** jako název souboru hello Java.</span><span class="sxs-lookup"><span data-stu-id="fa493-195">In this tutorial, we'll use **TSPClient.java** as hello Java file name.</span></span> <span data-ttu-id="fa493-196">Jak je uvedeno výše, upravte hello **vaše\_služby\_sběrnice\_obor názvů**, **vaše\_služby\_sběrnice\_vlastníka**, a **vaše\_služby\_sběrnice\_klíč** toouse zástupné symboly služby service bus **obor názvů**, **výchozí vystavitele**a **výchozí klíč** hodnoty v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="fa493-196">As shown earlier, modify hello **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders toouse your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
2. <span data-ttu-id="fa493-197">Export tooa aplikace hello knihovny do hello generované JAR vyžaduje spustitelného JAR a balíček hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-197">Export hello application tooa runnable JAR, and package hello required libraries into hello generated JAR.</span></span> <span data-ttu-id="fa493-198">V tomto kurzu použijeme **TSPClient.jar** jako název JAR hello vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="fa493-198">In this tutorial, we'll use **TSPClient.jar** as hello generated JAR name.</span></span>

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as hello default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display hello queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing toooccur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // hello queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-toorun-hello-java-applications"></a><span data-ttu-id="fa493-199">Jak toorun hello aplikací v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="fa493-199">How toorun hello Java applications</span></span>
<span data-ttu-id="fa493-200">Spuštění aplikace hello náročné, první toocreate hello fronty a potom toosolve hello cestě Saleseman problém, který přidá hello aktuální nejlepší trasy toohello frontou service bus.</span><span class="sxs-lookup"><span data-stu-id="fa493-200">Run hello compute-intensive application, first toocreate hello queue, then toosolve hello Traveling Saleseman Problem, which will add hello current best route toohello service bus queue.</span></span> <span data-ttu-id="fa493-201">Při hello náročné aplikace je spuštěná (nebo později) spusťte hello klienta toodisplay výsledky z hello frontou service bus.</span><span class="sxs-lookup"><span data-stu-id="fa493-201">While hello compute-intensive application is running (or afterwards), run hello client toodisplay results from hello service bus queue.</span></span>

### <a name="toorun-hello-compute-intensive-application"></a><span data-ttu-id="fa493-202">toorun hello náročné aplikace</span><span class="sxs-lookup"><span data-stu-id="fa493-202">toorun hello compute-intensive application</span></span>
1. <span data-ttu-id="fa493-203">Přihlaste se tooyour virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa493-203">Log on tooyour virtual machine.</span></span>
2. <span data-ttu-id="fa493-204">Vytvořte složku, kde budete spouštět aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa493-204">Create a folder where you will run your application.</span></span> <span data-ttu-id="fa493-205">Například **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="fa493-205">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="fa493-206">Kopírování **TSPSolver.jar** příliš**c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="fa493-206">Copy **TSPSolver.jar** too**c:\TSP**,</span></span>
4. <span data-ttu-id="fa493-207">Vytvořte soubor s názvem **c:\TSP\cities.txt** s hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="fa493-207">Create a file named **c:\TSP\cities.txt** with hello following contents.</span></span>
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. <span data-ttu-id="fa493-208">Na příkazovém řádku změňte adresáře tooc:\TSP.</span><span class="sxs-lookup"><span data-stu-id="fa493-208">At a command prompt, change directories tooc:\TSP.</span></span>
6. <span data-ttu-id="fa493-209">Zkontrolujte, zda je složka bin hello JRE v proměnné prostředí PATH hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-209">Ensure hello JRE's bin folder is in hello PATH environment variable.</span></span>
7. <span data-ttu-id="fa493-210">Před spuštěním hello TSP solver permutací, budete potřebovat frontou service bus toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-210">You'll need toocreate hello service bus queue before you run hello TSP solver permutations.</span></span> <span data-ttu-id="fa493-211">Spusťte následující příkaz toocreate hello service bus fronty hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-211">Run hello following command toocreate hello service bus queue.</span></span>
   
        java -jar TSPSolver.jar createqueue
8. <span data-ttu-id="fa493-212">Je teď vytvořená hello fronty, můžete spustit hello TSP solver permutací.</span><span class="sxs-lookup"><span data-stu-id="fa493-212">Now that hello queue is created, you can run hello TSP solver permutations.</span></span> <span data-ttu-id="fa493-213">Například spusťte následující příkaz toorun hello solver pro 8 města hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-213">For example, run hello following command toorun hello solver for 8 cities.</span></span>
   
        java -jar TSPSolver.jar 8
   
   <span data-ttu-id="fa493-214">Pokud nezadáte číslo, bude spuštěna pro 10 měst.</span><span class="sxs-lookup"><span data-stu-id="fa493-214">If you don't specify a number, it will run for 10 cities.</span></span> <span data-ttu-id="fa493-215">Jako hello solver vyhledá aktuální nejkratší tras, bude je přidat toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="fa493-215">As hello solver finds current shortest routes, it will add them toohello queue.</span></span>

> [!NOTE]
> <span data-ttu-id="fa493-216">číslo, kterou zadáte, se spustí hello delší hello solver text hello Hello větší.</span><span class="sxs-lookup"><span data-stu-id="fa493-216">hello larger hello number that you specify, hello longer hello solver will run.</span></span> <span data-ttu-id="fa493-217">Například běžet 14 města může trvat několik minut a spuštěna 15 města může trvat několik hodin.</span><span class="sxs-lookup"><span data-stu-id="fa493-217">For example, running for 14 cities could take several minutes, and running for 15 cities could take several hours.</span></span> <span data-ttu-id="fa493-218">Zvýšení too16 nebo více měst může mít za následek dnů modulu runtime (nakonec týdny, měsíce a letopočty).</span><span class="sxs-lookup"><span data-stu-id="fa493-218">Increasing too16 or more cities could result in days of runtime (eventually weeks, months, and years).</span></span> <span data-ttu-id="fa493-219">Toto je z důvodu toohello rychlý nárůst hello počet permutací vyhodnotit v Řešiteli hello jako hello počet zvyšuje měst.</span><span class="sxs-lookup"><span data-stu-id="fa493-219">This is due toohello rapid increase in hello number of permutations evaluated by hello solver as hello number of cities increases.</span></span>
> 
> 

### <a name="how-toorun-hello-monitoring-client-application"></a><span data-ttu-id="fa493-220">Jak toorun hello monitorování klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="fa493-220">How toorun hello monitoring client application</span></span>
1. <span data-ttu-id="fa493-221">Přihlaste se na tooyour počítači, kde budete spouštět hello klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa493-221">Log on tooyour machine where you will run hello client application.</span></span> <span data-ttu-id="fa493-222">To není nutné toobe hello stejný počítač s hello **TSPSolver** aplikace, i když může být.</span><span class="sxs-lookup"><span data-stu-id="fa493-222">This does not need toobe hello same machine running hello **TSPSolver** application, although it can be.</span></span>
2. <span data-ttu-id="fa493-223">Vytvořte složku, kde budete spouštět aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa493-223">Create a folder where you will run your application.</span></span> <span data-ttu-id="fa493-224">Například **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="fa493-224">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="fa493-225">Kopírování **TSPClient.jar** příliš**c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="fa493-225">Copy **TSPClient.jar** too**c:\TSP**,</span></span>
4. <span data-ttu-id="fa493-226">Zkontrolujte, zda je složka bin hello JRE v proměnné prostředí PATH hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-226">Ensure hello JRE's bin folder is in hello PATH environment variable.</span></span>
5. <span data-ttu-id="fa493-227">Na příkazovém řádku změňte adresáře tooc:\TSP.</span><span class="sxs-lookup"><span data-stu-id="fa493-227">At a command prompt, change directories tooc:\TSP.</span></span>
6. <span data-ttu-id="fa493-228">Spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fa493-228">Run hello following command.</span></span>
   
        java -jar TSPClient.jar
   
    <span data-ttu-id="fa493-229">Volitelně zadejte hello počet minut toosleep mezi kontroly fronty hello předáním v argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="fa493-229">Optionally, specify hello number of minutes toosleep in between checking hello queue, by passing in a command-line argument.</span></span> <span data-ttu-id="fa493-230">Hello výchozí režim spánku období kontroly fronty hello je 3 minuty, který se používá, pokud je příliš předán žádný argument příkazového řádku**TSPClient**.</span><span class="sxs-lookup"><span data-stu-id="fa493-230">hello default sleep period for checking hello queue is 3 minutes, which is used if no command-line argument is passed too**TSPClient**.</span></span> <span data-ttu-id="fa493-231">Pokud chcete toouse jinou hodnotu pro interval hello režimu spánku, například jednu minutu, spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa493-231">If you want toouse a different value for hello sleep interval, for example, one minute, run hello following command.</span></span>
   
        java -jar TSPClient.jar 1
   
    <span data-ttu-id="fa493-232">Hello klienta se spustí, dokud ho uvidí zprávu fronty služby "Dokončeno".</span><span class="sxs-lookup"><span data-stu-id="fa493-232">hello client will run until it sees a queue message of "Complete".</span></span> <span data-ttu-id="fa493-233">Všimněte si, že pokud spustíte vícenásobné výskyty hello solver bez spuštění klienta hello, může být nutné toorun hello klienta několikrát toocompletely prázdný hello fronty.</span><span class="sxs-lookup"><span data-stu-id="fa493-233">Note that if you run multiple occurrences of hello solver without running hello client, you may need toorun hello client multiple times toocompletely empty hello queue.</span></span> <span data-ttu-id="fa493-234">Alternativně můžete odstranit hello fronty a poté jej znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="fa493-234">Alternatively, you can delete hello queue and then create it again.</span></span> <span data-ttu-id="fa493-235">toodelete hello fronty, spusťte následující hello **TSPSolver** (ne **TSPClient**) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa493-235">toodelete hello queue, run hello following **TSPSolver** (not **TSPClient**)  command.</span></span>
   
        java -jar TSPSolver.jar deletequeue
   
    <span data-ttu-id="fa493-236">Řešitel Hello se spustí, dokud neskončí zkoumání všechny trasy.</span><span class="sxs-lookup"><span data-stu-id="fa493-236">hello solver will run until it finishes examining all routes.</span></span>

## <a name="how-toostop-hello-java-applications"></a><span data-ttu-id="fa493-237">Jak toostop hello aplikací v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="fa493-237">How toostop hello Java applications</span></span>
<span data-ttu-id="fa493-238">Pro hello solver a klientské aplikace, můžete stisknout **Ctrl + C** tooexit, pokud chcete, aby tooend předchozí toonormal dokončení.</span><span class="sxs-lookup"><span data-stu-id="fa493-238">For both hello solver and client applications, you can press **Ctrl+C** tooexit if you want tooend prior toonormal completion.</span></span>

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
