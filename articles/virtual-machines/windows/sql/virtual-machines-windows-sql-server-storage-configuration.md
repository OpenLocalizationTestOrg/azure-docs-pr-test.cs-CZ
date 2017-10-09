---
title: "aaaStorage konfiguraci pro virtuální počítače SQL serveru | Microsoft Docs"
description: "Toto téma popisuje, jak Azure konfiguruje úložiště pro virtuální počítače SQL serveru při zřizování (modelu nasazení Resource Manager). Také vysvětluje, jak konfigurace úložiště pro existující virtuální počítače SQL serveru."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: ninarn
ms.openlocfilehash: b50dbd698828780cfc044fa0966e8f4e2f3bb6c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storage-configuration-for-sql-server-vms"></a>Konfiguraci úložiště pro virtuální počítače serveru SQL
Když konfigurujete image virtuálního počítače systému SQL Server v Azure, pomáhá hello portál tooautomate konfiguraci úložiště. To zahrnuje připojení toohello úložiště virtuálních počítačů, provedení tohoto úložiště přístupné tooSQL serveru a nakonfigurovat jej toooptimize pro požadavky vaší konkrétní výkonu.

Toto téma vysvětluje, jak nakonfiguruje Azure úložiště pro virtuální počítače SQL serveru při zřizování i pro existující virtuální počítače. Tato konfigurace je založená na hello [osvědčené postupy z hlediska výkonu](virtual-machines-windows-sql-performance.md) pro virtuální počítače Azure se systémem SQL Server.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Požadavky
toouse hello automatizované nastavení konfigurace úložiště, virtuální počítač vyžaduje hello následující vlastnosti:

