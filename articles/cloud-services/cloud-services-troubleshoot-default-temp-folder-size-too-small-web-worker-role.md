---
title: "Velikost výchozí dočasné složky je příliš malá pro role | Microsoft Docs"
description: "Cloudové služby role má omezené množství místa pro dočasné složky. Tento článek obsahuje některé návrhy na tom, jak zamezit nedostatku místa."
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
ms.openlocfilehash: 577d090a009eb2331b401273257c7cc7c1eea772
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a><span data-ttu-id="87693-104">Velikost výchozí dočasné složky je příliš malá v roli web nebo worker cloudové služby</span><span class="sxs-lookup"><span data-stu-id="87693-104">Default TEMP folder size is too small on a cloud service web/worker role</span></span>
<span data-ttu-id="87693-105">Dočasný adresář výchozí role pracovního procesu nebo webové služby cloud má maximální velikost 100 MB, který může dojít k úplné v určitém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="87693-105">The default temporary directory of a cloud service worker or web role has a maximum size of 100 MB, which may become full at some point.</span></span> <span data-ttu-id="87693-106">Tento článek popisuje, jak se vyhnout nedostatku místa pro tento dočasný adresář.</span><span class="sxs-lookup"><span data-stu-id="87693-106">This article describes how to avoid running out of space for the temporary directory.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a><span data-ttu-id="87693-107">Proč dochází volné místo?</span><span class="sxs-lookup"><span data-stu-id="87693-107">Why do I run out of space?</span></span>
<span data-ttu-id="87693-108">Standardní proměnné prostředí systému Windows TEMP a TMP jsou k dispozici pro kód, který běží ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="87693-108">The standard Windows environment variables TEMP and TMP are available to code that is running in your application.</span></span> <span data-ttu-id="87693-109">TEMP a TMP bodu do jednoho adresáře, který má maximální velikost 100 MB.</span><span class="sxs-lookup"><span data-stu-id="87693-109">Both TEMP and TMP point to a single directory that has a maximum size of 100 MB.</span></span> <span data-ttu-id="87693-110">Žádná data, která je uložená v tomto adresáři není zachován po životního cyklu cloudové služby; Pokud jsou recyklovány instance rolí v cloudové službě, je vyčistit adresář.</span><span class="sxs-lookup"><span data-stu-id="87693-110">Any data that is stored in this directory is not persisted across the lifecycle of the cloud service; if the role instances in a cloud service are recycled, the directory is cleaned.</span></span>

## <a name="suggestion-to-fix-the-problem"></a><span data-ttu-id="87693-111">Návrh na opravě problému</span><span class="sxs-lookup"><span data-stu-id="87693-111">Suggestion to fix the problem</span></span>
<span data-ttu-id="87693-112">Implementujte jednu z následujících alternativních:</span><span class="sxs-lookup"><span data-stu-id="87693-112">Implement one of the following alternatives:</span></span>

* <span data-ttu-id="87693-113">Nakonfigurujte místní úložiště prostředků a k němu přístup přímo místo použití TEMP a TMP.</span><span class="sxs-lookup"><span data-stu-id="87693-113">Configure a local storage resource, and access it directly instead of using TEMP or TMP.</span></span> <span data-ttu-id="87693-114">Chcete-li získat přístup k prostředku Místní úložiště z kódu, který běží v rámci vaší aplikace, zavolejte [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="87693-114">To access a local storage resource from code that is running within your application, call the [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>
* <span data-ttu-id="87693-115">Nakonfigurujte místní úložiště prostředků a bodu adresáře TEMP a TMP tak, aby odkazoval na cestu prostředků místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="87693-115">Configure a local storage resource, and point the TEMP and TMP directories to point to the path of the local storage resource.</span></span> <span data-ttu-id="87693-116">Tato úprava měla v rámci [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="87693-116">This modification should be performed within the [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method.</span></span>

<span data-ttu-id="87693-117">Následující příklad kódu ukazuje, jak můžete upravit cíl adresáře TEMP a TMP z v rámci metody OnStart:</span><span class="sxs-lookup"><span data-stu-id="87693-117">The following code example shows how to modify the target directories for TEMP and TMP from within the OnStart method:</span></span>

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
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

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="87693-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87693-118">Next steps</span></span>
<span data-ttu-id="87693-119">Přečtěte si blog, který popisuje [jak zvětšit velikost Azure webovou roli ASP.NET dočasnou složku](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span><span class="sxs-lookup"><span data-stu-id="87693-119">Read a blog that describes [How to increase the size of the Azure Web Role ASP.NET Temporary Folder](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span></span>

<span data-ttu-id="87693-120">Zobrazení [řešení potíží s články](/?tag=top-support-issue&product=cloud-services) pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="87693-120">View more [troubleshooting articles](/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="87693-121">Informace o tom potíží cloudové služby role pomocí Azure PaaS počítače diagnostická data, zobrazit [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="87693-121">To learn how to troubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, view [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
