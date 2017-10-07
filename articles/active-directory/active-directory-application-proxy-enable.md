---
title: "Začínáme aaaAzure Proxy aplikace AD - nainstalovat konektor | Microsoft Docs"
description: "Zapněte Proxy aplikace v hello portál Azure a nainstalujte hello konektory pro reverzní proxy server hello."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ea79ffa92fa223584be04f49019fd31a0bfd8358
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-proxy-and-install-hello-connector"></a>Začínáme s Proxy aplikace a nainstalujte konektor hello
Tento článek vás provede kroky tooenable hello Microsoft Azure AD Application Proxy u cloudového adresáře ve službě Azure AD.

Pokud si nejste ještě vědět hello zabezpečení a produktivitu výhody přináší Proxy aplikace tooyour organizace, další informace o [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Požadavky na proxy aplikace
Než budete moct povolit a používat služby Proxy aplikace, je třeba toohave:

* [Základní nebo prémiové předplatné služby Microsoft Azure AD](active-directory-editions.md) a adresář služby Azure AD, u kterého jste globální správce.
* Serveru se systémem Windows Server 2012 R2 nebo 2016, na který nainstalujete konektor Proxy aplikace hello. Hello server musí toobe možné tooconnect toohello služby Proxy aplikace v cloudu hello a hello místní aplikace, které jsou publikování.
  * Pro tooyour přihlašování publikované aplikace pomocí omezené delegování Kerberos, tento počítač by měl být-připojené do domény v doméně hello stejnou AD jako hello aplikace, které jsou publikování. Informace najdete v tématu [použitím KCD pro jednotné přihlašování pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md).

Pokud vaše organizace používá proxy servery tooconnect toohello Internetu, přečtěte si [pracují se stávající místní proxy servery](application-proxy-working-with-proxy-servers.md) podrobnosti o tom, jak se tooconfigure ještě předtím, než dostanou začít s Proxy aplikace.

## <a name="open-your-ports"></a>Otevřít vaše porty

tooprepare prostředí pro Azure AD Application Proxy, je nutné nejprve tooenable komunikace tooAzure datových centrech. Pokud v cestě hello je brána firewall, ujistěte se, že je otevřené, že hello, které má konektor HTTPS (TCP) požaduje toohello Proxy aplikace.

1. Hello otevřít následující porty příliš**odchozí** provozu:

   | Číslo portu | Jak se používají |
   | --- | --- |
   | 80 | Stahování odvolání certifikátu seznamy (CRL) při ověřování certifikátu SSL hello |
   | 443 | Veškerá odchozí komunikace s hello Proxy aplikace služby |

   Pokud brána firewall vynucuje přenos podle toooriginating uživatelů, otevřete tyto porty pro přenos ze služby Windows, které běží jako síťové služby.

   > [!IMPORTANT]
   > Tabulka Hello odráží hello požadavky na porty pro konektor verze 1.5.132.0 a novějších. Pokud máte starší verzi konektoru, musíte taky tooenable hello následující porty v přidání too80 a 443:5671, 8080, 9090-9091 9350, 9352, 10100 – 10120.
   >
   >Informace o aktualizaci vašeho konektory toohello nejnovější verze najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md#automatic-updates).

2. Pokud brána firewall nebo proxy server umožňuje vytvoření seznamu povolených DNS, můžete seznam povolených adres připojení toomsappproxy.net a servicebus.windows.net. Pokud ne, budete potřebovat přístup toohello tooallow [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), které jsou aktualizovány každý týden.

3. Společnost Microsoft používá čtyři certifikáty tooverify adresy. Povolit přístup toohello následující adresy URL, pokud jste tak dosud neučinili pro ostatní produkty:
   * mscrl.microsoft.com:80
   * CRL.microsoft.com:80
   * OCSP.msocsp.com:80
   * www.microsoft.com:80

4. Vaše konektor potřebuje přístup toologin.windows.net a login.microsoftonline.net u registračního procesu hello.

5. Použití hello [Azure AD Application proxy serveru konektoru porty nástroj pro testování](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, aby vaše konektor přístup hello Proxy aplikace služby. Minimálně zkontrolujte, že oblast hello střed USA a tooyou nejbližší oblast hello jsou všechny zelené značky zaškrtnutí. Kromě toho další zelené značky zaškrtnutí znamená větší odolnost proti chybám.

## <a name="install-and-register-a-connector"></a>Instalace a registrace konektoru
1. Přihlaste se jako správce v hello [portál Azure](https://portal.azure.com/).
2. Aktuální adresář se zobrazí pod vaše uživatelské jméno v pravém horním rohu hello. Pokud potřebujete toochange adresáře, vyberte tuto ikonu.
3. Přejděte příliš**Azure Active Directory** > **Proxy aplikace**.

   ![Přejděte tooApplication Proxy](./media/active-directory-application-proxy-enable/app_proxy_navigate.png)

4. Vyberte **stáhnout konektor**.

   ![Stažení konektoru](./media/active-directory-application-proxy-enable/download_connector.png)

5. Spustit **AADApplicationProxyConnectorInstaller.exe** na hello server připravené podle toohello požadavky.
6. Postupujte podle pokynů Průvodce tooinstall hello hello. Během instalace jsou výzvami tooregister hello konektor s hello Proxy aplikace klienta služby Azure AD.

   * Zadejte vaše přihlašovací údaje globálního správce pro službu Azure AD. Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.
   * Zkontrolujte, zda text hello správce, který registruje konektor hello se hello stejný adresář, kde jste povolili hello Proxy aplikace služby. Například pokud hello klienta domény contoso.com, dobrý den, Správce musí být admin@contoso.com , případně jiný alias v této doméně.
   * Pokud **konfigurace rozšířeného zabezpečení aplikace Internet Explorer** je nastaven příliš**na** na hello serveru, kam instalujete konektor hello, nemusí zobrazí úvodní obrazovka registrace. tooget přístup, postupujte podle pokynů hello v hello chybová zpráva. Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.

V zájmu vyšší dostupnosti byste měli nasadit aspoň dva konektory. Každý konektor musí být zaregistrovaný samostatně.

## <a name="test-that-hello-connector-installed-correctly"></a>Testování hello konektor správně nainstalován

Můžete potvrdit, že nový konektor správně nainstalován kontrolou pro něj buď hello portál Azure nebo na serveru. 

V hello portálu Azure, přihlásit tooyour klienta a přejděte příliš**Azure Active Directory** > **Proxy aplikace**. Zobrazí všechny konektory a konektor skupiny na této stránce. Vyberte konektor toosee jeho podrobnosti nebo přesunout do skupiny jiný konektor. 

Na serveru zkontrolujte hello seznam aktivních služeb pro konektor hello a aktualizační konektor hello. Hello dvě služby by měl spustit okamžitě, ale pokud ne, je zapnout: 

   * **Microsoft AAD Application Proxy Connector** umožňuje propojení

   * **Microsoft AAD Application Proxy Connector Updater** je služba pro automatickou aktualizaci. aktualizační metoda Hello kontroluje nové verze konektoru hello a aktualizace hello konektoru podle potřeby.

   ![Služby konektoru proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-enable/app_proxy_services.png)

Informace o konektory a jak zůstanou až toodate najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md).


## <a name="next-steps"></a>Další kroky
Nyní jste připraveni příliš[publikování aplikací pomocí Proxy aplikace](application-proxy-publish-azure-portal.md).

Pokud máte aplikace, které jsou na samostatné sítě nebo jiné umístění, používáte konektor skupiny tooorganize hello různé konektory do logické jednotky. Další informace získáte v článku [Práce s konektory proxy aplikací](active-directory-application-proxy-connectors-azure-portal.md).
