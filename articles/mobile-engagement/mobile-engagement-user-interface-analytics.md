---
title: "aaaAzure Mobile Engagement uživatelské rozhraní - Analytics"
description: "Zjistěte, jak tooanalyze historická data o vaší aplikaci pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6b2533ac-b8ec-4e35-872c-d563895bdc0c
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4a9df11226fed6710cfb1337ae84ece7596d482f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooanalyze-historical-data-about-your-application"></a>Jak tooanalyze historická data o vaší aplikace
Tento článek popisuje hello **ANALYTICS** kartě hello **Mobile Engagement** portálu. Použít hello **Mobile Engagement** portálu toomonitor a spravovat své mobilní aplikace. Všimněte si, že toostart hello portálu musíte nejprve toocreate **Azure Mobile Engagement** účtu.

Hello Analytics části hello uživatelského rozhraní obsahuje souhrnné informace o aplikaci na základě historických dat, který se aktualizuje každých 24 hodin. Hello informace se zobrazí na jiné řídicí panely skládá z řádku, řádku/výsečové grafy, mřížky a mapy. Hello dat se dá stáhnout i jako soubory .csv. Většina stejné informace je k dispozici v reálném čase v hello monitorování části hello uživatelského rozhraní a můžete také přístupná z hello Analytics rozhraní API.

> [!NOTE]
> Mnoho oddílů hello **Mobile Engagement** portál uživatelského rozhraní obsahovat hello **zobrazit NÁPOVĚDU k** tlačítko. Stisknutím tohoto tlačítka tooget další kontextové informace o oddílu.

## <a name="standard-and-custom-analytics"></a>Standardní a vlastní Analytics
Azure Mobile Engagement poskytuje sadu basic, standard analytické informace o aplikacích, které může být proto vytvořen při integraci aplikace s hello SDK. Azure Mobile Engagement poskytuje také hello možnost toogather další analýza vlastní informace, které chcete o chování vlastní koncoví uživatelé. To provedete tak, že vytvoříte plán značek vlastní "značky (app-info)", vytvořené z **nastavení** tak, aby Azure Mobile Engagement může shromažďovat tato další data za vás.

## <a name="analytics"></a>Analýza
* Řídicí panel: Zobrazuje obecné informace o nových a aktivující uživatelům a jejich vývoj.
* Uživatelé: Uživatelé se identifikují podle identifikátoru zařízení: Tento identifikátor je jedinečný pro každé zařízení (jeden nový uživatel je ve skutečnosti jedno nové zařízení). Uživatel se považuje za nové v daném časovém intervalu Pokud uskutečnil během tohoto intervalu uskutečnil první relaci. Uživatel se považuje za udrženého, pokud uskutečnil alespoň jednu relaci během hello posledních 7 dnů. Aktivní uživatelé jsou uživatelé, kteří provedené alespoň jednu relaci během daného období. Můžete řadit, měsíčně, týdně, denní nebo každou hodinu časových období. Všechny grafy hello vypadat podobně jako ale umožňují toofilter různé funkce, jako je například hello verzi vaší aplikace a pak toosort podle v časovém intervalu. Hello standardní informace získané integrací hello SDK zahrnuje následující hello: aktivních uživatelů, nového uživatele, počet relací, délka každé relaci, technické informace o hello země, místní hodnoty, umístění, poskytovatel jazyk, zařízení, firmwaru, síť (Wi-Fi), verze aplikace hello a sady SDK, používaných zákazníky. Tuto informaci lze zobrazit v reálném čase z části monitorování hello.

> [!NOTE]
> Hello časové období je založena na datum hello z hello uživatelských nastavení zařízení, takže uživatel, jehož telefon je správně nastaveno datum hello může zobrazí v hello nesprávný časové období.

