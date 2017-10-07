---
title: "aaaExample návod infrastruktury Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení infrastruktury příklad v Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd6b6e904404bea0b5be37dfce6d60039daae636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a>Příklad infrastruktury Azure návod pro virtuální počítače Windows

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Tento článek vás provede vytváření infrastruktury příklad aplikace. Jsme podrobnosti navrhování infrastruktury pro jednoduché online obchodu, která spojuje všechny hello pokyny a rozhodnutí, která kolem názvů, skupiny dostupnosti, virtuální sítě a nástroje pro vyrovnávání zatížení a ve skutečnosti nasazení virtuálních počítačů (VM).

## <a name="example-workload"></a>Příklad úloh
Společnosti Adventure Works Cycles chce toobuild aplikace online úložiště v Azure, který se skládá z:

* Dva servery služby IIS se spuštěnou hello klienta front-endu v webovou vrstvu
* Zpracování dat a objednávky v aplikační vrstvě dva servery služby IIS
* Dvě instance Microsoft SQL Server se skupinami dostupnosti AlwaysOn (dva servery SQL a určujícího uzlu většina) pro ukládání dat produktu a objednávky v databázové vrstvy
* Dva řadiče domény služby Active Directory pro účty zákazníků a všichni dodavatelé v vrstvou ověřování
* Všechny servery hello se nacházejí v dvě podsítě:
  * podsíť front-end pro webové servery hello 
  * back-end podsíť pro aplikační servery hello clusteru SQL a řadiče domény

![Diagram různých vrstev pro infrastrukturu aplikace](./media/infrastructure-example/example-tiers.png)

Zabezpečené příchozí webové přenosy musí být vyrovnávání zatížení mezi hello webové servery jako zákazníci procházet hello online úložiště. Pořadí zpracování přenosů dat ve formuláři hello HTTP požadavků z webové hello, servery musí být rozložena mezi hello aplikační servery. Kromě toho musí být navrženy hello infrastruktury pro vysokou dostupnost.

Výsledný návrhu Hello musí obsahovat:

* Předplatné Azure a účet
* Jedna skupina prostředků
* Azure Managed Disks
* Virtuální síť se dvěma podsítěmi
* Sady dostupnosti pro hello virtuálních počítačů s podobnou roli
* Virtuální počítače

Všechny výše uvedené hello postupujte tyto zásady vytváření názvů:

* Adventure Works Cycles používá **[IT zatížení]-[umístění] – [prostředků Azure]** jako předponu
  * V tomto příkladu "**azos**" (online úložiště Azure) je hello název úlohy IT a "**použít**" (východní USA 2) je umístění hello
* Virtuální sítě pomocí AZOS. POUŽIJTE VN**[číslo]**
* Pomocí sad dostupnosti azos-použít-jako-**[role]**
* Názvy virtuálních počítačů pomocí azos-použít-vm -**[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Předplatná Azure a účty
Společnosti Adventure Works Cycles je pomocí své podnikové předplatné s názvem společnosti Adventure Works podnikové předplatné, tooprovide fakturace pro tuto úlohu IT.

## <a name="storage"></a>Úložiště
Společnosti Adventure Works Cycles určit, že by měli používat Azure spravované disky. Při vytváření virtuálních počítačů, se používají i vrstvy úložiště k dispozici úložiště:

* **Standardní úložiště** pro hello webové servery, aplikační servery a řadiče domény a jejich datových disků.
* **Storage úrovně Premium** pro virtuální počítače hello SQL serveru a jejich datových disků.

## <a name="virtual-network-and-subnets"></a>Virtuální sítě a podsítě
Protože hello virtuální sítě nemusí probíhající připojení toohello Adventure pracovní Cycles do místní sítě, budou se rozhodla ve virtuální síti jenom pro cloud.

Vytvářely jenom pro cloud virtuální síť s hello následující nastavení pomocí portálu Azure hello:

* Název: AZOS-použití VN01
* Umístění: Východní USA 2
* Adresní prostor virtuální sítě: 10.0.0.0/8
* První podsíť:
  * Název: front-endu
  * Adresní prostor: 10.0.1.0/24
* Druhou podsíť:
  * Název: back-end
  * Adresní prostor: 10.0.2.0/24

## <a name="availability-sets"></a>Skupiny dostupnosti
toomaintain vysokou dostupnost všechny čtyři úrovně jejich online úložiště společnosti Adventure Works Cycles rozhodla na čtyři skupiny dostupnosti:

* **azos použít jako webový** pro webové servery hello
* **azos používání jako aplikace** pro hello aplikační servery
* **azos použijte jako sql** pro servery SQL Server hello
* **azos použijte jako dc** pro řadiče domény hello

## <a name="virtual-machines"></a>Virtuální počítače
Společnosti Adventure Works Cycles se rozhodli hello následující názvy pro své virtuální počítače Azure:

* **azos použití virtuálních počítačů web01** pro první webový server hello
* **azos použití virtuálních počítačů web02** pro hello druhého webového serveru
* **azos použití virtuálních počítačů app01** pro první aplikační server hello
* **azos použití virtuálních počítačů app02** pro druhý server aplikace hello
* **azos použití virtuálních počítačů sql01** pro hello první server pro systém SQL Server v clusteru hello
* **azos použití virtuálních počítačů sql02** pro hello druhý server SQL Server v clusteru hello
* **azos použití virtuálních počítačů dc01** pro první řadič domény hello
* **azos použití virtuálních počítačů dc02** pro řadič domény druhé hello

Zde je hello Výsledná konfigurace.

![Infrastruktura konečné aplikace nasazené v Azure](./media/infrastructure-example/example-config.png)

Tato konfigurace zahrnuje:

* Čistě cloudové virtuální síť se dvěma podsítěmi (front-endové a back-end)
* Disky Azure spravované s disky, Standard a Premium
* Čtyři skupiny dostupnosti, jeden pro každou vrstvu úložiště online hello
* Hello virtuálních počítačů pro hello čtyři úrovně
* Externí s vyrovnáváním zatížení založený na protokolu HTTPS webových přenosů z hello Internet toohello webové servery
* K interní s vyrovnáváním zatížení nezašifrované webových přenosů z hello webové servery toohello aplikačních serverů
* Jedna skupina prostředků