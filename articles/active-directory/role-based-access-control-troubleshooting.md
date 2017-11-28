---
title: aaaTroubleshoot Azure RBAC | Microsoft Docs
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
ms.openlocfilehash: 15feced32d8459d90c4c246d335932f90e1fc91f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-troubleshooting"></a><span data-ttu-id="6fe2c-103">Na základě rolí řešení potíží s řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="6fe2c-103">Role-Based Access Control troubleshooting</span></span>

<span data-ttu-id="6fe2c-104">Tento dokument článku naleznete odpovědi na časté otázky týkající se hello konkrétní přístupová práva, kterým je uděleno oprávnění s rolemi, tak, aby víte, jaké tooexpect při použití hello rolí v hello portál Azure a můžete řešit problémy přístup.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-104">This document article answers common questions about hello specific access rights that are granted with roles, so that you know what tooexpect when using hello roles in hello Azure portal and can troubleshoot access problems.</span></span> <span data-ttu-id="6fe2c-105">Tyto tři role popisuje všechny typy prostředků:</span><span class="sxs-lookup"><span data-stu-id="6fe2c-105">These three roles cover all resource types:</span></span>

* <span data-ttu-id="6fe2c-106">Vlastník</span><span class="sxs-lookup"><span data-stu-id="6fe2c-106">Owner</span></span>  
* <span data-ttu-id="6fe2c-107">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="6fe2c-107">Contributor</span></span>  
* <span data-ttu-id="6fe2c-108">Čtenář</span><span class="sxs-lookup"><span data-stu-id="6fe2c-108">Reader</span></span>  

<span data-ttu-id="6fe2c-109">Vlastníci a přispěvatelé mají plný přístup toohello správu prostředí, ale Přispěvatel nelze udělit přístup tooother uživatele nebo skupiny.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-109">Owners and contributors both have full access toohello management experience, but a contributor can’t give access tooother users or groups.</span></span> <span data-ttu-id="6fe2c-110">Věcí získat něco zajímavějšího s rolí čtečky hello, tak, aby se, kde budete Věnujte nějaký čas.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-110">Things get a little more interesting with hello reader role, so that’s where we'll spend some time.</span></span> <span data-ttu-id="6fe2c-111">V tématu hello [řízení přístupu na základě Role get-started článku](role-based-access-control-configure.md) podrobnosti o tom, jak toogrant přístup.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-111">See hello [Role-Based Access Control get-started article](role-based-access-control-configure.md) for details on how toogrant access.</span></span>

## <a name="app-service-workloads"></a><span data-ttu-id="6fe2c-112">Úlohy služby aplikace</span><span class="sxs-lookup"><span data-stu-id="6fe2c-112">App service workloads</span></span>
### <a name="write-access-capabilities"></a><span data-ttu-id="6fe2c-113">Možnosti přístup pro zápis</span><span class="sxs-lookup"><span data-stu-id="6fe2c-113">Write access capabilities</span></span>
<span data-ttu-id="6fe2c-114">Pokud byste udělit uživatelské oprávnění jen pro čtení tooa jedné webové aplikace, některé funkce jsou vypnuté, nemusí očekáváte.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-114">If you grant a user read-only access tooa single web app, some features are disabled that you might not expect.</span></span> <span data-ttu-id="6fe2c-115">Hello následující možnosti správy vyžadují **zápisu** přístup k webové aplikaci tooa (Přispěvatel nebo vlastníka) a nejsou k dispozici v žádném scénáři jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-115">hello following management capabilities require **write** access tooa web app (either Contributor or Owner), and aren’t available in any read-only scenario.</span></span>

