---
title: "aaaAzure Windows virtuální počítač DotNet základní kurz 1 | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 14d5f250-1f76-49d4-898f-07b58fd39e7c
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8df69c496f44acb02d8afc45695349ec1f558f99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automating-application-deployments-toowindows-virtual-machines"></a>Automatizace aplikace tooWindows nasazení virtuálních počítačů

Tato řada sestávající ze čtyř částí podrobné informace o nasazení a konfigurace prostředků Azure a aplikace pomocí šablony Azure Resource Manager. V této série vzorové šablony nasazení a hello zkontrolován šablonu nasazení. Cílem této série Hello je tooeducate na hello vztah mezi prostředky Azure a předá tooprovide na zkušenosti s nasazením plně integrované šablony Azure Resource Manager. Tento dokument předpokládá základní úroveň znalostní báze pomocí Azure Resource Manageru, před zahájením tohoto kurzu Seznamte se se základními koncepty Azure Resource Manager.

## <a name="music-store-application"></a>Hudba aplikaci store
Hello Ukázka používá této série je .net Core aplikace simulaci Hudba obchod nakupování.. Tato aplikace může být nasazený tooeither Linux nebo Windows virtuálního systému ukázkové, které byly vytvořeny pro obě nasazení. aplikace Hello zahrnuje webovou aplikaci a databázi SQL. Než si přečtete hello články v této série, nasazení aplikace hello pomocí tlačítka nasazení hello najít na této stránce. Při nasazení plně, hello aplikace / Azure architektura vypadá hello následující diagram. 

šablony správce prostředků úložiště Hudba Hello je zde uveden, [šablony Hudba úložiště systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Aplikace Hudba úložiště](./media/dotnet-core-1-landing/music-store.png)

Každou z těchto součástí, včetně hello přiřadit šablonu, kterou je zkontrolován JSON v hello následující čtyři články.

* [**Architektura aplikace** ](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – součásti aplikace, jako weby a databáze potřebovat toobe hostované na prostředcích počítače Azure, jako je například virtuální počítače a databáze Azure SQL. Tento dokument vás provede mapování výpočetní potřeba, tooAzure prostředky a nasazení těchto prostředků pomocí šablony Azure Resource Manager. 
* [**Přístup a zabezpečení** ](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – při hostování aplikací v Azure, je nutné tooconsider jak aplikace hello přistupuje, a jinou aplikaci komponent druhému přistupovat. Tento dokument podrobně popisuje, poskytování a zabezpečení aplikací tooan přístup k Internetu a přístupu mezi jednotlivými součástmi aplikací.
* [**Dostupnost a dosah** ](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – dostupnost a dosah naleznete toohello aplikace možnost toostay během výpadků infrastruktury, a hello možnost tooscale výpočetní prostředky toomeet aplikace vyžádání. Tento dokument podrobnosti hello součásti potřebné toodeploy skupinu s vyrovnáváním zatížení a aplikace s vysokou dostupností.
* [**Nasazení aplikace** ](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – při nasazení aplikací na virtuálních počítačích Azure, ve které hello binární soubory aplikace jsou nainstalovány na hello virtuálního počítače metoda hello je třeba zvážit. Tento dokument podrobně popisuje automatizace instalace aplikace pomocí přípony skriptu vlastní virtuální počítač Azure.

cílem Hello při vývoji šablony Azure Resource Manager je tooautomate hello nasazení infrastruktury Azure a hello instalace a konfigurace všechny aplikace hostované na této infrastruktury Azure. Při absolvování těchto článcích poskytuje příklad toto prostředí.

## <a name="deploy-hello-music-store-application"></a>Nasadit aplikaci store Hudba hello.
Hello Hudba úložiště aplikací se dá nasadit pomocí tohoto tlačítka.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Šablona Azure Resource Manager Hello vyžaduje hello následující hodnoty parametrů.

| Název parametru | Popis |
| --- | --- |
| ADMINUSERNAME |Uživatelské jméno správce, který se používá na hello virtuální počítač a hello Azure SQL Database. |
| ADMINPASSWORD |Heslo, který se používá na hello virtuální počítač Azure a SQL Database. |
| NUMBEROFINSTANCES |Hello počet virtuálních počítačů toobe vytvořili. Každý z těchto virtuálních počítačů hostitele hello Hudba úložiště webové aplikace a veškerý provoz je vyrovnáváno zatížení napříč je. |
| PUBLICIPADDRESSDNSNAME |Globálně jedinečný název DNS přidružené hello veřejnou IP adresu. |

Po dokončení nasazení šablony hello procházet toohello veřejných IP adres pomocí libovolného prohlížeče internet. Hello .net zobrazí základní Hudba Web.

## <a name="next-steps"></a>Další kroky
<hr>

[Krok 1 – Architektura aplikace pomocí šablony Azure Resource Manageru](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Krok 2 – přístupu a zabezpečení v šablonách Azure Resource Manageru](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Krok 3 – dostupnost a dosah v šablonách Azure Resource Manageru](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Krok 4 – nasazení aplikace pomocí šablony Azure Resource Manageru](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

