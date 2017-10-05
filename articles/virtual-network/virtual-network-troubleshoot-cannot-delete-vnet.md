---
title: "Nelze odstranit virtuální sítě v Azure | Microsoft Docs"
description: "Zjistěte, jak vyřešit problém, ve kterém nelze odstranit virtuální sítě v Azure."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: genli
ms.openlocfilehash: 55c42a91bb1c5fad289b975ffae8ce4d6e7343dd
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a><span data-ttu-id="b3aa8-103">Řešení potíží: Nepodařilo se odstranit virtuální sítě v Azure</span><span class="sxs-lookup"><span data-stu-id="b3aa8-103">Troubleshooting: Failed to delete a virtual network in Azure</span></span>

<span data-ttu-id="b3aa8-104">Taky může docházet k chybám při pokusu o odstranění virtuální sítě v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-104">You might receive errors when you try to delete a virtual network in Microsoft Azure.</span></span> <span data-ttu-id="b3aa8-105">Tento článek obsahuje postup pro odstraňování potíží při řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-105">This article provides troubleshooting steps to help you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a><span data-ttu-id="b3aa8-106">Pokyny při řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b3aa8-106">Troubleshooting guidance</span></span> 

1. <span data-ttu-id="b3aa8-107">[Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="b3aa8-107">[Check whether a virtual network gateway is running in the virtual network](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span></span>
2. <span data-ttu-id="b3aa8-108">[Zkontrolujte, zda služby application gateway běží ve virtuální síti](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="b3aa8-108">[Check whether an application gateway is running in the virtual network](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span></span>
3. <span data-ttu-id="b3aa8-109">[Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="b3aa8-109">[Check whether Azure Active Directory Domain Service is enabled in the virtual network](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span></span>
4. <span data-ttu-id="b3aa8-110">[Zkontrolujte, zda je virtuální sítě připojený k jiný prostředek](#check-whether-the-virtual-network-is-connected-to-other-resource).</span><span class="sxs-lookup"><span data-stu-id="b3aa8-110">[Check whether the virtual network is connected to other resource](#check-whether-the-virtual-network-is-connected-to-other-resource).</span></span>
5. <span data-ttu-id="b3aa8-111">[Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="b3aa8-111">[Check whether a virtual machine is still running in the virtual network](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span></span>
6. <span data-ttu-id="b3aa8-112">[Zkontrolujte, zda virtuální sítě se zasekla v automatickém migrace](#check-whether-the-virtual-network-is-stuck-in-migration).</span><span class="sxs-lookup"><span data-stu-id="b3aa8-112">[Check whether the virtual network is stuck in migration](#check-whether-the-virtual-network-is-stuck-in-migration).</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="b3aa8-113">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b3aa8-113">Troubleshooting steps</span></span>

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a><span data-ttu-id="b3aa8-114">Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="b3aa8-114">Check whether a virtual network gateway is running in the virtual network</span></span>

<span data-ttu-id="b3aa8-115">Odebrat virtuální sítě, je nutné nejprve odebrat bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-115">To remove the virtual network, you must first remove the virtual network gateway.</span></span>

<span data-ttu-id="b3aa8-116">Klasické virtuální sítě, najdete **přehled** stránky klasické virtuální sítě na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-116">For classic virtual networks, go to the **Overview** page of the classic virtual network in the Azure portal.</span></span> <span data-ttu-id="b3aa8-117">V **připojení k síti VPN** část, pokud brána běží ve virtuální síti, zobrazí se na IP adresu brány.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-117">In the **VPN connections** section, if the gateway is running in the virtual network, you will see the IP address of the gateway.</span></span> 

![Zkontrolujte, jestli je brána spuštěná.](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

<span data-ttu-id="b3aa8-119">Pro virtuální sítě, přejděte na **přehled** stránky ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-119">For virtual networks, go to the **Overview** page of the virtual network.</span></span> <span data-ttu-id="b3aa8-120">Zkontrolujte **připojená zařízení** pro bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-120">Check **Connected devices** for the virtual network gateway.</span></span>

![Zkontrolujte připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

<span data-ttu-id="b3aa8-122">Před odebráním bránu, nejprve odeberte **připojení** objekty v bráně.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-122">Before you can remove the gateway, first remove any **Connection** objects in the gateway.</span></span> 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a><span data-ttu-id="b3aa8-123">Zkontrolujte, zda je ve virtuální síti spuštěn aplikační brány</span><span class="sxs-lookup"><span data-stu-id="b3aa8-123">Check whether an application gateway is running in the virtual network</span></span>

<span data-ttu-id="b3aa8-124">Přejděte na **přehled** stránky ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-124">Go to the **Overview** page of the virtual network.</span></span> <span data-ttu-id="b3aa8-125">Zkontrolujte **připojená zařízení** pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-125">Check the **Connected devices** for the application gateway.</span></span>

![Zkontrolujte připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

<span data-ttu-id="b3aa8-127">Pokud je služby application gateway, musí odebrat před odstraněním virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-127">If there is an application gateway, you must remove it before you can delete the virtual network.</span></span>

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a><span data-ttu-id="b3aa8-128">Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="b3aa8-128">Check whether Azure Active Directory Domain Service is enabled in the virtual network</span></span>

<span data-ttu-id="b3aa8-129">Pokud služba Active Directory Domain Services je povolen a připojen k virtuální síti, nelze odstranit tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-129">If the Active Directory Domain Service is enabled and connected to the virtual network, you cannot delete this virtual network.</span></span> 

![Zkontrolujte připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

<span data-ttu-id="b3aa8-131">Zakázat službu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b3aa8-131">To disable the service, follow these steps:</span></span>

1. <span data-ttu-id="b3aa8-132">Přejděte na [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b3aa8-132">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="b3aa8-133">V levém podokně vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-133">In the left pane, select  **Active Directory**.</span></span>
3. <span data-ttu-id="b3aa8-134">Vyberte adresář služby Azure Active Directory (Azure AD), který má služba Active Directory Domain Services povoleno.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-134">Select the Azure Active Directory (Azure AD) directory that has Active Directory Domain Service enabled.</span></span>
4. <span data-ttu-id="b3aa8-135">Vyberte kartu **Konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-135">Select the **Configure** tab.</span></span>
5. <span data-ttu-id="b3aa8-136">V části **služby domény**, změnit **povolit doménové služby pro tento adresář** možnost k **ne**.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-136">Under **domain services**, change the **Enable domain services for this directory** option to **No**.</span></span>  

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a><span data-ttu-id="b3aa8-137">Zkontrolujte, zda virtuální síť je připojená k jiné prostředku</span><span class="sxs-lookup"><span data-stu-id="b3aa8-137">Check whether the virtual network is connected to other resource</span></span>

<span data-ttu-id="b3aa8-138">Zkontrolujte propojení okruhu, připojení a partnerských vztahů virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-138">Check for Circuit Links, connections, and virtual network peerings.</span></span> <span data-ttu-id="b3aa8-139">Některý z těchto může způsobit odstranění virtuální sítě k selhání.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-139">Any of these can cause a virtual network deletion to fail.</span></span> 

<span data-ttu-id="b3aa8-140">Pořadí odstranění doporučené vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b3aa8-140">The recommended deletion order is as follows:</span></span>

1. <span data-ttu-id="b3aa8-141">Připojení brány</span><span class="sxs-lookup"><span data-stu-id="b3aa8-141">Gateway connections</span></span>
2. <span data-ttu-id="b3aa8-142">Brány</span><span class="sxs-lookup"><span data-stu-id="b3aa8-142">Gateways</span></span>
3. <span data-ttu-id="b3aa8-143">IP adresy</span><span class="sxs-lookup"><span data-stu-id="b3aa8-143">IPs</span></span>
4. <span data-ttu-id="b3aa8-144">Partnerské vztahy virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b3aa8-144">Virtual network peerings</span></span>
5. <span data-ttu-id="b3aa8-145">Služba App Service Environment (App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="b3aa8-145">App Service Environment (ASE)</span></span>

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a><span data-ttu-id="b3aa8-146">Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="b3aa8-146">Check whether a virtual machine is still running in the virtual network</span></span>

<span data-ttu-id="b3aa8-147">Ujistěte se, že žádný virtuální počítač je ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-147">Make sure that no virtual machine is in the virtual network.</span></span>

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a><span data-ttu-id="b3aa8-148">Zkontrolujte, zda virtuální sítě se zasekla v automatickém migrace</span><span class="sxs-lookup"><span data-stu-id="b3aa8-148">Check whether the virtual network is stuck in migration</span></span>

<span data-ttu-id="b3aa8-149">Pokud virtuální sítě se zasekla v automatickém migrace stavu, nelze jej odstranit.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-149">If the virtual network is stuck in a migration state, it cannot be deleted.</span></span> <span data-ttu-id="b3aa8-150">Spusťte následující příkaz k přerušení migrace a pak odstraňte virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b3aa8-150">Run the following command to abort the migration, and then delete the virtual network.</span></span>

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a><span data-ttu-id="b3aa8-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3aa8-151">Next steps</span></span>

- [<span data-ttu-id="b3aa8-152">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="b3aa8-152">Azure Virtual Network</span></span>](virtual-networks-overview.md)
- [<span data-ttu-id="b3aa8-153">Nejčastější dotazy ke službě Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="b3aa8-153">Azure Virtual Network frequently asked questions (FAQ)</span></span>](virtual-networks-faq.md)