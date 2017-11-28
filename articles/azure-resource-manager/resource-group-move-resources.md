---
title: "aaaMove prostředky Azure toonew předplatné nebo prostředek skupiny | Microsoft Docs"
description: "Pomocí Azure Resource Manager toomove prostředky tooa novou skupinu prostředků nebo předplatného."
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
ms.openlocfilehash: 09d35f0afbbcdc0c66779f98a982d878f0807497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-resources-toonew-resource-group-or-subscription"></a><span data-ttu-id="60a07-103">Přesunout skupiny prostředků toonew prostředků nebo předplatného</span><span class="sxs-lookup"><span data-stu-id="60a07-103">Move resources toonew resource group or subscription</span></span>
<span data-ttu-id="60a07-104">Toto téma ukazuje, jak toomove prostředky tooeither nové předplatné nebo nový prostředek skupiny v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="60a07-104">This topic shows you how toomove resources tooeither a new subscription or a new resource group in hello same subscription.</span></span> <span data-ttu-id="60a07-105">Můžete použít hello portálu, prostředí PowerShell, rozhraní příkazového řádku Azure nebo hello prostředků toomove REST API.</span><span class="sxs-lookup"><span data-stu-id="60a07-105">You can use hello portal, PowerShell, Azure CLI, or hello REST API toomove resource.</span></span> <span data-ttu-id="60a07-106">operace přesunutí Hello v tomto tématu jsou k dispozici tooyou bez jakékoli požádat o pomoc podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="60a07-106">hello move operations in this topic are available tooyou without any assistance from Azure support.</span></span>

<span data-ttu-id="60a07-107">Při přesouvání prostředků, zdrojová skupina hello i hello cílové skupiny jsou zamčené během operace hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-107">When moving resources, both hello source group and hello target group are locked during hello operation.</span></span> <span data-ttu-id="60a07-108">Zápis a operace odstranění jsou zablokované o skupinách prostředků hello až po dokončení přesunu hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-108">Write and delete operations are blocked on hello resource groups until hello move completes.</span></span> <span data-ttu-id="60a07-109">Tato Zámek znamená nelze přidat, aktualizovat nebo odstranit prostředky v hello skupiny prostředků, ale neznamená, že zmrazené hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="60a07-109">This lock means you cannot add, update, or delete resources in hello resource groups, but it does not mean hello resources are frozen.</span></span> <span data-ttu-id="60a07-110">Například pokud přesunete systému SQL Server a její databáze tooa novou skupinu prostředků, aplikace, která používá databázi hello vyskytne nedojde k žádnému výpadku.</span><span class="sxs-lookup"><span data-stu-id="60a07-110">For example, if you move a SQL Server and its database tooa new resource group, an application that uses hello database experiences no downtime.</span></span> <span data-ttu-id="60a07-111">Stále můžete číst a zapisovat toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="60a07-111">It can still read and write toohello database.</span></span>

<span data-ttu-id="60a07-112">Nelze změnit umístění hello hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="60a07-112">You cannot change hello location of hello resource.</span></span> <span data-ttu-id="60a07-113">Přesunutí prostředku pouze přesune ho tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-113">Moving a resource only moves it tooa new resource group.</span></span> <span data-ttu-id="60a07-114">Hello novou skupinu prostředků může mít jiné umístění, ale který nezmění umístění hello hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="60a07-114">hello new resource group may have a different location, but that does not change hello location of hello resource.</span></span>

> [!NOTE]
> <span data-ttu-id="60a07-115">Tento článek popisuje, jak účet nabízení toomove prostředky v rámci existující Azure.</span><span class="sxs-lookup"><span data-stu-id="60a07-115">This article describes how toomove resources within an existing Azure account offering.</span></span> <span data-ttu-id="60a07-116">Pokud chcete ve skutečnosti toochange účtu Azure nabídky (například upgrade z průběžnými platbami toopre platím) přitom dál toowork se stávajícími prostředky, najdete v [přepínač vaši nabídku tooanother předplatné](../billing/billing-how-to-switch-azure-offer.md).</span><span class="sxs-lookup"><span data-stu-id="60a07-116">If you actually want toochange your Azure account offering (such as upgrading from pay-as-you-go toopre-pay) while continuing toowork with your existing resources, see [Switch your Azure subscription tooanother offer](../billing/billing-how-to-switch-azure-offer.md).</span></span>
>
>

## <a name="checklist-before-moving-resources"></a><span data-ttu-id="60a07-117">Kontrolní seznam před přesunutím prostředků</span><span class="sxs-lookup"><span data-stu-id="60a07-117">Checklist before moving resources</span></span>
<span data-ttu-id="60a07-118">Existují některé důležité kroky tooperform před přesunutím prostředku.</span><span class="sxs-lookup"><span data-stu-id="60a07-118">There are some important steps tooperform before moving a resource.</span></span> <span data-ttu-id="60a07-119">Ověřením těchto podmínek se můžete vyhnout chybám.</span><span class="sxs-lookup"><span data-stu-id="60a07-119">By verifying these conditions, you can avoid errors.</span></span>

