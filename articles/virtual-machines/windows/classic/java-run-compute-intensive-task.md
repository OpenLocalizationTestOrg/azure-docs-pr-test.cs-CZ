---
title: "Aplikace Java náročné na virtuálním počítači | Microsoft Docs"
description: "Naučte se vytvářet Azure virtuální počítač, který spouští aplikaci Java náročné, která lze sledovat v jiné aplikaci Java."
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
ms.openlocfilehash: 8c51c0bb37e25ad61fe58a85dd641dabe0a1958c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a><span data-ttu-id="5e7c9-103">Jak spouštět úlohy náročné na výpočetní výkon v Javě na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="5e7c9-103">How to run a compute-intensive task in Java on a virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="5e7c9-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5e7c9-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="5e7c9-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="5e7c9-107">S Azure můžete pro zpracování úlohy náročné na výkon virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-107">With Azure, you can use a virtual machine to handle compute-intensive tasks.</span></span> <span data-ttu-id="5e7c9-108">Virtuální počítač můžete například zpracování úlohy a výsledky doručení do klientského počítače nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-108">For example, a virtual machine can handle tasks and deliver results to client machines or mobile applications.</span></span> <span data-ttu-id="5e7c9-109">Po přečtení tohoto článku, budete mít představu o tom, jak vytvořit virtuální počítač, který spouští aplikaci Java náročné, která lze sledovat v jiné aplikaci Java.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-109">After reading this article, you will have an understanding of how to create a virtual machine that runs a compute-intensive Java application that can be monitored by another Java application.</span></span>

<span data-ttu-id="5e7c9-110">Tento kurz předpokládá vědět, jak k vytvoření konzolové aplikace Java, můžete importovat knihovny do aplikace v jazyce Java a může generovat Java archivu (JAR).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-110">This tutorial assumes you know how to create Java console applications, can import libraries to your Java application, and can generate a Java archive (JAR).</span></span> <span data-ttu-id="5e7c9-111">Předpokládají se žádné znalosti ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-111">No knowledge of Microsoft Azure is assumed.</span></span>

<span data-ttu-id="5e7c9-112">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="5e7c9-112">You will learn:</span></span>

* <span data-ttu-id="5e7c9-113">Postup vytvoření virtuálního počítače s Java Development Kit (JDK) už nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-113">How to create a virtual machine with a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="5e7c9-114">Jak se vzdáleně přihlaste k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-114">How to remotely log in to your virtual machine.</span></span>
* <span data-ttu-id="5e7c9-115">Jak vytvořit obor názvů sběrnice.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-115">How to create a service bus namespace.</span></span>
* <span data-ttu-id="5e7c9-116">Jak vytvořit aplikaci Java, která provede náročné úlohy.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-116">How to create a Java application that performs a compute-intensive task.</span></span>
* <span data-ttu-id="5e7c9-117">Jak vytvořit aplikaci Java, která monitoruje průběh úlohy náročné na výkon.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-117">How to create a Java application that monitors the progress of the compute-intensive task.</span></span>
* <span data-ttu-id="5e7c9-118">Postup spuštění aplikací Java.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-118">How to run the Java applications.</span></span>
* <span data-ttu-id="5e7c9-119">Postup zastavení aplikací Java.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-119">How to stop the Java applications.</span></span>

<span data-ttu-id="5e7c9-120">V tomto kurzu použije Traveling prodejce problém pro úlohy náročné na výkon.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-120">This tutorial will use the Traveling Salesman Problem for the compute-intensive task.</span></span> <span data-ttu-id="5e7c9-121">Následuje příklad aplikace Java spuštění úlohy náročné na výkon.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-121">The following is an example of the Java application running the compute-intensive task.</span></span>

![Řešitel problém Traveling prodejce][solver_output]

<span data-ttu-id="5e7c9-123">Následuje příklad monitorování úlohy náročné na výkon aplikací Java.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-123">The following is an example of the Java application monitoring the compute-intensive task.</span></span>

