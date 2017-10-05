---
title: "Převést WordPress na nasazení ve více lokalitách ve službě Azure App Service"
description: "Informace o provedení stávající webovou aplikaci WordPress vytvořena prostřednictvím Galerie v Azure a převést jej do nasazení ve více lokalitách WordPress"
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: fe52dbf4-179c-42f1-adf9-d6a9af920c39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4a15fb5e97d2ca57e5883c07651c372c54021c92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Převést WordPress na nasazení ve více lokalitách ve službě Azure App Service
## <a name="overview"></a>Přehled
*Podle [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*

V tomto kurzu se dozvíte, jak využít stávající webovou aplikaci WordPress vytvořena prostřednictvím Galerie v Azure a převádět je do nasazení ve více lokalitách WordPress instalace. Kromě toho se dozvíte, jak přiřadit vlastní doménu všechny podřízené weby v rámci vaší instalace.

Předpokládá se, že máte existující instalaci WordPress. Pokud ho použít nechcete, postupujte podle pokynů uvedených v [vytvoření webu WordPress z Galerie v Azure][website-from-gallery].

Převádění existující WordPress instalovat jedné lokality do nasazení ve více lokalitách je obecně docela jednoduchá a řadu počáteční kroky pochází přímo z [vytvořit síť A] [ wordpress-codex-create-a-network] stránky na [ WordPress Codex](http://codex.wordpress.org).

Můžeme začít.

## <a name="allow-multisite"></a>Povolit nasazení ve více lokalitách
Nejprve musíte povolit nasazení ve více lokalitách pomocí `wp-config.php` soubor s **WP\_povolit\_nasazení ve více LOKALITÁCH** konstantní. Existují dvě metody k úpravě souborů webové aplikace: první je přes FTP a druhá prostřednictvím Git. Pokud jste obeznámeni s jak nastavit některé z těchto metod, naleznete v následujících kurzech:

* [Webové stránky PHP s MySQL a FTP][website-w-mysql-and-ftp-ftp-setup]
* [Webové stránky PHP s MySQL a Git][website-w-mysql-and-git-git-setup]

Otevřete `wp-config.php` pomocí zvoleného editoru souborů a přidejte následující výše `/* That's all, stop editing! Happy blogging. */` řádku.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Ujistěte se, soubor uložte a odešlete ji zpět k serveru!

## <a name="network-setup"></a>Nastavení sítě
Přihlaste se k *wp-admin* oblast webové aplikace a měla by se zobrazit novou položku pod **nástroje** nabídky s názvem **nastavení sítě**. Klikněte na tlačítko **nastavení sítě** a zadejte podrobnosti vaší sítě.

![Obrazovka nastavení sítě][wordpress-network-setup]

Tento kurz používá *podadresářích* lokality schématu, protože by měla vždycky fungovat a budeme zadávat vlastní domény pro každý podřízený web později v tomto kurzu. Ale musí být možné instalační program subdoména nainstalovat, pokud namapujete domény prostřednictvím [portálu Azure](https://portal.azure.com) a instalační program zástupný znak DNS správně.

Další informace o subdomény vs podadresáře nastavení najdete v článku [typy nasazení ve více lokalitách sítě] [ wordpress-codex-types-of-networks] článek na WordPress Codex.

## <a name="enable-the-network"></a>Povolit síť
Síť je nyní nakonfigurována v databázi, ale je jeden zbývající krok k povolení funkce sítě. Dokončení `wp-config.php` nastavení a ujistěte se, `web.config` správně směruje každou lokalitu.

Po kliknutí na tlačítko **nainstalovat** tlačítko *nastavení sítě* stránce WordPress bude pokus o aktualizaci `wp-config.php` a `web.config` soubory. Ale byste měli vždy zkontrolovat soubory, zajistěte, že aktualizace byla úspěšná. Pokud ne, tato obrazovka bude představovat potřebné aktualizace. Upravit a uložit soubory.

Po provedení těchto aktualizací, které budete muset odhlaste se a přihlaste se zpět do řídicího panelu webové části správce.

Došlo by teď měly být další nabídky na panelu Správce s názvem bez přípony **osobní weby**. Tato nabídka vám umožňuje řídit nové síti přes **správce sítě** řídicího panelu.

## <a name="adding-custom-domains"></a>Přidání vlastních domén
[WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] modulu plug-in umožňuje uloženy přidat vlastní domény k libovolné lokalitě v síti. Aby modul plug-in ke správnému fungování budete muset provést některé další nastavení na portálu a také u doménového registrátora.

## <a name="enable-domain-mapping-to-the-web-app"></a>Povolit mapování domény do webové aplikace
**Volné** [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plán režim nepodporuje přidávání vlastních domén do webové aplikace. Je nutné přepnout na **sdílené** nebo **standardní** režimu. Použijte následující postup:

* Přihlaste se k portálu Azure a vyhledejte vaší webové aplikace. 
* Klikněte na **škálovat** kartě v **nastavení**.
* V části **Obecné**, vyberte buď *SDÍLENÉ* nebo *standardní*
* Klikněte na tlačítko **uložit**

Může se zobrazit zpráva s žádostí o změnu ověřit a potvrdit, že vaše webová aplikace mohla vzniknout teď náklady, v závislosti na využití a další konfigurace, které nastavíte.

Jak dlouho trvá několik sekund zpracovat nové nastavení, takže teď je vhodná doba na spuštění nastavení vaší domény.

## <a name="verify-your-domain"></a>Ověřte svoji doménu
Před Azure Web Apps vám umožní chcete namapovat doménu k lokalitě, musíte nejdřív ověřte, zda jsou autorizaci mapování domény. To pokud chcete udělat, musíte přidat nový záznam CNAME do položky DNS.

* Přihlaste se k vaší doméně Správce DNS
* Vytvořit nový záznam CNAME *awverify*
* Bod *awverify* k *awverify. YOUR_DOMAIN.azurewebsites.NET*

To může trvat nějakou dobu, než se změny DNS přejděte do plný vliv, takže pokud tyto kroky nefungují okamžitě, přejděte zkontrolujte cup kávy, pak se vraťte a zkuste to znovu.

## <a name="add-the-domain-to-the-web-app"></a>Přidání domény do webové aplikace
Vrátit do vaší webové aplikace prostřednictvím portálu Azure, klikněte na tlačítko **nastavení**a potom klikněte na **vlastní domény a SSL**.

Když *nastavení SSL* se zobrazí, zobrazí se pole, které budete zadávat všechny domény, které chcete přiřadit k vaší webové aplikace. Pokud zde není uveden domény, nebude k dispozici pro mapování uvnitř WordPress, bez ohledu na to, jak doména DNS je instalační program.

![Dialogové okno vlastní domény spravovat][wordpress-manage-domains]

Po zadání vaší domény do textového pole, bude Azure ověřit záznam CNAME, který jste předtím vytvořili. Pokud DNS není plně rozšíří, se zobrazí červený indikátoru. Pokud bylo úspěšné, zobrazí zelené zaškrtnutí. 

Poznamenejte si adresu IP uvedených v dolní části dialogového okna. Budete potřebovat k nastavení záznamu A pro doménu.

## <a name="setup-the-domain-a-record"></a>Nastavení domény záznam
Pokud ostatní kroky byly úspěšné, mohou nyní přiřadit domény do Azure webové aplikace prostřednictvím záznam DNS A. 

Je důležité si uvědomit, webové aplikace Azure přijali CNAME a záznamy A, ale Tady můžete *musí* pomocí záznamu A povolit správné doméně mapování. Záznam CNAME nemůže předají do jiné CNAME, který je co Azure vytvořili pro vás s YOUR_DOMAIN.azurewebsites.net.

Pomocí IP adresy z předchozího kroku, vraťte se na Správce DNS a nastavit záznam A tak, aby odkazoval na tuto IP adresu.

## <a name="install-and-setup-the-plugin"></a>Instalace a nastavení modulu plug-in
Nasazení ve více lokalitách WordPress aktuálně nemá předdefinovanou metodu pro mapování vlastních domén. Existuje však o modul plug-in názvem [WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] , přidá funkce pro vás. Přihlaste se na správce sítě část vaší lokality a nainstalujte **WordPress MU domény mapování** modulu plug-in.

Po instalaci a aktivaci modul plug-in, navštivte **nastavení** > **domény mapování** nakonfigurovat modul plug-in. Do textového pole první *IP adresa serveru*, zadejte IP adresu, které jste použili k nastavení záznamu A pro doménu. Nastavte libovolné *možnosti domény* jste desire (výchozí hodnoty jsou často) a klikněte na tlačítko **Uložit**.

## <a name="map-the-domain"></a>Mapa domény
Přejděte **řídicí panel** pro lokalitu, které chcete namapovat doménu k. Klikněte na **nástroje** > **domény mapování** a zadejte nové domény do textového pole a klikněte na tlačítko **přidat**.

Ve výchozím nastavení bude se v nové doméně přepsané téma, které doméně lokality generován automaticky. Pokud chcete, aby veškerý provoz odeslaný do nové domény, zkontrolujte *primární domény pro tento blog* pole před uložením. Neomezený počet domén můžete přidat k lokalitě, ale může být pouze jeden primární.

## <a name="do-it-again"></a>Provést akci
Webové aplikace Azure umožňují přidání neomezený počet domén do webové aplikace. Chcete-li přidat jiné domény, budete muset provést **ověřte svoji doménu** a **nastavení domény záznam** oddíly pro každou doménu.    

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png


