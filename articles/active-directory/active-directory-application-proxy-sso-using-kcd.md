---
title: "přihlašování aaaSingle pomocí Proxy aplikací | Microsoft Docs"
description: "Popisuje, jak tooprovide jednotného přihlašování pomocí proxy aplikace služby Azure AD."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: ded0d9c9-45f6-47d7-bd0f-3f7fd99ab621
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 0047e834cd42e057a75ebc0c5dcf860734464a05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-tooyour-apps-with-application-proxy"></a>Omezené delegování Kerberos tooyour přihlašování aplikací pomocí Proxy aplikace

Můžete zadat jednotné přihlašování pro místní aplikace publikované prostřednictvím Proxy aplikace, které jsou zabezpečené integrované ověřování systému Windows. Tyto aplikace vyžadují lístek protokolu Kerberos pro přístup. Proxy aplikací používá protokol Kerberos vynuceným delegování použitím (KCD) toosupport tyto aplikace. 

Můžete povolit tooyour přihlášení aplikace, které používají integrované ověřování systému Windows (IWA) tím, že oprávnění konektory Proxy aplikace v tooimpersonate uživatelů služby Active Directory. konektory Hello použijte tato oprávnění toosend a přijímat tokeny jejich jménem.

## <a name="how-single-sign-on-with-kcd-works"></a>Jak jednotné přihlašování s použitím KCD funguje
Tento diagram vysvětluje hello toku, když se uživatel pokusí tooaccess místní aplikace, která používá integrované ověřování systému Windows.

