---
title: "Řešení potíží s Azure RBAC | Microsoft Docs"
description: "Získáte pomoc s problémy nebo dotazy týkající se řízení přístupu na základě Role prostředky."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 407c030ea159915d4d7ac21760a3d17ec2204372
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="role-based-access-control-troubleshooting"></a><span data-ttu-id="cea8d-103">Na základě rolí řešení potíží s řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="cea8d-103">Role-Based Access Control troubleshooting</span></span>

<span data-ttu-id="cea8d-104">Tento dokument článku naleznete odpovědi na časté otázky týkající se konkrétní přístupová práva, kterým je uděleno oprávnění s rolemi, abyste věděli, co očekávat při použití role v portálu Azure a může poradce při potížích přístup.</span><span class="sxs-lookup"><span data-stu-id="cea8d-104">This document article answers common questions about the specific access rights that are granted with roles, so that you know what to expect when using the roles in the Azure portal and can troubleshoot access problems.</span></span> <span data-ttu-id="cea8d-105">Tyto tři role popisuje všechny typy prostředků:</span><span class="sxs-lookup"><span data-stu-id="cea8d-105">These three roles cover all resource types:</span></span>

* <span data-ttu-id="cea8d-106">Vlastník</span><span class="sxs-lookup"><span data-stu-id="cea8d-106">Owner</span></span>  
* <span data-ttu-id="cea8d-107">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="cea8d-107">Contributor</span></span>  
* <span data-ttu-id="cea8d-108">Čtenář</span><span class="sxs-lookup"><span data-stu-id="cea8d-108">Reader</span></span>  

<span data-ttu-id="cea8d-109">Vlastníci a přispěvatelé mají plný přístup k prostředí pro správu, ale Přispěvatel nelze poskytnout přístup k jiné uživatele nebo skupiny.</span><span class="sxs-lookup"><span data-stu-id="cea8d-109">Owners and contributors both have full access to the management experience, but a contributor can’t give access to other users or groups.</span></span> <span data-ttu-id="cea8d-110">Věcí získat něco zajímavějšího k roli čtečky tak, aby se, kde budete Věnujte nějaký čas.</span><span class="sxs-lookup"><span data-stu-id="cea8d-110">Things get a little more interesting with the reader role, so that’s where we'll spend some time.</span></span> <span data-ttu-id="cea8d-111">Najdete v článku [řízení přístupu na základě Role get-started článku](role-based-access-control-configure.md) podrobnosti o tom, jak udělit přístup.</span><span class="sxs-lookup"><span data-stu-id="cea8d-111">See the [Role-Based Access Control get-started article](role-based-access-control-configure.md) for details on how to grant access.</span></span>

## <a name="app-service-workloads"></a><span data-ttu-id="cea8d-112">Úlohy služby aplikace</span><span class="sxs-lookup"><span data-stu-id="cea8d-112">App service workloads</span></span>
### <a name="write-access-capabilities"></a><span data-ttu-id="cea8d-113">Možnosti přístup pro zápis</span><span class="sxs-lookup"><span data-stu-id="cea8d-113">Write access capabilities</span></span>
<span data-ttu-id="cea8d-114">Když udělíte přístup jen pro čtení uživatelů do jedné webové aplikace, některé funkce jsou vypnuté, nemusí očekáváte.</span><span class="sxs-lookup"><span data-stu-id="cea8d-114">If you grant a user read-only access to a single web app, some features are disabled that you might not expect.</span></span> <span data-ttu-id="cea8d-115">Následující funkce správy vyžadují **zápisu** přístup do webové aplikace (Přispěvatel nebo vlastníka) a nejsou k dispozici v žádném scénáři jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="cea8d-115">The following management capabilities require **write** access to a web app (either Contributor or Owner), and aren’t available in any read-only scenario.</span></span>

