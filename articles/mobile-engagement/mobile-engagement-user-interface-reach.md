---
title: "aaaAzure Mobile Engagement uživatelské rozhraní - Reach"
description: "Zjistěte, jak tooreach toohello uživatelů vaší aplikace pomocí nabízených oznámení pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 40d5162ddeccec82c2c9f5b0d72b4cb10c9ddc38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreach-out-toohello-users-of-your-application-with-push-notifications"></a>Jak tooreach toohello uživatelů vaší aplikace pomocí nabízených oznámení
Tento článek popisuje hello **dosáhnout** kartě hello **Mobile Engagement** portálu. Použít hello **Mobile Engagement** portálu toomonitor a spravovat své mobilní aplikace. Všimněte si, že toostart hello portálu musíte nejprve toocreate **Azure Mobile Engagement** účtu. Další informace najdete v tématu [vytvoření účtu Azure Mobile Engagement](mobile-engagement-create.md).

Hello připojit části hello uživatelského rozhraní je hello nabízené kampaně nástroj pro správu kde můžete vytvořit nebo upravit nebo aktivovat nebo dokončit nebo monitorování a získat statistiky kampaní nabízených oznámení a funkce, které je přístupný také prostřednictvím hello Reach API (a některé prvky hello nízkou úroveň Push rozhraní API). Mějte na paměti, že ať používáte hello rozhraní API nebo hello uživatelského rozhraní, budete potřebovat toointegrate Azure Mobile Engagement a Reach do své aplikace pro každou platformu s hello SDK, abyste mohli používat kampaně Reach.

> [!NOTE]
> Mnoho oddílů hello **Mobile Engagement** portál uživatelského rozhraní obsahovat hello **zobrazit NÁPOVĚDU k** tlačítko. Stisknutím tohoto tlačítka tooget další kontextové informace o oddílu.
> 
> 

## <a name="four-types-of-push-notifications"></a>Čtyři typy nabízená oznámení
1. Oznámení – umožňují toousers toosend reklamní zprávy, který přesměruje je umístění tooanother uvnitř vaší aplikace nebo toosend je webová stránka tooa nebo uložit mimo vaší aplikace. 
2. Hlasování - povolit informace toogather koncovým uživatelům ve formě odpovědí na otázky.
3. Datová oznámení - povolit toosend base64 nebo binární datový soubor. se Hello informace obsažené v datovém oznámení odeslaných tooyour aplikace toomodify aktuální zkušeností uživatelů ve vaší aplikaci. Aplikace musí toobe možné tooprocess hello dat v datových oznámeních.

## <a name="campaign-details"></a>Podrobnosti kampaně
Můžete upravit, klonování, odstranit nebo aktivovat kampaní, které nebyly ještě aktivována podržením ukazatele nad jejich názvy, nebo můžete kliknout na tooopen je. Může klonovat kampaní, které již byla aktivována podržením ukazatele nad jejich názvy nebo můžete kliknout na tooopen je. Jakmile byl aktivován však nelze změnit na kampaň.

![Reach1][18]

## <a name="reach-feedback"></a>Dosažení zpětné vazby
Klikněte na **statistiky** toosee hello podrobnosti kampaně Reach. Hello **jednoduché** zobrazení nabízí vizuální znázornění hello tvar pruhový graf sloupec o co se stalo po kampaň aktivovala. Hello **Upřesnit** zobrazení obsahuje podrobnější informace o hello nabízené kampaně. Tyto údaje nebudou dostupné při odesílání zkušební kampaně tj nabízené odeslané tooa testovací zařízení. Zde je, jak by měl interpretovat tyto podrobnosti:

1. **Poslat** -určuje hello počet zpráv nabídnutých toohello zařízení. Tento počet bude záviset na hello cílovou skupinu, kterou jste zadali při vytváření kampaně nabízených hello. Pokud nezadáte žádné cílové skupiny, pak tato nabízená bude odesílat tooall hello zaregistrované zařízení. Podobně jako všechny ostatní nabízených služeb, jsme není nabízená oznámení hello přímo toohello zařízení, ale místo toho je push příslušné platformy toohello konkrétní služeb nabízených oznámení (PNS - APNS nebo GCM/WNS) tak, aby se doručení oznámení hello toohello zařízení. 
2. **Doručit** -určuje hello počet zpráv, které jsou úspěšně doručil hello systém PNS toohello zařízení a potvrzené jako přijaté Mobile Engagement SDK. 
   
   *Důvody pro doručené počet je menší než počet stisknutí:*
   
   1. Pokud uživatel hello odinstaloval hello aplikace ze zařízení hello ale hello systém PNS neví o něm v době hello pošleme nabízené toohello hello systém PNS bude zrušeno uvítací zprávu.
   2. Pokud zařízení hello má aplikace hello ale hello zařízeními jsou v režimu offline pro dlouhou dobu, pak hello systém PNS selže toodeliver hello zpráva toohello zařízení. 
   3. Pokud hello zprávu získat doručit toohello zařízení, ale hello Mobile Engagement SDK v aplikaci hello nerozpoznal hello obsah zprávy hello, pak zahodí zprávy. Toto může nastat, pokud hello přizpůsobení hello oznámení v aplikaci hello vygeneruje výjimku, která jsme catch hello SDK a drop uvítací zprávu. Situace může také nastat, pokud aplikace hello na hello zařízení používáte verzi hello Mobile Engagement SDK, která není možné toounderstand hello novější verzi hello nabízená zpráva odeslaný hello platformy, ale toto je pouze v případě, že aplikace hello byl upgradován po hello oznámení byl odeslán z platformy service hello. Hello **Upřesnit** kartě zjistit, kolik zpráv byly vyřazeny. 
   4. Na zařízeních s iOS někdy není získat doručeny zprávy je buď hello zařízení na nízký stav baterie. nebo pokud aplikace hello spotřebovává významné množství power při zpracování vzdáleného oznámení. Jedná se o omezení hello zařízení iOS.   
