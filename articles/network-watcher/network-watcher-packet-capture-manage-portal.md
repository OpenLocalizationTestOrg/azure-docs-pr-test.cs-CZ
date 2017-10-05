---
title: "Spravovat zachycení paketů s sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat funkci zachycení paketu sledovací proces sítě pomocí portálu Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 33390532cc4fc1129a4f960d589f41bc95e5a1ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a><span data-ttu-id="36c1b-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="36c1b-103">Manage packet captures with Azure Network Watcher using the portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="36c1b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="36c1b-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="36c1b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="36c1b-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="36c1b-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="36c1b-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="36c1b-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="36c1b-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="36c1b-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="36c1b-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="36c1b-109">Zachytáváním paketů sledovací proces sítě vám umožní vytvořit relace zachytávání sledovat provoz do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="36c1b-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="36c1b-110">Filtry jsou k dispozici pro relaci zachytávání zajistit, že zaznamenáte pouze provoz, který chcete.</span><span class="sxs-lookup"><span data-stu-id="36c1b-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="36c1b-111">Při diagnostice sítě anomálií reaktivně a proaktivně pomáhá zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="36c1b-112">Jiné účely zahrnují shromažďování statistiku sítě, získá informace o síti vniknutí, k ladění komunikaci klienta se serverem a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="36c1b-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="36c1b-113">Díky vzdáleně aktivovat paketu zachycení, tato funkce snižuje zátěž spuštěných zachytáváním paketů ručně a na požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="36c1b-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="36c1b-114">Tento článek vás provede úloh jiný správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-114">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="36c1b-115">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="36c1b-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="36c1b-116">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="36c1b-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="36c1b-117">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="36c1b-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="36c1b-118">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="36c1b-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="36c1b-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="36c1b-119">Before you begin</span></span>

<span data-ttu-id="36c1b-120">Tento článek předpokládá, že máte v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="36c1b-120">This article assumes that you have the following resources:</span></span>

- <span data-ttu-id="36c1b-121">Instance sledovací proces sítě v oblasti, kterou chcete vytvořit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-121">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="36c1b-122">Virtuální počítač s příponou zachytávání paketů povoleno.</span><span class="sxs-lookup"><span data-stu-id="36c1b-122">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="36c1b-123">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="36c1b-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="36c1b-124">Instalaci rozšíření na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="36c1b-124">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

### <a name="packet-capture-agent-extension-through-the-portal"></a><span data-ttu-id="36c1b-125">Rozšíření agenta zachytávání paketů prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="36c1b-125">Packet Capture agent extension through the portal</span></span>

<span data-ttu-id="36c1b-126">Chcete-li nainstalovat agenta virtuálního počítače zachytávání paketů prostřednictvím portálu, přejděte k virtuálnímu počítači, klikněte na **rozšíření** > **přidat** a vyhledejte **sítě sledovacích procesů agenta pro Windows**</span><span class="sxs-lookup"><span data-stu-id="36c1b-126">To install the packet capture VM agent through the portal, navigate to your virtual machine, click **Extensions** > **Add** and search for **Network Watcher Agent for Windows**</span></span>

![zobrazení agenta][agent]

## <a name="packet-capture-overview"></a><span data-ttu-id="36c1b-128">Přehled zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-128">Packet Capture overview</span></span>

