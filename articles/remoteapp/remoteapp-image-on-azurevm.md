---
title: "Vytvoření image Azure Remoteappu založené na virtuálním počítači Azure | Microsoft Docs"
description: "Naučte se vytvořit bitovou kopii pro Azure RemoteApp od virtuálního počítače Azure."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d41583ef-6cd8-4115-8dcb-b2cd5b3d301a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ee64b86835af8e6237cddcd8acc779fc6dac8214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Vytvoření image Azure Remoteappu založené na virtuální počítač Azure
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Z virtuálního počítače Azure můžete vytvořit Image Azure Remoteappu, (které s aplikacemi, které sdílíte v kolekci). Může se také rozhodnout použít bitovou kopii virtuálního počítače, kterou jsme přidali do Galerie bitové kopie virtuálního počítače Azure, který splňuje všechny požadavky image Azure Remoteappu – můžete použít této bitové kopie virtuálních počítačů jako výchozí bod pro vlastní virtuální počítač, pokud chcete. Právě hledejte bitovou kopii "Windows Server hostitele relace vzdálené plochy" v knihovně.

Existují dva kroky pro vytvoření vlastní image založené na virtuální počítač Azure – vytvoření bitové kopie a nahrajte ho z knihovny virtuálního počítače Azure do Azure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Vytvořit vlastní image založené na virtuálním počítači Azure
Tyto kroky použijte pro vytvoření bitové kopie založené na virtuálním počítači Azure.

1. Vytvořte virtuální počítač Azure. Můžete použít "Windows Server hostitele relace vzdálené plochy" nebo "Windows Server vzdálené plochy relace hostitele s Microsoft Office 365 ProPlus" bitové kopie z Galerie obrázků virtuální počítač Azure. Tento image splňuje všechny požadavky image Azure Remoteappu šablony.
   
    Podrobnosti najdete v tématu [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Připojení k virtuálnímu počítači a nainstalujte a nakonfigurujte aplikace, které chcete sdílet prostřednictvím vzdálené aplikace RemoteApp. Ujistěte se, že provádět žádné další konfigurace Windows vyžaduje vaše aplikace.
   
    Podrobnosti najdete v tématu [postup Přihlaste se k Windows serveru spuštěný virtuální počítač](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Pokud použijete jednu z imagí hostitele pro relace vzdálené plochy systému Windows Server, je ověření zahrnuté skript, který zajistí, že virtuální počítač splňuje pre-reqs. vzdálené aplikace RemoteApp Chcete-li spustit skript, dvakrát klikněte na **ValidateRemoteAppImage** na ploše. Ujistěte se, zda jsou před pokračováním k dalšímu kroku opravit všechny chyby, které skript nahlásí.
4. Nástroj SYSPREP generalize a zachycení bitové kopie. V tématu [jak zachytit virtuální počítač Windows pro použití jako šablona](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) pokyny.

## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Import bitovou kopii do bitové kopie knihovny Azure Remoteappu
Importovat novou bitovou kopii do Azure RemoteApp pomocí těchto kroků:

1. V **Image šablony** karty:
   
   * Pokud máte existující se žádné Image, klikněte na tlačítko **nahrát nebo importovat Image šablony**.
   * Pokud již máte alespoň jednu bitovou kopii, klikněte na tlačítko  **+**  přidat novou bitovou kopii.
2. Vyberte **importovat bitovou kopii z virtuálních počítačů** knihovny a pak klikněte na tlačítko **Další**.
3. Na další stránce ze seznamu vyberte vlastní image a potvrďte, že jste postupovali podle kroků uvedených při vytváření bitové kopie. Klikněte na **Další**.
4. Zadejte název pro novou bitovou kopii vzdálené aplikace RemoteApp a vyberte umístění a pak kliknutím na značku zaškrtnutí zahájíte proces importu.

> [!NOTE]
> Bitové kopie můžete importovat z libovolného místa a Azure podporována ve virtuálních počítačích Azure do libovolného umístění Azure, Azure RemoteApp nepodporuje. V závislosti na umístění importu může trvat až 25.
> 
> 

Nyní jste připraveni k vytvoření nové kolekce, buď [cloudu](remoteapp-create-cloud-deployment.md) kolekce nebo [hybridní](remoteapp-create-hybrid-deployment.md), v závislosti na vašich potřebách.

