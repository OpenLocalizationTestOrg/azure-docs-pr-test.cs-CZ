---
title: aaaAzure Mobile Engagement - integraci back-end
description: "Připojení Azure Mobile Engagement s SharePoint back-end toocreate kampaně ze služby SharePoint"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 06297b43-579f-46e6-8a58-961a68f9aa09
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 89e1ef57db607d63c326a760b20260ad439f08b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---api-integration"></a><span data-ttu-id="a7d8a-103">Azure Mobile Engagement – integrace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a7d8a-103">Azure Mobile Engagement - API integration</span></span>
<span data-ttu-id="a7d8a-104">V automatizovaný systém marketing vytvoření a aktivace hello marketingových kampaní také dojít k automaticky.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-104">In an automated marketing system, creating and activating hello marketing campaigns also occur automatically.</span></span> <span data-ttu-id="a7d8a-105">Pro tento účel - Azure Mobile Engagement umožňuje vytváření takové automatizované marketingových kampaní, které jsou také pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-105">For this purpose - Azure Mobile Engagement enables creating such automated marketing campaigns using APIs as well.</span></span> 

<span data-ttu-id="a7d8a-106">Obvykle zákazníci používat jako součást svých marketingových kampaní hello Mobile Engagement front-endové rozhraní toocreate oznámení nebo hlasování atd.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-106">Typically customers use hello Mobile Engagement front end interface toocreate announcements/polls etc as part of their marketing campaigns.</span></span> <span data-ttu-id="a7d8a-107">Ale jako hello marketingových kampaní stát vyspělá, je třeba tooleverage hello data zamknutý v hello back-end systémy (například systému CRM nebo systém CMS jako SharePoint), aby bylo možné vytvořit plně automatizovaného kanálu vytváří kampaně v Mobile Zapojení dynamicky podle hello dat odesílaných v z back-end systémy hello.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-107">However as hello marketing campaigns become mature, there is a need tooleverage hello data locked in hello backend systems (like any CRM system or CMS system like SharePoint) so that a fully automated pipeline can be created which creates campaigns in Mobile Engagement dynamically based on hello data flowing in from hello backend systems.</span></span> 

![][5]

<span data-ttu-id="a7d8a-108">Tento kurz vloží prostřednictvím scénáři, kde uživatel obchodní SharePoint naplní seznam serveru SharePoint s marketingové dat a automatizovaného procesu převezme položek seznamu hello a připojí hello Mobile Engagement systém pomocí hello k dispozici rozhraní REST API toocreate marketingovou kampaň z hello dat služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-108">This tutorial goes through such a scenario where a SharePoint business user populates a SharePoint list with marketing data and an automated process picks up items from hello list and connects with hello Mobile Engagement system using hello available REST APIs toocreate a marketing campaign from hello SharePoint data.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="a7d8a-109">Obecně platí můžete tato ukázka jako výchozí bod pro pochopení, jak toocall žádné Mobile Engagement REST API jako jeho podrobnosti hello dva klíčové aspekty volání hello rozhraní API - ověřování a předejte parametry.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-109">In general, you can use this sample as a starting point for understanding how toocall any Mobile Engagement REST API as it details hello two key aspects of calling hello APIs - authenticating and passing parameters.</span></span> 
> 
> 

## <a name="sharepoint-integration"></a><span data-ttu-id="a7d8a-110">Integrace služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="a7d8a-110">SharePoint integration</span></span>
1. <span data-ttu-id="a7d8a-111">Zde je ukázka jaké hello vypadá seznamu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-111">Here is what hello sample SharePoint list looks like.</span></span> <span data-ttu-id="a7d8a-112">**Název**, **kategorie**, **NotificationTitle**, **zpráva** a **URL** jsou používány k vytvoření hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-112">**Title**, **Category**, **NotificationTitle**, **Message** and **URL** are used for creating hello announcement.</span></span> <span data-ttu-id="a7d8a-113">Existuje sloupec s názvem **IsProcessed** který je používán procesem automatizace ukázka hello hello tvar program konzoly.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-113">There is a column called **IsProcessed** which is used by hello sample automation process in hello form of a console program.</span></span> <span data-ttu-id="a7d8a-114">Tento program konzoly můžete buď spustit jako webová úloha Azure tak, aby ji můžete naplánovat nebo přímo pomocí hello SharePoint pracovního postupu tooprogram vytvoření a aktivace hello oznámení při vložení položky do seznamu služby SharePoint hello.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-114">You can either run this console program as an Azure WebJob so that you can schedule it or you can directly use hello SharePoint workflow tooprogram creating and activating hello announcement when an item is inserted into hello SharePoint list.</span></span> <span data-ttu-id="a7d8a-115">V této ukázce používáme hello program konzoly, která přejde prostřednictvím položky hello v hello SharePoint seznam a vytvořit oznámení v Azure Mobile Engagementu pro každý z nich a nakonec označí hello **IsProcessed** příznak toobe true na vytvoření úspěšné oznámení.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-115">In this sample we use hello console program which goes through hello items in hello SharePoint list and create announcement in Azure Mobile Engagement for each of them and then finally marks hello **IsProcessed** flag toobe true on successful announcement creation.</span></span>
   
    ![][1]
