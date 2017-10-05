---
title: "Co jsou image šablon Azure Remote Appu? | Dokumentace Microsoftu"
description: "Informace o imagích šablon, které jsou součástí Azure RemoteAppu."
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
ms.openlocfilehash: 9cd4e1a16a7c42bd00d9e543d7b62b72e9de3fc8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-in-the-azure-remoteapp-template-images"></a><span data-ttu-id="ba3b8-104">Co jsou image šablon Azure Remote Appu?</span><span class="sxs-lookup"><span data-stu-id="ba3b8-104">What is in the Azure RemoteApp template images?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ba3b8-105">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-105">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="ba3b8-106">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-106">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="ba3b8-107">Vaše předplatné Azure RemoteAppu obsahuje tři image šablon:</span><span class="sxs-lookup"><span data-stu-id="ba3b8-107">Your Azure RemoteApp subscription includes three template images:</span></span>

* <span data-ttu-id="ba3b8-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="ba3b8-108">Windows Server 2012</span></span>
* <span data-ttu-id="ba3b8-109">Microsoft Office 365 ProPlus (požadováno předplatné služeb Office 365)</span><span class="sxs-lookup"><span data-stu-id="ba3b8-109">Microsoft Office 365 ProPlus (Office 365 subscription required)</span></span>
* <span data-ttu-id="ba3b8-110">Microsoft Office 2013 Professional Plus (pouze zkušební verze)</span><span class="sxs-lookup"><span data-stu-id="ba3b8-110">Microsoft Office 2013 Professional Plus (trial only)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba3b8-111">Předplatné Azure RemoteAppu vám uděluje přístup k softwaru v imagích, s výjimkou Office 365 ProPlus, který vyžaduje samostatné předplatné, a Office 2013, který nejde použít v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-111">Your Azure RemoteApp subscription grants you access to the software in the images, with the exception of Office 365 ProPlus, which requires a separate subscription, and Office 2013, which cannot be used in production.</span></span> <span data-ttu-id="ba3b8-112">To znamená, že můžete sdílet programy nebo aplikace v imagích šablon s uživateli.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-112">This means that you can share the programs or apps on the template images with your users.</span></span> <span data-ttu-id="ba3b8-113">Pokud vytvoříte kolekci, která využívá image systému Windows Server 2012 R2, můžete publikovat aplikaci System Center Endpoint Protection uživatelům, kteří k ní pak budou mít přístup přes RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-113">For example, if you create a collection that uses the Windows Server 2012 R2 image, you can publish System Center Endpoint Protection for users to access through RemoteApp.</span></span>
> 
> <span data-ttu-id="ba3b8-114">Další informace naleznete v části [Podrobnosti o licencování RemoteAppu](remoteapp-licensing.md).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-114">Check out the [RemoteApp licensing details](remoteapp-licensing.md) for more information.</span></span> <span data-ttu-id="ba3b8-115">Informace o licencování Officu najde v části [Použití Officu s Azure RemoteAppem](remoteapp-o365.md).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-115">And [Using Office with Azure RemoteApp](remoteapp-o365.md) for the Office licensing info.</span></span>
> 
> 

<span data-ttu-id="ba3b8-116">Přečtěte si podrobnosti o tom, co každý image obsahuje.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-116">Read on for details on what each image contains.</span></span>

## <a name="windows-server-2012-r2--the-vanilla-image"></a><span data-ttu-id="ba3b8-117">Windows Server 2012 R2 (dále jen image vanilla)</span><span class="sxs-lookup"><span data-stu-id="ba3b8-117">Windows Server 2012 R2  ("the vanilla image")</span></span>
<span data-ttu-id="ba3b8-118">Tento image je založený na operačním systému Microsoft Windows Server 2012 R2 Datacenter a má nainstalované následující role a funkce, aby splnil požadavky pro image šablony Azure RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="ba3b8-118">This image is based on Microsoft Windows Server 2012 R2 Datacenter operating system and has the following roles and features installed to meet the requirements for Azure RemoteApp template images:</span></span>

