---
title: "aaaAzure spravované aplikace v hello Marketplace | Microsoft Docs"
description: "Popisuje Azure spravované aplikace, které jsou k dispozici prostřednictvím hello Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b3cdf3f1fccdd47db699e4892ae8bce35118bfd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-in-hello-marketplace"></a>Azure spravované aplikace v hello Marketplace.

 MSPs, nezávislí dodavatelé softwaru a systémových integrátorech (si) můžete použít Azure spravované aplikace toooffer zákazníků řešení tooall Azure Marketplace. Taková řešení snížit hello údržby a režijní náklady na údržbu zákazníků. Vydavatelé můžete prodeje, infrastruktury a software prostřednictvím hello Marketplace. Můžete se připojit služeb a aplikací toomanaged provozní podpory. Další informace najdete v tématu [spravované aplikace přehled](managed-application-overview.md).

Tento článek vysvětluje, jak můžete publikovat aplikaci toohello Marketplace a nastavit jej jako široce dostupné toocustomers MSP, ISV nebo serveru.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Předpoklady pro publikování spravované aplikace

Požadavky toolisting v hello Marketplace:

* Technické

    *  Informace o základní strukturu hello a syntaxe šablon Azure Resource Manageru najdete v tématu [šablon Azure Resource Manageru](resource-group-authoring-templates.md).
    *  tooview úplnou šablonu řešení, najdete v části [šablony Azure Quickstart](https://azure.microsoft.com/en-us/documentation/templates/) nebo hello [úložiště šablony rychlý Start](https://github.com/azure/azure-quickstart-templates).
    *  Informace o tom, jak toocreate hello rozhraní pro zákazníky, kteří nasazují aplikace prostřednictvím hello Marketplace najdete v tématu [vytvoření uživatelského rozhraní definice souboru](managed-application-createuidefinition-overview.md).

* Běžné uživatele (obchodní požadavky)

    *   Vaše společnost nebo její dceřiné společnosti, musí být umístěny v určité zemi, kde je podporováno prodeje podle hello Marketplace.
    *   Tento produkt musí mít licenci způsobem, který je kompatibilní s modely fakturace nepodporuje hello Marketplace.
    *   Jste zajišťuje, aby se na technickou podporu k dispozici toocustomers vyvineme způsobem. Podpora Hello můžete být volná, placené, nebo prostřednictvím komunity podporovat.
    *   Jste zodpovědní za licencování softwaru a všechny závislosti software třetích stran.
    *   Je nutné zadat, že obsah, který splňuje kritéria pro vaši nabídku toobe uvedené v hello Marketplace a hello portálu Azure.
    *   Podmínky toohello hello Azure Marketplace zapojení zásad a vydavatele smlouvu, musíte souhlasit.
    *   Toocomply s hello podmínky použití, prohlášení o ochraně osobních údajů Microsoft a certifikované smlouvy programu služby Microsoft Azure, musíte souhlasit.

## <a name="create-a-new-azure-application-offer"></a>Vytvořit novou aplikaci Azure nabídka

Pokud jsou splněny požadavky hello jste připravené toocreate vaši nabídku spravované aplikace. Podívejme rychlý přehled o nabídku a SKU.

### <a name="offer"></a>Nabídka

Hello nabídka pro spravované aplikace odpovídá tooa třída produktu nabídky od vydavatele. Pokud máte nový typ řešení nebo aplikace, které chcete toomake k dispozici v hello Marketplace, můžete ho nastavit jako novou nabídku. Nabídka je kolekce SKU. Všechny nabídky se zobrazí jako vlastní entity v hello Marketplace.

### <a name="sku"></a>Skladová jednotka (SKU)

SKU je hello nejmenší nabízené ke koupi jednotka nabídku. Můžete použít SKU v rámci hello stejné toodifferentiate – třída (nabídka) produktu mezi:

* Různé funkce, které jsou podporovány.
* Nabídka hello toho, jestli spravované nebo nespravované.
* Modely fakturace, které jsou podporovány.

SKU se zobrazí pod hello nadřazené nabídka v hello Marketplace. Zobrazí se jako vlastní nabízené ke koupi entity v hello portálu Azure.

### <a name="set-up-an-offer"></a>Nastavit nabídky

1. Přihlaste se toohello [portál Cloud Partner](https://cloudpartner.azure.com/).

2. V navigačním podokně hello na levé straně hello vyberte **+ nové nabídky** > **aplikací Azure**.

    ![Nová nabídka](./media/managed-application-author-marketplace/newOffer.png)

3. Vyplnění hello formulářů, které se zobrazují na hello left v hello **Editor** zobrazení. Povinná pole jsou označené červenou hvězdičkou (*).

    ![Nabídka nastavení](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    Použít toocreate spravované aplikace jsou čtyři hlavní formuláře:

    a. Nabídka nastavení

    b. SKU

    c. Marketplace

    d. Podpora

Tyto formuláře jsou popsané v následující části hello podrobněji.

## <a name="offer-settings-form"></a>Nabídka nastavení formuláře
Použijte tento základní formulář toospecify hello nabídky nastavení.

1. Vyplňte hello **nabízejí nastavení** formuláře. Hello různých polí jsou:

    a. **ID nabídky**: Tento jedinečný identifikátor identifikuje hello nabídka v rámci profilu vydavatele. Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy. Může být složené jenom malé alfanumerické znaky nebo pomlčky (-). Hello ID nemůže končit pomlčkou. Je omezená tooa maximálně 50 znaků. Po nabídku přejde za provozu, toto pole je uzamčené.

    b. **ID vydavatele**: pomocí tohoto rozevíracího seznamu toochoose hello vydavatele profilu chcete toopublish v rámci této nabídky. Po nabídku přejde za provozu, toto pole je uzamčené.

    c. **Název**: Tento název zobrazení pro danou nabídku se zobrazí v hello Marketplace a hello portálu. Může mít maximálně 50 znaků. Zahrnují srozumitelný název značky pro svůj produkt. Nezadávejte název společnosti, pokud to není, jak je na trh. Pokud jste marketingové v rámci této nabídky na vlastní web, ujistěte se, zda text hello jméno přesně jak se zobrazuje na vašem webu.

2. Vyberte **Uložit** toosave průběh. 

## <a name="skus-form"></a>SKU formuláře
dalším krokem Hello je tooadd SKU pro vaši nabídku.

1. Vyberte **SKU** > **nové SKU**. 

    ![Vyberte nové SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. Zadejte **SKU ID**. SKU ID je jedinečný identifikátor SKU hello v rámci nabídku. Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy. Může být složené jenom malé alfanumerické znaky nebo pomlčky (-). Hello ID nemůže končit pomlčkou a je omezený tooa maximálně 50 znaků. Po nabídku přejde za provozu, toto pole je uzamčené. Můžete mít více SKU v rámci nabídku. Je nutné SKU pro každou bitovou kopii naplánovat toopublish.

3. Vyplňte hello **SKU podrobnosti** část hello následující formulář:

    ![Zadejte nové SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    Vyplňte následující pole hello:
    
    a. **Název**: Zadejte název pro tento SKU. Tento název se zobrazí v galerii hello pro tuto položku.

    b. **Souhrn**: Zadejte krátký souhrn pro tento SKU. Tento text se zobrazí pod položkou Název hello.

    c. **Popis**: Zadejte podrobný popis o hello SKU.

    d. **Typ SKU**: hello povolené hodnoty jsou **spravované aplikace** a **šablony řešení**. Pro tento případ, vyberte **spravované aplikace**.

4. Vyplňte hello **podrobnosti balíčku** část hello následující formulář:

    ![Balíček](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    Vyplňte následující pole hello:

    a. **Aktuální verze**: Zadejte verzi balíčku hello můžete nahrávat na server. Musí být ve formátu hello `{number}.{number}.{number}{number}`.

    b. **Vyberte soubor balíčku**: Tento balíček obsahuje následující soubory, které jsou komprimované a do souboru .zip hello:
    * **applianceMainTemplate.json**: hello soubor šablony nasazení, který byl použit toodeploy hello řešení nebo aplikace. Informace o tom najdete v části soubory šablony nasazení toocreate, [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).
    * **appliancecreateUIDefinition.json**: Tento soubor je používán hello Azure portálu toogenerate hello uživatelské rozhraní, které se používá tooprovision toto řešení aplikace. Další informace najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).
    * **mainTemplate.json**: Tento soubor šablony obsahuje pouze hello Microsoft.Solution/appliances prostředků. Hello mainTemplate soubor obsahuje hello následující vlastnosti:

        *  **druh**: použití **Marketplace** pro spravované aplikace v hello Marketplace.
        *  **ManagedResourceGroupId**: Tato skupina prostředků v předplatném hello zákazníka je, kde jsou všechny prostředky hello definované v applianceMainTemplate.json nasazen.
        *  **PublisherPackageId**: Tento řetězec jednoznačně identifikuje hello balíčku. Zadejte hodnotu hello ve formátu hello `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.

Získat hello **nabízejí ID** a **ID vydavatele** z hello publikování portálu, jak ukazuje následující obrázek hello:

![ID nabídky](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
Získat hello **SKU ID**, jak ukazuje následující obrázek hello:

![SKU ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
Získat balíček hello **verze**, jak ukazuje následující obrázek hello:

![Verze balíčku](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  Podle předchozích příklady hello, hello hodnotu **PublisherPackageId** je `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.

  Ukázka mainTemplate.json:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify hello name of hello storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

Tento balíček měl obsahovat všechny vnořené šablony nebo skripty, které jsou požadované toosuccessfully zřídit této aplikace. Hello mainTemplate.json, applianceMainTemplate.json a applianceCreateUIDefinition.json soubory musí být umístěny v kořenové složce hello.

* **Povolení**: definuje tato vlastnost, který získá přístup a hello úroveň přístupu toohello prostředky v rámci předplatných zákazníků. Vydavatel Hello ho můžete použít aplikace hello toomanage jménem zákazníka hello.
* **PrincipalId**: Tato vlastnost je identifikátor hello Azure Active Directory (Azure AD) uživatele, skupiny uživatelů nebo aplikací, který má oprávnění určité hello prostředky v předplatné hello zákazníka. Hello definice Role popisuje oprávnění, hello. 
* **Definice role**: Tato vlastnost je seznam všech hello předdefinované řízení přístupu na základě Role (RBAC) rolí poskytuje Azure AD. Můžete vybrat hello role, která je nejvhodnější toouse toomanage hello prostředky jménem zákazníka hello.

Můžete přidat více oprávnění. Doporučujeme vytvořit skupinu uživatele AD a zadejte své ID v **PrincipalId**. Tímto způsobem můžete přidat další skupiny uživatelů toohello uživatelé bez hello nutné tooupdate hello SKU.

Další informace o RBAC najdete v tématu [začít pracovat s RBAC v hello portál Azure](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Formulář Marketplace.

Hello Marketplace formuláře požádá o pole, které se zobrazí na hello [Azure Marketplace](https://azuremarketplace.microsoft.com) a na hello [portál Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>ID odběru Preview

Zadejte seznam předplatného Azure ID, kteří mohou přistupovat k hello nabídka po publikování. Můžete použít tyto odběry uvedené prázdné tootest hello zobrazení náhledu nabídka před jeho provedením za provozu. Prázdný seznam až too100 předplatná v portálu pro partnery hello můžete zkompilovat.

### <a name="suggested-categories"></a>Navrhované kategorií

Vyberte kategorie toofive hello seznamu, které vaši nabídku může být nejlépe přidružené. Tyto kategorie jsou použité toomap kategorie produktů toohello vaší nabídky, které jsou k dispozici v hello [Azure Marketplace](https://azuremarketplace.microsoft.com) a hello [portál Azure](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

Zobrazí souhrn Hello spravované aplikace hello následující pole:

![Souhrn Marketplace](./media/managed-application-author-marketplace/publishvm10.png)

Hello **přehled** kartě pro zobrazení spravované aplikace hello následující pole:

![Přehled webu Marketplace](./media/managed-application-author-marketplace/publishvm11.png)

Hello **plány + cenová** kartě pro zobrazení spravované aplikace hello následující pole:

![Plány Marketplace.](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a>portál Azure

Zobrazí souhrn Hello spravované aplikace hello následující pole:

![Portál souhrn](./media/managed-application-author-marketplace/publishvm12.png)

Přehled Hello pro spravované aplikace zobrazí hello následující pole:

![Přehled portálu](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a>Logu

Postupujte podle těchto pokynů pro všechny logo, který nahrajete na portálu Cloud Partner hello:

*   Hello Azure návrhu má palety barev jednoduché. Omezit počet hello primární a sekundární barev v loga.
*   barev motivů Hello hello portálu jsou bílé a černé. Nepoužívejte tyto barvy jako barvu pozadí hello loga. Použijte barvu, která umožňuje loga viditelného hello portálu. Doporučujeme, abyste jednoduché primární barvy. *Pokud používáte průhledné pozadí, ujistěte se, že hello logem a text nejsou bílé, černé nebo blue.*
*   Nepoužívejte barevného přechodu pozadí na hello logo.
*   Neukládejte text do hello logo, ani vaše společnost nebo název značky. Hello vzhled a chování vaše logo musí být plochý a vyhnout se přechody.
*   Zajistěte, aby hello logo není roztažená.

#### <a name="hero-logo"></a>Logo nejdůležitější

logo hrdina Hello je volitelné. Vydavatel Hello můžete zvolit není tooupload logo nejdůležitější. Po odeslání hello hrdina ikonu nelze odstranit. V ten moment se musí splňovat hello partnera hello Marketplace pravidla pro nejdůležitější ikony.

Postupujte podle těchto pokynů pro ikonu logo hrdina hello:

*   Název zobrazení Hello vydavatele, název plánu hello a hello nabídka dlouhé shrnutí jsou zobrazeny v prázdné. Proto nepoužívejte světla barvu pozadí hello ikony hrdina hello. Černé, bílé nebo průhledné pozadí není povolena pro nejdůležitější ikony.
*   Po hello nabídka je uvedena, hello vydavatele zobrazované jméno, název plánu hello, hello nabídka dlouhé shrnutí a hello **vytvořit** tlačítko jsou uvnitř hello hrdina logo vložených prostřednictvím kódu programu. V důsledku toho není zadat libovolný text při návrhu logo hrdina hello. Ponechte prázdné místo hello správné protože textu hello je do tohoto místa zahrnuto prostřednictvím kódu programu. Hello prázdné místo pro hello text by mělo být 415 × 100 pixelů na hello správné. Posunut 370 pixelů zleva hello.

    ![Příklad logo nejdůležitější](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a>Podpora formuláře

Vyplňte hello **podporu** formuláře s podporou kontaktuje z vaší společnosti. Tyto informace může technici, kontakty a kontakty podpory zákazníků.

## <a name="publish-an-offer"></a>Publikování nabídky

Po vyplnění všech oddílů hello, vyberte **publikovat** toostart hello proces, který je k dispozici toocustomers vaší nabídky.

## <a name="next-steps"></a>Další kroky

* Úvod toomanaged aplikace naleznete v [spravované aplikace přehled](managed-application-overview.md).
* Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).
* Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
* Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
