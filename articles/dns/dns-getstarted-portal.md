---
title: "Začínáme s DNS Azure pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Naučíte se vytvořit zónu a záznam DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat první zónu a záznam DNS pomocí webu Azure Portal."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 93b24e3d9fbb3fbb3ea995271fd63d1e82eb9c9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-the-azure-portal"></a><span data-ttu-id="b1fcd-104">Začínáme s DNS Azure pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b1fcd-104">Get started with Azure DNS using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b1fcd-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b1fcd-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="b1fcd-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b1fcd-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="b1fcd-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b1fcd-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="b1fcd-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b1fcd-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="b1fcd-109">Tento článek vás provede kroky k vytvoření první zóny a záznamu DNS pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-109">This article walks you through the steps to create your first DNS zone and record using the Azure portal.</span></span> <span data-ttu-id="b1fcd-110">Tyto kroky můžete provést také pomocí Azure PowerShellu nebo Azure CLI pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-110">You can also perform these steps using Azure PowerShell or the cross-platform Azure CLI.</span></span>

<span data-ttu-id="b1fcd-111">K hostování záznamů DNS v určité doméně se používá zóna DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-111">A DNS zone is used to host the DNS records for a particular domain.</span></span> <span data-ttu-id="b1fcd-112">Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-112">To start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name.</span></span> <span data-ttu-id="b1fcd-113">Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="b1fcd-114">Nakonec, pokud chcete zónu DNS publikovat na internetu, bude potřeba nakonfigurovat pro doménu názvové servery.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-114">Finally, to publish your DNS zone to the Internet, you need to configure the name servers for the domain.</span></span> <span data-ttu-id="b1fcd-115">Každý z těchto kroků je popsán v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-115">Each of these steps is described in the following steps.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="b1fcd-116">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="b1fcd-116">Create a DNS zone</span></span>

1. <span data-ttu-id="b1fcd-117">Přihlášení k webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b1fcd-117">Sign in to the Azure portal</span></span>
2. <span data-ttu-id="b1fcd-118">V nabídce centra klikněte na **Nový > Sítě >** a potom kliknutím na **Zóna DNS** otevřete okno Vytvořit zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-118">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![Zóna DNS](./media/dns-getstarted-portal/openzone650.png)

4. <span data-ttu-id="b1fcd-120">V okně **Vytvořit zónu DNS** zadejte následující hodnoty a pak klikněte na **Vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="b1fcd-120">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>


   | <span data-ttu-id="b1fcd-121">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-121">**Setting**</span></span> | <span data-ttu-id="b1fcd-122">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-122">**Value**</span></span> | <span data-ttu-id="b1fcd-123">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-123">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="b1fcd-124">**Název**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-124">**Name**</span></span>|<span data-ttu-id="b1fcd-125">contoso.com</span><span class="sxs-lookup"><span data-stu-id="b1fcd-125">contoso.com</span></span>|<span data-ttu-id="b1fcd-126">Název zóny DNS</span><span class="sxs-lookup"><span data-stu-id="b1fcd-126">The name of the DNS zone</span></span>|
   |<span data-ttu-id="b1fcd-127">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-127">**Subscription**</span></span>|<span data-ttu-id="b1fcd-128">[Vaše předplatné]</span><span class="sxs-lookup"><span data-stu-id="b1fcd-128">[Your subscription]</span></span>|<span data-ttu-id="b1fcd-129">Vyberte předplatné, ve kterém chcete vytvořit zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-129">Select a subscription to create the DNS zone in.</span></span>|
   |<span data-ttu-id="b1fcd-130">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-130">**Resource group**</span></span>|<span data-ttu-id="b1fcd-131">**Vytvořit novou:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="b1fcd-131">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="b1fcd-132">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-132">Create a resource group.</span></span> <span data-ttu-id="b1fcd-133">Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-133">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="b1fcd-134">Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="b1fcd-134">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="b1fcd-135">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-135">**Location**</span></span>|<span data-ttu-id="b1fcd-136">Západní USA</span><span class="sxs-lookup"><span data-stu-id="b1fcd-136">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="b1fcd-137">Skupina prostředků označuje umístění skupiny prostředků a nemá žádný vliv na zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-137">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="b1fcd-138">Umístění zóny DNS je vždy globální a není zobrazeno.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-138">The DNS zone location is always "global", and is not shown.</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="b1fcd-139">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="b1fcd-139">Create a DNS record</span></span>