* <span data-ttu-id="ba3b8-119">Rozhraní .NET Framework 4.5, 3.5.1, 3.5</span><span class="sxs-lookup"><span data-stu-id="ba3b8-119">.NET Framework 4.5, 3.5.1, 3.5</span></span>
* <span data-ttu-id="ba3b8-120">Desktopové prostředí</span><span class="sxs-lookup"><span data-stu-id="ba3b8-120">Desktop Experience</span></span>
* <span data-ttu-id="ba3b8-121">Služba podpory rukopisu</span><span class="sxs-lookup"><span data-stu-id="ba3b8-121">Ink and Handwriting Services</span></span>
* <span data-ttu-id="ba3b8-122">Media Foundation</span><span class="sxs-lookup"><span data-stu-id="ba3b8-122">Media Foundation</span></span>
* <span data-ttu-id="ba3b8-123">Hostitel relace vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="ba3b8-123">Remote Desktop Session Host</span></span>
* <span data-ttu-id="ba3b8-124">Windows PowerShell 4.0</span><span class="sxs-lookup"><span data-stu-id="ba3b8-124">Windows PowerShell 4.0</span></span>
* <span data-ttu-id="ba3b8-125">Integrované skriptovací prostředí (ISE) v prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="ba3b8-125">Windows PowerShell ISE</span></span>
* <span data-ttu-id="ba3b8-126">Podpora subsystémů WoW64</span><span class="sxs-lookup"><span data-stu-id="ba3b8-126">WoW64 Support</span></span>

<span data-ttu-id="ba3b8-127">Tento image má také nainstalované následující aplikace:</span><span class="sxs-lookup"><span data-stu-id="ba3b8-127">This image also has the following applications installed:</span></span>

* <span data-ttu-id="ba3b8-128">Adobe Flash Player</span><span class="sxs-lookup"><span data-stu-id="ba3b8-128">Adobe Flash Player</span></span>
* <span data-ttu-id="ba3b8-129">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="ba3b8-129">Microsoft Silverlight</span></span>
* <span data-ttu-id="ba3b8-130">Microsoft System Center 2012 Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="ba3b8-130">Microsoft System Center 2012 Endpoint Protection</span></span>
* <span data-ttu-id="ba3b8-131">Microsoft Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="ba3b8-131">Microsoft Windows Media Player</span></span>

## <a name="microsoft-office-365-proplus-subscription-required"></a><span data-ttu-id="ba3b8-132">Microsoft Office 365 ProPlus (požadován odběr)</span><span class="sxs-lookup"><span data-stu-id="ba3b8-132">Microsoft Office 365 ProPlus (subscription required)</span></span>
<span data-ttu-id="ba3b8-133">Office 365 je nejpožadovanější aplikace, takže jsme vytvořili „vlastní“ image, se kterým můžete pracovat.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-133">Office 365 is the most requested application, so we created a "custom" image for you to work with.</span></span>

