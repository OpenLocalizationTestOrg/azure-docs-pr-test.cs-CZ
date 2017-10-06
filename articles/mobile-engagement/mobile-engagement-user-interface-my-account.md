---
title: "aaaAzure Mobile Engagement uživatelské rozhraní – Můj účet"
description: "Zjistěte, jak toomanage účet profilu a testovací zařízení pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 22832678-3959-4b8c-9fb2-f2ff5974e5d1
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1d85f0e87c43605f59f6536ae42a7fb6a99ee36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-your-account-profile-and-test-devices"></a>Jak toomanage účet profilu a testovací zařízení
Tento článek popisuje hello **Domů** stránku hello **Mobile Engagement** portálu. Použít hello **Mobile Engagement** portálu toomonitor a spravovat své mobilní aplikace. 

tooget toohello **Můj účet** klikněte na váš účet na hello horní části stránky hello.

Hello části Můj účet hello uživatelského rozhraní je, kde můžete zobrazit a změnit nastavení hello spojené s vaším účtem, včetně nastavení profilu a otestovat ID zařízení. Tato nastavení obsahují položky, které je přístupný také prostřednictvím hello rozhraní API pro zařízení.

![MyAccount1][7]  

## <a name="profile"></a>Profil:
Můžete zobrazit nebo změnit libovolné nastavení svého účtu, který je uvedeno níže. Můžete také udělit jiné toouse oprávnění uživatele aplikace podle jejich e-mailovou adresu z hello [Domů](mobile-engagement-user-interface-home.md).

![MyAccount2][8]  

## <a name="devices"></a>Zařízení:
Můžete zobrazit, přidat nebo odebrat testovací zařízení ID hello testovací zařízení, které můžete použít tootest vaše **dosáhnout** nebo **nabízené** kampaně. Kontextová pokyny, jak toofind hello ID zařízení pro každou platformu zařízení (iOS, Android, Windows Phone, atd.) se zobrazí po kliknutí na tlačítko "Nové zařízení". 

![MyAccount3][9]  

toouse Push rozhraní API nebo Device API budete potřebovat tooknow vaši uživatelé jedinečný identifikátor zařízení (parametr hello deviceid). Existuje několik způsobů tooretrieve ho:

1. Z váš back-end můžete použít funkci "Get" hello aplikace hello rozhraní API pro zařízení tooget hello úplný seznam identifikátorů zařízení.
2. Z vaší aplikace, můžete použít hello SDK tooget ho. (V systému Android se volání funkce getDeviceID() hello hello Agent – třída a v systému iOS, přečtěte si hello deviceid vlastnost hello Agent – třída.)
3. Z oznámení Reach Pokud adresa URL akce hello přidružené hello oznámení obsahuje vzor hello {deviceid} ho se automaticky nahradí identifikátorem hello hello zařízení spouštěcí akci hello.
   http://<example>.com nebo registeruser? deviceid = {deviceid} & otherparam = myparamdata budou nahrazeny: http://<example>.com nebo registeruser? deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata 
4. Z webové oznámení Reach Pokud hello HTML kód hello oznámení obsahuje vzor hello {deviceid}, se automaticky nahradí hello identifikátor zařízení hello se zobrazuje sdělení webovém hello.
   Tady je identifikátor Moje zařízení: {deviceid} budou nahrazeny: tady je identifikátor Moje zařízení: XXXXXXXXXXXXXXXX
5. Otevřete aplikaci na vašem zařízení a provádět událost ve vaší aplikaci, která má kterých byl označen.
   Z "UI - app - monitorování – události – podrobnosti" najde hello událostí, které jste provedli v seznamu hello.
   Klikněte na tlačítko toothis událost v hello monitorování.
   V seznamu hello hello zařízení, které jste udělali Tato událost by měl zjistit ID zařízení.
   Potom můžete zkopírovat toto ID zařízení a zaregistrovat ji hello "UI - Můj účet - zařízení – nové zařízení - vyberte platformu zařízení".
   >(Mějte na paměti, že při IDFA je zakázán pro iOS, hello ID zařízení může změnit časem hello Pokud odinstalovat a znovu nainstalovat aplikaci.)

## <a name="troubleshooting-guide"></a>Průvodce odstraňováním potíží s
* [Průvodce odstraňováním potíží s – služby][Link 24]

## <a name="see-also"></a>Viz také
* [Dokumentace k uživatelského rozhraní – Domovská][Link 13]

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




