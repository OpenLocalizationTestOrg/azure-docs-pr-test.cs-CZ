---
title: "aaaEnable Azure AD Application Proxy portálu classic hello | Microsoft Docs"
description: "Zapněte Proxy aplikace v hello portál Azure classic a nainstalujte hello konektory pro reverzní proxy server hello."
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
ms.date: 07/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 8be9416a61993e1b46a20152e172c5133e54c0a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-application-proxy-in-hello-classic-portal-and-download-connectors"></a>Povolení Proxy aplikace v portálu classic hello a stáhnout konektory
Tento článek vás provede kroky tooenable hello Microsoft Azure AD Application Proxy u cloudového adresáře ve službě Azure AD.

Pokud jste obeznámeni s co Proxy aplikací vám můžou pomoct, další informace o [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Požadavky na proxy aplikace
Než budete moct povolit a používat služby Proxy aplikace, je třeba toohave:

* [Základní nebo prémiové předplatné služby Microsoft Azure AD](active-directory-editions.md) a adresář služby Azure AD, u kterého jste globální správce.
* Serveru se systémem Windows Server 2012 R2 nebo 2016, na který nainstalujete konektor Proxy aplikace hello. Hello server odešle požadavky služby Proxy aplikace toohello v cloudu hello a je nutné protokolu HTTP nebo HTTPS připojení toohello aplikace, které jsou publikování.
  * Pro tooyour přihlašování publikovaným aplikacím, tento počítač by měl být připojené k doméně v doméně hello stejnou AD jako hello aplikace, které jsou publikování. Informace najdete v tématu [jednotné přihlašování pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
* Pokud vaše organizace používá proxy servery tooconnect toohello Internetu, přečtěte si [pracují se stávající místní proxy servery](application-proxy-working-with-proxy-servers.md) podrobnosti o tom tooconfigure je.

## <a name="open-your-ports"></a>Otevřít vaše porty

tooprepare prostředí pro Azure AD Application Proxy, je nutné nejprve tooenable komunikace tooAzure datových centrech. Pokud v cestě hello je brána firewall, ujistěte se, že je otevřené, že hello, které má konektor HTTPS (TCP) požaduje toohello Proxy aplikace.

1. Hello otevřít následující porty příliš**odchozí** provozu:

   | Číslo portu | Jak se používají |
   | --- | --- |
   | 80 | Stahování odvolání certifikátu seznamy (CRL) při ověřování certifikátu SSL hello |
   | 443 | Veškerá odchozí komunikace s hello Proxy aplikace služby |

   Pokud brána firewall vynucuje přenos podle toooriginating uživatelů, otevřete tyto porty pro provoz přicházející ze služeb systému Windows spuštěná jako síťová služba.

   > [!IMPORTANT]
   > Tabulka Hello odráží hello požadavky na porty pro konektor verze 1.5.132.0 a novějších. Pokud máte starší verzi konektoru, musíte taky tooenable hello následující porty: 5671, 8080, 9090, 9091, 9350, 9352 a 10100 – 10120.
   >
   >Informace o aktualizaci vašeho konektory toohello nejnovější verze najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md#automatic-updates).

2. Pokud brána firewall nebo proxy server umožňuje vytvoření seznamu povolených DNS, můžete seznam povolených adres připojení toomsappproxy.net a servicebus.windows.net. Pokud ne, budete potřebovat přístup toohello tooallow [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), které jsou aktualizovány každý týden.

3. Použití hello [Azure AD Application proxy serveru konektoru porty nástroj pro testování](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, aby vaše konektor přístup hello Proxy aplikace služby. Minimálně zkontrolujte, že oblast hello střed USA a tooyou nejbližší oblast hello jsou všechny zelené značky zaškrtnutí. Kromě toho další zelené značky zaškrtnutí znamená větší odolnost proti chybám.

## <a name="enable-application-proxy-in-azure-ad"></a>Povolení Proxy aplikace ve službě Azure AD
1. Přihlaste se jako správce v hello [portál Azure classic](https://manage.windowsazure.com/).
2. Přejděte tooActive adresář a vyberte hello adresář, ve kterém chcete tooenable Proxy aplikace.

    ![Active Directory – ikona](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Vyberte **konfigurace** ze stránky hello adresář a přejděte dolů na příliš**Proxy aplikace**.
4. Přepnutí **povolit služby Proxy aplikace pro tento adresář** příliš**povoleno**.

    ![Povolení proxy aplikace](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Vyberte **Stáhnout**. Hello **Azure AD stažení konektoru Proxy aplikace** otevře. Přečtěte si a přijměte licenční podmínky pro hello a klikněte na tlačítko **Stáhnout** toosave hello Instalační služby systému Windows soubor (.exe) pro konektor hello.

## <a name="install-and-register-hello-connector"></a>Nainstalujte a zaregistrujte hello konektoru
1. Spustit **AADApplicationProxyConnectorInstaller.exe** na hello server připravené podle toohello požadavky.
2. Postupujte podle pokynů Průvodce tooinstall hello hello.
3. Během instalace jsou výzvami tooregister hello konektor s hello Proxy aplikace klienta služby Azure AD.

   * Zadejte vaše přihlašovací údaje globálního správce pro službu Azure AD. Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.
   * Zkontrolujte, zda text hello správce, který registruje konektor hello se hello stejný adresář, kde jste povolili hello Proxy aplikace služby. Například pokud hello klienta domény contoso.com, dobrý den, Správce musí být admin@contoso.com , případně jiný alias v této doméně.
   * Pokud **konfigurace rozšířeného zabezpečení aplikace Internet Explorer** je nastaven příliš**na** na serveru hello může úvodní obrazovka registrace zablokuje. tooallow přístup, postupujte podle pokynů hello v hello chybová zpráva. Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.
   * Pokud registrace konektoru selže, podívejte se do článku [Poradce při potížích s proxy aplikace](active-directory-application-proxy-troubleshoot.md).  
4. Po dokončení instalace hello přidají dvě nové služby tooyour serveru:

   * **Microsoft AAD Application Proxy Connector** umožňuje propojení

     * **Microsoft AAD Application Proxy Connector Updater** je služba pro automatickou aktualizaci. Pravidelně kontroluje nové verze konektoru hello a aktualizuje hello konektoru podle potřeby.

     ![Služby konektoru proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Klikněte na tlačítko **Dokončit** v okně instalace hello.

Informace o konektorech najdete v [popisu konektorů proxy aplikace služby Azure AD](application-proxy-understand-connectors.md).

V zájmu vyšší dostupnosti byste měli nasadit aspoň dva konektory. toodeploy více konektorů, opakujte kroky 2 a 3. Každý konektor musí být zaregistrovaný samostatně.

Pokud chcete toouninstall hello konektor, odinstalujte hello službu konektoru i aktualizační službu hello. Restartujte služby počítače odebrat toofully hello.

## <a name="next-steps"></a>Další kroky
Nyní jste připraveni příliš[publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md).

Pokud máte aplikace, které jsou na samostatné sítě nebo jiné umístění, můžete použít konektor skupiny tooorganize hello různé konektory do logické jednotky. Další informace získáte v článku [Práce s konektory proxy aplikací](active-directory-application-proxy-connectors.md).