<span data-ttu-id="ba3b8-134">Tento image je rozšířením image vanilla a kromě komponent popsaných v imagi Windows Server 2012 R2 má nainstalované následující komponenty aplikace Microsoft Office 365 ProPlus:</span><span class="sxs-lookup"><span data-stu-id="ba3b8-134">This image is an extension of the vanilla image and has the following components of Microsoft Office 365 ProPlus installed in addition to the components described in the Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="ba3b8-135">Access</span><span class="sxs-lookup"><span data-stu-id="ba3b8-135">Access</span></span>
* <span data-ttu-id="ba3b8-136">Excel</span><span class="sxs-lookup"><span data-stu-id="ba3b8-136">Excel</span></span>
* <span data-ttu-id="ba3b8-137">Lync</span><span class="sxs-lookup"><span data-stu-id="ba3b8-137">Lync</span></span>
* <span data-ttu-id="ba3b8-138">OneNote</span><span class="sxs-lookup"><span data-stu-id="ba3b8-138">OneNote</span></span>
* <span data-ttu-id="ba3b8-139">OneDrive pro firmy (použití synchronizačního agenta s Azure RemoteAppem se nepodporuje)</span><span class="sxs-lookup"><span data-stu-id="ba3b8-139">OneDrive for Business (note that the sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="ba3b8-140">Outlook</span><span class="sxs-lookup"><span data-stu-id="ba3b8-140">Outlook</span></span>
* <span data-ttu-id="ba3b8-141">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="ba3b8-141">PowerPoint</span></span>
* <span data-ttu-id="ba3b8-142">Word</span><span class="sxs-lookup"><span data-stu-id="ba3b8-142">Word</span></span>
* <span data-ttu-id="ba3b8-143">Nástroje kontroly pravopisu systému Microsoft Office</span><span class="sxs-lookup"><span data-stu-id="ba3b8-143">Microsoft Office Proofing Tools</span></span>

<span data-ttu-id="ba3b8-144">Image také zahrnuje Visio Pro a Project Pro.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-144">The image also includes Visio Pro and Project Pro.</span></span>

<span data-ttu-id="ba3b8-145">A také následující aplikace:</span><span class="sxs-lookup"><span data-stu-id="ba3b8-145">And the following applications, as well:</span></span>

* <span data-ttu-id="ba3b8-146">Nativní klient SQL</span><span class="sxs-lookup"><span data-stu-id="ba3b8-146">SQL Native client</span></span>
* <span data-ttu-id="ba3b8-147">Ovladač ODBC</span><span class="sxs-lookup"><span data-stu-id="ba3b8-147">ODBC Driver</span></span>
* <span data-ttu-id="ba3b8-148">Klient pro získávání dat SQL Server</span><span class="sxs-lookup"><span data-stu-id="ba3b8-148">SQL Server Data Mining client</span></span>
* <span data-ttu-id="ba3b8-149">Klient MasterDataServices</span><span class="sxs-lookup"><span data-stu-id="ba3b8-149">MasterDataServices client</span></span>
* <span data-ttu-id="ba3b8-150">Microsoft Publisher</span><span class="sxs-lookup"><span data-stu-id="ba3b8-150">Microsoft Publisher</span></span>
* <span data-ttu-id="ba3b8-151">PowerQuery</span><span class="sxs-lookup"><span data-stu-id="ba3b8-151">PowerQuery</span></span>
* <span data-ttu-id="ba3b8-152">PowerMap</span><span class="sxs-lookup"><span data-stu-id="ba3b8-152">PowerMap</span></span>

<span data-ttu-id="ba3b8-153">Plná funkčnost aplikací Office 365 ProPlus je k dispozici pouze pro uživatele, kteří mají plán Office 365 ProPlus.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-153">Full functionality of Office 365 ProPlus apps is available only for users who have an Office 365 ProPlus plan.</span></span> <span data-ttu-id="ba3b8-154">Další podrobnosti o plánu předplatného aplikace Office 365 najdete v části [Plány služeb Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-154">For more details on the Office 365 subscription plans see [Office 365 service plans](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span></span> <span data-ttu-id="ba3b8-155">Máte další otázky?</span><span class="sxs-lookup"><span data-stu-id="ba3b8-155">Still have questions?</span></span> <span data-ttu-id="ba3b8-156">Přečtěte si informace v části [Office 365 + RemoteApp](remoteapp-o365.md).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-156">Check out the [Office 365 + RemoteApp](remoteapp-o365.md) information.</span></span> <span data-ttu-id="ba3b8-157">Také si přečtěte nový článek [Použití předplatného služeb Office 365 s Azure RemoteAppem](remoteapp-officesubscription.md).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-157">Also check out the new article, [How to use your Office 365 subscription with Azure RemoteApp](remoteapp-officesubscription.md).</span></span>

<span data-ttu-id="ba3b8-158">Upozorňujeme, že k aplikacím Office 365 ProPlus, Visio Pro a Project Pro potřebujete samostatnou licenci (každá má svou vlastní).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-158">Note that you need to license Office 365 ProPlus, Visio Pro, and Project Pro separately - they each have their own license.</span></span>

## <a name="microsoft-office-2013-professional-plus-trial-only"></a><span data-ttu-id="ba3b8-159">Microsoft Office 2013 Professional Plus (pouze zkušební verze)</span><span class="sxs-lookup"><span data-stu-id="ba3b8-159">Microsoft Office 2013 Professional Plus (trial only)</span></span>
<span data-ttu-id="ba3b8-160">Během bezplatné zkušební doby můžete testovat službu s imagem Office 2013.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-160">During the free trial period, you can test the service with the Office 2013 image.</span></span>

<span data-ttu-id="ba3b8-161">Tento image je rozšířením image vanilla a kromě komponent popsaných v imagi Windows Server 2012 R2 má nainstalované následující komponenty aplikace Microsoft Office 2013 Professional Plus:</span><span class="sxs-lookup"><span data-stu-id="ba3b8-161">This image is an extension of the vanilla image and has the following components of Microsoft Office 2013 Professional Plus installed in addition to the components described in the Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="ba3b8-162">Access</span><span class="sxs-lookup"><span data-stu-id="ba3b8-162">Access</span></span>
* <span data-ttu-id="ba3b8-163">Excel</span><span class="sxs-lookup"><span data-stu-id="ba3b8-163">Excel</span></span>
* <span data-ttu-id="ba3b8-164">Lync</span><span class="sxs-lookup"><span data-stu-id="ba3b8-164">Lync</span></span>
* <span data-ttu-id="ba3b8-165">OneNote</span><span class="sxs-lookup"><span data-stu-id="ba3b8-165">OneNote</span></span>
* <span data-ttu-id="ba3b8-166">OneDrive pro firmy (použití synchronizačního agenta s Azure RemoteAppem se nepodporuje)</span><span class="sxs-lookup"><span data-stu-id="ba3b8-166">OneDrive for Business (note that the sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="ba3b8-167">Outlook</span><span class="sxs-lookup"><span data-stu-id="ba3b8-167">Outlook</span></span>
* <span data-ttu-id="ba3b8-168">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="ba3b8-168">PowerPoint</span></span>
* <span data-ttu-id="ba3b8-169">Project</span><span class="sxs-lookup"><span data-stu-id="ba3b8-169">Project</span></span>
* <span data-ttu-id="ba3b8-170">Visio</span><span class="sxs-lookup"><span data-stu-id="ba3b8-170">Visio</span></span>
* <span data-ttu-id="ba3b8-171">Word</span><span class="sxs-lookup"><span data-stu-id="ba3b8-171">Word</span></span>
* <span data-ttu-id="ba3b8-172">Nástroje kontroly pravopisu systému Microsoft Office</span><span class="sxs-lookup"><span data-stu-id="ba3b8-172">Microsoft Office Proofing Tools</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba3b8-173">**Právní informace:** Tento image neobsahuje licenci aplikace Microsoft Office a*nelze ho použít pro produkci*.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-173">**Legal information:** This image does not include a Microsoft Office license and *cannot be used for production*.</span></span> <span data-ttu-id="ba3b8-174">Image aplikace Professional Plus 2013 Office je určen pouze pro zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-174">The Office 2013 Professional Plus image is intended for trial use only.</span></span> <span data-ttu-id="ba3b8-175">Pokud chcete používat aplikace Office v Azure RemoteAppu pro produkci, je třeba použít image Office 365 ProPlus.</span><span class="sxs-lookup"><span data-stu-id="ba3b8-175">If you want to use Office apps in Azure RemoteApp for production, you need to use the Office 365 ProPlus image.</span></span> <span data-ttu-id="ba3b8-176">Další informace o licencování Officu najdete v části [Použití  Officu 365 s Azure RemoteAppem](remoteapp-o365.md).</span><span class="sxs-lookup"><span data-stu-id="ba3b8-176">For more details on licensing Office, see [Using Office 365 with Azure RemoteApp](remoteapp-o365.md)</span></span>
> 
> 

