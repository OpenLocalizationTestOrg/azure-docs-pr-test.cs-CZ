---
title: "Všimněte si 1 vyřazení hostovaného operačního systému rodiny | Microsoft Docs"
description: "Poskytuje informace o při vyřazení Azure hostovaného operačního systému rodiny 1 stalo a jak určit, pokud se vás"
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
ms.openlocfilehash: 3178a09dab1cb972a3460d54dc9908fb95cce68b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="guest-os-family-1-retirement-notice"></a><span data-ttu-id="15796-103">Všimněte si vyřazení hostovaného operačního systému rodiny 1</span><span class="sxs-lookup"><span data-stu-id="15796-103">Guest OS Family 1 retirement notice</span></span>
<span data-ttu-id="15796-104">Vyřazení operačního systému rodiny 1 byla poprvé oznámeno na 1 červen 2013.</span><span class="sxs-lookup"><span data-stu-id="15796-104">The retirement of OS Family 1 was first announced on June 1, 2013.</span></span>

<span data-ttu-id="15796-105">**2. září 2014** Azure hostovaný operační systém (hostovaného operačního systému) rodiny 1.x, která je založena na operační systém Windows Server 2008, byl oficiálně vyřazeno.</span><span class="sxs-lookup"><span data-stu-id="15796-105">**Sept 2, 2014** The Azure Guest operating system (Guest OS) Family 1.x, which is based on the Windows Server 2008 operating system, was officially retired.</span></span> <span data-ttu-id="15796-106">Všechny pokusy o nasazení nové služby nebo upgradovat stávající služby pomocí řady 1 se nezdaří s chybovou zprávu oznamující, že byl vyřazen 1 hostovaného operačního systému rodiny.</span><span class="sxs-lookup"><span data-stu-id="15796-106">All attempts to deploy new services or upgrade existing services using Family 1 will fail with an error message informing you that the Guest OS Family 1 has been retired.</span></span>

<span data-ttu-id="15796-107">**3. listopadu 2014** ukončení rozšířené podpory pro hostovaného operačního systému rodiny 1 a plně je vyřazeno.</span><span class="sxs-lookup"><span data-stu-id="15796-107">**November 3, 2014** Extended support for Guest OS Family 1 ended and it is fully retired.</span></span> <span data-ttu-id="15796-108">Bude mít dopad na všechny služby ještě na 1 rodiny.</span><span class="sxs-lookup"><span data-stu-id="15796-108">All services still on Family 1 will be impacted.</span></span> <span data-ttu-id="15796-109">Můžeme kdykoli přestat těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="15796-109">We may stop those services at any time.</span></span> <span data-ttu-id="15796-110">Neexistuje žádná záruka, které vaše služby bude nadále spouštět Pokud provedete ruční upgrade je sami.</span><span class="sxs-lookup"><span data-stu-id="15796-110">There is no guarantee your services will continue to run unless you manually upgrade them yourself.</span></span>

