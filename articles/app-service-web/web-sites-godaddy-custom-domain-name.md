---
title: "Konfigurace vlastního názvu domény v Azure App Service (GoDaddy)"
description: "Naučte se používat název domény z GoDaddy s Azure Web Apps"
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
ms.openlocfilehash: 158c5dc06f83e16633d3c2fbb4eb27d3e8af030c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a>Konfigurace vlastní domény ve službě Azure App Service (zakoupené přímo od GoDaddy)
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

Pokud jste zakoupili prostřednictvím Azure App Service Web Apps domény pak odkazovat na posledním kroku [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md).

Tento článek obsahuje pokyny k používání vlastního názvu domény, který byl zakoupen přímo z [GoDaddy](https://godaddy.com) s [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Principy záznamy DNS
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Přidejte záznam DNS pro vaši vlastní doménu.
Pokud chcete přiřadit vlastní domény webové aplikace ve službě App Service, musí přidáte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných GoDaddy. Použijte následující postup k vyhledání DNS nástroje pro GoDaddy.com

1. Přihlaste se k účtu s GoDaddy.com a vyberte **Můj účet** a potom **Spravovat mé domény**. Vyberte rozevírací nabídku pro název domény, který chcete používat s vaší webové aplikace Azure a vyberte **spravovat DNS**.
   
    ![stránka vlastní doménu pro GoDaddy](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. Z **podrobnosti doméně** stránky, přejděte k položce **souboru zóny DNS** kartě. Toto je část používat pro přidávání a úprava záznamů DNS pro název domény.
   
    ![Karta souboru zóny DNS](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    Vyberte **přidat záznam** přidání existujícího záznamu.
   
    K **upravit** stávajícího záznamu, vyberte pera & papír Ikona vedle záznamu.
   
   > [!NOTE]
   > Před přidáním nové záznamy, Všimněte si, že GoDaddy má již vytvořené záznamy DNS pro oblíbené subdomén (nazývá **hostitele** v editoru), jako **e-mailu**, **soubory**, **e-mailu**a další. Pokud název, který chcete použít, již existuje, upravte existující záznamu místo vytvoření nové.
   > 
   > 
3. Při přidávání záznam, musíte nejdřív vybrat typ záznamu.
   
    ![Vyberte typ záznamu](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    Dále je nutné zadat **hostitele** (vlastní doménu nebo subdomény) a co správci it **odkazuje na**.
   
    ![Přidejte záznam zóny](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * Při přidávání **záznam (hostitel)** -je nutné nastavit **hostitele** pole buď  **@**  (reprezentuje název kořenové domény, jako například **contoso.com** ,) * (zástupný znak pro párování více subdomén) nebo subdomény, které chcete použít (například **www**.) Je nutné nastavit **odkazuje na** pole IP adresu vaší webové aplikace Azure.
   * Při přidávání **záznam CNAME (alias)** -je nutné nastavit **hostitele** do subdomény, které chcete použít. Například **www**. Je nutné nastavit **odkazuje na** do **. azurewebsites.net** název domény vaší webové aplikace Azure. Například **contoso.azurewebsites.net**.
4. Klikněte na tlačítko **přidat další**.
5. Vyberte **TXT** jako typ záznamu, zadejte **hostitele** hodnotu  **@**  a **odkazuje na** hodnotu  **&lt;yourwebappname&gt;. azurewebsites.net**.
   
   > [!NOTE]
   > Tento záznam TXT se používají v Azure k ověření vlastnictví domény popsaného záznam A nebo na první záznam TXT. Jakmile doména byla mapována na webovou aplikaci na portálu Azure, lze odebrat tuto položku záznamu TXT.
   > 
   > 
6. Při přidání nebo úprava záznamů, klikněte na tlačítko **Dokončit** uložte změny.

<a name="enabledomain"></a>

## <a name="enable-the-domain-name-on-your-web-app"></a>Povolit názvů domén ve vaší webové aplikace
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

