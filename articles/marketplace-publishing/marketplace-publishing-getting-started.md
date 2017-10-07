---
title: "aaaOverview jak toocreate a nasazení toohello nabídku Marketplace | Microsoft Docs"
description: "Hello kroky požadované toobecome schválené Microsoft developer pochopit a vytvářet a nasazovat bitové kopie virtuálního počítače, šablony, data služby nebo služby pro vývojáře v hello Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.openlocfilehash: ac5480b98b8b1021a595db951ed9c974f74415dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-and-manage-an-offer-in-hello-azure-marketplace"></a>Publikovat a spravovat nabídku v hello Azure Marketplace
Toohelp vývojářům vytvářet, nasazovat a spravovat svá řešení uvedené v hello Azure Marketplace pro další Azure zákazníci a partneři toopurchase a použít je k dispozici v tomto článku.

## <a name="marketplace-publishing"></a>Publikování webu Marketplace
Jako Azure vydavatele můžete distribuovat a prodej inovativní řešení nebo vývojáři tooother služeb, nezávislí výrobci softwaru, a IT profesionály při hello Marketplace. Prostřednictvím hello Marketplace, můžete uživatele oslovit zákazníky, kteří chtějí tooquickly vyvíjet jejich cloudové aplikace a mobilní řešení. Pokud vaše řešení cílem podnikoví uživatelé, můžete chtít tooconsider hello [AppSource](http://appsource.microsoft.com) marketplace.


## <a name="supported-types-of-solutions"></a>Podporované typy řešení
Hello první věc, kterou chcete toodo jako vydavatel je toodefine, jaký druh řešení nabídka je vaší společnosti. Hello Marketplace podporuje následující typy nabízí hello:

|Typ řešení|Virtuální počítač|Šablona řešení|
|---|---|---|
|**Definice**|Předkonfigurované obrázky s plně nainstalovaného operačního systému a jednu nebo více aplikací. Image virtuálního počítače poskytuje hello informace nezbytné toocreate a nasazení virtuálních počítačů v hello služby Azure Virtual Machines.|Datová struktura, která může odkazovat na jeden nebo více služeb odlišné Azure, včetně služeb publikovat další prodejci. Předplatitelé služby Azure můžete použít toodeploy jeden nebo více nabídek jeden, koordinovaným způsobem.|
|**Příklad**|Jako Azure vydavatele jste vytvořili a ověřit virtuálního počítače pomocí služby inovativní databáze. Další předplatitelé služby Azure má tooprocure a nasadit tento virtuální počítač do jejich prostředí cloudové služby.|Jako Azure vydavatele jste dodávat sadu služeb z napříč Azure, aby byla rychlé toodeploy cloudových služeb s Vyrovnávání zatížení, lepší zabezpečení a vysokou dostupnost. Další předplatitelé služby Azure můžete ušetřit čas tím nákup šablona hello řešení, který splňuje svůj cíl. Nemají toomanually hledání, zakoupit, nasazení a konfigurace hello stejné nebo podobné služby Azure.|

> [!NOTE]
> Některé kroky jsou sdílené mezi různé typy hello řešení a jiné jsou odlišné toohello příslušného typu řešení. Tento článek poskytuje stručný přehled kroků hello potřebujete toocomplete k libovolnému typu řešení.

## <a name="publish-a-solution"></a>Publikování řešení
![Určit, registrace, publikování](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Jmenovat řešení pro předběžné schválení
toopublish virtuálního počítače [řešení](https://createopportunity.azurewebsites.net) toohello Marketplace, Microsoft Azure certifikaci kompletní hello **řešení navrženém formuláře**.

>[!NOTE]
> Pokud pracujete s partnera účet manažera nebo partnera správce Directx, požádejte je, toonominate řešení Azure certifikované programu hello. Můžete také přejít toohello [Microsoft Azure certifikované](http://createopportunity.azurewebsites.net) webovou stránku a vyplňte out hello formulář žádosti. Zadejte e-mailu hello partnera Account manažera nebo partnera Manager Directx v hello **Microsoft sponzor kontaktujte** pole.

Pokud splňují kritéria způsobilosti hello v hello [zásady zapojení Azure Marketplace](http://go.microsoft.com/fwlink/?LinkID=526833) a aplikace je schválená, můžeme začít pracovat s tooonboard je vaše řešení toohello Marketplace.

### <a name="register-your-account-as-a-microsoft-seller"></a>Registrace účtu jako prodejce Microsoft
Registrace účtu Microsoft jako [účtu Microsoft Developer](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Publikování řešení
toopublish řešení toohello Marketplace, postupujte takto:
1. Splnění požadavků hello běžné uživatele.

    a. Splnění hello [běžné uživatele požadavky](marketplace-publishing-pre-requisites.md).

    b. Splnění hello [technické požadavky virtuálního počítače](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Splnění hello [technické předpoklady šablony řešení](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Vytvořte nabídku.

    a. Vytvoření [virtuální počítač](marketplace-publishing-vm-image-creation.md) nabízejí.

    b. Vytvoření [šablona řešení](marketplace-publishing-solution-template-creation.md) nabízejí.

3. Vytvořit nabídku [marketingové obsah](marketplace-publishing-push-to-staging.md).

4. Při přípravě otestujte vaši nabídku.

    a. Otestovat vaši nabídku virtuálních počítačů v [pracovní](marketplace-publishing-vm-image-test-in-staging.md).

    b. Otestovat vaši nabídku šablony řešení v [pracovní](marketplace-publishing-solution-template-test-in-staging.md).

5. Nasazení vaší nabídka toohello [Marketplace](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Vytvářet a spravovat bitové kopie virtuálního počítače
Vytvoření a správa image virtuálního počítače pomocí těchto prostředků:
* Vytvoření image virtuálního počítače [místní](marketplace-publishing-vm-image-creation-on-premise.md).
* Vytvoření virtuálního počítače se systémem [Windows v portálu Azure hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Vytvoření virtuálního počítače se systémem [Linux v hello portál Azure](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Řešení běžných problémů s došlo během [vytvoření virtuálního pevného disku](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Spravovat vaše řešení
Spravovat vaše řešení pomoci z hello následující prostředky:
* [Nabízí čtení hello po Provozní příručka pro virtuální počítač](marketplace-publishing-vm-image-post-publishing.md)
* [Aktualizovat podrobnosti běžné uživatele hello nabídku nebo SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Aktualizovat hello technické podrobnosti o nabídku nebo SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Přidat nové skladová položka v seznamu nabídky](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Změnit počet datových disků pro uvedené SKU hello](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [Odstranit uvedené nabídka z Marketplace hello](marketplace-publishing-vm-image-post-publishing.md)
* [Odstranit uvedené SKU z hello Marketplace.](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [Odstranit hello aktuální verzi uvedené SKU z hello Marketplace.](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [Vrátit hello výpis tooproduction hodnoty cen](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Vrátit hello fakturační model tooproduction hodnoty](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [Obnovit nastavení viditelnosti hello uvedené hodnoty produkce toohello SKU](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)
* [Změnit incentive prodejce, u vašeho poskytovatele Cloud Solution Provider](marketplace-publishing-csp-incentive.md)
* [Pochopení výběr sestav](marketplace-publishing-report-payout.md)
* [Získat podporu jako vydavatel](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>Další zdroje
[Nastavení prostředí Azure PowerShell](marketplace-publishing-powershell-setup.md)
