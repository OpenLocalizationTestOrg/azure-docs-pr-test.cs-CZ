---
title: "aaaGetting začít s Azure CDN | Microsoft Docs"
description: "Toto téma ukazuje, jak tooenable hello Azure Content Delivery Network (CDN). kurz Hello provede hello vytvoření nového profilu CDN a koncového bodu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0ce9802bfd7b60e70a9a62330f5593fc17ea07d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-cdn"></a>Začínáme s Azure CDN
Toto téma vás provede povolením Azure CDN prostřednictvím vytvoření nového profilu a koncového bodu CDN.

> [!IMPORTANT]
> Úvod toohow CDN funguje, jakož i seznam funkcí, najdete v části hello [přehled CDN](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Vytvoření nového profilu CDN
Profil CDN je kolekcí koncových bodů CDN.  Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.  Můžete toouse více profilů tooorganize koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.

> [!NOTE]
> Ve výchozím nastavení je jedno předplatné Azure je omezený tooeight profilů CDN. Jednotlivé profily CDN jsou omezené tooten koncových bodů CDN.
> 
> Ceny CDN se uplatní na úrovni profilu CDN hello. Pokud chcete toouse směs Azure CDN cenové úrovně, budete potřebovat víc profilů CDN.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Vytvoření nového koncového bodu CDN
**toocreate nový koncový bod CDN**

1. V hello [portálu Azure](https://portal.azure.com), přejděte tooyour profil CDN.  Je může mít připnutý toohello řídicí panel v předchozím kroku hello.  Pokud není, můžete najdete ho kliknutím na **Procházet**, pak **profilů CDN**, a kliknutím na profil hello máte v plánu tooadd koncový bod.
   
    Otevře se okno profil CDN Hello.
   
    ![Profil CDN][cdn-profile-settings]
2. Klikněte na tlačítko hello **přidat koncový bod** tlačítko.
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    Hello **přidání koncového bodu** se zobrazí okno.
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. Zadejte **Název** tohoto koncového bodu CDN.  Tento název bude použité tooaccess prostředkům v mezipaměti v doméně hello `<endpointname>.azureedge.net`.
4. V hello **typ původu** rozevíracího seznamu, vyberte typ původu.  V případě účtu Azure Storage vyberte položku **Storage**, v případě cloudové služby Azure vyberte položku **Cloudová služba**, v případě webové aplikace Azure vyberte položku **Webová aplikace** a v případě jakéhokoli jiného veřejně přístupného původu webového serveru (hostovaného v Azure nebo jinde) vyberte položku **Vlastní původ**.
   
    ![Typ původu CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. V hello **název počátečního hostitele** rozevíracího seznamu, vyberte nebo zadejte doménu původu.  rozevírací Hello se zobrazí seznam všech dostupných zdroje hello typu, který jste zadali v kroku 4.  Pokud jste vybrali *vlastní původ* jako vaše **typ původu**, zadáte doménu vlastního původu hello.
6. V hello **cesty ke zdroji** textové pole, zadejte hello cesta toohello prostředky chcete toocache, nebo nechte prázdné tooallow mezipaměti libovolný prostředek v doméně hello zadané v kroku 5.
7. V hello **hlavičky hostitele počátku**, zadejte hlavičku hostitele hello chcete hello CDN toosend spolu s každou žádostí, nebo ponechte výchozí hello.
   
   > [!WARNING]
   > Některé typy původu, například Azure Storage a Web Apps, vyžadují hello hostitele záhlaví toomatch hello domény hello původu. Pokud nemáte původ, který vyžaduje hlavičku hostitele odlišnou od své domény, nechte hello výchozí hodnotu.
   > 
   > 
8. Pro **protokol** a **port původu**, zadejte hello protokoly a porty používané tooaccess k prostředkům v původu hello.  Je nutné vybrat alespoň jeden protokol (HTTP nebo HTTPS).
   
   > [!NOTE]
   > Hello **port původu** ovlivňuje pouze co koncový bod hello port používá tooretrieve informace z počátku hello.  Hello koncový bod jako takový bude pouze klienti k dispozici tooend na hello výchozí porty HTTP a HTTPS (80 a 443), bez ohledu na to hello **port původu**.  
   > 
   > **Azure CDN společnosti Akamai** koncové body neumožňují hello plný rozsah portů pro zdroje.  Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).  
   > 
   > Přístup k CDN obsah pomocí protokolu HTTPS má hello následující omezení:
   > 
   > * Je nutné použít certifikát SSL hello poskytované hello CDN. Certifikáty třetích stran nejsou podporovány.
   > * Je nutné použít doménu poskytnutou CDN hello (`<endpointname>.azureedge.net`) tooaccess obsahu HTTPS. Podpora protokolu HTTPS není k dispozici pro názvy vlastních domén (záznamů CNAME), protože hello CDN v tuto chvíli nepodporuje vlastní certifikáty.
   > 
   > 
9. Klikněte na tlačítko hello **přidat** tlačítko toocreate hello nový koncový bod.
10. Po vytvoření koncového bodu hello se zobrazí v seznamu koncových bodů pro profil hello. zobrazení seznamu Hello ukazuje, že hello URL toouse tooaccess do mezipaměti obsah, a také doménu původu hello.
    
    ![Koncový bod CDN][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > Hello koncový bod nebude hned dostupný pro použití, jak dlouho trvá dobu hello registrace toopropagate prostřednictvím hello CDN.  V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
    > 
    > Uživatelů, kteří chtějí název domény CDN hello toouse před hello konfigurace koncového bodu má rozšíří toohello bodů POP, obdrží kódy odpovědí HTTP 404.  Pokud jste vytvořili koncový bod před několika hodinami, a přesto obdržíte kód odpovědi 404, prostudujte prosím téma [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md).
    > 
    > 

## <a name="see-also"></a>Viz také
* [Řízení chování ukládání do mezipaměti u žádostí s řetězci dotazu](cdn-query-string.md)
* [Jak tooMap obsahu CDN tooa vlastní domény](cdn-map-content-to-custom-domain.md)
* [Předběžné načtení prostředků v koncovém bodu Azure CDN](cdn-preload-endpoint.md)
* [Vyprázdnění koncového bodu Azure CDN](cdn-purge-endpoint.md)
* [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