* <span data-ttu-id="cea8d-116">Příkazy (například spuštění, zastavení, atd.)</span><span class="sxs-lookup"><span data-stu-id="cea8d-116">Commands (like start, stop, etc.)</span></span>
* <span data-ttu-id="cea8d-117">Změna nastavení, jako je obecná konfigurace, nastavení škálování, nastavení zálohování a nastavení monitorování</span><span class="sxs-lookup"><span data-stu-id="cea8d-117">Changing settings like general configuration, scale settings, backup settings, and monitoring settings</span></span>
* <span data-ttu-id="cea8d-118">Přístup k pověření k publikování a jiné tajné klíče, jako je nastavení aplikace a připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="cea8d-118">Accessing publishing credentials and other secrets like app settings and connection strings</span></span>
* <span data-ttu-id="cea8d-119">Protokoly streamování</span><span class="sxs-lookup"><span data-stu-id="cea8d-119">Streaming logs</span></span>
* <span data-ttu-id="cea8d-120">Konfigurace diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="cea8d-120">Diagnostic logs configuration</span></span>
* <span data-ttu-id="cea8d-121">Konzole (příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="cea8d-121">Console (command prompt)</span></span>
* <span data-ttu-id="cea8d-122">Aktivní a poslední nasazení (pro místní git průběžné nasazování)</span><span class="sxs-lookup"><span data-stu-id="cea8d-122">Active and recent deployments (for local git continuous deployment)</span></span>
* <span data-ttu-id="cea8d-123">Odhadované tráví</span><span class="sxs-lookup"><span data-stu-id="cea8d-123">Estimated spend</span></span>
* <span data-ttu-id="cea8d-124">Testy webu</span><span class="sxs-lookup"><span data-stu-id="cea8d-124">Web tests</span></span>
* <span data-ttu-id="cea8d-125">Virtuální síť (viditelné pouze pro čtečku, pokud uživatel s přístup pro zápis byl dříve nakonfigurován virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="cea8d-125">Virtual network (only visible to a reader if a virtual network has previously been configured by a user with write access).</span></span>

<span data-ttu-id="cea8d-126">Pokud nemůžete použít žádnou z těchto dlaždicích, budete muset požádat správce pro přispěvatele přístup do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cea8d-126">If you can't access any of these tiles, you need to ask your administrator for Contributor access to the web app.</span></span>

### <a name="dealing-with-related-resources"></a><span data-ttu-id="cea8d-127">Práci s související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="cea8d-127">Dealing with related resources</span></span>
<span data-ttu-id="cea8d-128">Webové aplikace jsou ztěžuje přítomnost několik různých prostředků, které vztahu.</span><span class="sxs-lookup"><span data-stu-id="cea8d-128">Web apps are complicated by the presence of a few different resources that interplay.</span></span> <span data-ttu-id="cea8d-129">Tady je skupina prostředků typické se několik webů:</span><span class="sxs-lookup"><span data-stu-id="cea8d-129">Here is a typical resource group with a couple websites:</span></span>

![Skupina prostředků webové aplikace](./media/role-based-access-control-troubleshooting/website-resource-model.png)

<span data-ttu-id="cea8d-131">V důsledku toho, pokud někdo udělíte přístup do právě webové aplikace, mnoho funkcí v okně Web na portálu Azure je zakázán.</span><span class="sxs-lookup"><span data-stu-id="cea8d-131">As a result, if you grant someone access to just the web app, much of the functionality on the website blade in the Azure portal is disabled.</span></span>

<span data-ttu-id="cea8d-132">Tyto položky vyžadují **zápisu** přístup k **plán služby App Service** odpovídající na váš web:</span><span class="sxs-lookup"><span data-stu-id="cea8d-132">These items require **write** access to the **App Service plan** that corresponds to your website:</span></span>  

* <span data-ttu-id="cea8d-133">Zobrazení webové aplikace je cenová úroveň (volné nebo Standard)</span><span class="sxs-lookup"><span data-stu-id="cea8d-133">Viewing the web app’s pricing tier (Free or Standard)</span></span>  
* <span data-ttu-id="cea8d-134">Škálování konfigurace (počet instancí, velikost virtuálního počítače, nastavení automatického škálování)</span><span class="sxs-lookup"><span data-stu-id="cea8d-134">Scale configuration (number of instances, virtual machine size, autoscale settings)</span></span>  
* <span data-ttu-id="cea8d-135">Kvóty (úložiště, šířku pásma, procesoru)</span><span class="sxs-lookup"><span data-stu-id="cea8d-135">Quotas (storage, bandwidth, CPU)</span></span>  

<span data-ttu-id="cea8d-136">Tyto položky vyžadují **zápisu** přístup k celé **skupiny prostředků** svůj web, který obsahuje:</span><span class="sxs-lookup"><span data-stu-id="cea8d-136">These items require **write** access to the whole **Resource group** that contains your website:</span></span>  

* <span data-ttu-id="cea8d-137">Certifikáty SSL a vazeb (certifikáty SSL lze sdílet mezi lokalitami ve stejné skupině prostředků a geografického umístění)</span><span class="sxs-lookup"><span data-stu-id="cea8d-137">SSL Certificates and bindings (SSL certificates can be shared between sites in the same resource group and geo-location)</span></span>  
* <span data-ttu-id="cea8d-138">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="cea8d-138">Alert rules</span></span>  
* <span data-ttu-id="cea8d-139">nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="cea8d-139">Autoscale settings</span></span>  
* <span data-ttu-id="cea8d-140">Přehled součásti aplikace</span><span class="sxs-lookup"><span data-stu-id="cea8d-140">Application insights components</span></span>  
* <span data-ttu-id="cea8d-141">Testy webu</span><span class="sxs-lookup"><span data-stu-id="cea8d-141">Web tests</span></span>  

## <a name="virtual-machine-workloads"></a><span data-ttu-id="cea8d-142">Úlohy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="cea8d-142">Virtual machine workloads</span></span>
<span data-ttu-id="cea8d-143">Podobně jako s webovými aplikacemi, některé funkce v okně virtuálního počítače vyžadují přístup pro zápis k virtuálnímu počítači nebo k jiným prostředkům ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cea8d-143">Much like with web apps, some features on the virtual machine blade require write access to the virtual machine, or to other resources in the resource group.</span></span>

<span data-ttu-id="cea8d-144">Virtuální počítače jsou související s názvy domény, virtuálních sítí, účty úložiště a pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="cea8d-144">Virtual machines are related to Domain names, virtual networks, storage accounts, and alert rules.</span></span>

<span data-ttu-id="cea8d-145">Tyto položky vyžadují **zápisu** přístup k **virtuální počítač**:</span><span class="sxs-lookup"><span data-stu-id="cea8d-145">These items require **write** access to the **Virtual machine**:</span></span>

* <span data-ttu-id="cea8d-146">Koncové body</span><span class="sxs-lookup"><span data-stu-id="cea8d-146">Endpoints</span></span>  
* <span data-ttu-id="cea8d-147">IP adresy</span><span class="sxs-lookup"><span data-stu-id="cea8d-147">IP addresses</span></span>  
* <span data-ttu-id="cea8d-148">Disky</span><span class="sxs-lookup"><span data-stu-id="cea8d-148">Disks</span></span>  
* <span data-ttu-id="cea8d-149">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="cea8d-149">Extensions</span></span>  

<span data-ttu-id="cea8d-150">Vyžadují **zápisu** přístup do obou **virtuálního počítače**a **skupiny prostředků** (spolu s názvem domény) je to v:</span><span class="sxs-lookup"><span data-stu-id="cea8d-150">These require **write** access to both the **Virtual machine**, and the **Resource group** (along with the Domain name) that it is in:</span></span>  

* <span data-ttu-id="cea8d-151">Skupina dostupnosti</span><span class="sxs-lookup"><span data-stu-id="cea8d-151">Availability set</span></span>  
* <span data-ttu-id="cea8d-152">Skupině s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="cea8d-152">Load balanced set</span></span>  
* <span data-ttu-id="cea8d-153">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="cea8d-153">Alert rules</span></span>  

<span data-ttu-id="cea8d-154">Pokud nemůžete použít žádnou z těchto dlaždicích, požádejte správce pro přispěvatele přístup ke skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cea8d-154">If you can't access any of these tiles, ask your administrator for Contributor access to the Resource group.</span></span>

## <a name="see-more"></a><span data-ttu-id="cea8d-155">Přečíst si k tomu víc</span><span class="sxs-lookup"><span data-stu-id="cea8d-155">See more</span></span>
* <span data-ttu-id="cea8d-156">[Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cea8d-156">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in the Azure portal.</span></span>
* <span data-ttu-id="cea8d-157">[Předdefinované role](role-based-access-built-in-roles.md): získat tak podrobné údaje o rolích, které jsou v RBAC standardní.</span><span class="sxs-lookup"><span data-stu-id="cea8d-157">[Built-in roles](role-based-access-built-in-roles.md): Get details about the roles that come standard in RBAC.</span></span>
* <span data-ttu-id="cea8d-158">[Vlastní role v Azure RBAC](role-based-access-control-custom-roles.md): Naučte se vytvářet vlastní role podle vašich potřeb přístup.</span><span class="sxs-lookup"><span data-stu-id="cea8d-158">[Custom roles in Azure RBAC](role-based-access-control-custom-roles.md): Learn how to create custom roles to fit your access needs.</span></span>
* <span data-ttu-id="cea8d-159">[Vytvoření sestavy historie změn přístupu](role-based-access-control-access-change-history-report.md): udržování přehledu o změně přiřazení rolí v RBAC.</span><span class="sxs-lookup"><span data-stu-id="cea8d-159">[Create an access change history report](role-based-access-control-access-change-history-report.md): Keep track of changing role assignments in RBAC.</span></span>

