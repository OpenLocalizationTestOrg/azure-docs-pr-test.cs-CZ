---
title: "Postup telefonní hovor z Twilio (.NET) | Microsoft Docs"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořené v rozhraní .NET."
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
ms.openlocfilehash: 0899a49cbfda775017dab7fc6d8963bbeb86d74c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a><span data-ttu-id="0fec4-104">Postup telefonní hovor pomocí Twilio ve webové roli v Azure</span><span class="sxs-lookup"><span data-stu-id="0fec4-104">How to make a phone call using Twilio in a web role on Azure</span></span>
<span data-ttu-id="0fec4-105">Tato příručka ukazuje, jak používat Twilio k volání z webové stránky hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="0fec4-105">This guide demonstrates how to use Twilio to make a call from a web page hosted in Azure.</span></span> <span data-ttu-id="0fec4-106">Výsledná aplikace vyzve uživatele, aby volání s danou číslo a zprávy, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="0fec4-106">The resulting application prompts the user to make a call with the given number and message, as shown in the following screenshot.</span></span>

![Azure volání formulář s využitím Twilio a ASP.NET][twilio_dotnet_basic_form]

## <span data-ttu-id="0fec4-108"><a name="twilio-prereqs"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="0fec4-108"><a name="twilio-prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="0fec4-109">Budete muset následujícím postupem použít kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="0fec4-109">You will need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="0fec4-110">Získání účtu Twilio a ověřování z tokenu [Twilio konzoly][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="0fec4-110">Acquire a Twilio account and authentication token from the [Twilio Console][twilio_console].</span></span> <span data-ttu-id="0fec4-111">Začínáme s Twilio zaregistrovat na [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="0fec4-111">To get started with Twilio, sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="0fec4-112">Je možné vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="0fec4-112">You can evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="0fec4-113">Informace o rozhraní API poskytované Twilio najdete v tématu [http://www.twilio.com/voice/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="0fec4-113">For information about the API provided by Twilio, see [http://www.twilio.com/voice/api][twilio_api].</span></span>
2. <span data-ttu-id="0fec4-114">Přidat *knihovny Twilio .NET* pro vaši webovou roli.</span><span class="sxs-lookup"><span data-stu-id="0fec4-114">Add the *Twilio .NET libary* to your web role.</span></span> <span data-ttu-id="0fec4-115">V tématu **pro přidání do projektu webové role knihovny Twilio**dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="0fec4-115">See **To add the Twilio libraries to your web role project**, later in this topic.</span></span>

<span data-ttu-id="0fec4-116">Měli byste se seznámit s vytvářením základní [webové Role v Azure][azure_webroles_get_started].</span><span class="sxs-lookup"><span data-stu-id="0fec4-116">You should be familiar with creating a basic [Web Role on Azure][azure_webroles_get_started].</span></span>

## <span data-ttu-id="0fec4-117"><a name="howtocreateform"></a>Postupy: vytvoření webového formuláře pro volání</span><span class="sxs-lookup"><span data-stu-id="0fec4-117"><a name="howtocreateform"></a>How to: Create a web form for making a call</span></span>
<span data-ttu-id="0fec4-118"><a id="use_nuget"></a>Přidání knihovny Twilio do projektu webové role:</span><span class="sxs-lookup"><span data-stu-id="0fec4-118"><a id="use_nuget"></a>To add the Twilio libraries to your web role project:</span></span>

1. <span data-ttu-id="0fec4-119">Otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0fec4-119">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="0fec4-120">Klikněte pravým tlačítkem na **odkazy**.</span><span class="sxs-lookup"><span data-stu-id="0fec4-120">Right-click **References**.</span></span>
3. <span data-ttu-id="0fec4-121">Klikněte na tlačítko **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0fec4-121">Click **Manage NuGet Packages**.</span></span>
4. <span data-ttu-id="0fec4-122">Klikněte na tlačítko **Online**.</span><span class="sxs-lookup"><span data-stu-id="0fec4-122">Click **Online**.</span></span>
5. <span data-ttu-id="0fec4-123">Zadejte do vyhledávacího pole online *twilio*.</span><span class="sxs-lookup"><span data-stu-id="0fec4-123">In the search online box, type *twilio*.</span></span>
6. <span data-ttu-id="0fec4-124">Klikněte na tlačítko **nainstalovat** na balíček Twilio.</span><span class="sxs-lookup"><span data-stu-id="0fec4-124">Click **Install** on the Twilio package.</span></span>

<span data-ttu-id="0fec4-125">Následující kód ukazuje, jak vytvořit webového formuláře pro načtení dat uživatele pro volání.</span><span class="sxs-lookup"><span data-stu-id="0fec4-125">The following code shows how to create a web form to retrieve user data for making a call.</span></span> <span data-ttu-id="0fec4-126">V tomto příkladu webovou roli ASP.NET s názvem **TwilioCloud** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="0fec4-126">In this example, an ASP.NET Web Role named **TwilioCloud** is created.</span></span>

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