* <span data-ttu-id="6fe2c-116">Příkazy (například spuštění, zastavení, atd.)</span><span class="sxs-lookup"><span data-stu-id="6fe2c-116">Commands (like start, stop, etc.)</span></span>
* <span data-ttu-id="6fe2c-117">Změna nastavení, jako je obecná konfigurace, nastavení škálování, nastavení zálohování a nastavení monitorování</span><span class="sxs-lookup"><span data-stu-id="6fe2c-117">Changing settings like general configuration, scale settings, backup settings, and monitoring settings</span></span>
* <span data-ttu-id="6fe2c-118">Přístup k pověření k publikování a jiné tajné klíče, jako je nastavení aplikace a připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="6fe2c-118">Accessing publishing credentials and other secrets like app settings and connection strings</span></span>
* <span data-ttu-id="6fe2c-119">Protokoly streamování</span><span class="sxs-lookup"><span data-stu-id="6fe2c-119">Streaming logs</span></span>
* <span data-ttu-id="6fe2c-120">Konfigurace diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="6fe2c-120">Diagnostic logs configuration</span></span>
* <span data-ttu-id="6fe2c-121">Konzole (příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="6fe2c-121">Console (command prompt)</span></span>
* <span data-ttu-id="6fe2c-122">Aktivní a poslední nasazení (pro místní git průběžné nasazování)</span><span class="sxs-lookup"><span data-stu-id="6fe2c-122">Active and recent deployments (for local git continuous deployment)</span></span>
* <span data-ttu-id="6fe2c-123">Odhadované tráví</span><span class="sxs-lookup"><span data-stu-id="6fe2c-123">Estimated spend</span></span>
* <span data-ttu-id="6fe2c-124">Testy webu</span><span class="sxs-lookup"><span data-stu-id="6fe2c-124">Web tests</span></span>
* <span data-ttu-id="6fe2c-125">Virtuální sítě (jenom viditelné tooa čtečky Pokud uživatel s přístup pro zápis byl dříve nakonfigurován virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="6fe2c-125">Virtual network (only visible tooa reader if a virtual network has previously been configured by a user with write access).</span></span>

<span data-ttu-id="6fe2c-126">Pokud nemůžete použít žádnou z těchto dlaždicích, je třeba tooask správce pro přispěvatele přístup toohello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-126">If you can't access any of these tiles, you need tooask your administrator for Contributor access toohello web app.</span></span>

### <a name="dealing-with-related-resources"></a><span data-ttu-id="6fe2c-127">Práci s související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="6fe2c-127">Dealing with related resources</span></span>
<span data-ttu-id="6fe2c-128">Webové aplikace jsou ztěžuje hello přítomnost několik různých prostředků, které vztahu.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-128">Web apps are complicated by hello presence of a few different resources that interplay.</span></span> <span data-ttu-id="6fe2c-129">Tady je skupina prostředků typické se několik webů:</span><span class="sxs-lookup"><span data-stu-id="6fe2c-129">Here is a typical resource group with a couple websites:</span></span>

![Skupina prostředků webové aplikace](./media/role-based-access-control-troubleshooting/website-resource-model.png)

<span data-ttu-id="6fe2c-131">Výsledkem je pokud někdo udělíte přístup toojust hello webové aplikace, většinu funkcí hello v okně hello webu v hello portál Azure je zakázané.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-131">As a result, if you grant someone access toojust hello web app, much of hello functionality on hello website blade in hello Azure portal is disabled.</span></span>

<span data-ttu-id="6fe2c-132">Tyto položky vyžadují **zápisu** přístup toohello **plán služby App Service** odpovídající tooyour webu:</span><span class="sxs-lookup"><span data-stu-id="6fe2c-132">These items require **write** access toohello **App Service plan** that corresponds tooyour website:</span></span>  

* <span data-ttu-id="6fe2c-133">Zobrazení hello webové aplikace je cenová úroveň (volné nebo Standard)</span><span class="sxs-lookup"><span data-stu-id="6fe2c-133">Viewing hello web app’s pricing tier (Free or Standard)</span></span>  
* <span data-ttu-id="6fe2c-134">Škálování konfigurace (počet instancí, velikost virtuálního počítače, nastavení automatického škálování)</span><span class="sxs-lookup"><span data-stu-id="6fe2c-134">Scale configuration (number of instances, virtual machine size, autoscale settings)</span></span>  
* <span data-ttu-id="6fe2c-135">Kvóty (úložiště, šířku pásma, procesoru)</span><span class="sxs-lookup"><span data-stu-id="6fe2c-135">Quotas (storage, bandwidth, CPU)</span></span>  

<span data-ttu-id="6fe2c-136">Tyto položky vyžadují **zápisu** přístup toohello celou **skupiny prostředků** svůj web, který obsahuje:</span><span class="sxs-lookup"><span data-stu-id="6fe2c-136">These items require **write** access toohello whole **Resource group** that contains your website:</span></span>  

* <span data-ttu-id="6fe2c-137">Certifikáty SSL a vazeb (certifikáty protokolu SSL může být sdílena mezi lokalitami v hello stejnou skupinu prostředků a geografického umístění)</span><span class="sxs-lookup"><span data-stu-id="6fe2c-137">SSL Certificates and bindings (SSL certificates can be shared between sites in hello same resource group and geo-location)</span></span>  
* <span data-ttu-id="6fe2c-138">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="6fe2c-138">Alert rules</span></span>  
* <span data-ttu-id="6fe2c-139">nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="6fe2c-139">Autoscale settings</span></span>  
* <span data-ttu-id="6fe2c-140">Přehled součásti aplikace</span><span class="sxs-lookup"><span data-stu-id="6fe2c-140">Application insights components</span></span>  
* <span data-ttu-id="6fe2c-141">Testy webu</span><span class="sxs-lookup"><span data-stu-id="6fe2c-141">Web tests</span></span>  

## <a name="virtual-machine-workloads"></a><span data-ttu-id="6fe2c-142">Úlohy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6fe2c-142">Virtual machine workloads</span></span>
<span data-ttu-id="6fe2c-143">Mnohem jako s webovými aplikacemi, některé funkce v okně hello virtuálního počítače vyžadují přístup pro zápis toohello virtuálního počítače nebo tooother prostředky ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-143">Much like with web apps, some features on hello virtual machine blade require write access toohello virtual machine, or tooother resources in hello resource group.</span></span>

<span data-ttu-id="6fe2c-144">Virtuální počítače jsou názvy související tooDomain, virtuálních sítí, účty úložiště a pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-144">Virtual machines are related tooDomain names, virtual networks, storage accounts, and alert rules.</span></span>

<span data-ttu-id="6fe2c-145">Tyto položky vyžadují **zápisu** přístup toohello **virtuální počítač**:</span><span class="sxs-lookup"><span data-stu-id="6fe2c-145">These items require **write** access toohello **Virtual machine**:</span></span>

* <span data-ttu-id="6fe2c-146">Koncové body</span><span class="sxs-lookup"><span data-stu-id="6fe2c-146">Endpoints</span></span>  
* <span data-ttu-id="6fe2c-147">IP adresy</span><span class="sxs-lookup"><span data-stu-id="6fe2c-147">IP addresses</span></span>  
* <span data-ttu-id="6fe2c-148">Disky</span><span class="sxs-lookup"><span data-stu-id="6fe2c-148">Disks</span></span>  
* <span data-ttu-id="6fe2c-149">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="6fe2c-149">Extensions</span></span>  

<span data-ttu-id="6fe2c-150">Vyžadují **zápisu** přístup tooboth hello **virtuálního počítače**a hello **skupiny prostředků** (spolu s názvem domény hello) je to v:</span><span class="sxs-lookup"><span data-stu-id="6fe2c-150">These require **write** access tooboth hello **Virtual machine**, and hello **Resource group** (along with hello Domain name) that it is in:</span></span>  

* <span data-ttu-id="6fe2c-151">Skupina dostupnosti</span><span class="sxs-lookup"><span data-stu-id="6fe2c-151">Availability set</span></span>  
* <span data-ttu-id="6fe2c-152">Skupině s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="6fe2c-152">Load balanced set</span></span>  
* <span data-ttu-id="6fe2c-153">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="6fe2c-153">Alert rules</span></span>  

<span data-ttu-id="6fe2c-154">Pokud nemůžete použít žádnou z těchto dlaždicích, požádejte správce pro skupinu prostředků toohello Přispěvatel přístup.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-154">If you can't access any of these tiles, ask your administrator for Contributor access toohello Resource group.</span></span>

## <a name="see-more"></a><span data-ttu-id="6fe2c-155">Přečíst si k tomu víc</span><span class="sxs-lookup"><span data-stu-id="6fe2c-155">See more</span></span>
* <span data-ttu-id="6fe2c-156">[Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-156">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in hello Azure portal.</span></span>
* <span data-ttu-id="6fe2c-157">[Předdefinované role](role-based-access-built-in-roles.md): získání podrobností o hello rolí, které jsou v RBAC standardní.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-157">[Built-in roles](role-based-access-built-in-roles.md): Get details about hello roles that come standard in RBAC.</span></span>
* <span data-ttu-id="6fe2c-158">[Vlastní role v Azure RBAC](role-based-access-control-custom-roles.md): Zjistěte, jak se toocreate vlastní role toofit přístup musí.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-158">[Custom roles in Azure RBAC](role-based-access-control-custom-roles.md): Learn how toocreate custom roles toofit your access needs.</span></span>
* <span data-ttu-id="6fe2c-159">[Vytvoření sestavy historie změn přístupu](role-based-access-control-access-change-history-report.md): udržování přehledu o změně přiřazení rolí v RBAC.</span><span class="sxs-lookup"><span data-stu-id="6fe2c-159">[Create an access change history report](role-based-access-control-access-change-history-report.md): Keep track of changing role assignments in RBAC.</span></span>

