---
title: "aaaMFA software development kit vlastních aplikací | Microsoft Docs"
description: "Tento článek ukazuje, jak toodownload a používání hello Azure MFA SDK tooenable dvoustupňové ověřování pro vaše vlastní aplikace."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 10e02e844bf3928575bfca79dbc34717a31a08b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Vytváření služby Multi-Factor Authentication do vlastní aplikace (SDK)

transakce procesy aplikací v klientovi služby Azure AD nebo vám Hello Azure Multi-Factor Authentication Software Development Kit (SDK) umožňuje vytvářet dvoustupňové ověření přímo do hello přihlášení.

Hello SDK služby Multi-Factor Authentication je k dispozici pro C#, Visual Basic (.NET), Java, Perl, PHP a Ruby. Hello SDK poskytuje dynamické obálku kolem dvoustupňové ověřování. Obsahuje všechno, co že potřebujete toowrite kódu, včetně soubory komentáři zdrojového kódu, například soubory a podrobné souboru ReadMe. Každý SDK zahrnuje také certifikát a soukromý klíč pro šifrování transakce, které jsou jedinečné tooyour zprostředkovatel vícefaktorového ověřování. Tak dlouho, dokud máte poskytovatele, si můžete stáhnout hello SDK v jako v mnoha jazycích a formáty podle potřeby.

Struktura Hello hello rozhraní API v hello Multi-Factor Authentication SDK je jednoduché. Ujistěte se, jedné funkce volání rozhraní API tooan s parametry Multi-Factor možnost hello (jako jsou ověřování režimu) a dat uživatele (například hello telefonní číslo toocall nebo číslo toovalidate hello kód PIN). Hello rozhraní API převede volání funkce hello do toohello žádosti webové služby založené na cloudu Azure Multi-Factor Authentication Service. Všechna volání musí obsahovat odkaz toohello privátní certifikát, který je zahrnuta v každé sadě SDK.

Protože hello rozhraní API není k dispozici přístup toousers zaregistrované v Azure Active Directory, je třeba zadat informace o uživateli v souboru nebo databáze. Navíc hello rozhraní API neposkytují funkce správy zápisu nebo uživatele, proto musíte toobuild tyto procesy do vaší aplikace.

> [!IMPORTANT]
> toodownload hello SDK, je nutné toocreate poskytovatele Azure Multi-Factor Auth i v případě, že máte licence Azure MFA, AAD Premium nebo EMS. Pokud jste pro tento účel vytvořit poskytovatele Azure Multi-Factor Auth a už máte licence, ujistěte se, zda toocreate hello zprostředkovatele s hello **za povoleného uživatele** modelu. Pak propojte hello zprostředkovatele toohello adresář, který obsahuje hello licence Azure MFA, Azure AD Premium nebo EMS. Tato konfigurace zajistí, že se pouze účtují Pokud máte více jedinečných uživatelů, kteří pomocí hello SDK než hello počet licencí, které vlastníte.


## <a name="download-hello-sdk"></a>Stáhnout hello SDK
Stahování hello SDK Azure Multi-Factor vyžaduje [zprostředkovatel vícefaktorového ověřování Azure](multi-factor-authentication-get-started-auth-provider.md).  To vyžaduje úplné předplatné, i když jsou ve vlastnictví licence Azure MFA, Azure AD Premium nebo Enterprise Mobility Suite.  hello toodownload SDK, přejděte toohello Multi-Factor Management Portal. Můžete uživatele oslovit hello portál Správa hello zprostředkovatel vícefaktorového ověřování přímo, nebo kliknutím hello **"Přejděte toohello portál"** odkaz na stránce nastavení služby hello vícefaktorového ověřování.

### <a name="download-from-hello-azure-classic-portal"></a>Stáhnout z portálu Azure classic hello
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce.
2. Na levé straně hello vyberte **služby Active Directory**.
3. Na stránce služby Active Directory hello v horní vyberte hello **zprostředkovatelé vícefaktorového ověřování**
4. V dolní části hello vyberte **spravovat**. Otevře se nová stránka.
5. Na hello ponechány na dolním hello, klikněte na tlačítko **SDK**.
   <center>![Stahování](./media/multi-factor-authentication-sdk/download.png)</center>
6. Vyberte hello jazyk a klikněte na jednu hello související odkazy na stažení.
7. Uložte hello stahování.

### <a name="download-from-hello-service-settings"></a>Stáhnout z nastavení služby hello
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce.
2. Na levé straně hello vyberte **služby Active Directory**.
3. Dvakrát klikněte na svoji instanci služby Azure AD.
4. V nahoře klikněte na hello **konfigurace**
5. V části ověřování Multi-Factor authentication, vyberte **spravovat nastavení služby**
   ![stáhnout](./media/multi-factor-authentication-sdk/download2.png)