## <span data-ttu-id="0fec4-127"><a id="howtocreatecode"></a>Postupy: vytvoření kódu pro volání</span><span class="sxs-lookup"><span data-stu-id="0fec4-127"><a id="howtocreatecode"></a>How to: Create the code to make the call</span></span>
<span data-ttu-id="0fec4-128">Následující kód, který je volána, když uživatel dokončí formuláře, vytvoří zprávu volání a generuje volání.</span><span class="sxs-lookup"><span data-stu-id="0fec4-128">The following code, which is called when the user completes the form, creates the call message and generates the call.</span></span> <span data-ttu-id="0fec4-129">V tomto příkladu je kód spuštěn v obslužné rutině události onclick tlačítko ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="0fec4-129">In this example, the code is run in the onclick event handler of the button on the form.</span></span> <span data-ttu-id="0fec4-130">(Použití vašeho účtu Twilio a ověřování tokenu místo zástupné hodnoty přiřazené `accountSID` a `authToken` v následujícím kódu.)</span><span class="sxs-lookup"><span data-stu-id="0fec4-130">(Use your Twilio account and authentication token instead of the placeholder values assigned to `accountSID` and `authToken` in the code below.)</span></span>

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
            // the placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of the Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve the account, used later to retrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve the values entered by the user.
                var to = PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using the Twilio message and the user-entered
                // text. You must replace spaces in the user's text with '%20'
                // to make the text suitable for a URL.
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display the endpoint, API version, and the URL for the message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"The URL is {url}");

                // Place the call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

<span data-ttu-id="0fec4-131">Při volání a jsou zobrazeny Twilio koncový bod, verze rozhraní API a stav volání.</span><span class="sxs-lookup"><span data-stu-id="0fec4-131">The call is made, and the Twilio endpoint, API version, and the call status are displayed.</span></span> <span data-ttu-id="0fec4-132">Následující snímek obrazovky ukazuje výstup z ukázkové spuštění.</span><span class="sxs-lookup"><span data-stu-id="0fec4-132">The following screenshot shows output from a sample run.</span></span>

![Azure volání odpovědi pomocí Twilio a ASP.NET][twilio_dotnet_basic_form_output]

<span data-ttu-id="0fec4-134">Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml].</span><span class="sxs-lookup"><span data-stu-id="0fec4-134">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml].</span></span> <span data-ttu-id="0fec4-135">Další informace o &lt;indikované&gt; a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="0fec4-135">More information about &lt;Say&gt; and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>

## <span data-ttu-id="0fec4-136"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fec4-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="0fec4-137">Tento kód byl poskytnut tak, aby zobrazovalo základních funkcí pomocí Twilio webovou roli ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="0fec4-137">This code was provided to show you basic functionality using Twilio in an ASP.NET web role on Azure.</span></span> <span data-ttu-id="0fec4-138">Před nasazením do Azure v produkčním prostředí, můžete přidat další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="0fec4-138">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="0fec4-139">Například:</span><span class="sxs-lookup"><span data-stu-id="0fec4-139">For example:</span></span>

* <span data-ttu-id="0fec4-140">Místo použití webového formuláře, můžete použít úložiště objektů Blob v Azure nebo Azure SQL Database instance k ukládání telefonních čísel a volání text.</span><span class="sxs-lookup"><span data-stu-id="0fec4-140">Instead of using a web form, you could use Azure Blob storage or an Azure SQL Database instance to store phone numbers and call text.</span></span> <span data-ttu-id="0fec4-141">Informace o použití objektů BLOB v Azure najdete v tématu [jak používat službu úložiště objektů Blob v Azure v rozhraní .NET][howto_blob_storage_dotnet].</span><span class="sxs-lookup"><span data-stu-id="0fec4-141">For information about using Blobs in Azure, see [How to use the Azure Blob storage service in .NET][howto_blob_storage_dotnet].</span></span> <span data-ttu-id="0fec4-142">Informace o používání databáze SQL najdete v tématu [jak používat Azure SQL Database v aplikacích .NET][howto_sql_azure_dotnet].</span><span class="sxs-lookup"><span data-stu-id="0fec4-142">For information about using SQL Database, see [How to use Azure SQL Database in .NET applications][howto_sql_azure_dotnet].</span></span>
* <span data-ttu-id="0fec4-143">Můžete použít `RoleEnvironment.getConfigurationSettings` načíst Twilio ID účtu a ověřování tokenu z nastavení konfigurace vašeho nasazení, místo pevně kódováno hodnoty do formuláře.</span><span class="sxs-lookup"><span data-stu-id="0fec4-143">You could use `RoleEnvironment.getConfigurationSettings` to retrieve the Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding the values in your form.</span></span> <span data-ttu-id="0fec4-144">Informace o `RoleEnvironment` třídy najdete v tématu [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span><span class="sxs-lookup"><span data-stu-id="0fec4-144">For information about the `RoleEnvironment` class, see [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span></span>
* <span data-ttu-id="0fec4-145">Přečtěte si pokyny zabezpečení Twilio v [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="0fec4-145">Read the Twilio Security Guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>
* <span data-ttu-id="0fec4-146">Další informace o Twilio v [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="0fec4-146">Learn more about Twilio at [https://www.twilio.com/docs][twilio_docs].</span></span>

## <span data-ttu-id="0fec4-147"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="0fec4-147"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="0fec4-148">Jak používat Twilio pro hlasový a SMS možnosti z Azure</span><span class="sxs-lookup"><span data-stu-id="0fec4-148">How to use Twilio for Voice and SMS capabilities from Azure</span></span>](twilio-dotnet-how-to-use-for-voice-sms.md)

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
