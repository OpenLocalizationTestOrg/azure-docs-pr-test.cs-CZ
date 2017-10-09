---
title: "aaaSet vlastní domovskou stránku pro publikovaných aplikací pomocí proxy aplikace služby Azure AD | Microsoft Docs"
description: "Popisuje hello základní informace o Azure AD Application Proxy konektory"
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
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5bb695e904d285c3b440520f107c7bf63ba5cac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Nastavit vlastní domovskou stránku pro publikovaných aplikací pomocí proxy aplikace služby Azure AD

Tento článek popisuje jak tooconfigure aplikace toodirect uživatelé tooa vlastní domovskou stránku. Při publikování aplikace pomocí Proxy aplikací nastavíte interní adresa URL někdy je to ale není vaši uživatelé měli vidět první stránku hello. Nastavte vlastní domovskou stránku tak, aby vaši uživatelé přejít pravá stránka toohello při přístupu aplikace hello z hello přístupový Panel Azure Active Directory nebo Spouštěč aplikace hello Office 365.

Když uživatelé spustí aplikaci hello, budou se přesměruje ve výchozí toohello kořenové domény URL pro hello publikované aplikace. Cílová stránka Hello se obvykle nastavuje jako adresa URL domovskou stránku hello. Hello Azure AD PowerShell modulu toodefine vlastní domovskou stránku URL používejte, když chcete, aby uživatelé tooland aplikaci na konkrétní stránky v rámci aplikace hello. 

Například:
- Uvnitř podnikové sítě, uživatelé přejít příliš*https://ExpenseApp/login/login.aspx* toosign v a přístup k vaší aplikaci.
- Protože máte dalších prostředků, jako jsou bitové kopie, které Proxy aplikace potřebuje tooaccess na nejvyšší úrovni hello struktura složek hello, můžete publikovat aplikaci hello s *https://ExpenseApp* jako hello interní adresa URL.
- Hello výchozí externí adresa URL *https://ExpenseApp-contoso.msappproxy.net*, který neberou toohello přihlašovacích údajů uživatele na stránce.  
- Nastavit *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* jako toogive URL domovskou stránku hello uživatelům jednoduché prostředí. 

>[!NOTE]
>Když poskytnete uživatelům přístup toopublished aplikace, hello aplikace se zobrazují v hello [přístupový Panel Azure AD](active-directory-saas-access-panel-introduction.md) a hello [Spouštěč aplikace Office 365](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

## <a name="before-you-start"></a>Než začnete

Před nastavením URL domovskou stránku hello, mějte na paměti hello následující požadavky:

* Zkontrolujte, zda tento hello cesta, kterou zadáte cestu subdomény hello kořenové domény adresy URL.

  Pokud adresa URL hello kořenové domény, například https://apps.contoso.com/app1/, hello domovskou stránku adresa URL, kterou nakonfigurujete musí začínat https://apps.contoso.com/app1/.

* Pokud změníte toohello publikované aplikace, změnit hello může resetovat hello hodnotu adresy URL domovskou stránku hello. Při aktualizaci aplikace hello v hello budoucí doporučujeme znovu zkontrolovat a, v případě potřeby aktualizujte adresu URL hello domovské stránky.

## <a name="change-hello-home-page-in-hello-azure-portal"></a>Změnit domovskou stránku hello v hello portálu Azure

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.
2. Přejděte příliš**Azure Active Directory** > **registrace aplikace** a vyberte aplikaci ze seznamu hello. 
3. Vyberte **vlastnosti** z nastavení hello.
4. Aktualizace hello **adresa URL domovské stránky** pole s novou cestu. 

   ![Zadejte novou adresu URL domovské stránky](./media/application-proxy-office365-app-launcher/homepage.png)

5. Vyberte **uložit**

## <a name="change-hello-home-page-with-powershell"></a>Změnit hello domovskou stránku pomocí prostředí PowerShell

### <a name="install-hello-azure-ad-powershell-module"></a>Instalace modulu Azure AD PowerShell hello

Před definujete adresu URL vlastní domovskou stránku pomocí prostředí PowerShell, nainstalujte modul Azure AD PowerShell hello. Hello balíček si můžete stáhnout z hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), která používá hello koncový bod rozhraní Graph API. 

tooinstall hello balíček, postupujte takto:

1. Otevřete standardní okno prostředí PowerShell a spusťte následující příkaz hello:

    ```
     Install-Module -Name AzureAD
    ```
    Pokud používáte hello příkaz jako bez oprávnění správce, použijte hello `-scope currentuser` možnost.
2. Během instalace hello vyberte **Y** tooinstall dva balíčky z Nuget.org. Oba balíčky jsou povinné. 

### <a name="find-hello-objectid-of-hello-app"></a>Najde hello ObjectID aplikace hello

Získat hello ObjectID hello aplikace a pak vyhledejte aplikace hello podle jeho domovskou stránku.

1. Otevřete prostředí PowerShell a naimportovat modul hello Azure AD.

    ```
    Import-Module AzureAD
    ```

2. Přihlaste se toohello modulu Azure AD jako správce klienta hello.

    ```
    Connect-AzureAD
    ```
3. Najít hello aplikace založené na jeho adresa URL domovské stránky. Adresa URL hello můžete najít v portálu hello přechodem příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**. Tento příklad používá *sharepoint iddemo*.

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. Měli byste obdržet výsledek, který je podobný toohello té, kterou tady vidíte. Zkopírujte hello ObjectID GUID toouse v další části hello.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-hello-home-page-url"></a>Aktualizovat adresu URL domovskou stránku hello

V hello provádět stejné modul prostředí PowerShell, který jste použili v kroku 1, hello následující kroky:

1. Potvrďte, že máte hello opravte aplikace a nahradit *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* s hello ObjectID, kterou jste zkopírovali v předchozím kroku hello.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Teď, když jste potvrzení aplikace hello jste připravené tooupdate hello domovskou stránku, následujícím způsobem.

2. Vytvořte objekt toohold prázdné aplikace hello změny, které chcete toomake. Tato proměnná obsahuje hello hodnoty, které chcete tooupdate. Nic se vytvoří v tomto kroku.

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. Hello domovskou stránku URL toohello hodnotu, která chcete nastavit. Hello hodnota musí být cesta subdomény hello publikované aplikace. Například pokud změníte hello adresu URL domovské stránky z *https://sharepoint-iddemo.msappproxy.net/* příliš*https://sharepoint-iddemo.msappproxy.net/hybrid/*, aplikace uživatelé přejít přímo toohello vlastní Domovská stránka.

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. Ujistěte se, hello aktualizace pomocí hello GUID (ObjectID), kterou jste zkopírovali v "krok 1: najít hello ObjectID hello aplikace."

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. tooconfirm, že změna hello bylo úspěšné, restartujte aplikace hello.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Všechny změny provedené v aplikaci toohello může resetovat URL domovskou stránku hello. Pokud adresa URL domovské stránky obnoví, opakujte krok 2.

## <a name="next-steps"></a>Další kroky

- [Povolit tooSharePoint vzdáleného přístupu s proxy aplikace služby Azure AD](application-proxy-enable-remote-access-sharepoint.md)
- [Povolení Proxy aplikace v hello portálu Azure](active-directory-application-proxy-enable.md)
