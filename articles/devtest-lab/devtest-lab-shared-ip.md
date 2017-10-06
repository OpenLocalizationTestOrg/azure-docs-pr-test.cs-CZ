---
title: "aaaUnderstand sdílet IP adresy v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak Azure DevTest Labs využívá sdílený IP adresy toominimize hello veřejné IP adresy požadované tooaccess testovacího prostředí virtuálních počítačů."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: casoper
ms.openlocfilehash: 8756410117a9d550d567d372174bf1ea96703c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Pochopení sdílené IP adresy v Azure DevTest Labs

Umožňuje prostředí Azure DevTest Labs sdílenou složku virtuální počítače hello stejný veřejnou IP adresu toominimize hello počet veřejných IP adres vyžaduje tooaccess testovacího prostředí jednotlivé virtuální počítače.  Tento článek popisuje, jak sdílené pracovní IP adresy a jejich související možnosti konfigurace.

## <a name="shared-ip-setting"></a>Sdílené nastavení protokolu IP

Při vytváření testovacího prostředí se nachází v podsíti virtuální sítě.  Ve výchozím nastavení, tato podsíť je vytvořena s **povolení sdíleného veřejnou IP adresu** nastavit příliš*Ano*.  Tato konfigurace vytvoří jednu veřejnou IP adresu pro celou podsíť hello.  Další informace o konfiguraci virtuální sítě a podsítě, najdete v části [konfigurace virtuální sítě v Azure DevTest Labs](devtest-lab-configure-vnet.md).

![Novou podsíť testovacího prostředí](media/devtest-lab-shared-ip/lab-subnet.png)

Pro existující labs, můžete tuto možnost můžete povolit výběrem **konfiguraci a zásady > virtuální sítě**. Potom vyberte virtuální síť ze seznamu hello a zvolte **povolit SDÍLENÉ VEŘEJNOU IP adresu** pro vybranou podsíť. Tato možnost v jakékoli prostředí lze zakázat i pokud nechcete, aby tooshare veřejnou IP adresu v prostředí virtuálních počítačů.

Všechny virtuální počítače vytvořené v této laboratoře výchozí toousing sdílené IP.  Při vytváření hello virtuálních počítačů, toto nastavení může být dodržen v hello **upřesňující nastavení** okno pod **konfiguraci IP adresy**.

![Nový virtuální počítač](media/devtest-lab-shared-ip/new-vm.png)

- **Sdílené:** všechny virtuální počítače vytvořené jako **sdílené** se umístí do jedné skupiny prostředků (RG). Jedna IP adresa je přiřazen, pro který RG a všechny virtuální počítače v hello RG použije tuto IP adresu.
- **Veřejná:** každý virtuální počítač vytvoříte má svou vlastní IP adresu a je vytvořen v jeho vlastní skupiny prostředků.
- **Privátní:** každý virtuální počítač vytvoříte používá privátní IP adresy. Nebudete moct tooconnect toothis virtuálních počítačů přímo z hello Internetu pomocí vzdálené plochy.

Při každém přidání toohello podsíť virtuálního počítače pomocí sdíleného IP protokol povolen DevTest Labs automaticky přidá Vyrovnávání zatížení tooa hello virtuálních počítačů a přiřadí číslo portu TCP na hello veřejnou IP adresu předávání toohello portu RDP na hello virtuálních počítačů.  

## <a name="using-hello-shared-ip"></a>Používání hello sdíleného IP

- **Uživatelé Linux:** SSH toohello virtuálních počítačů pomocí hello IP adresu nebo plně kvalifikovaný název domény, následovaným dvojtečkou, za nímž následuje hello portu. Například v bitové kopii hello níže, hello RDP adres tooconnect toohello virtuálního počítače je `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![Příklad virtuálních počítačů](media/devtest-lab-shared-ip/vm-info.png)

- **Uživatelé systému Windows:** vyberte hello **Connect** tlačítko hello Azure portálu toodownload předem nakonfigurovaný soubor RDP a přístup k hello virtuálních počítačů.

## <a name="next-steps"></a>Další kroky

* [Definovat zásady testovacího prostředí v Azure DevTest Labs](devtest-lab-set-lab-policy.md)
* [Konfigurace virtuální sítě v Azure DevTest Labs](devtest-lab-configure-vnet.md)





