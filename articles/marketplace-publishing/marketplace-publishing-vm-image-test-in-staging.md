---
title: "Nabídka služeb virtuálního počítače pro hello Marketplace aaaTest | Microsoft Docs"
description: "Pochopte, jak tootest virtuálního počítače obrázků pro hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: ab166d2c3c536810a3a8f48330f0482b9b4e58d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-vm-offer-for-hello-azure-marketplace-in-staging"></a>Při přípravě otestovat vaši nabídku virtuálních počítačů pro hello Azure Marketplace
Pracovní znamená nasazení vaší SKU privátního "izolovaného", kde můžete testovat a ověřit jeho funkce ještě před nasazením toohello Marketplace. Hello SKU se zobrazí v pracovním stejně, jako by tooa zákazníkovi, který je nasazena. Bitové kopie virtuálního počítače musí být toostaging certifikovaných toobe poslat.

## <a name="step-1-push-your-offer-toostaging"></a>Krok 1: Push vaší toostaging nabídka
1. Na hello **publikovat** , klikněte na **Push tooStaging**.
   
    ![Kreslení](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Pokud hello publikování portál upozorňuje na všechny chyby, opravte je.
3. V hello **přístup dvoufázové instalace nabídku?** dialogovém okně zadejte hello seznam předplatných Azure, že použijete toopreview vaši nabídku v hello [portál Azure preview](https://portal.azure.com).
   
   > [!NOTE]
   > V případě virtuálních počítačů a šablony řešení prosím **nepodporují** povolených odběrů typu CSP, DreamSpark nebo Azure v otevřené.
   > 
   > 

    > V případě virtuálních počítačů, když kliknete na tlačítko hello **nabízené tooSTAGING**, hello následující kroky jsou prováděny za scény hello. Bude moct tooview hello průběh jednotlivých kroků v části kartu hello publikovat v hello publikování portálu. Je nutné zkontrolovat tuto stránku v pravidelných intervalech (dokud hello stavu zobrazuje PŘIPRAVENÝ) pro všechny informace o selhání, které musí oprava z vaší elementu end.

    > - Na první pracovní žádost přejde toohello certifikační týmu, který ověření hello virtuálního pevného disku. Ale pokud vaše žádost má potom pouze marketingové změnit, pak hello certifikační krok se přeskočí.
    > - Po dokončení hello certifikační replikace hello nabídka start napříč všemi hello datových centrech Azure. Obvykle trvá 24 48hours pro toocomplete hello replikace, ale může trvat až tooa týden v závislosti na velikosti hello hello virtuálního pevného disku. Pokud vaše žádost má potom pouze marketingové změnit, pak hello replikace je však rychlejší.
    > - Po dokončení replikace hello pak hello nabídka bude k dispozici v hello [portál Azure](http:/portal.azure.com). V tento čas hello stav stát dvoufázové instalace v hello publikování portálu. Dvoufázové instalace nabídka je viditelný v hello [portál Azure](http:/portal.azure.com) jenom pomocí ID e-mailu hello přidružené k předplatnému hello s které hello nabídka připravený.

1. Přihlaste se toohello [portál Azure preview](https://portal.azure.com) pomocí jedné z hello předplatná Azure uvedené v předchozím kroku hello.
2. Najít vaši nabídku a ověřit bodům bitové kopie virtuálního počítače:
   
   * Ujistěte se, že marketingové obsah se zobrazí správně v hello Marketplace.
   * Nasazení začátku do konce hello image virtuálního počítače.
     
      ![IMG. Mapa portálu](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Vaši nabídku zůstane v pracovním dokud oznámit Microsoft prostřednictvím hello publikování portálu [**publikovat** kartě > klikněte na tlačítko hello **"Požádat o schválení tooPush tooProduction"**] se připravené toopush tooproduction. Jde ideální čas toohave, všichni členové týmu kontrolu nad vše v rámci přípravy vaši nabídku budete uvedené.
> 
> Hello pracovní platformy je určená pro testování hello nabídka v režimu náhledu hello vydavatelem. Důrazně bránit pomocí této platofrm pro obchodní účely.
> 
> 

## <a name="next-steps"></a>Další kroky
Teď, když vaši nabídku je "připravený" a testování její funkce a marketingu obsah, abyste mohli pokračovat toohello konečné publikování fázi **krok 4**: [nasazení vaší toohello nabídku Marketplace](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Viz také
* [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)

