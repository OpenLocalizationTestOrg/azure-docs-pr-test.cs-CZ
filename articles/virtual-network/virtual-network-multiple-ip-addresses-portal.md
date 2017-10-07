---
title: "aaaMultiple IP adresy pro virtuální počítače Azure - Portal | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí hello portálu Azure | Správce prostředků."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
ms.openlocfilehash: 34075766ac68c8de38c258a4d70e35881f28bb0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-portal"></a>Přiřadit více IP adres toovirtual počítačů pomocí hello portálu Azure

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
Tento článek vysvětluje, jak hello toocreate virtuální počítač (VM) pomocí modelu nasazení Azure Resource Manager hello pomocí portálu Azure. Tooresources vytvořené pomocí modelu nasazení classic hello nelze přiřadit více IP adres. Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Vytvoření virtuálního počítače s více IP adres

Pokud chcete toocreate virtuálního počítače s více IP adres nebo statickou privátní IP adresu, musíte vytvořit pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku Azure. Klikněte na tlačítko hello prostředí PowerShell nebo rozhraní příkazového řádku možnosti hello horní části tohoto článku toolearn jak. Virtuální počítač můžete vytvořit pomocí jediné dynamické privátní IP adresy a (volitelně) jednu veřejnou IP adresu pomocí hello portálu podle následujících kroků hello v hello [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) nebo [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-portal.md) články. Po vytvoření hello virtuálních počítačů, můžete změnit typ hello IP adresy z dynamické toostatic a přidejte další IP adresy pomocí hello portálu podle následujících kroků v hello [přidat IP adresy virtuálních počítačů tooa](#add) tohoto článku.

## <a name="add"></a>Přidat tooa IP adresy virtuálních počítačů

Privátní a veřejné IP adresy tooa síťovou kartu můžete přidat pomocí hello kroků, které následují. Hello příklady v následující části hello předpokládají, že už máte virtuální počítač s konfigurací protokolu IP hello tři popsané v hello [scénář](#Scenario) v tomto článku, ale není to nutné, abyste provedli.

### <a name="coreadd"></a>Základní kroky

1. Procházet toohello portálu Azure v https://portal.azure.com a přihlášení do ní, v případě potřeby.
2. Hello portálu, klikněte na tlačítko **další služby** > typ *virtuální počítače* v hello pole filtru a potom klikněte na **virtuální počítače**.
3. V hello **virtuální počítače** okně klikněte na virtuální počítač má tooadd IP adres k hello. Klikněte na tlačítko **síťových rozhraní** v zobrazeném okně hello virtuálního počítače a pak vyberte hello sítě rozhraní chcete tooadd hello IP adres k. Následující obrázek, hello hello příkladu v hello síťový adaptér s názvem *myNIC* z hello virtuálního počítače s názvem *Můjvp* je vybrán:

    ![Síťové rozhraní](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. V hello zobrazeném okně pro hello seskupování, které jste vybrali, klikněte na **konfigurace protokolu IP**.

Dokončení hello kroky v jednom z hello oddíly, které následují, v závislosti na typu hello IP adresy chcete tooadd.

### <a name="add-a-private-ip-address"></a>**Přidejte privátní IP adresy**

Proveďte následující kroky tooadd novou privátní IP adresu hello:

1. Kroky dokončení hello hello [základní kroky](#coreadd) tohoto článku.
2. Klikněte na tlačítko **Přidat**. V hello **přidat IP konfigurace** okno, které se zobrazí, vytvořit konfiguraci IP adres s názvem *IPConfig 4* s *10.0.0.7* jako *statické* privátní IP adres a pak klikněte na **OK**.

    > [!NOTE]
    > Při přidávání statickou IP adresu, zadejte v hello hello podsítě, které síťový adaptér je připojen k nepoužívané, platná adresa. Pokud není k dispozici hello adresu, kterou vyberete, hello portálu se zobrazí X pro hello IP adresu a tooselect budete potřebovat jiný.

3. Po kliknutí na tlačítko OK, hello okno se zavře a zobrazí se nová konfigurace IP hello uvedené. Klikněte na tlačítko **OK** tooclose hello **přidat IP konfigurace** okno.
4. Můžete kliknout na **přidat** tooadd další konfigurace protokolu IP, nebo zavřete všechny otevřené okna toofinish přidávání IP adresy.
5. Přidat hello privátní IP adresy toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.

### <a name="add-a-public-ip-address"></a>Přidejte veřejnou IP adresu

Veřejná IP adresa se přidá spojením veřejnou IP adresu prostředku tooeither na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres.

> [!NOTE]
> Veřejné IP adresy mají nominální poplatek. více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.
> 

### <a name="create-public-ip"></a>Vytvořte prostředek veřejné IP adresy

Veřejná IP adresa je jedno nastavení pro prostředek veřejné IP adresy. Pokud máte na veřejnou IP adresu prostředek, který není aktuálně přidružené tooan konfigurace protokolu IP, který chcete konfiguraci IP adresy tooan tooassociate, přeskočte hello následující kroky a proveďte hello kroky v jednom z hello oddíly, které následují, potřebujete. Pokud nemáte k dispozici prostředek veřejné adresy IP, proveďte následující kroky toocreate jeden hello:

1. Procházet toohello portálu Azure v https://portal.azure.com a přihlášení do ní, v případě potřeby.
3. Hello portálu, klikněte na tlačítko **nový** > **sítě** > **veřejnou IP adresu**.
4. V hello **vytvoření veřejné IP adresy** okno, které se zobrazí, zadejte **název**, vyberte možnost **přiřazení IP adresy** typ, **předplatné**, **Skupiny prostředků**a **umístění**, pak klikněte na tlačítko **vytvořit**, jak ukazuje následující obrázek hello:

    ![Vytvořte prostředek veřejné IP adresy](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Dokončení hello kroky v jednom z následujících tooassociate hello veřejnou IP adresu tooan IP konfigurace prostředků hello částech.

#### <a name="associate-hello-public-ip-address-resource-tooa-new-ip-configuration"></a>Přidružení hello veřejné IP adresy prostředků tooa nová konfigurace IP

1. Kroky dokončení hello hello [základní kroky](#coreadd) tohoto článku.
2. Klikněte na tlačítko **Přidat**. V hello **přidat IP konfigurace** okno, které se zobrazí, vytvořit konfiguraci IP adres s názvem *IPConfig 4*. Povolit hello **veřejnou IP adresu** a vyberte existující, k dispozici prostředek veřejné IP adresy z hello **zvolte veřejnou IP adresu** okno, které se zobrazí.

    Po dokončení výběru prostředek hello veřejné IP adresy, klikněte na tlačítko **OK** a hello okno se zavře. Pokud nemáte existující veřejnou IP adresu, můžete vytvořit jeden pomocí kroků hello v hello [vytvořte prostředek veřejné IP adresy](#create-public-ip) tohoto článku. 

3. Zkontrolujte hello nová konfigurace IP. To i v případě, že privátní IP adresa nebyla přiřazena explicitně, jeden byla automaticky přiřadí toohello konfiguraci IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy.
4. Můžete kliknout na **přidat** tooadd další konfigurace protokolu IP, nebo zavřete všechny otevřené okna toofinish přidávání IP adresy.
5. Přidat hello privátní IP adresu toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejnou IP adresu toohello operačního systému.

#### <a name="associate-hello-public-ip-address-resource-tooan-existing-ip-configuration"></a>Přidružení hello veřejné konfiguraci IP adresy prostředků tooan stávající IP

1. Kroky dokončení hello hello [základní kroky](#coreadd) tohoto článku.
2. Klikněte na tlačítko požadovaná tooadd hello prostředek veřejné IP adresy do konfigurace IP hello.
3. V okně hello IPConfig, který se zobrazí, klikněte na tlačítko **IP adresu**.
4. V hello **zvolte veřejnou IP adresu** okno, které se zobrazí, vyberte veřejnou IP adresu.
5. Klikněte na tlačítko **Uložit** a bude hello okna zavřete. Pokud nemáte existující veřejnou IP adresu, můžete vytvořit jeden pomocí kroků hello v hello [vytvořte prostředek veřejné IP adresy](#create-public-ip) tohoto článku.
3. Zkontrolujte hello nová konfigurace IP.
4. Můžete kliknout na **přidat** tooadd další konfigurace protokolu IP, nebo zavřete všechny otevřené okna toofinish přidávání IP adresy. Nepřidávejte hello veřejnou IP adresu toohello operačního systému.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