1. <span data-ttu-id="60a07-120">Hello zdrojové a cílové odběry, musí existovat v rámci hello stejné [klienta Azure Active Directory](../active-directory/active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="60a07-120">hello source and destination subscriptions must exist within hello same [Azure Active Directory tenant](../active-directory/active-directory-howto-tenant.md).</span></span> <span data-ttu-id="60a07-121">toocheck oba odběry mají hello stejné ID klienta, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="60a07-121">toocheck that both subscriptions have hello same tenant ID, use Azure PowerShell or Azure CLI.</span></span>

  <span data-ttu-id="60a07-122">Pro Azure PowerShell použijte:</span><span class="sxs-lookup"><span data-stu-id="60a07-122">For Azure PowerShell, use:</span></span>

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  <span data-ttu-id="60a07-123">Azure CLI 2.0 použijte:</span><span class="sxs-lookup"><span data-stu-id="60a07-123">For Azure CLI 2.0, use:</span></span>

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  <span data-ttu-id="60a07-124">Pokud ID klienta hello nejsou hello zdrojové a cílové předplatné hello stejné, můžete se pokusit toochange hello adresáře pro předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-124">If hello tenant IDs for hello source and destination subscriptions are not hello same, you can attempt toochange hello directory for hello subscription.</span></span> <span data-ttu-id="60a07-125">Tato možnost je však pouze k dispozici tooService správci, kteří jsou podepsané pomocí účtu Microsoft (není účet organizace).</span><span class="sxs-lookup"><span data-stu-id="60a07-125">However, this option is only available tooService Administrators who are signed in with a Microsoft account (not an organizational account).</span></span> <span data-ttu-id="60a07-126">Změna hello adresáře, přihlášení toohello tooattempt [portálu classic](https://manage.windowsazure.com/)a vyberte **nastavení**a vyberte předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-126">tooattempt changing hello directory, log in toohello [classic portal](https://manage.windowsazure.com/), and select **Settings**, and select hello subscription.</span></span> <span data-ttu-id="60a07-127">Pokud hello **upravit adresář** ikonu je k dispozici, vyberte ji toochange hello přidružené Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="60a07-127">If hello **Edit Directory** icon is available, select it toochange hello associated Azure Active Directory.</span></span>

  ![Upravit adresář](./media/resource-group-move-resources/edit-directory.png)

  <span data-ttu-id="60a07-129">Pokud tuto ikonu není k dispozici, obraťte se na podporu toomove hello prostředky tooa nového klienta.</span><span class="sxs-lookup"><span data-stu-id="60a07-129">If that icon is not available, you must contact support toomove hello resources tooa new tenant.</span></span>

2. <span data-ttu-id="60a07-130">Hello služby musíte povolit hello možnost toomove prostředky.</span><span class="sxs-lookup"><span data-stu-id="60a07-130">hello service must enable hello ability toomove resources.</span></span> <span data-ttu-id="60a07-131">Toto téma uvádí služby, které Povolit přesunutí prostředků a služby, které nepovolíte přesunutí prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-131">This topic lists which services enable moving resources and which services do not enable moving resources.</span></span>
3. <span data-ttu-id="60a07-132">Hello cílového odběru musí být zaregistrován pro poskytovatele prostředků hello přesouvání prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-132">hello destination subscription must be registered for hello resource provider of hello resource being moved.</span></span> <span data-ttu-id="60a07-133">Pokud ne, se zobrazí chyba s oznámením, že hello **předplatné není zaregistrované pro typ prostředku**.</span><span class="sxs-lookup"><span data-stu-id="60a07-133">If not, you receive an error stating that hello **subscription is not registered for a resource type**.</span></span> <span data-ttu-id="60a07-134">Tento problém může dojít při přesouvání prostředků tooa nové předplatné, ale toto předplatné dosud nebyl použit s tohoto typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="60a07-134">You might encounter this problem when moving a resource tooa new subscription, but that subscription has never been used with that resource type.</span></span> <span data-ttu-id="60a07-135">toolearn způsobu toocheck hello stav registrace a registraci zprostředkovatele prostředků najdete v části [zprostředkovatelé prostředků a typy](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="60a07-135">toolearn how toocheck hello registration status and register resource providers, see [Resource providers and types](resource-manager-supported-services.md).</span></span>

## <a name="when-toocall-support"></a><span data-ttu-id="60a07-136">Když toocall podporovat</span><span class="sxs-lookup"><span data-stu-id="60a07-136">When toocall support</span></span>
<span data-ttu-id="60a07-137">Většina prostředkům prostřednictvím operace hello samoobslužné služby zobrazí v tomto tématu se můžete přesunout.</span><span class="sxs-lookup"><span data-stu-id="60a07-137">You can move most resources through hello self-service operations shown in this topic.</span></span> <span data-ttu-id="60a07-138">Použijte na operací hello samoobslužné služby:</span><span class="sxs-lookup"><span data-stu-id="60a07-138">Use hello self-service operations to:</span></span>

* <span data-ttu-id="60a07-139">Přesouvání prostředků Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="60a07-139">Move Resource Manager resources.</span></span>
* <span data-ttu-id="60a07-140">Přesunout klasické prostředky podle toohello [omezení nasazení classic](#classic-deployment-limitations).</span><span class="sxs-lookup"><span data-stu-id="60a07-140">Move classic resources according toohello [classic deployment limitations](#classic-deployment-limitations).</span></span>

<span data-ttu-id="60a07-141">Až budete potřebovat, obraťte se na podporu:</span><span class="sxs-lookup"><span data-stu-id="60a07-141">Call support when you need to:</span></span>

* <span data-ttu-id="60a07-142">Přesunete prostředky tooa nový účet Azure (a klienta Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="60a07-142">Move your resources tooa new Azure account (and Azure Active Directory tenant).</span></span>
* <span data-ttu-id="60a07-143">Přesunout klasické prostředky, ale máte potíže se omezeních hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-143">Move classic resources but are having trouble with hello limitations.</span></span>

## <a name="services-that-enable-move"></a><span data-ttu-id="60a07-144">Služby, které umožňují přesunout</span><span class="sxs-lookup"><span data-stu-id="60a07-144">Services that enable move</span></span>
<span data-ttu-id="60a07-145">Prozatím se hello služeb, které umožňují přesunutí tooboth novou skupinu prostředků a předplatného jsou:</span><span class="sxs-lookup"><span data-stu-id="60a07-145">For now, hello services that enable moving tooboth a new resource group and subscription are:</span></span>

* <span data-ttu-id="60a07-146">API Management</span><span class="sxs-lookup"><span data-stu-id="60a07-146">API Management</span></span>
* <span data-ttu-id="60a07-147">Aplikace služby App Service (webové aplikace) – viz [omezení služby App Service](#app-service-limitations)</span><span class="sxs-lookup"><span data-stu-id="60a07-147">App Service apps (web apps) - see [App Service limitations](#app-service-limitations)</span></span>
* <span data-ttu-id="60a07-148">Application Insights</span><span class="sxs-lookup"><span data-stu-id="60a07-148">Application Insights</span></span>
* <span data-ttu-id="60a07-149">Automation</span><span class="sxs-lookup"><span data-stu-id="60a07-149">Automation</span></span>
* <span data-ttu-id="60a07-150">Batch</span><span class="sxs-lookup"><span data-stu-id="60a07-150">Batch</span></span>
* <span data-ttu-id="60a07-151">Mapy Bing</span><span class="sxs-lookup"><span data-stu-id="60a07-151">Bing Maps</span></span>
* <span data-ttu-id="60a07-152">CDN</span><span class="sxs-lookup"><span data-stu-id="60a07-152">CDN</span></span>
* <span data-ttu-id="60a07-153">Cloudové služby - viz [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="60a07-153">Cloud Services - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="60a07-154">Kognitivní služby</span><span class="sxs-lookup"><span data-stu-id="60a07-154">Cognitive Services</span></span>
* <span data-ttu-id="60a07-155">Content Moderator</span><span class="sxs-lookup"><span data-stu-id="60a07-155">Content Moderator</span></span>
* <span data-ttu-id="60a07-156">Data Catalog</span><span class="sxs-lookup"><span data-stu-id="60a07-156">Data Catalog</span></span>
* <span data-ttu-id="60a07-157">Data Factory</span><span class="sxs-lookup"><span data-stu-id="60a07-157">Data Factory</span></span>
* <span data-ttu-id="60a07-158">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="60a07-158">Data Lake Analytics</span></span>
* <span data-ttu-id="60a07-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="60a07-159">Data Lake Store</span></span>
* <span data-ttu-id="60a07-160">DNS</span><span class="sxs-lookup"><span data-stu-id="60a07-160">DNS</span></span>
* <span data-ttu-id="60a07-161">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="60a07-161">Azure Cosmos DB</span></span>
* <span data-ttu-id="60a07-162">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="60a07-162">Event Hubs</span></span>
* <span data-ttu-id="60a07-163">Clustery HDInsight - najdete v části [omezení HDInsight](#hdinsight-limitations)</span><span class="sxs-lookup"><span data-stu-id="60a07-163">HDInsight clusters - see [HDInsight limitations](#hdinsight-limitations)</span></span>
* <span data-ttu-id="60a07-164">Centra IoT</span><span class="sxs-lookup"><span data-stu-id="60a07-164">IoT Hubs</span></span>
* <span data-ttu-id="60a07-165">Key Vault</span><span class="sxs-lookup"><span data-stu-id="60a07-165">Key Vault</span></span>
* <span data-ttu-id="60a07-166">Nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="60a07-166">Load Balancers</span></span>
* <span data-ttu-id="60a07-167">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="60a07-167">Logic Apps</span></span>
* <span data-ttu-id="60a07-168">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="60a07-168">Machine Learning</span></span>
* <span data-ttu-id="60a07-169">Media Services</span><span class="sxs-lookup"><span data-stu-id="60a07-169">Media Services</span></span>
* <span data-ttu-id="60a07-170">Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="60a07-170">Mobile Engagement</span></span>
* <span data-ttu-id="60a07-171">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="60a07-171">Notification Hubs</span></span>
* <span data-ttu-id="60a07-172">Operational Insights</span><span class="sxs-lookup"><span data-stu-id="60a07-172">Operational Insights</span></span>
* <span data-ttu-id="60a07-173">Správa operací</span><span class="sxs-lookup"><span data-stu-id="60a07-173">Operations Management</span></span>
* <span data-ttu-id="60a07-174">Power BI</span><span class="sxs-lookup"><span data-stu-id="60a07-174">Power BI</span></span>
* <span data-ttu-id="60a07-175">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="60a07-175">Redis Cache</span></span>
* <span data-ttu-id="60a07-176">Scheduler</span><span class="sxs-lookup"><span data-stu-id="60a07-176">Scheduler</span></span>
* <span data-ttu-id="60a07-177">Search</span><span class="sxs-lookup"><span data-stu-id="60a07-177">Search</span></span>
* <span data-ttu-id="60a07-178">Správa serveru</span><span class="sxs-lookup"><span data-stu-id="60a07-178">Server Management</span></span>
* <span data-ttu-id="60a07-179">Service Bus</span><span class="sxs-lookup"><span data-stu-id="60a07-179">Service Bus</span></span>
* <span data-ttu-id="60a07-180">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="60a07-180">Service Fabric</span></span>
* <span data-ttu-id="60a07-181">Úložiště</span><span class="sxs-lookup"><span data-stu-id="60a07-181">Storage</span></span>
* <span data-ttu-id="60a07-182">Najdete v části úložiště (klasické) - [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="60a07-182">Storage (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="60a07-183">Stream Analytics - Stream Analytics úlohy nelze přesunout, při spuštění ve stavu.</span><span class="sxs-lookup"><span data-stu-id="60a07-183">Stream Analytics - Stream Analytics jobs cannot be moved when in running state.</span></span>
* <span data-ttu-id="60a07-184">Databáze systému SQL server - hello databázi a server se musí nacházet v hello stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-184">SQL Database server - hello database and server must reside in hello same resource group.</span></span> <span data-ttu-id="60a07-185">Když přesouváte systému SQL server, budou přesunuty také všechny její databáze.</span><span class="sxs-lookup"><span data-stu-id="60a07-185">When you move a SQL server, all its databases are also moved.</span></span>
* <span data-ttu-id="60a07-186">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="60a07-186">Traffic Manager</span></span>
* <span data-ttu-id="60a07-187">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="60a07-187">Virtual Machines</span></span>
* <span data-ttu-id="60a07-188">Virtuální počítače s certifikát uložený v Key Vault - přesun toonew prostředků skupiny v rámci stejného předplatného je povoleno, ale přesun křížové předplatného není povolená.</span><span class="sxs-lookup"><span data-stu-id="60a07-188">Virtual Machines with certificate stored in Key Vault - Move toonew resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="60a07-189">Virtuální počítače (klasické) - najdete v části [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="60a07-189">Virtual Machines (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="60a07-190">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="60a07-190">Virtual Machine Scale Sets</span></span>
* <span data-ttu-id="60a07-191">Virtuální sítě - aktuálně, peered virtuální sítě nelze přesunout, dokud partnerský vztah virtuální sítě je zakázané.</span><span class="sxs-lookup"><span data-stu-id="60a07-191">Virtual Networks - Currently, a peered Virtual Network cannot be moved until VNet peering has been disabled.</span></span> <span data-ttu-id="60a07-192">Jakmile zakázaná, hello virtuální sítě možné úspěšně přesunout a lze je povolit partnerský vztah hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="60a07-192">Once disabled, hello Virtual Network can be moved successfully and hello VNet peering can be enabled.</span></span> <span data-ttu-id="60a07-193">Kromě toho virtuální sítě nesmí být přesunutý tooa jiného předplatného, pokud hello virtuální síť obsahuje všechny podsítě s odkazy na zdroje navigace.</span><span class="sxs-lookup"><span data-stu-id="60a07-193">In addition, a Virtual Network cannot be moved tooa different subscription if hello Virtual Network contains any subnet with resource navigation links.</span></span> <span data-ttu-id="60a07-194">Například podsíť virtuální sítě nemá navigační odkaz na prostředek, pokud prostředek redis Microsoft.Cache je nasazený do dané podsítě.</span><span class="sxs-lookup"><span data-stu-id="60a07-194">For example, a Virtual Network subnet has a resource navigation link when a Microsoft.Cache redis resource is deployed into this subnet.</span></span>
* <span data-ttu-id="60a07-195">VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="60a07-195">VPN Gateway</span></span>


## <a name="services-that-do-not-enable-move"></a><span data-ttu-id="60a07-196">Služby, které nepovolujte přesunutí</span><span class="sxs-lookup"><span data-stu-id="60a07-196">Services that do not enable move</span></span>
<span data-ttu-id="60a07-197">Hello služby, které aktuálně nepovolujte přesunutí prostředku jsou:</span><span class="sxs-lookup"><span data-stu-id="60a07-197">hello services that currently do not enable moving a resource are:</span></span>

* <span data-ttu-id="60a07-198">Služba AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="60a07-198">AD Domain Services</span></span>
* <span data-ttu-id="60a07-199">Hybridní AD Health Service</span><span class="sxs-lookup"><span data-stu-id="60a07-199">AD Hybrid Health Service</span></span>
* <span data-ttu-id="60a07-200">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="60a07-200">Application Gateway</span></span>
* <span data-ttu-id="60a07-201">S virtuálními počítači s disky spravovat sady dostupnosti</span><span class="sxs-lookup"><span data-stu-id="60a07-201">Availability sets with Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="60a07-202">BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="60a07-202">BizTalk Services</span></span>
* <span data-ttu-id="60a07-203">Container Service</span><span class="sxs-lookup"><span data-stu-id="60a07-203">Container Service</span></span>
* <span data-ttu-id="60a07-204">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="60a07-204">Express Route</span></span>
* <span data-ttu-id="60a07-205">DevTest Labs – přesun toonew prostředků skupiny v rámci stejného předplatného je povoleno, ale mezi přesun předplatného není povolená.</span><span class="sxs-lookup"><span data-stu-id="60a07-205">DevTest Labs - Move toonew resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="60a07-206">Dynamics LCS</span><span class="sxs-lookup"><span data-stu-id="60a07-206">Dynamics LCS</span></span>
* <span data-ttu-id="60a07-207">Bitové kopie vytvořené z spravované disků</span><span class="sxs-lookup"><span data-stu-id="60a07-207">Images created from Managed Disks</span></span>
* <span data-ttu-id="60a07-208">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="60a07-208">Managed Disks</span></span>
* <span data-ttu-id="60a07-209">Spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="60a07-209">Managed Applications</span></span>
* <span data-ttu-id="60a07-210">Trezor služeb zotavení – také provést není přesunutí hello výpočty, síť a úložiště prostředky přidružené služeb zotavení hello trezoru, najdete v části [služeb zotavení omezení](#recovery-services-limitations).</span><span class="sxs-lookup"><span data-stu-id="60a07-210">Recovery Services vault - also do not move hello Compute, Network, and Storage resources associated with hello Recovery Services vault, see [Recovery Services limitations](#recovery-services-limitations).</span></span>
* <span data-ttu-id="60a07-211">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="60a07-211">Security</span></span>
* <span data-ttu-id="60a07-212">Snímky vytvořené z spravované disky</span><span class="sxs-lookup"><span data-stu-id="60a07-212">Snapshots created from Managed Disks</span></span>
* <span data-ttu-id="60a07-213">Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60a07-213">StorSimple Device Manager</span></span>
* <span data-ttu-id="60a07-214">Virtuální počítače s spravované disky</span><span class="sxs-lookup"><span data-stu-id="60a07-214">Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="60a07-215">Najdete v části virtuální sítě (klasické) - [omezení nasazení Classic](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="60a07-215">Virtual Networks (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="60a07-216">Virtuální počítače vytvořené z Marketplace prostředků – nelze přesunout ve předplatných.</span><span class="sxs-lookup"><span data-stu-id="60a07-216">Virtual Machines created from Marketplace resources - cannot be moved across subscriptions.</span></span> <span data-ttu-id="60a07-217">Prostředek musí toobe zrušit v aktuálním předplatném hello a znovu nasadit v nové předplatné hello</span><span class="sxs-lookup"><span data-stu-id="60a07-217">Resource needs toobe deprovisioned in hello current subscription and deployed again in hello new subscription</span></span>

## <a name="app-service-limitations"></a><span data-ttu-id="60a07-218">Omezení služby App Service</span><span class="sxs-lookup"><span data-stu-id="60a07-218">App Service limitations</span></span>
<span data-ttu-id="60a07-219">Při práci s aplikacemi App Service, nemůžete přesunout pouze plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="60a07-219">When working with App Service apps, you cannot move only an App Service plan.</span></span> <span data-ttu-id="60a07-220">aplikace služby App Service toomove, možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="60a07-220">toomove App Service apps, your options are:</span></span>

* <span data-ttu-id="60a07-221">V této prostředků skupiny tooa novou skupinu prostředků, prostředků služby App Service již nemá přesuňte hello plán služby App Service a všechny ostatní prostředky služby App Service.</span><span class="sxs-lookup"><span data-stu-id="60a07-221">Move hello App Service plan and all other App Service resources in that resource group tooa new resource group that does not already have App Service resources.</span></span> <span data-ttu-id="60a07-222">Tento požadavek, že znamená, je nutné přesunout i hello prostředky služby App Service, nejsou přidružené hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="60a07-222">This requirement means you must move even hello App Service resources that are not associated with hello App Service plan.</span></span>
* <span data-ttu-id="60a07-223">Přesunout hello aplikace tooa jiné skupině prostředků, ale ponechat všechny plány služby App Service ve skupině prostředků původní hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-223">Move hello apps tooa different resource group, but keep all App Service plans in hello original resource group.</span></span>

<span data-ttu-id="60a07-224">Hello plán nemusí tooreside v App Service hello stejné skupině prostředků jako hello aplikace pro toofunction aplikace hello správně.</span><span class="sxs-lookup"><span data-stu-id="60a07-224">hello App Service plan does not need tooreside in hello same resource group as hello app for hello app toofunction correctly.</span></span>

<span data-ttu-id="60a07-225">Například, pokud obsahuje vaší skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="60a07-225">For example, if your resource group contains:</span></span>

* <span data-ttu-id="60a07-226">**webové a** který je přidružen **plánu a**</span><span class="sxs-lookup"><span data-stu-id="60a07-226">**web-a** which is associated with **plan-a**</span></span>
* <span data-ttu-id="60a07-227">**Web-b** který je přidružen **plán b**</span><span class="sxs-lookup"><span data-stu-id="60a07-227">**web-b** which is associated with **plan-b**</span></span>

<span data-ttu-id="60a07-228">Možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="60a07-228">Your options are:</span></span>

* <span data-ttu-id="60a07-229">Přesunout **webové a**, **plán a**, **web-b**, a **plán b**</span><span class="sxs-lookup"><span data-stu-id="60a07-229">Move **web-a**, **plan-a**, **web-b**, and **plan-b**</span></span>
* <span data-ttu-id="60a07-230">Přesunout **webové a** a **web-b**</span><span class="sxs-lookup"><span data-stu-id="60a07-230">Move **web-a** and **web-b**</span></span>
* <span data-ttu-id="60a07-231">Přesunout **webové a**</span><span class="sxs-lookup"><span data-stu-id="60a07-231">Move **web-a**</span></span>
* <span data-ttu-id="60a07-232">Přesunout **web-b**</span><span class="sxs-lookup"><span data-stu-id="60a07-232">Move **web-b**</span></span>

<span data-ttu-id="60a07-233">Všechny ostatní kombinace zahrnovat ponechat za typ prostředku, který nemůže být ponecháno za při přesunu plán služby App Service (libovolný typ prostředku služby App Service).</span><span class="sxs-lookup"><span data-stu-id="60a07-233">All other combinations involve leaving behind a resource type that can't be left behind when moving an App Service plan (any type of App Service resource).</span></span>

<span data-ttu-id="60a07-234">Pokud vaše webová aplikace se nachází v jiné skupině prostředků než jeho plán služby App Service, ale chcete toomove novou skupinu prostředků obou tooa, je nutné provést přesun hello ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="60a07-234">If your web app resides in a different resource group than its App Service plan but you want toomove both tooa new resource group, you must perform hello move in two steps.</span></span> <span data-ttu-id="60a07-235">Například:</span><span class="sxs-lookup"><span data-stu-id="60a07-235">For example:</span></span>

* <span data-ttu-id="60a07-236">**webové a** se nachází v **skupinu webových**</span><span class="sxs-lookup"><span data-stu-id="60a07-236">**web-a** resides in **web-group**</span></span>
* <span data-ttu-id="60a07-237">**plán a** se nachází v **plán skupiny**</span><span class="sxs-lookup"><span data-stu-id="60a07-237">**plan-a** resides in **plan-group**</span></span>
* <span data-ttu-id="60a07-238">Chcete, aby **webové a** a **plán a** tooreside v **kombinaci skupiny**</span><span class="sxs-lookup"><span data-stu-id="60a07-238">You want **web-a** and **plan-a** tooreside in **combined-group**</span></span>

<span data-ttu-id="60a07-239">tooaccomplish to přesunout, proveďte dvě samostatné přesunutí operace v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="60a07-239">tooaccomplish this move, perform two separate move operations in hello following sequence:</span></span>

1. <span data-ttu-id="60a07-240">Přesunout hello **webové a** příliš**plán skupiny**</span><span class="sxs-lookup"><span data-stu-id="60a07-240">Move hello **web-a** too**plan-group**</span></span>
2. <span data-ttu-id="60a07-241">Přesunout **webové a** a **plán a** příliš**kombinaci skupiny**.</span><span class="sxs-lookup"><span data-stu-id="60a07-241">Move **web-a** and **plan-a** too**combined-group**.</span></span>

<span data-ttu-id="60a07-242">Můžete přesunout certifikát služby aplikace tooa novou skupinu prostředků nebo předplatného bez problémů.</span><span class="sxs-lookup"><span data-stu-id="60a07-242">You can move an App Service Certificate tooa new resource group or subscription without any issues.</span></span> <span data-ttu-id="60a07-243">Ale pokud vaše webová aplikace obsahuje certifikát SSL zakoupili externě a nahrát aplikaci toohello, musíte odstranit certifikát hello před přesunutí hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="60a07-243">However, if your web app includes an SSL certificate that you purchased externally and uploaded toohello app, you must delete hello certificate before moving hello web app.</span></span> <span data-ttu-id="60a07-244">Můžete například provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60a07-244">For example, you can perform hello following steps:</span></span>

1. <span data-ttu-id="60a07-245">Odstranit hello nahrát certifikát z hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="60a07-245">Delete hello uploaded certificate from hello web app</span></span>
2. <span data-ttu-id="60a07-246">Přesunutí hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="60a07-246">Move hello web app</span></span>
3. <span data-ttu-id="60a07-247">Nahrání hello certifikát toohello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="60a07-247">Upload hello certificate toohello web app</span></span>

## <a name="recovery-services-limitations"></a><span data-ttu-id="60a07-248">Omezení služby obnovení</span><span class="sxs-lookup"><span data-stu-id="60a07-248">Recovery Services limitations</span></span>
<span data-ttu-id="60a07-249">Přesunutí není povoleno pro úložiště, sítě, nebo výpočetní prostředky používat tooset až zotavení po havárii s Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="60a07-249">Move is not enabled for Storage, Network, or Compute resources used tooset up disaster recovery with Azure Site Recovery.</span></span>

<span data-ttu-id="60a07-250">Například předpokládejme, že jste nastavili replikaci účtu místního počítače tooa úložiště (Storage1) a chcete hello chráněný počítač toocome až po převzetí služeb při selhání tooAzure jako virtuální počítač (VM1) připojen tooa virtuální sítě (Network1).</span><span class="sxs-lookup"><span data-stu-id="60a07-250">For example, suppose you have set up replication of your on-premises machines tooa storage account (Storage1) and want hello protected machine toocome up after failover tooAzure as a virtual machine (VM1) attached tooa virtual network (Network1).</span></span> <span data-ttu-id="60a07-251">Nelze přesunout některé z těchto prostředků Azure - Storage1, VM1, a Network1 - napříč prostředků skupiny v rámci hello stejné předplatné nebo ve předplatných.</span><span class="sxs-lookup"><span data-stu-id="60a07-251">You cannot move any of these Azure resources - Storage1, VM1, and Network1 - across resource groups within hello same subscription or across subscriptions.</span></span>

## <a name="hdinsight-limitations"></a><span data-ttu-id="60a07-252">Omezení HDInsight</span><span class="sxs-lookup"><span data-stu-id="60a07-252">HDInsight limitations</span></span>

<span data-ttu-id="60a07-253">Můžete přesunout HDInsight clustery tooa nové předplatné nebo skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-253">You can move HDInsight clusters tooa new subscription or resource group.</span></span> <span data-ttu-id="60a07-254">Však nelze přesouvat mezi odběry hello sítě clusteru HDInsight toohello propojené prostředky (například hello virtuální sítě, síťové karty nebo nástroj pro vyrovnávání zatížení).</span><span class="sxs-lookup"><span data-stu-id="60a07-254">However, you cannot move across subscriptions hello networking resources linked toohello HDInsight cluster (such as hello virtual network, NIC, or load balancer).</span></span> <span data-ttu-id="60a07-255">Kromě toho nelze přesunout tooa novou skupinu prostředků síťovou kartu, která je připojená tooa virtuálního počítače pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="60a07-255">In addition, you cannot move tooa new resource group a NIC that is attached tooa virtual machine for hello cluster.</span></span>

<span data-ttu-id="60a07-256">Při přesunu tooa clusteru HDInsight pro nové předplatné, nejprve přesunete jiné prostředky (například účet úložiště hello).</span><span class="sxs-lookup"><span data-stu-id="60a07-256">When moving an HDInsight cluster tooa new subscription, first move other resources (like hello storage account).</span></span> <span data-ttu-id="60a07-257">Pak přesuňte clusteru HDInsight hello samostatně.</span><span class="sxs-lookup"><span data-stu-id="60a07-257">Then, move hello HDInsight cluster by itself.</span></span>

## <a name="classic-deployment-limitations"></a><span data-ttu-id="60a07-258">Omezení nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="60a07-258">Classic deployment limitations</span></span>
<span data-ttu-id="60a07-259">Hello možnosti pro přesun prostředků nasazené pomocí klasického modelu hello se liší v závislosti na tom, jestli se přesun prostředků hello v rámci předplatného nebo tooa nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="60a07-259">hello options for moving resources deployed through hello classic model differ based on whether you are moving hello resources within a subscription or tooa new subscription.</span></span>

### <a name="same-subscription"></a><span data-ttu-id="60a07-260">Stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="60a07-260">Same subscription</span></span>
<span data-ttu-id="60a07-261">Při přesunu prostředků z jedné skupiny tooanother prostředků skupiny prostředků v rámci hello použít stejného předplatného, hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="60a07-261">When moving resources from one resource group tooanother resource group within hello same subscription, hello following restrictions apply:</span></span>

* <span data-ttu-id="60a07-262">Nelze přesunout virtuální sítě (klasické).</span><span class="sxs-lookup"><span data-stu-id="60a07-262">Virtual networks (classic) cannot be moved.</span></span>
* <span data-ttu-id="60a07-263">Virtuální počítače (klasické) je třeba přesunout s cloudovou službou hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-263">Virtual machines (classic) must be moved with hello cloud service.</span></span>
* <span data-ttu-id="60a07-264">Cloudové služby se dají přesunout pouze při přesunutí hello zahrnuje všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="60a07-264">Cloud service can only be moved when hello move includes all its virtual machines.</span></span>
* <span data-ttu-id="60a07-265">Současně lze přesunout jenom jeden cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="60a07-265">Only one cloud service can be moved at a time.</span></span>
* <span data-ttu-id="60a07-266">Současně lze přesouvat pouze jeden účet úložiště (klasické).</span><span class="sxs-lookup"><span data-stu-id="60a07-266">Only one storage account (classic) can be moved at a time.</span></span>
* <span data-ttu-id="60a07-267">Účet úložiště (klasické) nelze přesunout v hello stejné operace virtuálního počítače nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="60a07-267">Storage account (classic) cannot be moved in hello same operation with a virtual machine or a cloud service.</span></span>

<span data-ttu-id="60a07-268">toomove klasické prostředky tooa novou skupinu prostředků v hello stejného předplatného, použijte standardní Přesunutí operací hello prostřednictvím hello [portál](#use-portal), [prostředí Azure PowerShell](#use-powershell), [rozhraní příkazového řádku Azure](#use-azure-cli), nebo [rozhraní REST API](#use-rest-api).</span><span class="sxs-lookup"><span data-stu-id="60a07-268">toomove classic resources tooa new resource group within hello same subscription, use hello standard move operations through hello [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), or [REST API](#use-rest-api).</span></span> <span data-ttu-id="60a07-269">Používáte hello stejné operace jako použijte pro přesun prostředků Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="60a07-269">You use hello same operations as you use for moving Resource Manager resources.</span></span>

### <a name="new-subscription"></a><span data-ttu-id="60a07-270">Nové předplatné</span><span class="sxs-lookup"><span data-stu-id="60a07-270">New subscription</span></span>
<span data-ttu-id="60a07-271">Při přesunu prostředků tooa nové předplatné, platí následující omezení hello:</span><span class="sxs-lookup"><span data-stu-id="60a07-271">When moving resources tooa new subscription, hello following restrictions apply:</span></span>

* <span data-ttu-id="60a07-272">Všechny klasické prostředky v předplatném hello je třeba přesunout v hello stejné operace.</span><span class="sxs-lookup"><span data-stu-id="60a07-272">All classic resources in hello subscription must be moved in hello same operation.</span></span>
* <span data-ttu-id="60a07-273">cílové předplatné Hello nesmí obsahovat všechny klasické prostředky.</span><span class="sxs-lookup"><span data-stu-id="60a07-273">hello target subscription must not contain any other classic resources.</span></span>
* <span data-ttu-id="60a07-274">Přesunutí Hello mohou být požadována pouze přes samostatné REST API pro přesun classic.</span><span class="sxs-lookup"><span data-stu-id="60a07-274">hello move can only be requested through a separate REST API for classic moves.</span></span> <span data-ttu-id="60a07-275">Hello standardní Resource Manager přesunutí příkazy nefungují při přesunu klasické prostředky tooa nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="60a07-275">hello standard Resource Manager move commands do not work when moving classic resources tooa new subscription.</span></span>

<span data-ttu-id="60a07-276">toomove klasické prostředky tooa nové předplatné, použijte hello REST operace, které jsou specifické tooclassic prostředky.</span><span class="sxs-lookup"><span data-stu-id="60a07-276">toomove classic resources tooa new subscription, use hello REST operations that are specific tooclassic resources.</span></span> <span data-ttu-id="60a07-277">toouse REST, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="60a07-277">toouse REST, perform hello following steps:</span></span>

1. <span data-ttu-id="60a07-278">Kontrola, pokud hello zdrojové předplatné mohl účastnit mezi předplatnými přesunout.</span><span class="sxs-lookup"><span data-stu-id="60a07-278">Check if hello source subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="60a07-279">Použijte hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="60a07-279">Use hello following operation:</span></span>

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="60a07-280">V textu žádosti hello patří:</span><span class="sxs-lookup"><span data-stu-id="60a07-280">In hello request body, include:</span></span>

  ```json
  {
    "role": "source"
  }
  ```

     <span data-ttu-id="60a07-281">Hello odpověď pro operace ověření hello je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="60a07-281">hello response for hello validation operation is in hello following format:</span></span>

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. <span data-ttu-id="60a07-282">Kontrola, pokud hello cílového odběru mohou být součástí mezi předplatnými přesunout.</span><span class="sxs-lookup"><span data-stu-id="60a07-282">Check if hello destination subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="60a07-283">Použijte hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="60a07-283">Use hello following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="60a07-284">V textu žádosti hello patří:</span><span class="sxs-lookup"><span data-stu-id="60a07-284">In hello request body, include:</span></span>

  ```json
  {
    "role": "target"
  }
  ```

     <span data-ttu-id="60a07-285">odpověď Hello je v hello stejný formát jako hello zdrojové předplatné ověření.</span><span class="sxs-lookup"><span data-stu-id="60a07-285">hello response is in hello same format as hello source subscription validation.</span></span>
3. <span data-ttu-id="60a07-286">Pokud oba odběry projít ověřením, přesune všechny klasické prostředky z předplatného tooanother jedno předplatné s hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="60a07-286">If both subscriptions pass validation, move all classic resources from one subscription tooanother subscription with hello following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    <span data-ttu-id="60a07-287">V textu žádosti hello patří:</span><span class="sxs-lookup"><span data-stu-id="60a07-287">In hello request body, include:</span></span>

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

<span data-ttu-id="60a07-288">operace Hello může běžet několik minut.</span><span class="sxs-lookup"><span data-stu-id="60a07-288">hello operation may run for several minutes.</span></span>

## <a name="use-portal"></a><span data-ttu-id="60a07-289">Použít portál</span><span class="sxs-lookup"><span data-stu-id="60a07-289">Use portal</span></span>
<span data-ttu-id="60a07-290">toomove prostředky, vyberte skupinu prostředků hello obsahující tyto prostředky a potom vyberte hello **přesunout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60a07-290">toomove resources, select hello resource group containing those resources, and then select hello **Move** button.</span></span>

![Přesunutí prostředků](./media/resource-group-move-resources/select-move.png)

<span data-ttu-id="60a07-292">Vyberte, zda jsou přesunutí hello prostředky tooa novou skupinu prostředků nebo nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="60a07-292">Select whether you are moving hello resources tooa new resource group or a new subscription.</span></span>

<span data-ttu-id="60a07-293">Vyberte toomove hello prostředků a skupina prostředků cílového hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-293">Select hello resources toomove and hello destination resource group.</span></span> <span data-ttu-id="60a07-294">Na vědomí, že potřebujete tooupdate skripty pro tyto prostředky a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="60a07-294">Acknowledge that you need tooupdate scripts for these resources and select **OK**.</span></span> <span data-ttu-id="60a07-295">Pokud předplatné ikonu pro úpravu hello jste vybrali v předchozím kroku hello, je nutné vybrat také hello cílového předplatného.</span><span class="sxs-lookup"><span data-stu-id="60a07-295">If you selected hello edit subscription icon in hello previous step, you must also select hello destination subscription.</span></span>

![Vyberte cíl](./media/resource-group-move-resources/select-destination.png)

<span data-ttu-id="60a07-297">V **oznámení**, uvidíte, že hello přesunout probíhá operace.</span><span class="sxs-lookup"><span data-stu-id="60a07-297">In **Notifications**, you see that hello move operation is running.</span></span>

![Zobrazit stav přesunutí](./media/resource-group-move-resources/show-status.png)

<span data-ttu-id="60a07-299">Pokud ho dokončit, zobrazí se zpráva výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-299">When it has completed, you are notified of hello result.</span></span>

![Zobrazit přesunout výsledků](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a><span data-ttu-id="60a07-301">Použití prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="60a07-301">Use PowerShell</span></span>
<span data-ttu-id="60a07-302">toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello `Move-AzureRmResource` příkaz.</span><span class="sxs-lookup"><span data-stu-id="60a07-302">toomove existing resources tooanother resource group or subscription, use hello `Move-AzureRmResource` command.</span></span>

<span data-ttu-id="60a07-303">Hello první příklad ukazuje způsob toomove jeden prostředek tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-303">hello first example shows how toomove one resource tooa new resource group.</span></span>

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

<span data-ttu-id="60a07-304">Hello sekundu příklad ukazuje, jak toomove více prostředků tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-304">hello second example shows how toomove multiple resources tooa new resource group.</span></span>

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

<span data-ttu-id="60a07-305">toomove tooa nové předplatné, vložte hodnotu pro hello `DestinationSubscriptionId` parametr.</span><span class="sxs-lookup"><span data-stu-id="60a07-305">toomove tooa new subscription, include a value for hello `DestinationSubscriptionId` parameter.</span></span>

<span data-ttu-id="60a07-306">Jste vyzváni, které chcete toomove hello tooconfirm zadané prostředky.</span><span class="sxs-lookup"><span data-stu-id="60a07-306">You are asked tooconfirm that you want toomove hello specified resources.</span></span>

```powershell
Confirm
Are you sure you want toomove these resources toohello resource group
'/subscriptions/{guid}/resourceGroups/newRG' hello resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a><span data-ttu-id="60a07-307">Použití Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="60a07-307">Use Azure CLI 2.0</span></span>
<span data-ttu-id="60a07-308">toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello `az resource move` příkaz.</span><span class="sxs-lookup"><span data-stu-id="60a07-308">toomove existing resources tooanother resource group or subscription, use hello `az resource move` command.</span></span> <span data-ttu-id="60a07-309">Zadejte hello prostředků ID toomove prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-309">Provide hello resource IDs of hello resources toomove.</span></span> <span data-ttu-id="60a07-310">ID prostředku můžete získat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="60a07-310">You can get resource IDs with hello following command:</span></span>

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

<span data-ttu-id="60a07-311">Hello následující příklad ukazuje, jak toomove úložiště účet tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-311">hello following example shows how toomove a storage account tooa new resource group.</span></span> <span data-ttu-id="60a07-312">V hello `--ids` parametr, poskytovat seznam toomove ID prostředku hello oddělených mezerami.</span><span class="sxs-lookup"><span data-stu-id="60a07-312">In hello `--ids` parameter, provide a space-separated list of hello resource IDs toomove.</span></span>

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

<span data-ttu-id="60a07-313">toomove tooa nové předplatné, zadejte hello `--destination-subscription-id` parametr.</span><span class="sxs-lookup"><span data-stu-id="60a07-313">toomove tooa new subscription, provide hello `--destination-subscription-id` parameter.</span></span>

## <a name="use-azure-cli-10"></a><span data-ttu-id="60a07-314">Použití Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="60a07-314">Use Azure CLI 1.0</span></span>
<span data-ttu-id="60a07-315">toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello `azure resource move` příkaz.</span><span class="sxs-lookup"><span data-stu-id="60a07-315">toomove existing resources tooanother resource group or subscription, use hello `azure resource move` command.</span></span> <span data-ttu-id="60a07-316">Zadejte hello prostředků ID toomove prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-316">Provide hello resource IDs of hello resources toomove.</span></span> <span data-ttu-id="60a07-317">ID prostředku můžete získat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="60a07-317">You can get resource IDs with hello following command:</span></span>

```azurecli
azure resource list -g sourceGroup --json
```

<span data-ttu-id="60a07-318">Která vrací hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="60a07-318">Which returns hello following format:</span></span>

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

<span data-ttu-id="60a07-319">Hello následující příklad ukazuje, jak toomove úložiště účet tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a07-319">hello following example shows how toomove a storage account tooa new resource group.</span></span> <span data-ttu-id="60a07-320">V hello `-i` parametr, poskytovat seznam toomove ID prostředku hello oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="60a07-320">In hello `-i` parameter, provide a comma-separated list of hello resource IDs toomove.</span></span>

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

<span data-ttu-id="60a07-321">Jste vyzváni, které chcete toomove hello tooconfirm zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="60a07-321">You are asked tooconfirm that you want toomove hello specified resource.</span></span>

## <a name="use-rest-api"></a><span data-ttu-id="60a07-322">Použití rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="60a07-322">Use REST API</span></span>
<span data-ttu-id="60a07-323">toomove existující prostředky tooanother skupinu prostředků nebo předplatného, spusťte:</span><span class="sxs-lookup"><span data-stu-id="60a07-323">toomove existing resources tooanother resource group or subscription, run:</span></span>

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

<span data-ttu-id="60a07-324">V textu žádosti hello zadejte hello cílová skupina prostředků a prostředky toomove hello.</span><span class="sxs-lookup"><span data-stu-id="60a07-324">In hello request body, you specify hello target resource group and hello resources toomove.</span></span> <span data-ttu-id="60a07-325">Další informace o operaci REST přesunutí hello najdete v tématu [přesunout prostředky](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span><span class="sxs-lookup"><span data-stu-id="60a07-325">For more information about hello move REST operation, see [Move resources](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="60a07-326">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60a07-326">Next steps</span></span>
* <span data-ttu-id="60a07-327">toolearn o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="60a07-327">toolearn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="60a07-328">toolearn o rozhraní příkazového řádku Azure pro správu předplatného, najdete v části [hello pomocí rozhraní příkazového řádku Azure s Resource Managerem](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="60a07-328">toolearn about Azure CLI commands for managing your subscription, see [Using hello Azure CLI with Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="60a07-329">toolearn o funkcích portálu pro správu předplatného, najdete v části [pomocí prostředků Azure portálu toomanage hello](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="60a07-329">toolearn about portal features for managing your subscription, see [Using hello Azure portal toomanage resources](resource-group-portal.md).</span></span>
* <span data-ttu-id="60a07-330">toolearn o použití tooyour prostředky logické organizaci, najdete v části [pomocí značky tooorganize vaše prostředky](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="60a07-330">toolearn about applying a logical organization tooyour resources, see [Using tags tooorganize your resources](resource-group-using-tags.md).</span></span>