2. <span data-ttu-id="a7d8a-116">Používáme hello kód z ukázkové hello *vzdáleného ověřování v hello SharePoint Online pomocí objektového modelu klienta* [sem](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate s hello seznamu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-116">We are using hello code from hello sample *Remote Authentication in SharePoint Online Using hello Client Object Model* [here](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate with hello SharePoint list.</span></span>
3. <span data-ttu-id="a7d8a-117">Po ověření, jsme projít toofind položky seznamu hello na všech nově vytvořených položek (které budou mít **IsProcessed** = false).</span><span class="sxs-lookup"><span data-stu-id="a7d8a-117">Once authenticated, we loop through hello list items toofind out any newly created items (which will have **IsProcessed** = false).</span></span> 
   
         static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);
   
                // Load hello SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
   
                // Loop through hello list toogo through all hello items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;
   
                        // Send an HTTP request toocreate AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;
   
                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this tootrue
                            item["IsProcessed"] = "Yes";
   
                            // Now try tooactivate hello campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a><span data-ttu-id="a7d8a-118">Integrace Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="a7d8a-118">Mobile Engagement integration</span></span>
1. <span data-ttu-id="a7d8a-119">Jakmile se nám najít položku, která vyžaduje zpracování – jsme extrahovat informace hello požadované toocreate oznámení z hello položky seznamu a volání `CreateAzMECampaign` toocreate jej a následně `ActivateAzMECampaign` tooactivate ho.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-119">Once we find an item which requires processing - we extract hello information required toocreate an announcement from hello list item and call `CreateAzMECampaign` toocreate it and subsequently `ActivateAzMECampaign` tooactivate it.</span></span> <span data-ttu-id="a7d8a-120">Toto jsou v podstatě volání rozhraní REST API volání toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-120">These are essentially REST API calls calling toohello Mobile Engagement backend.</span></span> 
2. <span data-ttu-id="a7d8a-121">vyžadovat, Hello Mobile Engagement REST API **základní ověřování schématu autorizační HTTP hlavičky** který se skládá z hello `ApplicationId` a hello `ApiKey` který můžete získat z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-121">hello Mobile Engagement REST APIs require a **Basic auth scheme authorization HTTP header** which is composed of hello `ApplicationId` and hello `ApiKey` which you get from hello Azure portal.</span></span> <span data-ttu-id="a7d8a-122">Ujistěte se, že používáte hello klíč z hello **klíče rozhraní api** části a *není* z hello **sdk klíče** části.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-122">Make sure that you are using hello Key from hello **api keys** section and *not* from hello **sdk keys** section.</span></span> 
   
   ![][2]
   
       static string CreateAuthZHeader()
       {
           string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
           return BasicAuthzHeaderString;
       }
   
       static public string EncodeTo64(string toEncode)
       {
           byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
           string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
           return returnValue;
       }  
3. <span data-ttu-id="a7d8a-123">Pro vytvoření typ oznámení hello kampaně – odkazovat toohello [dokumentaci](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7d8a-123">For creating hello announcement type campaign - refer toohello [documentation](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span></span> <span data-ttu-id="a7d8a-124">Je nutné se, že určíte hello kampaň toomake `kind` jako *oznámení* a hello [datové části](https://msdn.microsoft.com/library/azure/mt683751.aspx) a předejte ji jako FormUrlEncodedContent.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-124">You need toomake sure that you are specifying hello campaign `kind` as *announcement* and hello [payload](https://msdn.microsoft.com/library/azure/mt683751.aspx) and passing it as FormUrlEncodedContent.</span></span> 
   
        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";
   
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                // Create hello payload toosend hello content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });
   
                // Send hello POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is hello campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }
4. <span data-ttu-id="a7d8a-125">Jakmile máte hello oznámení vytvořen, zobrazí se něco podobného jako hello následující na portál Mobile Engagement hello (Všimněte si, že hello stav = Koncept a aktivované = není k dispozici)</span><span class="sxs-lookup"><span data-stu-id="a7d8a-125">Once you have hello announcement created, you will see something like hello following on hello Mobile Engagement portal (note that hello State=Draft and Activated = N/A)</span></span>
   
    ![][3]
5. <span data-ttu-id="a7d8a-126">`CreateAzMECampaign`Vytvoří oznámení kampaně a vrátí jeho Id toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-126">`CreateAzMECampaign` creates an announcement campaign and returns its Id toohello caller.</span></span> <span data-ttu-id="a7d8a-127">`ActivateAzMECampaign`Toto Id vyžaduje jako hello parametr tooactivate hello kampaně.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-127">`ActivateAzMECampaign` requires this Id as hello parameter tooactivate hello campaign.</span></span> 
   
        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });
   
                // Send hello POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }
6. <span data-ttu-id="a7d8a-128">Až budete mít hello oznámení aktivovat, zobrazí se něco podobného jako následující hello na portál Mobile Engagement hello:</span><span class="sxs-lookup"><span data-stu-id="a7d8a-128">Once you have hello announcement activated, you will see something like hello following on hello Mobile Engagement portal:</span></span>
   
    ![][4]
7. <span data-ttu-id="a7d8a-129">Při aktivaci kampaně hello získá jakékoli zařízení, které splňují kritéria hello na kampaň hello začne zobrazuje oznámení.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-129">As soon as hello campaign gets activated, any devices which satisfy hello criterion on hello campaign will start seeing notifications.</span></span> 
8. <span data-ttu-id="a7d8a-130">Také si všimnete, že položka seznamu hello označena s IsProcessed = false je nastavená tooTrue po vytvoření hello oznámení kampaně.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-130">You will also notice that hello list item marked with IsProcessed = false has been set tooTrue once hello announcement campaign is created.</span></span>  

<span data-ttu-id="a7d8a-131">Tato ukázka vytvořit jednoduché oznámení kampaň zadání většinou hello požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a7d8a-131">This sample created a simple announcement campaign specifying mostly hello required properties.</span></span> <span data-ttu-id="a7d8a-132">Tuto možnost můžete upravit z portálu hello co nejvíce, pomocí informací o hello [zde](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7d8a-132">You can customize this as much as you can from hello portal by using hello information [here](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span></span> 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



