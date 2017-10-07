---
title: "aaaCreate na virtuální počítač Azure na základě image Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak toocreate obrázek pro Azure RemoteApp od virtuálního počítače Azure."
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
ms.openlocfilehash: 2d432bcb15be68a2946d91b5f36f41d980726338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Vytvoření image Azure Remoteappu založené na virtuální počítač Azure
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Z virtuálního počítače Azure můžete vytvořit Image Azure Remoteappu, (které uložení hello aplikace, které sdílíte v kolekci). Může také vybrat toouse jsme přidali toohello Galerie obrázků virtuálního počítače Azure, která splňuje všechny požadavky na image Azure Remoteappu hello image virtuálního počítače – můžete použít této bitové kopie virtuálních počítačů jako výchozí bod pro vlastní virtuální počítač, pokud chcete. Vyhledejte právě hello "Windows Server hostitele relace vzdálené plochy" image v hello knihovně.

Existují dva kroky toocreate vlastní image založené na virtuální počítač Azure – vytvoření hello bitové kopie a nahrajte ho z hello virtuálního počítače Azure knihovna tooAzure vzdálené aplikace RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Vytvořit vlastní image založené na virtuálním počítači Azure
Pomocí těchto kroků toocreate image založené na virtuálním počítači Azure.

1. Vytvořte virtuální počítač Azure. Můžete použít hello "Windows Server hostitele relace vzdálené plochy" nebo "Windows Server vzdálené plochy relace hostitele s Microsoft Office 365 ProPlus" image hello z Galerie obrázků hello virtuální počítač Azure. Tuto bitovou kopii splňuje požadavky image šablony vzdálené aplikace Azure RemoteApp na všechny hello.
   
    Podrobnosti najdete v tématu [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Připojit toohello virtuálních počítačů a nainstalujte a nakonfigurujte hello aplikace, které chcete tooshare prostřednictvím vzdálené aplikace RemoteApp. Ujistěte se, že tooperform žádné další konfigurace Windows vyžaduje vaše aplikace.
   
    Podrobnosti najdete v tématu [jak tooLog na tooa virtuální počítač spuštěný Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Pokud použijete jednu z imagí hello hostitele pro relace vzdálené plochy systému Windows Server, je ověření zahrnuté skript, který zajistí, že virtuální počítač splňuje hello vzdálené aplikace RemoteApp pre-reqs. toorun skriptu, klikněte dvakrát na **ValidateRemoteAppImage** na ploše hello. Ujistěte se, zda jsou před pokračováním toohello další krok opravit všechny chyby hlášené hello skriptu.
4. Nástroj SYSPREP generalize a zachycení bitové kopie hello. V tématu [jak tooCapture tooUse virtuálního počítače Windows jako šablona](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) pokyny.

## <a name="import-hello-image-into-hello-azure-remoteapp-image-library"></a>Importovat hello bitovou kopii do knihovny image Azure Remoteappu hello
Použijte tyto kroky tooimport hello novou bitovou kopii do Azure RemoteApp:

1. V hello **Image šablony** karty:
   
   * Pokud máte existující se žádné Image, klikněte na tlačítko **nahrát nebo importovat Image šablony**.
   * Pokud již máte alespoň jednu bitovou kopii, klikněte na tlačítko  **+**  tooadd novou bitovou kopii.
2. Vyberte **importovat bitovou kopii z virtuálních počítačů** knihovny a pak klikněte na tlačítko **Další**.
3. Na další stránku hello vyberte ze seznamu hello vlastní image a potvrďte, že jste udělali kroky hello uvedené při vytváření bitové kopie. Klikněte na **Další**.
4. Zadejte název nové image vzdálené aplikace RemoteApp hello a vyberte hello umístění a pak klikněte na hello zaškrtnutí toostart hello importu.

> [!NOTE]
> Bitové kopie můžete importovat z libovolného místa a Azure nepodporuje virtuální počítače Azure tooany umístění Azure, které podporuje Azure RemoteApp. V závislosti na umístění hello hello importu může trvat too25 minut.
> 
> 

Nyní jste připraveni toocreate nové kolekce, buď [cloudu](remoteapp-create-cloud-deployment.md) kolekce nebo [hybridní](remoteapp-create-hybrid-deployment.md), v závislosti na vašich potřebách.