![Microsoft AAD ověřování vývojový diagram](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Hello uživatel zadá hello URL tooaccess hello místní aplikace prostřednictvím Proxy aplikace.
2. Proxy aplikace přesměruje hello požadavek tooAzure AD ověřování služby toopreauthenticate. V tomto okamžiku Azure AD platí všechny použitelné ověřování a zásady autorizace, jako například vícefaktorové ověřování. Pokud ověření hello uživatele Azure AD vytvoří token a odešle ho toohello uživatele.
3. uživatel Hello předá hello tokenu tooApplication proxy serveru.
4. Proxy aplikací ověří hello token a načte z něj hello hlavní název uživatele (UPN) a poté odešle hello požadavku, hello UPN a hello hlavní název služby (SPN) toohello konektor prostřednictvím obousměrně ověřené zabezpečeného kanálu.
5. Hello konektor provede vyjednávání protokolu Kerberos vynuceným delegování použitím (KCD) s hello místní AD, zosobňování uživatele tooget hello k aplikaci tokenu toohello protokolu Kerberos.
6. Služby Active Directory odešle hello token protokolu Kerberos pro toohello aplikace hello konektor.
7. Hello konektor odešle hello původní žádost toohello aplikační server, pomocí tokenu hello protokolu Kerberos, obdrženého ze služby Active Directory.
8. aplikace Hello odešle odpověď toohello hello konektor, který je pak vrácen toohello Proxy aplikace služby a v neposlední řadě toohello uživatele.

## <a name="prerequisites"></a>Požadavky
Před zahájením práce s jednotné přihlašování pro aplikace, integrované ověřování systému Windows, ujistěte se, je prostředí připravené s hello následující nastavení a konfigurace:

* Aplikace, jako jsou aplikace webové služby SharePoint, jsou nastavené toouse integrované ověřování systému Windows. Další informace najdete v tématu [povolení podpory pro ověřování protokolem Kerberos](https://technet.microsoft.com/library/dd759186.aspx), nebo SharePoint najdete v tématu [plánování pro ověřování protokolem Kerberos v SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).
* Všechny aplikace mají [hlavní názvy služby](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).
* spuštěné hello Hello serveru konektoru a hello serveru se systémem hello aplikace jsou připojené k doméně a součástí hello stejné doméně nebo důvěryhodné domény. Další informace o připojení k doméně, najdete v části [připojení k počítači tooa domény](https://technet.microsoft.com/library/dd807102.aspx).
* Hello serveru se systémem hello konektor má přístup tooread hello TokenGroupsGlobalAndUniversal atribut pro uživatele. Toto výchozí nastavení může mít vliv zabezpečení posílení zabezpečení hello prostředí.

### <a name="configure-active-directory"></a>Konfigurace Active Directory
Konfigurace služby Active Directory Hello se liší, v závislosti na tom, jestli vaše konektor Proxy aplikace a hello aplikačního serveru jsou v hello stejné doméně, nebo ne.

#### <a name="connector-and-application-server-in-hello-same-domain"></a>Konektor a aplikační server v hello stejné doméně
1. Ve službě Active Directory, přejděte příliš**nástroje** > **uživatelé a počítače**.
2. Vyberte server hello spuštěným konektorem hello.
3. Klikněte pravým tlačítkem a vyberte **vlastnosti** > **delegování**.
4. Vyberte **důvěřovat tomuto počítači pro delegování pouze toospecified službám**. 
5. V části **toowhich služby tento účet může prokázat delegovanými přihlašovacími údaji** přidejte hello hodnotu pro hello identita SPN hello aplikačního serveru. To umožňuje hello konektor Proxy aplikace tooimpersonate uživatelů ve službě AD proti aplikace hello definované v seznamu hello.

   ![Snímek obrazovky okna vlastností konektoru SVR](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Konektor a aplikační server v různých doménách
1. Seznam požadavků pro práci s použitím KCD napříč doménami, najdete v části [omezené delegování Kerberos mezi doménami](https://technet.microsoft.com/library/hh831477.aspx).
2. Použití hello `principalsallowedtodelegateto` vlastnost hello konektor serveru tooenable hello Proxy aplikace toodelegate hello serveru konektoru. Aplikační server Hello `sharepointserviceaccount` a delegování server hello je `connectormachineaccount`. Pro systém Windows 2012 R2 tento kód použijte jako příklad:

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount

Sharepointserviceaccount může být účet počítače hello aktualizace Service Pack nebo účet služby, pod které hello aktualizace Service Pack běží fondu aplikací.

## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování 
1. Publikování aplikace podle toohello pokynů v tématu [publikování aplikací pomocí Proxy aplikace](application-proxy-publish-azure-portal.md). Ujistěte se, že tooselect **Azure Active Directory** jako hello **metoda předběžného ověření**.
2. Když vaše aplikace se zobrazí v seznamu hello podnikové aplikace, vyberte ho a klikněte na **jednotného přihlašování**.
3. Nastavení příliš hello jeden přihlašování v režimu**integrované ověřování systému Windows**.  
4. Zadejte hello **interní aplikace SPN** hello aplikačního serveru. V tomto příkladu je hello SPN pro naše publikovaná aplikace http/www.contoso.com. Tento hlavní název služby musí toobe v hello seznam služeb toowhich hello konektoru může prokázat delegovanými přihlašovacími údaji. 
5. Zvolte hello **delegované Identity přihlášení** pro konektor toouse hello jménem uživatele. Další informace najdete v tématu [práce s jinou místní a cloudové identity](#Working-with-different-on-premises-and-cloud-identities)

   ![Konfigurace pokročilé aplikace](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>Jednotné přihlašování pro aplikace systému Windows
Hello tok delegování protokolu Kerberos v Azure AD Application Proxy spustí, když služba Azure AD ověřuje uživatele hello v cloudu hello. Jakmile požadavek hello dorazí na místě, hello hello Azure AD Application Proxy connector vystavuje lístek protokolu Kerberos jménem uživatele hello interakcí s místní službou Active Directory. Tento proces je odkazované tooas Kerberos vynuceným delegování použitím (KCD). V hello další fázi, že je požadavek odeslán toohello back-end aplikace s Tento lístek protokolu Kerberos. Existuje několik protokolů, které definují, jak se toosend například požadavky. Většina serverů jiný systém než Windows očekávat Negotiate/SPNego, který se teď podporuje v Azure AD Application Proxy.

Další informace o protokolu Kerberos najdete v tématu [všechny chcete tooknow o použitím protokolu Kerberos omezené delegování (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

Aplikace pro Windows bez obvykle uživatelská jména uživatelů a názvy účtů SAM místo domény e-mailové adresy. Pokud tato situace vztahuje tooyour aplikace, musíte tooconfigure hello delegovaný přihlášení identity pole tooconnect cloudové identity tooyour aplikace identity. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>Práce s jinou místní a cloudové identity
Proxy aplikace se předpokládá, že uživatelé mají přesně hello stejnou identitu v hello cloudu a místně. Pokud to není hello případ, může stále můžete použitím KCD pro jednotné přihlašování. Konfigurace **delegované identity přihlášení** pro každou aplikaci toospecify identity, které se mají použít, při provádění jednotné přihlašování.  

Tato možnost umožňuje řada organizací, které mají různých místních a cloudových identit toohave jednotného přihlašování z hello cloudové tooon místní aplikace bez nutnosti hello uživatelé tooenter různých uživatelských jmen a hesel. To zahrnuje organizace který:

* Interně mají několik domén (joe@us.contoso.com, joe@eu.contoso.com) a jednu doménu v cloudu hello (joe@contoso.com).
* Interně mít název směrovat domény (joe@contoso.usa) a právní jeden v cloudu hello.
* Nepoužívejte názvy domén interně (Jan)
* Použít různé aliasy místní a v cloudu hello. Například joe-johns@contoso.com vs.joej@contoso.com  

Pomocí Proxy aplikace můžete vybrat které identity toouse tooobtain hello lístek protokolu Kerberos. Toto nastavení je na aplikaci. Některé z těchto možností jsou vhodné pro systémy, které nepřijímá formát e-mailové adresy, jiné jsou navržené pro alternativní přihlášení.

![Snímek obrazovky parametr identity delegované přihlášení](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Pokud se používá k přihlášení delegované identity, nemusí být hodnota hello jedinečný mezi všechny hello doménami nebo doménovými strukturami ve vaší organizaci. Tento problém se můžete vyhnout tím, že publikujete tyto aplikace dvakrát pomocí dvou různých skupin konektor. Vzhledem k tomu, že každá aplikace má cílovou skupinu jiného uživatele, můžete připojit k jeho konektory tooa jinou doménu.

### <a name="configure-sso-for-different-identities"></a>Konfigurovat jednotné přihlašování pro různé identity
1. Konfigurace nastavení Azure AD Connect, tak, aby hlavní identita hello hello e-mailovou adresu (e-mailu). To slouží jako součást hello přizpůsobení procesu, změna hello **hlavní název uživatele** pole v nastavení synchronizace hello. Tato nastavení taky určit, jak uživatelé přihlašovat tooOffice365, Windows10 zařízení a jiných aplikací, které používají Azure AD jako své úložiště identit.  
   ![Identifikace uživatelů – snímek obrazovky - rozevírací hlavní název uživatele](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. V nastavení konfigurace aplikace hello aplikace hello byste chtěli toomodify, vyberte hello **delegované Identity přihlášení** toobe použít:

   * Hlavní název uživatele (například joe@contoso.com)
   * Alternativní hlavní název uživatele (například joed@contoso.local)
   * Uživatelské jméno součástí hlavní název uživatele (například Jan)
   * Uživatelské jméno součástí alternativní hlavní název uživatele (například joed)
   * Název účtu SAM místní (závisí na konfiguraci řadiče domény hello)

### <a name="troubleshooting-sso-for-different-identities"></a>Řešení potíží s jednotného přihlašování pro různé identity
Pokud dojde k chybě v hello proces jednotné přihlašování, zobrazí se v protokolu událostí počítače hello konektor jak je popsáno v [Poradce při potížích s](application-proxy-back-end-kerberos-constrained-delegation-how-to.md).
Ale v některých případech hello je požadavek úspěšně odeslán toohello back-end aplikace během této aplikace reaguje v různých dalších odpovědi HTTP. Řešení potíží s těchto případech by se měl spustit tak, že prověří číslo události 24029 na počítači konektor hello v protokolu událostí relaci Proxy aplikace hello. v poli "user" hello v rámci hello podrobnosti události se zobrazí Hello identitu uživatele, která byla použita pro delegování. Vyberte tooturn v relaci protokolu **ukazují analytické a ladicí protokoly** v nabídce Zobrazit prohlížeč událostí hello.

## <a name="next-steps"></a>Další kroky

* [Jak tooconfigure toouse aplikace Proxy aplikace omezené delegování Kerberos](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Řešení problémů, které máte s pomocí Proxy aplikace](active-directory-application-proxy-troubleshoot.md)


Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)

