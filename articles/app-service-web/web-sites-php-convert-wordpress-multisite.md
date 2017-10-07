---
title: aaaConvert tooMultisite WordPress v Azure App Service
description: "Zjistěte, jak tootake stávající webovou aplikaci WordPress vytvořena prostřednictvím Galerie hello v Azure a převeďte ho tooWordPress nasazení ve více lokalitách"
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
ms.openlocfilehash: 1153f0a8043de875f081704cd0a124776758878c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-wordpress-toomultisite-in-azure-app-service"></a>Převést tooMultisite WordPress v Azure App Service
## <a name="overview"></a>Přehled
*Podle [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*

V tomto kurzu se dozvíte, jak tootake stávající webovou aplikaci WordPress vytvořena prostřednictvím hello Galerie ve službě Azure a převést jej do WordPress ve víc lokalitách nainstalovat. Kromě toho se dozvíte, jak tooassign vlastní domény tooeach Dobrý den podřízené weby v rámci vaší instalace.

Předpokládá se, že máte existující instalaci WordPress. Pokud ho použít nechcete, postupujte podle pokynů hello uvedených v [vytvoření webu WordPress z Galerie hello v Azure][website-from-gallery].

Převod existující WordPress tooMultisite instalace jedné lokality je obecně velmi jednoduché a řadu hello zde počáteční kroky pochází přímo z hello [vytvořit síť A] [ wordpress-codex-create-a-network] stránky na hello [WordPress Codex](http://codex.wordpress.org).

Můžeme začít.

## <a name="allow-multisite"></a>Povolit nasazení ve více lokalitách
Je nutné nejprve tooenable nasazení ve více lokalitách prostřednictvím hello `wp-config.php` soubor s hello **WP\_povolit\_nasazení ve více LOKALITÁCH** konstantní. Existují dvě metody tooedit souborů vaší webové aplikace: hello je nejdřív prostřednictvím FTP a hello druhý prostřednictvím Git. Pokud jste obeznámeni s postupy toosetup žádnou z těchto metod naleznete toohello následující kurzy:

* [Webové stránky PHP s MySQL a FTP][website-w-mysql-and-ftp-ftp-setup]
* [Webové stránky PHP s MySQL a Git][website-w-mysql-and-git-git-setup]

Otevřete hello `wp-config.php` soubor s hello editor dle vašeho výběru a přidejte následující hello výše hello `/* That's all, stop editing! Happy blogging. */` řádku.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Být aby toosave hello soubor a nahrajte ho back toohello serveru!

## <a name="network-setup"></a>Nastavení sítě
Přihlaste se toohello *wp správce* oblast webové aplikace a měla by se zobrazit novou položku pod hello **nástroje** nabídky s názvem **nastavení sítě**. Klikněte na tlačítko **nastavení sítě** a vyplňte hello podrobnosti o síti.

![Obrazovka nastavení sítě][wordpress-network-setup]

Tento kurz používá hello *podadresářích* lokality schématu, protože by měla vždycky fungovat a budeme zadávat vlastní domény pro každý podřízený web později v kurzu hello. Však bude možné toosetup subdoména nainstalovat, pokud namapovat doménu prostřednictvím hello [portálu Azure](https://portal.azure.com) a instalační program zástupný znak DNS správně.

Další informace o subdomény vs podadresáře nastavení najdete v části hello [typy nasazení ve více lokalitách sítě] [ wordpress-codex-types-of-networks] článek na hello WordPress Codex.

## <a name="enable-hello-network"></a>Povolit hello sítě
Hello sítě je nyní nakonfigurována v hello databázi, ale není jeden zbývající krok tooenable hello síťové funkce. Dokončení hello `wp-config.php` nastavení a ujistěte se, `web.config` správně směruje každou lokalitu.

Po kliknutí na hello **nainstalovat** na hello tlačítko *nastavení sítě* stránce WordPress pokusí tooupdate hello `wp-config.php` a `web.config` soubory. Ale byste měli vždy zkontrolovat hello soubory tooensure hello aktualizace byla úspěšná. Pokud ne, tato obrazovka bude představovat hello potřebné aktualizace. Upravit a uložit soubory hello.

Po provedení těchto aktualizací, budete potřebovat toolog se a přihlaste zpět do řídicího panelu webové části správce hello.

Došlo by teď měly být další nabídky na panelu Správce hello s názvem bez přípony **osobní weby**. Tato nabídka vám umožní toocontrol nové síti přes hello **správce sítě** řídicího panelu.

## <a name="adding-custom-domains"></a>Přidání vlastních domén
Hello [WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] modul plug-in je uloženy tooadd vlastní domény tooany lokality ve vaší síti. Pro modul plug-in toooperate hello správně, je třeba splnit toodo některé další nastavení na hello portálu a také u doménového registrátora.

## <a name="enable-domain-mapping-toohello-web-app"></a>Povolit domény mapování toohello webové aplikace
Hello **volné** [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plán režim nepodporuje přidávání tooWeb vlastní domény aplikace. Budete potřebovat tooswitch příliš**sdílené** nebo **standardní** režimu. toodo toto:

* Přihlaste se toohello portálu Azure a vyhledejte vaší webové aplikace. 
* Klikněte na hello **škálovat** kartě v **nastavení**.
* V části **Obecné**, vyberte buď *SDÍLENÉ* nebo *standardní*
* Klikněte na tlačítko **uložit**

Může se zobrazí zpráva s dotazem, tooverify hello změnu a informovanost o tom vaší webové aplikace může nyní zpoplatněná náklady, v závislosti na využití a hello ostatní konfigurace, které nastavíte.

Bude trvat několik sekund tooprocess hello nové nastavení, takže teď je vhodná doba toostart, nastavení vaší domény.

## <a name="verify-your-domain"></a>Ověřte svoji doménu
Před Azure Web Apps vám umožní toomap lokalitu toohello domény, musíte nejprve tooverify, abyste měli hello autorizace toomap hello domény. toodo Ano, musíte přidat nový záznam CNAME tooyour záznamů DNS.

* Přihlášení správce DNS tooyour domény
* Vytvořit nový záznam CNAME *awverify*
* Bod *awverify* příliš*awverify. YOUR_DOMAIN.azurewebsites.NET*

Ho může trvat nějakou dobu hello DNS změny toogo úplné platit, takže pokud hello následující kroky nefungují okamžitě, přejděte zkontrolujte cup kávy, pak se vraťte a zkuste to znovu.

## <a name="add-hello-domain-toohello-web-app"></a>Přidání hello domény toohello webové aplikace
Klikněte na tlačítko návratový tooyour webové aplikace prostřednictvím portálu Azure, hello **nastavení**a potom klikněte na **vlastní domény a SSL**.

Když hello *nastavení SSL* se zobrazí, zobrazí se pole hello, kde budete zadávat všechny hello domén, které chcete tooassign tooyour webové aplikace. Pokud zde není uveden domény, nebude k dispozici pro mapování uvnitř WordPress, bez ohledu na to, jak hello domény DNS je instalační program.

![Dialogové okno vlastní domény spravovat][wordpress-manage-domains]

Po zadání vaší domény do textového pole pro hello, ověří Azure hello záznam CNAME, který jste předtím vytvořili. Pokud hello DNS není plně rozšíří, se zobrazí červený indikátoru. Pokud bylo úspěšné, zobrazí zelené zaškrtnutí. 

Poznamenejte si hello, IP adresa je uvedena v dolní části hello hello dialogového okna. Je nutné tento toosetup hello záznam pro vaši doménu.

## <a name="setup-hello-domain-a-record"></a>Instalační program hello záznam A domény
Pokud hello další kroky byly úspěšné, můžete nyní může přiřadit hello domény tooyour webové aplikace Azure prostřednictvím záznam DNS A. 

Je zde důležité toonote, webové aplikace Azure přijali CNAME a záznamy A, ale můžete *musí* použít mapování A záznamů tooenable správné doméně. Záznam CNAME nemůže být předán tooanother CNAME, který je co Azure vytvořili pro vás s YOUR_DOMAIN.azurewebsites.net.

Pomocí IP adresy hello hello v předchozím kroku, vrátí tooyour Správce DNS a hello instalační program záznamů toopoint toothat IP.

## <a name="install-and-setup-hello-plugin"></a>Instalace a nastavení hello modulu plug-in
Nasazení ve více lokalitách WordPress aktuálně nemá předdefinovanou metodu vlastní domény toomap. Existuje však o modul plug-in názvem [WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] který přidává funkce hello za vás. V protokolu v části správce sítě toohello vašeho webu a nainstalujte hello **WordPress MU domény mapování** modulu plug-in.

Po instalaci a aktivaci hello modul plug-in, navštivte **nastavení** > **domény mapování** tooconfigure hello modulu plug-in. V hello první textovému poli *IP adresa serveru*, vstupní hello adresa IP použitá toosetup hello záznam pro doménu hello. Nastavte libovolné *možnosti domény* požadavky (hello výchozí hodnoty jsou často) a klikněte na tlačítko **Uložit**.

## <a name="map-hello-domain"></a>Mapa hello domény
Navštivte hello **řídicí panel** pro lokalitu hello chcete toomap hello domény. Klikněte na **nástroje** > **domény mapování** a typ hello nové domény do textového pole hello a klikněte na tlačítko **přidat**.

Ve výchozím nastavení bude nové domény hello doméně lokality přepsaná toohello generován automaticky. Pokud chcete toohave všechny přenosy odesílané toohello nové domény, zkontrolujte hello *primární domény pro tento blog* pole před uložením. Můžete přidat libovolný počet domén tooa lokality, ale může být pouze jeden primární.

## <a name="do-it-again"></a>Provést akci
Webové aplikace Azure umožňují tooadd neomezený počet domén tooa webové aplikace. tooadd jiné domény, budete potřebovat tooexecute hello **ověřte svoji doménu** a **nastavení domény A záznam hello** oddíly pro každou doménu.    

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

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


