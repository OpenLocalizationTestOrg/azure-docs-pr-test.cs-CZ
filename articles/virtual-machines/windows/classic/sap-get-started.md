---
title: "aaaUsing SAP na virtuálních počítačích s Windows | Microsoft Docs"
description: "Zrušte o používání SAP na Windows virtuální počítače (VM) v Microsoft Azure"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: 1b455be4-c02f-43e3-8d39-c2d5f216e646
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 6c4d8a066a4a8805668e78e67fd2110f2000ee75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Pomocí SAP na virtuálních počítačích s Windows v Azure
Cloud Computing je často používaný termín, který je získání více význam v rámci hello IT odvětví, z malých společností až toolarge a mezinárodních společnosti. Microsoft Azure je hello cloudové služby společnosti Microsoft, který nabízí široké spektrum nové možnosti. Nyní zákazníci jsou zřídit a deaktivace zřízení aplikací mít toorapidly jako cloudové služby, takže nejsou omezené tootechnical nebo rozpočet omezení. Společnosti můžete místo času a investovat do hardwaru infrastruktury, soustředit na aplikace hello, podnikové procesy a jeho výhody pro zákazníky a uživatele.

S virtuálními počítači Microsoft Azure společnost Microsoft nabízí komplexní infrastruktury jako služby (IaaS) platformu. Služba Azure Virtual Machines teď v rámci IaaS podporuje aplikace využívající SAP NetWeaver. Hello dokumenty White Paper níže popisují, jak tooplan a implementujte SAP NetWeaver na základě aplikací na virtuálních počítačích s Windows v Azure. Můžete taky implementovat SAP NetWeaver na základě aplikací na [virtuální počítače s Linuxem](../../linux/classic/sap-get-started.md).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver ve službě Azure - HA
Title: aaaSAP NetWeaver ve službě Azure - Clustering SAP ASC nebo SCS instancí pomocí clusteru převzetí služeb při selhání systému Windows Server v Azure s DataKeeper s

Souhrn: ' Tento dokument popisuje, jak toouse s DataKeeper tooset až SAP ASC nebo SCS konfiguraci s vysokou dostupností v Azure. SAP chrání jejich jediný bod selhání komponenty, jako jsou služby zařazování replikace nebo SAP ASC nebo SCS s konfigurace clusteru převzetí služeb při selhání systému Windows Server, které vyžadují sdílené disky. Tyto součásti SAP jsou nezbytné pro funkce hello systému SAP. Proto potřebám funkce vysoké dostupnosti, kterou toobe vložit umístit toomake, že tyto součásti tolerovat selhání serveru nebo na virtuální počítač jako dokončení konfigurace clusteru se systémem Windows pro úplné obnovení a prostředí Hyper-V. K srpnu 2015 nemůže Azure sám na sobě poskytovat vysoce dostupné konfigurace požadované pro tyto důležité součásti SAP základě sdílených disků, které by byla zapotřebí pro hello Windows. Ale pomocí hello produktu hello DataKeeper podle SIOS konfigurace clusteru převzetí služeb při selhání Windows serveru podle potřeby pro SAP ASC nebo SCS se dají vytvářet na platformě Azure IaaS hello. Tento dokument popisuje v kroku krok přístupu, jak tooinstall konfigurace clusteru převzetí služeb při selhání systému Windows Server s sdíleného disku poskytované s Datakeeper v Azure. Hello dokument vysvětluje podrobnosti v konfigurace na straně hello Azure, Windows a SAP, což konfiguraci vysoké dostupnosti hello fungovat optimální způsobem nastavit. zadané platformy Hello dokumentu doplňuje hello SAP instalace dokumentace a poznámky k SAP, které představují hello primární prostředky pro instalace a nasazení softwaru SAP na.

Aktualizované: Srpen 2015

[Stáhnout teď tuto příručku](http://go.microsoft.com/fwlink/?LinkId=613056)

