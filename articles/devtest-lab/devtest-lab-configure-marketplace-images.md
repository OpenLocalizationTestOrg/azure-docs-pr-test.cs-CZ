---
title: "nastavení bitové kopie aaaConfigure Azure Marketplace v Azure DevTest Labs | Microsoft Docs"
description: "Konfigurace, které Azure Marketplace obrázky lze použít při vytváření virtuálního počítače v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: bb4b7f1c0cbe967bee724f7ee20f64f8c4ea58ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Konfigurace nastavení Azure Marketplace bitové kopie v Azure DevTest Labs
DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace v závislosti na tom, jak jste nakonfigurovali toobe Image Azure Marketplace použít ve svém testovacím prostředí. Tento článek ukazuje, jak toospecify, který, pokud existuje, Azure Marketplace bitové kopie lze použít při vytváření virtuálních počítačů v testovacím prostředí. Tím se zajistí, že má váš tým pouze přístup toohello Marketplace obrázky, které potřebují. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Vyberte, které Azure Marketplace obrázků jsou povoleny při vytváření virtuálního počítače
1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello. 
4. V okně prostředí hello vyberte **konfiguraci a zásady**.
5. Na testovacího prostředí na **konfiguraci a zásady** okno pod **základů virtuálního počítače**, vyberte **Marketplace image**.
6. Zadejte, zda chcete, že všechny hello kvalifikovaný Image Azure Marketplace toobe k dispozici pro použití jako základ nového virtuálního počítače. Pokud vyberete **Ano**, pak všechny Image Azure Marketplace hello které splňují všechny hello následující kritéria jsou povoleny v testovacím prostředí hello:
   
   * Obrázek Hello vytvoří jeden virtuální počítač, **a**
   * Obrázek Hello používá tooprovision virtuálních počítačích Azure Resource Manager, **a**
   * Obrázek Hello nevyžaduje zakoupení velmi licenčního plánu
     
    Pokud chcete, aby žádné toobe bitové kopie povolené, nebo chcete toospecify obrázky, které lze použít, vyberte **ne**.
     
     ![Možnost tooallow všechny bitové kopie toobe Marketplace použít jako základní bitové kopie u virtuálních počítačů](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Pokud vyberete **ne** toohello předchozí krok, hello **bitové kopie nebo vyberte povoleno, všechny** zaškrtávací políčko je aktivní. 
   Můžete použít tuto možnost společně s hello vyhledávací pole tooquickly vyberte nebo zrušte všechny položky hello zobrazí v seznamu hello.
   * Vyberte hello Image Azure Marketplace chcete tooallow pro vytvoření virtuálních počítačů jednotlivě zaškrtnutím políčka odpovídající každé bitové kopie.
   * Pokud nechcete, aby tooallow žádné toobe Image Azure Marketplace v testovacím prostředí hello používá, vyberte ze seznamu hello nic.
   
    ![Můžete zadat, které Azure Marketplace obrázky lze použít jako základní bitové kopie u virtuálních počítačů](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Nakonfigurované tak, jak jsou povoleny Azure Marketplace obrázky, při vytváření virtuálního počítače, je dalším krokem hello příliš[přidání testovacího prostředí virtuálních počítačů tooyour](devtest-lab-add-vm-with-artifacts.md).

