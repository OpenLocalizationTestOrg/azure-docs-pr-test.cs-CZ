---
title: "aaaCreate testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Vytvoření testovacího prostředí ve službě Azure DevTest Labs pro virtuální počítače"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/30/2017
ms.author: tarcher
ms.openlocfilehash: 2ec5498f10e0cc06a196a71e319e340937abb1a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Vytvoření testovacího prostředí v Azure DevTest Labs
Testovacího prostředí v Azure DevTest Labs je hello infrastrukturu, která zahrnuje skupinu prostředků, třeba virtuální počítače (VM), které vám umožní lépe spravovat tyto prostředky zadáním omezení a kvóty. Tento článek vás provede procesem vytvoření testovacího prostředí pomocí portálu Azure hello hello.

## <a name="prerequisites"></a>Požadavky
toocreate testovacím prostředí, je třeba:

* Předplatné Azure. toolearn o možnostech nákupu Azure najdete v části [jak toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) nebo [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/). Musíte být vlastníkem hello hello předplatné toocreate hello prostředí.

## <a name="steps-toocreate-a-lab-in-azure-devtest-labs"></a>Kroky toocreate testovacího prostředí v Azure DevTest Labs
Hello následující kroky ukazují, jak toouse hello Azure portálu toocreate testovacího prostředí v Azure DevTest Labs. 

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Hello hlavní nabídky na levé straně hello, vyberte **více služeb** (v hello dolní části seznamu hello).

    ![Možnost nabídky Další služby](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. Ze seznamu dostupných služeb, hello **DevTest Labs**.
1. Na hello **DevTest Labs** vyberte **přidat**.
   
    ![Přidání testovacího prostředí](./media/devtest-lab-create-lab/add-lab-button.png)

1. Na hello **vytvoření testovacího prostředí DevTest** okno:
   
    1. Zadejte **název testovacího prostředí** pro hello nového testovacího prostředí.
    2. Vyberte hello **předplatné** tooassociate s testovacím hello.
    3. Vyberte **umístění** v testovacím prostředí které toostore hello.
    4. Vyberte **Auto-shutdown** toospecify Pokud chcete tooenable - a definovat parametry hello - hello automatické vypínání virtuálních počítačů. všechny hello prostředí. Hello automaticky vypnutí funkce je především náklady na ukládání funkce, které můžete zadat, když chcete hello virtuální počítač vypnout tooautomatically. Vypnutí automatického nastavení můžete změnit po vytvoření testovacího prostředí hello podle následujících kroků hello uvedených v článku hello [Správa všech zásad pro testovací prostředí v Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).
    5. Vyberte **Pin tooDashboard** Pokud chcete zástupce hello tooappear testovacího prostředí na řídicí panel portálu hello.
    6. Vyberte **Možnosti automatizace** tooget šablon Azure Resource Manageru pro automatizaci konfigurace. 
    7. Vyberte **Vytvořit**. Po výběru **vytvořit**, hello **DevTest Labs** zobrazí okno. Stav hello hello proces vytvoření testovacího prostředí můžete sledovat pomocí sledování hello **oznámení** oblasti. Po dokončení aktualizace hello stránky toosee hello nově vytvořený testovacího prostředí v seznamu hello laboratoře.  
    
    ![Okno pro vytvoření testovacího prostředí](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Po vytvoření testovacího prostředí, zde jsou některé další kroky tooconsider:

* [Laboratoř tooa zabezpečeného přístupu](devtest-lab-add-devtest-user.md).
* [Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)
* [Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)
* [Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)
* [Přidání virtuálního počítače s artefakty tooa testovacím](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).