<span data-ttu-id="b1fcd-140">Následující příklad vás provede procesem vytvoření nového záznamu A.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-140">The following example walks you through the process of creating new 'A' record.</span></span> <span data-ttu-id="b1fcd-141">Informace o dalších typech záznamů a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b1fcd-141">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span> 

1. <span data-ttu-id="b1fcd-142">Když máte vytvořenou zónu DNS, na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-142">With the DNS Zone created, in the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="b1fcd-143">V okně Všechny prostředky klikněte na zónu DNS **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-143">Click the **contoso.com** DNS zone in the All resources blade.</span></span> <span data-ttu-id="b1fcd-144">Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat **contoso.com** do pole **Filtrovat podle názvu...**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-144">If the subscription you selected already has several resources in it, you can enter **contoso.com** in the **Filter by name…**</span></span> <span data-ttu-id="b1fcd-145">pro snadný přístup k zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-145">box to easily access the DNS Zone.</span></span>

1. <span data-ttu-id="b1fcd-146">V horní části okna **Zóna DNS** vyberte **Sada záznamů**. Otevře se okno **Přidat sadu záznamů**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-146">At the top of the **DNS zone** blade, select **+ Record set** to open the **Add record set** blade.</span></span>

1. <span data-ttu-id="b1fcd-147">V okně **Přidat sadu záznamů** zadejte následující hodnoty a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-147">On the **Add record set** blade, enter the following values, and click **OK**.</span></span> <span data-ttu-id="b1fcd-148">V tomto příkladu vytváříte záznam A.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-148">In this example, you are creating an A record.</span></span>

   |<span data-ttu-id="b1fcd-149">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-149">**Setting**</span></span> | <span data-ttu-id="b1fcd-150">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-150">**Value**</span></span> | <span data-ttu-id="b1fcd-151">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-151">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="b1fcd-152">**Název**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-152">**Name**</span></span>|<span data-ttu-id="b1fcd-153">www</span><span class="sxs-lookup"><span data-stu-id="b1fcd-153">www</span></span>|<span data-ttu-id="b1fcd-154">Název záznamu</span><span class="sxs-lookup"><span data-stu-id="b1fcd-154">Name of the record</span></span>|
   |<span data-ttu-id="b1fcd-155">**Typ**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-155">**Type**</span></span>|<span data-ttu-id="b1fcd-156">A</span><span class="sxs-lookup"><span data-stu-id="b1fcd-156">A</span></span>| <span data-ttu-id="b1fcd-157">Typ záznamu DNS, který se má vytvořit. Přípustné hodnoty jsou A, AAAA, CNAME, MX, NS, SRV, TXT, a PTR.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-157">Type of DNS record to create, acceptable values are A, AAAA, CNAME, MX, NS, SRV, TXT, and PTR.</span></span>  <span data-ttu-id="b1fcd-158">Další informace o typech záznamů najdete v tématu [Přehled záznamů a zón DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="b1fcd-158">For more information about record types, visit [Overview of DNS zones and records](dns-zones-records.md)</span></span>|
   |<span data-ttu-id="b1fcd-159">**Hodnota TTL**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-159">**TTL**</span></span>|<span data-ttu-id="b1fcd-160">1</span><span class="sxs-lookup"><span data-stu-id="b1fcd-160">1</span></span>|<span data-ttu-id="b1fcd-161">Hodnota TTL (Time to Live) žádosti DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-161">Time-to-live of the DNS request.</span></span>|
   |<span data-ttu-id="b1fcd-162">**Jednotka hodnoty TTL**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-162">**TTL unit**</span></span>|<span data-ttu-id="b1fcd-163">Hodiny</span><span class="sxs-lookup"><span data-stu-id="b1fcd-163">Hours</span></span>|<span data-ttu-id="b1fcd-164">Měřené jednotky času pro hodnotu TTL.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-164">Measurement of time for TTL value.</span></span>|
   |<span data-ttu-id="b1fcd-165">**IP adresa**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-165">**IP address**</span></span>|<span data-ttu-id="b1fcd-166">ipAddressValue</span><span class="sxs-lookup"><span data-stu-id="b1fcd-166">ipAddressValue</span></span>| <span data-ttu-id="b1fcd-167">Tato hodnota je IP adresa, kterou záznam DNS překládá.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-167">This value is the IP address that the DNS record resolves.</span></span>|

## <a name="view-records"></a><span data-ttu-id="b1fcd-168">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="b1fcd-168">View records</span></span>

<span data-ttu-id="b1fcd-169">V dolní části okna zóny DNS se zobrazí záznamy pro zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-169">In the lower part of the DNS zone blade, you can see the records for the DNS zone.</span></span> <span data-ttu-id="b1fcd-170">Měli byste vidět výchozí záznamy DNS a SOA, které se vytvoří v každé zóně, a nové záznamy, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-170">You should see the default DNS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

![zóna](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a><span data-ttu-id="b1fcd-172">Aktualizace názvových serverů</span><span class="sxs-lookup"><span data-stu-id="b1fcd-172">Update name servers</span></span>

<span data-ttu-id="b1fcd-173">Jakmile budete spokojeni se správným nastavením zóny a záznamů DNS, bude potřeba nakonfigurovat váš název domény tak, aby používal názvové servery DNS Azure.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-173">Once you are satisfied that your DNS zone and records have been set up correctly, you need to configure your domain name to use the Azure DNS name servers.</span></span> <span data-ttu-id="b1fcd-174">Tím umožníte ostatním uživatelům na internetu najít vaše záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-174">This enables other users on the Internet to find your DNS records.</span></span>

<span data-ttu-id="b1fcd-175">Názvové servery vaší zóny můžete zobrazit na webu Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="b1fcd-175">The name servers for your zone are given in the Azure portal:</span></span>

![zóna](./media/dns-getstarted-portal/viewzonens500.png)

<span data-ttu-id="b1fcd-177">Tyto názvové servery by měly být nakonfigurované u registrátora názvu domény (u kterého jste zakoupili název domény).</span><span class="sxs-lookup"><span data-stu-id="b1fcd-177">These name servers should be configured with the domain name registrar (where you purchased the domain name).</span></span> <span data-ttu-id="b1fcd-178">Registrátor nabízí možnost nastavit názvové servery pro doménu.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-178">Your registrar offers the option to set up the name servers for the domain.</span></span> <span data-ttu-id="b1fcd-179">Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="b1fcd-179">For more information, see [Delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="b1fcd-180">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="b1fcd-180">Delete all resources</span></span>

<span data-ttu-id="b1fcd-181">Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1fcd-181">To delete all resources created in this article, complete the following steps:</span></span>

1. <span data-ttu-id="b1fcd-182">Na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-182">In the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="b1fcd-183">V okně Všechny prostředky klikněte na skupinu prostředků **MyResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-183">Click the **MyResourceGroup** resource group in the All resources blade.</span></span> <span data-ttu-id="b1fcd-184">Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat **MyResourceGroup** do pole **Filtrovat podle názvu...**</span><span class="sxs-lookup"><span data-stu-id="b1fcd-184">If the subscription you selected already has several resources in it, you can enter **MyResourceGroup** in the **Filter by name…**</span></span> <span data-ttu-id="b1fcd-185">pro snadný přístup ke skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-185">box to easily access the resource group.</span></span>
1. <span data-ttu-id="b1fcd-186">V okně **MyResourceGroup** klikněte na tlačítko **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-186">In the **MyResourceGroup** blade, click the **Delete** button.</span></span>
1. <span data-ttu-id="b1fcd-187">Portál požaduje, abyste zadali název skupiny prostředků pro potvrzení, že ji skutečně chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-187">The portal requires you to type the name of the resource group to confirm that you want to delete it.</span></span> <span data-ttu-id="b1fcd-188">Klikněte na **Odstranit**, jako název skupiny prostředků zadejte *MyResourceGroup*, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-188">Click **Delete**, Type *MyResourceGroup* for the resource group name, then click **Delete**.</span></span> <span data-ttu-id="b1fcd-189">Odstraněním skupiny prostředků se odstraní všechny prostředky v rámci dané skupiny prostředků. Proto nikdy nezapomeňte před odstraněním skupiny prostředků zkontrolovat její obsah.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-189">Deleting a resource group deletes all resources within the resource group, so always be sure to confirm the contents of a resource group before deleting it.</span></span> <span data-ttu-id="b1fcd-190">Portál odstraní všechny prostředky v rámci skupiny prostředků a potom odstraní samotnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-190">The portal deletes all resources contained within the resource group, then deletes the resource group itself.</span></span> <span data-ttu-id="b1fcd-191">Tento proces trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="b1fcd-191">This process takes several minutes.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b1fcd-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1fcd-192">Next steps</span></span>

<span data-ttu-id="b1fcd-193">Další informace o DNS Azure najdete v tématu [Přehled DNS Azure](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b1fcd-193">To learn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="b1fcd-194">Další informace o správě záznamů DNS v DNS Azure najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b1fcd-194">To learn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

