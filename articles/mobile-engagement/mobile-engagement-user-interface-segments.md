---
title: "aaaAzure Mobile Engagement uživatelské rozhraní – segmenty"
description: "Zjistěte, jak toocreate a správa segmentů uživatelů tooidentify vzorce pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: bb214c45d05ebfbf243978658a7e331d4a7c6e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-segments-of-users-tooidentify-usage-patterns"></a>Jak toocreate a správa segmentů uživatelů tooidentify vzorce
Tento článek popisuje hello **SEGMENTY** kartě hello **Mobile Engagement** portálu. Použít hello **Mobile Engagement** portálu toomonitor a spravovat své mobilní aplikace.

část segmenty Hello hello uživatelského rozhraní můžete toowork na segmentace uživatelů na základě hello různé chování a analýz, které můžete získat z aplikace hello a můžete také přistupovat prostřednictvím hello segmenty rozhraní API. Segmenty se vypočítávají nejprve 24 hodin po jejich vytvoření a jejich jsou přepočítávány každých 24 hodin na základě hello nejnovější analytics informací. Jakmile se počítá segment, zobrazuje graf "Den tooday historie" každý den.

> [!NOTE]
> Mnoho oddílů hello **Mobile Engagement** portál uživatelského rozhraní obsahovat hello **zobrazit NÁPOVĚDU k** tlačítko. Stisknutím tohoto tlačítka tooget další kontextové informace o oddílu.
> 
> 

## <a name="create-segments"></a>Vytvoření segmentů
Můžete vytvořit na základě až too10 kritéria na určitou dobu až too60 dní v hello segment minulosti z oddílu analytics hello. Můžete například vytvořit segment podle hello lidé, kteří mají zobrazit určité stránky nebo vyhledávat konkrétní obsahu v rámci vaší aplikace v rámci hello posledních 10 dnů. Tyto informace jsou k dispozici v části analytics hello. Ano, můžete ho použít toocreate segment a pak nastavit nabízených oznámení tootarget tuto podmnožinu uživatelů tooget je toocome back toohello aplikace. 

> [!NOTE]
> Jakmile je vypočítána segment, nemůže být upraven; může být pouze klonovat (zkopírovaný) nebo zničení (odstraněno). Segment můžete klonovat v rámci hello stejnou aplikaci (s hello stejné AppID), a můžete ho také klonovat do jiných aplikací (s jinou AppID). 

 ![segments1][35] 

## <a name="examples-segments"></a>Příklady segmenty
 ![segments2][36]

Segmenty umožní koncovým uživatelům hello toosegment vaší aplikace.
Segmentace vaši uživatelé je důležité marketingové strategie. Azure Mobile Engagement vám umožní tooget historických dat a vytvořte vlastní segmenty. Tento výkonný nástroj umožňuje toolearn o zákazníkům ve vaší aplikaci. Můžete snadno analyzovat segmenty a použít jako cíle nabízené segmenty.
Je běžné případ použití, které chcete toosend nabízená oznámení tooencourage vaši koncoví uživatelé toorate aplikace v úložišti hello. Místo odesílání oznámení tooall koncovým uživatelům, můžete vytvořit segment, který byste zadat jenom uživatelé, kteří použili aplikaci každý den pro hello poslední měsíc a neměla skvělé uživatelské prostředí. Jakmile odešlete méně, vysoce cílové nabízená oznámení, získáte lepší návratnost investic.

 ![segments3][37]

### <a name="segments-you-can-create-based-on-hello-major-azure-mobile-engagement-elements"></a>Segmenty, které můžete vytvořit podle hello důležité elementy: Azure Mobile Engagement:
* Události: vytvoření segment tohoto cíle jeden konkrétní události hello aplikace, která došlo k více než dvakrát týdně. 
* Relace: vytvoření segment uživatelů, kteří použili aplikaci hello více než 5 výskyty minulého týdne.
* Aktivita: vytvořte segment uživatelů, kteří použili jednu stránku nebo obsah vyšší nebo nižší než 10 čas poslední měsíc.
* Úlohu: vytvořte segment uživatelů, kteří dokončili úlohy maximálně dvakrát denně.
* Havárií: vytvořte segment všech uživatelů hello, které má havárie více než 10 výskyty minulého týdne. (Tento segment omluva nebo i kupónů může push!)
* Chyba: vytvořte segment všechny hello uživatele, kteří mají došlo k chybě víc než 100krát poslední 3 dny.
* Informace o aplikaci: vytvořte segment, který cílí vlastní informace o aplikaci, která nastala v průběhu hello posledních 25 dnů.
  
  ![segments4][38]

toouse Segment s optimálně, musíte provést vlastní integrace hello SDK v aplikaci s označování plánu značek "Informace o aplikaci".
Pak přejděte toohello domovskou stránku hello rozhraní, vyberte hello aplikace a klikněte na v oddílu "Segmenty" hello.

1. Vyberte část "Segmenty" hello.
2. Klikněte na hello "Nový segment" tlačítko toocreate nový segment.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Skutečné životnosti příklad: Vytvoření jednoduché segmentu na základě informací "Relace"
Vytvořte segment všichni hello koncoví uživatelé, kteří použili aplikaci alespoň 50 časy v hello minulého týdne. Z tohoto místa najít pouze hello uživatelů, kteří mají stráví nejméně 30 sekund ve vaší aplikaci na relaci. Zobrazí všechny hello koncoví uživatelé, kteří mají pozitivní zkušenost ve vaší aplikaci. Potom hello segment vytvořili může být použité toopush tooask koncoví uživatelé toothese oznámení je toorate aplikace v rámci hello uložit.

 ![segments5][39]

1. Pojmenujte segment v pořadí toofind rychle v seznamu "Segment" hello.
2. Klikněte na hello tlačítko "Vytvořit".
   
   ![segments6][40]

Vyberte relaci.

 ![segments7][41]

1. Vyberte dobu hello "Poslední týden".
2. Klikněte na tlačítko Další.
   
   ![segments8][42]
3. Hello vyberte operátor, který je relevantní mezi seznamu hello: =; ≥, ≤.
4. Zadejte počet chcete hello.
5. Vyberte hello výskyt chcete. 
6. Klikněte na tlačítko Další.
   V tomto příkladu je nastaven tak, že přes hello poslední týden, shodu uživatele, kteří provedli alespoň 50 relací.
   
   ![segments9][43]

Pro hello relace segmentace můžete hello délka relace jako kritérium.

1. Vyberte ze seznamu hello hello operátor.
2. Zadejte hello délka relace.
3. Klikněte na Další.
   V tomto příkladu se říká, že na všech hello relací, které byly segmentované v oddílu hello výskyt, vyberte pouze hello uživatele, které strávily déle než 30 sekund na relaci.
   
   ![segments10][44]

Název kritériem v pořadí tooretrieve bude v hello dokončit Trychtýřový a klikněte na tlačítko Dokončit.

 ![segments11][45]

Po dokončení nastavení kritériem se zobrazí v segmentu hello trychtýřového grafu.
Protože segment je na základě analýzy dat, se vypočítávají segmenty jednou za den.
V tomto příkladu 47,7 % celkový koncoví uživatelé hello shodná hello kritérium. Měly by být hello uživatelů, kteří mají měl kvalitní a bude se pravděpodobně tooprovide vyšší hodnocení, pokud je nabízená oznámení výzvou toorate hello aplikaci ve hello store.

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

