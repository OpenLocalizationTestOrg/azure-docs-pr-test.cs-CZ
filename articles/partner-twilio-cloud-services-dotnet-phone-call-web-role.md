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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-web-role-on-azure"></a><span data-ttu-id="118de-104">Jak toomake telefonní hovor pomocí Twilio ve webové roli v Azure</span><span class="sxs-lookup"><span data-stu-id="118de-104">How toomake a phone call using Twilio in a web role on Azure</span></span>
<span data-ttu-id="118de-105">Tato příručka ukazuje, jak toouse Twilio toomake volání z webové stránky hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="118de-105">This guide demonstrates how toouse Twilio toomake a call from a web page hosted in Azure.</span></span> <span data-ttu-id="118de-106">Hello výsledná aplikace vyzve uživatele toomake hello volání s hello daného číslo a zprávy, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="118de-106">hello resulting application prompts hello user toomake a call with hello given number and message, as shown in hello following screenshot.</span></span>

![Azure volání formulář s využitím Twilio a ASP.NET][twilio_dotnet_basic_form]

## <span data-ttu-id="118de-108"><a name="twilio-prereqs"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="118de-108"><a name="twilio-prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="118de-109">Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="118de-109">You will need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="118de-110">Získání účtu Twilio a ověřování tokenu z hello [Twilio konzoly][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="118de-110">Acquire a Twilio account and authentication token from hello [Twilio Console][twilio_console].</span></span> <span data-ttu-id="118de-111">tooget začal s Twilio, přihlaste na [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="118de-111">tooget started with Twilio, sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="118de-112">Je možné vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="118de-112">You can evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="118de-113">Informace o rozhraní API poskytované Twilio hello najdete v tématu [http://www.twilio.com/voice/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="118de-113">For information about hello API provided by Twilio, see [http://www.twilio.com/voice/api][twilio_api].</span></span>
2. <span data-ttu-id="118de-114">Přidat hello *knihovny Twilio .NET* tooyour webové role.</span><span class="sxs-lookup"><span data-stu-id="118de-114">Add hello *Twilio .NET libary* tooyour web role.</span></span> <span data-ttu-id="118de-115">V tématu **tooadd hello Twilio knihovny tooyour webový projekt role**dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="118de-115">See **tooadd hello Twilio libraries tooyour web role project**, later in this topic.</span></span>

<span data-ttu-id="118de-116">Měli byste se seznámit s vytvářením základní [webové Role v Azure][azure_webroles_get_started].</span><span class="sxs-lookup"><span data-stu-id="118de-116">You should be familiar with creating a basic [Web Role on Azure][azure_webroles_get_started].</span></span>

## <span data-ttu-id="118de-117"><a name="howtocreateform"></a>Postupy: vytvoření webového formuláře pro volání</span><span class="sxs-lookup"><span data-stu-id="118de-117"><a name="howtocreateform"></a>How to: Create a web form for making a call</span></span>
<span data-ttu-id="118de-118"><a id="use_nuget"></a>tooadd hello Twilio knihovny tooyour projekt webové role:</span><span class="sxs-lookup"><span data-stu-id="118de-118"><a id="use_nuget"></a>tooadd hello Twilio libraries tooyour web role project:</span></span>

1. <span data-ttu-id="118de-119">Otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="118de-119">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="118de-120">Klikněte pravým tlačítkem na **odkazy**.</span><span class="sxs-lookup"><span data-stu-id="118de-120">Right-click **References**.</span></span>
3. <span data-ttu-id="118de-121">Klikněte na tlačítko **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="118de-121">Click **Manage NuGet Packages**.</span></span>
4. <span data-ttu-id="118de-122">Klikněte na tlačítko **Online**.</span><span class="sxs-lookup"><span data-stu-id="118de-122">Click **Online**.</span></span>
5. <span data-ttu-id="118de-123">Zadejte online pole hledání hello *twilio*.</span><span class="sxs-lookup"><span data-stu-id="118de-123">In hello search online box, type *twilio*.</span></span>
6. <span data-ttu-id="118de-124">Klikněte na tlačítko **nainstalovat** na hello Twilio balíčku.</span><span class="sxs-lookup"><span data-stu-id="118de-124">Click **Install** on hello Twilio package.</span></span>

<span data-ttu-id="118de-125">Hello následující kód ukazuje, jak toocreate webové formuláři tooretrieve uživatelská data pro volání.</span><span class="sxs-lookup"><span data-stu-id="118de-125">hello following code shows how toocreate a web form tooretrieve user data for making a call.</span></span> <span data-ttu-id="118de-126">V tomto příkladu webovou roli ASP.NET s názvem **TwilioCloud** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="118de-126">In this example, an ASP.NET Web Role named **TwilioCloud** is created.</span></span>

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

