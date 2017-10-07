---
title: "aaaGet spuštění se soukromými šablonami | Microsoft Docs"
description: "Přidat, spravovat a sdílejte svoje soukromé šablony pomocí hello portálu Azure, hello příkazového řádku Azure CLI nebo Powershellu."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
ms.openlocfilehash: 1fe2c6422f62a98f7ae9ba5c61b9639d993f0bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-private-templates-on-hello-azure-portal"></a>Začínáme se soukromými šablonami na portálu Azure hello
[Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) šablona je deklarativní šablona, která používá toodefine vaše nasazení. Můžete definovat hello toodeploy prostředky pro řešení a určit parametry a proměnné, které umožňují tooinput hodnoty pro různá prostředí. Hello šablona se skládá z JSON a výrazy, které můžete použít hodnoty tooconstruct pro vaše nasazení.

Hello můžete použít nové **šablony** funkci hello [portálu Azure](https://portal.azure.com) společně s hello **Microsoft.Gallery** poskytovatele prostředků jako rozšíření nástroje hello [ Azure Marketplace](https://azure.microsoft.com/marketplace/) tooenable uživatelé toocreate, správě a nasazování soukromých šablon z osobní knihovny.

Tento dokument vás provede přidáním, správou a sdílením soukromé provede **šablony** pomocí hello portálu Azure.

## <a name="guidance"></a>Doprovodné materiály
Hello následující návrhy vám pomohou naplno využít **šablony** při práci se svými řešeními:

* **Šablona** je zapouzdřující prostředek, který obsahuje šablonu Resource Manageru a další metadata. Chová se velmi podobně tooan položky v hello Marketplace. Hello klíčovým rozdílem je, že se jedná o soukromou položku jako toohello názvem na rozdíl od veřejných položek Marketplace.
* Hello **šablony** knihovny funguje dobře pro uživatele, kteří potřebují toocustomize jeho nasazení.
* **Šablony** dobře fungují pro uživatele, kteří potřebují jednoduché úložiště v rámci Azure.
* Začněte s existující šablonou Resource Manageru. Najděte šablony na [GitHubu](https://github.com/Azure/azure-quickstart-templates) nebo [exportujte šablonu](../azure-resource-manager/resource-manager-export-template.md) z existující skupiny prostředků.
* **Šablony** vázanou toohello uživatelem, který je publikuje. Název vydavatele Hello je viditelná tooeveryone, který má tooit přístup pro čtení.
* **Šablony** jsou prostředky Resource Manageru a po publikování je nelze přejmenovat.

## <a name="add-a-template-resource"></a>Přidání prostředku šablony
Existují dva způsoby toocreate **šablony** prostředku v hello portálu Azure.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Způsob 1: Vytvoření nového prostředku šablony ze spuštěné skupiny prostředků
1. Přejděte tooan existující skupinu prostředků na hello portálu Azure. V **Nastavení** vyberte **Exportovat šablonu**.
2. Po exportu šablony Resource Manageru hello použít hello **uložit šablonu** toosave tlačítko ji toohello **šablony** úložiště. Podrobné informace o exportování šablony najdete [zde](../azure-resource-manager/resource-manager-export-template.md).
   <br /><br />
   ![Export skupiny prostředků](media/rg-export-portal1.PNG)  <br />
3. Vyberte hello **uložit tooTemplate** příkazového tlačítka.
   <br /><br />
4. Zadejte hello následující informace:
   
   * Název – název objektu hello šablony (Poznámka: Toto je název založená na správci prostředků Azure. Platí všechna omezení pro pojmenovávání a název nelze po vytvoření změnit).
   * Popis – stručný popis šablony hello.
     
     ![Uložení šablony](media/save-template-portal1.PNG)  <br />
5. Klikněte na **Uložit**.
   
   > [!NOTE]
   > Hello okně Exportovat šablonu zobrazuje oznámení když hello vyexportované šablony Resource Manageru obsahuje chyby, ale stále budete moct toosave tento toohello šablony správce prostředků šablony. Ujistěte se, že jste zkontrolovali a opravili všechny Resource Manager problémy se šablonou před hello opětovné nasazení vyexportované šablony Resource Manageru.
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a>Způsob 2: Přidání nového prostředku šablony z procházení
Můžete také přidat nový **šablony** od začátku pomocí hello příkazového tlačítka + přidat v **procházet > šablony**. Je nutné tooprovide název, popis a hello JSON šablony Resource Manageru.

![Přidání šablony](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> Microsoft.Gallery je poskytovatel prostředků Azure umístěný na Tenantu. Hello prostředek šablony je vázané toohello uživatele, který ho vytvořil. Není vázaná tooany konkrétní předplatné. Předplatné je toobe zvolit pouze při nasazování šablony.
> 
> 

## <a name="view-template-resources"></a>Zobrazení prostředků šablony
Všechny **šablony** k dispozici tooyou si můžete prohlédnout v **procházet > šablony**. To zahrnuje jak **šablony**, které jste vytvořili, tak i šablony, které jsou s vámi sdílené s různými úrovněmi oprávnění. Další podrobnosti v hello [řízení přístupu](#access-control-for-a-tenant-resource-provider) části níže.

![Zobrazit šablonu](media/view-template-portal1.PNG)  <br />

Můžete zobrazit podrobnosti o hello **šablony** kliknutím na položku v seznamu hello.

![Zobrazit šablonu](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Úprava prostředku šablony
Můžete zahájit tok upravování hello **šablony** kliknutím pravým tlačítkem na hello položka v seznamu hello procházet nebo vybráním příkazového tlačítka Upravit hello.

![Úprava šablony](media/edit-template-portal1a.PNG)  <br />

Můžete upravit hello popis nebo text šablony Resource Manageru. Název hello nelze upravit, protože se jedná o název prostředku Resource Manageru. Při úpravách JSON šablony Resource Manageru hello ověříme tooensure, že se jedná o platný kód JSON. Zvolte **OK** a potom **Uložit** toosave aktualizovanou šablonu.

![Úprava šablony](media/edit-template-portal2a.PNG)  <br />

Jednou hello **šablony** je uložen se zobrazí oznámení o potvrzení.

![Úprava šablony](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Nasazení prostředku šablony
Můžete nasadit jakoukoli **šablonu**, ke které máte oprávnění ke **čtení**. Hello tok nasazení spustí standardní okno nasazení šablony Azure hello. Vyplňte hodnoty hello hello Resource Manager šablony parametry tooproceed hello nasazení.

![Nasazení šablony](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Úprava prostředku šablony
Prostředek **Šablony** můžete sdílet s ostatními uživateli. Sdílení se chová podobně jako příliš[přiřazování rolí pro jakýkoli prostředek na Azure](../active-directory/role-based-access-control-configure.md). Hello **šablony** vlastníka poskytuje oprávnění tooother uživatelé, kteří mohou interagovat s prostředkem šablony. Hello osoba nebo skupina osob sdílet hello **šablony** se bude moct toosee šablony Resource Manageru hello a její vlastnosti galerie.

### <a name="access-control-for-hello-microsoftgallery-resources"></a>Řízení přístupu pro prostředky Microsoft.Gallery hello
| Role | Oprávnění |
| --- | --- |
| Vlastník |Povoluje úplné řízení hello prostředku šablony včetně sdílení. |
| Čtenář |Povoluje čtení a spouštění (nasazování) na hello prostředku šablony |
| Přispěvatel |Povoluje upravování a právo odstranit v hello prostředku šablony. Uživatele nelze sdílet s ostatními hello šablony |

Vyberte **sdílené složky** na procházené položce hello kliknutím pravým tlačítkem nebo v okně hello zobrazení konkrétní položky. Spustí se Možnosti sdílení.

![Sdílení šablony](media/share-template-portal1a.png)  <br />

 Teď můžete zvolit roli a uživatele nebo skupinu tooprovide přístup tooa konkrétní **šablony**. Hello dostupné role jsou vlastník, Čtenář a Přispěvatel. Další podrobnosti v hello [řízení přístupu](#access-control-for-a-tenant-resource-provider) část výše.

![Sdílení šablony](media/share-template-portal2b.png)  <br />

![Sdílení šablony](media/share-template-portal3b.png)  <br />

Klikněte na **Vybrat** a **Ok**. Nyní můžete vidět hello uživatele nebo skupiny přidat toohello prostředků.

![Sdílení šablony](media/share-template-portal4b.png)  <br />

> [!NOTE]
> Šablony lze sdílet pouze s uživateli a skupinami v hello stejné klienta Azure Active Directory. Pokud šablonu sdílíte s e-mailovou adresu, která není ve vašem klientovi, pozvánka odešle žádostí hello uživatele toojoin hello klienta jako Host.
> 
> 

## <a name="next-steps"></a>Další kroky
* toolearn o vytváření šablon Resource Manageru, najdete v části [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md)
* Funkce hello toounderstand můžete použít v šabloně Resource Manageru, najdete v části [šablony funkcí](../azure-resource-manager/resource-group-template-functions.md)
* Doprovodné materiály o navrhování šablon naleznete v tématu [Osvědčené postupy pro navrhování šablon Azure Resource Manageru](../azure-resource-manager/best-practices-resource-manager-design-templates.md).

