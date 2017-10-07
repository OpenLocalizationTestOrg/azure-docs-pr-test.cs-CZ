---
title: "aaaConfigure vlastní název domény ve službě Azure App Service (GoDaddy)"
description: "Zjistěte, jak název toouse domény z GoDaddy s Azure Web Apps"
services: app-service
documentationcenter: 
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 33233e30-5846-488f-83f3-b32e5c114564
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 48158ee752f9833249bbf85adf80f572d1c68486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a>Konfigurace vlastní domény ve službě Azure App Service (zakoupené přímo od GoDaddy)
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

Pokud jste zakoupili domény prostřednictvím Azure App Service Web Apps naleznete v posledním kroku toohello [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md).

Tento článek obsahuje pokyny k používání vlastního názvu domény, který byl zakoupen přímo z [GoDaddy](https://godaddy.com) s [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Principy záznamy DNS
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Přidejte záznam DNS pro vaši vlastní doménu.
tooassociate vlastní domény s webovou aplikaci ve službě App Service, musí přidáte nový záznam v tabulce hello DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných GoDaddy. Použijte následující postup toolocate hello DNS nástroje pro GoDaddy.com hello

1. Přihlaste se tooyour účet s GoDaddy.com a vyberte **Můj účet** a potom **Spravovat mé domény**. Rozevírací nabídky vyberte hello pro název domény hello chcete toouse s Azure webové aplikace a vyberte **spravovat DNS**.
   
    ![stránka vlastní doménu pro GoDaddy](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. Z hello **podrobnosti doméně** přejděte toohello **souboru zóny DNS** kartě. Toto je část hello používá pro přidávání a úprava záznamů DNS pro název domény.
   
    ![Karta souboru zóny DNS](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    Vyberte **přidat záznam** tooadd existujícího záznamu.
   
    příliš**upravit** existujícím záznamem, vyberte hello pera a papír Ikona vedle záznamu hello.
   
   > [!NOTE]
   > Před přidáním nové záznamy, Všimněte si, že GoDaddy má již vytvořené záznamy DNS pro oblíbené subdomén (nazývá **hostitele** v editoru), jako **e-mailu**, **soubory**, **e-mailu**a další. Pokud název hello chcete toouse již existuje, upravte existující záznamu hello místo vytvoření nové.
   > 
   > 
3. Při přidávání záznam, musíte nejdřív vybrat typ záznamu hello.
   
    ![Vyberte typ záznamu](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    Dále je nutné zadat hello **hostitele** (hello vlastní doménu nebo subdomény) a co správci it **odkazuje na**.
   
    ![Přidejte záznam zóny](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * Při přidávání **záznam (hostitel)** -je nutné nastavit hello **hostitele** pole tooeither  **@**  (reprezentuje název kořenové domény, jako například  **contoso.com**,) * (zástupný znak pro párování více subdomén) nebo subdomény hello chcete toouse (například **www**.) Je nutné nastavit hello **odkazuje na** pole toohello IP adresa vaší webové aplikace Azure.
   * Při přidávání **záznam CNAME (alias)** -je nutné nastavit hello **hostitele** pole toohello subdomény chcete toouse. Například **www**. Je nutné nastavit hello **odkazuje na** pole toohello **. azurewebsites.net** název domény vaší webové aplikace Azure. Například **contoso.azurewebsites.net**.
4. Klikněte na tlačítko **přidat další**.
5. Vyberte **TXT** jako typ záznamu hello, zadejte **hostitele** hodnotu  **@**  a **odkazuje na** hodnotu  **&lt;yourwebappname&gt;. azurewebsites.net**.
   
   > [!NOTE]
   > Tento záznam TXT používá Azure toovalidate, že jste vlastníkem, že hello domény popsaného hello záznam nebo hello první záznam TXT. Jakmile hello domény už namapované toohello webové aplikace ve hello portálu Azure, můžete odebrat tuto položku záznamu TXT.
   > 
   > 
6. Při přidání nebo úprava záznamů, klikněte na tlačítko **Dokončit** toosave změny.

<a name="enabledomain"></a>

## <a name="enable-hello-domain-name-on-your-web-app"></a>Povolit hello názvů domén ve vaší webové aplikace
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

