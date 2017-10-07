---
title: "aaaView hello měsíční odhadované testovacím trend náklady v Azure DevTest Labs | Microsoft Docs"
description: "Další informace o hello Azure DevTest Labs měsíční odhadované náklady trend grafu."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 47c7dd4cf91b76de74b502c50f05e79cd501ee35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-hello-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Zobrazení hello měsíční odhadované testovacím trend náklady v Azure DevTest Labs
Funkce náklady na správu Hello DevTest Labs vám pomůže sledovat hello náklady na vašem testovacím prostředí. Tento článek ukazuje, jak toouse hello **měsíční odhadované náklady Trend** grafu tooview hello aktuální kalendářní měsíc odhadované náklady na začátku a hello předpokládané koncový měsíc náklady pro hello aktuální kalendářní měsíc. V tomto článku se dozvíte, jak tooview hello měsíční odhadované náklady graf trendů v hello portálu Azure.

## <a name="viewing-hello-monthly-estimated-cost-trend-chart"></a>Zobrazení grafu měsíční odhadované náklady Trend hello
tooview hello měsíční Trend odhadované náklady na graf, postupujte takto: 

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello.   
4. V okně prostředí hello vyberte **náklady nastavení**.
5. V testovacím hello **náklady nastavení** vyberte **trend náklady na testovacím**.
6. Hello následující snímek obrazovky ukazuje příklad grafu náklady. 
   
    ![Cenově grafu](./media/devtest-lab-configure-cost-management/graph.png)

Hello **odhadované náklady na** hodnota je hello aktuální kalendářní měsíc odhadované náklady na začátku. Hello **plánované náklady** hello odhadované náklady pro celou hello aktuální kalendářní měsíc vypočítáván hello testovacím náklady pro hello předchozí pět dní.

objemy Hello náklady jsou zaokrouhlit toohello nejbližší celé číslo. Například: 

* 5.01 zaokrouhlí too6 
* 5.50 zaokrouhlí too6
* 5.99 zaokrouhlí too6

Jak je uvedeno výše hello grafu, jsou náklady hello se zobrazí v grafu hello *odhadované* stojí pomocí [průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) nabízejí sazby.
Kromě toho jsou následující hello *není* součástí hello výpočet nákladů:

* Předplatné Dreamspark a CSP nejsou aktuálně podporovány jako Azure DevTest Labs používá hello [rozhraní API Azure fakturace](../billing/billing-usage-rate-card-overview.md) testovacím hello toocalculate náklady, který nepodporuje zprostředkovatele kryptografických služeb nebo Dreamspark odběry.
* Nabídka sazby. V současné době jsme nejsou možné toouse nabídka sazby (zobrazené v rámci svého předplatného), že máte vyjedná se společnosti Microsoft nebo Microsoft partnery. Používáme průběžné platby sazby.
* Vaše daně
* Vaše slevy
* Vaše fakturační Měna. V současné době náklady hello testovacího prostředí se zobrazí pouze v USD měny.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související
* [Dva další věci tookeep vaše náklady na sledování v DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Proč náklady prahové hodnoty?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Další kroky
Zde jsou další tootry některé věci:

* [Definování zásad testovacího](devtest-lab-set-lab-policy.md) – zjistěte, jak tooset hello různé zásady použít toogovern použití testovacího prostředí a jeho virtuální počítače. 
* [Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace. Tento článek ukazuje, jak toocreate vlastní obrázek z soubor virtuálního pevného disku.
* [Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace. Tento článek ukazuje, jak toospecify, který, pokud existuje, Azure Marketplace bitové kopie lze použít při vytváření virtuálních počítačů v testovacím prostředí.
* [Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -ukazuje, jak toocreate virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a jak toowork s artefakty ve vašem virtuálním počítači.

