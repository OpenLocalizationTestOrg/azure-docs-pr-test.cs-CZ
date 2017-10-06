---
title: "aaaTesting služby dat nabízejí pro hello Marketplace | Microsoft Docs"
description: "Pochopte, jak tootest služby dat nabízejí pro hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 1b9c7027d8e0818b9bdee5cfca971bab25dd1959
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Při přípravě testování vaši nabídku Data Service
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Po dokončení hello první dva kroky [vytvoření účtu Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) a [vytváří nabízejí služby Data publikování portálu](marketplace-publishing-data-service-creation.md) jste připraveni zpřístupňuje vaši nabídku v hello Azure Marketplace. Toto téma se nejprve vás provede procesem hello, mezilehlé, krok názvem "přípravy"

Pracovní znamená nasazení vaši nabídku v soukromé "izolovaného", kde můžete otestovat a ověřit jeho funkčnost před odesláním ji tooproduction. Nabídka Hello se zobrazí v pracovním stejně, jako by tooa zákazníkovi, který je nasazena.

## <a name="step-1-pushing-your-offer-toostaging"></a>Krok 1. Když zavedete vaší toostaging nabídka
Vkládání vaší toostaging nabídka umožňuje tootest hello nabídka předtím, než bude k dispozici toofuture odběratele.  Uvidíte, jak vaši nabídku zobrazených a funkce pro ty přihlášení k odběru tooyour data.  

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Přihlášení do hello [publikování portálu](https://publish.windowsazure.com)
2. Vyberte **datové služby** v levém navigačním okně hello
3. Vyberte nabídku chcete toopush toostaging. Zobrazí se hello výše obrazovky.
4. Klikněte na tlačítko **Push tooStaging** tlačítko.  
5. Pokud dochází k potížím s hello nabídka, která potřeby toobe dokončit předchozí toopushing toostaging, zobrazí se seznam zobrazí.  Kliknutím na každou položku v seznamu hello opravte tyto položky. Po kliknutí na všechny opravy provedené, **Push tooStaging** tlačítko znovu.

Pokud zde nejsou žádné problémy s vaši nabídku uvidíte hello automaticky otevíraném okně níže.  

Pokud si nejste plánování nebo nebyla schválena toosurface vaši nabídku na portálu Azure (aktuálně má omezenou kapacitu), pak právě zavřít hello automaticky otevírané okno.

tootest Data služby v portálu Azure (v přidání toohello DataMarket portál), budete potřebovat tootest ID předplatného Azure s.  Toto ID předplatného určují hello účet, který bude povoleno tootest vaši nabídku.  

Vyjmout a vložit svoje ID předplatného a klikněte na značku zaškrtnutí toocontinue hello.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> ID těchto předplatných Azure jsou pouze požadované pro testovacích a přípravných v hello [portálu pro správu Azure](https://manage.windowsazure.com). Nejsou požadované tootest v Azure Marketplace.
> 
> 

Hello na další obrazovce zobrazí ukazuje, že se zobrazením hello "Probíhá" Probíhá publikování ikonu zvýrazněná žlutý níže. Když zavedete toostaging trvat 10 minut too15.  Pokud trvá déle, nejprve aktualizujte webový prohlížeč (stiskněte F5 v aplikaci Internet Explorer).  Ve výjimečných případech hello kde vaši nabídku stále předává toostaging po hodině klikněte na tlačítko hello kontaktujte nás na odkaz toolet nám vědět, že se vyskytl problém.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Po dokončení hello nabízené tooStaging hello "Probíhá" zastaví ikona přesunutí a hello stav bude aktualizován příliš "připravený".  Můžete je nyní připraven tootest vaši nabídku.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Krok 2. Otestovat vaši nabídku dvoufázové instalace v DataMarket
Kliknutím na odkaz hello následující hello text **"v tématu služby nabízejí v..."** toodisplay úvodní obrazovka, která hello odběratele se zobrazí, když vaši nabídku přejde tooproduction a zobrazí se v DataMarket.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testování nebo ověřte všechny položky 12 hello označen výše tooensure všechny loga, ceny nebo transakce, text, obrázky, dokumentace a odkazy jsou správné a funkční správně.  Toto je vhodná doba tooensure, které všechny testovací hodnoty, které jste zadali při vytváření vaši nabídku byly nahrazeny skutečnými hodnotami.

1. Logo nabídka
2. Název nabídky
3. Vydavatel název/propojení tooyour web společnosti
4. Kategorie vyhledávání pro vaši nabídku
5. Vaši nabídku podporu odkaz tooassist odběratele
6. Kontextová popis pro vaši nabídku
7. Nabídka plán znázorňující fakturačních údajů
8. Kód tooimplementation vazby
9. Ukázka bitové kopie, které ilustrují použití dat nabídky
10. Metadata vstupu a výstupu u každé služby v rámci nabídky hello
11. Nabídka pro podmínky použití
12. Náhled hello nabídka dat

Nakonec zkontrolujte, že služba hello bude fungovat prostřednictvím hello Datamarket kliknutím na odkaz hello "Prozkoumat tento datovou sadu".  Otevře se nové okno v nástroji hello říkáme "Service Explorer", můžete zobrazit náhled hello výsledků dotazu na vaši službu.  V tomto okně můžete zadat hello parametry nejsou potřebné a zobrazte výsledky hello zobrazí z dotazu na vaši službu.   Navíc zobrazí je hello URL pro svůj dotaz.  

> [!NOTE]
> Být jisti tooreview hello textový popis hello služby zobrazí v horní části hello.  A pokud vaši nabídku se skládá z více než jedna služba volání, klikněte na karty hello v hello dolní tooswitch toohello další služby tooreview a testování.
> 
> 

## <a name="next-step"></a>Další krok
Pokud potřebujete další pomoc a problémy s jejich řešení obraťte se prosím [Azure technickou podporu vydavatele](http://go.microsoft.com/fwlink/?LinkId=272975).

Pokud budete spokojeni a připravena toopublish vaši nabídku přečtěte hello [schválení žádosti o změnu tooPush tooProduction](marketplace-publishing-push-to-production.md) dokumentaci.

## <a name="see-also"></a>Viz také
* [Začínáme: Jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)

