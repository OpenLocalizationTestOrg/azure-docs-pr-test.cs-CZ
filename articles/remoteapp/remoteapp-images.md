---
title: "aaaWhat je v hello Image šablony Azure RemoteApp? | Dokumentace Microsoftu"
description: "Další informace o imagích šablon hello součástí Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ea012cec8dc581a8bd4a5a138ce302de19d5c6af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-in-hello-azure-remoteapp-template-images"></a><span data-ttu-id="ea83d-104">Co jsou Image šablony hello Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="ea83d-104">What is in hello Azure RemoteApp template images?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ea83d-105">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="ea83d-105">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="ea83d-106">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ea83d-106">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="ea83d-107">Vaše předplatné Azure RemoteAppu obsahuje tři image šablon:</span><span class="sxs-lookup"><span data-stu-id="ea83d-107">Your Azure RemoteApp subscription includes three template images:</span></span>

* <span data-ttu-id="ea83d-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="ea83d-108">Windows Server 2012</span></span>
* <span data-ttu-id="ea83d-109">Microsoft Office 365 ProPlus (požadováno předplatné služeb Office 365)</span><span class="sxs-lookup"><span data-stu-id="ea83d-109">Microsoft Office 365 ProPlus (Office 365 subscription required)</span></span>
* <span data-ttu-id="ea83d-110">Microsoft Office 2013 Professional Plus (pouze zkušební verze)</span><span class="sxs-lookup"><span data-stu-id="ea83d-110">Microsoft Office 2013 Professional Plus (trial only)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea83d-111">Vaše předplatné Azure Remoteappu vám uděluje že přístup toohello software v hello Image, s výjimkou hello Office 365 ProPlus, který vyžaduje samostatné předplatné a Office 2013, který nelze použít v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ea83d-111">Your Azure RemoteApp subscription grants you access toohello software in hello images, with hello exception of Office 365 ProPlus, which requires a separate subscription, and Office 2013, which cannot be used in production.</span></span> <span data-ttu-id="ea83d-112">To znamená, že můžete sdílet hello programy nebo aplikace v imagích šablon hello s uživateli.</span><span class="sxs-lookup"><span data-stu-id="ea83d-112">This means that you can share hello programs or apps on hello template images with your users.</span></span> <span data-ttu-id="ea83d-113">Například pokud vytvoříte kolekci, která využívá image systému Windows Server 2012 R2 hello, můžete publikovat System Center Endpoint Protection pro uživatele tooaccess prostřednictvím vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="ea83d-113">For example, if you create a collection that uses hello Windows Server 2012 R2 image, you can publish System Center Endpoint Protection for users tooaccess through RemoteApp.</span></span>
> 
> <span data-ttu-id="ea83d-114">Podívejte se na hello [podrobnosti o licencování Remoteappu](remoteapp-licensing.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ea83d-114">Check out hello [RemoteApp licensing details](remoteapp-licensing.md) for more information.</span></span> <span data-ttu-id="ea83d-115">A [použití Officu s Azure Remoteappem](remoteapp-o365.md) pro hello Office licenční informace.</span><span class="sxs-lookup"><span data-stu-id="ea83d-115">And [Using Office with Azure RemoteApp](remoteapp-o365.md) for hello Office licensing info.</span></span>
> 
> 

<span data-ttu-id="ea83d-116">Přečtěte si podrobnosti o tom, co každý image obsahuje.</span><span class="sxs-lookup"><span data-stu-id="ea83d-116">Read on for details on what each image contains.</span></span>

## <a name="windows-server-2012-r2--hello-vanilla-image"></a><span data-ttu-id="ea83d-117">Windows Server 2012 R2 ("hello vanilla image)</span><span class="sxs-lookup"><span data-stu-id="ea83d-117">Windows Server 2012 R2  ("hello vanilla image")</span></span>
<span data-ttu-id="ea83d-118">Tento image je založený na operačním systému Microsoft Windows Server 2012 R2 Datacenter a hello tyto role a funkce nainstaloval toomeet hello požadavky pro Image šablony Azure RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="ea83d-118">This image is based on Microsoft Windows Server 2012 R2 Datacenter operating system and has hello following roles and features installed toomeet hello requirements for Azure RemoteApp template images:</span></span>

* <span data-ttu-id="ea83d-119">Rozhraní .NET Framework 4.5, 3.5.1, 3.5</span><span class="sxs-lookup"><span data-stu-id="ea83d-119">.NET Framework 4.5, 3.5.1, 3.5</span></span>
* <span data-ttu-id="ea83d-120">Desktopové prostředí</span><span class="sxs-lookup"><span data-stu-id="ea83d-120">Desktop Experience</span></span>
* <span data-ttu-id="ea83d-121">Služba podpory rukopisu</span><span class="sxs-lookup"><span data-stu-id="ea83d-121">Ink and Handwriting Services</span></span>
* <span data-ttu-id="ea83d-122">Media Foundation</span><span class="sxs-lookup"><span data-stu-id="ea83d-122">Media Foundation</span></span>
* <span data-ttu-id="ea83d-123">Hostitel relace vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="ea83d-123">Remote Desktop Session Host</span></span>
* <span data-ttu-id="ea83d-124">Windows PowerShell 4.0</span><span class="sxs-lookup"><span data-stu-id="ea83d-124">Windows PowerShell 4.0</span></span>
* <span data-ttu-id="ea83d-125">Integrované skriptovací prostředí (ISE) v prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="ea83d-125">Windows PowerShell ISE</span></span>
* <span data-ttu-id="ea83d-126">Podpora subsystémů WoW64</span><span class="sxs-lookup"><span data-stu-id="ea83d-126">WoW64 Support</span></span>

<span data-ttu-id="ea83d-127">Tento image má také následující aplikace nainstalované hello:</span><span class="sxs-lookup"><span data-stu-id="ea83d-127">This image also has hello following applications installed:</span></span>

* <span data-ttu-id="ea83d-128">Adobe Flash Player</span><span class="sxs-lookup"><span data-stu-id="ea83d-128">Adobe Flash Player</span></span>
* <span data-ttu-id="ea83d-129">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="ea83d-129">Microsoft Silverlight</span></span>
* <span data-ttu-id="ea83d-130">Microsoft System Center 2012 Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="ea83d-130">Microsoft System Center 2012 Endpoint Protection</span></span>
* <span data-ttu-id="ea83d-131">Microsoft Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="ea83d-131">Microsoft Windows Media Player</span></span>

## <a name="microsoft-office-365-proplus-subscription-required"></a><span data-ttu-id="ea83d-132">Microsoft Office 365 ProPlus (požadován odběr)</span><span class="sxs-lookup"><span data-stu-id="ea83d-132">Microsoft Office 365 ProPlus (subscription required)</span></span>
<span data-ttu-id="ea83d-133">Office 365 je hello nejvíce požadovaná aplikace, takže jsme vytvořili "vlastní" image pro vás toowork s.</span><span class="sxs-lookup"><span data-stu-id="ea83d-133">Office 365 is hello most requested application, so we created a "custom" image for you toowork with.</span></span>

<span data-ttu-id="ea83d-134">Tento image je rozšířením hello vanilla image a hello následující komponenty aplikace Microsoft Office 365 ProPlus nainstaloval kromě toohello komponent popsaných v imagi Windows Server 2012 R2 hello:</span><span class="sxs-lookup"><span data-stu-id="ea83d-134">This image is an extension of hello vanilla image and has hello following components of Microsoft Office 365 ProPlus installed in addition toohello components described in hello Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="ea83d-135">Access</span><span class="sxs-lookup"><span data-stu-id="ea83d-135">Access</span></span>
* <span data-ttu-id="ea83d-136">Excel</span><span class="sxs-lookup"><span data-stu-id="ea83d-136">Excel</span></span>
* <span data-ttu-id="ea83d-137">Lync</span><span class="sxs-lookup"><span data-stu-id="ea83d-137">Lync</span></span>
* <span data-ttu-id="ea83d-138">OneNote</span><span class="sxs-lookup"><span data-stu-id="ea83d-138">OneNote</span></span>
* <span data-ttu-id="ea83d-139">OneDrive pro firmy (Všimněte si, Tento agent hello synchronizace není podporována pro použití s Azure Remoteappem)</span><span class="sxs-lookup"><span data-stu-id="ea83d-139">OneDrive for Business (note that hello sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="ea83d-140">Outlook</span><span class="sxs-lookup"><span data-stu-id="ea83d-140">Outlook</span></span>
* <span data-ttu-id="ea83d-141">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="ea83d-141">PowerPoint</span></span>
* <span data-ttu-id="ea83d-142">Word</span><span class="sxs-lookup"><span data-stu-id="ea83d-142">Word</span></span>
* <span data-ttu-id="ea83d-143">Nástroje kontroly pravopisu systému Microsoft Office</span><span class="sxs-lookup"><span data-stu-id="ea83d-143">Microsoft Office Proofing Tools</span></span>

<span data-ttu-id="ea83d-144">Hello image také zahrnuje Visio Pro a Project Pro.</span><span class="sxs-lookup"><span data-stu-id="ea83d-144">hello image also includes Visio Pro and Project Pro.</span></span>

<span data-ttu-id="ea83d-145">A hello aplikací také následující:</span><span class="sxs-lookup"><span data-stu-id="ea83d-145">And hello following applications, as well:</span></span>

* <span data-ttu-id="ea83d-146">Nativní klient SQL</span><span class="sxs-lookup"><span data-stu-id="ea83d-146">SQL Native client</span></span>
* <span data-ttu-id="ea83d-147">Ovladač ODBC</span><span class="sxs-lookup"><span data-stu-id="ea83d-147">ODBC Driver</span></span>
* <span data-ttu-id="ea83d-148">Klient pro získávání dat SQL Server</span><span class="sxs-lookup"><span data-stu-id="ea83d-148">SQL Server Data Mining client</span></span>
* <span data-ttu-id="ea83d-149">Klient MasterDataServices</span><span class="sxs-lookup"><span data-stu-id="ea83d-149">MasterDataServices client</span></span>
* <span data-ttu-id="ea83d-150">Microsoft Publisher</span><span class="sxs-lookup"><span data-stu-id="ea83d-150">Microsoft Publisher</span></span>
* <span data-ttu-id="ea83d-151">PowerQuery</span><span class="sxs-lookup"><span data-stu-id="ea83d-151">PowerQuery</span></span>
* <span data-ttu-id="ea83d-152">PowerMap</span><span class="sxs-lookup"><span data-stu-id="ea83d-152">PowerMap</span></span>

<span data-ttu-id="ea83d-153">Plná funkčnost aplikací Office 365 ProPlus je k dispozici pouze pro uživatele, kteří mají plán Office 365 ProPlus.</span><span class="sxs-lookup"><span data-stu-id="ea83d-153">Full functionality of Office 365 ProPlus apps is available only for users who have an Office 365 ProPlus plan.</span></span> <span data-ttu-id="ea83d-154">Další informace o odběru hello Office 365 najdete v části plány [plány služeb Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea83d-154">For more details on hello Office 365 subscription plans see [Office 365 service plans](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span></span> <span data-ttu-id="ea83d-155">Máte další otázky?</span><span class="sxs-lookup"><span data-stu-id="ea83d-155">Still have questions?</span></span> <span data-ttu-id="ea83d-156">Podívejte se na hello [Office 365 + RemoteApp](remoteapp-o365.md) informace.</span><span class="sxs-lookup"><span data-stu-id="ea83d-156">Check out hello [Office 365 + RemoteApp](remoteapp-o365.md) information.</span></span> <span data-ttu-id="ea83d-157">Seznamte se také hello nový článek, [jak toouse předplatné služeb Office 365 s Azure Remoteappem](remoteapp-officesubscription.md).</span><span class="sxs-lookup"><span data-stu-id="ea83d-157">Also check out hello new article, [How toouse your Office 365 subscription with Azure RemoteApp](remoteapp-officesubscription.md).</span></span>

<span data-ttu-id="ea83d-158">Všimněte si, že budete potřebovat samostatně toolicense Office 365 ProPlus, Visio Pro a Project Pro – každá má své vlastní licenci.</span><span class="sxs-lookup"><span data-stu-id="ea83d-158">Note that you need toolicense Office 365 ProPlus, Visio Pro, and Project Pro separately - they each have their own license.</span></span>

## <a name="microsoft-office-2013-professional-plus-trial-only"></a><span data-ttu-id="ea83d-159">Microsoft Office 2013 Professional Plus (pouze zkušební verze)</span><span class="sxs-lookup"><span data-stu-id="ea83d-159">Microsoft Office 2013 Professional Plus (trial only)</span></span>
<span data-ttu-id="ea83d-160">Během hello bezplatné zkušební doby můžete testovat službu hello s bitovou kopií hello Office 2013.</span><span class="sxs-lookup"><span data-stu-id="ea83d-160">During hello free trial period, you can test hello service with hello Office 2013 image.</span></span>

<span data-ttu-id="ea83d-161">Tento image je rozšířením hello vanilla image a hello následující komponenty aplikace Microsoft Office 2013 Professional Plus nainstaloval kromě toohello komponent popsaných v imagi Windows Server 2012 R2 hello:</span><span class="sxs-lookup"><span data-stu-id="ea83d-161">This image is an extension of hello vanilla image and has hello following components of Microsoft Office 2013 Professional Plus installed in addition toohello components described in hello Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="ea83d-162">Access</span><span class="sxs-lookup"><span data-stu-id="ea83d-162">Access</span></span>
* <span data-ttu-id="ea83d-163">Excel</span><span class="sxs-lookup"><span data-stu-id="ea83d-163">Excel</span></span>
* <span data-ttu-id="ea83d-164">Lync</span><span class="sxs-lookup"><span data-stu-id="ea83d-164">Lync</span></span>
* <span data-ttu-id="ea83d-165">OneNote</span><span class="sxs-lookup"><span data-stu-id="ea83d-165">OneNote</span></span>
* <span data-ttu-id="ea83d-166">OneDrive pro firmy (Všimněte si, Tento agent hello synchronizace není podporována pro použití s Azure Remoteappem)</span><span class="sxs-lookup"><span data-stu-id="ea83d-166">OneDrive for Business (note that hello sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="ea83d-167">Outlook</span><span class="sxs-lookup"><span data-stu-id="ea83d-167">Outlook</span></span>
* <span data-ttu-id="ea83d-168">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="ea83d-168">PowerPoint</span></span>
* <span data-ttu-id="ea83d-169">Project</span><span class="sxs-lookup"><span data-stu-id="ea83d-169">Project</span></span>
* <span data-ttu-id="ea83d-170">Visio</span><span class="sxs-lookup"><span data-stu-id="ea83d-170">Visio</span></span>
* <span data-ttu-id="ea83d-171">Word</span><span class="sxs-lookup"><span data-stu-id="ea83d-171">Word</span></span>
* <span data-ttu-id="ea83d-172">Nástroje kontroly pravopisu systému Microsoft Office</span><span class="sxs-lookup"><span data-stu-id="ea83d-172">Microsoft Office Proofing Tools</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea83d-173">**Právní informace:** Tento image neobsahuje licenci aplikace Microsoft Office a*nelze ho použít pro produkci*.</span><span class="sxs-lookup"><span data-stu-id="ea83d-173">**Legal information:** This image does not include a Microsoft Office license and *cannot be used for production*.</span></span> <span data-ttu-id="ea83d-174">image Office 2013 Professional Plus Hello je určena pouze pro zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="ea83d-174">hello Office 2013 Professional Plus image is intended for trial use only.</span></span> <span data-ttu-id="ea83d-175">Pokud chcete aplikace Office toouse v Azure Remoteappu pro produkci, je nutné image Office 365 ProPlus toouse hello.</span><span class="sxs-lookup"><span data-stu-id="ea83d-175">If you want toouse Office apps in Azure RemoteApp for production, you need toouse hello Office 365 ProPlus image.</span></span> <span data-ttu-id="ea83d-176">Další informace o licencování Officu najdete v části [Použití  Officu 365 s Azure RemoteAppem](remoteapp-o365.md).</span><span class="sxs-lookup"><span data-stu-id="ea83d-176">For more details on licensing Office, see [Using Office 365 with Azure RemoteApp](remoteapp-o365.md)</span></span>
> 
> 

