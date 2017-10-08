---
title: "skupiny zabezpečení sítě (klasické) aaaCreate ve službě Azure - prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin Nsg v klasickém režimu pomocí prostředí PowerShell"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 86810b0d-0240-46a2-8548-fca22daa56f3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 835097c9f23cdd551f97797e142c6c2a3c978cd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-nsgs-classic-in-powershell"></a><span data-ttu-id="ac18a-103">Jak toocreate skupiny zabezpečení sítě (klasické) v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac18a-103">How toocreate NSGs (classic) in PowerShell</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="ac18a-104">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="ac18a-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="ac18a-105">Můžete také [vytvářet skupiny Nsg v modelu nasazení Resource Manager hello](virtual-networks-create-nsg-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ac18a-105">You can also [create NSGs in hello Resource Manager deployment model](virtual-networks-create-nsg-arm-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="ac18a-106">Ukázka Hello jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na scénář hello výše.</span><span class="sxs-lookup"><span data-stu-id="ac18a-106">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="ac18a-107">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí podle [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ac18a-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment by [creating a VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="ac18a-108">Jak toocreate hello skupina NSG pro podsíť pro front-end hello</span><span class="sxs-lookup"><span data-stu-id="ac18a-108">How toocreate hello NSG for hello front-end subnet</span></span>
<span data-ttu-id="ac18a-109">toocreate s názvem skupiny NSG s názvem **NSG front-endu** podle hello scénář výše, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="ac18a-109">toocreate an NSG named named **NSG-FrontEnd** based on hello scenario above, follow hello steps below:</span></span>

1. <span data-ttu-id="ac18a-110">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="ac18a-110">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="ac18a-111">Vytvořte skupinu zabezpečení sítě s názvem **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ac18a-111">Create a network security group named **NSG-FrontEnd**.</span></span>
   
        New-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" -Location uswest `
            -Label "Front end subnet NSG"
   
    <span data-ttu-id="ac18a-112">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="ac18a-112">Expected output:</span></span>

        Name         Location   Label               
        
        NSG-FrontEnd West US     Front end subnet NSG

3. <span data-ttu-id="ac18a-113">Vytvořte pravidlo zabezpečení povolení přístupu z Internetu tooport hello 3389.</span><span class="sxs-lookup"><span data-stu-id="ac18a-113">Create a security rule allowing access from hello Internet tooport 3389.</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
        | Set-AzureNetworkSecurityRule -Name rdp-rule `
            -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
            -SourceAddressPrefix Internet  -SourcePortRange '*' `
            -DestinationAddressPrefix '*' -DestinationPortRange '3389'
   
    <span data-ttu-id="ac18a-114">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="ac18a-114">Expected output:</span></span>
   
        Name     : NSG-FrontEnd
        Location : Central US
        Label    : Front end subnet NSG
        Rules    :
   
                      Type: Inbound
   
                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   rdp-rule             100       Allow    INTERNET        *             *                3389           TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       

                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *

1. <span data-ttu-id="ac18a-115">Vytvořte pravidlo zabezpečení povolení přístupu z Internetu tooport hello 80.</span><span class="sxs-lookup"><span data-stu-id="ac18a-115">Create a security rule allowing access from hello Internet tooport 80.</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
        | Set-AzureNetworkSecurityRule -Name web-rule `
            -Action Allow -Protocol TCP -Type Inbound -Priority 200 `
            -SourceAddressPrefix Internet  -SourcePortRange '*' `
            -DestinationAddressPrefix '*' -DestinationPortRange '80'
   
    <span data-ttu-id="ac18a-116">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="ac18a-116">Expected output:</span></span>

        Name     : NSG-FrontEnd
        Location : Central US
        Label    : Front end subnet NSG
        Rules    :

                      Type: Inbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   rdp-rule             100       Allow    INTERNET        *             *                3389           TCP     
                   web-rule             200       Allow    INTERNET        *             *                80             TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       


                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *   

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="ac18a-117">Jak toocreate hello skupina NSG pro hello zpět ukončení podsítě</span><span class="sxs-lookup"><span data-stu-id="ac18a-117">How toocreate hello NSG for hello back end subnet</span></span>
1. <span data-ttu-id="ac18a-118">Vytvořte skupinu zabezpečení sítě s názvem **NSG back-end**.</span><span class="sxs-lookup"><span data-stu-id="ac18a-118">Create a network security group named **NSG-BackEnd**.</span></span>
   
        New-AzureNetworkSecurityGroup -Name "NSG-BackEnd" -Location uswest `
            -Label "Back end subnet NSG"
   
    <span data-ttu-id="ac18a-119">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="ac18a-119">Expected output:</span></span>
   
        Name        Location   Label              
        
        NSG-BackEnd West US    Back end subnet NSG
2. <span data-ttu-id="ac18a-120">Vytvořte pravidlo zabezpečení povolení přístupu z hello front-endu podsíť tooport 1433 (výchozí port používaný systémem SQL Server).</span><span class="sxs-lookup"><span data-stu-id="ac18a-120">Create a security rule allowing access from hello front end subnet tooport 1433 (default port used by SQL Server).</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
        | Set-AzureNetworkSecurityRule -Name rdp-rule `
            -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
            -SourceAddressPrefix 192.168.1.0/24  -SourcePortRange '*' `
            -DestinationAddressPrefix '*' -DestinationPortRange '1433'
   
    <span data-ttu-id="ac18a-121">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="ac18a-121">Expected output:</span></span>
   
        Name     : NSG-BackEnd
        Location : Central US
        Label    : Back end subnet NSG
        Rules    :
   
                      Type: Inbound
   
                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   fe-rule              100       Allow    192.168.1.0/24  *             *                1433           TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       

                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *      

1. <span data-ttu-id="ac18a-122">Vytvořte pravidlo zabezpečení blokuje přístup z hello podsíť toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ac18a-122">Create a security rule blocking access from hello subnet toohello Internet.</span></span>
   
        Get-AzureNetworkSecurityGroup -Name "NSG-BackEnd" `
        | Set-AzureNetworkSecurityRule -Name block-internet `
            -Action Deny -Protocol '*' -Type Outbound -Priority 200 `
            -SourceAddressPrefix '*'  -SourcePortRange '*' `
            -DestinationAddressPrefix Internet -DestinationPortRange '*'
   
    <span data-ttu-id="ac18a-123">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="ac18a-123">Expected output:</span></span>
   
        Name     : NSG-BackEnd
        Location : Central US
        Label    : Back end subnet NSG
        Rules    :
   
                      Type: Inbound
   
                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   fe-rule              100       Allow    192.168.1.0/24  *             *                1433           TCP     
                   ALLOW VNET INBOUND   65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW AZURE LOAD     65001     Allow    AZURE_LOADBALAN *             *                *              *       
                   BALANCER INBOUND                        CER                                                                   
                   DENY ALL INBOUND     65500     Deny     *               *             *                *              *       

                      Type: Outbound

                   Name                 Priority  Action   Source Address  Source Port   Destination      Destination    Protocol
                                                           Prefix          Range         Address Prefix   Port Range             
                   
                   block-internet       200       Deny     *               *             INTERNET         *              *       
                   ALLOW VNET OUTBOUND  65000     Allow    VIRTUAL_NETWORK *             VIRTUAL_NETWORK  *              *       
                   ALLOW INTERNET       65001     Allow    *               *             INTERNET         *              *       
                   OUTBOUND                                                                                                      
                   DENY ALL OUTBOUND    65500     Deny     *               *             *                *              *   
