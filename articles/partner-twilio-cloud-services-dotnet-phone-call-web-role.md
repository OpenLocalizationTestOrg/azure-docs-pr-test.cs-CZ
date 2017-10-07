---
title: "aaaHow toomake telefonní hovor z Twilio (.NET) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořené v rozhraní .NET."
services: 
documentationcenter: .net
author: devinrader
manager: timlt
editor: 
ms.assetid: 789185ad-69dc-4e9e-a936-42e0a25315c8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/04/2016
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 857d89961c563a51fef944f4a72828036af79b43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Jak toomake telefonní hovor pomocí Twilio ve webové roli v Azure
Tato příručka ukazuje, jak toouse Twilio toomake volání z webové stránky hostované v Azure. Hello výsledná aplikace vyzve uživatele toomake hello volání s hello daného číslo a zprávy, jak ukazuje následující snímek obrazovky hello.

![Azure volání formulář s využitím Twilio a ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Požadavky
Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:

1. Získání účtu Twilio a ověřování tokenu z hello [Twilio konzoly][twilio_console]. tooget začal s Twilio, přihlaste na [https://www.twilio.com/try-twilio][try_twilio]. Je možné vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing]. Informace o rozhraní API poskytované Twilio hello najdete v tématu [http://www.twilio.com/voice/api][twilio_api].
2. Přidat hello *knihovny Twilio .NET* tooyour webové role. V tématu **tooadd hello Twilio knihovny tooyour webový projekt role**dál v tomto tématu.

Měli byste se seznámit s vytvářením základní [webové Role v Azure][azure_webroles_get_started].

## <a name="howtocreateform"></a>Postupy: vytvoření webového formuláře pro volání
<a id="use_nuget"></a>tooadd hello Twilio knihovny tooyour projekt webové role:

1. Otevřete řešení v sadě Visual Studio.
2. Klikněte pravým tlačítkem na **odkazy**.
3. Klikněte na tlačítko **Správa balíčků NuGet**.
4. Klikněte na tlačítko **Online**.
5. Zadejte online pole hledání hello *twilio*.
6. Klikněte na tlačítko **nainstalovat** na hello Twilio balíčku.

Hello následující kód ukazuje, jak toocreate webové formuláři tooretrieve uživatelská data pro volání. V tomto příkladu webovou roli ASP.NET s názvem **TwilioCloud** je vytvořena.

```aspx
<%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
    AutoEventWireup="true" CodeBehind="Default.aspx.cs"
    Inherits="WebRole1._Default" %>

<asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
</asp:Content>
<asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
    <div>
        <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
        </asp:BulletedList>
    </div>
    <div>
        <p>Fill in all fields and click <b>Make this call</b>.</p>
        <div>
            To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
            Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
            <asp:Button ID="callpage" runat="server" Text="Make this call"
                onclick="callpage_Click" />
        </div>
    </div>
</asp:Content>
```

## <a id="howtocreatecode"></a>Postupy: vytvoření hello kód toomake hello volání
Hello následující kód, který je volána, když uživatel hello dokončí hello formuláře, vytvoří zprávu volání hello a vygeneruje hello volání. V tomto příkladu hello kód běží v obslužné rutiny události onclick hello hello tlačítko ve formuláři hello. (Použití vašeho účtu Twilio a ověřování tokenu místo hello zástupné hodnoty přiřazené příliš`accountSID` a `authToken` v hello kód níže.)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using Twilio;
using Twilio.Http;
using Twilio.Types;
using Twilio.Rest.Api.V2010;

namespace WebRole1
{
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void callpage_Click(object sender, EventArgs e)
        {
            // Call porcessing happens here.

            // Use your account SID and authentication token instead of
            // hello placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of hello Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve hello account, used later tooretrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve hello values entered by hello user.
                var too= PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using hello Twilio message and hello user-entered
                // text. You must replace spaces in hello user's text with '%20'
                // toomake hello text suitable for a URL.
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display hello endpoint, API version, and hello URL for hello message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"hello URL is {url}");

                // Place hello call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

Při volání Hello a jsou zobrazeny hello Twilio koncový bod, verze rozhraní API a stav volání hello. Následující snímek obrazovky ukazuje výstup z ukázku spustit Hello.

![Azure volání odpovědi pomocí Twilio a ASP.NET][twilio_dotnet_basic_form_output]

Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml]. Další informace o &lt;indikované&gt; a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Další kroky
Tento kód byl poskytnut tooshow je základní funkce pomocí Twilio webovou roli ASP.NET v Azure. Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce. Například:

* Místo použití webového formuláře, může používat úložiště objektů Blob v Azure nebo Azure SQL Database instance toostore telefonních čísel a volání text. Informace o použití objektů BLOB v Azure najdete v tématu [jak toouse hello služby úložiště objektů Blob Azure v rozhraní .NET][howto_blob_storage_dotnet]. Informace o používání databáze SQL najdete v tématu [jak toouse Azure SQL databáze v aplikacích .NET][howto_sql_azure_dotnet].
* Můžete použít `RoleEnvironment.getConfigurationSettings` ID účtu Twilio hello tooretrieve a ověřování tokenu z nastavení konfigurace vašeho nasazení, místo pevně kódováno hello hodnoty do formuláře. Informace o hello `RoleEnvironment` třídy najdete v tématu [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Přečtěte si pokyny pro zabezpečení Twilio hello v [https://www.twilio.com/docs/security][twilio_docs_security].
* Další informace o Twilio v [https://www.twilio.com/docs][twilio_docs].

## <a name="seealso"></a>Viz také
* [Jak toouse Twilio pro hlasový a SMS možnosti z Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
[azure_webroles_get_started]: https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-get-started
