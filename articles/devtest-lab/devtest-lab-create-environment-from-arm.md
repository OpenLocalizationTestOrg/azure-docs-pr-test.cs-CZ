---
title: "aaaCreate prostředí více virtuálních počítačů a PaaS prostředků pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Zjistěte, jak toocreate prostředí více virtuálních počítačů a PaaS prostředky v Azure DevTest Labs z šablony Azure Resource Manager"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: tarcher
ms.openlocfilehash: ab8628f6cb5a666435258efb93921ec69ad3a13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Vytvoření prostředí více virtuálních počítačů a PaaS prostředky pomocí šablony Azure Resource Manager

Hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) vám umožní tooeasily [vytvoření a přidání testovacího prostředí virtuálních počítačů tooa](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-add-vm). To funguje dobře pro vytvoření virtuálního počítače, jeden v čase. Ale pokud hello prostředí obsahuje víc virtuálních počítačů, každý virtuální počítač má toobe vytvoření jednotlivě. Pro scénáře, jako je vícevrstvé webové aplikace nebo farmy služby SharePoint je mechanismus tooallow potřebné pro vytvoření hello více virtuálních počítačů v jediném kroku. Pomocí šablony Azure Resource Manager, můžete teď určit hello infrastruktury a konfigurace řešení Azure a opakovaně nasazovat více virtuálních počítačů v konzistentním stavu. Tato funkce poskytuje hello následující výhody:

- Šablony Azure Resource Manageru jsou načítány přímo ze svým úložištěm řízení zdrojů (GitHub nebo Team Services Git).
- Po nakonfigurování mohou uživatelé vytvářet prostředí výběrem šablonu Azure Resource Manageru z portálu Azure jako co mohou provádět u jiných typů hello [virtuálních počítačů základů](./devtest-lab-comparing-vm-base-image-types.md).
- Prostředky Azure PaaS se dá zřídit v prostředí z šablony Azure Resource Manager v tooIaaS přidání virtuálních počítačů.
- náklady na Hello prostředí lze sledovat v testovacím prostředí hello v přidání tooindividual virtuální počítače vytvořené jiné typy základny.
- PaaS prostředky se vytvoří a zobrazí se v náklady sledování; vypnutí virtuálního počítače automaticky nevztahuje tooPaaS prostředky.
- Uživatelé mají hello stejné řízení zásad virtuálních počítačů pro prostředí, musí být stejné pro virtuální počítače jedním prostředí.

Další informace o hello mnoho [výhody používání šablon Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) toodeploy, aktualizovat nebo odstranit všechny vaše prostředky testovacího prostředí v rámci jedné operace.

> [!NOTE]
> Při použití šablony Resource Manageru jako základ toocreate další prostředí virtuálních počítačů, nejsou některé rozdíly tookeep Pamatujte, ať již vytváříte více – virtuální počítače nebo virtuální jednoho počítače. Virtuální počítač pomocí šablony Azure Resource Manageru vysvětluje tyto rozdíly podrobněji.
>
>

## <a name="configure-azure-resource-manager-template-repositories"></a>Konfigurace úložiště šablony Azure Resource Manager

Jako jeden z hello se mají spravovat osvědčené postupy s jako kód infrastruktury a konfigurace jako kód, šablony prostředí ve správě zdrojového kódu. Azure DevTest Labs následuje tento postup a načte všechny šablony Azure Resource Manager přímo z vašeho úložiště GitHub nebo služby VSTS Git. V důsledku toho šablony správce prostředků lze napříč celou verzi cyklu hello z hello testovací prostředí toohello produkčního prostředí.

Existuje několik pravidel toofollow tooorganize šablony Azure Resource Manager v úložišti:

