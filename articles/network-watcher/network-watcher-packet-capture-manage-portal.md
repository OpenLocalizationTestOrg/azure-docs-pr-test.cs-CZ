---
title: "paket aaaManage zachytávali sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage hello funkce zachytávání paketů sledovací proces sítě pomocí portálu Azure"
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
ms.openlocfilehash: 431b145ee215b8d9421fb2aacdce08a0de90b35e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-hello-portal"></a><span data-ttu-id="9810a-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="9810a-103">Manage packet captures with Azure Network Watcher using hello portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="9810a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9810a-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="9810a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9810a-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="9810a-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9810a-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="9810a-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9810a-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="9810a-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="9810a-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="9810a-109">Zachytáváním paketů sledovací proces sítě vám umožní toocreate zaznamenání relace tootrack provoz tooand z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9810a-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="9810a-110">Filtry jsou podle hello zaznamenání relace tooensure že zaznamenáte jenom provoz hello, které chcete.</span><span class="sxs-lookup"><span data-stu-id="9810a-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="9810a-111">Zachytáváním paketů pomáhá toodiagnose sítě anomálií reaktivně a proaktivně.</span><span class="sxs-lookup"><span data-stu-id="9810a-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="9810a-112">Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="9810a-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="9810a-113">Tím, že je schopný tooremotely aktivační událost paketu zachycení, tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně a hello požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="9810a-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="9810a-114">Tento článek vás provede hello úlohy různých správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="9810a-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="9810a-115">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="9810a-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="9810a-116">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="9810a-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="9810a-117">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="9810a-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="9810a-118">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="9810a-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="9810a-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9810a-119">Before you begin</span></span>

<span data-ttu-id="9810a-120">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="9810a-120">This article assumes that you have hello following resources:</span></span>

- <span data-ttu-id="9810a-121">Instance sledovací proces sítě v hello oblasti, kterou chcete toocreate zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9810a-121">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="9810a-122">Virtuální počítač s hello paketu zachytit rozšíření povolené.</span><span class="sxs-lookup"><span data-stu-id="9810a-122">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9810a-123">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="9810a-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="9810a-124">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="9810a-124">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

### <a name="packet-capture-agent-extension-through-hello-portal"></a><span data-ttu-id="9810a-125">Rozšíření agenta zachytávání paketů přes portál hello</span><span class="sxs-lookup"><span data-stu-id="9810a-125">Packet Capture agent extension through hello portal</span></span>

<span data-ttu-id="9810a-126">zachytáváním paketů hello tooinstall agenta virtuálního počítače přes portál hello přejděte tooyour virtuální počítač, klikněte na **rozšíření** > **přidat** a vyhledejte **sítě sledovacích procesů agenta pro Windows**</span><span class="sxs-lookup"><span data-stu-id="9810a-126">tooinstall hello packet capture VM agent through hello portal, navigate tooyour virtual machine, click **Extensions** > **Add** and search for **Network Watcher Agent for Windows**</span></span>

![zobrazení agenta][agent]

## <a name="packet-capture-overview"></a><span data-ttu-id="9810a-128">Přehled zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9810a-128">Packet Capture overview</span></span>

