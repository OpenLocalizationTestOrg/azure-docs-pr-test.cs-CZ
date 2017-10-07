---
title: "Azure AD Connect: Aktualizace hello certifikát SSL pro farmy služby Active Directory Federation Services (AD FS) | Microsoft Docs"
description: "Tento dokument podrobnosti hello kroky tooupdate hello certifikátu SSL farmu služby AD FS pomocí Azure AD Connect."
services: active-directory
keywords: "služby Azure ad connect, aktualizace ssl služby AD FS, aktualizace certifikátů služby AD FS, změnit certifikát služby AD FS, nový certifikát služby AD FS, certifikát služby AD FS, aktualizace služby AD FS certifikát ssl, aktualizace ssl certifikát služby AD FS, nakonfigurovat certifikát ssl služby AD FS, služba AD FS, ssl, certifikát, certifikát komunikace služby AD FS, federation aktualizace, konfigurace federace, aad connect"
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: anandy
ms.openlocfilehash: bce7f75aab83b6abacb8472a6895054d137e10e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-hello-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Aktualizovat certifikát SSL hello pro farmu služby Active Directory Federation Services (AD FS)

## <a name="overview"></a>Přehled
Tento článek popisuje, jak můžete použít certifikát SSL hello tooupdate Azure AD Connect pro farmu služby Active Directory Federation Services (AD FS). Můžete certifikát SSL hello tooeasily hello Azure AD Connect nástroje aktualizace pro farmu hello AD FS, i v případě, že není vybraná metoda přihlašovací hello uživatele služby AD FS.

Můžete provést hello celou operaci aktualizace certifikát SSL pro farmy hello AD FS ve všech federace a servery Proxy webové aplikace (WAP) ve třech jednoduchých kroků:

