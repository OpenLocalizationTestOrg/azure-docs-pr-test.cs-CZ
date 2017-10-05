---
title: "Přesunutí nové předplatné nebo prostředek skupiny prostředků Azure | Microsoft Docs"
description: "Azure Resource Manager využívat k přesunu prostředků do nové skupiny prostředků nebo předplatného."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: tomfitz
ms.openlocfilehash: e138f80e808968ab4bf5c11cfd5fd46fe4a1bcce
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a><span data-ttu-id="945fc-103">Přesunutím prostředků do nové skupiny prostředků nebo předplatného</span><span class="sxs-lookup"><span data-stu-id="945fc-103">Move resources to new resource group or subscription</span></span>
<span data-ttu-id="945fc-104">Toto téma ukazuje, jak k přesunu prostředků do nového předplatného nebo novou skupinu prostředků ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="945fc-104">This topic shows you how to move resources to either a new subscription or a new resource group in the same subscription.</span></span> <span data-ttu-id="945fc-105">Můžete na portálu, prostředí PowerShell, rozhraní příkazového řádku Azure nebo rozhraní REST API pro přesun prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-105">You can use the portal, PowerShell, Azure CLI, or the REST API to move resource.</span></span> <span data-ttu-id="945fc-106">Operace přesunutí v tomto tématu jsou vám k dispozici bez jakékoli pomoci z podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="945fc-106">The move operations in this topic are available to you without any assistance from Azure support.</span></span>

<span data-ttu-id="945fc-107">Při přesouvání prostředků, skupině zdrojové a cílové skupiny jsou zamčené během operace.</span><span class="sxs-lookup"><span data-stu-id="945fc-107">When moving resources, both the source group and the target group are locked during the operation.</span></span> <span data-ttu-id="945fc-108">Zápis a operace odstranění jsou zablokované na skupiny prostředků, až po dokončení přesunu.</span><span class="sxs-lookup"><span data-stu-id="945fc-108">Write and delete operations are blocked on the resource groups until the move completes.</span></span> <span data-ttu-id="945fc-109">Tato Zámek znamená nelze přidat, aktualizovat nebo odstranit prostředky ve skupině prostředků, ale neznamená, že zmrazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="945fc-109">This lock means you cannot add, update, or delete resources in the resource groups, but it does not mean the resources are frozen.</span></span> <span data-ttu-id="945fc-110">Pokud přesunete SQL Server a jeho databáze do nové skupiny prostředků, například aplikace, která používá databázi vyskytne nedojde k žádnému výpadku.</span><span class="sxs-lookup"><span data-stu-id="945fc-110">For example, if you move a SQL Server and its database to a new resource group, an application that uses the database experiences no downtime.</span></span> <span data-ttu-id="945fc-111">Můžete dál číst a zapisovat do databáze.</span><span class="sxs-lookup"><span data-stu-id="945fc-111">It can still read and write to the database.</span></span>

<span data-ttu-id="945fc-112">Nelze změnit umístění prostředku.</span><span class="sxs-lookup"><span data-stu-id="945fc-112">You cannot change the location of the resource.</span></span> <span data-ttu-id="945fc-113">Přesunutí prostředku přesouvá pouze ji do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-113">Moving a resource only moves it to a new resource group.</span></span> <span data-ttu-id="945fc-114">Nové skupiny prostředků může mít jiné umístění, ale které nelze změnit umístění prostředku.</span><span class="sxs-lookup"><span data-stu-id="945fc-114">The new resource group may have a different location, but that does not change the location of the resource.</span></span>

> [!NOTE]
> <span data-ttu-id="945fc-115">Tento článek popisuje postup přesunutí prostředků v Azure existující účet nabízení.</span><span class="sxs-lookup"><span data-stu-id="945fc-115">This article describes how to move resources within an existing Azure account offering.</span></span> <span data-ttu-id="945fc-116">Pokud skutečně chcete změnit účtu Azure nabídky (například upgrade z průběžné platby předem věnovat) přitom dál pracovat se stávajícími prostředky, najdete v článku [vašeho předplatného Azure přepnout na jinou nabídku](../billing/billing-how-to-switch-azure-offer.md).</span><span class="sxs-lookup"><span data-stu-id="945fc-116">If you actually want to change your Azure account offering (such as upgrading from pay-as-you-go to pre-pay) while continuing to work with your existing resources, see [Switch your Azure subscription to another offer](../billing/billing-how-to-switch-azure-offer.md).</span></span>
>
>

## <a name="checklist-before-moving-resources"></a><span data-ttu-id="945fc-117">Kontrolní seznam před přesunutím prostředků</span><span class="sxs-lookup"><span data-stu-id="945fc-117">Checklist before moving resources</span></span>
<span data-ttu-id="945fc-118">Před přesunutím prostředku je nutné provést několik důležitých kroků.</span><span class="sxs-lookup"><span data-stu-id="945fc-118">There are some important steps to perform before moving a resource.</span></span> <span data-ttu-id="945fc-119">Ověřením těchto podmínek se můžete vyhnout chybám.</span><span class="sxs-lookup"><span data-stu-id="945fc-119">By verifying these conditions, you can avoid errors.</span></span>

