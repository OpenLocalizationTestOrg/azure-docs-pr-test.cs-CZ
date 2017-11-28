---
title: "aaaCheck připojení s sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak zkontrolovat připojení toouse s sledovací proces sítě pomocí hello portálu Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: gwallace
ms.openlocfilehash: ef6ecccd688f06f70003a5b59771c15bcbe8f3e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a><span data-ttu-id="55d5b-103">Zkontrolujte připojení s sledovací proces sítě Azure pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="55d5b-103">Check connectivity with Azure Network Watcher using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="55d5b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="55d5b-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="55d5b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="55d5b-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="55d5b-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="55d5b-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="55d5b-107">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="55d5b-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="55d5b-108">Zjistěte, jak by bylo možné navázat připojení tooverify toouse Pokud přímé připojení TCP z virtuálního počítače tooa, zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="55d5b-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="55d5b-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="55d5b-109">Before you begin</span></span>

<span data-ttu-id="55d5b-110">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="55d5b-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="55d5b-111">Instance sledovací proces sítě v hello oblasti, kterou chcete toocheck připojení.</span><span class="sxs-lookup"><span data-stu-id="55d5b-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="55d5b-112">Virtuální počítače připojení toocheck s.</span><span class="sxs-lookup"><span data-stu-id="55d5b-112">Virtual machines toocheck connectivity with.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55d5b-113">Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="55d5b-113">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="55d5b-114">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="55d5b-114">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="55d5b-115">Zkontrolujte připojení k tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="55d5b-115">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="55d5b-116">Tento příklad zkontroluje připojení tooa cílového virtuálního počítače přes port 80.</span><span class="sxs-lookup"><span data-stu-id="55d5b-116">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

<span data-ttu-id="55d5b-117">Přejděte tooyour sledovací proces sítě a klikněte na tlačítko **Kontrola připojení (Preview)**.</span><span class="sxs-lookup"><span data-stu-id="55d5b-117">Navigate tooyour Network Watcher and click **Connectivity check (Preview)**.</span></span> <span data-ttu-id="55d5b-118">Vyberte ze hello k toocheck virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="55d5b-118">Select hello virtual machine toocheck connectivity from.</span></span> <span data-ttu-id="55d5b-119">V hello **cílové** vyberte **vyberte virtuální počítač** a zvolte hello správný virtuální počítač a tootest portu.</span><span class="sxs-lookup"><span data-stu-id="55d5b-119">In hello **Destination** section choose **Select a virtual machine** and choose hello correct virtual machine and port tootest.</span></span>

<span data-ttu-id="55d5b-120">Po kliknutí na tlačítko **zkontrolujte**, se kontroluje hello připojení mezi virtuálními počítači hello na zadaný port hello.</span><span class="sxs-lookup"><span data-stu-id="55d5b-120">Once you click **Check**, hello connectivity between hello virtual machines on hello port specified are checked.</span></span> <span data-ttu-id="55d5b-121">V příkladu hello hello cílový počítač nedostupný, zobrazí se seznam všech segmentů směrování.</span><span class="sxs-lookup"><span data-stu-id="55d5b-121">In hello example, hello destination VM is unreachable, a listing of hops are shown.</span></span>

![Výsledky kontroly připojení pro virtuální počítač][1]

## <a name="check-remote-endpoint-connectivity"></a><span data-ttu-id="55d5b-123">Zkontrolujte připojení vzdáleného koncového bodu</span><span class="sxs-lookup"><span data-stu-id="55d5b-123">Check remote endpoint connectivity</span></span>

<span data-ttu-id="55d5b-124">toocheck hello připojení a latence tooa vzdálený koncový bod, vyberte hello **ručně zadejte** přepínač v hello **cílové** části, zadejte hello adresy url a hello port a klikněte na tlačítko **zkontrolujte** .</span><span class="sxs-lookup"><span data-stu-id="55d5b-124">toocheck hello connectivity and latency tooa remote endpoint, choose hello **Specify manually** radio button in hello **Destination** section, input hello url and hello port and click **Check**.</span></span>  <span data-ttu-id="55d5b-125">Používá se pro vzdálené koncové body jako koncové body weby a úložiště.</span><span class="sxs-lookup"><span data-stu-id="55d5b-125">This is used for remote endpoints like websites and storage endpoints.</span></span>

![Výsledky kontroly připojení pro webovou stránku][2]

## <a name="next-steps"></a><span data-ttu-id="55d5b-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55d5b-127">Next steps</span></span>

<span data-ttu-id="55d5b-128">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="55d5b-128">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="55d5b-129">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="55d5b-129">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
