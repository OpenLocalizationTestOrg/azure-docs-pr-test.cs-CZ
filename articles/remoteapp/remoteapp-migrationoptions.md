---
title: "Možnosti pro migraci z Azure Remoteappu | Microsoft Docs"
description: "Informace o možnostech pro migraci z Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9ab63124e2521ee1922d15c1e388c54d50eb8301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a><span data-ttu-id="8a6fe-103">Možnosti pro migraci z Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="8a6fe-103">Options for migrating out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8a6fe-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8a6fe-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>


<span data-ttu-id="8a6fe-106">Pokud je nutné zastavit pomocí Azure Remoteappu z důvodu [vyřazení oznámení](https://go.microsoft.com/fwlink/?linkid=821148) nebo protože dokončení testování, je potřeba migrovat z Azure RemoteApp do jiné služby app service.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-106">If you have stopped using Azure RemoteApp because of the [retirement announcement](https://go.microsoft.com/fwlink/?linkid=821148) or because you've finished your evaluation, you need to migrate off of Azure RemoteApp to another app service.</span></span> <span data-ttu-id="8a6fe-107">Existují dva různé přístupy k migraci: spravuje vlastními silami (často říká infrastruktury jako služby [IaaS]) nasazení nebo plně spravovaná (často označované jako platforma jako služba) nebo softwaru jako služby [PaaS nebo SaaS] nabídky.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-107">There are two different approaches for migrating: a self-managed (often called Infrastructure as a Service [IaaS]) deployment or a fully managed (often called Platform as a Service or Software as a Service [PaaS/SaaS]) offering.</span></span> 

<span data-ttu-id="8a6fe-108">Samoobslužné služby IaaS je samoobslužné nasazení, které je spravovaná, provozovat a vlastní můžete nasadit přímo na virtuální počítače (VM) nebo fyzickými systémy.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-108">Self-service IaaS is a do-it-yourself deployment that is managed, operated, and owned by you, directly deployed on virtual machines (VMs) or physical systems.</span></span> <span data-ttu-id="8a6fe-109">Na druhém konci plně spravovaná PaaS nebo SaaS nabídka je informace, například Azure RemoteApp – partnera poskytuje služby vrstvu nad vzdálenou komunikaci řešení, která zpracovává provozní a obsluhu, při, jako zákazník, do správy některé image a aplikací.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-109">At the other end, a fully managed PaaS/SaaS offering is more like Azure RemoteApp - a partner provides a service layer on top of a remoting solution that handles operational and servicing, while you, as the customer, do some image and app management.</span></span>

<span data-ttu-id="8a6fe-110">[Zobrazit webináře Azure RemoteApp na možnosti migrace](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), nebo si můžete přečíst na další informace (včetně příkladů různé možnosti hostování).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-110">[View the Azure RemoteApp webinars on migration options](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), or read on for more information (including examples of the different hosting options).</span></span>

## <a name="self-managed-iaas-solutions"></a><span data-ttu-id="8a6fe-111">Spravuje vlastními silami řešení (IaaS)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-111">Self-managed (IaaS) solutions</span></span>
### <a name="rds-on-iaas"></a><span data-ttu-id="8a6fe-112">**Vzdálená plocha na IaaS**</span><span class="sxs-lookup"><span data-stu-id="8a6fe-112">**RDS on IaaS**</span></span>
<span data-ttu-id="8a6fe-113">Nasazení můžete nativní na bázi relace služby Vzdálená plocha (v systému Windows Server) pomocí vzdálené aplikace RemoteApp nebo klienty na místně nebo v hostovaném prostředí (jako na virtuálních počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-113">You can deploy a native session-based Remote Desktop Services (in Windows Server) deployment using either RemoteApp or desktops on-premises or in a hosted environment (like on Azure VMs).</span></span> <span data-ttu-id="8a6fe-114">Vzdálená plocha na IaaS nasazení jsou nejlepší pro zákazníky už znáte a které mají existující technických otázek u nasazení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-114">RDS on IaaS deployments are best for customers already familiar with and that have existing technical expertise with RDS deployments.</span></span> 

> [!NOTE]
> <span data-ttu-id="8a6fe-115">Musíte Volume Licensing s Software Assurance (SA) pro vzdálené plochy licencí pro klientský přístup pro použití této možnosti nasazení.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-115">You need Volume Licensing with Software Assurance (SA) for RDS client access licenses to use this deployment option.</span></span>

<span data-ttu-id="8a6fe-116">Nasazení vzdálené plochy na virtuálních počítačích Azure je jednodušší než kdy při použití nasazení a opravy chyb šablony (čtení [přehled](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) a potom [přejděte získat je](https://aka.ms/rdautomation)).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-116">Deploying RDS on Azure VMs is easier than ever when you use deployment and patching templates (read an [overview](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) and then [go get them](https://aka.ms/rdautomation)).</span></span> <span data-ttu-id="8a6fe-117">Můžete získat stejné možnosti elastické škálování s Azure classic nasazení modelu prostředky (ne prostředky Model prostředků Azure) v rámci Azure RemoteApp pomocí [automatické škálování skriptu](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), i když existují další přizpůsobení a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-117">You can get the same elastic scaling capabilities with Azure classic deployment model resources (not Azure Resource Model resources) within Azure RemoteApp by using the [auto scaling script](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), although there are more customizations and configurations.</span></span> <span data-ttu-id="8a6fe-118">Při nasazení vzdálené plochy na virtuálních počítačích Azure, je poskytována prostřednictvím podpory [podporu Azure](https://azure.microsoft.com/support/plans/), stejné podporují Odborníci v oblasti, která je podporována s Azure Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-118">When you deploy RDS on Azure VMs, support is provided through [Azure Support](https://azure.microsoft.com/support/plans/), the same support professionals that supported you with Azure RemoteApp.</span></span> <span data-ttu-id="8a6fe-119">Můžete získat náklady odhady založené na vaší stávající využití kontaktováním [podporu Azure](https://azure.microsoft.com/support/plans/), nebo můžete výpočty provést sami pomocí brzy být vydané kalkulačky náklady.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-119">You can get cost estimates based on your existing usage by contacting [Azure Support](https://azure.microsoft.com/support/plans/), or you can perform calculations yourself using a soon to be released Cost Calculator.</span></span>  <span data-ttu-id="8a6fe-120">S virtuálními počítači N-series (aktuálně v soukromém náhledu) můžete také přidat vGPU - informace o přidávání vGPU a o tom, jak nás [budou využívat vylepšení vzdálené plochy v systému Windows Server 2016](https://myignite.microsoft.com/videos/2794) v našem Ignite relaci.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-120">Also, with N-series VMs (currently in private preview) you can add vGPU - hear more about adding vGPU and about how to [harness RDS improvements in Windows Server 2016](https://myignite.microsoft.com/videos/2794) in our Ignite session.</span></span>   

<span data-ttu-id="8a6fe-121">Máme průvodcích nasazením krok za krokem pro [Windows Server 2012 R2](http://aka.ms/rdsonazure) a [systému Windows Server 2016](http://aka.ms/rdsonazure2016) pomoct s nasazením.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-121">We have step by step deployment guides for [Windows Server 2012 R2](http://aka.ms/rdsonazure) and [Windows Server 2016](http://aka.ms/rdsonazure2016) to assist with your deployment.</span></span> <span data-ttu-id="8a6fe-122">Podívejte se [vzdálené plochy blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) nejnovější informace.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-122">Check out the [Remote Desktop blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) for the latest news.</span></span>

### <a name="citrix-on-iaas"></a><span data-ttu-id="8a6fe-123">**Citrix na IaaS**</span><span class="sxs-lookup"><span data-stu-id="8a6fe-123">**Citrix on IaaS**</span></span>
<span data-ttu-id="8a6fe-124">Nativní Citrix nasazení na základě relace XenApp nebo XenDesktop lze nasadit místně nebo v hostovaném prostředí (například na virtuálních počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-124">A native Citrix deployment of session-based XenApp or XenDesktop can be deployed on-premises or within a hosted environment (such as on Azure VMs).</span></span> 

<span data-ttu-id="8a6fe-125">Podívejte se na průvodci podrobný postup nasazení [Citrix XA 7.6 v Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), další informace.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-125">Check out the step-by-step deployment guide, [Citrix XA 7.6 on Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), for more information.</span></span> <span data-ttu-id="8a6fe-126">Další informace o [Citrix v Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), včetně kalkulačky ceny.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-126">Read more about [Citrix on Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), including a price calculator.</span></span> <span data-ttu-id="8a6fe-127">Můžete také vyhledat [Citrix obraťte se na](http://citrix.com/English/contact/index.asp) dohodnout se vaše možnosti.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-127">You can also find a [Citrix contact](http://citrix.com/English/contact/index.asp) to discuss your options with.</span></span>

## <a name="fully-managed-paassaas-offerings"></a><span data-ttu-id="8a6fe-128">Plně spravovaná nabídky (PaaS nebo SaaS)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-128">Fully managed (PaaS/SaaS) offerings</span></span>

### <a name="citrix-xenapp-essentials-released-april-2017"></a><span data-ttu-id="8a6fe-129">Citrix XenApp Essentials (vydané dubna 2017)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-129">Citrix XenApp Essentials (released April 2017)</span></span>
<span data-ttu-id="8a6fe-130">Nyní dostupné na [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials je nová služba virtualizace aplikace, kombinace výkon a flexibilitu Citrix Cloudová platforma s jednoduchý, doporučený a snadno využívat vizi Microsoft Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-130">Available now on the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials is the new application virtualization service, combining the power and flexibility of the Citrix Cloud platform with the simple, prescriptive, and easy-to-consume vision of Microsoft Azure RemoteApp.</span></span> 

<span data-ttu-id="8a6fe-131">Stávající zákazníky služby Azure RemoteApp můžete [zaregistrovat k bezplatné zkušební verzi](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-131">Existing Azure RemoteApp customers can [register for a free trial](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span></span>  <span data-ttu-id="8a6fe-132">Poznámka: Pouze Citrix poplatek za uživatele služby je volné, použít Azure náklady na výpočetních operací a úložiště</span><span class="sxs-lookup"><span data-stu-id="8a6fe-132">Note: Only Citrix user service charge is free, Azure compute and storage costs apply</span></span>

<span data-ttu-id="8a6fe-133">Víc se uč:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-133">Learn More:</span></span>
- [<span data-ttu-id="8a6fe-134">Migrace z Azure Remoteappu k Citrix XenApp Essentials</span><span class="sxs-lookup"><span data-stu-id="8a6fe-134">Migrate from Azure RemoteApp to Citrix XenApp Essentials</span></span>](remoteapp-migrate-citrix.md)
- [<span data-ttu-id="8a6fe-135">Citrix a společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="8a6fe-135">Citrix and Microsoft</span></span>](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- <span data-ttu-id="8a6fe-136">[Prezentace Citrix XenApp Essentials](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-136">[Citrix XenApp Essentials presentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span></span>  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a><span data-ttu-id="8a6fe-137">Cloud XenApp Citrix a XenDesktop služba</span><span class="sxs-lookup"><span data-stu-id="8a6fe-137">Citrix Cloud XenApp Service and XenDesktop Service</span></span> 

<span data-ttu-id="8a6fe-138">[Cloud XenApp Citrix a služba XenDesktop](https://www.citrix.com/products/citrix-cloud/services.html) je nejlepší řešení pro doručování aplikace a stolní počítače, plus pokročilé správy i možnosti monitorování.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-138">[Citrix Cloud XenApp Service and XenDesktop Service](https://www.citrix.com/products/citrix-cloud/services.html) is the best solution for the delivery of both apps and desktops, plus advanced management and monitoring capabilities.</span></span> 

#### <a name="conexlink-platform-name-mycloudit"></a><span data-ttu-id="8a6fe-139">Conexlink (název platformy: MyCloudIT)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-139">Conexlink (Platform name: MyCloudIT)</span></span>
<span data-ttu-id="8a6fe-140">[MyCloudIT](https://mycloudit.com) platformu automatizace pro IT společnosti zjednodušit, optimalizace a škálovat migrace a doručování vzdálené plochy, vzdálené aplikace a infrastrukturu v cloudu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-140">[MyCloudIT](https://mycloudit.com) is an automation platform for IT companies to simplify, optimize, and scale the migration and delivery of remote desktops, remote applications, and infrastructure in the Microsoft Azure Cloud.</span></span> 

<span data-ttu-id="8a6fe-141">Platforma MyCloudIT snižuje dobu nasazení 95 %, Azure náklady 30 % a přesouvá celé infrastruktury IT jejich klienta do cloudu v řádu tahy pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-141">The MyCloudIT platform reduces deployment time by 95%, Azure cost by 30%, and moves their client's entire IT infrastructure into the cloud in a matter of a few key strokes.</span></span> <span data-ttu-id="8a6fe-142">Partneři teď můžou zákazníci spravovat z jedné globální řídicího panelu, koncoví uživatelé služby po celém světě jako nikdy předtím a růst výnosů bez přidání dalších zásahů nebo rozsáhlé Azure školení.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-142">Partners can now manage customers from one global dashboard, service end users around the world like never before, and grow revenues without adding additional overhead or extensive Azure training.</span></span>  

> <span data-ttu-id="8a6fe-143">Primární umístění: Dallas, TX, USA</span><span class="sxs-lookup"><span data-stu-id="8a6fe-143">Primary location: Dallas, TX, USA</span></span>
> 
> <span data-ttu-id="8a6fe-144">Operace oblast: po celém světě</span><span class="sxs-lookup"><span data-stu-id="8a6fe-144">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="8a6fe-145">Partner stav: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-145">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span></span>
> 
> <span data-ttu-id="8a6fe-146">Zprostředkovatele služby Microsoft Cloud: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-146">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="8a6fe-147">Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak</span><span class="sxs-lookup"><span data-stu-id="8a6fe-147">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="8a6fe-148">Migrace řešení Azure RemoteApp: Ano, [Další informace](https://mycloudit.com/remote-app-microsoft/)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-148">Azure RemoteApp migration solutions: Yes, [learn more](https://mycloudit.com/remote-app-microsoft/)</span></span>
> 
> <span data-ttu-id="8a6fe-149">Brian Garoutte, VP obchodní vývoj</span><span class="sxs-lookup"><span data-stu-id="8a6fe-149">Brian Garoutte, VP of Business Development</span></span>
> 
> <span data-ttu-id="8a6fe-150">Phone: 972-218-0741</span><span class="sxs-lookup"><span data-stu-id="8a6fe-150">Phone: 972-218-0741</span></span>
>   
> <span data-ttu-id="8a6fe-151">E-mailu:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-151">Email: [brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span></span>

### <a name="frame"></a><span data-ttu-id="8a6fe-152">Rámce</span><span class="sxs-lookup"><span data-stu-id="8a6fe-152">Frame</span></span>

<span data-ttu-id="8a6fe-153">Organizace IT v podniku a government, zprostředkovatelé spravované služby a předními dodavateli softwaru vybrat rámce vytvářet a spravovat jejich zabezpečení, softwarově definované pracovní prostory v cloudu.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-153">IT organizations in enterprise and government, managed service providers, and leading software vendors choose Frame to create and manage their secure, software-defined workspaces in the cloud.</span></span> <span data-ttu-id="8a6fe-154">Z malé a velké organizace, rámeček umožňuje velmi snadno tak, aby uživatelé přístup k aplikacím Windows prostřednictvím libovolného prohlížeče z libovolného zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-154">From small to large organizations, Frame makes it incredibly easy to let users access Windows applications on any browser from any device.</span></span> <span data-ttu-id="8a6fe-155">Platforma rámce obsahuje všechno, co správce potřebuje k nasazení aplikací z cloudu, včetně infrastruktury Azure a licencí Vzdálené plochy (přinesou vlastní účet Azure a licencí je volitelný).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-155">The Frame platform includes everything an admin needs to deploy applications from the cloud including the Azure infrastructure and RDS licenses (bringing your own Azure account and licenses is optional).</span></span> 

<span data-ttu-id="8a6fe-156">Další informace o [rámce v Azure](https://www.fra.me/ara).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-156">Learn more about [Frame on Azure](https://www.fra.me/ara).</span></span> 

> <span data-ttu-id="8a6fe-157">Primární umístění: Mateo sítě San, certifikační Autority, USA</span><span class="sxs-lookup"><span data-stu-id="8a6fe-157">Primary location: San Mateo, CA, USA</span></span>
>
> <span data-ttu-id="8a6fe-158">Operace oblast: po celém světě</span><span class="sxs-lookup"><span data-stu-id="8a6fe-158">Operation region: Worldwide</span></span>
>
> <span data-ttu-id="8a6fe-159">Partner společnosti Microsoft: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-159">Microsoft Partner: Yes</span></span>
> 
> <span data-ttu-id="8a6fe-160">Telefon: 1-480-269-4668</span><span class="sxs-lookup"><span data-stu-id="8a6fe-160">Phone: 1-480-269-4668</span></span>

### <a name="awingu"></a><span data-ttu-id="8a6fe-161">Awingu</span><span class="sxs-lookup"><span data-stu-id="8a6fe-161">Awingu</span></span>
<span data-ttu-id="8a6fe-162">Awingu poskytuje jednoduché online prostoru řešení spuštěná starší verze aplikace, SaaS a dokumenty z prohlížeče html5.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-162">Awingu provides a simple online workspace solution running legacy apps, SaaS and documents from an html5 browser.</span></span> <span data-ttu-id="8a6fe-163">Jako takový zpřístupnění všechny aplikace bezpečně na všech typů zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-163">As such, making any applications securely available on any type of device.</span></span> <span data-ttu-id="8a6fe-164">Pro služby SaaS je k dispozici široké škále op Single-Sign-On možnosti.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-164">For SaaS services, a wide range op Single-Sign-On options is available.</span></span> <span data-ttu-id="8a6fe-165">Systémy souborů různých (cloud) také může být úzce integruje, do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-165">Also diverse (cloud) files systems can be deeply integrated into your workspace.</span></span> <span data-ttu-id="8a6fe-166">Vedle úplné mobility získáte bohaté online prostoru na Awingu optimální zabezpečení granulární ovládacích prvků (například stahování nebo odesílání), úplná využití auditování, Multi-Factor Authentication (např. Azure MFA), záznam relace a další.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-166">Next to full mobility, Awingu's rich online workspace will give optimal security with granular controls (e.g. downloading/uploading), full usage auditing, Multi-Factor Authentication (e.g. Azure MFA), session recording and more.</span></span> <span data-ttu-id="8a6fe-167">Out-of-the-box, Awingu umožňuje dokumentu a sdílení aplikací relace pro optimalizované a zabezpečenou spolupráci.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-167">Out-of-the-box, Awingu enables document and application session sharing for optimized and secure collaboration.</span></span>
<span data-ttu-id="8a6fe-168">Na Awingu řešení je více klientů, službu AD a otevřené rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-168">Awingu's solution is multi-tenant, multi-AD and open API.</span></span> <span data-ttu-id="8a6fe-169">Se používají v malých a velkých firmách, poskytovatele cloudových služeb a [ISV](http://www.isv2saas.com).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-169">It is used by small and large businesses, Cloud Service Providers and [ISVs](http://www.isv2saas.com).</span></span> <span data-ttu-id="8a6fe-170">Těchto zákazníků ocení zejména snadné používání, snadná instalace a nízké náklady na vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-170">These customers especially appreciate the easy-of-use, ease-to-install and low TCO.</span></span>

<span data-ttu-id="8a6fe-171">Je Awingu vše v jednom [dostupné v Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) s 2 předdefinované souběžných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-171">Awingu All-in-One is [available in the Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) with 2 built-in concurrent users.</span></span> <span data-ttu-id="8a6fe-172">Další licence jsou k dispozici prostřednictvím [širokou škálu distributorů a prodejce](http://www.awingu.com/reseller).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-172">Additional licenses are available through a [wide range of distributors and resellers](http://www.awingu.com/reseller).</span></span>

<span data-ttu-id="8a6fe-173">Další informace o [Awingu na jako alternativní do Azure Remoteappu](http://alternative-for-azure-remoteapp.awingu.com/).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-173">Learn more about [Awingu on as alternative to Azure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).</span></span>


> <span data-ttu-id="8a6fe-174">Primární umístění: Belgie</span><span class="sxs-lookup"><span data-stu-id="8a6fe-174">Primary location: Belgium</span></span>
> 
> <span data-ttu-id="8a6fe-175">Operační oblastí: Severní Americe, regionu EMEA a Brazílie</span><span class="sxs-lookup"><span data-stu-id="8a6fe-175">Operating regions: EMEA, North America and Brazil</span></span>
> 
> <span data-ttu-id="8a6fe-176">Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak</span><span class="sxs-lookup"><span data-stu-id="8a6fe-176">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span> 
> 
> <span data-ttu-id="8a6fe-177">**Globální**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-177">**Global**:</span></span>
> 
> <span data-ttu-id="8a6fe-178">Arnaudu Marlière, podílu</span><span class="sxs-lookup"><span data-stu-id="8a6fe-178">Arnaud Marlière, CMO</span></span>
> 
> <span data-ttu-id="8a6fe-179">E-mailu:[arnaud@awingu.com](mailto:arnaud@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-179">Email: [arnaud@awingu.com](mailto:arnaud@awingu.com)</span></span>
> 
> <span data-ttu-id="8a6fe-180">Phone: +1 646 583 3025</span><span class="sxs-lookup"><span data-stu-id="8a6fe-180">Phone: +1 646 583 3025</span></span>
> 
> <span data-ttu-id="8a6fe-181">**Ústředí Belgie**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-181">**Belgium HQ**:</span></span>
> 
> <span data-ttu-id="8a6fe-182">Ottergemsesteenweg-Zuid 808 B44</span><span class="sxs-lookup"><span data-stu-id="8a6fe-182">Ottergemsesteenweg-Zuid 808 B44</span></span>
> 
> <span data-ttu-id="8a6fe-183">9000 služ</span><span class="sxs-lookup"><span data-stu-id="8a6fe-183">9000 Gent</span></span>
> 
> <span data-ttu-id="8a6fe-184">E-mailu:[info@awingu.com](mailto:info@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-184">Email: [info@awingu.com](mailto:info@awingu.com)</span></span> 
> 
> <span data-ttu-id="8a6fe-185">Telefon: + 32 9 296 40 11</span><span class="sxs-lookup"><span data-stu-id="8a6fe-185">Phone: +32 9 296 40 11</span></span>
> 
> <span data-ttu-id="8a6fe-186">**USA**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-186">**USA**:</span></span>
> 
> <span data-ttu-id="8a6fe-187">7. podlaží 1177 průměr z Americas,</span><span class="sxs-lookup"><span data-stu-id="8a6fe-187">7th floor, 1177 Ave of the Americas,</span></span>
> 
> <span data-ttu-id="8a6fe-188">New York, NY 10036</span><span class="sxs-lookup"><span data-stu-id="8a6fe-188">New York, NY 10036</span></span>
> 
> <span data-ttu-id="8a6fe-189">E-mailu:[info.us@awingu.com](mailto:info.us@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-189">Email: [info.us@awingu.com](mailto:info.us@awingu.com)</span></span>

### <a name="microsoft-hosted-service-provider"></a><span data-ttu-id="8a6fe-190">Microsoft hostované poskytovatele služeb</span><span class="sxs-lookup"><span data-stu-id="8a6fe-190">Microsoft Hosted Service Provider</span></span>
<span data-ttu-id="8a6fe-191">Hostování partnery obvykle nabízejí plně spravovaná hostované Windows desktop a služba aplikace, které mohou zahrnovat Správa prostředků Azure, operačních systémů, aplikací a pomocí partnerovi technickou podporu této licenční smlouvy s Microsoft a jiných softwarové poskytovatele spolu se služba Zprostředkovatel licenční smlouvu umožňující následný z odběratele přístup licence (SAL).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-191">Hosting partners typically offer a fully managed hosted Windows desktop and application service, which may include managing the Azure resources, operating systems, applications, and helpdesk using the partner's licensing agreements with Microsoft and other software providers along with being a Service Provider License Agreement to allow reselling of Subscriber Access License (SAL).</span></span> <span data-ttu-id="8a6fe-192">Následující informace poskytují podrobnosti a kontaktní informace pro některé z hostitelů, které se specializují na zákazníky s jejich Azure RemoteApp migrace, které.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-192">The following information provides details and contact information for some of the hosters that specialize in assisting customers with their Azure RemoteApp migration.</span></span> <span data-ttu-id="8a6fe-193">Podívejte se na [aktuální seznam zprostředkovatelů služby hostované](http://aka.ms/rdsonazurecertified) který dokončili vzdálené plochy na IaaS učení cestu a hodnocení.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-193">Check out [the current list of Hosted Service Providers](http://aka.ms/rdsonazurecertified) that have completed the RDS on IaaS learning path and assessment.</span></span>  

### <a name="citrix-service-provider-program"></a><span data-ttu-id="8a6fe-194">Program poskytovatele služeb systému Citrix</span><span class="sxs-lookup"><span data-stu-id="8a6fe-194">Citrix Service Provider Program</span></span>
<span data-ttu-id="8a6fe-195">Program poskytovatele služeb Citrix snadno pro poskytovatele služeb k poskytování jednoduchost virtuální cloud computing chcete SMB, jejich nabízení služby, které chtějí ve model snadno, průběžnými platbami.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-195">The Citrix Service Provider Program makes it easy for service providers to deliver the simplicity of virtual cloud computing to SMBs, offering them the services they want in an easy, pay-as-you-go model.</span></span> <span data-ttu-id="8a6fe-196">Poskytovatelé služeb Citrix rozvoji svého podnikání Microsoft SPLA a rozbalte jejich investic platformy vzdálené plochy pro jakékoli zařízení, přístup odkudkoli, nejširší podporu aplikace, bohaté prostředí, zvýšení zabezpečení a vyšší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-196">Citrix Service Providers grow their Microsoft SPLA businesses and expand their RDS platform investments with any device, anywhere access, the broadest application support, a rich experience, added security and increased scalability.</span></span> <span data-ttu-id="8a6fe-197">Naopak poskytovatelé služeb Citrix přilákat další odběratele, zvýšit spokojenost zákazníků a snížit provozní náklady.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-197">In turn, Citrix Service Providers attract more subscribers, increase customer satisfaction and reduce their operational costs.</span></span> <span data-ttu-id="8a6fe-198">[Další informace](http://www.citrix.com/products/service-providers.html) nebo [najít partnera](https://www.citrix.com/buy/partnerlocator.html).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-198">[Learn more](http://www.citrix.com/products/service-providers.html) or [find a partner](https://www.citrix.com/buy/partnerlocator.html).</span></span>

#### <a name="acuutech"></a><span data-ttu-id="8a6fe-199">Acuutech</span><span class="sxs-lookup"><span data-stu-id="8a6fe-199">Acuutech</span></span>
<span data-ttu-id="8a6fe-200">[Acuutech](http://www.acuutech.com) se specializuje na poskytování hostované plochy řešení doručování úplné ploše a aplikacím ISV prostředí založený na technologii Microsoft na základní globální klienta z Azure a vlastních datových center.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-200">[Acuutech](http://www.acuutech.com) specializes in providing hosted desktop solutions, delivering full desktop and ISV applications experiences built on Microsoft technology to a global client base from Azure and their own datacenters.</span></span>

> <span data-ttu-id="8a6fe-201">Primární umístění: Londýn, Spojené království; Singapur; Houston, TX</span><span class="sxs-lookup"><span data-stu-id="8a6fe-201">Primary location: London, UK; Singapore; Houston, TX</span></span>
> 
> <span data-ttu-id="8a6fe-202">Operace oblast: po celém světě</span><span class="sxs-lookup"><span data-stu-id="8a6fe-202">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="8a6fe-203">Partner stav: Gold</span><span class="sxs-lookup"><span data-stu-id="8a6fe-203">Partner status: Gold</span></span>
> 
> <span data-ttu-id="8a6fe-204">Zprostředkovatele služby Microsoft Cloud: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-204">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="8a6fe-205">Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak</span><span class="sxs-lookup"><span data-stu-id="8a6fe-205">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="8a6fe-206">Migrace řešení Azure RemoteApp: Ano, [Další informace](http://www.acuutech.com/ara-migration/)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-206">Azure RemoteApp migration solutions: Yes, [learn more](http://www.acuutech.com/ara-migration/)</span></span>
> 
> <span data-ttu-id="8a6fe-207">**Spojené království**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-207">**United Kingdom**:</span></span>
>   
> <span data-ttu-id="8a6fe-208">5/6 Yorku úklidové, Langston silniční</span><span class="sxs-lookup"><span data-stu-id="8a6fe-208">5/6 York House, Langston Road,</span></span>
>   
> <span data-ttu-id="8a6fe-209">Loughton, Essexu IG10 3TQ</span><span class="sxs-lookup"><span data-stu-id="8a6fe-209">Loughton, Essex IG10 3TQ</span></span>
>   
> <span data-ttu-id="8a6fe-210">Telefon: + 44 (0) 20 8502 2155</span><span class="sxs-lookup"><span data-stu-id="8a6fe-210">Phone: +44 (0) 20 8502 2155</span></span>
> 
> <span data-ttu-id="8a6fe-211">**Singapur**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-211">**Singapore**:</span></span>
>   
> <span data-ttu-id="8a6fe-212">100 Cecil ulici, #09-02</span><span class="sxs-lookup"><span data-stu-id="8a6fe-212">100 Cecil Street, #09-02,</span></span> 
>   
> <span data-ttu-id="8a6fe-213">Celém světě, Singapur 069532</span><span class="sxs-lookup"><span data-stu-id="8a6fe-213">The Globe, Singapore 069532</span></span>
> 
> <span data-ttu-id="8a6fe-214">Phone: 4933 6709 + 65</span><span class="sxs-lookup"><span data-stu-id="8a6fe-214">Phone: +65 6709 4933</span></span>
>   
> <span data-ttu-id="8a6fe-215">**Severní Amerika**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-215">**North America**:</span></span>
>   
> <span data-ttu-id="8a6fe-216">3601 S. Sandman St.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-216">3601 S. Sandman St.</span></span>
>   
> <span data-ttu-id="8a6fe-217">Sada 200, Houston, TX 77098</span><span class="sxs-lookup"><span data-stu-id="8a6fe-217">Suite 200, Houston, TX 77098</span></span>
>   
> <span data-ttu-id="8a6fe-218">Phone: +1 713 691 0800</span><span class="sxs-lookup"><span data-stu-id="8a6fe-218">Phone: +1 713 691 0800</span></span>

#### <a name="aspex"></a><span data-ttu-id="8a6fe-219">ASPEX</span><span class="sxs-lookup"><span data-stu-id="8a6fe-219">ASPEX</span></span>
<span data-ttu-id="8a6fe-220">[ASPEX](http://www.aspex.be/en) se specializuje na ISV přechodu do cloudu a ISV' vyhledávání za účelem optimalizace jejich aktuální nastavení cloudu.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-220">[ASPEX](http://www.aspex.be/en) specializes in ISVs transitioning to the Cloud and ISV‘ looking to optimize their current cloud setups.</span></span> <span data-ttu-id="8a6fe-221">ASPEX nabízí širokou škálu spravované služby, devops a konzultační služby.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-221">ASPEX offers a wide range of managed services, devops, and consulting services.</span></span>  

> <span data-ttu-id="8a6fe-222">Primární umístění: Antverpy, Belgie</span><span class="sxs-lookup"><span data-stu-id="8a6fe-222">Primary location: Antwerp, Belgium</span></span>
> 
> <span data-ttu-id="8a6fe-223">Operace oblast: západní Evropa</span><span class="sxs-lookup"><span data-stu-id="8a6fe-223">Operation region: Western Europe</span></span>
> 
> <span data-ttu-id="8a6fe-224">Partner stav: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-224">Partner status: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span></span>
> 
> <span data-ttu-id="8a6fe-225">Zprostředkovatele služby Microsoft Cloud: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-225">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="8a6fe-226">Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak</span><span class="sxs-lookup"><span data-stu-id="8a6fe-226">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="8a6fe-227">Migrace řešení Azure RemoteApp: Ano, [Další informace](https://www.aspex.be/en/azure-remote-apps)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-227">Azure RemoteApp migration solutions: Yes, [learn more](https://www.aspex.be/en/azure-remote-apps)</span></span>
> 
> <span data-ttu-id="8a6fe-228">Phone: +3232202198</span><span class="sxs-lookup"><span data-stu-id="8a6fe-228">Phone: +3232202198</span></span>
> 
> <span data-ttu-id="8a6fe-229">E-mailu:[info@aspex.be](mailto:info@aspex.be)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-229">Mail: [info@aspex.be](mailto:info@aspex.be)</span></span>
> 
> <span data-ttu-id="8a6fe-230">Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-230">Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span></span>

#### <a name="caasecom"></a><span data-ttu-id="8a6fe-231">Caase.com</span><span class="sxs-lookup"><span data-stu-id="8a6fe-231">Caase.com</span></span>
<span data-ttu-id="8a6fe-232">[Caase.com](http://www.caase.com/) pomáhá podnikům, místní správy, jiných vládních subjektů a poskytovatelem zdravotní instituce s cestě směrem inteligentnější způsob práce v Microsoft cloudu.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-232">[Caase.com](http://www.caase.com/) helps businesses, local governments, non-governmental bodies and healthcare institutions with their journey towards a smarter way of work in the Microsoft Cloud.</span></span> <span data-ttu-id="8a6fe-233">Probíhá produktivitu a zabezpečení v jakémkoli místě s jakýmkoli a nízké náklady na IT.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-233">Being productive and secure at any place, with any device and at low IT cost.</span></span> <span data-ttu-id="8a6fe-234">Caase.com je true specialisty Microsoft Office365, Azure, Enterprise Mobility a zabezpečení a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-234">Caase.com is a true specialist for Microsoft Office365, Azure, Enterprise Mobility and Security and Windows.</span></span> <span data-ttu-id="8a6fe-235">U naše poradenské služby migrace, přijetí programy, školení správy a podpory Caase.com vytvoří optimalizované a zabezpečené platformu pro spolupráci pro obě zákazníků zaměstnance, partnery a dodavateli.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-235">With our consultancy, migration services, adoption programs, training, management and support Caase.com creates an optimized and secure platform for collaboration for both customers’ employees, partners and suppliers.</span></span>
<span data-ttu-id="8a6fe-236">Caase.com je mastermind vzdálené prostoru Azure (mobilní síti na pracovišti) a digitální síti na pracovišti (sociální Intranet).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-236">Caase.com is the mastermind of the Azure Remote Workspace (mobile workplace) and the Digital Workplace (Social Intranet).</span></span> <span data-ttu-id="8a6fe-237">Obě řešení – dosáhnout s přijetím – jsou foundation, která zajistí, že uživatelé z těchto řešení prostředí příjemný, úspěšné a efektivní v jejich trasy ke cloudu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-237">Both solutions – accomplished with adoption – are the foundation which ensures that the users of these solutions have the most pleasant, successful and effective experience in their route to the Microsoft Cloud.</span></span>
<span data-ttu-id="8a6fe-238">Holandská překlad ánd podpůrné film přes zde: http://caase.com/over-ons/</span><span class="sxs-lookup"><span data-stu-id="8a6fe-238">Dutch translation ánd a supporting movie over here: http://caase.com/over-ons/</span></span>

> <span data-ttu-id="8a6fe-239">Operace oblast: na základě holandština, globální sítě</span><span class="sxs-lookup"><span data-stu-id="8a6fe-239">Operation region: Dutch based, global reach</span></span>
> 
> <span data-ttu-id="8a6fe-240">Partner stav: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-240">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span></span>
> 
> <span data-ttu-id="8a6fe-241">Zprostředkovatele služby Microsoft Cloud: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-241">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="8a6fe-242">Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak</span><span class="sxs-lookup"><span data-stu-id="8a6fe-242">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="8a6fe-243">Migrace řešení Azure RemoteApp: Ano, [Další](http://caase.com/diensten/microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-243">Azure RemoteApp migration solutions: Yes, [learn more](http://caase.com/diensten/microsoft-azure/).</span></span>
> 
> 
> <span data-ttu-id="8a6fe-244">Nizozemsko:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-244">Netherlands:</span></span>
> 
> <span data-ttu-id="8a6fe-245">Rigtersbleek-Zandvoort 10 (De Spinnerij)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-245">Rigtersbleek-Zandvoort 10 (De Spinnerij)</span></span>
> 
> <span data-ttu-id="8a6fe-246">7521 BE, nizozemském Enschede</span><span class="sxs-lookup"><span data-stu-id="8a6fe-246">7521 BE, Enschede</span></span>
> 
> <span data-ttu-id="8a6fe-247">Phone: +31 (0) 88 4320 000</span><span class="sxs-lookup"><span data-stu-id="8a6fe-247">Phone: +31 (0) 88 4320 000</span></span>


#### <a name="nerdio"></a><span data-ttu-id="8a6fe-248">Nerdio</span><span class="sxs-lookup"><span data-stu-id="8a6fe-248">Nerdio</span></span>
<span data-ttu-id="8a6fe-249">[Nerdio pro Azure](http://getnerdio.com/nfa/) platformu automatizace IT, který nabízí ridiculously jednoduché zřizování, správu a optimalizace dokončení prostředí IT v cloudu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-249">[Nerdio for Azure](http://getnerdio.com/nfa/) is an IT automation platform that delivers ridiculously simple provisioning, management and optimization of complete IT environments in the Microsoft cloud.</span></span> <span data-ttu-id="8a6fe-250">Stát virtuálním plochám, vzdálené aplikace a servery v několika hodin.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-250">Stand up virtual desktops, remote apps and servers in a couple of hours.</span></span> <span data-ttu-id="8a6fe-251">Spravovat prostředí v tři klepnutí nebo méně s Nerdio portál pro správu.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-251">Administer the environment in three clicks or less with Nerdio Admin Portal.</span></span> <span data-ttu-id="8a6fe-252">Použití inteligentního automatické škálování a uložit 40 až 60 % prostředky Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-252">Use intelligent auto-scaling and save 40 to 60% in Azure IaaS resources.</span></span>

> <span data-ttu-id="8a6fe-253">Primární umístění: operace Chicagu, IL oblast: stav po celém světě partnera: [Gold](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) poskytovatele cloudové služby společnosti Microsoft: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-253">Primary location: Chicago, IL Operation region: Worldwide Partner status: [Gold](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="8a6fe-254">Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak</span><span class="sxs-lookup"><span data-stu-id="8a6fe-254">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="8a6fe-255">Migrace řešení Azure RemoteApp: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-255">Azure RemoteApp migration solutions: Yes</span></span>
> 
> 
> <span data-ttu-id="8a6fe-256">8001 Lincoln průměr</span><span class="sxs-lookup"><span data-stu-id="8a6fe-256">8001 Lincoln Ave</span></span>
> 
> <span data-ttu-id="8a6fe-257">Suite 212</span><span class="sxs-lookup"><span data-stu-id="8a6fe-257">Suite 212</span></span>
> 
> <span data-ttu-id="8a6fe-258">Skokie, IL 60077</span><span class="sxs-lookup"><span data-stu-id="8a6fe-258">Skokie, IL 60077</span></span>
> 
> <span data-ttu-id="8a6fe-259">USA</span><span class="sxs-lookup"><span data-stu-id="8a6fe-259">USA</span></span>
> 
> <span data-ttu-id="8a6fe-260">Ext. 4NERDIO (844)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-260">(844) 4NERDIO ext.</span></span> <span data-ttu-id="8a6fe-261">6</span><span class="sxs-lookup"><span data-stu-id="8a6fe-261">6</span></span>
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a><span data-ttu-id="8a6fe-262">**SaaSplaza**</span><span class="sxs-lookup"><span data-stu-id="8a6fe-262">**SaaSplaza**</span></span>
<span data-ttu-id="8a6fe-263">[SaaSplaza](http://www.saasplaza.com/) nabízí kompletní Microsoft Dynamics portfolií (navigaci, AX, zásad skupiny, SL, CRM) privátní a veřejné cloudu (Azure).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-263">[SaaSplaza](http://www.saasplaza.com/) offers complete Microsoft Dynamics portfolio (NAV, AX, GP, SL, CRM) private and public cloud (Azure).</span></span>

> <span data-ttu-id="8a6fe-264">Primární umístění: Nizozemsko</span><span class="sxs-lookup"><span data-stu-id="8a6fe-264">Primary location: Netherlands</span></span>
> 
> <span data-ttu-id="8a6fe-265">Operace oblast: po celém světě</span><span class="sxs-lookup"><span data-stu-id="8a6fe-265">Operation Region: Worldwide</span></span>
> 
> <span data-ttu-id="8a6fe-266">Partner stav: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span><span class="sxs-lookup"><span data-stu-id="8a6fe-266">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span></span>
> 
> <span data-ttu-id="8a6fe-267">Zprostředkovatele služby Microsoft Cloud: Ano</span><span class="sxs-lookup"><span data-stu-id="8a6fe-267">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="8a6fe-268">Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak</span><span class="sxs-lookup"><span data-stu-id="8a6fe-268">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="8a6fe-269">**EMEA**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-269">**EMEA**:</span></span>
> 
> <span data-ttu-id="8a6fe-270">Prins Mauritslaan 29 35</span><span class="sxs-lookup"><span data-stu-id="8a6fe-270">Prins Mauritslaan 29-35</span></span>
> 
> <span data-ttu-id="8a6fe-271">Badhoevedorp 71 lineárního programování ÚLOH</span><span class="sxs-lookup"><span data-stu-id="8a6fe-271">71 LP Badhoevedorp</span></span>
> 
> <span data-ttu-id="8a6fe-272">Nizozemsko</span><span class="sxs-lookup"><span data-stu-id="8a6fe-272">The Netherlands</span></span>
> 
> <span data-ttu-id="8a6fe-273">Phone: 8060 547 20 +31</span><span class="sxs-lookup"><span data-stu-id="8a6fe-273">Phone: +31 20 547 8060</span></span> 
> 
>  <span data-ttu-id="8a6fe-274">**Americas**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-274">**Americas**:</span></span>
> 
> <span data-ttu-id="8a6fe-275">171 Sasko silniční, Suite 105</span><span class="sxs-lookup"><span data-stu-id="8a6fe-275">171 Saxony Road, Suite 105</span></span>
> 
> <span data-ttu-id="8a6fe-276">92024 Encinitas, certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="8a6fe-276">Encinitas, CA 92024</span></span>
> 
> <span data-ttu-id="8a6fe-277">Síť San Diego</span><span class="sxs-lookup"><span data-stu-id="8a6fe-277">San Diego</span></span>
> 
> <span data-ttu-id="8a6fe-278">Spojené státy</span><span class="sxs-lookup"><span data-stu-id="8a6fe-278">United States</span></span>
> 
> <span data-ttu-id="8a6fe-279">Phone: +1 858 385 8900</span><span class="sxs-lookup"><span data-stu-id="8a6fe-279">Phone: +1 858 385 8900</span></span> 
> 
> <span data-ttu-id="8a6fe-280">**APAC**:</span><span class="sxs-lookup"><span data-stu-id="8a6fe-280">**APAC**:</span></span>
> 
> <span data-ttu-id="8a6fe-281">105 Cecil ulice</span><span class="sxs-lookup"><span data-stu-id="8a6fe-281">105 Cecil Street</span></span>
>    
> <span data-ttu-id="8a6fe-282">\#11-08, Osmiúhelník</span><span class="sxs-lookup"><span data-stu-id="8a6fe-282">\#11-08, The Octagon</span></span>
> 
> <span data-ttu-id="8a6fe-283">Singapur 069534</span><span class="sxs-lookup"><span data-stu-id="8a6fe-283">Singapore 069534</span></span>
> 
> <span data-ttu-id="8a6fe-284">Singapur</span><span class="sxs-lookup"><span data-stu-id="8a6fe-284">Singapore</span></span>
>   
> <span data-ttu-id="8a6fe-285">Phone - Singapur: 6591 6222 + 65</span><span class="sxs-lookup"><span data-stu-id="8a6fe-285">Phone - Singapore: +65 6222 6591</span></span>
> 
> <span data-ttu-id="8a6fe-286">Phone – Austrálie: + 61 2 8310 5568</span><span class="sxs-lookup"><span data-stu-id="8a6fe-286">Phone - Australia: +61 2 8310 5568</span></span> 
>    
> <span data-ttu-id="8a6fe-287">Phone – Nový Zéland: 0321 488 4 + 64</span><span class="sxs-lookup"><span data-stu-id="8a6fe-287">Phone - New Zealand: +64 4 488 0321</span></span>
> 
## <a name="need-more-help"></a><span data-ttu-id="8a6fe-288">Potřebujete další pomoc?</span><span class="sxs-lookup"><span data-stu-id="8a6fe-288">Need more help?</span></span>
<span data-ttu-id="8a6fe-289">Stále potřebujete pomoc, výběr nebo máte další otázky?</span><span class="sxs-lookup"><span data-stu-id="8a6fe-289">Still need help choosing or have further questions?</span></span> <span data-ttu-id="8a6fe-290">Jak získat nápovědu, použijte jednu z následujících metod.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-290">Use one of the following methods to get help.</span></span> 

1. <span data-ttu-id="8a6fe-291">E-mailu nás na adrese [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-291">Email us at [arainfo@microsoft.com](mailto:arainfo@microsoft.com).</span></span>
2. <span data-ttu-id="8a6fe-292">Obraťte se na [podporu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-292">Contact [Azure support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="8a6fe-293">Začněte otevřením [případu podpory Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-293">Start by opening an [Azure support case](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
3. <span data-ttu-id="8a6fe-294">Kontaktovat nás telefonicky.</span><span class="sxs-lookup"><span data-stu-id="8a6fe-294">Call us.</span></span> <span data-ttu-id="8a6fe-295">[Nalezení místní prodejní čísla](https://azure.microsoft.com/overview/sales-number/).</span><span class="sxs-lookup"><span data-stu-id="8a6fe-295">[Find a local sales number](https://azure.microsoft.com/overview/sales-number/).</span></span>

