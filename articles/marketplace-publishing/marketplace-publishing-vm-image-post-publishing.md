---
title: "aaaManaging bitové kopie virtuálního počítače v hello Azure Marketplace | Microsoft Docs"
description: "Podrobný průvodce na tom, jak toomanage virtuálního počítače bitové kopie v Azure Marketplace hello po počáteční publikace"
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: 47a082e686e1248ceacb844d3c0f2f5c33133dab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="post-production-guide-for-virtual-machine-offers-in-hello-azure-marketplace"></a>Po Provozní příručka pro virtuální počítač nabízí v hello Azure Marketplace
Tento článek vysvětluje, jak můžete aktualizovat nabídka migraci virtuálního počítače v hello Azure Marketplace. Provede vás procesem hello přidávání jeden nebo více nových SKU tooan stávající nabídky. Také provede vás procesem hello odebrání nabídka migraci virtuálního počítače nebo SKU z hello Marketplace.

Po nabídka nebo SKU je dvoufázové instalace v hello [portál Azure](http://portal.azure.com), nelze změnit hello následujících polí:

* **Nabízejí identifikátor**: hello publikování portálu, přejděte v příliš**virtuální počítače** a vyberte vaši nabídku. Pak klikněte na tlačítko **Image virtuálních počítačů** > **nabízejí identifikátor**.
* **Identifikátor SKU**: hello publikování portálu, přejděte v příliš**virtuální počítače** a vyberte vaši nabídku. Pak klikněte na tlačítko **SKU** > **přidat SKU**.
* **Vydavatel Namespace**: hello publikování portálu, přejděte v příliš**virtuální počítače** > **návod** > **Řekněte nám o vaše společnost** (tuto možnost najdete v části "Krok 2 zaregistrovat vaše společnost") > **vydavatele Namespace** > **Namespace**.

Po hello nabídka nebo SKU je uvedena v hello [Marketplace](http://azure.microsoft.com/marketplace), nelze změnit hello následujících polí:

* **Nabízejí identifikátor**: hello publikování portálu, přejděte v příliš**virtuální počítače** a vyberte vaši nabídku. Pak klikněte na tlačítko **Image virtuálních počítačů** > **nabízejí identifikátor**.
* **Identifikátor SKU**: hello publikování portálu, přejděte v příliš**virtuální počítače** a vyberte vaši nabídku. Pak klikněte na tlačítko **SKU** > **přidat SKU**.
* **Vydavatel Namespace**: hello publikování portálu, přejděte v příliš**virtuální počítače** > **návod** > **Řekněte nám o vaše společnost** (tuto možnost najdete v části "Krok 2 Register") **Vydavatele Namespace** > **Namespace**.
* **Porty**: hello publikování portálu, přejděte v příliš**virtuální počítače** a vyberte vaši nabídku. Pak klikněte na tlačítko **Image virtuálních počítačů** > **otevřete porty**.
* **Změna seznamu SKU(s) – ceny**
* **Změna modelu fakturace uvedené SKU(s)**
* **Odebrání fakturace oblasti uvedené SKU(s)**
* **Počet uvedených SKU(s) na disku se měnícími se daty hello**

## <a name="update-hello-technical-details-of-a-sku"></a>Aktualizovat hello technické podrobnosti SKU
tooadd nové verze toohello uvedené SKU a znovu publikovat vaši nabídku, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **Image virtuálních počítačů** kartě.
4. V hello **SKU** část, vyhledejte hello SKU, které chcete tooupdate.
5. Přidat nové číslo verze pro hello SKU a klikněte na hello ** + ** tlačítko. nové verze Hello musí být ve formátu X.Y.Z, kde X, Y a jsou celá čísla. Verze změny by měly být jenom přírůstkové.
6. V hello **URL virtuálního pevného disku operačního systému** zadejte hello sdílený přístupový podpis URI vytvořený pro hello operační systém virtuálního pevného disku a uložit změny hello.

   > [!IMPORTANT]
   > Můžete nelze přírůstek nebo snížení hello počet datových disků z uvedených SKU. V takovém případě musíte toocreate nové SKU. Podrobné pokyny najdete v tématu část toohello [přidat nové skladová položka v seznamu nabídky](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Image virtuálních počítačů](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-hello-nontechnical-details-of-an-offer-or-a-sku"></a>Aktualizovat podrobnosti běžné uživatele hello nabídku nebo SKU
Můžete aktualizovat hello běžné uživatele (marketing, právních, podpora, kategorie) podrobností o provozu nabídka nebo SKU v hello Marketplace.

### <a name="update-hello-offer-description-and-logos"></a>Popis nabídky hello aktualizace a loga
tooupdate hello nabízejí podrobnosti a znovu publikovat vaši nabídku, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **MARKETING** kartě.
4. Klikněte na tlačítko **Angličtina (USA)**.
5. Klikněte na tlačítko hello **podrobnosti** kartě. V hello **popis** část, aktualizace hello nabídka **název**, **Souhrn**, a **DLOUHÝ Souhrn** a hello změny uložte.

   > [!NOTE]
   > Při aktualizaci hello SKU podrobnosti, mějte na paměti tato omezení: 
   * Nezadávejte duplicitní text pro popis nabídky hello a popis SKU hello.
   * Nezadávejte text duplicitní název SKU hello a hello nabídka dlouhé shrnutí. 
   * Nezadávejte duplicitní text pro nadpis hello SKU a souhrn nabídka hello.
   >
   >

    ![Podrobnosti](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. V hello **loga** části hello **podrobnosti** aktualizujte hello loga. Ujistěte se, že hello loga podle hello [pokyny pro Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > Ikona nejdůležitější je volitelný. Můžete není tooupload ikonou nejdůležitější. Po odeslání ikonou nejdůležitější neexistuje však žádná toodelete zřídit z hello publikování portálu. Postupujte podle hello [nejdůležitější ikonu pokyny](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Loga](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-hello-sku-description"></a>Popis SKU hello aktualizace
tooupdate hello SKU údaje a znovu publikovat nabídku, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **MARKETING** kartě.
4. Klikněte na tlačítko **Angličtina (USA)**.
5. Klikněte na tlačítko hello **plány** kartě. V hello **SKU** část, aktualizujte hello SKU **název**, **Souhrn**, a **popis** a hello změny uložte.

   > [!NOTE]
   > Při aktualizaci hello SKU podrobnosti, mějte na paměti tato omezení: 
   * Nezadávejte duplicitní text pro popis nabídky hello a popis SKU hello. 
   * Nezadávejte text duplicitní název SKU hello a hello nabídka dlouhé shrnutí. 
   * Nezadávejte duplicitní text pro nadpis hello SKU a souhrn nabídka hello.
   >
   >
6. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Plány](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Změnit existující odkazy nebo přidat nové odkazy
existující odkazy toochange nebo přidat nové odkazy a poté jej znovu publikovat vaši nabídku, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **MARKETING** kartě.
4. Klikněte na tlačítko **Angličtina (USA)**.
5. Klikněte na tlačítko hello **odkazy** kartě.
6. tooadd nové propojení, v hello **odkazy** klikněte na tlačítko **+ přidat odkaz**. V hello **přidat odkaz** dialogovém okně zadejte odkaz hello **název** a **URL** a hello změny uložte. Můžete zadat všechny odkaz, který obsahuje informace, které můžou pomoct zákazníků.
7. tooupdate nebo odstranit existující odkaz, vyberte odkaz hello a klikněte na tlačítko hello **upravit** tlačítko nebo hello **odstranit** tlačítko.

   > [!NOTE]
   > Zkontrolujte, že hello odkazy, které jste zadali v této části jsou funguje správně, protože tyto odkazy získat ověření během zpracování žádosti o vaše produkční.
   >
   >
8. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Odkazy](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Přidání odkazu](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Změňte stávající image ukázka nebo přidat novou bitovou kopii ukázka
toochange existující vzorek bitovou kopii nebo přidat nové ukázkové obrázky a poté jej znovu publikovat vaši nabídku, postupujte takto:

> [!NOTE]
> Ukázkový pouze jeden obrázek se zobrazí v hello [portál Azure](https://portal.azure.com).
>
>

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **MARKETING** kartě.
4. Klikněte na tlačítko **Angličtina (USA)**.
5. Klikněte na tlačítko hello **UKÁZKOVÉ obrázky** kartě.
6. tooadd novou bitovou kopii ukázkové, v hello **ukázkové obrázky** klikněte na tlačítko **NAHRÁT novou IMAGE A** a hello změny uložte.

   > [!NOTE]
   > Včetně ukázkového obrázku je volitelný.
   >
7. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Ukázkové obrázky](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-hello-legal-content"></a>Aktualizovat obsah právní hello
tooupdate hello právní obsah a znovu publikovat vaši nabídku, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **MARKETING** kartě.
4. Klikněte na tlačítko **Angličtina (USA)**.
5. Klikněte na tlačítko hello **právní** kartě. V hello **právní** část, aktualizovat vaše zásady nebo podmínky použití. Zadejte nebo vložte hello zásady nebo podmínky v hello **podmínky použití** pole a hello změny uložte.
6. Hello limit pro počet znaků pro hello právní podmínky použití je 1 milion znaků.
7. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Právní informace](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-hello-support-information"></a>Aktualizovat informace o podpoře hello
tooupdate hello informace o podpoře a znovu publikovat vaši nabídku, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **podporu** kartě.
4. V hello **Engineering obraťte se na** část aktualizace hello engineering kontaktní údaje. Tyto informace se používají pro interní komunikaci mezi hello partnera a Microsoft jenom.
5. V hello **zákaznickou podporu** část, aktualizace hello podporu kontaktní údaje a hello **podporu URL**. Tyto informace se používají pro interní komunikaci mezi hello partnera a Microsoft jenom.

   > [!NOTE]
   > Pokud chcete pouze e-mailová podpora tooprovide, zadejte fiktivní telefonní číslo v hello **zákaznickou podporu** části. V takovém případě se místo toho používá hello e-mailu, který jste zadali.
   >
   >
6. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Podpora](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-hello-categories"></a>Kategorie hello aktualizací
tooupdate hello kategorií část vaši nabídku a znovu publikovat nabídku, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **kategorie** kartě.
4. V hello **kategorie** část, aktualizovat hello kategorie pro vaši nabídku a uložit změny hello. Můžete si toofive kategorií pro galerii hello Azure Marketplace.
5. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
6. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![Kategorie](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Přidat nové skladová položka v seznamu nabídky
tooadd nové SKU do vaší nabídky za provozu, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **SKU** kartě. Pak klikněte na tlačítko **přidat SKU**. 
4. V dialogovém okně hello, zadejte **identifikátor SKU** na malá písmena. Vyberte hello **přineste si vlastní licenci (BYOL) fakturační model** zaškrtávací políčko, pokud chcete, aby toopublish hello nové SKU s fakturační model BYOL. Jinak zrušte zaškrtnutí políčka hello. Klikněte na tlačítko hello osové značky toocreate nové SKU. Pokud jste nezvolili fakturační model BYOL hello, hello fakturační model bude automaticky nastavena toohourly. Pokud chcete hello 30denní bezplatné zkušební verze pro hello hodinové fakturační model, vyberte **jeden měsíc** pro **je k dispozici bezplatná zkušební verze?** Jinak vyberte možnost **ne zkušební verze**. (**Je k dispozici bezplatná zkušební verze? ** se zobrazí jenom v případě, že jste nevybrali BYOL, při vytváření nové SKU hello.)

   > [!IMPORTANT]
   > **Skrýt tento SKU z hello Marketplace, protože by měl koupili vždy prostřednictvím šablonu řešení** by měla být **Ano** *pouze* jste schválení pro publikování šablonu řešení. Jinak, tato možnost by měla být vždy **ne**.
   >
   >
4. V nabídce hello hello vlevo, klikněte na tlačítko hello **Image virtuálních počítačů** kartě a zjistěte hello nové SKU, kterou jste vytvořili.
5. tooset až hello nové SKU, najdete v části [získat certifikační pro bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) pokyny.
6. tooadd marketingových materiálů pro hello nové SKU, najdete v části [poskytují Marketplace marketingové obsah](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. informace o cenách za tooadd hello nové SKU najdete v tématu [nastavit vaše ceny](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

    ![SKU](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Přidat SKU](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-hello-data-disk-count-for-a-listed-sku"></a>Změnit počet datových disků pro uvedené SKU hello
Můžete nelze přírůstek nebo snížení hello počet datových disků z uvedených SKU. V takovém případě musíte toocreate nové SKU. Podrobné pokyny najdete v tématu část toohello [přidat nové skladová položka v seznamu nabídky](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-hello-marketplace"></a>Odstranit uvedené nabídka z Marketplace hello
Různé aspekty potřebovat toobe postaráno v případě hello tooremove požadavek na nabídku za provozu. tooget pokynů od tooremove tým podpory hello uvedené nabídka z hello Marketplace, postupujte takto:

1. Vyvolat lístek podpory na hello [vytvořit incident](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) stránky.

2. Vyberte **typ problému** jako **Správa nabízí**a vyberte **kategorie** jako **úprava nabídka nebo skladová položka již v produkčním prostředí**.
3. Odešlete žádost o hello.

tým podpory Hello vás provede proces odstraňování hello nabídka nebo SKU.

> [!NOTE]
> Když se stav Koncept (ale ne pracovním nebo produkčním), můžete odstranit vždy nabídka hello. Na hello **HISTORIE** , klikněte na **ZAHODIT koncept**.
>
>

## <a name="delete-a-listed-sku-from-hello-marketplace"></a>Odstranit uvedené SKU z hello Marketplace.
toodelete uvedené SKU z hello Marketplace, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).

2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V podokně hello na levé straně hello, klikněte na hello **SKU** kartě.
4. Vyberte hello SKU má toodelete a klikněte na tlačítko hello **odstranit** tlačítko.
5. Přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish hello nabídka v hello Marketplace.
6. Po hello nabídka je publikovat v hello Marketplace, hello SKU je odstraněn z hello Marketplace a hello portálu Azure.

## <a name="delete-hello-current-version-of-a-listed-sku-from-hello-marketplace"></a>Odstranit hello aktuální verzi uvedené SKU z hello Marketplace.
toodelete hello aktuální verzi uvedené SKU z hello Marketplace, postupujte takto: 

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).

2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **Image virtuálních počítačů** kartě.
4. Vyberte hello SKU, jejíž aktuální verze toodelete a klikněte na hello **odstranit** tlačítko.
5. Přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish hello nabídka v hello Marketplace.
6. Po hello nabídka získá publikovat v hello Marketplace, hello aktuální verzi hello uvedené SKU je odstraněna z hello Marketplace a hello portálu Azure. Hello SKU je pak vrácena tooits předchozí verze.

## <a name="revert-hello-listing-price-tooproduction-values"></a>Vrátit hello výpis tooproduction hodnoty cen
toorevert hello výpis cena tooproduction hodnoty, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).
2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **cenová** kartě.
4. Vyberte oblast, jejichž ceny chcete tooreset.

    ![Ceny oblastí](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Pro SKU s hodinové fakturační model resetovat hello ceny pro všechny hello jader, jsou v produkčním prostředí pro vybranou oblast hello. Pro SKU s fakturační model BYOL, ujistěte se, hello SKU k dispozici v oblasti hello výběrem hello zaškrtávací políčko pro hello SKU v hello **EXTERNALLY-LICENSED (BYOL) SKU dostupnosti** části.

    ![Cenových úrovních](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Klikněte na tlačítko **AUTOPRICE jiných trzích podle ceny ve Spojených státech**.

   > [!NOTE]
   > Popisek tlačítka Hello se může lišit v závislosti na hello oblast, kterou jste vybrali. Vzhledem k tomu, že jsme vybrali Spojených státech amerických, je označené hello tlačítko **AUTOPRICE ostatní trhů na základě ON ceny IN Spojené státy**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Na stránce 1 hello Autoprice průvodce, zvolte základní trhu hello a klikněte na hello **šipku** tlačítko.

    ![Základní trhu](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Na stránce 2, zvolte plány služeb a měřidla (jader) a klikněte na tlačítko hello **šipku** tlačítko.

    ![Služba plány a měřidla](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Na stránce 3, klikněte na **přepnutí všechny** tooselect všech oblastech. Nebo můžete ručně zaškrtněte políčka pro konkrétní oblasti. Klikněte na tlačítko hello **šipku** tlačítko.

    ![Zvolte trzích](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Na stránce 4, zkontrolujte hello kurzů a klikněte na **Dokončit**. Průvodce Hello obnoví hello ceny podle tooyour výběry.

11. Na hello **cenová** , klikněte na **zobrazit souhrn změny**.
    Pro **zobrazení verze**, vyberte **koncept**a pro **porovnání s**, vyberte **produkční**. Pokud se zobrazí žádné cenové rozdíly, ceny hello toohello produkční hodnoty vrátila úspěšně.

    ![Zobrazení souhrnu a změny](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
13. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

## <a name="revert-hello-billing-model-tooproduction-values"></a>Vrátit hello fakturační model tooproduction hodnoty
toorevert hello fakturační model tooproduction hodnoty, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).

2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **SKU** kartě.
4. Klikněte na tlačítko hello **upravit** tlačítko toorevert hello fakturační model. V otevřeném okně hello zaškrtněte nebo zrušte hello **fakturace a licencování provádí externě z Azure (neboli model použití vlastní licence)** zaškrtávací políčko.

    ![Upravit fakturace](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Postupujte podle kroků hello v "Hello vrátit výpis hodnoty cen tooproduction" v tomto článku.
6. Přejděte toohello **publikovat** a klikněte na **nabízené tooSTAGING**. Podrobné pokyny k testování vaši nabídku v hello pracovní prostředí, najdete v části [otestovat vaši nabídku virtuálních počítačů pro hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Po otestujete vaši nabídku v pracovní, přejděte toohello **publikovat** ve hello publikování portálu. Klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

## <a name="revert-hello-visibility-setting-of-a-listed-sku-toohello-production-value"></a>Obnovit nastavení viditelnosti hello uvedené hodnoty produkce toohello SKU
nastavení viditelnosti hello toorevert uvedené SKU toohello hodnoty produkce, postupujte takto:

1. Přihlaste se toohello [publikování portál](https://publish.windowsazure.com).

2. Přejděte toohello **virtuální počítače** a vyberte vaši nabídku.
3. V nabídce hello hello vlevo, klikněte na tlačítko hello **SKU** kartě.
4. Vyberte vaše SKU a obnovit nastavení viditelnosti hello hello SKU toohello produkční hodnotu.

    ![Viditelnost](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Jakmile jste hotovi s hello změny, klikněte na tlačítko **schválení žádosti o změnu tooPUSH tooPRODUCTION** toorepublish vaši nabídku v hello Marketplace.

## <a name="see-also"></a>Viz také
* [Začínáme GET: Publikování toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)
* [Pochopení výběr vytváření sestav](marketplace-publishing-report-payout.md)
* [Změnit incentive prodejce, u vašeho poskytovatele Cloud Solution Provider](marketplace-publishing-csp-incentive.md)
* [Řešení běžných potíží publikování v hello Marketplace.](marketplace-publishing-support-common-issues.md)
* [Získat podporu jako vydavatel](marketplace-publishing-get-publisher-support.md)
* [Vytvoření image virtuálního počítače na místě](marketplace-publishing-vm-image-creation-on-premise.md)
* [Vytvoření virtuálního počítače se systémem Windows na portálu Azure preview hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