![Tři kroky](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
>toolearn Další informace o certifikáty, které používají službu AD FS, najdete v části [Principy certifikátů používaných službou AD FS](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Požadavky

* **Farma služby AD FS**: Ujistěte se, že farmu služby AD FS je založená na Windows Server 2012 R2 nebo novější.
* **Azure AD Connect**: Ujistěte se, že hello verze služby Azure AD Connect je 1.1.443.0 nebo novější. Budete používat hello úloh **certifikát SSL služby FS aktualizace AD**.

![Úloha aktualizace SSL](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>Krok 1: Zadání informací farmy služby AD FS

Azure AD Connect automaticky pokusí tooobtain informace o farmu hello AD FS:
1. Dotaz na informace farmy hello ze služby AD FS (Windows Server 2016 nebo novější).
2. Odkazování na hello informace z předchozích spuštění, které jsou uloženy místně službou Azure AD Connect.

Můžete upravit hello seznam serverů, které se zobrazují přidáním nebo odebráním hello servery tooreflect hello aktuální konfiguraci farmy hello AD FS. Jakmile je poskytována informace o serveru hello, Azure AD Connect zobrazí hello připojení a aktuální stav certifikátu protokolu SSL.

![Informace o serveru AD FS](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

Pokud seznam hello obsahuje server, který je už součástí farmy hello AD FS, klikněte na tlačítko **odebrat** toodelete hello server hello seznamu serverů ve farmě služby AD FS.

![Offline serveru v seznamu](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> Odebrání serveru z hello seznam serverů služby AD FS farmy v Azure AD Connect je místní operace a aktualizace hello informace pro hello farmu služby AD FS, která udržuje Azure AD Connect místně. Azure AD Connect nezmění konfigurace hello při změně hello tooreflect služby AD FS.    

## <a name="step-2-provide-a-new-ssl-certificate"></a>Krok 2: Zadejte nový certifikát SSL

Když jste potvrdili hello informace o serverech farmy služby AD FS, Azure AD Connect požádá o nový certifikát SSL hello. Zadejte chráněný heslem PFX toocontinue hello instalaci certifikátu.

![Certifikát SSL](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

Po zadání hello certifikát Azure AD Connect projde řadu požadavky. Ověřte, zda je správný pro farmu služby AD FS hello tooensure hello certifikát, který hello certifikátu:

-   Hello subjektu název nebo alternativní název subjektu certifikátu hello je stejný jako název federační služby hello hello, nebo je certifikát se zástupným znakem.
-   Hello certifikát je platný pro více než 30 dní.
-   řetěz certifikátů Hello certifikát je platný.
-   Hello certifikát je chráněný heslem.

## <a name="step-3-select-servers-for-hello-update"></a>Krok 3: Vyberte servery pro aktualizaci hello

V dalším kroku hello vyberte hello servery, které je třeba certifikát SSL hello toohave aktualizovat. Servery, které jsou offline nelze vybrat pro aktualizaci hello.

![Vyberte servery tooupdate](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

Po dokončení konfigurace hello Azure AD Connect zobrazí hello zprávu, která označuje stav hello hello aktualizace a poskytuje možnost tooverify hello služby AD FS přihlášení.

![Dokončení konfigurace](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>Nejčastější dotazy

* **Co by měla být hello název subjektu certifikátu hello pro nový certifikát SSL služby AD FS hello?**

    Azure AD Connect kontroluje, zda subjektu název nebo alternativní název subjektu hello hello certifikátu obsahuje název federační služby hello. Například pokud název vaší služby federačního serveru fs.contoso.com, musí být název subjektu název nebo alternativní subjektu hello fs.contoso.com.  Certifikáty se zástupnými znaky, jsou také přijaty.

* **Proč se zobrazuje zpráva pro přihlašovací údaje znovu na stránce server hello WAP?**

    Pokud hello přihlašovacích údajů, které zadáte pro připojení serverů služby FS tooAD také nemají hello oprávnění toomanage hello WAP servery, pak Azure AD Connect vyzve k zadání přihlašovacích údajů, které mají oprávnění správce na serverech WAP hello.

* **Hello server se zobrazuje v režimu offline. Co bych měl/a dělat?**

    Azure AD Connect nelze provést všechny operace, pokud je hello server offline. Pokud je hello server součástí hello farmu služby AD FS, zkontrolujte hello připojení toohello serveru. Poté, co jste hello problém vyřešili, stiskněte tlačítko hello aktualizace ikonu tooupdate hello stav v Průvodci hello. Pokud hello server byl součástí z hello farmy dříve, ale teď už existuje, klikněte na **odebrat** toodelete hello seznamu serverů, které Azure AD Connect udržuje. Odebrání serveru hello hello seznamu ve službě Azure AD Connect není změnit hello konfigurace služby AD FS, sám sebe. Pokud používáte služby AD FS v systému Windows Server 2016 nebo novější, zůstanou server hello v nastavení konfigurace hello a zobrazí se znovu hello příštím hello úloha je spuštěna.

* **Můžete aktualizovat podmnožinu Moje servery ve farmě s hello nový certifikát SSL?**

    Ano. Hello úlohu lze spustit vždy **certifikát SSL aktualizace** znovu tooupdate hello zbývající servery. Na hello **vyberte servery pro protokol SSL certifikát aktualizace** stránky, můžete řadit hello seznam serverů na **datum vypršení platnosti SSL** tooeasily přístup hello servery, které ještě nejsou aktualizovány.

* **Po odebrání hello server hello předchozí, spuštění, ale stále se zobrazuje jako offline a uvedené na stránce hello AD FS servery. Proč je hello offline serveru stále existují i po jeho po odebrání?**

    Odebrání serveru hello hello seznamu ve službě Azure AD Connect neodstraní v hello konfigurace služby AD FS. Azure AD Connect odkazuje služby AD FS (Windows Server 2016 nebo vyšší) pro žádné informace o hello farmy. Pokud hello server se stále nachází na hello konfigurace služby AD FS, objeví se zpět v seznamu hello.  

## <a name="next-steps"></a>Další kroky

- [Azure AD Connect a federace](active-directory-aadconnectfed-whatis.md)
- [Přizpůsobení službou Azure AD Connect a správy služby Active Directory Federation Services](active-directory-aadconnect-federation-management.md)