![Problém Traveling prodejce klienta][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a><span data-ttu-id="5e7c9-125">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5e7c9-125">To create a virtual machine</span></span>
1. <span data-ttu-id="5e7c9-126">Přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-126">Log in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="5e7c9-127">Klikněte na tlačítko **nový**, klikněte na tlačítko **výpočetní**, klikněte na tlačítko **virtuálního počítače**a potom klikněte na **z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-127">Click **New**, click **Compute**, click **Virtual machine**, and then click **From Gallery**.</span></span>
3. <span data-ttu-id="5e7c9-128">V **vyberte bitovou kopii virtuálního počítače** dialogové okno, vyberte **JDK 7 Windows serveru 2012**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-128">In the **Virtual machine image select** dialog box, select **JDK 7 Windows Server 2012**.</span></span>
   <span data-ttu-id="5e7c9-129">Všimněte si, že **JDK 6 systému Windows Server 2012** je k dispozici v případě, že máte starší aplikace, které ještě nejsou připravené ke spuštění v JDK 7.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-129">Note that **JDK 6 Windows Server 2012** is available in case you have legacy applications that are not yet ready to run in JDK 7.</span></span>
4. <span data-ttu-id="5e7c9-130">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-130">Click **Next**.</span></span>
5. <span data-ttu-id="5e7c9-131">V **konfigurace virtuálního počítače** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="5e7c9-131">In the **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="5e7c9-132">Zadejte název pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-132">Specify a name for the virtual machine.</span></span>
   2. <span data-ttu-id="5e7c9-133">Zadejte velikost používat pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-133">Specify the size to use for the virtual machine.</span></span>
   3. <span data-ttu-id="5e7c9-134">Zadejte název pro správce v **uživatelské jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-134">Enter a name for the administrator in the **User Name** field.</span></span> <span data-ttu-id="5e7c9-135">Nezapomeňte toto jméno a heslo, které budou vedle zadat, je budou používat při vzdálené přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-135">Remember this name and the password you will enter next, you will use them when you remotely log in to the virtual machine.</span></span>
   4. <span data-ttu-id="5e7c9-136">Zadejte heslo do **nové heslo** pole a zadejte jej v znovu **potvrdit** pole.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-136">Enter a password in the **New password** field, and re-enter it in the **Confirm** field.</span></span> <span data-ttu-id="5e7c9-137">Toto je heslo k účtu správce.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-137">This is the Administrator account password.</span></span>
   5. <span data-ttu-id="5e7c9-138">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-138">Click **Next**.</span></span>
6. <span data-ttu-id="5e7c9-139">V dalším **konfigurace virtuálního počítače** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="5e7c9-139">In the next **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="5e7c9-140">Pro **Cloudová služba**, použijte výchozí **vytvořte novou cloudovou službu**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-140">For **Cloud service**, use the default **Create a new cloud service**.</span></span>
   2. <span data-ttu-id="5e7c9-141">Hodnota **název cloudové služby DNS** musí být jedinečný v rámci cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-141">The value for **Cloud service DNS name** must be unique across cloudapp.net.</span></span> <span data-ttu-id="5e7c9-142">V případě potřeby upravte tuto hodnotu tak, že Azure označuje, že je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-142">If needed, modify this value so that Azure indicates it is unique.</span></span>
   3. <span data-ttu-id="5e7c9-143">Zadejte oblast, skupiny vztahů nebo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-143">Specify a region, affinity group, or virtual network.</span></span> <span data-ttu-id="5e7c9-144">Pro účely tohoto kurzu, zadejte v oblasti **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-144">For purposes of this tutorial, specify a region such as **West US**.</span></span>
   4. <span data-ttu-id="5e7c9-145">Pro **účet úložiště**, vyberte **použít účet úložiště automaticky generované**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-145">For **Storage Account**, select **Use an automatically generated storage account**.</span></span>
   5. <span data-ttu-id="5e7c9-146">Pro **sadu dostupnosti**, vyberte **(None)**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-146">For **Availability Set**, select **(None)**.</span></span>
   6. <span data-ttu-id="5e7c9-147">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-147">Click **Next**.</span></span>
7. <span data-ttu-id="5e7c9-148">V konečné **konfigurace virtuálního počítače** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="5e7c9-148">In the final **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="5e7c9-149">Přijměte výchozí koncový bod položky.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-149">Accept the default endpoint entries.</span></span>
   2. <span data-ttu-id="5e7c9-150">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-150">Click **Complete**.</span></span>

## <a name="to-remotely-log-in-to-your-virtual-machine"></a><span data-ttu-id="5e7c9-151">Vzdáleně se přihlásit k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="5e7c9-151">To remotely log in to your virtual machine</span></span>
1. <span data-ttu-id="5e7c9-152">Přihlaste se na [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-152">Log on to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="5e7c9-153">Klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-153">Click **Virtual machines**.</span></span>
3. <span data-ttu-id="5e7c9-154">Klikněte na název virtuálního počítače, který chcete k přihlášení do.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-154">Click the name of the virtual machine that you want to log in to.</span></span>
4. <span data-ttu-id="5e7c9-155">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-155">Click **Connect**.</span></span>
5. <span data-ttu-id="5e7c9-156">Reagujete na výzvy podle potřeby pro připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-156">Respond to the prompts as needed to connect to the virtual machine.</span></span> <span data-ttu-id="5e7c9-157">Pokud budete vyzváni k správce jména a hesla, použijte hodnoty, které jste zadali při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-157">When prompted for the administrator name and password, use the values that you provided when you created the virtual machine.</span></span>

<span data-ttu-id="5e7c9-158">Všimněte si, že funkci Azure Service Bus vyžaduje certifikát Baltimore CyberTrust Root, který chcete nainstalovat jako součást vašeho prostředí JRE **cacerts** uložit.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-158">Note that the Azure Service Bus functionality requires the Baltimore CyberTrust Root certificate to be installed as part of your JRE's **cacerts** store.</span></span> <span data-ttu-id="5e7c9-159">Tento certifikát je automaticky zahrnuty v prostředí Java Runtime (JRE) používané v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-159">This certificate is automatically included in the Java Runtime Environment (JRE) used by this tutorial.</span></span> <span data-ttu-id="5e7c9-160">Pokud jste tento certifikát ve vašem prostředí JRE **cacerts** úložiště najdete v tématu [přidání certifikátu do úložiště certifikátů certifikační Autority Java] [ add_ca_cert] informace o přidání (a také informace o zobrazení certifikáty v úložišti cacerts).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-160">If you do not have this certificate in your JRE **cacerts** store, see [Adding a Certificate to the Java CA Certificate Store][add_ca_cert] for information on adding it (as well as information on viewing the certificates in your cacerts store).</span></span>

## <a name="how-to-create-a-service-bus-namespace"></a><span data-ttu-id="5e7c9-161">Postup vytvoření oboru názvů service bus</span><span class="sxs-lookup"><span data-stu-id="5e7c9-161">How to create a service bus namespace</span></span>
<span data-ttu-id="5e7c9-162">Pokud chcete začít používat fronty služby Service Bus v Azure, musíte nejdřív vytvořit obor názvů služby.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-162">To begin using Service Bus queues in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="5e7c9-163">Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-163">A service namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="5e7c9-164">Vytvoření oboru názvů služby:</span><span class="sxs-lookup"><span data-stu-id="5e7c9-164">To create a service namespace:</span></span>

1. <span data-ttu-id="5e7c9-165">Přihlaste se na [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-165">Log on to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="5e7c9-166">V levém navigačním podokně portálu Azure classic, klikněte na **Service Bus, řízení přístupu a ukládání do mezipaměti**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-166">In the lower-left navigation pane of the Azure classic portal, click **Service Bus, Access Control & Caching**.</span></span>
3. <span data-ttu-id="5e7c9-167">V levém podokně portálu Azure classic, klikněte na **Service Bus** uzel a klikněte **nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-167">In the upper-left pane of the Azure classic portal, click the **Service Bus** node, and then click the **New** button.</span></span>  
   <span data-ttu-id="5e7c9-168">![Snímek obrazovky uzel Service Bus][svc_bus_node]</span><span class="sxs-lookup"><span data-stu-id="5e7c9-168">![Service Bus Node screenshot][svc_bus_node]</span></span>
4. <span data-ttu-id="5e7c9-169">V **vytvoření nové služby Namespace** dialogovém okně zadejte **Namespace**a pokud chcete mít jistotu, že je jedinečný, klikněte **zkontrolovat dostupnost** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-169">In the **Create a new Service Namespace** dialog box, enter a **Namespace**, and then to make sure that it is unique, click the **Check Availability** button.</span></span>  
   <span data-ttu-id="5e7c9-170">![Vytvořit nový Namespace snímek][create_namespace]</span><span class="sxs-lookup"><span data-stu-id="5e7c9-170">![Create a New Namespace screenshot][create_namespace]</span></span>
5. <span data-ttu-id="5e7c9-171">Po dostupné zajistit, že název oboru názvů vyberte zemi nebo oblast, ve kterém by měl být hostován vašeho oboru názvů a klikněte **vytvořit Namespace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-171">After making sure the namespace name is available, choose the country or region in which your namespace should be hosted, and then click the **Create Namespace** button.</span></span>  
   
   <span data-ttu-id="5e7c9-172">Obor názvů, který jste vytvořili se potom zobrazí na portálu Azure classic a aktivovat chvíli trvá.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-172">The namespace you created will then appear in the Azure classic portal and takes a moment to activate.</span></span> <span data-ttu-id="5e7c9-173">Počkejte, dokud je stav **Active** před pokračováním na další krok.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-173">Wait until the status is **Active** before continuing with the next step.</span></span>

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a><span data-ttu-id="5e7c9-174">Získání výchozí pověření pro správu oboru názvů</span><span class="sxs-lookup"><span data-stu-id="5e7c9-174">Obtain the Default Management Credentials for the namespace</span></span>
<span data-ttu-id="5e7c9-175">Aby bylo možné provádět operace správy, jako je například vytváření fronty, na novém oboru názvů, potřebujete získat přihlašovací údaje pro obor názvů správu.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-175">In order to perform management operations, such as creating a queue, on the new namespace, you need to obtain the management credentials for the namespace.</span></span>

1. <span data-ttu-id="5e7c9-176">V levém navigačním podokně klikněte **Service Bus** uzlu zobrazíte seznam dostupných oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-176">In the left navigation pane, click the **Service Bus** node to display the list of available namespaces.</span></span>
   <span data-ttu-id="5e7c9-177">![K dispozici snímek obory názvů][avail_namespaces]</span><span class="sxs-lookup"><span data-stu-id="5e7c9-177">![Available Namespaces screenshot][avail_namespaces]</span></span>
2. <span data-ttu-id="5e7c9-178">Vyberte obor názvů, který jste právě vytvořili, v zobrazeném seznamu.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-178">Select the namespace you just created from the list shown.</span></span>
   <span data-ttu-id="5e7c9-179">![Snímek obrazovky seznamu Namespace][namespace_list]</span><span class="sxs-lookup"><span data-stu-id="5e7c9-179">![Namespace List screenshot][namespace_list]</span></span>
3. <span data-ttu-id="5e7c9-180">Pravém **vlastnosti** podokně jsou uvedeny vlastnosti pro nový obor názvů.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-180">The right-hand **Properties** pane lists the properties for the new namespace.</span></span>
   <span data-ttu-id="5e7c9-181">![Snímek obrazovky podokna Vlastnosti][properties_pane]</span><span class="sxs-lookup"><span data-stu-id="5e7c9-181">![Properties Pane screenshot][properties_pane]</span></span>
4. <span data-ttu-id="5e7c9-182">**Výchozí klíč** skryt.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-182">The **Default Key** is hidden.</span></span> <span data-ttu-id="5e7c9-183">Klikněte **zobrazení** tlačítko pro zobrazení zabezpečovací přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-183">Click the **View** button to display the security credentials.</span></span>
   <span data-ttu-id="5e7c9-184">![Výchozí klíč – snímek obrazovky][default_key]</span><span class="sxs-lookup"><span data-stu-id="5e7c9-184">![Default Key screenshot][default_key]</span></span>
5. <span data-ttu-id="5e7c9-185">Poznamenejte si **výchozí vystavitele** a **výchozí klíč** jako tento níže uvedené informace se používají k provádění operací s oborem názvů.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-185">Make a note of the **Default Issuer** and the **Default Key** as you will use this information below to perform operations with the namespace.</span></span>

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a><span data-ttu-id="5e7c9-186">Jak vytvořit aplikaci Java, která provede náročné úlohy</span><span class="sxs-lookup"><span data-stu-id="5e7c9-186">How to create a Java application that performs a compute-intensive task</span></span>
1. <span data-ttu-id="5e7c9-187">Na vývojovém počítači (která nemusí být virtuální počítač, který jste vytvořili) stáhněte si [Azure SDK pro jazyk Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-187">On your development machine (which does not have to be the virtual machine that you created), download the [Azure SDK for Java](https://azure.microsoft.com/develop/java/).</span></span>
2. <span data-ttu-id="5e7c9-188">Vytvořte konzolovou aplikaci Java pomocí ukázkový kód na konci této části.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-188">Create a Java console application using the example code at the end of this section.</span></span> <span data-ttu-id="5e7c9-189">V tomto kurzu použijeme **TSPSolver.java** jako název souboru Java.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-189">In this tutorial, we'll use **TSPSolver.java** as the Java file name.</span></span> <span data-ttu-id="5e7c9-190">Změnit **vaše\_služby\_sběrnice\_obor názvů**, **vaše\_služby\_sběrnice\_vlastníka**a **vaše\_služby\_sběrnice\_klíč** zástupné symboly pro použití služby service bus **obor názvů**, **výchozí vystavitele** a  **Výchozí klíč** hodnoty v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-190">Modify the **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders to use your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
3. <span data-ttu-id="5e7c9-191">Po kódování, exportovat aplikaci spustitelného archivu Java (JAR) a balíček požadované knihovny do vygenerované JAR.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-191">After coding, export the application to a runnable Java archive (JAR), and package the required libraries into the generated JAR.</span></span> <span data-ttu-id="5e7c9-192">V tomto kurzu použijeme **TSPSolver.jar** jako vygenerovaný název JAR.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-192">In this tutorial, we'll use **TSPSolver.jar** as the generated JAR name.</span></span>

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

        //  Value specifying how often to provide an update to the console.
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

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
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



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a><span data-ttu-id="5e7c9-193">Jak vytvořit aplikaci Java, která monitoruje průběh úlohy náročné na výkon</span><span class="sxs-lookup"><span data-stu-id="5e7c9-193">How to create a Java application that monitors the progress of the compute-intensive task</span></span>
1. <span data-ttu-id="5e7c9-194">Na vývojovém počítači vytvořte konzolovou aplikaci Java pomocí ukázkový kód na konci této části.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-194">On your development machine, create a Java console application using the example code at the end of this section.</span></span> <span data-ttu-id="5e7c9-195">V tomto kurzu použijeme **TSPClient.java** jako název souboru Java.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-195">In this tutorial, we'll use **TSPClient.java** as the Java file name.</span></span> <span data-ttu-id="5e7c9-196">Jak je uvedeno výše, upravit **vaše\_služby\_sběrnice\_obor názvů**, **vaše\_služby\_sběrnice\_vlastníka**, a **vaše\_služby\_sběrnice\_klíč** zástupné symboly pro použití služby service bus **obor názvů**, **výchozí vystavitele**a **výchozí klíč** hodnoty v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-196">As shown earlier, modify the **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders to use your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
2. <span data-ttu-id="5e7c9-197">Exportujte aplikace spustitelného JAR a balíček požadované knihovny do vygenerované JAR.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-197">Export the application to a runnable JAR, and package the required libraries into the generated JAR.</span></span> <span data-ttu-id="5e7c9-198">V tomto kurzu použijeme **TSPClient.jar** jako vygenerovaný název JAR.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-198">In this tutorial, we'll use **TSPClient.jar** as the generated JAR name.</span></span>

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

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
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

                            // Display the queue message.
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
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
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

## <a name="how-to-run-the-java-applications"></a><span data-ttu-id="5e7c9-199">Spuštění aplikace Java</span><span class="sxs-lookup"><span data-stu-id="5e7c9-199">How to run the Java applications</span></span>
<span data-ttu-id="5e7c9-200">Spuštění aplikace náročné na výkon, nejprve k vytvoření fronty, potom k vyřešení problému Saleseman cestě, která přidá aktuální nejlepší směrování do fronty service bus.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-200">Run the compute-intensive application, first to create the queue, then to solve the Traveling Saleseman Problem, which will add the current best route to the service bus queue.</span></span> <span data-ttu-id="5e7c9-201">Při spuštění (nebo později) aplikace náročné spuštění klienta zobrazit výsledky z fronty service bus.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-201">While the compute-intensive application is running (or afterwards), run the client to display results from the service bus queue.</span></span>

### <a name="to-run-the-compute-intensive-application"></a><span data-ttu-id="5e7c9-202">Ke spuštění aplikace náročné na výkon</span><span class="sxs-lookup"><span data-stu-id="5e7c9-202">To run the compute-intensive application</span></span>
1. <span data-ttu-id="5e7c9-203">Přihlaste se k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-203">Log on to your virtual machine.</span></span>
2. <span data-ttu-id="5e7c9-204">Vytvořte složku, kde budete spouštět aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-204">Create a folder where you will run your application.</span></span> <span data-ttu-id="5e7c9-205">Například **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-205">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="5e7c9-206">Kopírování **TSPSolver.jar** k **c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="5e7c9-206">Copy **TSPSolver.jar** to **c:\TSP**,</span></span>
4. <span data-ttu-id="5e7c9-207">Vytvořte soubor s názvem **c:\TSP\cities.txt** s tímto obsahem.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-207">Create a file named **c:\TSP\cities.txt** with the following contents.</span></span>
   
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
5. <span data-ttu-id="5e7c9-208">Na příkazovém řádku změňte adresáře na c:\TSP.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-208">At a command prompt, change directories to c:\TSP.</span></span>
6. <span data-ttu-id="5e7c9-209">Zkontrolujte, zda složky Bin přejít prostředí JRE v proměnné prostředí PATH.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-209">Ensure the JRE's bin folder is in the PATH environment variable.</span></span>
7. <span data-ttu-id="5e7c9-210">Budete muset vytvořit frontou service bus před spuštěním permutací solver TSP.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-210">You'll need to create the service bus queue before you run the TSP solver permutations.</span></span> <span data-ttu-id="5e7c9-211">Spusťte následující příkaz k vytvoření fronty service bus.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-211">Run the following command to create the service bus queue.</span></span>
   
        java -jar TSPSolver.jar createqueue
8. <span data-ttu-id="5e7c9-212">Teď, když je vytvářena fronta, můžete spustit permutací solver TSP.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-212">Now that the queue is created, you can run the TSP solver permutations.</span></span> <span data-ttu-id="5e7c9-213">Například spusťte následující příkaz ke spuštění Řešitele pro 8 měst.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-213">For example, run the following command to run the solver for 8 cities.</span></span>
   
        java -jar TSPSolver.jar 8
   
   <span data-ttu-id="5e7c9-214">Pokud nezadáte číslo, bude spuštěna pro 10 měst.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-214">If you don't specify a number, it will run for 10 cities.</span></span> <span data-ttu-id="5e7c9-215">Jako Řešitele vyhledá aktuální nejkratší trasy, přidá je do fronty.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-215">As the solver finds current shortest routes, it will add them to the queue.</span></span>

> [!NOTE]
> <span data-ttu-id="5e7c9-216">Čím větší číslo, které zadáte, tím déle solver spustí.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-216">The larger the number that you specify, the longer the solver will run.</span></span> <span data-ttu-id="5e7c9-217">Například běžet 14 města může trvat několik minut a spuštěna 15 města může trvat několik hodin.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-217">For example, running for 14 cities could take several minutes, and running for 15 cities could take several hours.</span></span> <span data-ttu-id="5e7c9-218">Zvýšení na 16 nebo víc měst může mít za následek dnů modulu runtime (nakonec týdny, měsíce a letopočty).</span><span class="sxs-lookup"><span data-stu-id="5e7c9-218">Increasing to 16 or more cities could result in days of runtime (eventually weeks, months, and years).</span></span> <span data-ttu-id="5e7c9-219">Příčinou je rychlý nárůst počtu permutací pomocí Řešitele vyhodnotit jako číslo zvyšuje měst.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-219">This is due to the rapid increase in the number of permutations evaluated by the solver as the number of cities increases.</span></span>
> 
> 

### <a name="how-to-run-the-monitoring-client-application"></a><span data-ttu-id="5e7c9-220">Spuštění monitorování klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="5e7c9-220">How to run the monitoring client application</span></span>
1. <span data-ttu-id="5e7c9-221">Přihlaste se k počítači, kde budete spouštět klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-221">Log on to your machine where you will run the client application.</span></span> <span data-ttu-id="5e7c9-222">Toto nemusí být stejný počítač s **TSPSolver** aplikace, i když může být.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-222">This does not need to be the same machine running the **TSPSolver** application, although it can be.</span></span>
2. <span data-ttu-id="5e7c9-223">Vytvořte složku, kde budete spouštět aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-223">Create a folder where you will run your application.</span></span> <span data-ttu-id="5e7c9-224">Například **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-224">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="5e7c9-225">Kopírování **TSPClient.jar** k **c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="5e7c9-225">Copy **TSPClient.jar** to **c:\TSP**,</span></span>
4. <span data-ttu-id="5e7c9-226">Zkontrolujte, zda složky Bin přejít prostředí JRE v proměnné prostředí PATH.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-226">Ensure the JRE's bin folder is in the PATH environment variable.</span></span>
5. <span data-ttu-id="5e7c9-227">Na příkazovém řádku změňte adresáře na c:\TSP.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-227">At a command prompt, change directories to c:\TSP.</span></span>
6. <span data-ttu-id="5e7c9-228">Spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-228">Run the following command.</span></span>
   
        java -jar TSPClient.jar
   
    <span data-ttu-id="5e7c9-229">Volitelně můžete zadejte počet minut do režimu spánku mezi kontroly fronty předáním v argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-229">Optionally, specify the number of minutes to sleep in between checking the queue, by passing in a command-line argument.</span></span> <span data-ttu-id="5e7c9-230">Doba spánku výchozí kontroly fronty je 3 minuty, který je použit, pokud je předán žádný argument příkazového řádku **TSPClient**.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-230">The default sleep period for checking the queue is 3 minutes, which is used if no command-line argument is passed to **TSPClient**.</span></span> <span data-ttu-id="5e7c9-231">Pokud chcete použít jiné hodnoty pro interval režimu spánku, například jednu minutu, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-231">If you want to use a different value for the sleep interval, for example, one minute, run the following command.</span></span>
   
        java -jar TSPClient.jar 1
   
    <span data-ttu-id="5e7c9-232">Klient se spustí, dokud ho uvidí zprávu fronty služby "Dokončeno".</span><span class="sxs-lookup"><span data-stu-id="5e7c9-232">The client will run until it sees a queue message of "Complete".</span></span> <span data-ttu-id="5e7c9-233">Poznámka: Pokud vícenásobné výskyty Řešitele spustíte bez spuštění klienta, pravděpodobně muset spustit klienta vícekrát zcela vyprázdnit frontu.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-233">Note that if you run multiple occurrences of the solver without running the client, you may need to run the client multiple times to completely empty the queue.</span></span> <span data-ttu-id="5e7c9-234">Alternativně můžete odstranit a vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-234">Alternatively, you can delete the queue and then create it again.</span></span> <span data-ttu-id="5e7c9-235">Pokud chcete odstranit frontu, spusťte následující **TSPSolver** (ne **TSPClient**) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-235">To delete the queue, run the following **TSPSolver** (not **TSPClient**)  command.</span></span>
   
        java -jar TSPSolver.jar deletequeue
   
    <span data-ttu-id="5e7c9-236">Řešitele se spustí, dokud neskončí zkoumání všechny trasy.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-236">The solver will run until it finishes examining all routes.</span></span>

## <a name="how-to-stop-the-java-applications"></a><span data-ttu-id="5e7c9-237">Postup zastavení aplikací Java</span><span class="sxs-lookup"><span data-stu-id="5e7c9-237">How to stop the Java applications</span></span>
<span data-ttu-id="5e7c9-238">Řešitel i klientské aplikace, můžete stisknout **Ctrl + C** ukončíte, pokud chcete ukončit před dokončením normální.</span><span class="sxs-lookup"><span data-stu-id="5e7c9-238">For both the solver and client applications, you can press **Ctrl+C** to exit if you want to end prior to normal completion.</span></span>

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