3. **Zobrazí** -určuje hello počet zpráv, které jsou úspěšně zobrazený toohello aplikace uživatele na zařízení hello hello tvar systémové oznámení nabízených nebo out z – aplikaci v centru oznámení hello nebo oznámení v aplikaci v rámci hello mobilní aplikace.  Hello **Upřesnit** na kartě se zjistit, kolik byly systémová oznámení a kolik byly oznámení v aplikaci. 
   
   *Důvody pro zobrazené počet je menší než počet doručené (čekání toobe zobrazí)*
   
   1. Pokud kampaň oznámení hello měl koncové datum v jej, pak je možné, že byla doručena hello oznámení, ale když přišel čas hello tooopen a zobrazit ji toohello aplikace uživatele, byl již vypršela, a proto se nikdy zobrazit.   
   2. Pokud hello oznámení je oznámení v aplikaci pak hello oznámení se zobrazí pouze když uživatel aplikace hello otevře aplikace hello. V případech, kdy uživatel aplikace hello neotevřel aplikace hello hello SDK oznámí, že byla oznámení hello doručit ale ještě nebyla nezobrazí, dokud otevření aplikace hello. 
   3. Pokud hello oznámení je oznámení v aplikaci a nakonfigurovat toobe zobrazen na konkrétní aktivity nebo obrazovce pak také hello oznámení se ohlásí jako doručen, ale ještě doručit, dokud není hello uživatel otevře aplikaci hello na konkrétní obrazovce. 
4. **Interakce uživatele** -určuje hello počet zpráv, které uživatel aplikace hello má zpracoval s a bude obsahovat hello zprávy, které jsou zpracované nebo ukončené. 
   
   * *uživatel aplikace Hello může akce oznámení v některém z následujících způsobů hello:*
     
     1. Pokud hello oznámení systému nebo na více systémů z – aplikace se oznámení nebo oznámení v aplikaci odesílá jen oznámení potom hello aplikace uživatel klikne na hello oznámení.
     2. Pokud hello oznámení je oznámení v aplikaci s textem nebo webové zobrazení nebo hlasování pak hello aplikace uživatel klikne na hello tlačítko akce v hello oznámení.
     3. Pokud se oznámení hello oznámení v aplikaci s webové zobrazení pak hello aplikace uživatel klikne na na adrese URL hello webové pouze v zobrazení [Android]
   * *uživatel aplikace Hello můžete ukončit oznámení v některém z následujících způsobů hello:*
     
     1. Kliknutím na tlačítko Zavřít hello na hello oznámení přímo. 
     2. K načtení rychle nebo odstraněním hello oznámení. 
     3. Oznámení v aplikaci s text nebo webového obsahu a hlasování jsou obvykle zobrazených toohello aplikace uživatele ve dvou krocích. Nejprve zobrazí oznámení a po kliknutí na něm, zobrazí se jim hello následné obsahu text/web/dotazování. uživatel aplikace Hello můžete ukončit oznámení v některém z těchto kroků a hello podrobností v rozšířeném zobrazení hello zachytí to. 
5. **Reakcemi** -určuje hello počet zpráv, které byly explicitně reakcemi uživatelem aplikace hello. Toto je nejvíce zajímavé číslo hello jako to informuje o tom, kolik uživatelů aplikace byly zajímají o uvítací zprávu, kterou instalováni v hello oznámení. 

> [!NOTE]
> Na iOS & platformy systému Windows, pokud má uživatel hello hello aplikaci otevřete a hello kampaň byla kampaň "Kdykoliv" je možné, že oba mimo oznámení v aplikaci a aplikace se zobrazují v hello stejnou dobu. To může způsobit vyšší než hello doručené zobrazené počtu. Pokud uživatel hello pracuje nebo akce hello oznámení, pak i hello uživatelské interakce/Actioned počet může být větší než doručené. 
> 
> 

![Reach2][19]

## <a name="see-also"></a>Viz také
* [Koncepty][Link 6]
* [Řešení potíží s příručce služby][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

