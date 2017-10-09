---
title: "aaaAuthenticate s Mobile Engagement REST API – ruční instalaci"
description: "Popisuje, jak nastavit toomanually ověřování rozhraní REST API Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3884f94afcd6b9a62bfcf498fb6ee84bb6e837b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Ověření pomocí Mobile Engagement REST API – ruční instalaci
Tato dokumentace příloha je příliš[ověřit pomocí rozhraní API REST Mobile Engagement](mobile-engagement-api-authentication.md). Zajistěte, aby že čtení první tooget hello kontextu. Popisuje jiný způsob, jak toodo hello jednorázové instalační program pro nastavení ověřování pro použití rozhraní REST API Mobile Engagement hello hello portálu Azure. 

> [!NOTE]
> níže uvedené pokyny Hello jsou založené na tomto [služby Active Directory průvodce](../azure-resource-manager/resource-group-create-service-principal-portal.md) a přizpůsobit pro co je vyžadován pro ověření pro zapojení mobilních rozhraní API. Pokud chcete, aby toounderstand hello následující postup podrobně tak naleznete tooit. 
> 
> 

1. Přihlášení tooyour účet Azure prostřednictvím hello [portálu classic](https://manage.windowsazure.com/).
2. Vyberte **služby Active Directory** v levém podokně hello.
   
     ![Vyberte možnost Active Directory][1]
3. Zvolte hello **výchozí služby Active Directory** na portálu Azure. 
   
     ![Vybrat adresář.][2]
   
   > [!IMPORTANT]
   > Tento postup funguje jenom v případě, že pracujete v hello výchozí služby Active Directory vašeho účtu a nebude fungovat, pokud je to dělají ve službě Active Directory, kterou jste vytvořili ve vašem účtu. 
   > 
   > 
4. aplikace hello tooview ve vašem adresáři, klikněte na **aplikace**.
   
     ![Zobrazit aplikace][3]
5. Klikněte na **přidat**. 
   
     ![Přidání aplikace][4]
6. Klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**
   
     ![Nová aplikace][5]
7. Zadejte název aplikace hello a vyberte hello typu aplikace jako **webové aplikace nebo webové rozhraní API** a klikněte na tlačítko Další hello.
   
     ![název aplikace][6]
8. Můžete zadat všechny fiktivní adresy URL pro **adresa URL přihlašování** a **identifikátor ID URI aplikace**. Nejsou použity pro náš scénář a adresy URL hello sami nejsou ověřené.  
   
     ![Vlastnosti aplikace][7]
9. Na konci hello tohoto budete mít AAD aplikace, které jste zadali dříve jako následující hello názvem hello. Toto je vaše **AD\_aplikace\_název** a poznamenejte si ho.  
   
     ![Název aplikace.][8]
10. Klikněte na název aplikace hello a klikněte na **konfigurace**.
    
      ![Konfigurace aplikace][9]
11. Poznamenejte si hello ID klienta, který se použije jako **klienta\_ID** pro vaše rozhraní API volání. 
    
     ![Konfigurace aplikace][10]
12. Projděte dolů toohello **klíče** a přidání klíče se nejlépe doba trvání 2 roky (vypršení platnosti) a klikněte na **Uložit**. 
    
     ![Konfigurace aplikace][11]
13. Zkopírujte okamžitě hello hodnotu, která je pro klíč hello zobrazen, jak se zobrazují pouze nyní a nejsou uložena, se někdy znovu nezobrazí. Pokud ho ztratíte pak budete mít toogenerate nový klíč. Bude jím hello **tajný klíč CLIENT_SECRET** pro vaše rozhraní API volání. 
    
     ![Konfigurace aplikace][12]
    
    > [!IMPORTANT]
    > Tento klíč vyprší na konci hello hello trvání určeného Ano toorenew se, ujistěte se, které je čas hello vycházejí jinak ověřování rozhraní API už nebude fungovat. Můžete také odstranit a znovu vytvořit tento klíč, pokud se domníváte, že byl napaden.
    > 
    > 
14. Klikněte na **zobrazit koncové body** tlačítko nyní které se otevře hello **koncových bodů aplikace** dialogové okno. 
    
    ![][13]
15. Z dialogového okna koncových bodů aplikace hello, zkopírujte hello **koncový bod TOKENU OAUTH 2.0**. 
    
    ![][14]
16. Tento koncový bod se bude nacházet v hello následující formulář, kde je hello identifikátor GUID v adrese URL hello vaše **TENANT_ID** , poznamenejte si ji: 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. Nyní jsme bude pokračovat tooconfigure hello oprávnění v této aplikaci. To bude mít tooopen až hello [portál Azure](https://portal.azure.com). 
18. Klikněte na **skupiny prostředků** a najde hello **Mobile Engagement** skupinu prostředků.  
    
    ![][15]
19. Klikněte na tlačítko hello **Mobile Engagement** prostředků skupiny a přejděte toohello **nastavení** okno sem. 
    
    ![][16]
20. Klikněte na **uživatelé** v hello okno nastavení a potom klikněte na **přidat** tooadd uživatele. 
    
    ![][17]
21. Klikněte na **vybrat roli**
    
    ![][18]
22. Klikněte na **vlastníka**
    
    ![][19]
23. Vyhledávání pro název vaší aplikace hello **AD\_aplikace\_název** hello vyhledávacího pole. Se nezobrazí ve výchozím nastavení v tomto poli. Po nalezení, vyberte ho a klikněte na **vyberte** v hello dolní části okna hello. 
    
    ![][20]
24. Na hello **přidat přístup** okně se zobrazí jako **1 uživatel, 0 skupiny**. Klikněte na tlačítko **OK** na tuto změnu hello tooconfirm okno. 
    
    ![][21]

Teď jste dokončili hello vyžaduje konfiguraci AAD a jsou všechny hello toocall sadu rozhraní API. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



