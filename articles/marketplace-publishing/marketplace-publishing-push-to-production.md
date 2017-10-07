---
title: "aaaDeploy toohello vaši nabídku Azure Marketplace | Microsoft Docs"
description: "Další informace o a provede hello pokyny toodeploy vaši nabídku – bitovou kopii virtuálního počítače, služby pro vývojáře, služba data atd – toohello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: hascipio
ms.openlocfilehash: ab0bb7c78020187505c2d5f09c4de246987ecd97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-offer-toohello-azure-marketplace"></a>Nasazení vaší toohello nabídka Azure Marketplace
Až budete spokojeni s vaši nabídku (tedy testování zákazníka scénáře marketingové obsah, atd.) a jsou připravené toolaunch požadavku **Push tooproduction** na hello **publikovat** kartě.  

1. Hello čtyři kroky v části hello návod stránku hello publikování portál musí být dokončena a zelená. Pro virtuální počítač nabízí zajistěte dodržíte tento hello následující pokyny.
   
    ![Kreslení][img-pubportal-walkthru-checked]
2. Vyberte hello **publikovat** kartě hello seznamu na levé straně hello.
   
    ![Kreslení][img-pubportal-menu-publish]
3. Klikněte na tlačítko hello **požádat o schválení toopush tooproduction**. Jakmile hello požadavku, hello schválení team provede kontrolu konečné a pak budou k dispozici v Azure Marketplace hello vaši nabídku.
   
    ![Kreslení][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> V případě virtuálních počítačů když kliknete na hello tlačítko žádost o schválení toopush tooproduction, jsou hello následující kroky provést za scény hello. Bude moct tooview hello průběh jednotlivých kroků v části kartu hello publikovat v hello publikování portálu. Je nutné zkontrolovat tuto stránku v pravidelných intervalech (dokud hello stavu zobrazuje "Uvedené") pro všechny informace o selhání, které musí oprava z vaší elementu end.
> 
> * Na první žádost produkční přejde toohello certifikační týmu, který ověření hello virtuálního pevného disku. Ale pokud jsou aktualizace již uvedené nabídku a hello požadavek má potom pouze marketingové změnit, pak hello certifikační krok se přeskočí.
> * V dalším kroku hello pocházet hello žádost týmu toohello ověření obsahu, který ověřte hello marketingové obsah nabídka hello.
> * Pokud hello výše kroky úspěšné, je schválen hello nabídka v produkčním prostředí. V tomto okamžiku hello stav stane "uvedených" hello publikování portálu. Tento stav "Uvedené" však neznamená, že byl dokončen proces hello. Následující kroky nutné toobe dokončení před hello nabídka Hello je k dispozici v hello Azure Marketplace.
> * Po schválení hello nabídka v produkčním prostředí v předchozí krok text hello replikace hello nabídka start napříč všemi hello datových centrech Azure. Obvykle trvá 24 48hours pro toocomplete hello replikace, ale může trvat až tooa týden v závislosti na velikosti hello hello virtuálního pevného disku. Pokud aktualizujete již uvedené nabídku a má tu, pouze marketingové změnit, pak hello replikace je ale rychlejší.
> * Po dokončení replikace hello hello nabídka bude k dispozici v hello Azure Marketplace.
> 
> Nabídka hello můžete vždy odstranit, když je v **koncept** stav (tj, nikdy **Push toostaging** nebo **Push tooproduction**). Na hello **historie** , klikněte na hello **zahodit koncept** tlačítko dole hello toodelete stránku hello koncept.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Produkční kontrolní seznam pro všechny nabídky virtuálního počítače
* Zajistěte, aby vám partnera Microsoft Azure Certified
* Kartě SKU hello hello možnost "Skrýt tuto SKU z hello Marketplace vzhledem k tomu, že by měl koupili vždy prostřednictvím šablonu řešení" by měl být označen jako Ano pouze pokud hello SKU je součástí šablonu řešení. Ve všech hello ostatních případech, tato možnost by měla být vždy označena jako číslo.
* Mějte na paměti: Jakmile hello skladová položka je uvedena byste neměli měnit nastavení viditelnosti SKU hello. Tato funkce nepodporujeme.
* Ujistěte se, že hello loga splňovat níže uvedené pokyny pro logo toohello Azure Marketplace.
* Nabídky a SKU popis nesmí být stejné.
* Název společnosti SKU a nabízejí dlouhý Souhrn nesmí být stejné.
* Název SKU a nabízejí Souhrn nesmí být stejné.
* Názvy SKU by neměl být identické nabídku více skladových položek.

**Azure Marketplace logu**

* Hello Azure návrhu má palety barev jednoduché. Zachovat hello počet primární a sekundární barvy na vaše logo nízkou.
* Hello barev motivu hello portálu Azure jsou bílé a černé. Proto Vyhněte se používání těchto barvy jako barva pozadí hello vašeho loga. Pomocí některé barvu, která by zkontrolujte, že vaše loga viditelného v hello portálu Azure. Doporučujeme, abyste jednoduché primární barvy. Pokud používáte průhledné pozadí, ujistěte se, že hello logo/text není bílé nebo blokovaných.
* Nepoužívejte hello logo barevného přechodu pozadí.
* Vyhněte se umístění text, i vaše společnost nebo název značky na hello logo.
* Hello vzhled a chování vaše logo musí být 'ploché' a byste neměli přechody.
* Hello logo nesmí dojít k roztažení.

**Další pokyny pro hello hrdina loga:**

* logo hrdina Hello je volitelné. Vydavatel Hello můžete zvolit není tooupload logo nejdůležitější. **Ikona hrdina ale jednou nahrané hello nelze odstranit z hello publikování portálu. V té době hello partnera musí splňovat pravidla Azure Marketplace hello hrdina ikony else nabídka hello nepůjde schválené tooproduction.**
* Hello zobrazovaný název vydavatele, název SKU a hello nabízejí dlouhý souhrn se zobrazují v barva bílé písma. Proto byste neměli uchování jakékoli světla barev v pozadí hello hello hrdina ikonu. Černé, bílé a průhledné pozadí není povolena pro nejdůležitější ikony.
* Hello zobrazovaný název vydavatele, název SKU, hello nabídka dlouhé shrnutí a hello vytvořit tlačítko jsou prostřednictvím kódu programu vložená do hello hrdina logo po hello nabídka přejde uvedené. Při návrhu hello hrdina logo, proto by neměl zadat libovolný text. Nechte prázdné místo v pravé hello, protože hello textu (tj. zobrazovaný název vydavatele, název SKU, hello nabídka dlouhé shrnutí) budou zahrnuty prostřednictvím kódu programu tím, abychom přes zde. Hello prázdné místo pro hello text by mělo být 415 × 100 na hello vpravo (a posunut 370px zleva hello).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Nabízí další produkční kontrolní seznam pro již uvedené virtuálního počítače
* Zkontrolujte, že pokud je již nabídku s hello stejný název nabízejí z vaší společnosti. Pokud ano, měli byste v hello existující nabídka místo vytvoření nové duplicitní nabídka přidat novou verzi hello SKU.
* Datový disk neměli měnit mezi dvěma verzemi hello stejné SKU.
* Hello Azure Marketplace nepodporuje ceny změnu hello uvedené SKU, jako je ovlivňuje hello fakturace hello existující zákazníků. Ujistěte se, že neměnit hello ceny z SKU hello uvedené v hello oblastech, kde je k dispozici hello SKU. Můžete však přidat nové SKU nebo přidat nové oblasti tooan existující SKU.

## <a name="next-steps"></a>Další kroky
Jakmile hello nabídka přejde za provozu, test toovalidate hello zákazníka scénáře, které všechny smlouvy hello a funkce fungovat správně v provozním prostředí hello jako hello otestované a ověřené v testovacím prostředí.

## <a name="see-also"></a>Viz také
* [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
