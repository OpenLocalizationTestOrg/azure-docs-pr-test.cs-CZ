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
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Velikost výchozí dočasné složky je příliš malá v roli web nebo worker cloudové služby
Výchozí Hello dočasný adresář role pracovního procesu nebo webové služby cloud má maximální velikost 100 MB, který může dojít k úplné v určitém okamžiku. Tento článek popisuje, jak tooavoid volné místo pro dočasný adresář hello.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Proč dochází volné místo?
Hello standardní Windows proměnné prostředí TEMP a TMP jsou k dispozici toocode, který běží ve vaší aplikaci. TEMP a TMP bodu tooa jeden adresář, který má maximální velikost 100 MB. Žádná data, která je uložená v tomto adresáři není zachován po životního cyklu hello hello cloudové služby; Pokud jsou recyklovány hello instance rolí v cloudové službě, je vyčistit adresář hello.

## <a name="suggestion-toofix-hello-problem"></a>Návrh toofix hello problém
Implementujte jednu z následujících alternativních hello:

* Nakonfigurujte místní úložiště prostředků a k němu přístup přímo místo použití TEMP a TMP. tooaccess místní úložiště prostředků z kódu, který běží v rámci vaší aplikace, volání hello [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metoda.
* Nakonfigurujte místní úložiště prostředků a bodu hello TEMP a TMP adresáře toopoint toohello cesta prostředku hello místní úložiště. Tato úprava měla v rámci hello [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoda.

Hello následující příklad kódu ukazuje, jak toomodify hello cílové adresáře pro TEMP a TMP z v rámci metoda OnStart hello:

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

## <a name="next-steps"></a>Další kroky
Přečtěte si blog, který popisuje [jak tooincrease hello velikost hello dočasnou složku ASP.NET pro Azure webové Role](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Zobrazení [řešení potíží s články](/?tag=top-support-issue&product=cloud-services) pro cloudové služby.

toolearn jak problémy tootroubleshoot cloudové služby role pomocí Azure PaaS počítače diagnostická data zobrazit [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
