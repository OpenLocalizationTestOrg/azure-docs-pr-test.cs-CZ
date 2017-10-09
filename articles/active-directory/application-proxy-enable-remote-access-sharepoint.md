---
title: "aaaEnable tooSharePoint vzdáleného přístupu s proxy aplikace služby Azure AD | Microsoft Docs"
description: "Popisuje hello základní informace o tom, toointegrate aplikace Azure AD Application Proxy na místní server SharePoint."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6ab413462fcaf387e150449df9c97505c4108bcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-access-toosharepoint-with-azure-ad-application-proxy"></a>Povolit tooSharePoint vzdáleného přístupu s proxy aplikace služby Azure AD

Tento článek popisuje jak toointegrate místní server SharePoint pomocí Proxy aplikace služby Azure Active Directory (Azure AD).

tooenable tooSharePoint vzdáleného přístupu s Azure AD Application Proxy, postupujte podle hello části v tomto článku krok za krokem.

## <a name="prerequisites"></a>Požadavky

Tento článek předpokládá, že už máte SharePoint 2013 nebo novější ve vašem prostředí. Kromě toho zvažte hello následující požadavky:

* SharePoint zahrnuje nativní podpora protokolu Kerberos. Uživatelé, kteří přistupují k interním lokalitám vzdáleně přes Azure AD Application Proxy tedy můžete předpokládat, že toohave jednotné přihlašování (SSO) prostředí.

* Je nutné toomake několik serveru SharePoint tooyour změny konfigurace. Doporučujeme používat pracovní prostředí. Tímto způsobem můžete provést aktualizace tooyour první pracovní server a pak usnadnění cyklus testování před přechodem do produkčního prostředí.

* Předpokládáme, že jste již nastavili SSL pro službu SharePoint, protože jsme požadovat že protokol SSL u hello publikovat adresu URL. Musíte na interní web, že odkazy odeslaných/správně namapovány tooensure toohave, které povolen protokol SSL. Pokud jste nenakonfigurovali SSL, najdete v části [konfigurace protokolu SSL pro službu SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) pokyny. Ujistěte se také, že hello konektor počítač vztahy důvěryhodnosti hello certifikát, který odešlete. (hello certifikát nemusí toobe veřejně vydaných.)

## <a name="step-1-set-up-single-sign-on-toosharepoint"></a>Krok 1: Nastavení tooSharePoint přihlášení

Naše zákazníky má hello nejlepší jednotného přihlašování k prostředí pro své aplikace back-end serveru SharePoint v tomto případě. V tomto běžném scénáři Azure AD hello ověření uživatele jenom jednou, protože uživatelé nebudou vyzváni k zadání ověření znovu.