<span data-ttu-id="9810a-129">Přejděte toohello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sítě** > **sledovací proces sítě** > **zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="9810a-129">Navigate toohello [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **Packet Capture**</span></span>

<span data-ttu-id="9810a-130">stránka s přehledem Hello zobrazuje že seznam všech paketů zaznamená, který neexistuje bez ohledu na to hello stavu.</span><span class="sxs-lookup"><span data-stu-id="9810a-130">hello overview page shows a list of all packet captures that exist no matter hello state.</span></span>

> [!NOTE]
> <span data-ttu-id="9810a-131">Zachytáváním paketů vyžaduje účet úložiště toohello připojení přes port 443.</span><span class="sxs-lookup"><span data-stu-id="9810a-131">Packet capture requires connectivity toohello storage account over port 443.</span></span>

![obrazovka Přehled zachytávání paketů][1]

## <a name="start-a-packet-capture"></a><span data-ttu-id="9810a-133">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9810a-133">Start a packet capture</span></span>

<span data-ttu-id="9810a-134">Klikněte na tlačítko **přidat** toocreate zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="9810a-134">Click **Add** toocreate a packet capture.</span></span>

<span data-ttu-id="9810a-135">Hello vlastnosti, které lze definovat v zachytáváním paketů jsou:</span><span class="sxs-lookup"><span data-stu-id="9810a-135">hello properties that can be defined on a packet capture are:</span></span>

<span data-ttu-id="9810a-136">**Hlavní nastavení**</span><span class="sxs-lookup"><span data-stu-id="9810a-136">**Main settings**</span></span>

- <span data-ttu-id="9810a-137">**Předplatné** – tato hodnota je hello odběr, který se používá, je potřeba instanci sledovací proces sítě v každém předplatném.</span><span class="sxs-lookup"><span data-stu-id="9810a-137">**Subscription** - This value is hello subscription that is used, an instance of network watcher is needed in each subscription.</span></span>
- <span data-ttu-id="9810a-138">**Skupina prostředků** -hello hello virtuální počítač, který je cílem skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="9810a-138">**Resource group** - hello resource group of hello virtual machine that is being targeted.</span></span>
- <span data-ttu-id="9810a-139">**Cílové virtuální počítač** -hello virtuální počítač, který běží zachytáváním paketů hello</span><span class="sxs-lookup"><span data-stu-id="9810a-139">**Target virtual machine** - hello virtual machine that is running hello packet capture</span></span>
- <span data-ttu-id="9810a-140">**Název zachycení paketu** – tato hodnota je název hello zachytáváním paketů hello.</span><span class="sxs-lookup"><span data-stu-id="9810a-140">**Packet capture name** - This value is hello name of hello packet capture.</span></span>

<span data-ttu-id="9810a-141">**Zachycení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="9810a-141">**Capture configuration**</span></span>

- <span data-ttu-id="9810a-142">**Účet úložiště** -Určuje, pokud se zachytáváním paketů je uložen v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9810a-142">**Storage Account** - Determines if packet capture is saved in a storage account.</span></span>
- <span data-ttu-id="9810a-143">**Soubor** -Určuje, pokud se zachytáváním paketů uložená lokálně na hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9810a-143">**File** - Determines if a packet capture is saved locally on hello virtual machine.</span></span>
- <span data-ttu-id="9810a-144">**Účty úložiště** -hello vybrané zachytáváním paketů úložiště účet toosave hello v.</span><span class="sxs-lookup"><span data-stu-id="9810a-144">**Storage Accounts** - hello selected storage account toosave hello packet capture in.</span></span> <span data-ttu-id="9810a-145">Výchozí umístění je id name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription účtu https://{storage} /resourcegroups/ {název počítače name}/providers/microsoft.compute/virtualmachines/{virtual skupiny prostředků} / {RR} / {MM} / {DD} / {HH} packetcapture__{MM}_CAP _ {XXX} {SS}.</span><span class="sxs-lookup"><span data-stu-id="9810a-145">Default location is https://{storage account name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription id}/resourcegroups/{resource group name}/providers/microsoft.compute/virtualmachines/{virtual machine name}/{YY}/{MM}/{DD}/packetcapture_{HH}_{MM}_{SS}_{XXX}.cap.</span></span> <span data-ttu-id="9810a-146">(Aktivní, pouze pokud **úložiště** je vybraná)</span><span class="sxs-lookup"><span data-stu-id="9810a-146">(Only enabled if **Storage** is selected)</span></span>
- <span data-ttu-id="9810a-147">**Cesta k souboru místní** -hello místní cestu na zachycení virtuálního počítače toosave hello paketů.</span><span class="sxs-lookup"><span data-stu-id="9810a-147">**Local file path** - hello local path on a virtual machine toosave hello packet capture.</span></span> <span data-ttu-id="9810a-148">(Aktivní, pouze pokud **soubor** je vybraný).</span><span class="sxs-lookup"><span data-stu-id="9810a-148">(Only enabled if **File** is selected).</span></span> <span data-ttu-id="9810a-149">Je nutné zadat platnou cestu</span><span class="sxs-lookup"><span data-stu-id="9810a-149">A Valid path must be supplied</span></span>
- <span data-ttu-id="9810a-150">**Maximální počet bajtů paketu** -hello počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné.</span><span class="sxs-lookup"><span data-stu-id="9810a-150">**Maximum bytes per packet** - hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span>
- <span data-ttu-id="9810a-151">**Maximální počet bajtů za relace** – celkový počet bajtů, které jsou zachyceny po dosažení Zastaví zachytávání paketů hello hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9810a-151">**Maximum bytes per session** - Total number of bytes that are captured, once hello value is reached hello packet capture stops.</span></span>
- <span data-ttu-id="9810a-152">**Časový limit (sekundy)** -nastaví časový limit pro toostop zachytávání paketů hello.</span><span class="sxs-lookup"><span data-stu-id="9810a-152">**Time limit (seconds)** - Sets a time limit for hello packet capture toostop.</span></span> <span data-ttu-id="9810a-153">Výchozí hodnota je 18000 sekund.</span><span class="sxs-lookup"><span data-stu-id="9810a-153">Default is 18000 seconds.</span></span>

> [!NOTE]
> <span data-ttu-id="9810a-154">Účty služby Premium storage aktuálně nejsou podporované pro ukládání paketu zaznamená.</span><span class="sxs-lookup"><span data-stu-id="9810a-154">Premium storage accounts are currently not supported for storing packet captures.</span></span>

<span data-ttu-id="9810a-155">**Filtrování (volitelné)**</span><span class="sxs-lookup"><span data-stu-id="9810a-155">**Filtering (Optional)**</span></span>

- <span data-ttu-id="9810a-156">**Protokol** -hello toofilter protokol pro zachytávání paketů hello.</span><span class="sxs-lookup"><span data-stu-id="9810a-156">**Protocol** - hello protocol toofilter for hello packet capture.</span></span> <span data-ttu-id="9810a-157">Hello dostupné jsou hodnoty TCP, UDP a všechny.</span><span class="sxs-lookup"><span data-stu-id="9810a-157">hello available values are TCP, UDP, and Any.</span></span>
- <span data-ttu-id="9810a-158">**Místní IP adresu** -tuto hodnotu filtry toopackets zachytávání paketů hello kde hello místní IP adresa odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="9810a-158">**Local IP address** - This value filters hello packet capture toopackets where hello local IP address matches this filter value.</span></span>
- <span data-ttu-id="9810a-159">**Místní port** -tuto hodnotu filtry toopackets zachytávání paketů hello kde místního portu hello odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="9810a-159">**Local port** - This value filters hello packet capture toopackets where hello local port matches this filter value.</span></span>
- <span data-ttu-id="9810a-160">**Vzdálené IP adresy** -tuto hodnotu filtry toopackets zachytávání paketů hello kde hello vzdálené IP odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="9810a-160">**Remote IP address** - This value filters hello packet capture toopackets where hello remote IP matches this filter value.</span></span>
- <span data-ttu-id="9810a-161">**Vzdálený port** -tuto hodnotu filtry toopackets zachytávání paketů hello kde vzdálených portů hello odpovídá této hodnotě filtru.</span><span class="sxs-lookup"><span data-stu-id="9810a-161">**Remote port** - This value filters hello packet capture toopackets where hello remote port matches this filter value.</span></span>

> [!NOTE]
> <span data-ttu-id="9810a-162">Hodnoty port a IP adres může být jedna hodnota, rozsah hodnot nebo u sady.</span><span class="sxs-lookup"><span data-stu-id="9810a-162">Port and IP address values can be a single value, range of values, or a set.</span></span> <span data-ttu-id="9810a-163">(to znamená, 80 – 1024 pro port) Můžete definovat libovolný počet filtrů tak, jak chcete.</span><span class="sxs-lookup"><span data-stu-id="9810a-163">(that is, 80-1024 for port) You can define as many filters as you want.</span></span>

<span data-ttu-id="9810a-164">Jakmile jsou doplnit hello hodnoty, klikněte na možnost **OK** zachytáváním paketů toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="9810a-164">Once hello values are filled out, click **OK** toocreate hello packet capture.</span></span>

![Vytvoření zachytávání paketů][2]

<span data-ttu-id="9810a-166">Po nastavení hello časový limit na zachytáváním paketů hello vypršela platnost, zachytáváním paketů hello se zastaví a můžete zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="9810a-166">After hello time limit set on hello packet capture has expired, hello packet capture will stop and can be reviewed.</span></span> <span data-ttu-id="9810a-167">Můžete také ručně zastavit hello paketu zachycení relace.</span><span class="sxs-lookup"><span data-stu-id="9810a-167">You can also manually stop hello packet captures sessions.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="9810a-168">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9810a-168">Delete a packet capture</span></span>

<span data-ttu-id="9810a-169">V paketu hello zaznamenat zobrazení, klikněte na tlačítko hello **kontextovou nabídku** (...) nebo klikněte pravým tlačítkem a klikněte na tlačítko **odstranit** zachytáváním paketů toostop hello</span><span class="sxs-lookup"><span data-stu-id="9810a-169">In hello packet capture view, click hello **context menu** (...) or right click, and click **delete** toostop hello packet capture</span></span>

![Odstranit zachytávání paketů][3]

> [!NOTE]
> <span data-ttu-id="9810a-171">Odstraňuje se zachytáváním paketů nedojde k odstranění hello souboru v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="9810a-171">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

<span data-ttu-id="9810a-172">Jste vyzváni tooconfirm chcete zachytáváním paketů hello toodelete, klikněte na tlačítko **Ano**</span><span class="sxs-lookup"><span data-stu-id="9810a-172">You are asked tooconfirm you want toodelete hello packet capture, click **Yes**</span></span>

![Potvrzení][4]

## <a name="stop-a-packet-capture"></a><span data-ttu-id="9810a-174">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9810a-174">Stop a packet capture</span></span>

<span data-ttu-id="9810a-175">V paketu hello zaznamenat zobrazení, klikněte na tlačítko hello **kontextovou nabídku** (...) nebo klikněte pravým tlačítkem a klikněte na tlačítko **Zastavit** zachytáváním paketů toostop hello</span><span class="sxs-lookup"><span data-stu-id="9810a-175">In hello packet capture view, click hello **context menu** (...) or right click, and click **Stop** toostop hello packet capture</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="9810a-176">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="9810a-176">Download a packet capture</span></span>

<span data-ttu-id="9810a-177">Po dokončení relace zachytávání paketů hello zachycení nahrané tooblob úložiště nebo tooa místní soubor je na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9810a-177">Once your packet capture session has completed, hello capture file is uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="9810a-178">umístění úložiště Hello zachytáváním paketů hello se definuje při vytvoření relace hello.</span><span class="sxs-lookup"><span data-stu-id="9810a-178">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="9810a-179">Vhodné nástroje tooaccess tyto zaznamenat soubory uložené tooa účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="9810a-179">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="9810a-180">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="9810a-180">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="9810a-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9810a-181">Next steps</span></span>

<span data-ttu-id="9810a-182">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="9810a-182">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="9810a-183">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="9810a-183">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













