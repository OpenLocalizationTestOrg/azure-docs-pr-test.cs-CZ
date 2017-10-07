---
title: "aaaCannot odstranění virtuální sítě v Azure | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot hello problém, ve kterém nelze odstranit virtuální sítě v Azure."
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
ms.openlocfilehash: a9050ab238ccb0380fd46130430222efb8f42388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-failed-toodelete-a-virtual-network-in-azure"></a><span data-ttu-id="66558-103">Řešení potíží: Toodelete virtuální sítě v Azure se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="66558-103">Troubleshooting: Failed toodelete a virtual network in Azure</span></span>

<span data-ttu-id="66558-104">Při pokusu o toodelete virtuální sítě v Microsoft Azure se může zobrazit chyby.</span><span class="sxs-lookup"><span data-stu-id="66558-104">You might receive errors when you try toodelete a virtual network in Microsoft Azure.</span></span> <span data-ttu-id="66558-105">Tento článek obsahuje řešení problémů s kroky toohelp vyřešíte tento problém.</span><span class="sxs-lookup"><span data-stu-id="66558-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a><span data-ttu-id="66558-106">Pokyny při řešení potíží</span><span class="sxs-lookup"><span data-stu-id="66558-106">Troubleshooting guidance</span></span> 

1. <span data-ttu-id="66558-107">[Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti hello](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="66558-107">[Check whether a virtual network gateway is running in hello virtual network](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span></span>
2. <span data-ttu-id="66558-108">[Zkontrolujte, zda služby application gateway běží ve virtuální síti hello](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="66558-108">[Check whether an application gateway is running in hello virtual network](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span></span>
3. <span data-ttu-id="66558-109">[Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti hello](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="66558-109">[Check whether Azure Active Directory Domain Service is enabled in hello virtual network](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span></span>
4. <span data-ttu-id="66558-110">[Zkontrolujte, zda hello virtuální sítě je připojený tooother prostředků](#check-whether-the-virtual-network-is-connected-to-other-resource).</span><span class="sxs-lookup"><span data-stu-id="66558-110">[Check whether hello virtual network is connected tooother resource](#check-whether-the-virtual-network-is-connected-to-other-resource).</span></span>
5. <span data-ttu-id="66558-111">[Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti hello](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="66558-111">[Check whether a virtual machine is still running in hello virtual network](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span></span>
6. <span data-ttu-id="66558-112">[Zkontrolujte, zda text hello virtuální sítě se zasekla v automatickém migrace](#check-whether-the-virtual-network-is-stuck-in-migration).</span><span class="sxs-lookup"><span data-stu-id="66558-112">[Check whether hello virtual network is stuck in migration](#check-whether-the-virtual-network-is-stuck-in-migration).</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="66558-113">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="66558-113">Troubleshooting steps</span></span>

### <a name="check-whether-a-virtual-network-gateway-is-running-in-hello-virtual-network"></a><span data-ttu-id="66558-114">Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti hello</span><span class="sxs-lookup"><span data-stu-id="66558-114">Check whether a virtual network gateway is running in hello virtual network</span></span>

<span data-ttu-id="66558-115">tooremove hello virtuální síť, musíte nejprve odebrat hello brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-115">tooremove hello virtual network, you must first remove hello virtual network gateway.</span></span>

<span data-ttu-id="66558-116">Pro klasické virtuální sítě přejděte toohello **přehled** stránku hello v hello portál Azure classic virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-116">For classic virtual networks, go toohello **Overview** page of hello classic virtual network in hello Azure portal.</span></span> <span data-ttu-id="66558-117">V hello **připojení k síti VPN** část, pokud hello brána je spuštěná ve virtuální síti hello, zobrazí se hello IP adresu brány hello.</span><span class="sxs-lookup"><span data-stu-id="66558-117">In hello **VPN connections** section, if hello gateway is running in hello virtual network, you will see hello IP address of hello gateway.</span></span> 

![Zkontrolujte, jestli je brána spuštěná.](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

<span data-ttu-id="66558-119">Pro virtuální sítě přejděte toohello **přehled** stránku hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-119">For virtual networks, go toohello **Overview** page of hello virtual network.</span></span> <span data-ttu-id="66558-120">Zkontrolujte **připojená zařízení** pro bránu virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="66558-120">Check **Connected devices** for hello virtual network gateway.</span></span>

![Zkontrolujte hello připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

<span data-ttu-id="66558-122">Před odebráním hello bránu, nejprve odeberte **připojení** objekty v hello brány.</span><span class="sxs-lookup"><span data-stu-id="66558-122">Before you can remove hello gateway, first remove any **Connection** objects in hello gateway.</span></span> 

### <a name="check-whether-an-application-gateway-is-running-in-hello-virtual-network"></a><span data-ttu-id="66558-123">Zkontrolujte, zda je ve virtuální síti hello spuštěn aplikační brány</span><span class="sxs-lookup"><span data-stu-id="66558-123">Check whether an application gateway is running in hello virtual network</span></span>

<span data-ttu-id="66558-124">Přejděte toohello **přehled** stránku hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-124">Go toohello **Overview** page of hello virtual network.</span></span> <span data-ttu-id="66558-125">Zkontrolujte hello **připojená zařízení** hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="66558-125">Check hello **Connected devices** for hello application gateway.</span></span>

![Zkontrolujte hello připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

<span data-ttu-id="66558-127">Pokud je služby application gateway, musí odebrat před odstraněním hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-127">If there is an application gateway, you must remove it before you can delete hello virtual network.</span></span>

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-hello-virtual-network"></a><span data-ttu-id="66558-128">Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti hello</span><span class="sxs-lookup"><span data-stu-id="66558-128">Check whether Azure Active Directory Domain Service is enabled in hello virtual network</span></span>

<span data-ttu-id="66558-129">Pokud hello služba Active Directory Domain Services je toohello povolené a připojené virtuální sítě, nelze odstranit tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="66558-129">If hello Active Directory Domain Service is enabled and connected toohello virtual network, you cannot delete this virtual network.</span></span> 

![Zkontrolujte hello připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

<span data-ttu-id="66558-131">toodisable hello služby, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="66558-131">toodisable hello service, follow these steps:</span></span>

1. <span data-ttu-id="66558-132">Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="66558-132">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="66558-133">V levém podokně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="66558-133">In hello left pane, select  **Active Directory**.</span></span>
3. <span data-ttu-id="66558-134">Vyberte adresář hello Azure Active Directory (Azure AD), který má služba Active Directory Domain Services povoleno.</span><span class="sxs-lookup"><span data-stu-id="66558-134">Select hello Azure Active Directory (Azure AD) directory that has Active Directory Domain Service enabled.</span></span>
4. <span data-ttu-id="66558-135">Vyberte hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="66558-135">Select hello **Configure** tab.</span></span>
5. <span data-ttu-id="66558-136">V části **služby domény**, změňte hello **povolit doménové služby pro tento adresář** možnost příliš**ne**.</span><span class="sxs-lookup"><span data-stu-id="66558-136">Under **domain services**, change hello **Enable domain services for this directory** option too**No**.</span></span>  

### <a name="check-whether-hello-virtual-network-is-connected-tooother-resource"></a><span data-ttu-id="66558-137">Zkontrolujte, zda hello virtuální sítě je připojený tooother prostředků</span><span class="sxs-lookup"><span data-stu-id="66558-137">Check whether hello virtual network is connected tooother resource</span></span>

<span data-ttu-id="66558-138">Zkontrolujte propojení okruhu, připojení a partnerských vztahů virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-138">Check for Circuit Links, connections, and virtual network peerings.</span></span> <span data-ttu-id="66558-139">Některý z těchto může způsobit odstranění toofail virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-139">Any of these can cause a virtual network deletion toofail.</span></span> 

<span data-ttu-id="66558-140">Hello doporučené odstranění pořadí je následující:</span><span class="sxs-lookup"><span data-stu-id="66558-140">hello recommended deletion order is as follows:</span></span>

1. <span data-ttu-id="66558-141">Připojení brány</span><span class="sxs-lookup"><span data-stu-id="66558-141">Gateway connections</span></span>
2. <span data-ttu-id="66558-142">Brány</span><span class="sxs-lookup"><span data-stu-id="66558-142">Gateways</span></span>
3. <span data-ttu-id="66558-143">IP adresy</span><span class="sxs-lookup"><span data-stu-id="66558-143">IPs</span></span>
4. <span data-ttu-id="66558-144">Partnerské vztahy virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="66558-144">Virtual network peerings</span></span>
5. <span data-ttu-id="66558-145">Služba App Service Environment (App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="66558-145">App Service Environment (ASE)</span></span>

### <a name="check-whether-a-virtual-machine-is-still-running-in-hello-virtual-network"></a><span data-ttu-id="66558-146">Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti hello</span><span class="sxs-lookup"><span data-stu-id="66558-146">Check whether a virtual machine is still running in hello virtual network</span></span>

<span data-ttu-id="66558-147">Ujistěte se, že žádný virtuální počítač je ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="66558-147">Make sure that no virtual machine is in hello virtual network.</span></span>

### <a name="check-whether-hello-virtual-network-is-stuck-in-migration"></a><span data-ttu-id="66558-148">Zkontrolujte, zda text hello virtuální sítě se zasekla v automatickém migrace</span><span class="sxs-lookup"><span data-stu-id="66558-148">Check whether hello virtual network is stuck in migration</span></span>

<span data-ttu-id="66558-149">Pokud hello virtuální sítě se zasekla v automatickém migrace stavu, nelze jej odstranit.</span><span class="sxs-lookup"><span data-stu-id="66558-149">If hello virtual network is stuck in a migration state, it cannot be deleted.</span></span> <span data-ttu-id="66558-150">Spusťte následující příkaz tooabort hello migrace hello a pak odstraňte hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66558-150">Run hello following command tooabort hello migration, and then delete hello virtual network.</span></span>

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a><span data-ttu-id="66558-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66558-151">Next steps</span></span>

- [<span data-ttu-id="66558-152">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="66558-152">Azure Virtual Network</span></span>](virtual-networks-overview.md)
- [<span data-ttu-id="66558-153">Nejčastější dotazy ke službě Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="66558-153">Azure Virtual Network frequently asked questions (FAQ)</span></span>](virtual-networks-faq.md)