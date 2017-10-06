---
title: "Všimněte si 1 vyřazení aaaGuest operačního systému rodiny | Microsoft Docs"
description: "Poskytuje informace o při vyřazení hello Azure hostovaného operačního systému rodiny 1 se stalo a jak toodetermine, pokud se vás"
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 37b422e9-0713-4a81-a942-f553ef478064
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: fa8b904c6560dbbe4982c301f818c7a5cbc4eacb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guest-os-family-1-retirement-notice"></a><span data-ttu-id="16270-103">Všimněte si vyřazení hostovaného operačního systému rodiny 1</span><span class="sxs-lookup"><span data-stu-id="16270-103">Guest OS Family 1 retirement notice</span></span>
<span data-ttu-id="16270-104">vyřazení Hello operačního systému rodiny 1 byla poprvé oznámeno na 1 červen 2013.</span><span class="sxs-lookup"><span data-stu-id="16270-104">hello retirement of OS Family 1 was first announced on June 1, 2013.</span></span>

<span data-ttu-id="16270-105">**2. září 2014** hello Azure hostovaný operační systém (hostovaného operačního systému) rodiny 1.x, která je založena na systému Windows Server 2008 hello operačního systému, byl oficiálně vyřazeno.</span><span class="sxs-lookup"><span data-stu-id="16270-105">**Sept 2, 2014** hello Azure Guest operating system (Guest OS) Family 1.x, which is based on hello Windows Server 2008 operating system, was officially retired.</span></span> <span data-ttu-id="16270-106">Všechny pokusy o toodeploy nových služeb nebo upgradu existujících služeb pomocí řady 1 se nezdaří s chybovou zprávu oznamující, že hello, že byl vyřazen hostovaného operačního systému rodiny 1.</span><span class="sxs-lookup"><span data-stu-id="16270-106">All attempts toodeploy new services or upgrade existing services using Family 1 will fail with an error message informing you that hello Guest OS Family 1 has been retired.</span></span>

<span data-ttu-id="16270-107">**3. listopadu 2014** ukončení rozšířené podpory pro hostovaného operačního systému rodiny 1 a plně je vyřazeno.</span><span class="sxs-lookup"><span data-stu-id="16270-107">**November 3, 2014** Extended support for Guest OS Family 1 ended and it is fully retired.</span></span> <span data-ttu-id="16270-108">Bude mít dopad na všechny služby ještě na 1 rodiny.</span><span class="sxs-lookup"><span data-stu-id="16270-108">All services still on Family 1 will be impacted.</span></span> <span data-ttu-id="16270-109">Můžeme kdykoli přestat těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="16270-109">We may stop those services at any time.</span></span> <span data-ttu-id="16270-110">Neexistuje žádná záruka, že vaše služby bude pokračovat toorun, pokud provedete ruční upgrade je sami.</span><span class="sxs-lookup"><span data-stu-id="16270-110">There is no guarantee your services will continue toorun unless you manually upgrade them yourself.</span></span>

