---
title: "aaaCreate virtuální počítač s statickou veřejnou IP adresu - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální počítač s statickou veřejnou IP adresu pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f74d2132785f06148757409ee0a44b98d1e4b98e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-portal"></a>Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Šablona](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Classic)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Vytvoření virtuálního počítače se statickou veřejnou IP adresu

toocreate virtuálního počítače se statickou veřejnou IP adresou v hello portál Azure, dokončení hello následující kroky:

1. V prohlížeči přejděte toohello [portál Azure](https://portal.azure.com) a v případě potřeby, přihlaste se pomocí účtu Azure.
2. V levém horním rohu hello hello portálu, klikněte na tlačítko **nový**>>**výpočetní**>**Windows Server 2012 R2 Datacenter**.
3. V hello **vybrat model nasazení** vyberte **Resource Manager** a klikněte na tlačítko **vytvořit**.
4. V hello **Základy** okno, zadejte informace o hello virtuálních počítačů, jak je uvedeno níže a pak klikněte na tlačítko **OK**.
   
    ![Portál Azure – základy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. V hello **zvolte velikost** okně klikněte na tlačítko **A1 standardní** jak je uvedeno níže a potom klikněte na **vyberte**.
   
    ![Portál Azure – zvolte velikost](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. V hello **nastavení** okně klikněte na tlačítko **veřejnou IP adresu**, potom v hello **vytvoření veřejné IP adresy** okno, v části **přiřazení**, klikněte na **Statické** jak je uvedeno níže. A pak klikněte na **OK**.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. V hello **nastavení** okně klikněte na tlačítko **OK**.
8. Zkontrolujte hello **Souhrn** okno, jak je uvedeno níže a pak klikněte na tlačítko **OK**.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Všimněte si hello nové dlaždice na řídicím panelu.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Po vytvoření virtuálního počítače hello hello **nastavení** okno se zobrazí, jak je uvedeno níže
    
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

