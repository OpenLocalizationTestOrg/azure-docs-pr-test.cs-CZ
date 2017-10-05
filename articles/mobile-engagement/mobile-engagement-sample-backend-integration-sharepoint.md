---
title: "Azure Mobile Engagement – integrace back-end"
description: "Připojení Azure Mobile Engagement s back-end služby SharePoint, k vytvoření kampaně ze služby SharePoint"
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
ms.openlocfilehash: d49f1094f4c3f170f3618f3e19e42266f9ae8858
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement---api-integration"></a><span data-ttu-id="4e93b-103">Azure Mobile Engagement – integrace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e93b-103">Azure Mobile Engagement - API integration</span></span>
<span data-ttu-id="4e93b-104">V automatizovaný systém marketing vytvoření a aktivace marketingových kampaní taky dojít automaticky.</span><span class="sxs-lookup"><span data-stu-id="4e93b-104">In an automated marketing system, creating and activating the marketing campaigns also occur automatically.</span></span> <span data-ttu-id="4e93b-105">Pro tento účel - Azure Mobile Engagement umožňuje vytváření takové automatizované marketingových kampaní, které jsou také pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e93b-105">For this purpose - Azure Mobile Engagement enables creating such automated marketing campaigns using APIs as well.</span></span> 

<span data-ttu-id="4e93b-106">Obvykle zákazníkům použít rozhraní front-endu Mobile Engagement k vytvoření oznámení nebo hlasování podobně jako součást svých marketingových kampaní.</span><span class="sxs-lookup"><span data-stu-id="4e93b-106">Typically customers use the Mobile Engagement front end interface to create announcements/polls etc as part of their marketing campaigns.</span></span> <span data-ttu-id="4e93b-107">Ale protože stále vyspělá marketingových kampaní, je zapotřebí využívat data zamknutý v back-end systémy (například systému CRM nebo systém CMS jako SharePoint), aby bylo možné vytvořit plně automatizovaného kanálu vytváří kampaně v Mobile Engagementu dynamicky na základě dat odesílaných v z back-end systémy.</span><span class="sxs-lookup"><span data-stu-id="4e93b-107">However as the marketing campaigns become mature, there is a need to leverage the data locked in the backend systems (like any CRM system or CMS system like SharePoint) so that a fully automated pipeline can be created which creates campaigns in Mobile Engagement dynamically based on the data flowing in from the backend systems.</span></span> 

![][5]

<span data-ttu-id="4e93b-108">V tomto kurzu projde scénář, kde uživatel obchodní SharePoint naplní seznam serveru SharePoint marketing daty a automatizovaného procesu převezme položek v seznamu a připojí se sadou Mobile Engagement použití dostupných rozhraní API REST k vytvoření marketingovou kampaň z dat služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="4e93b-108">This tutorial goes through such a scenario where a SharePoint business user populates a SharePoint list with marketing data and an automated process picks up items from the list and connects with the Mobile Engagement system using the available REST APIs to create a marketing campaign from the SharePoint data.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="4e93b-109">Obecně platí můžete tuto ukázku jako výchozí bod porozumět tomu, jak voláním jakéhokoli rozhraní API služby Mobile Engagement REST jako obsahuje podrobné informace o dva klíčové aspekty volání rozhraní API - ověřování a předejte parametry.</span><span class="sxs-lookup"><span data-stu-id="4e93b-109">In general, you can use this sample as a starting point for understanding how to call any Mobile Engagement REST API as it details the two key aspects of calling the APIs - authenticating and passing parameters.</span></span> 
> 
> 

## <a name="sharepoint-integration"></a><span data-ttu-id="4e93b-110">Integrace služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="4e93b-110">SharePoint integration</span></span>
1. <span data-ttu-id="4e93b-111">Zde je ukázka seznamu služby SharePoint, která bude vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="4e93b-111">Here is what the sample SharePoint list looks like.</span></span> <span data-ttu-id="4e93b-112">**Název**, **kategorie**, **NotificationTitle**, **zpráva** a **URL** jsou používány k vytvoření oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e93b-112">**Title**, **Category**, **NotificationTitle**, **Message** and **URL** are used for creating the announcement.</span></span> <span data-ttu-id="4e93b-113">Existuje sloupec s názvem **IsProcessed** který je používán procesem automatizace ukázka ve formě program konzoly.</span><span class="sxs-lookup"><span data-stu-id="4e93b-113">There is a column called **IsProcessed** which is used by the sample automation process in the form of a console program.</span></span> <span data-ttu-id="4e93b-114">Můžete buď spustit tato konzola programu jako webová úloha služby Azure tak, aby ji můžete naplánovat nebo můžete přímo pracovního postupu služby SharePoint k programu, vytvoření a aktivace oznámení, když vložíte položky do seznamu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="4e93b-114">You can either run this console program as an Azure WebJob so that you can schedule it or you can directly use the SharePoint workflow to program creating and activating the announcement when an item is inserted into the SharePoint list.</span></span> <span data-ttu-id="4e93b-115">V této ukázce používáme program konzoly, která přejde prostřednictvím položky v SharePoint seznam a vytvořit oznámení v Azure Mobile Engagementu pro každý z nich a nakonec označí **IsProcessed** příznak pravdivá na vytvoření úspěšné oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e93b-115">In this sample we use the console program which goes through the items in the SharePoint list and create announcement in Azure Mobile Engagement for each of them and then finally marks the **IsProcessed** flag to be true on successful announcement creation.</span></span>
   
    ![][1]