- musí mít název souboru hlavní šablony Hello `azuredeploy.json`. 

    ![Soubory šablon klíč Azure Resource Manager](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Pokud chcete toouse hodnot parametrů definovaných v souboru parametrů, musí mít název souboru parametrů hello `azuredeploy.parameters.json`.
- Pomocí parametrů hello `_artifactsLocation` a `_artifactsLocationSasToken` tooconstruct hello parametersLink hodnota identifikátoru URI, což DevTest Labs tooautomatically spravovat vnořené šablony. V tématu [jak Azure DevTest Labs usnadňuje vnořené Resource Manager šablona nasazení pro testovací prostředí](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) Další informace.
- Metadata může být definovaná toospecify hello zobrazovaný název a popis šablony. Tato metadata musí být v souboru s názvem `metadata.json`. Hello soubor metadat následující příklad ukazuje, jak zobrazit toospecify hello název a popis: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of hello template>"
 
}
```

Hello následující kroky vás provede přidáním úložiště tooyour testovacím prostředí pomocí hello portálu Azure. 

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
1. Ze seznamu hello labs vyberte požadované prostředí hello.   
1. V okně prostředí hello vyberte **konfiguraci a zásady**.

    ![Konfigurace a zásady](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Z hello **konfiguraci a zásady** seznam nastavení, vyberte **úložiště**. Hello **úložiště** okno uvádí hello úložiště, které byly přidány toohello testovacího prostředí. Úložiště s názvem `Public Repo` se automaticky vygeneroval pro všechna prostředí a připojí toohello [úložiště DevTest Labs GitHub](https://github.com/Azure/azure-devtestlab) obsahující různých artefaktů virtuálních počítačů pro vaše použití.

    ![Veřejné úložiště](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Vyberte **přidat +** tooadd úložiště šablony Azure Resource Manager.
1. Když hello druhý **úložiště** otevře se okno, zadejte informace potřebné hello následujícím způsobem:
    - **Název** – zadejte název hello úložiště, který se používá v testovacím prostředí hello.
    - **Adresa URL klonu Git** – zadejte adresu URL klonu GIT HTTPS hello z Githubu nebo Visual Studio Team Services.  
    - **Větev** -zadejte tooaccess název větve hello definic šablony Azure Resource Manager. 
    - **Osobní přístupový token** -hello osobní přístupový token se používá toosecurely přístup úložiště. Vyberte tokenu z Visual Studio Team Services, tooget  **&lt;jméno >> Můj profil > zabezpečení > veřejný přístup token**. tooget tokenu z Githubu, vyberte miniatury a potom výběrem **Nastavení > veřejný přístup token**. 
    - **Cesty ke složce zadat** – pomocí jedné z hello dvě vstupní pole, zadejte cestu ke složce hello začínající lomítkem - / - a je relativní tooyour tooeither URI klon Git artefaktů definic (první vstupní pole) nebo šablony Azure Resource Manager definice.   
    
        ![Veřejné úložiště](./media/devtest-lab-create-environment-from-arm/repo-values.png)

1. Jakmile všechna pole hello vyžaduje zadání a projít ověřením hello, vyberte **Uložit**.

Hello další části vás provede procesem vytvoření prostředí z šablony Azure Resource Manager.

## <a name="create-an-environment-from-an-azure-resource-manager-template"></a>Vytvoření prostředí z šablony Azure Resource Manager

Jakmile byl nakonfigurován jako úložiště šablony Azure Resource Manager v testovacím prostředí hello, uživatelé testovacího prostředí můžete vytvořit prostředí pomocí portálu Azure hello následující kroky:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
1. Ze seznamu hello labs vyberte požadované prostředí hello.   
1. V okně prostředí hello vyberte **přidat +**.
1. Hello **zvolte na základní** zobrazuje hello základní bitové kopie, můžete pomocí šablony Azure Resource Manager hello uvedená jako první. Vyberte hello požadované šablony Azure Resource Manageru.

    ![Vyberte na základní](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Na hello **přidat** okno, zadejte hello **název prostředí** hodnotu. Název prostředí Hello je, co je uživatelé zobrazit tooyour v testovacím prostředí hello. Hello zbývající vstupní pole jsou definovány v hello šablony Azure Resource Manageru. Pokud výchozí hodnoty jsou definovány v šabloně hello nebo hello `azuredeploy.parameter.json` je soubor k dispozici, výchozí hodnoty jsou zobrazeny v těchto vstupních polí. Pro parametry typu *zabezpečení řetězec*, můžete použít hello tajné klíče uložené v prostředí hello [osobním úložišti tajný](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Okno Přidání](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Existuje několik hodnot parametrů, které - i v případě, že zadaný - se zobrazují jako prázdné hodnoty. Proto pokud uživatelé přiřadit tyto hodnoty tooparameters v šablonu Azure Resource Manager, DevTest Labs nezobrazí hello hodnoty; Místo toho zobrazující prázdný vstupní pole, kde hello lab uživatelů musí tooenter hodnotu při vytváření prostředí pro hello.
    > 
    > - GEN JEDINEČNÝ
    > - -JEDINEČNÉ - GENERACE [N]
    > - GEN--PUB-KLÍČ SSH
    > - GEN-HESLO 
 
1. Vyberte **přidat** toocreate hello prostředí. prostředí Hello spustí okamžitě zřizování s hello stav zobrazení v hello **Můj virtuální počítače** seznamu. Novou skupinu prostředků se automaticky vytvoří pomocí hello testovacím tooprovision všechny prostředky hello definované v hello šablony Azure Resource Manageru.
1. Po vytvoření prostředí hello vyberte hello prostředí v hello **Můj virtuální počítače** okně skupiny prostředků hello tooopen seznam a vyhledejte všechny prostředky hello zřízené v prostředí hello.
    
    ![Moje seznam virtuálních počítačů](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Můžete také rozšířit hello prostředí tooview jenom hello seznam virtuálních počítačů, které jsou zřízené v prostředí hello.
    
    ![Moje seznam virtuálních počítačů](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Klikněte na jakékoliv hello prostředí tooview hello dostupné akce – například použití artefaktů, připojení datových disků, změny času automatického vypnutí a další.

    ![Akce prostředí](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="next-steps"></a>Další kroky
* Po vytvoření virtuálního počítače, můžete připojit toohello virtuálních počítačů tak, že vyberete **Connect** v okně hello Virtuálního počítače.
* Zobrazit a spravovat prostředky v prostředí s výběrem hello prostředí v hello **Můj virtuální počítače** seznamu ve svém testovacím prostředí. 
* Prozkoumejte hello [šablon Azure Resource Manageru z Galerie šablon Azure rychlý start](https://github.com/Azure/azure-quickstart-templates)