1. <span data-ttu-id="945fc-120">Zdrojové a cílové odběry, musí existovat v rámci stejné [klienta Azure Active Directory](../active-directory/active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="945fc-120">The source and destination subscriptions must exist within the same [Azure Active Directory tenant](../active-directory/active-directory-howto-tenant.md).</span></span> <span data-ttu-id="945fc-121">Chcete-li zkontrolovat, že oba odběry obsahují stejné ID klienta, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="945fc-121">To check that both subscriptions have the same tenant ID, use Azure PowerShell or Azure CLI.</span></span>

  <span data-ttu-id="945fc-122">Pro Azure PowerShell použijte:</span><span class="sxs-lookup"><span data-stu-id="945fc-122">For Azure PowerShell, use:</span></span>

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  <span data-ttu-id="945fc-123">Azure CLI 2.0 použijte:</span><span class="sxs-lookup"><span data-stu-id="945fc-123">For Azure CLI 2.0, use:</span></span>

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  <span data-ttu-id="945fc-124">Pokud klient ID pro zdrojové a cílové předplatné nejsou stejné, pokuste se změnit adresář pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="945fc-124">If the tenant IDs for the source and destination subscriptions are not the same, you can attempt to change the directory for the subscription.</span></span> <span data-ttu-id="945fc-125">Však tato možnost je dostupná jenom pro správce služby, který je přihlášený pomocí účtu Microsoft (není účet organizace).</span><span class="sxs-lookup"><span data-stu-id="945fc-125">However, this option is only available to Service Administrators who are signed in with a Microsoft account (not an organizational account).</span></span> <span data-ttu-id="945fc-126">Aby se změna adresáři, přihlaste se k [portálu classic](https://manage.windowsazure.com/)a vyberte **nastavení**a vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="945fc-126">To attempt changing the directory, log in to the [classic portal](https://manage.windowsazure.com/), and select **Settings**, and select the subscription.</span></span> <span data-ttu-id="945fc-127">Pokud **upravit adresář** ikonu je k dispozici, vyberte ji chcete změnit přidružené Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="945fc-127">If the **Edit Directory** icon is available, select it to change the associated Azure Active Directory.</span></span>

  ![Upravit adresář](./media/resource-group-move-resources/edit-directory.png)

  <span data-ttu-id="945fc-129">Pokud tuto ikonu není k dispozici, obraťte se na podporu pro přesun prostředků do nového klienta.</span><span class="sxs-lookup"><span data-stu-id="945fc-129">If that icon is not available, you must contact support to move the resources to a new tenant.</span></span>

2. <span data-ttu-id="945fc-130">Služba musí umožňovat operaci přesouvání prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-130">The service must enable the ability to move resources.</span></span> <span data-ttu-id="945fc-131">Toto téma uvádí služby, které Povolit přesunutí prostředků a služby, které nepovolíte přesunutí prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-131">This topic lists which services enable moving resources and which services do not enable moving resources.</span></span>
3. <span data-ttu-id="945fc-132">Cílové předplatné musí být registrováno pro poskytovatele přesouvaného prostředku.</span><span class="sxs-lookup"><span data-stu-id="945fc-132">The destination subscription must be registered for the resource provider of the resource being moved.</span></span> <span data-ttu-id="945fc-133">Pokud ne, se zobrazí chybová zpráva s informacemi, které **předplatné není zaregistrované pro typ prostředku**.</span><span class="sxs-lookup"><span data-stu-id="945fc-133">If not, you receive an error stating that the **subscription is not registered for a resource type**.</span></span> <span data-ttu-id="945fc-134">K problému může dojít, pokud přesouváte prostředek do nového předplatného, ale toto předplatné nebylo pro příslušný typ prostředku nikdy použito.</span><span class="sxs-lookup"><span data-stu-id="945fc-134">You might encounter this problem when moving a resource to a new subscription, but that subscription has never been used with that resource type.</span></span> <span data-ttu-id="945fc-135">Další informace o stavu registrace a registraci poskytovatelů prostředků najdete v tématu [Typy prostředků a jejich poskytovatelé](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="945fc-135">To learn how to check the registration status and register resource providers, see [Resource providers and types](resource-manager-supported-services.md).</span></span>

## <a name="when-to-call-support"></a><span data-ttu-id="945fc-136">Při volání podpory</span><span class="sxs-lookup"><span data-stu-id="945fc-136">When to call support</span></span>
<span data-ttu-id="945fc-137">Většina prostředkům prostřednictvím operace samoobslužné služby zobrazí v tomto tématu se můžete přesunout.</span><span class="sxs-lookup"><span data-stu-id="945fc-137">You can move most resources through the self-service operations shown in this topic.</span></span> <span data-ttu-id="945fc-138">Pomocí operace samoobslužné služby, které se:</span><span class="sxs-lookup"><span data-stu-id="945fc-138">Use the self-service operations to:</span></span>

* <span data-ttu-id="945fc-139">Přesouvání prostředků Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="945fc-139">Move Resource Manager resources.</span></span>
* <span data-ttu-id="945fc-140">Přesunout klasické prostředky podle požadavků [omezení nasazení classic](#classic-deployment-limitations).</span><span class="sxs-lookup"><span data-stu-id="945fc-140">Move classic resources according to the [classic deployment limitations](#classic-deployment-limitations).</span></span>

<span data-ttu-id="945fc-141">Až budete potřebovat, obraťte se na podporu:</span><span class="sxs-lookup"><span data-stu-id="945fc-141">Call support when you need to:</span></span>

* <span data-ttu-id="945fc-142">Přesuňte vašich prostředků na nový účet Azure (a klienta Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="945fc-142">Move your resources to a new Azure account (and Azure Active Directory tenant).</span></span>
* <span data-ttu-id="945fc-143">Přesunout klasické prostředky ale dochází k potížím s omezeními.</span><span class="sxs-lookup"><span data-stu-id="945fc-143">Move classic resources but are having trouble with the limitations.</span></span>

## <a name="services-that-enable-move"></a><span data-ttu-id="945fc-144">Služby, které umožňují přesunout</span><span class="sxs-lookup"><span data-stu-id="945fc-144">Services that enable move</span></span>
<span data-ttu-id="945fc-145">Prozatím se služeb, které Přesun na novou skupinu prostředků a předplatného jsou:</span><span class="sxs-lookup"><span data-stu-id="945fc-145">For now, the services that enable moving to both a new resource group and subscription are:</span></span>

* <span data-ttu-id="945fc-146">API Management</span><span class="sxs-lookup"><span data-stu-id="945fc-146">API Management</span></span>
* <span data-ttu-id="945fc-147">Aplikace služby App Service (webové aplikace) – viz [omezení služby App Service](#app-service-limitations)</span><span class="sxs-lookup"><span data-stu-id="945fc-147">App Service apps (web apps) - see [App Service limitations](#app-service-limitations)</span></span>
* <span data-ttu-id="945fc-148">Application Insights</span><span class="sxs-lookup"><span data-stu-id="945fc-148">Application Insights</span></span>
* <span data-ttu-id="945fc-149">Automation</span><span class="sxs-lookup"><span data-stu-id="945fc-149">Automation</span></span>
* <span data-ttu-id="945fc-150">Batch</span><span class="sxs-lookup"><span data-stu-id="945fc-150">Batch</span></span>
* <span data-ttu-id="945fc-151">Mapy Bing</span><span class="sxs-lookup"><span data-stu-id="945fc-151">Bing Maps</span></span>
* <span data-ttu-id="945fc-152">CDN</span><span class="sxs-lookup"><span data-stu-id="945fc-152">CDN</span></span>
* <span data-ttu-id="945fc-153">Cloudové služby - viz [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="945fc-153">Cloud Services - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="945fc-154">Kognitivní služby</span><span class="sxs-lookup"><span data-stu-id="945fc-154">Cognitive Services</span></span>
* <span data-ttu-id="945fc-155">Content Moderator</span><span class="sxs-lookup"><span data-stu-id="945fc-155">Content Moderator</span></span>
* <span data-ttu-id="945fc-156">Data Catalog</span><span class="sxs-lookup"><span data-stu-id="945fc-156">Data Catalog</span></span>
* <span data-ttu-id="945fc-157">Data Factory</span><span class="sxs-lookup"><span data-stu-id="945fc-157">Data Factory</span></span>
* <span data-ttu-id="945fc-158">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="945fc-158">Data Lake Analytics</span></span>
* <span data-ttu-id="945fc-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="945fc-159">Data Lake Store</span></span>
* <span data-ttu-id="945fc-160">DNS</span><span class="sxs-lookup"><span data-stu-id="945fc-160">DNS</span></span>
* <span data-ttu-id="945fc-161">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="945fc-161">Azure Cosmos DB</span></span>
* <span data-ttu-id="945fc-162">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="945fc-162">Event Hubs</span></span>
* <span data-ttu-id="945fc-163">Clustery HDInsight - najdete v části [omezení HDInsight](#hdinsight-limitations)</span><span class="sxs-lookup"><span data-stu-id="945fc-163">HDInsight clusters - see [HDInsight limitations](#hdinsight-limitations)</span></span>
* <span data-ttu-id="945fc-164">Centra IoT</span><span class="sxs-lookup"><span data-stu-id="945fc-164">IoT Hubs</span></span>
* <span data-ttu-id="945fc-165">Key Vault</span><span class="sxs-lookup"><span data-stu-id="945fc-165">Key Vault</span></span>
* <span data-ttu-id="945fc-166">Nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="945fc-166">Load Balancers</span></span>
* <span data-ttu-id="945fc-167">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="945fc-167">Logic Apps</span></span>
* <span data-ttu-id="945fc-168">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="945fc-168">Machine Learning</span></span>
* <span data-ttu-id="945fc-169">Media Services</span><span class="sxs-lookup"><span data-stu-id="945fc-169">Media Services</span></span>
* <span data-ttu-id="945fc-170">Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="945fc-170">Mobile Engagement</span></span>
* <span data-ttu-id="945fc-171">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="945fc-171">Notification Hubs</span></span>
* <span data-ttu-id="945fc-172">Operational Insights</span><span class="sxs-lookup"><span data-stu-id="945fc-172">Operational Insights</span></span>
* <span data-ttu-id="945fc-173">Správa operací</span><span class="sxs-lookup"><span data-stu-id="945fc-173">Operations Management</span></span>
* <span data-ttu-id="945fc-174">Power BI</span><span class="sxs-lookup"><span data-stu-id="945fc-174">Power BI</span></span>
* <span data-ttu-id="945fc-175">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="945fc-175">Redis Cache</span></span>
* <span data-ttu-id="945fc-176">Scheduler</span><span class="sxs-lookup"><span data-stu-id="945fc-176">Scheduler</span></span>
* <span data-ttu-id="945fc-177">Search</span><span class="sxs-lookup"><span data-stu-id="945fc-177">Search</span></span>
* <span data-ttu-id="945fc-178">Správa serveru</span><span class="sxs-lookup"><span data-stu-id="945fc-178">Server Management</span></span>
* <span data-ttu-id="945fc-179">Service Bus</span><span class="sxs-lookup"><span data-stu-id="945fc-179">Service Bus</span></span>
* <span data-ttu-id="945fc-180">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="945fc-180">Service Fabric</span></span>
* <span data-ttu-id="945fc-181">Úložiště</span><span class="sxs-lookup"><span data-stu-id="945fc-181">Storage</span></span>
* <span data-ttu-id="945fc-182">Najdete v části úložiště (klasické) - [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="945fc-182">Storage (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="945fc-183">Stream Analytics - Stream Analytics úlohy nelze přesunout, při spuštění ve stavu.</span><span class="sxs-lookup"><span data-stu-id="945fc-183">Stream Analytics - Stream Analytics jobs cannot be moved when in running state.</span></span>
* <span data-ttu-id="945fc-184">Databáze SQL server – databáze a server se musí nacházet ve stejné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-184">SQL Database server - The database and server must reside in the same resource group.</span></span> <span data-ttu-id="945fc-185">Když přesouváte systému SQL server, budou přesunuty také všechny její databáze.</span><span class="sxs-lookup"><span data-stu-id="945fc-185">When you move a SQL server, all its databases are also moved.</span></span>
* <span data-ttu-id="945fc-186">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="945fc-186">Traffic Manager</span></span>
* <span data-ttu-id="945fc-187">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="945fc-187">Virtual Machines</span></span>
* <span data-ttu-id="945fc-188">Virtuální počítače s certifikátem uložené v Key Vault – přesunout do nového prostředku skupiny v rámci stejného předplatného je povolena, ale přesun křížové předplatného není povolená.</span><span class="sxs-lookup"><span data-stu-id="945fc-188">Virtual Machines with certificate stored in Key Vault - Move to new resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="945fc-189">Virtuální počítače (klasické) - najdete v části [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="945fc-189">Virtual Machines (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="945fc-190">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="945fc-190">Virtual Machine Scale Sets</span></span>
* <span data-ttu-id="945fc-191">Virtuální sítě - aktuálně, peered virtuální sítě nelze přesunout, dokud partnerský vztah virtuální sítě je zakázané.</span><span class="sxs-lookup"><span data-stu-id="945fc-191">Virtual Networks - Currently, a peered Virtual Network cannot be moved until VNet peering has been disabled.</span></span> <span data-ttu-id="945fc-192">Jakmile zakázaná, virtuální sítě možné úspěšně přesunout a lze je povolit partnerského vztahu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="945fc-192">Once disabled, the Virtual Network can be moved successfully and the VNet peering can be enabled.</span></span> <span data-ttu-id="945fc-193">Kromě toho virtuální sítě nelze přesunout do jiného předplatného, pokud virtuální síť obsahuje všechny podsítě s odkazy na zdroje navigace.</span><span class="sxs-lookup"><span data-stu-id="945fc-193">In addition, a Virtual Network cannot be moved to a different subscription if the Virtual Network contains any subnet with resource navigation links.</span></span> <span data-ttu-id="945fc-194">Například podsíť virtuální sítě nemá navigační odkaz na prostředek, pokud prostředek redis Microsoft.Cache je nasazený do dané podsítě.</span><span class="sxs-lookup"><span data-stu-id="945fc-194">For example, a Virtual Network subnet has a resource navigation link when a Microsoft.Cache redis resource is deployed into this subnet.</span></span>
* <span data-ttu-id="945fc-195">VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="945fc-195">VPN Gateway</span></span>


## <a name="services-that-do-not-enable-move"></a><span data-ttu-id="945fc-196">Služby, které nepovolujte přesunutí</span><span class="sxs-lookup"><span data-stu-id="945fc-196">Services that do not enable move</span></span>
<span data-ttu-id="945fc-197">Služby, které aktuálně nepovolujte přesunutí prostředku jsou:</span><span class="sxs-lookup"><span data-stu-id="945fc-197">The services that currently do not enable moving a resource are:</span></span>

* <span data-ttu-id="945fc-198">Služba AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="945fc-198">AD Domain Services</span></span>
* <span data-ttu-id="945fc-199">Hybridní AD Health Service</span><span class="sxs-lookup"><span data-stu-id="945fc-199">AD Hybrid Health Service</span></span>
* <span data-ttu-id="945fc-200">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="945fc-200">Application Gateway</span></span>
* <span data-ttu-id="945fc-201">S virtuálními počítači s disky spravovat sady dostupnosti</span><span class="sxs-lookup"><span data-stu-id="945fc-201">Availability sets with Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="945fc-202">BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="945fc-202">BizTalk Services</span></span>
* <span data-ttu-id="945fc-203">Container Service</span><span class="sxs-lookup"><span data-stu-id="945fc-203">Container Service</span></span>
* <span data-ttu-id="945fc-204">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="945fc-204">Express Route</span></span>
* <span data-ttu-id="945fc-205">DevTest Labs – přesunout do nové skupiny prostředků v rámci stejného předplatného je povoleno, ale přesunutí křížové předplatného není povolená.</span><span class="sxs-lookup"><span data-stu-id="945fc-205">DevTest Labs - Move to new resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="945fc-206">Dynamics LCS</span><span class="sxs-lookup"><span data-stu-id="945fc-206">Dynamics LCS</span></span>
* <span data-ttu-id="945fc-207">Bitové kopie vytvořené z spravované disků</span><span class="sxs-lookup"><span data-stu-id="945fc-207">Images created from Managed Disks</span></span>
* <span data-ttu-id="945fc-208">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="945fc-208">Managed Disks</span></span>
* <span data-ttu-id="945fc-209">Spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="945fc-209">Managed Applications</span></span>
* <span data-ttu-id="945fc-210">Trezor služeb zotavení – také proveďte není přesunout prostředky výpočty, síť a úložiště přidružený k trezoru služeb zotavení, najdete v části [služeb zotavení omezení](#recovery-services-limitations).</span><span class="sxs-lookup"><span data-stu-id="945fc-210">Recovery Services vault - also do not move the Compute, Network, and Storage resources associated with the Recovery Services vault, see [Recovery Services limitations](#recovery-services-limitations).</span></span>
* <span data-ttu-id="945fc-211">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="945fc-211">Security</span></span>
* <span data-ttu-id="945fc-212">Snímky vytvořené z spravované disky</span><span class="sxs-lookup"><span data-stu-id="945fc-212">Snapshots created from Managed Disks</span></span>
* <span data-ttu-id="945fc-213">Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="945fc-213">StorSimple Device Manager</span></span>
* <span data-ttu-id="945fc-214">Virtuální počítače s spravované disky</span><span class="sxs-lookup"><span data-stu-id="945fc-214">Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="945fc-215">Najdete v části virtuální sítě (klasické) - [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="945fc-215">Virtual Networks (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="945fc-216">Virtuální počítače vytvořené z Marketplace prostředků – nelze přesunout ve předplatných.</span><span class="sxs-lookup"><span data-stu-id="945fc-216">Virtual Machines created from Marketplace resources - cannot be moved across subscriptions.</span></span> <span data-ttu-id="945fc-217">Prostředků musí být v aktuálním předplatném zrušit a znovu nasadit v rámci nového předplatného</span><span class="sxs-lookup"><span data-stu-id="945fc-217">Resource needs to be deprovisioned in the current subscription and deployed again in the new subscription</span></span>

## <a name="app-service-limitations"></a><span data-ttu-id="945fc-218">Omezení služby App Service</span><span class="sxs-lookup"><span data-stu-id="945fc-218">App Service limitations</span></span>
<span data-ttu-id="945fc-219">Při práci s aplikacemi App Service, nemůžete přesunout pouze plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="945fc-219">When working with App Service apps, you cannot move only an App Service plan.</span></span> <span data-ttu-id="945fc-220">Chcete-li přesunout aplikací App Service, možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="945fc-220">To move App Service apps, your options are:</span></span>

* <span data-ttu-id="945fc-221">Přesuňte plán služby App Service a všechny ostatní prostředky služby App Service v příslušné skupině prostředků do nové skupiny prostředků, který ještě nemá prostředky služby App Service.</span><span class="sxs-lookup"><span data-stu-id="945fc-221">Move the App Service plan and all other App Service resources in that resource group to a new resource group that does not already have App Service resources.</span></span> <span data-ttu-id="945fc-222">Tento požadavek znamená, že je nutné přesunout i prostředky služby App Service, které nejsou přidružené plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="945fc-222">This requirement means you must move even the App Service resources that are not associated with the App Service plan.</span></span>
* <span data-ttu-id="945fc-223">Přesunutí aplikace do jiné skupině prostředků, ale ponechat všechny plány služby App Service v původní skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-223">Move the apps to a different resource group, but keep all App Service plans in the original resource group.</span></span>

<span data-ttu-id="945fc-224">Plán služby App Service se nemusí být umístěné ve stejné skupině prostředků jako aplikace pro aplikaci správné fungování.</span><span class="sxs-lookup"><span data-stu-id="945fc-224">The App Service plan does not need to reside in the same resource group as the app for the app to function correctly.</span></span>

<span data-ttu-id="945fc-225">Například, pokud obsahuje vaší skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="945fc-225">For example, if your resource group contains:</span></span>

* <span data-ttu-id="945fc-226">**webové a** který je přidružen **plánu a**</span><span class="sxs-lookup"><span data-stu-id="945fc-226">**web-a** which is associated with **plan-a**</span></span>
* <span data-ttu-id="945fc-227">**Web-b** který je přidružen **plán b**</span><span class="sxs-lookup"><span data-stu-id="945fc-227">**web-b** which is associated with **plan-b**</span></span>

<span data-ttu-id="945fc-228">Možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="945fc-228">Your options are:</span></span>

* <span data-ttu-id="945fc-229">Přesunout **webové a**, **plán a**, **web-b**, a **plán b**</span><span class="sxs-lookup"><span data-stu-id="945fc-229">Move **web-a**, **plan-a**, **web-b**, and **plan-b**</span></span>
* <span data-ttu-id="945fc-230">Přesunout **webové a** a **web-b**</span><span class="sxs-lookup"><span data-stu-id="945fc-230">Move **web-a** and **web-b**</span></span>
* <span data-ttu-id="945fc-231">Přesunout **webové a**</span><span class="sxs-lookup"><span data-stu-id="945fc-231">Move **web-a**</span></span>
* <span data-ttu-id="945fc-232">Přesunout **web-b**</span><span class="sxs-lookup"><span data-stu-id="945fc-232">Move **web-b**</span></span>

<span data-ttu-id="945fc-233">Všechny ostatní kombinace zahrnovat ponechat za typ prostředku, který nemůže být ponecháno za při přesunu plán služby App Service (libovolný typ prostředku služby App Service).</span><span class="sxs-lookup"><span data-stu-id="945fc-233">All other combinations involve leaving behind a resource type that can't be left behind when moving an App Service plan (any type of App Service resource).</span></span>

<span data-ttu-id="945fc-234">Pokud vaše webová aplikace se nachází v jiné skupině prostředků než jeho plán služby App Service, ale chcete přesunout na novou skupinu prostředků, je nutné provést přesun ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="945fc-234">If your web app resides in a different resource group than its App Service plan but you want to move both to a new resource group, you must perform the move in two steps.</span></span> <span data-ttu-id="945fc-235">Například:</span><span class="sxs-lookup"><span data-stu-id="945fc-235">For example:</span></span>

* <span data-ttu-id="945fc-236">**webové a** se nachází v **skupinu webových**</span><span class="sxs-lookup"><span data-stu-id="945fc-236">**web-a** resides in **web-group**</span></span>
* <span data-ttu-id="945fc-237">**plán a** se nachází v **plán skupiny**</span><span class="sxs-lookup"><span data-stu-id="945fc-237">**plan-a** resides in **plan-group**</span></span>
* <span data-ttu-id="945fc-238">Chcete, aby **webové a** a **plán a** být umístěné ve **kombinaci skupiny**</span><span class="sxs-lookup"><span data-stu-id="945fc-238">You want **web-a** and **plan-a** to reside in **combined-group**</span></span>

<span data-ttu-id="945fc-239">Aby bylo možné tento přesun, proveďte dvě samostatné přesunutí operace v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="945fc-239">To accomplish this move, perform two separate move operations in the following sequence:</span></span>

1. <span data-ttu-id="945fc-240">Přesunout **webové a** k **plán skupiny**</span><span class="sxs-lookup"><span data-stu-id="945fc-240">Move the **web-a** to **plan-group**</span></span>
2. <span data-ttu-id="945fc-241">Přesunout **webové a** a **plán a** k **kombinaci skupiny**.</span><span class="sxs-lookup"><span data-stu-id="945fc-241">Move **web-a** and **plan-a** to **combined-group**.</span></span>

<span data-ttu-id="945fc-242">Certifikát služby aplikace můžete přesunout do nové skupiny prostředků nebo předplatného bez problémů.</span><span class="sxs-lookup"><span data-stu-id="945fc-242">You can move an App Service Certificate to a new resource group or subscription without any issues.</span></span> <span data-ttu-id="945fc-243">Pokud vaše webová aplikace obsahuje certifikát SSL, který jste zakoupili externě a nahrané do aplikace, můžete před přesunutím webové aplikace musíte odstranit certifikát.</span><span class="sxs-lookup"><span data-stu-id="945fc-243">However, if your web app includes an SSL certificate that you purchased externally and uploaded to the app, you must delete the certificate before moving the web app.</span></span> <span data-ttu-id="945fc-244">Například můžete provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="945fc-244">For example, you can perform the following steps:</span></span>

1. <span data-ttu-id="945fc-245">Odstranění se nahraný certifikát z webové aplikace</span><span class="sxs-lookup"><span data-stu-id="945fc-245">Delete the uploaded certificate from the web app</span></span>
2. <span data-ttu-id="945fc-246">Přesunout webové aplikace</span><span class="sxs-lookup"><span data-stu-id="945fc-246">Move the web app</span></span>
3. <span data-ttu-id="945fc-247">Nahrajte certifikát do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="945fc-247">Upload the certificate to the web app</span></span>

## <a name="recovery-services-limitations"></a><span data-ttu-id="945fc-248">Omezení služby obnovení</span><span class="sxs-lookup"><span data-stu-id="945fc-248">Recovery Services limitations</span></span>
<span data-ttu-id="945fc-249">Přesunutí není povolen pro úložiště, sítě, nebo výpočetní prostředky, které jsou používána k nastavení zotavení po havárii s Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="945fc-249">Move is not enabled for Storage, Network, or Compute resources used to set up disaster recovery with Azure Site Recovery.</span></span>

<span data-ttu-id="945fc-250">Předpokládejme například, jste nastavili replikaci počítačů na místě na účet úložiště (Storage1) a chcete chráněného počítače přijít po převzetí služeb při selhání do Azure jako virtuální počítač (VM1) připojených k virtuální síti (Network1).</span><span class="sxs-lookup"><span data-stu-id="945fc-250">For example, suppose you have set up replication of your on-premises machines to a storage account (Storage1) and want the protected machine to come up after failover to Azure as a virtual machine (VM1) attached to a virtual network (Network1).</span></span> <span data-ttu-id="945fc-251">Některé z těchto prostředků Azure - Storage1, VM1 a Network1 - nelze přesunout skupiny prostředků v rámci stejného předplatného nebo pro odběry.</span><span class="sxs-lookup"><span data-stu-id="945fc-251">You cannot move any of these Azure resources - Storage1, VM1, and Network1 - across resource groups within the same subscription or across subscriptions.</span></span>

## <a name="hdinsight-limitations"></a><span data-ttu-id="945fc-252">Omezení HDInsight</span><span class="sxs-lookup"><span data-stu-id="945fc-252">HDInsight limitations</span></span>

<span data-ttu-id="945fc-253">Clustery HDInsight se můžete přesunout do nové předplatné nebo skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-253">You can move HDInsight clusters to a new subscription or resource group.</span></span> <span data-ttu-id="945fc-254">Však nelze přesouvat mezi odběry síťových prostředků propojené ke clusteru HDInsight (například virtuální sítě, síťové karty nebo nástroj pro vyrovnávání zatížení).</span><span class="sxs-lookup"><span data-stu-id="945fc-254">However, you cannot move across subscriptions the networking resources linked to the HDInsight cluster (such as the virtual network, NIC, or load balancer).</span></span> <span data-ttu-id="945fc-255">Kromě toho nelze přesunout do nové skupiny prostředků síťový adaptér, který je připojen k virtuálnímu počítači pro cluster.</span><span class="sxs-lookup"><span data-stu-id="945fc-255">In addition, you cannot move to a new resource group a NIC that is attached to a virtual machine for the cluster.</span></span>

<span data-ttu-id="945fc-256">Při přesunu clusteru HDInsight na nové předplatné, nejprve přesunete jiné prostředky (například účet úložiště).</span><span class="sxs-lookup"><span data-stu-id="945fc-256">When moving an HDInsight cluster to a new subscription, first move other resources (like the storage account).</span></span> <span data-ttu-id="945fc-257">HDInsight cluster, pak přesuňte samostatně.</span><span class="sxs-lookup"><span data-stu-id="945fc-257">Then, move the HDInsight cluster by itself.</span></span>

## <a name="classic-deployment-limitations"></a><span data-ttu-id="945fc-258">Omezení nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="945fc-258">Classic deployment limitations</span></span>
<span data-ttu-id="945fc-259">Možnosti pro přesun prostředků nasazené pomocí klasického modelu se liší v závislosti na tom, jestli jsou přesun prostředků v rámci předplatného nebo do nového předplatného.</span><span class="sxs-lookup"><span data-stu-id="945fc-259">The options for moving resources deployed through the classic model differ based on whether you are moving the resources within a subscription or to a new subscription.</span></span>

### <a name="same-subscription"></a><span data-ttu-id="945fc-260">Stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="945fc-260">Same subscription</span></span>
<span data-ttu-id="945fc-261">Při přesouvání prostředků z jedné skupiny prostředků do jiné skupiny prostředků v rámci stejného předplatného, platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="945fc-261">When moving resources from one resource group to another resource group within the same subscription, the following restrictions apply:</span></span>

* <span data-ttu-id="945fc-262">Nelze přesunout virtuální sítě (klasické).</span><span class="sxs-lookup"><span data-stu-id="945fc-262">Virtual networks (classic) cannot be moved.</span></span>
* <span data-ttu-id="945fc-263">Virtuální počítače (klasické) je třeba přesunout s cloudovou službou.</span><span class="sxs-lookup"><span data-stu-id="945fc-263">Virtual machines (classic) must be moved with the cloud service.</span></span>
* <span data-ttu-id="945fc-264">Cloudové služby se dají přesunout pouze při přesunutí zahrnuje všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="945fc-264">Cloud service can only be moved when the move includes all its virtual machines.</span></span>
* <span data-ttu-id="945fc-265">Současně lze přesunout jenom jeden cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="945fc-265">Only one cloud service can be moved at a time.</span></span>
* <span data-ttu-id="945fc-266">Současně lze přesouvat pouze jeden účet úložiště (klasické).</span><span class="sxs-lookup"><span data-stu-id="945fc-266">Only one storage account (classic) can be moved at a time.</span></span>
* <span data-ttu-id="945fc-267">Účet úložiště (klasické) nelze přesunout do stejné operace virtuálního počítače nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="945fc-267">Storage account (classic) cannot be moved in the same operation with a virtual machine or a cloud service.</span></span>

<span data-ttu-id="945fc-268">K přesunutí klasické prostředky na novou skupinu prostředků v rámci stejného předplatného, použijte standardní Přesunutí operací prostřednictvím [portál](#use-portal), [prostředí Azure PowerShell](#use-powershell), [rozhraní příkazového řádku Azure](#use-azure-cli), nebo [rozhraní REST API](#use-rest-api).</span><span class="sxs-lookup"><span data-stu-id="945fc-268">To move classic resources to a new resource group within the same subscription, use the standard move operations through the [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), or [REST API](#use-rest-api).</span></span> <span data-ttu-id="945fc-269">Můžete použít stejné operace jako použijte pro přesun prostředků Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="945fc-269">You use the same operations as you use for moving Resource Manager resources.</span></span>

### <a name="new-subscription"></a><span data-ttu-id="945fc-270">Nové předplatné</span><span class="sxs-lookup"><span data-stu-id="945fc-270">New subscription</span></span>
<span data-ttu-id="945fc-271">Při přesunu prostředků do nového předplatného, platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="945fc-271">When moving resources to a new subscription, the following restrictions apply:</span></span>

* <span data-ttu-id="945fc-272">Všechny klasické prostředky v předplatném je třeba přesunout v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="945fc-272">All classic resources in the subscription must be moved in the same operation.</span></span>
* <span data-ttu-id="945fc-273">Cílové předplatné nesmí obsahovat všechny klasické prostředky.</span><span class="sxs-lookup"><span data-stu-id="945fc-273">The target subscription must not contain any other classic resources.</span></span>
* <span data-ttu-id="945fc-274">Přesunutí může požadovat pouze přes samostatné REST API pro classic přesune.</span><span class="sxs-lookup"><span data-stu-id="945fc-274">The move can only be requested through a separate REST API for classic moves.</span></span> <span data-ttu-id="945fc-275">Standardní příkazy přesunutí Resource Manager nefungují při přesunu klasické prostředky na nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="945fc-275">The standard Resource Manager move commands do not work when moving classic resources to a new subscription.</span></span>

<span data-ttu-id="945fc-276">Chcete-li přesunout klasické prostředky do nového předplatného, použijte operace REST, které jsou specifické pro klasické prostředky.</span><span class="sxs-lookup"><span data-stu-id="945fc-276">To move classic resources to a new subscription, use the REST operations that are specific to classic resources.</span></span> <span data-ttu-id="945fc-277">Abyste mohli použít REST, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="945fc-277">To use REST, perform the following steps:</span></span>

1. <span data-ttu-id="945fc-278">Zkontrolujte, pokud zdrojové předplatné mohl účastnit přesunu mezi předplatnými.</span><span class="sxs-lookup"><span data-stu-id="945fc-278">Check if the source subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="945fc-279">Použijte následující operace:</span><span class="sxs-lookup"><span data-stu-id="945fc-279">Use the following operation:</span></span>

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="945fc-280">V textu požadavku patří:</span><span class="sxs-lookup"><span data-stu-id="945fc-280">In the request body, include:</span></span>

  ```json
  {
    "role": "source"
  }
  ```

     <span data-ttu-id="945fc-281">Odpověď pro operace ověření je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="945fc-281">The response for the validation operation is in the following format:</span></span>

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. <span data-ttu-id="945fc-282">Zkontrolujte, pokud cílového odběru mohl účastnit přesunu mezi předplatnými.</span><span class="sxs-lookup"><span data-stu-id="945fc-282">Check if the destination subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="945fc-283">Použijte následující operace:</span><span class="sxs-lookup"><span data-stu-id="945fc-283">Use the following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="945fc-284">V textu požadavku patří:</span><span class="sxs-lookup"><span data-stu-id="945fc-284">In the request body, include:</span></span>

  ```json
  {
    "role": "target"
  }
  ```

     <span data-ttu-id="945fc-285">Odpověď je ve stejném formátu jako zdroj ověření předplatného.</span><span class="sxs-lookup"><span data-stu-id="945fc-285">The response is in the same format as the source subscription validation.</span></span>
3. <span data-ttu-id="945fc-286">Pokud oba odběry projít ověřením, přesune všechny klasické prostředky z jedno předplatné do jiného předplatného s následující operace:</span><span class="sxs-lookup"><span data-stu-id="945fc-286">If both subscriptions pass validation, move all classic resources from one subscription to another subscription with the following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    <span data-ttu-id="945fc-287">V textu požadavku patří:</span><span class="sxs-lookup"><span data-stu-id="945fc-287">In the request body, include:</span></span>

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

<span data-ttu-id="945fc-288">Operaci může běžet několik minut.</span><span class="sxs-lookup"><span data-stu-id="945fc-288">The operation may run for several minutes.</span></span>

## <a name="use-portal"></a><span data-ttu-id="945fc-289">Použít portál</span><span class="sxs-lookup"><span data-stu-id="945fc-289">Use portal</span></span>
<span data-ttu-id="945fc-290">Chcete-li přesunout prostředky, vyberte skupinu prostředků obsahující tyto prostředky a pak vyberte **přesunout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="945fc-290">To move resources, select the resource group containing those resources, and then select the **Move** button.</span></span>

![Přesunutí prostředků](./media/resource-group-move-resources/select-move.png)

<span data-ttu-id="945fc-292">Vyberte, jestli přecházíte prostředky na novou skupinu prostředků nebo nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="945fc-292">Select whether you are moving the resources to a new resource group or a new subscription.</span></span>

<span data-ttu-id="945fc-293">Vyberte zdroje, které chcete přesunout a cílové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-293">Select the resources to move and the destination resource group.</span></span> <span data-ttu-id="945fc-294">Na vědomí, že budete muset aktualizovat skripty pro tyto prostředky a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="945fc-294">Acknowledge that you need to update scripts for these resources and select **OK**.</span></span> <span data-ttu-id="945fc-295">Pokud jste v předchozím kroku vybrali na ikonu pro úpravu předplatné, musíte také vybrat cílového odběru.</span><span class="sxs-lookup"><span data-stu-id="945fc-295">If you selected the edit subscription icon in the previous step, you must also select the destination subscription.</span></span>

![Vyberte cíl](./media/resource-group-move-resources/select-destination.png)

<span data-ttu-id="945fc-297">V **oznámení**, uvidíte, že je spuštěna operaci přesunutí.</span><span class="sxs-lookup"><span data-stu-id="945fc-297">In **Notifications**, you see that the move operation is running.</span></span>

![Zobrazit stav přesunutí](./media/resource-group-move-resources/show-status.png)

<span data-ttu-id="945fc-299">Pokud ho dokončit, zobrazí se zpráva výsledku.</span><span class="sxs-lookup"><span data-stu-id="945fc-299">When it has completed, you are notified of the result.</span></span>

![Zobrazit přesunout výsledků](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a><span data-ttu-id="945fc-301">Použití prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="945fc-301">Use PowerShell</span></span>
<span data-ttu-id="945fc-302">Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, použijte `Move-AzureRmResource` příkaz.</span><span class="sxs-lookup"><span data-stu-id="945fc-302">To move existing resources to another resource group or subscription, use the `Move-AzureRmResource` command.</span></span>

<span data-ttu-id="945fc-303">V prvním příkladu ukazuje, jak přesunout jeden prostředek do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-303">The first example shows how to move one resource to a new resource group.</span></span>

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

<span data-ttu-id="945fc-304">Druhý příklad ukazuje, jak přesunout více prostředků do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-304">The second example shows how to move multiple resources to a new resource group.</span></span>

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

<span data-ttu-id="945fc-305">Pokud chcete přesunout do nového předplatného, obsahovat hodnotu pro `DestinationSubscriptionId` parametr.</span><span class="sxs-lookup"><span data-stu-id="945fc-305">To move to a new subscription, include a value for the `DestinationSubscriptionId` parameter.</span></span>

<span data-ttu-id="945fc-306">Zobrazí se výzva k potvrzení, že chcete přesunout zadané prostředky.</span><span class="sxs-lookup"><span data-stu-id="945fc-306">You are asked to confirm that you want to move the specified resources.</span></span>

```powershell
Confirm
Are you sure you want to move these resources to the resource group
'/subscriptions/{guid}/resourceGroups/newRG' the resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a><span data-ttu-id="945fc-307">Použití Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="945fc-307">Use Azure CLI 2.0</span></span>
<span data-ttu-id="945fc-308">Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, použijte `az resource move` příkaz.</span><span class="sxs-lookup"><span data-stu-id="945fc-308">To move existing resources to another resource group or subscription, use the `az resource move` command.</span></span> <span data-ttu-id="945fc-309">Zadejte ID prostředků pro přesun prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-309">Provide the resource IDs of the resources to move.</span></span> <span data-ttu-id="945fc-310">Můžete získat ID prostředků pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="945fc-310">You can get resource IDs with the following command:</span></span>

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

<span data-ttu-id="945fc-311">Následující příklad ukazuje, jak přesunout do nové skupiny prostředků účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="945fc-311">The following example shows how to move a storage account to a new resource group.</span></span> <span data-ttu-id="945fc-312">V `--ids` parametr, zadejte místo oddělený seznam ID pro přesun prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-312">In the `--ids` parameter, provide a space-separated list of the resource IDs to move.</span></span>

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

<span data-ttu-id="945fc-313">Chcete-li přesunout do nového předplatného, zadat `--destination-subscription-id` parametr.</span><span class="sxs-lookup"><span data-stu-id="945fc-313">To move to a new subscription, provide the `--destination-subscription-id` parameter.</span></span>

## <a name="use-azure-cli-10"></a><span data-ttu-id="945fc-314">Použití Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="945fc-314">Use Azure CLI 1.0</span></span>
<span data-ttu-id="945fc-315">Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, použijte `azure resource move` příkaz.</span><span class="sxs-lookup"><span data-stu-id="945fc-315">To move existing resources to another resource group or subscription, use the `azure resource move` command.</span></span> <span data-ttu-id="945fc-316">Zadejte ID prostředků pro přesun prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-316">Provide the resource IDs of the resources to move.</span></span> <span data-ttu-id="945fc-317">Můžete získat ID prostředků pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="945fc-317">You can get resource IDs with the following command:</span></span>

```azurecli
azure resource list -g sourceGroup --json
```

<span data-ttu-id="945fc-318">Která vrací v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="945fc-318">Which returns the following format:</span></span>

```azurecli
[
  {
    "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
    "name": "storagedemo",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "southcentralus",
    "tags": {},
    "kind": "Storage",
    "sku": {
      "name": "Standard_RAGRS",
      "tier": "Standard"
    }
  }
]
```

<span data-ttu-id="945fc-319">Následující příklad ukazuje, jak přesunout do nové skupiny prostředků účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="945fc-319">The following example shows how to move a storage account to a new resource group.</span></span> <span data-ttu-id="945fc-320">V `-i` parametr zadejte čárkami oddělený seznam ID pro přesun prostředků.</span><span class="sxs-lookup"><span data-stu-id="945fc-320">In the `-i` parameter, provide a comma-separated list of the resource IDs to move.</span></span>

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

<span data-ttu-id="945fc-321">Zobrazí se výzva k potvrzení, že chcete přesunout zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="945fc-321">You are asked to confirm that you want to move the specified resource.</span></span>

## <a name="use-rest-api"></a><span data-ttu-id="945fc-322">Použití rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="945fc-322">Use REST API</span></span>
<span data-ttu-id="945fc-323">Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, spusťte:</span><span class="sxs-lookup"><span data-stu-id="945fc-323">To move existing resources to another resource group or subscription, run:</span></span>

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

<span data-ttu-id="945fc-324">V těle žádosti je zadat cílová skupina prostředků a prostředky, které chcete přesunout.</span><span class="sxs-lookup"><span data-stu-id="945fc-324">In the request body, you specify the target resource group and the resources to move.</span></span> <span data-ttu-id="945fc-325">Další informace o operaci REST přesunutí najdete v tématu [přesunout prostředky](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span><span class="sxs-lookup"><span data-stu-id="945fc-325">For more information about the move REST operation, see [Move resources](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="945fc-326">Další kroky</span><span class="sxs-lookup"><span data-stu-id="945fc-326">Next steps</span></span>
* <span data-ttu-id="945fc-327">Další informace o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="945fc-327">To learn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="945fc-328">Další informace o rozhraní příkazového řádku Azure pro správu předplatného najdete v tématu [pomocí rozhraní příkazového řádku Azure s Resource Managerem](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="945fc-328">To learn about Azure CLI commands for managing your subscription, see [Using the Azure CLI with Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="945fc-329">Další informace o portálu funkce pro správu předplatného najdete v tématu [pomocí portálu Azure ke správě prostředků](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="945fc-329">To learn about portal features for managing your subscription, see [Using the Azure portal to manage resources](resource-group-portal.md).</span></span>
* <span data-ttu-id="945fc-330">Další informace o použití logického organizace pro vaše prostředky najdete v tématu [použití značek k uspořádání prostředků](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="945fc-330">To learn about applying a logical organization to your resources, see [Using tags to organize your resources](resource-group-using-tags.md).</span></span>