<span data-ttu-id="36c1b-129">Přejděte na [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sítě** > **sledovací proces sítě** > **zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="36c1b-129">Navigate to the [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **Packet Capture**</span></span>

<span data-ttu-id="36c1b-130">Přehled stránka obsahuje že seznam všech paketů zaznamená, který neexistuje bez ohledu na stav.</span><span class="sxs-lookup"><span data-stu-id="36c1b-130">The overview page shows a list of all packet captures that exist no matter the state.</span></span>

> [!NOTE]
> <span data-ttu-id="36c1b-131">Zachytáváním paketů vyžaduje připojení k účtu úložiště přes port 443.</span><span class="sxs-lookup"><span data-stu-id="36c1b-131">Packet capture requires connectivity to the storage account over port 443.</span></span>

![obrazovka Přehled zachytávání paketů][1]

## <a name="start-a-packet-capture"></a><span data-ttu-id="36c1b-133">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-133">Start a packet capture</span></span>

<span data-ttu-id="36c1b-134">Klikněte na tlačítko **přidat** vytvořit zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-134">Click **Add** to create a packet capture.</span></span>

<span data-ttu-id="36c1b-135">Vlastnosti, které lze definovat na zachytáváním paketů jsou:</span><span class="sxs-lookup"><span data-stu-id="36c1b-135">The properties that can be defined on a packet capture are:</span></span>

<span data-ttu-id="36c1b-136">**Hlavní nastavení**</span><span class="sxs-lookup"><span data-stu-id="36c1b-136">**Main settings**</span></span>

- <span data-ttu-id="36c1b-137">**Předplatné** – tato hodnota je odběr, který se používá, je potřeba instanci sledovací proces sítě v každém předplatném.</span><span class="sxs-lookup"><span data-stu-id="36c1b-137">**Subscription** - This value is the subscription that is used, an instance of network watcher is needed in each subscription.</span></span>
- <span data-ttu-id="36c1b-138">**Skupina prostředků** – prostředek skupiny virtuální počítač, který je cílem.</span><span class="sxs-lookup"><span data-stu-id="36c1b-138">**Resource group** - The resource group of the virtual machine that is being targeted.</span></span>
- <span data-ttu-id="36c1b-139">**Cílové virtuální počítač** -virtuální počítač, který běží v zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-139">**Target virtual machine** - The virtual machine that is running the packet capture</span></span>
- <span data-ttu-id="36c1b-140">**Název zachycení paketu** – tato hodnota je název zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-140">**Packet capture name** - This value is the name of the packet capture.</span></span>

<span data-ttu-id="36c1b-141">**Zachycení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="36c1b-141">**Capture configuration**</span></span>

- <span data-ttu-id="36c1b-142">**Účet úložiště** -Určuje, pokud se zachytáváním paketů je uložen v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="36c1b-142">**Storage Account** - Determines if packet capture is saved in a storage account.</span></span>
- <span data-ttu-id="36c1b-143">**Soubor** -Určuje, pokud se zachytáváním paketů se místně uloží na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="36c1b-143">**File** - Determines if a packet capture is saved locally on the virtual machine.</span></span>
- <span data-ttu-id="36c1b-144">**Účty úložiště** – vybraný účet úložiště se zachytáváním paketů v uložit.</span><span class="sxs-lookup"><span data-stu-id="36c1b-144">**Storage Accounts** - The selected storage account to save the packet capture in.</span></span> <span data-ttu-id="36c1b-145">Výchozí umístění je id name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription účtu https://{storage} /resourcegroups/ {název počítače name}/providers/microsoft.compute/virtualmachines/{virtual skupiny prostředků} / {RR} / {MM} / {DD} / {HH} packetcapture__{MM}_CAP _ {XXX} {SS}.</span><span class="sxs-lookup"><span data-stu-id="36c1b-145">Default location is https://{storage account name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription id}/resourcegroups/{resource group name}/providers/microsoft.compute/virtualmachines/{virtual machine name}/{YY}/{MM}/{DD}/packetcapture_{HH}_{MM}_{SS}_{XXX}.cap.</span></span> <span data-ttu-id="36c1b-146">(Aktivní, pouze pokud **úložiště** je vybraná)</span><span class="sxs-lookup"><span data-stu-id="36c1b-146">(Only enabled if **Storage** is selected)</span></span>
- <span data-ttu-id="36c1b-147">**Cesta k souboru místní** -místní cestu na virtuální počítač uložit zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-147">**Local file path** - The local path on a virtual machine to save the packet capture.</span></span> <span data-ttu-id="36c1b-148">(Aktivní, pouze pokud **soubor** je vybraný).</span><span class="sxs-lookup"><span data-stu-id="36c1b-148">(Only enabled if **File** is selected).</span></span> <span data-ttu-id="36c1b-149">Je nutné zadat platnou cestu</span><span class="sxs-lookup"><span data-stu-id="36c1b-149">A Valid path must be supplied</span></span>
- <span data-ttu-id="36c1b-150">**Maximální počet bajtů paketu** – počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné.</span><span class="sxs-lookup"><span data-stu-id="36c1b-150">**Maximum bytes per packet** - The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span>
- <span data-ttu-id="36c1b-151">**Maximální počet bajtů za relace** – celkový počet bajtů, které jsou zachyceny, hodnota v případě dosažení Zastaví zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-151">**Maximum bytes per session** - Total number of bytes that are captured, once the value is reached the packet capture stops.</span></span>
- <span data-ttu-id="36c1b-152">**Časový limit (sekundy)** -nastaví časový limit pro zachytávání paketů zastavit.</span><span class="sxs-lookup"><span data-stu-id="36c1b-152">**Time limit (seconds)** - Sets a time limit for the packet capture to stop.</span></span> <span data-ttu-id="36c1b-153">Výchozí hodnota je 18000 sekund.</span><span class="sxs-lookup"><span data-stu-id="36c1b-153">Default is 18000 seconds.</span></span>

> [!NOTE]
> <span data-ttu-id="36c1b-154">Účty služby Premium storage aktuálně nejsou podporované pro ukládání paketu zaznamená.</span><span class="sxs-lookup"><span data-stu-id="36c1b-154">Premium storage accounts are currently not supported for storing packet captures.</span></span>

<span data-ttu-id="36c1b-155">**Filtrování (volitelné)**</span><span class="sxs-lookup"><span data-stu-id="36c1b-155">**Filtering (Optional)**</span></span>

- <span data-ttu-id="36c1b-156">**Protokol** – protokol, který se zachytáváním paketů filtrovat.</span><span class="sxs-lookup"><span data-stu-id="36c1b-156">**Protocol** - The protocol to filter for the packet capture.</span></span> <span data-ttu-id="36c1b-157">Dostupné hodnoty jsou TCP, UDP a všechny.</span><span class="sxs-lookup"><span data-stu-id="36c1b-157">The available values are TCP, UDP, and Any.</span></span>
- <span data-ttu-id="36c1b-158">**Místní IP adresu** -tuto hodnotu filtry zachytáváním paketů na pakety, kde místní IP adresa odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="36c1b-158">**Local IP address** - This value filters the packet capture to packets where the local IP address matches this filter value.</span></span>
- <span data-ttu-id="36c1b-159">**Místní port** -tuto hodnotu filtry zachytáváním paketů na pakety, kde místního portu odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="36c1b-159">**Local port** - This value filters the packet capture to packets where the local port matches this filter value.</span></span>
- <span data-ttu-id="36c1b-160">**Vzdálené IP adresy** -tuto hodnotu filtry zachytáváním paketů na pakety, kde vzdálené IP odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="36c1b-160">**Remote IP address** - This value filters the packet capture to packets where the remote IP matches this filter value.</span></span>
- <span data-ttu-id="36c1b-161">**Vzdálený port** -tuto hodnotu filtry zachytáváním paketů na pakety, kde vzdálený port odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="36c1b-161">**Remote port** - This value filters the packet capture to packets where the remote port matches this filter value.</span></span>

> [!NOTE]
> <span data-ttu-id="36c1b-162">Hodnoty port a IP adres může být jedna hodnota, rozsah hodnot nebo u sady.</span><span class="sxs-lookup"><span data-stu-id="36c1b-162">Port and IP address values can be a single value, range of values, or a set.</span></span> <span data-ttu-id="36c1b-163">(to znamená, 80 – 1024 pro port) Můžete definovat libovolný počet filtrů tak, jak chcete.</span><span class="sxs-lookup"><span data-stu-id="36c1b-163">(that is, 80-1024 for port) You can define as many filters as you want.</span></span>

<span data-ttu-id="36c1b-164">Po vyplnění hodnoty, klikněte na **OK** vytvořit zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-164">Once the values are filled out, click **OK** to create the packet capture.</span></span>

![Vytvoření zachytávání paketů][2]

<span data-ttu-id="36c1b-166">Po vypršení časového limitu určeného v zachytáváním paketů zachytáváním paketů se zastaví a můžete zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="36c1b-166">After the time limit set on the packet capture has expired, the packet capture will stop and can be reviewed.</span></span> <span data-ttu-id="36c1b-167">Můžete také ručně zastavit relací zachycení paketů.</span><span class="sxs-lookup"><span data-stu-id="36c1b-167">You can also manually stop the packet captures sessions.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="36c1b-168">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-168">Delete a packet capture</span></span>

<span data-ttu-id="36c1b-169">V zobrazení zachytávání paketů, klikněte na tlačítko **kontextovou nabídku** (...) nebo klikněte pravým tlačítkem a klikněte na tlačítko **odstranit** zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-169">In the packet capture view, click the **context menu** (...) or right click, and click **delete** to stop the packet capture</span></span>

![Odstranit zachytávání paketů][3]

> [!NOTE]
> <span data-ttu-id="36c1b-171">Odstraňuje se zachytáváním paketů nedojde k odstranění souboru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="36c1b-171">Deleting a packet capture does not delete the file in the storage account.</span></span>

<span data-ttu-id="36c1b-172">Jste vyzváni k potvrzení chcete odstranit zachytáváním paketů, klikněte na **Ano**</span><span class="sxs-lookup"><span data-stu-id="36c1b-172">You are asked to confirm you want to delete the packet capture, click **Yes**</span></span>

![Potvrzení][4]

## <a name="stop-a-packet-capture"></a><span data-ttu-id="36c1b-174">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-174">Stop a packet capture</span></span>

<span data-ttu-id="36c1b-175">V zobrazení zachytávání paketů, klikněte na tlačítko **kontextovou nabídku** (...) nebo klikněte pravým tlačítkem a klikněte na tlačítko **Zastavit** zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-175">In the packet capture view, click the **context menu** (...) or right click, and click **Stop** to stop the packet capture</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="36c1b-176">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="36c1b-176">Download a packet capture</span></span>

<span data-ttu-id="36c1b-177">Po dokončení relace zachytávání paketů zachycení soubor nahraje do úložiště objektů blob nebo do místního souboru virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="36c1b-177">Once your packet capture session has completed, the capture file is uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="36c1b-178">Umístění úložiště pro zachytávání paketů se definuje při vytvoření relace.</span><span class="sxs-lookup"><span data-stu-id="36c1b-178">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="36c1b-179">Nástroj vhodné pro přístup k těmto zachycení soubory uložené na účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="36c1b-179">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="36c1b-180">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají na účet úložiště v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="36c1b-180">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="36c1b-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36c1b-181">Next steps</span></span>

<span data-ttu-id="36c1b-182">Informace o automatizaci paketu zachytává se virtuální počítač výstrahy zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="36c1b-182">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="36c1b-183">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="36c1b-183">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













