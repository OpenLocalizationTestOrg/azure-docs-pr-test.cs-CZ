---
title: "aaaCustom domén v Azure AD Application Proxy | Microsoft Docs"
description: "Správa vlastních domén v Azure AD Application Proxy, abyste tuto hello adresu URL pro aplikaci hello je hello stejný bez ohledu na to, kde uživatelé k němu přístup."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7a433c411976077210a2435c3c087991c7430755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Práce s vlastní domény v Azure AD Application Proxy

Při publikování aplikace prostřednictvím Proxy aplikace služby Active Directory Azure, vytvořte externí adresu URL pro vaše toowhen toogo uživatelů, které pracují vzdáleně. Tato adresa URL získá hello výchozí doménu *yourtenant.msappproxy.net*. Například pokud jste publikovali aplikaci s názvem výdaje a váš klient je s názvem Contoso, pak hello externí adresa URL bude https://expenses-contoso.msappproxy.net. Pokud chcete toouse svůj vlastní název domény, nakonfigurujte vlastní doménu pro vaši aplikaci. 

Doporučujeme, abyste nastavili vlastní domény pro vaše aplikace, pokud je to možné. Mezi výhody hello vlastní domény patří:

- Uživatelé dostanou toohello aplikace s hello stejnou adresu URL, jestli pracují uvnitř nebo vně vaší sítě.
- Pokud mají všechny aplikace hello stejné interní a externí adresy URL, pokračujte odkazy v jedné aplikace, které odkazují tooanother toowork i mimo firemní síť hello. 
- Řízení branding a vytvoření adresy URL hello chcete. 


## <a name="configure-a-custom-domain"></a>Konfigurace vlastní domény

### <a name="prerequisites"></a>Požadavky

Než začnete konfigurovat vlastní doménu, ujistěte se, že máte hello následující požadavky, které jsou připravené: 
- A [ověřené domény přidat tooAzure služby Active Directory](active-directory-domains-add-azure-portal.md).
- Vlastní certifikát pro doménu hello hello tvar souboru PFX. 
- Místní aplikace [publikované prostřednictvím Proxy aplikace](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Konfigurace vlastní domény

Až budete mít tyto tři požadavky připraven, postupujte podle těchto kroků tooset si vaši vlastní doménu:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Přejděte příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** a vyberte aplikaci, hello chcete toomanage.
3. Vyberte **Proxy aplikace**. 
4. V poli hello externí adresu URL použijte hello rozevíracího seznamu tooselect vaši vlastní doménu. Pokud nevidíte doménu v seznamu hello, pak jej nebyla ověřena ještě. 
5. Vyberte **uložit**
5. Hello **certifikát** nepovolíte pole, která byla zakázána. Toto políčko zaškrtněte. 

   ![Klikněte na tlačítko tooupload certifikát](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Pokud jste již odeslali certifikát pro tuto doménu, pole certifikátu hello zobrazí informace o certifikátu hello. 

6. Nahrání certifikátu PFX hello a zadejte hello heslo pro certifikát hello. 
7. Vyberte **Uložit** toosave změny. 
8. Přidat [záznam DNS](../dns/dns-operations-recordsets-portal.md) , přesměrování hello nové externí domény msappproxy.net toohello adresy URL. 

>[!TIP] 
>Potřebujete jenom jeden certifikát tooupload za vlastní doménu. Jakmile nahrajete certifikát, můžete zvolit vlastní domény hello při publikování nové aplikace a není nutné toodo další konfigurace s výjimkou záznamu DNS hello. 

## <a name="manage-certificates"></a>Správa certifikátů

### <a name="certificate-format"></a>Formát certifikátu
Neexistuje žádné omezení na hello certifikát podpis metody. Šifrování ECC (Elliptic Curve), alternativní název předmětu (SAN) a další běžné typy certifikátů jsou všechny podporované. 

Certifikát se zástupným znakem můžete použít také zástupné hello odpovídá hello potřeby externí adresu URL. 

Můžete použít certifikáty podepsané svým držitelem, také. Pokud používáte privátní certifikační autority, by měly být veřejné hello distribučního místa seznamu CRL (certifikát odvolání bodu distribučního bodu) pro certifikát hello.

### <a name="changing-hello-domain"></a>Změna domény hello
Všechny ověřené domény jsou uvedeny v seznamu rozevírací hello externí adresu URL pro vaši aplikaci. toochange hello domény, jenom aktualizace, která pole aplikace hello. Pokud chcete domény hello není v seznamu hello [přidejte jej jako ověřené domény](active-directory-domains-add-azure-portal.md). Pokud vyberete domény, který ještě nemá přidružený certifikát, postupujte podle kroků 5 až 7 tooadd hello certifikátu. Ujistěte se, že aktualizujete hello tooredirect záznamů DNS z hello nový externí adresu URL. 

### <a name="certificate-management"></a>Správa certifikátů
Můžete použít stejný certifikát pro více aplikací, pokud aplikace hello sdílet externím hostiteli hello. 

Se zobrazí upozornění, když vyprší platnost certifikátu, informující o tooupload jiný certifikát prostřednictvím portálu hello. Hello certifikát je odvolaný, může uživatelům přístup k aplikaci hello zobrazí upozornění zabezpečení. Neprovádějte jsme kontroly odvolání certifikátů.  tooupdate hello certifikát pro danou aplikaci, přejděte toohello aplikace a postupujte podle kroků 5 až 7 pro konfiguraci vlastních domén na publikované aplikace tooupload nový certifikát. Pokud hello starý certifikát není používán jinými aplikacemi, je automaticky odstraněna. 

Aktuálně všechny správy certifikátů je prostřednictvím stránky jednotlivých aplikací, takže budete potřebovat certifikáty toomanage v kontextu hello hello příslušných aplikací. 

## <a name="next-steps"></a>Další kroky
* [Povolit jednotné přihlašování](active-directory-application-proxy-sso-using-kcd.md) tooyour publikované aplikace pomocí ověřování Azure AD.
* [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md) tooyour publikované aplikace.
* [Přidejte název tooAzure vaše vlastní domény AD](active-directory-domains-add-azure-portal.md)


