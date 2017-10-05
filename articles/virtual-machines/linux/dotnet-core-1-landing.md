---
title: "Základní DotNet. virtuální počítač Azure Linux kurz 1 | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b3652e86-0c44-4ac9-8cd1-27abdeaea4d4
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 928d90f4bee593f04a41254b5f37d9328e59e05e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automating-application-deployments-to-linux-virtual-machines"></a>Automatizace nasazení aplikace na virtuální počítače s Linuxem 

Tato řada sestávající ze čtyř částí podrobné informace o nasazení a konfigurace prostředků Azure a aplikace pomocí šablony Azure Resource Manageru. V této série vzorové šablony nasazení a zkontrolován šablonu nasazení. Cílem této série je informovat o vztahu mezi prostředky Azure a stanovit rukou na zkušenosti s nasazením plně integrované šablony Azure Resource Manager. Tento dokument předpokládá základní úroveň znalostní báze pomocí Azure Resource Manageru, před zahájením tohoto kurzu Seznamte se se základními koncepty Azure Resource Manager. 

## <a name="music-store-application"></a>Hudba aplikaci store
Ukázka používá této série je .net Core aplikace simulaci Hudba obchod nakupování.. Tato aplikace se dá nasadit na virtuální prostředí Linux nebo Windows v systému, ukázkové nasazení byly vytvořeny pro obojí. Aplikace zahrnuje webovou aplikaci a databázi SQL. Než si přečtete články v této série, nasazení aplikace pomocí tlačítka nasazení najít na této stránce. Při nasazení plně, architektuře aplikace / Azure vypadá jako na následujícím diagramu. 

Šablony správce prostředků úložiště Hudba naleznete zde, [Hudba úložiště Linux šablony](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db)

![Aplikace Hudba úložiště](./media/dotnet-core-1-landing/music-store.png)

Každou z těchto součástí, včetně přidružení šablony JSON je zkontrolován v těchto článcích čtyři.

* [**Architektura aplikace** ](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – součásti aplikace, jako weby a databáze musí být hostované na prostředcích počítače Azure, jako je například virtuální počítače a databáze Azure SQL. Tento dokument vás provede mapování výpočetní potřeby, prostředků Azure a nasazení těchto prostředků pomocí šablony Azure Resource Manager. 
* [**Přístup a zabezpečení** ](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – při hostování aplikací v Azure, je potřeba vzít v úvahu jak přistupovat k aplikaci a jak různé součásti přístup k aplikaci navzájem. Tento dokument podrobně popisuje, poskytování a zabezpečení Internetu přístup k aplikaci a přístupu mezi jednotlivými součástmi aplikací.
* [**Dostupnost a dosah** ](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – dostupnost a dosah odkazy na aplikace umožňuje zůstat spuštěná během výpadků infrastruktury a možnost škálovat výpočetní prostředky ke splnění požadavků aplikace. Tento dokument podrobnosti komponent potřebných pro nasazení vyrovnáváním zatížení a aplikace s vysokou dostupností.
* [**Nasazení aplikace** ](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – Pokud je třeba zvážit nasazení aplikací na virtuálních počítačích Azure, metoda, podle kterého jsou nainstalovány binární soubory aplikace na virtuálním počítači. Tento dokument podrobně popisuje automatizace instalace aplikace pomocí přípony skriptu vlastní virtuální počítač Azure.

Cílem při vývoji šablony Azure Resource Manager je k automatizaci nasazení infrastruktury Azure a instalace a konfigurace všechny aplikace hostované na této infrastruktury Azure. Při absolvování těchto článcích poskytuje příklad toto prostředí.

## <a name="deploy-the-music-store-application"></a>Nasadit aplikaci store Hudba.
Hudba úložiště aplikací se dá nasadit pomocí tohoto tlačítka.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-linux%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Šablona Azure Resource Manager vyžaduje následující hodnoty parametrů.

| Název parametru | Popis |
| --- | --- |
| SSHKEYDATA |Data klíče SSH, používá k zabezpečení přístupu k virtuálnímu počítači. Informace o vytváření dvojici klíčů SSH naleznete v tématu [vytváření SSH klíče pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). |
| ADMINUSERNAME |Uživatelské jméno správce, který se používá na virtuální počítač a databáze SQL Azure. |
| SQLADMINPASSWORD |Heslo, které se používá v databázi SQL Azure. |
| NUMBEROFINSTANCES |Počet virtuálních počítačů, který se má vytvořit. Každý z těchto virtuálních počítačů hostovat aplikaci webové Hudba úložiště a veškerý provoz je vyrovnáváno zatížení napříč je. |
| PUBLICIPADDRESSDNSNAME |Globálně jedinečný název DNS související s veřejnou IP adresu. |

Po dokončení nasazení šablony, přejděte na veřejnou IP adresu pomocí libovolného prohlížeče internet. .Net zobrazí základní Hudba Web.

## <a name="next-steps"></a>Další kroky
<hr>

[Krok 1 – Architektura aplikace pomocí šablony Azure Resource Manageru](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Krok 2 – přístupu a zabezpečení v šablonách Azure Resource Manageru](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Krok 3 – dostupnost a dosah v šablonách Azure Resource Manageru](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Krok 4 – nasazení aplikace pomocí šablony Azure Resource Manageru](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