<span data-ttu-id="15796-111">Pokud máte další dotazy, navštivte [fóra služby Cloud](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) nebo [požádejte podporu Azure](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="15796-111">If you have additional questions, visit the [Cloud Services Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) or [contact Azure support](https://azure.microsoft.com/support/options/).</span></span>

## <a name="are-you-affected"></a><span data-ttu-id="15796-112">Je zasaženo?</span><span class="sxs-lookup"><span data-stu-id="15796-112">Are you affected?</span></span>
<span data-ttu-id="15796-113">Cloudové služby se týká, pokud platí kterákoli z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="15796-113">Your Cloud Services are affected if any one of the following applies:</span></span>

1. <span data-ttu-id="15796-114">Mít hodnotu "osFamily ="1"explicitně zadané v souboru ServiceConfiguration.cscfg soubor pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="15796-114">You have a value of "osFamily = "1" explicitly specified in the ServiceConfiguration.cscfg file for your Cloud Service.</span></span>
2. <span data-ttu-id="15796-115">Nemáte hodnotu pro atribut osFamily explicitně zadané v souboru ServiceConfiguration.cscfg soubor pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="15796-115">You do not have a value for osFamily explicitly specified in the ServiceConfiguration.cscfg file for your Cloud Service.</span></span> <span data-ttu-id="15796-116">V současné době systém použije výchozí hodnotu 1"v takovém případě.</span><span class="sxs-lookup"><span data-stu-id="15796-116">Currently, the system uses the default value of "1" in this case.</span></span>
3. <span data-ttu-id="15796-117">Portál Azure uvádí rodiny hodnota hostovaný operační systém jako "Windows Server 2008".</span><span class="sxs-lookup"><span data-stu-id="15796-117">The Azure portal lists your Guest Operating System family value as "Windows Server 2008".</span></span>

<span data-ttu-id="15796-118">K vyhledání, která z cloudové služby běží které operačních systémů, můžete spustit následující skript v prostředí Azure PowerShell, i když je potřeba [nastavení prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) první.</span><span class="sxs-lookup"><span data-stu-id="15796-118">To find which of your cloud services are running which OS Family, you can run the following script in Azure PowerShell, though you must [set up Azure PowerShell](/powershell/azureps-cmdlets-docs) first.</span></span> <span data-ttu-id="15796-119">Další informace o skriptu najdete v tématu [Azure hostovaného operačního systému rodiny 1 End z životnosti: června 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="15796-119">For more information on the script, see [Azure Guest OS Family 1 End of Life: June 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span></span>

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

<span data-ttu-id="15796-120">Cloudové služby bude ovlivněná podle operačního systému rodiny 1 vyřazení sloupce osFamily ve výstupu skriptu je prázdný nebo obsahuje "1".</span><span class="sxs-lookup"><span data-stu-id="15796-120">Your cloud services will be impacted by OS Family 1 retirement if the osFamily column in the script output is empty or contains a "1".</span></span>

## <a name="recommendations-if-you-are-affected"></a><span data-ttu-id="15796-121">Doporučení, pokud se vás</span><span class="sxs-lookup"><span data-stu-id="15796-121">Recommendations if you are affected</span></span>
<span data-ttu-id="15796-122">Doporučujeme, abyste že migrovat role cloudové služby na jednu z podporovaných rodin OS hosta:</span><span class="sxs-lookup"><span data-stu-id="15796-122">We recommend you migrate your Cloud Service roles to one of the supported Guest OS Families:</span></span>

<span data-ttu-id="15796-123">**Hostovaného operačního systému rodiny 4.x** -Windows Server 2012 R2 *(doporučeno)*</span><span class="sxs-lookup"><span data-stu-id="15796-123">**Guest OS family 4.x** - Windows Server 2012 R2 *(recommended)*</span></span>

1. <span data-ttu-id="15796-124">Ujistěte se, že vaše aplikace používá SDK 2.1 nebo vyšší v rozhraní .NET framework 4.0, 4.5 a 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="15796-124">Ensure that your application is using SDK 2.1 or later with .NET framework 4.0, 4.5 or 4.5.1.</span></span>
2. <span data-ttu-id="15796-125">Nastavte atribut osFamily, který se "4" v souboru ServiceConfiguration.cscfg souboru a znovu nasaďte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="15796-125">Set the osFamily attribute to “4” in the ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="15796-126">**Hostovaného operačního systému rodiny 3.x** -Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="15796-126">**Guest OS family 3.x** - Windows Server 2012</span></span>

1. <span data-ttu-id="15796-127">Ujistěte se, že vaše aplikace používá SDK 1,8 nebo novější s rozhraním .NET framework 4.0 nebo 4.5.</span><span class="sxs-lookup"><span data-stu-id="15796-127">Ensure that your application is using SDK 1.8 or later with .NET framework 4.0 or 4.5.</span></span>
2. <span data-ttu-id="15796-128">V souboru ServiceConfiguration.cscfg souboru nastavený atribut osFamily na "3" a znovu nasaďte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="15796-128">Set the osFamily attribute to “3” in the ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="15796-129">**Hostovaného operačního systému rodiny 2.x** -Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="15796-129">**Guest OS family 2.x** - Windows Server 2008 R2</span></span>

1. <span data-ttu-id="15796-130">Ujistěte se, že vaše aplikace používá SDK 1.3 a výše uvedený v rozhraní .NET framework 3.5 nebo 4.0.</span><span class="sxs-lookup"><span data-stu-id="15796-130">Ensure that your application is using SDK 1.3 and above with .NET framework 3.5 or 4.0.</span></span>
2. <span data-ttu-id="15796-131">V souboru ServiceConfiguration.cscfg souboru nastavený atribut osFamily na 2 a znovu nasaďte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="15796-131">Set the osFamily attribute to "2" in the ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a><span data-ttu-id="15796-132">Rozšířené podpory pro hostovaného operačního systému rodiny 1 skončila 3 listopadu 2014</span><span class="sxs-lookup"><span data-stu-id="15796-132">Extended Support for Guest OS Family 1 ended Nov 3, 2014</span></span>
<span data-ttu-id="15796-133">Cloudové služby na hostovaného operačního systému rodiny 1 již nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="15796-133">Cloud services on Guest OS family 1 are no longer supported.</span></span> <span data-ttu-id="15796-134">Migrujte z rodiny 1 co nejdříve chcete zabránit přerušení služby.</span><span class="sxs-lookup"><span data-stu-id="15796-134">Migrate off family 1 as soon as possible to avoid service disruption.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="15796-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15796-135">Next steps</span></span>
<span data-ttu-id="15796-136">Přečtěte si nejnovější [uvolní hostovaného operačního systému](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="15796-136">Review the latest [Guest OS releases](cloud-services-guestos-update-matrix.md).</span></span>