## <span data-ttu-id="118de-127"><a id="howtocreatecode"></a>Postupy: vytvoření hello kód toomake hello volání</span><span class="sxs-lookup"><span data-stu-id="118de-127"><a id="howtocreatecode"></a>How to: Create hello code toomake hello call</span></span>
<span data-ttu-id="118de-128">Hello následující kód, který je volána, když uživatel hello dokončí hello formuláře, vytvoří zprávu volání hello a vygeneruje hello volání.</span><span class="sxs-lookup"><span data-stu-id="118de-128">hello following code, which is called when hello user completes hello form, creates hello call message and generates hello call.</span></span> <span data-ttu-id="118de-129">V tomto příkladu hello kód běží v obslužné rutiny události onclick hello hello tlačítko ve formuláři hello.</span><span class="sxs-lookup"><span data-stu-id="118de-129">In this example, hello code is run in hello onclick event handler of hello button on hello form.</span></span> <span data-ttu-id="118de-130">(Použití vašeho účtu Twilio a ověřování tokenu místo hello zástupné hodnoty přiřazené příliš`accountSID` a `authToken` v hello kód níže.)</span><span class="sxs-lookup"><span data-stu-id="118de-130">(Use your Twilio account and authentication token instead of hello placeholder values assigned too`accountSID` and `authToken` in hello code below.)</span></span>

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

<span data-ttu-id="118de-131">Při volání Hello a jsou zobrazeny hello Twilio koncový bod, verze rozhraní API a stav volání hello.</span><span class="sxs-lookup"><span data-stu-id="118de-131">hello call is made, and hello Twilio endpoint, API version, and hello call status are displayed.</span></span> <span data-ttu-id="118de-132">Následující snímek obrazovky ukazuje výstup z ukázku spustit Hello.</span><span class="sxs-lookup"><span data-stu-id="118de-132">hello following screenshot shows output from a sample run.</span></span>

![Azure volání odpovědi pomocí Twilio a ASP.NET][twilio_dotnet_basic_form_output]

<span data-ttu-id="118de-134">Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml].</span><span class="sxs-lookup"><span data-stu-id="118de-134">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml].</span></span> <span data-ttu-id="118de-135">Další informace o &lt;indikované&gt; a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="118de-135">More information about &lt;Say&gt; and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>

## <span data-ttu-id="118de-136"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="118de-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="118de-137">Tento kód byl poskytnut tooshow je základní funkce pomocí Twilio webovou roli ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="118de-137">This code was provided tooshow you basic functionality using Twilio in an ASP.NET web role on Azure.</span></span> <span data-ttu-id="118de-138">Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="118de-138">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="118de-139">Například:</span><span class="sxs-lookup"><span data-stu-id="118de-139">For example:</span></span>

* <span data-ttu-id="118de-140">Místo použití webového formuláře, může používat úložiště objektů Blob v Azure nebo Azure SQL Database instance toostore telefonních čísel a volání text.</span><span class="sxs-lookup"><span data-stu-id="118de-140">Instead of using a web form, you could use Azure Blob storage or an Azure SQL Database instance toostore phone numbers and call text.</span></span> <span data-ttu-id="118de-141">Informace o použití objektů BLOB v Azure najdete v tématu [jak toouse hello služby úložiště objektů Blob Azure v rozhraní .NET][howto_blob_storage_dotnet].</span><span class="sxs-lookup"><span data-stu-id="118de-141">For information about using Blobs in Azure, see [How toouse hello Azure Blob storage service in .NET][howto_blob_storage_dotnet].</span></span> <span data-ttu-id="118de-142">Informace o používání databáze SQL najdete v tématu [jak toouse Azure SQL databáze v aplikacích .NET][howto_sql_azure_dotnet].</span><span class="sxs-lookup"><span data-stu-id="118de-142">For information about using SQL Database, see [How toouse Azure SQL Database in .NET applications][howto_sql_azure_dotnet].</span></span>
* <span data-ttu-id="118de-143">Můžete použít `RoleEnvironment.getConfigurationSettings` ID účtu Twilio hello tooretrieve a ověřování tokenu z nastavení konfigurace vašeho nasazení, místo pevně kódováno hello hodnoty do formuláře.</span><span class="sxs-lookup"><span data-stu-id="118de-143">You could use `RoleEnvironment.getConfigurationSettings` tooretrieve hello Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding hello values in your form.</span></span> <span data-ttu-id="118de-144">Informace o hello `RoleEnvironment` třídy najdete v tématu [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span><span class="sxs-lookup"><span data-stu-id="118de-144">For information about hello `RoleEnvironment` class, see [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span></span>
* <span data-ttu-id="118de-145">Přečtěte si pokyny pro zabezpečení Twilio hello v [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="118de-145">Read hello Twilio Security Guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>
* <span data-ttu-id="118de-146">Další informace o Twilio v [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="118de-146">Learn more about Twilio at [https://www.twilio.com/docs][twilio_docs].</span></span>

## <span data-ttu-id="118de-147"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="118de-147"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="118de-148">Jak toouse Twilio pro hlasový a SMS možnosti z Azure</span><span class="sxs-lookup"><span data-stu-id="118de-148">How toouse Twilio for Voice and SMS capabilities from Azure</span></span>](twilio-dotnet-how-to-use-for-voice-sms.md)

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