Pro místní aplikace, které vyžadují nebo používají ověřování systému Windows můžete dosáhnout jednotného přihlašování pomocí ověřovacího protokolu Kerberos hello a funkci omezené delegování protokolu Kerberos (použitím KCD). Použitím KCD, při konfiguraci, umožňuje tooobtain konektoru Proxy aplikace hello windows lístků nebo token pro uživatele, i když hello uživatel není přihlášený tooWindows přímo. toolearn Další informace o použitím KCD, najdete v části [přehled omezené delegování protokolu Kerberos](https://technet.microsoft.com/library/jj553400.aspx).

tooset až použitím KCD pro server SharePoint, použijte postupy hello v hello následující sekvenční části:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>Ujistěte se, že služby SharePoint běží pod účtem služby

Nejprve se ujistěte, že služby SharePoint běží pod účtem služby definované – není místní systém, místní služba nebo síťové služby. K tomu, aby se můžete připojit služby (SPN) pro hlavní názvy tooa platný účet. Hlavní názvy služby SPN se, jak hello protokolu Kerberos identifikuje různé služby. A bude nutné hello účet novější tooconfigure hello použitím KCD.

tooensure, které vaše lokality běží pod účtem služby definované proveďte hello následující kroky:

1. Otevřete hello **Centrální správa služby SharePoint 2013** lokality.
2. Přejděte příliš**zabezpečení** a vyberte **konfigurovat účty služby**.
3. Vyberte **fond webových aplikací – SharePoint - 80**. Možnosti Hello můžou mírně lišit na základě názvu hello fondu váš web nebo pokud hello webové fondu používá protokol SSL ve výchozím nastavení.

  ![Možnosti pro konfiguraci účtu služby](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. Pokud **vyberte účet pro tuto součást** je **místní služba** nebo **síťové služby**, je nutné toocreate účet. Pokud ne, jste hotovi a můžete přesunout toohello další části.
5. Vyberte **zaregistrovat nový spravovaný účet**. Po vytvoření účtu, je nutné nastavit **fond aplikací webové** před použitím hello účtu.

> [!NOTE]
Je třeba toohave a dříve vytvořili účet Azure AD pro službu hello. Doporučujeme povolit změny automatické hesla. Další informace o hello úplnou sadu kroků a řešení problémů najdete v tématu [nakonfigurovat Automatická změna hesla v SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).

### <a name="configure-sharepoint-for-kerberos"></a>Konfigurace služby SharePoint pro protokol Kerberos

Použijte použitím KCD tooperform jeden přihlašování toohello serveru SharePoint, a to funguje pouze s protokolem Kerberos.

tooconfigure vaše SharePoint lokality pro ověřování protokolu Kerberos:

1. Otevřete hello **Centrální správa služby SharePoint 2013** lokality.
2. Přejděte příliš**Správa aplikací**, vyberte **správu webových aplikací**a vyberte web služby SharePoint. V tomto příkladu je **SharePoint - 80**.

  ![Výběr hello webu služby SharePoint](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)

3. Klikněte na tlačítko **zprostředkovatele ověřování** na panelu nástrojů hello.
4. V hello **zprostředkovatele ověřování** pole, klikněte na tlačítko **výchozí pásmo** tooview hello nastavení.
5. V hello **upravit ověřování** dialogové okno pole, posuňte se dolů, dokud neuvidíte **typy ověřování deklarací identity** a ujistěte se, že oba **povolit ověřování systému Windows** a  **Integrované ověřování systému Windows** jsou vybrány.
6. V rozevíracím seznamu hello, ujistěte se, že **Vyjednat (Kerberos)** je vybrána.

  ![Dialog Upravit ověřování](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. Na konci hello hello **upravit ověřování** dialogové okno, klikněte na tlačítko **Uložit**.

### <a name="set-a-service-principal-name-for-hello-sharepoint-service-account"></a>Nastavení hlavního názvu služby pro hello účet služby SharePoint

Než začnete konfigurovat hello použitím KCD, musíte tooidentify hello SharePoint – služba spuštěna jako účet služby hello, který jste nakonfigurovali. To provedete nastavením název SPN. Další informace najdete v tématu [hlavní názvy služby](https://technet.microsoft.com/library/cc961723.aspx).

Formát názvu SPN Hello je:

```
<service class>/<host>:<port>
```

Ve formátu hlavního názvu služby hello:

* _Třída služby_ je jedinečný název pro službu hello. Pro službu SharePoint, můžete použít **HTTP**.

* _hostitele_ je hello plně kvalifikované domény nebo název NetBIOS hello hostitele, který hello service běží na. Pro web služby SharePoint tento text může být nutné toobe hello adresu URL webu hello, v závislosti na hello verze služby IIS, který používáte.

* _port_ je volitelný.

Pokud je hello plně kvalifikovaný název domény serveru SharePoint Server hello:

```
sharepoint.demo.o365identity.us
```

Pak je hello hlavní název služby:

```
HTTP/ sharepoint.demo.o365identity.us demo
```

Můžete také potřebovat tooset SPN pro určité weby na vašem serveru. Další informace najdete v tématu [ověřování protokolem Kerberos konfigurace](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Platit zvýšené pozornosti toohello části "Vytvoření hlavní názvy služby pro vaší webové aplikace s použitím ověřování protokolu Kerberos."

Nejjednodušší způsob Hello pro tooset SPN je toofollow hello SPN formáty, které již mohou být k dispozici pro vaše lokality. Zkopírujte tyto tooregister SPN pro účet služby hello. toodo toto:

1. Procházejte toohello lokality s hello SPN z jiného počítače.
 Když provedete, hello relevantní sadu lístky protokolu Kerberos se uloží do mezipaměti na počítači hello. Tyto lístky obsahují hello SPN hello cílovou lokalitu, kterou jste vyhledali.

2. Může vyžádat hello hlavního názvu služby pro danou lokalitu pomocí nástroje názvem [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html). V příkazovém okně, který běží v hello hello stejné oblasti jako hello uživatele, který používaná hello web v prohlížeči hello, spusťte následující příkaz:
```
Klist
```
Klist pak vrátí hello sadu cíl SPN. Hodnota hello zvýrazněných v tomto příkladu je hello hlavní název služby, který je potřeba:

  ![Příklad Klist výsledky](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)

4. Teď, když máte hello SPN, je nutné toomake se, zda je správně nakonfigurovaná na hello účet služby, které jste nastavili pro webovou aplikaci hello dříve. Spusťte následující příkaz z příkazového řádku hello jako správce domény hello hello:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Tento příkaz nastaví hello hlavního názvu služby pro službu SharePoint hello účet běžící jako _demo\sp_svc_.

 Nahraďte _http/sharepoint.demo.o365identity.us_ s hello SPN pro váš server a _demo\sp_svc_ pomocí účtu služby hello ve vašem prostředí. příkaz Setspn Hello vyhledá hello SPN před přidáním ho. V takovém případě může dojít **duplicitní hodnota SPN** chyby. Pokud se zobrazí tato chyba, ujistěte se, že hodnota hello je přidružen účet služby hello.

Můžete ověřit, že hello přidají SPN spuštěním příkazu Setspn hello s možností -l hello. toolearn Další informace o tomto příkazu najdete v části [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-hello-connector-is-set-as-a-trusted-delegate-toosharepoint"></a>Ujistěte se, že konektor hello je nastaven jako důvěryhodný delegáta tooSharePoint

Hello použitím KCD nakonfigurujte, aby služba Azure AD Application Proxy hello můžete delegovat služba SharePoint toohello identity uživatele. Můžete to udělat povolením konektoru Proxy aplikace hello tooretrieve lístky protokolu Kerberos pro vaše uživatele, kteří byli ověřeni ve službě Azure AD. Potom tento server v takovém případě předá hello kontextu toohello cílová aplikace nebo služby SharePoint.

tooconfigure hello použitím KCD, opakování hello následující kroky pro každý počítač konektor:

1. Přihlaste se jako správce tooa domény řadiče domény a pak otevřete **Active Directory Users and Computers**.
2. Najít hello počítač, který hello konektor běží na. V tomto příkladu ho má stejné hello serveru SharePoint.
3. Dvakrát klikněte na hello počítače a pak klikněte na tlačítko hello **delegování** kartě.
4. Ujistěte se, zda text hello delegování nastavení příliš**důvěřovat tomuto počítači pro delegování toohello zadané služby pouze**a potom vyberte **používající libovolný protokol pro ověřování**.

  ![Nastavení delegování](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)

5. Klikněte na tlačítko hello **přidat** tlačítko, klikněte na tlačítko **uživatele nebo počítače**a vyhledejte účet služby hello.

  ![Přidání hello SPN pro účet služby hello](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

6. V seznamu hello SPN vyberte text hello, ten, který jste dříve vytvořili pro účet služby hello.
7. Klikněte na **OK**. Klikněte na tlačítko **OK** znovu toosave hello změny.

## <a name="step-2-enable-remote-access-toosharepoint"></a>Krok 2: Povolení tooSharePoint vzdáleného přístupu

Teď, když jste povolené služby SharePoint pro protokol Kerberos a nakonfigurované použitím KCD, jste připravené tooset až tooSharePoint přihlášení. Potom z hello konektoru, můžete publikovat hello farmy služby SharePoint pro vzdálený přístup prostřednictvím proxy aplikace služby Azure AD.

tooperform hello následující kroky, je nutné toobe členem hello Role Globální správce v účtu Azure Active Directory vaší organizace.

1. Přihlaste se toohello [portál Azure](https://manage.windowsazure.com) a najít klientovi Azure AD.
2. Klikněte na tlačítko **aplikace**a potom klikněte na **přidat**.
3. Vyberte **Publikování aplikace, která bude přístupná mimo vaši síť**. Pokud nevidíte tuto možnost, ujistěte se, že máte Azure AD Basic nebo Premium nastavit v klientovi hello.
4. Každá z možností hello dokončení následujícím způsobem:
 * **Název**: použijte jakoukoli hodnotu, která chcete – například **SharePoint**.
 * **Interní adresa URL**: Toto je adresa URL hello webu služby SharePoint hello interně jako **https://SharePoint/**. V tomto příkladu, ujistěte se, že toouse **https**.
 * **Metoda předběžného ověření**: vyberte **Azure Active Directory**.

  ![Možnosti pro přidání aplikace](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. Po publikování aplikace hello, klikněte na tlačítko hello **konfigurace** kartě.
6. Projděte dolů možnost toohello **překládat adresy URL v hlavičkách**. Hello výchozí hodnota je **Ano**. Změnit příliš**ne**.

 SharePoint používá hello _Hlavička hostitele_ hodnotu toolook až hello lokality. Generuje také odkazy na základě této hodnoty. čistý efekt Hello je toomake se, že všechny odkaz, který generuje SharePoint je publikovanou adresu URL, která je správně nastavena toouse hello externí adresu URL. Nastavení hodnoty hello příliš**Ano** také umožňuje hello konektor tooforward hello požadavek toohello back-end aplikace. Však příliš nastavení hodnoty hello**ne** znamená, že hello konektor nepošle název hello interní hostitele. Místo toho hello konektor odesílá hlavičku hostitele hello jako hello publikovaný URL toohello back-end aplikace.

 Tooensure také, že přijímá tuto adresu URL služby SharePoint, musíte toocomplete jeden další konfigurace na serveru SharePoint hello. Můžete to udělat v další části hello.

7. Změna **interní metodu ověřování** příliš**integrované ověřování systému Windows**. Pokud klientovi Azure AD používá název UPN hello cloudu, který se liší od hello UPN v místě, mějte na paměti, tooupdate **delegované Identity přihlášení** také.
8. Nastavit **interní aplikace SPN** toohello hodnotu, která jste nastavili dříve. Například použít **http/sharepoint.demo.o365identity.us**.
9. Přiřaďte tooyour aplikace hello cíloví uživatelé.

Aplikace by měl vypadat podobně jako toohello následující ukázka:

  ![Dokončení aplikace](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

## <a name="step-3-ensure-that-sharepoint-knows-about-hello-external-url"></a>Krok 3: Zajistěte, aby SharePoint ví o externí adresu URL hello

Vaše poslední krok tooensure, SharePoint, můžete najít hello lokality podle hello externí adresu URL, tak, aby se budou vykreslovat odkazy podle této externí adresu URL. To uděláte tak, že nakonfigurujete mapování alternativních adres URL pro web služby SharePoint hello.

1. Otevřete hello **Centrální správa služby SharePoint 2013** lokality.
2. V části **nastavení systému**, vyberte **konfigurace mapování alternativních adres**.

 Tím se otevře hello **mapování alternativních adres** pole.

  ![Alternativní pole mapování adres URL](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. V rozevíracím seznamu hello vedle položky **kolekce mapování alternativních adres**, vyberte **změnit alternativní přístup mapování kolekci**.
4. Vyberte svou lokalitu – například **SharePoint - 80**.

  ![Výběr lokality](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. Je možné, že tooadd hello publikovat adresu URL jako interní adresa URL nebo veřejnou adresu URL. Tento příklad používá veřejnou adresu URL jako hello extranetu.
6. Klikněte na tlačítko **Upravit veřejné adresy URL** v hello **Extranet** cestu a pak zadejte cestu hello hello publikovaných aplikací, jako v předchozím kroku hello. Zadejte například **https://sharepoint-iddemo.msappproxy.net**.

  ![Zadání cesty hello](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. Klikněte na **Uložit**.

 Nyní můžete k webu služby SharePoint hello externě prostřednictvím proxy aplikace služby Azure AD.

## <a name="next-steps"></a>Další kroky

- [Jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md)
- [Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
- [Publikování serveru SharePoint 2016 a Office Online serveru s proxy aplikace služby Azure AD](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/)