6. Na stránce nastavení služby hello klikněte v hello dolní části obrazovky hello **přejděte toohello portál**. Otevře se nová stránka.
   ![Stáhnout](./media/multi-factor-authentication-sdk/download3a.png)
7. Na hello ponechány na dolním hello, klikněte na tlačítko **SDK**.
8. Vyberte hello jazyk a klikněte na jednu hello související odkazy na stažení.
9. Uložte hello stahování.

## <a name="whats-in-hello-sdk"></a>Co je v hello SDK
Hello SDK zahrnuje hello následující položky:

* **SOUBOR README**. Vysvětluje, jak toouse hello rozhraní API vícefaktorového ověřování v nové nebo existující aplikace.
* **Zdrojové soubory** pro službu Multi-Factor Authentication
* **Klientský certifikát** použijte toocommunicate s hello službou Multi-Factor Authentication
* **Privátní klíč** hello certifikátu
* **Výsledky volání.** Seznam kódy výsledků volání. tooopen tento soubor, použijte aplikaci s formátování textu, například WordPad. Použití hello tootest kódy výsledků volání a řešení potíží s hello implementaci vícefaktorového ověřování ve vaší aplikaci. Nejsou ověřování stavové kódy.
* **Příklady.** Ukázkový kód pro základní pracovní implementaci služby Multi-Factor Authentication.

> [!WARNING]
> Hello klientský certifikát je jedinečný privátní certifikát, který byl vygenerován speciálně pro vás. Sdílené složky nebo ztratit tento soubor. Je vaše klíče tooensuring hello zabezpečení komunikace se službou Multi-Factor Authentication hello.

## <a name="code-sample"></a>Ukázka kódu
Tento příklad ukazuje, jak toouse hello rozhraní API v hello sady SDK Azure Multi-Factor Authentication tooadd standardní režim hlasového volat ověření tooyour aplikace. Je standardní režim telefonního hovoru, který uživatel odpoví tooby stisknutím klávesy # hello hello.

Tento příklad používá hello C# .NET 2.0 Multi-Factor Authentication SDK v základní aplikace ASP.NET pomocí jazyka C# logiku na straně serveru, ale hello proces je podobný v dalších jazycích. Protože hello SDK obsahuje zdrojové soubory, není spustitelné soubory, lze vytvořit hello soubory a odkazujte na ně nebo je přímo do aplikace zahrnout.

> [!NOTE]
> Při implementaci vícefaktorového ověřování, použijte další metody hello (telefonního hovoru nebo textové zprávy) jako sekundární nebo terciární ověření toosupplement vaší primární metoda ověřování (uživatelské jméno a heslo). Tyto metody se nedají jako primární metody ověřování.

### <a name="code-sample-overview"></a>Přehled ukázka kódu
Tento ukázkový kód pro jednoduchou webovou aplikaci ukázku používá telefonního hovoru s ověřováním uživatele # klíče odpovědi tooverify hello. Tento faktor telefonní hovor je ověřování službou Multi-Factor Authentication označuje jako standardní režim.

kódu na straně klienta Hello neobsahuje žádné elementy specifické pro službu Multi-Factor Authentication. Protože hello další faktory ověřování jsou nezávislé na hello primární ověřování, můžete je přidat beze změny hello existujícího přihlašování rozhraní. Hello rozhraní API v hello SDK Multi-Factor umožňují přizpůsobit hello uživatelské prostředí, ale pravděpodobně nebudete potřebovat toochange nic vůbec.

kódu na straně serveru Hello přidá standardní režim ověřování v kroku 2. Vytvoří objekt PfAuthParams s hello parametry, které jsou požadovány pro standardní režim ověřování: uživatelské jméno, telefonní číslo a režim a hello cesta toohello klientský certifikát (CertFilePath), který je požadován při každém volání. Ukázka všech parametrů v PfAuthParams, najdete v části hello příklad souboru v hello SDK.

V dalším kroku hello kód předá hello PfAuthParams objekt toohello pf_authenticate() funkce. Návratová hodnota Hello označuje hello úspěšné nebo neúspěšné ověřování hello. Hello parametry, callStatus a ID chyby, obsahuje informace o další volání výsledek. kódy výsledků volání Hello jsou popsané v souboru výsledků volání hello v hello SDK.

Tato minimální implementace může být napsán v pár řádků. V produkčním kódu, bude však zahrnovat sofistikovanější zpracování chyb, kód další databáze a vylepšené uživatelské prostředí.

### <a name="web-client-code"></a>Kódu klienta webové
Hello následuje kódu klienta webové stránky ukázku.

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Kód na straně serveru
V hello následující kódu na straně serveru je Multi-Factor Authentication nakonfigurovat a spustit v kroku 2. Standardní režim (MODE_STANDARD) je, že uživatel hello toowhich telefonní hovor odpoví stisknutím klávesy # hello.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate hello username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from hello user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains hello private key for hello client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