<span data-ttu-id="16270-111">Pokud máte další dotazy, navštivte hello [fóra služby Cloud](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) nebo [požádejte podporu Azure](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="16270-111">If you have additional questions, visit hello [Cloud Services Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) or [contact Azure support](https://azure.microsoft.com/support/options/).</span></span>

## <a name="are-you-affected"></a><span data-ttu-id="16270-112">Je zasaženo?</span><span class="sxs-lookup"><span data-stu-id="16270-112">Are you affected?</span></span>
<span data-ttu-id="16270-113">Cloudové služby se týká, pokud platí kterákoli hello následující:</span><span class="sxs-lookup"><span data-stu-id="16270-113">Your Cloud Services are affected if any one of hello following applies:</span></span>

1. <span data-ttu-id="16270-114">Mít hodnotu "osFamily ="1"výslovně uvedené v souboru ServiceConfiguration.cscfg souboru hello vaší cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="16270-114">You have a value of "osFamily = "1" explicitly specified in hello ServiceConfiguration.cscfg file for your Cloud Service.</span></span>
2. <span data-ttu-id="16270-115">Nemáte hodnotu pro atribut osFamily výslovně uvedené v souboru ServiceConfiguration.cscfg souboru hello vaší cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="16270-115">You do not have a value for osFamily explicitly specified in hello ServiceConfiguration.cscfg file for your Cloud Service.</span></span> <span data-ttu-id="16270-116">V současné době systému hello používá hello výchozí hodnotu 1"v takovém případě.</span><span class="sxs-lookup"><span data-stu-id="16270-116">Currently, hello system uses hello default value of "1" in this case.</span></span>
3. <span data-ttu-id="16270-117">Hello portál Azure uvádí rodiny hodnota hostovaný operační systém jako "Windows Server 2008".</span><span class="sxs-lookup"><span data-stu-id="16270-117">hello Azure portal lists your Guest Operating System family value as "Windows Server 2008".</span></span>

<span data-ttu-id="16270-118">toofind, která z cloudové služby běží které operačních systémů, můžete spustit hello následující skript v prostředí Azure PowerShell, i když je potřeba [nastavení prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) první.</span><span class="sxs-lookup"><span data-stu-id="16270-118">toofind which of your cloud services are running which OS Family, you can run hello following script in Azure PowerShell, though you must [set up Azure PowerShell](/powershell/azureps-cmdlets-docs) first.</span></span> <span data-ttu-id="16270-119">Další informace o skriptu hello najdete v tématu [Azure hostovaného operačního systému rodiny 1 End z životnosti: června 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="16270-119">For more information on hello script, see [Azure Guest OS Family 1 End of Life: June 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span></span>

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

<span data-ttu-id="16270-120">Cloudové služby bude ovlivněná podle operačního systému rodiny 1 vyřazení sloupce osFamily hello ve výstupu skriptu hello je prázdný nebo obsahuje "1".</span><span class="sxs-lookup"><span data-stu-id="16270-120">Your cloud services will be impacted by OS Family 1 retirement if hello osFamily column in hello script output is empty or contains a "1".</span></span>

## <a name="recommendations-if-you-are-affected"></a><span data-ttu-id="16270-121">Doporučení, pokud se vás</span><span class="sxs-lookup"><span data-stu-id="16270-121">Recommendations if you are affected</span></span>
<span data-ttu-id="16270-122">Doporučujeme, abyste že migrujete vaše cloudové služby role tooone hello podporované hostovaného operačního systému rodin:</span><span class="sxs-lookup"><span data-stu-id="16270-122">We recommend you migrate your Cloud Service roles tooone of hello supported Guest OS Families:</span></span>

<span data-ttu-id="16270-123">**Hostovaného operačního systému rodiny 4.x** -Windows Server 2012 R2 *(doporučeno)*</span><span class="sxs-lookup"><span data-stu-id="16270-123">**Guest OS family 4.x** - Windows Server 2012 R2 *(recommended)*</span></span>

1. <span data-ttu-id="16270-124">Ujistěte se, že vaše aplikace používá SDK 2.1 nebo vyšší v rozhraní .NET framework 4.0, 4.5 a 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="16270-124">Ensure that your application is using SDK 2.1 or later with .NET framework 4.0, 4.5 or 4.5.1.</span></span>
2. <span data-ttu-id="16270-125">Nastavit hello osFamily atribut hello příliš "4" v souboru ServiceConfiguration.cscfg soubor a znovu nasaďte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="16270-125">Set hello osFamily attribute too“4” in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="16270-126">**Hostovaného operačního systému rodiny 3.x** -Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="16270-126">**Guest OS family 3.x** - Windows Server 2012</span></span>

1. <span data-ttu-id="16270-127">Ujistěte se, že vaše aplikace používá SDK 1,8 nebo novější s rozhraním .NET framework 4.0 nebo 4.5.</span><span class="sxs-lookup"><span data-stu-id="16270-127">Ensure that your application is using SDK 1.8 or later with .NET framework 4.0 or 4.5.</span></span>
2. <span data-ttu-id="16270-128">Nastavit hello osFamily atribut příliš "3" hello v souboru ServiceConfiguration.cscfg soubor a znovu nasaďte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="16270-128">Set hello osFamily attribute too“3” in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="16270-129">**Hostovaného operačního systému rodiny 2.x** -Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="16270-129">**Guest OS family 2.x** - Windows Server 2008 R2</span></span>

1. <span data-ttu-id="16270-130">Ujistěte se, že vaše aplikace používá SDK 1.3 a výše uvedený v rozhraní .NET framework 3.5 nebo 4.0.</span><span class="sxs-lookup"><span data-stu-id="16270-130">Ensure that your application is using SDK 1.3 and above with .NET framework 3.5 or 4.0.</span></span>
2. <span data-ttu-id="16270-131">Nastavit hello osFamily atribut příliš "2" hello v souboru ServiceConfiguration.cscfg soubor a znovu nasaďte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="16270-131">Set hello osFamily attribute too"2" in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a><span data-ttu-id="16270-132">Rozšířené podpory pro hostovaného operačního systému rodiny 1 skončila 3 listopadu 2014</span><span class="sxs-lookup"><span data-stu-id="16270-132">Extended Support for Guest OS Family 1 ended Nov 3, 2014</span></span>
<span data-ttu-id="16270-133">Cloudové služby na hostovaného operačního systému rodiny 1 již nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="16270-133">Cloud services on Guest OS family 1 are no longer supported.</span></span> <span data-ttu-id="16270-134">Migrujte z rodiny 1 také možné tooavoid služby přerušení.</span><span class="sxs-lookup"><span data-stu-id="16270-134">Migrate off family 1 as soon as possible tooavoid service disruption.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="16270-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16270-135">Next steps</span></span>
<span data-ttu-id="16270-136">Zkontrolujte hello nejnovější [uvolní hostovaného operačního systému](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="16270-136">Review hello latest [Guest OS releases](cloud-services-guestos-update-matrix.md).</span></span>