* Zřízená [image SQL serveru Galerie](virtual-machines-windows-sql-server-iaas-overview.md#option-1-create-a-sql-vm-with-per-minute-licensing).
* Hello používá [modelu nasazení Resource Manager](../../../azure-resource-manager/resource-manager-deployment-model.md).
* Používá [Storage úrovně Premium](../../../storage/common/storage-premium-storage.md).

## <a name="new-vms"></a>Nové virtuální počítače
Hello následující části popisují, jak tooconfigure úložiště pro nové virtuální počítače systému SQL Server.

### <a name="azure-portal"></a>Azure Portal
Při zřizování virtuálního počítače Azure pomocí Galerie bitovou kopii systému SQL Server, můžete zvolit tooautomatically konfigurace hello úložiště pro nový virtuální počítač. Určete velikost úložiště hello, výkon a typ úlohy. Hello následující snímek obrazovky ukazuje okno Konfigurace úložiště hello používá během virtuální počítač SQL zřizování.

![Konfigurace úložiště virtuálního počítače SQL serveru při zřizování](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Podle vaší volby, provede Azure hello následující úkoly konfigurace úložiště po vytvoření hello virtuálních počítačů:

* Vytvoří a připojí premium úložiště dat disky toohello virtuálního počítače.
* Nakonfiguruje hello datové disky toobe přístupné tooSQL serveru.
* Nakonfiguruje hello datových disků do úložiště fondu podle hello zadané požadavky na velikost a výkon (IOPS a propustnost).
* Přidruží hello fondu úložiště na nový disk ve virtuálním počítači hello.
* Optimalizuje tento nový disk, na základě vašeho typu zadané úlohy (datové sklady, zpracování transakcí nebo Obecné).

Další informace o tom, jak Azure nakonfiguruje nastavení úložiště najdete v tématu hello [úložiště konfigurační oddíl](#storage-configuration). Úplné návod, jak toocreate virtuální počítač SQL Server v hello portálu Azure, najdete v části [hello zřizování kurzu](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Spravovat šablony prostředků
Pokud používáte hello následující šablony Resource Manageru, jsou připojené dva premium datových disků ve výchozím nastavení se žádné konfigurace fondu úložiště. Ale můžete přizpůsobit tyto šablony toochange hello počet datových disků premium, které jsou připojené toohello virtuálního počítače.

* [Vytvoření virtuálního počítače s automatizované zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Vytvoření virtuálního počítače s automatizované opravy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [Vytvoření virtuálního počítače s integrace se službou AZURE](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Existující virtuální počítače
Pro existující virtuální počítače SQL serveru můžete upravit některá nastavení úložiště v hello portálu Azure. Vyberte virtuální počítač, přejděte toohello nastavení oblasti a pak vyberte konfiguraci systému SQL Server. okno Konfigurace serveru SQL Server Hello ukazuje aktuální využití úložiště hello vašeho virtuálního počítače. V tomto grafu se zobrazují všechny jednotky, které existují na vašem virtuálním počítači. Pro každou jednotku hello prostor úložiště zobrazí v čtyři části:

* SQL data
* Protokolu databáze SQL
* Jiné (mimo SQL úložiště)
* Dostupné

![Konfigurace úložiště pro existující systém SQL Server virtuálního počítače](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

tooconfigure hello tooadd úložiště na nový disk nebo rozšířit existující jednotce, klikněte na odkaz pro úpravy hello výše hello grafu.

Hello možnosti konfigurace, které vidíte, se liší v závislosti na tom, jestli používáte tuto funkci před. Pokud používáte pro hello poprvé, můžete určit požadavky na úložiště pro nový disk. Pokud jste dříve používali toocreate této funkce na jednotku, můžete vybrat tooextend této jednotky úložiště.

### <a name="use-for-hello-first-time"></a>Použití pro hello poprvé
Pokud je poprvé pomocí této funkce, můžete zadat hello omezení velikosti a výkonu úložiště pro nový disk. Toto prostředí je podobné toowhat, zobrazí se při zřizování čas. Hello hlavní rozdíl je, že nejsou povolené toospecify hello zatížení typu. Toto omezení zabrání přerušení všechny existující konfigurace systému SQL Server na virtuálním počítači hello.

![Konfigurace SQL serveru úložiště posuvníky](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure vytvoří na nový disk podle vašich požadavků. V tomto scénáři Azure provádí následující úkoly konfigurace úložiště hello:

* Vytvoří a připojí premium úložiště dat disky toohello virtuálního počítače.
* Nakonfiguruje hello datové disky toobe přístupné tooSQL serveru.
* Nakonfiguruje hello datových disků do úložiště fondu podle hello zadané požadavky na velikost a výkon (IOPS a propustnost).
* Přidruží hello fondu úložiště na nový disk ve virtuálním počítači hello.

Další informace o tom, jak Azure nakonfiguruje nastavení úložiště najdete v tématu hello [úložiště konfigurační oddíl](#storage-configuration).

### <a name="add-a-new-drive"></a>Přidat nový disk
Pokud jste již nakonfigurovali úložiště na virtuální počítač s SQL serverem, rozšíření úložiště zobrazí dvě nové možnosti. první možnost Hello je tooadd na nový disk, což může zvýšit úroveň výkonu hello vašeho virtuálního počítače.

![Přidat nový disk tooa virtuální počítač SQL](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Po přidání hello disku, ale musíte provést zvýšení výkonu hello tooachieve některé velmi ruční konfigurace.

### <a name="extend-hello-drive"></a>Rozšíření hello jednotky
Hello další možnost pro rozšíření úložiště je tooextend hello existující jednotce. Tato možnost zvyšuje hello úložiště k dispozici pro vašeho disku, ale nezvyšuje výkon. Díky fondům úložiště nelze změnit hello počet sloupců, po vytvoření fondu úložiště hello. Hello počet sloupců určuje hello počet paralelní zápisy, které může být rozdělená mezi hello datových disků. Proto všechny disky přidané dat nelze zvýšit výkon. Můžete pouze poskytují další úložiště pro data hello zapisovaný. Toto omezení také znamená, že vzhledem k tomu, když rozšíření hello jednotky, hello počet sloupců určuje hello minimální počet datových disků, které je možné přidat. Proto pokud vytvoříte fond úložiště s čtyři datových disků, hello počet sloupců je také čtyři. Kdykoli můžete rozšířit hello úložiště, je nutné přidat alespoň čtyři datových disků.

![Rozšíření pro virtuální počítač SQL na jednotku](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Konfigurace úložiště
Tato část obsahuje odkaz pro hello změny konfigurace úložiště, které Azure automaticky provede při zřizování virtuálních počítačů SQL nebo konfigurace v hello portálu Azure.

* Pokud vyberete méně než dvě TBs úložiště pro virtuální počítač Azure nevytvoří fondu úložiště.
* Pokud vyberete aspoň dva TBs úložiště pro virtuální počítač Azure nakonfiguruje fond úložiště. Hello další části tohoto tématu poskytuje hello podrobnosti o konfiguraci fondu úložiště hello.
* Konfigurace automatického úložiště vždy používá [storage úrovně premium](../../../storage/common/storage-premium-storage.md) P30 datových disků. Proto je mapování 1:1 mezi vaší vybraný počet terabajtů a hello počet datových disků připojených tooyour virtuálních počítačů.

Informace o cenách naleznete v tématu hello [Storage – ceny](https://azure.microsoft.com/pricing/details/storage) stránky na hello **diskového úložiště** kartě.

### <a name="creation-of-hello-storage-pool"></a>Vytvoření fondu úložiště hello
Azure používá hello následující nastavení toocreate hello úložiště fondu na virtuálních počítačích SQL Server.

| Nastavení | Hodnota |
| --- | --- |
| Velikost stripe |256 KB (datové sklady); 64 KB (transakcí) |
| Velikost disků |1 TB |
| Mezipaměť |Čtení |
| Velikost alokační |Velikost alokační jednotky 64 KB systému souborů NTFS |
| Inicializace rychlých souboru |Povoleno |
| Zamknout stránky paměti |Povoleno |
| Obnovení |Obnovení Simple (bez odolnosti) |
| Počet sloupců |Počet datových disků<sup>1</sup> |
| Umístění databáze TempDB |Uložené na datové disky<sup>2</sup> |

<sup>1</sup> po vytvoření fondu úložiště hello nelze změnit hello počet sloupců ve fondu úložiště hello.

<sup>2</sup> toto nastavení platí jenom toohello první disk můžete vytvořit pomocí funkce hello úložiště konfigurace.

## <a name="workload-optimization-settings"></a>Nastavení úloh optimalizace
Hello následující tabulka popisuje hello tři zatížení typu dostupných možností a jejich odpovídající optimalizace:

| Typ úlohy | Popis | Optimalizace |
| --- | --- | --- |
| **Obecné** |Výchozí nastavení, která podporuje většinu úloh |Žádný |
| **Zpracování transakcí** |Optimalizuje hello úložiště pro standardní úlohy databází OLTP |Příznak trasování. 1117<br/>Příznak trasování 1118 |
| **Datového skladu** |Optimalizuje hello úložiště pro úlohy analýz a generování sestav |Příznak trasování 610<br/>Příznak trasování. 1117 |

> [!NOTE]
> Typ zatížení hello můžete zadat pouze při zřizování virtuálního počítače systému SQL tak, že vyberete v kroku konfigurace úložiště hello.
>
>

## <a name="next-steps"></a>Další kroky
Další témata související s toorunning systému SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