2. <span data-ttu-id="4e93b-116">Používáme kód z ukázky *vzdáleného ověřování ve službě SharePoint Online pomocí objektového modelu klienta* [sem](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) k ověření pomocí seznamu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="4e93b-116">We are using the code from the sample *Remote Authentication in SharePoint Online Using the Client Object Model* [here](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) to authenticate with the SharePoint list.</span></span>
3. <span data-ttu-id="4e93b-117">Po ověření, jsme projít položky seznamu a zjistěte, všechny nově vytvořené položky (které budou mít **IsProcessed** = false).</span><span class="sxs-lookup"><span data-stu-id="4e93b-117">Once authenticated, we loop through the list items to find out any newly created items (which will have **IsProcessed** = false).</span></span> 
   
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
   
                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
   
                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;
   
                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;
   
                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";
   
                            // Now try to activate the campaign also
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

## <a name="mobile-engagement-integration"></a><span data-ttu-id="4e93b-118">Integrace Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="4e93b-118">Mobile Engagement integration</span></span>
1. <span data-ttu-id="4e93b-119">Jakmile se nám najít položku, která vyžaduje zpracování - jsme extrahovat informace požadované pro vytvoření oznámení z položky seznamu a volání `CreateAzMECampaign` ji vytvořit a následně `ActivateAzMECampaign` aktivovat.</span><span class="sxs-lookup"><span data-stu-id="4e93b-119">Once we find an item which requires processing - we extract the information required to create an announcement from the list item and call `CreateAzMECampaign` to create it and subsequently `ActivateAzMECampaign` to activate it.</span></span> <span data-ttu-id="4e93b-120">Toto jsou v podstatě volání rozhraní REST API na back-end Mobile Engagementu volání.</span><span class="sxs-lookup"><span data-stu-id="4e93b-120">These are essentially REST API calls calling to the Mobile Engagement backend.</span></span> 
2. <span data-ttu-id="4e93b-121">Vyžadovat Mobile Engagement REST API **základní ověřování schématu autorizační HTTP hlavičky** který se skládá z `ApplicationId` a `ApiKey` který můžete získat z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e93b-121">The Mobile Engagement REST APIs require a **Basic auth scheme authorization HTTP header** which is composed of the `ApplicationId` and the `ApiKey` which you get from the Azure portal.</span></span> <span data-ttu-id="4e93b-122">Ujistěte se, že používáte klíč z **klíče rozhraní api** části a *není* z **sdk klíče** části.</span><span class="sxs-lookup"><span data-stu-id="4e93b-122">Make sure that you are using the Key from the **api keys** section and *not* from the **sdk keys** section.</span></span> 
   
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
3. <span data-ttu-id="4e93b-123">Pro vytvoření oznámení zadejte kampaň – odkazovat [dokumentaci](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e93b-123">For creating the announcement type campaign - refer to the [documentation](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span></span> <span data-ttu-id="4e93b-124">Musíte zajistit, že určíte kampaň `kind` jako *oznámení* a [datové části](https://msdn.microsoft.com/library/azure/mt683751.aspx) a předejte ji jako FormUrlEncodedContent.</span><span class="sxs-lookup"><span data-stu-id="4e93b-124">You need to make sure that you are specifying the campaign `kind` as *announcement* and the [payload](https://msdn.microsoft.com/library/azure/mt683751.aspx) and passing it as FormUrlEncodedContent.</span></span> 
   
        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";
   
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                // Create the payload to send the content
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
   
                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
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
4. <span data-ttu-id="4e93b-125">Jakmile máte oznámení vytvořen, zobrazí se něco jako následující na portál Mobile Engagement (Všimněte si, že stav = Koncept a aktivované = není k dispozici)</span><span class="sxs-lookup"><span data-stu-id="4e93b-125">Once you have the announcement created, you will see something like the following on the Mobile Engagement portal (note that the State=Draft and Activated = N/A)</span></span>
   
    ![][3]
5. <span data-ttu-id="4e93b-126">`CreateAzMECampaign`Vytvoří oznámení kampaně a vrátí jeho Id na volajícího.</span><span class="sxs-lookup"><span data-stu-id="4e93b-126">`CreateAzMECampaign` creates an announcement campaign and returns its Id to the caller.</span></span> <span data-ttu-id="4e93b-127">`ActivateAzMECampaign`Toto Id vyžaduje jako parametr k aktivaci kampaně.</span><span class="sxs-lookup"><span data-stu-id="4e93b-127">`ActivateAzMECampaign` requires this Id as the parameter to activate the campaign.</span></span> 
   
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
   
                // Send the POST request
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
6. <span data-ttu-id="4e93b-128">Až budete mít aktivovaný oznámení, zobrazí se něco jako následující na portál Mobile Engagement:</span><span class="sxs-lookup"><span data-stu-id="4e93b-128">Once you have the announcement activated, you will see something like the following on the Mobile Engagement portal:</span></span>
   
    ![][4]
7. <span data-ttu-id="4e93b-129">Po aktivaci kampaně získá jakékoli zařízení, které splňují kritérium na kampaň začne zobrazuje oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e93b-129">As soon as the campaign gets activated, any devices which satisfy the criterion on the campaign will start seeing notifications.</span></span> 
8. <span data-ttu-id="4e93b-130">Také si všimnete, že položka seznamu označené IsProcessed = false je nastavená na hodnotu True, po vytvoření kampaně oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e93b-130">You will also notice that the list item marked with IsProcessed = false has been set to True once the announcement campaign is created.</span></span>  

<span data-ttu-id="4e93b-131">Tato ukázka vytvořit jednoduché oznámení kampaň zadání nejčastěji požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4e93b-131">This sample created a simple announcement campaign specifying mostly the required properties.</span></span> <span data-ttu-id="4e93b-132">Tuto možnost můžete upravit tolik, kolik z portálu můžete pomocí informací o [zde](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e93b-132">You can customize this as much as you can from the portal by using the information [here](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span></span> 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



