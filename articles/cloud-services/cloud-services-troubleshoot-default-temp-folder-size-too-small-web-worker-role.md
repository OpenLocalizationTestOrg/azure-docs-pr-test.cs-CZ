---
title: "velikost aaaDefault dočasné složky je příliš malá pro role | Microsoft Docs"
description: "Cloudové služby role má omezené množství místa hello dočasné složky. Tento článek obsahuje některé návrhy tooavoid volné místo."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 307dc20f3264e29d122a6616be0028d2ec1282c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a><span data-ttu-id="fef89-104">Velikost výchozí dočasné složky je příliš malá v roli web nebo worker cloudové služby</span><span class="sxs-lookup"><span data-stu-id="fef89-104">Default TEMP folder size is too small on a cloud service web/worker role</span></span>
<span data-ttu-id="fef89-105">Výchozí Hello dočasný adresář role pracovního procesu nebo webové služby cloud má maximální velikost 100 MB, který může dojít k úplné v určitém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="fef89-105">hello default temporary directory of a cloud service worker or web role has a maximum size of 100 MB, which may become full at some point.</span></span> <span data-ttu-id="fef89-106">Tento článek popisuje, jak tooavoid volné místo pro dočasný adresář hello.</span><span class="sxs-lookup"><span data-stu-id="fef89-106">This article describes how tooavoid running out of space for hello temporary directory.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a><span data-ttu-id="fef89-107">Proč dochází volné místo?</span><span class="sxs-lookup"><span data-stu-id="fef89-107">Why do I run out of space?</span></span>
<span data-ttu-id="fef89-108">Hello standardní Windows proměnné prostředí TEMP a TMP jsou k dispozici toocode, který běží ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fef89-108">hello standard Windows environment variables TEMP and TMP are available toocode that is running in your application.</span></span> <span data-ttu-id="fef89-109">TEMP a TMP bodu tooa jeden adresář, který má maximální velikost 100 MB.</span><span class="sxs-lookup"><span data-stu-id="fef89-109">Both TEMP and TMP point tooa single directory that has a maximum size of 100 MB.</span></span> <span data-ttu-id="fef89-110">Žádná data, která je uložená v tomto adresáři není zachován po životního cyklu hello hello cloudové služby; Pokud jsou recyklovány hello instance rolí v cloudové službě, je vyčistit adresář hello.</span><span class="sxs-lookup"><span data-stu-id="fef89-110">Any data that is stored in this directory is not persisted across hello lifecycle of hello cloud service; if hello role instances in a cloud service are recycled, hello directory is cleaned.</span></span>

## <a name="suggestion-toofix-hello-problem"></a><span data-ttu-id="fef89-111">Návrh toofix hello problém</span><span class="sxs-lookup"><span data-stu-id="fef89-111">Suggestion toofix hello problem</span></span>
<span data-ttu-id="fef89-112">Implementujte jednu z následujících alternativních hello:</span><span class="sxs-lookup"><span data-stu-id="fef89-112">Implement one of hello following alternatives:</span></span>

* <span data-ttu-id="fef89-113">Nakonfigurujte místní úložiště prostředků a k němu přístup přímo místo použití TEMP a TMP.</span><span class="sxs-lookup"><span data-stu-id="fef89-113">Configure a local storage resource, and access it directly instead of using TEMP or TMP.</span></span> <span data-ttu-id="fef89-114">tooaccess místní úložiště prostředků z kódu, který běží v rámci vaší aplikace, volání hello [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="fef89-114">tooaccess a local storage resource from code that is running within your application, call hello [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>
* <span data-ttu-id="fef89-115">Nakonfigurujte místní úložiště prostředků a bodu hello TEMP a TMP adresáře toopoint toohello cesta prostředku hello místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="fef89-115">Configure a local storage resource, and point hello TEMP and TMP directories toopoint toohello path of hello local storage resource.</span></span> <span data-ttu-id="fef89-116">Tato úprava měla v rámci hello [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="fef89-116">This modification should be performed within hello [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method.</span></span>

<span data-ttu-id="fef89-117">Hello následující příklad kódu ukazuje, jak toomodify hello cílové adresáře pro TEMP a TMP z v rámci metoda OnStart hello:</span><span class="sxs-lookup"><span data-stu-id="fef89-117">hello following code example shows how toomodify hello target directories for TEMP and TMP from within hello OnStart method:</span></span>

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // hello local resource declaration must have been added toothe
            // service definition file for hello role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // hello rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="fef89-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fef89-118">Next steps</span></span>
<span data-ttu-id="fef89-119">Přečtěte si blog, který popisuje [jak tooincrease hello velikost hello dočasnou složku ASP.NET pro Azure webové Role](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span><span class="sxs-lookup"><span data-stu-id="fef89-119">Read a blog that describes [How tooincrease hello size of hello Azure Web Role ASP.NET Temporary Folder](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span></span>

<span data-ttu-id="fef89-120">Zobrazení [řešení potíží s články](/?tag=top-support-issue&product=cloud-services) pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fef89-120">View more [troubleshooting articles](/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="fef89-121">toolearn jak problémy tootroubleshoot cloudové služby role pomocí Azure PaaS počítače diagnostická data zobrazit [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="fef89-121">toolearn how tootroubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, view [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
