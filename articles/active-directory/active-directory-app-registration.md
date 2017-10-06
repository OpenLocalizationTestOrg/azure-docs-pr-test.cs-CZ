---
title: aaaAzure registrace aplikace Active Directory | Microsoft Docs
description: "Tento článek popisuje, jak toouse hello Azure portálu tooregister aplikace s Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: priyamo
ms.reviewer: elisol
ms.openlocfilehash: 0134e299dcc53919a6f789a0878a1cf64a8e244d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Registrace aplikace pomocí klienta služby Azure Active Directory

Aplikace můžete použít hello Azure portálu tooregister s klienta služby Azure Active Directory (Azure AD). Tím se vytvoří ID aplikací pro aplikace hello a povolí ho tooreceive tokeny.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V levém navigačním podokně hello zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.
4. Postupujte podle pokynů hello a vytvořte novou aplikaci. Pokud chcete konkrétní příklady pro webové aplikace nebo nativních aplikací, podívejte se na naše [– elementy QuickStart](active-directory-developers-guide.md).
  * Pro webové aplikace, zadejte hello **přihlašovací adresa URL**, což je hello základní adresu URL aplikace, kde uživatelé se mohou přihlásit v např `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * U nativních aplikací, zadejte **identifikátor URI pro přesměrování**, které Azure AD používá tooreturn odpovědi tokenu. Zadejte hodnotu konkrétní tooyour aplikace pomocí. např`http://MyFirstAADApp`
5. Po dokončení registrace Azure AD přiřadí aplikace identifikátor jedinečných klientských hello ID aplikace.

## <a name="update-application-settings-from-hello-azure-portal"></a>Aktualizovat nastavení aplikace z portálu Azure hello

Můžete snadno upravit existující aplikaci nastavení pomocí portálu Azure hello. Například můžete tooconfigure adresa URL odpovědi, což je, kde Azure AD vydá token odpovědi. Můžete také tooconfigure oprávnění tooother aplikace, pro instance tooallow tooaccess vaší aplikace hello Microsoft Graph API. Můžete k tomu všechny prostřednictvím stránky nastavení aplikace hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V levém navigačním podokně hello zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a vyberte aplikaci ze seznamu hello.
4. Klikněte na tlačítko **nastavení** tooopen stránku hello nastavení pro aplikace hello.
  * Hello **vlastnosti** stránce lze upravit hello obecné informace pro aplikace hello. To zahrnuje název aplikace hello, hello přihlašovací adresa URL a adresy URL odhlašovací hello.
  * Hello **adresy URL odpovědí** stránka vám umožní tooadd adresa URL odpovědi, což je, kde Azure AD odešle token odpovědi.
  * Hello **vlastníky** stránka vám umožní tooadd vlastníci aplikace.
  * Hello **oprávnění** stránka vám umožní tooconfigure oprávnění pro aplikace hello. Například tooaccess hello Microsoft Graph API, klikněte na možnost **přidat** a vyberte **Microsoft Graph** v hello rozhraní API pro výběr, zvolte hello oprávnění požadované, například **čtení dat adresáře** .
  * Hello **klíče** stránka vám umožní aplikaci tooadd tajných klíčů. Hello tajný klíč zobrazí pouze jednou okamžitě po vytvoření, tak zkontrolujte, zda toocopy ji pro další použití.

## <a name="use-hello-inline-manifest-editor"></a>Pomocí editoru manifestu vložené hello

Můžete použít hello vložené manifestu editor toomodify určité vlastnosti aplikace, které nejsou vystaveny přímo v hello portálu Azure. Například ho můžete použít identifikátor ID URI aplikace hello toomodify aplikace nebo tooenable hello OAuth2.0 implicitního toku místo hello výchozí autorizační kód tok poskytování.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V levém navigačním podokně hello zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a vyberte aplikaci ze seznamu hello.
4. Klikněte na tlačítko **Manifest** z hello stránky tooopen hello vložené manifestu editoru aplikace.
5. Přímo vám provádět změny toohello manifest a uložit ho, až budete připraveni. Alternativně můžete stáhnout manifestu tooopen hello jeho v oblíbeném editoru a nahrání hello aktualizace manifestu.

## <a name="next-steps"></a>Další kroky

1. Podívejte se na hello [– elementy QuickStart](active-directory-developers-guide.md) pro podrobné návody pro aplikace pro ověřování pomocí služby Azure AD.
2. Podívejte se na naše úplný seznam ukázky kódu v [Githubu](https://github.com/azure-samples).