* Uchování: Uživatel se považuje za udrženého, pokud během tohoto intervalu uskutečnil první relaci uskutečnil v daném časovém intervalu. Můžete změnit hello časové intervaly, za které udržených uživatelů (a noví) uživatelé počítají toohours, dny, týdny nebo měsíce. uchování analýzy chování uživatelů Hello je postavená na kohorty. Kohorty je sada hello všichni noví uživatelé hello zjistil za dané období (tj, hello sadu uživatelů provedením jejich první relaci během této doby). Používáme kohorty 1 den, den 2, 4 dní, 7 dní nebo 1 měsíc. Zadaný kohorty, každý 1 den, den 2, 4 dní, 7 dní nebo 1 měsíce, Azure Mobile Engagement výpočtů hello sadu všichni uživatelé, kteří patří toohello kohorty a jsou stále aktivní (tedy hello sadu uživatelů, kteří provést na alespoň jednu relaci během období hello). Tuto sadu uživatelů, se nazývá kohorty verze. (Azure Mobile Engagement můžete zjistit, kolik uživatelů jsou stále používají vaši aplikaci, ale pouze konkrétní úložiště platformy hello poznáte, kolik uživatelů odinstalovat aplikace – například GooglePlay iTunes, Windows Store, atd.).
* Relace: Jedno použití aplikace hello uživatelem. Relace se generují z hello posloupnost aktivit prováděné uživateli (aktivita je obvykle spojovány toohello využití jeden obrazovky aplikace hello, ale může se to lišit v závislosti na hello způsob hello SDK integrovaná v aplikaci hello). Uživatel může provádět pouze jednu aktivitu najednou: relace začíná, jakmile hello uživatel zahájí první aktivitu a zastaví, jakmile skončí poslední aktivita. Je-li uživatel více než několik sekund bez provádění všech činností, jeho pořadí aktivit rozdělit na dvě odlišné relace.
* Aktivity: hello názvy jednotlivých obrazovky v aplikaci a hello délka uživatelé vynaložit na každý obrazovky. Aktivity jsou vlastní analytické možnost, která bude odpovídat toohello "informace o aplikaci" značky, které jste nastavili pro vlastní aplikace:
* Cesta k uživatele: Ukazuje, jak uživatelé přecházejí mezi aktivitami (obrazovkami) aplikace. Přesunutím posuvníku hello tooadjust hello úroveň podrobností. Modré uzly představují aktivity aplikace. Jejich velikost je úměrná toohello uživatelé stráví ve frontě. Bílé uzly představují zahájení a ukončení relace. Červené uzly představují havárie. Spojnice představují přechody mezi aktivitami aplikace (nebo mezi aktivitami a haváriemi). Klikněte na uzel nebo odkaz toodisplay popisek s dalšími informacemi o vašich dat: hello časem stráveným na konkrétní obrazovce, hello počtem přechodů a procentem přechodů ze hello zdroj aktivity toohello cílová aktivita hello. (---60 %---> B znamená, že uživatelé se u aktivity vloží tooactivity B 60 % doby hello.) Jak chcete tooclarify, můžete přeuspořádat hello grafu. jeho pozice se uloží pokaždé, když provedete změny. Můžete zobrazit nebo skrýt hello havárií toolighten hello grafu.
* Události: Konkrétní akce provedené uživatelem v aplikaci hello. distribuce Hello událostí se zobrazí jako hello počet událostí na uživatele na relaci. Událost představuje okamžitou akci, například kliknutí na tlačítko nebo hello příjem oznámení. (hello význam událostí závisí na tom, jak má hello SDK Integrovaná aplikace hello.) Událost může dojít během relace nebo úlohy, případně může být samostatná.
* Úlohy: Podobné tooevents kromě se zaměřit na hello délka hello akce. Například úlohy může zjistit technické informace o tom, jak dlouho trvalo obsahu tooload nebo službu tooweb volání. Ho může také zobrazit, jak dlouho trvalo toofill uživatele na formuláři, vytvoření účtu nebo nákupu. Úlohu představuje hello dobu trvání nějakého úkolu, pro příklad, hello dobu stahování nebo hello čas banner se zobrazí na úvodní obrazovka. (hello význam úloh závisí na tom, jak má hello SDK Integrovaná aplikace hello.) Úlohy jsou obvykle spojovány s úkoly na pozadí, které se provádějí mimo rozsah relace (tedy bez zásahu uživatele) hello.
* Technicals: Technické informace o zařízení hello hello uživatele vaší aplikace, které můžete sledovat, jako je například hello národního prostředí, operátora, sítě, zařízení, firmwaru a obrazovky velikost zařízení hello uživatelů a hello verzi aplikace a hello verze sady SDK používaná ve vaší aplikaci.
* Chyby: Informace o technických chyby uvnitř hello aplikace, které nezpůsobí toocrash aplikace hello. Chybu představuje náhlý problém, například chybu sítě nebo chybnou manipulaci. (hello význam událostí závisí na tom, jak má hello SDK Integrovaná aplikace hello.) K chybě může dojít během relace nebo úlohy, případně může být samostatná.
* Dojde k chybě: Informace o chybách, které způsobí toocrash vaší aplikace. Havárie je vznikl nečekaně stav, kdy hello aplikace přestane vykonávat své předpokládané funkce a musí být zastaven. Havárie je většinou kvůli tooa chyb v aplikaci hello.

![Analytics2][11]

## <a name="accessing-hello-retention-overview"></a>Přístup k hello uchování – přehled
![Analytics3][12]

Přehled uchování Hello je rozdělena střední hello do několik karet, každý zobrazuje přehled hello po určitou dobu uchování. Doba uchování 2 dny Hello je vidět v příkladu hello. Hello ostatní karty zobrazit hello 4 dny a období uchovávání 7 dní.

## <a name="understanding-hello-retention-overview-cards"></a>Principy hello uchování Přehled karty
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Každou kartu se skládá ze 3 hlavní části:
1. 1: hello kohorty a hello období považovány za
2. 2 – 4: hello aktuální období uchovávání dat pro hello
3. 5: minigraf hello historie

### <a name="here-is-detailed-information-about-each-element"></a>Tady je podrobné informace o jednotlivých prvků:
1. Kohorty a období: Tato titulku dává hello typ kohorty. Zde "2 dny období" znamená, že se podíváme na hello chování uživatelů více než 2 dny, uživatelé, které byly přijaty během období 2 dny a jestli se připojení v hello následující bloky dvou dnů. výše uvedený příklad Hello zvažuje hello aktivity uživatelů mezi hello 21 a 22 listopadu.
2. To poskytuje míru uchování hello nad hello 21 a 22 listopadu pro uživatele hello přicházejících 19 a 20 listopadu. Zde jsme měli 1 aktivního uživatele mezi hello 21 a 22nd, přes hello 3, které byly nové uživatele mezi hello 19th a 20.
3. Tato hello poskytuje vizuální indikátor graficky znázornit stejné informace jako výše. (třetí hello hello kroužek je z čísla 33 % hello.) Barva Hello jsou uvedeny další informace: zelená značí toto číslo se ročně zvýší z předchozí výpočtu hello. Žlutý znamená stabilní a red znamená snížení.
4. To znamená hello hodnoty pro výpočet hello.
5. Toto je minigraf hello historie hello uchování hodnot. Umožňuje toosee hello hodnoty v hello po toohave široký zobrazení jak vyvinuly.

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
