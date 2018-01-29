---
title: "Více IP adres pro virtuální počítače Azure - Portal | Microsoft Docs"
description: "Zjistěte, jak přiřadit více IP adres k virtuálnímu počítači pomocí portálu Azure | Správce prostředků."
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
ms.openlocfilehash: d264bd47d76db8015a64f09248c57c94572e2693
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Přiřadit více IP adres virtuálních počítačů pomocí portálu Azure

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
Tento článek vysvětluje, jak vytvořit virtuální počítač (VM) pomocí modelu nasazení Azure Resource Manager pomocí portálu Azure. Nelze přiřadit více IP adres k prostředkům, které jsou vytvořené pomocí modelu nasazení classic. Další informace o modelech nasazení Azure, najdete [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Vytvoření virtuálního počítače s více IP adres

Pokud chcete vytvořit virtuální počítač s více IP adres nebo statickou privátní IP adresu, musíte vytvořit pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure. Klikněte na možnosti prostředí PowerShell nebo rozhraní příkazového řádku v horní části v tomto článku se dozvíte jak. Virtuální počítač můžete vytvořit pomocí jediné dynamické privátní IP adresy a (volitelně) jednu veřejnou IP adresu pomocí kroků v portálu [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) nebo [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-portal.md) články. Po vytvoření virtuálního počítače, můžete změnit typ IP adresy z dynamického na statické a přidejte další IP adresy pomocí portálu podle následujících kroků v [přidat IP adresy pro virtuální počítač](#add) tohoto článku.

## <a name="add"></a>Přidání IP adres pro virtuální počítač

Provedením následujících kroků můžete přidat privátní a veřejné IP adresy pro síťový adaptér. Příklady v následujících částech předpokládají, že už máte virtuální počítač s tři konfigurace protokolu IP, které jsou popsané v [scénář](#Scenario) v tomto článku, ale není to nutné, abyste provedli.

### <a name="coreadd"></a>Základní kroky

1. Přejděte na portál Azure na adrese https://portal.azure.com a přihlaste do ní, v případě potřeby.
2. Na portálu, klikněte na tlačítko **další služby** > typ *virtuální počítače* pole filtru, a pak klikněte na **virtuální počítače**.
3. V **virtuální počítače** okno, klikněte na virtuální počítač, který chcete přidat IP adres k. Klikněte na tlačítko **síťových rozhraní** ve virtuálním počítači okno, které se zobrazí a potom vyberte síťové rozhraní, které chcete přidat IP adres k. V příkladu znázorněno na následujícím obrázku s názvem síťový adaptér *myNIC* z virtuálního počítače s názvem *Můjvp* je vybrán:

    ![Síťové rozhraní](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. V okně, který se zobrazí pro síťový adaptér, který jste vybrali, klikněte na tlačítko **konfigurace protokolu IP**.

Proveďte kroky v jednom z následujících, na základě typu IP adresu, kterou chcete přidat.

### <a name="add-a-private-ip-address"></a>**Přidejte privátní IP adresy**

Proveďte následující postup pro přidání nové privátní IP adresy:

1. Proveďte kroky v [základní kroky](#coreadd) tohoto článku.
2. Klikněte na tlačítko **Přidat**. V **přidat IP konfigurace** okno, které se zobrazí, vytvořit konfiguraci IP adres s názvem *IPConfig 4* s *10.0.0.7* jako *statické* privátní IP adres a pak klikněte na **OK**.

    > [!NOTE]
    > Při přidávání statickou IP adresu, musíte zadat Nepoužívané, platná adresa v podsíti, které síťový adaptér je připojen k. Pokud není k dispozici na adresu, kterou vyberete, portálu se zobrazí X pro IP adresu a budete muset vybrat jinou krychli.

3. Po kliknutí na tlačítko OK, okno se zavře a zobrazí se nová konfigurace IP uvedené. Klikněte na tlačítko **OK** zavřete **přidat IP konfigurace** okno.
4. Můžete kliknout na **přidat** přidat další konfigurace protokolu IP, nebo zavřete všechna otevřená okna dokončíte přidání IP adres.
5. Přidání privátních IP adres do operačního systému virtuálního počítače pomocí kroků pro operační systém v [přidat IP adresy na operační systém virtuálního počítače](#os-config) tohoto článku.

### <a name="add-a-public-ip-address"></a>Přidejte veřejnou IP adresu

Veřejná IP adresa se přidá tím, že přidružíte prostředek veřejné IP adresy na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres.

> [!NOTE]
> Veřejné IP adresy mají nominální poplatek. Další informace o cenách IP adresu, najdete [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Maximální počet veřejné IP adresy, které lze použít v předplatném je. Další informace o omezeních najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md#networking-limits).
> 

### <a name="create-public-ip"></a>Vytvořte prostředek veřejné IP adresy

Veřejná IP adresa je jedno nastavení pro prostředek veřejné IP adresy. Pokud máte na veřejnou IP adresu prostředek, který není aktuálně přidružena ke konfiguraci IP adres, kterou chcete přidružit k konfiguraci IP adres, následující kroky přeskočte a dokončete kroky v jednom z následujících, potřebujete. Pokud nemáte k dispozici prostředek veřejné adresy IP, proveďte následující kroky k jeho vytvoření:

1. Přejděte na portál Azure na adrese https://portal.azure.com a přihlaste do ní, v případě potřeby.
3. Na portálu, klikněte na tlačítko **nový** > **sítě** > **veřejnou IP adresu**.
4. V **vytvoření veřejné IP adresy** okno, které se zobrazí, zadejte **název**, vyberte možnost **přiřazení IP adresy** typ, **předplatné**, **Skupiny prostředků**a **umístění**, pak klikněte na tlačítko **vytvořit**, jak je znázorněno na následujícím obrázku:

    ![Vytvořte prostředek veřejné IP adresy](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Proveďte kroky v jednom z následujících přidružit prostředek veřejné IP adresy pro konfiguraci IP adres.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Přidružte prostředek veřejné IP adresy na novou konfiguraci protokolu IP

1. Proveďte kroky v [základní kroky](#coreadd) tohoto článku.
2. Klikněte na tlačítko **Přidat**. V **přidat IP konfigurace** okno, které se zobrazí, vytvořit konfiguraci IP adres s názvem *IPConfig 4*. Povolit **veřejnou IP adresu** a vybrat stávající, k dispozici veřejnou IP adresu prostředek z **zvolte veřejnou IP adresu** okno, které se zobrazí.

    Po dokončení výběru prostředek veřejné IP adresy, klikněte na tlačítko **OK** a okno se zavře. Pokud nemáte existující veřejnou IP adresu, můžete vytvořit jeden pomocí kroků v [vytvořte prostředek veřejné IP adresy](#create-public-ip) tohoto článku. 

3. Zkontrolujte novou konfiguraci protokolu IP. Přestože privátní IP adresa nebyla přiřazena explicitně, jeden byl automaticky přiřazen ke konfiguraci IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy.
4. Můžete kliknout na **přidat** přidat další konfigurace protokolu IP, nebo zavřete všechna otevřená okna dokončíte přidání IP adres.
5. Přidání privátní IP adresu do operačního systému virtuálního počítače pomocí kroků pro operační systém v [přidat IP adresy na operační systém virtuálního počítače](#os-config) tohoto článku. Nepřidávejte veřejnou IP adresu do operačního systému.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Přidružte prostředek veřejné IP adresy na existující konfiguraci IP adres

1. Proveďte kroky v [základní kroky](#coreadd) tohoto článku.
2. Klikněte na tlačítko chcete přidat prostředek veřejné IP adresy pro konfiguraci protokolu IP.
3. V okně IPConfig, který se zobrazí, klikněte na tlačítko **IP adresu**.
4. V **zvolte veřejnou IP adresu** okno, které se zobrazí, vyberte veřejnou IP adresu.
5. Klikněte na tlačítko **Uložit** a okna se zavře. Pokud nemáte existující veřejnou IP adresu, můžete vytvořit jeden pomocí kroků v [vytvořte prostředek veřejné IP adresy](#create-public-ip) tohoto článku.
3. Zkontrolujte novou konfiguraci protokolu IP.
4. Můžete kliknout na **přidat** přidat další konfigurace protokolu IP, nebo zavřete všechna otevřená okna dokončíte přidání IP adres. Nepřidávejte veřejnou IP adresu do operačního systému.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
