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
# <a name="azure-mobile-engagement---api-integration"></a>Azure Mobile Engagement – integrace rozhraní API
V automatizovaný systém marketing vytvoření a aktivace hello marketingových kampaní také dojít k automaticky. Pro tento účel - Azure Mobile Engagement umožňuje vytváření takové automatizované marketingových kampaní, které jsou také pomocí rozhraní API. 

Obvykle zákazníci používat jako součást svých marketingových kampaní hello Mobile Engagement front-endové rozhraní toocreate oznámení nebo hlasování atd. Ale jako hello marketingových kampaní stát vyspělá, je třeba tooleverage hello data zamknutý v hello back-end systémy (například systému CRM nebo systém CMS jako SharePoint), aby bylo možné vytvořit plně automatizovaného kanálu vytváří kampaně v Mobile Zapojení dynamicky podle hello dat odesílaných v z back-end systémy hello. 

![][5]

Tento kurz vloží prostřednictvím scénáři, kde uživatel obchodní SharePoint naplní seznam serveru SharePoint s marketingové dat a automatizovaného procesu převezme položek seznamu hello a připojí hello Mobile Engagement systém pomocí hello k dispozici rozhraní REST API toocreate marketingovou kampaň z hello dat služby SharePoint. 

> [!IMPORTANT]
> Obecně platí můžete tato ukázka jako výchozí bod pro pochopení, jak toocall žádné Mobile Engagement REST API jako jeho podrobnosti hello dva klíčové aspekty volání hello rozhraní API - ověřování a předejte parametry. 
> 
> 

## <a name="sharepoint-integration"></a>Integrace služby SharePoint
1. Zde je ukázka jaké hello vypadá seznamu služby SharePoint. **Název**, **kategorie**, **NotificationTitle**, **zpráva** a **URL** jsou používány k vytvoření hello oznámení. Existuje sloupec s názvem **IsProcessed** který je používán procesem automatizace ukázka hello hello tvar program konzoly. Tento program konzoly můžete buď spustit jako webová úloha Azure tak, aby ji můžete naplánovat nebo přímo pomocí hello SharePoint pracovního postupu tooprogram vytvoření a aktivace hello oznámení při vložení položky do seznamu služby SharePoint hello. V této ukázce používáme hello program konzoly, která přejde prostřednictvím položky hello v hello SharePoint seznam a vytvořit oznámení v Azure Mobile Engagementu pro každý z nich a nakonec označí hello **IsProcessed** příznak toobe true na vytvoření úspěšné oznámení.
   
    ![][1]
2. Používáme hello kód z ukázkové hello *vzdáleného ověřování v hello SharePoint Online pomocí objektového modelu klienta* [sem](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate s hello seznamu služby SharePoint.
3. Po ověření, jsme projít toofind položky seznamu hello na všech nově vytvořených položek (které budou mít **IsProcessed** = false). 
   
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

## <a name="mobile-engagement-integration"></a>Integrace Mobile Engagement
1. Jakmile se nám najít položku, která vyžaduje zpracování – jsme extrahovat informace hello požadované toocreate oznámení z hello položky seznamu a volání `CreateAzMECampaign` toocreate jej a následně `ActivateAzMECampaign` tooactivate ho. Toto jsou v podstatě volání rozhraní REST API volání toohello Mobile Engagement back-end. 
2. vyžadovat, Hello Mobile Engagement REST API **základní ověřování schématu autorizační HTTP hlavičky** který se skládá z hello `ApplicationId` a hello `ApiKey` který můžete získat z hello portálu Azure. Ujistěte se, že používáte hello klíč z hello **klíče rozhraní api** části a *není* z hello **sdk klíče** části. 
   
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
3. Pro vytvoření typ oznámení hello kampaně – odkazovat toohello [dokumentaci](https://msdn.microsoft.com/library/azure/mt683750.aspx). Je nutné se, že určíte hello kampaň toomake `kind` jako *oznámení* a hello [datové části](https://msdn.microsoft.com/library/azure/mt683751.aspx) a předejte ji jako FormUrlEncodedContent. 
   
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
4. Jakmile máte hello oznámení vytvořen, zobrazí se něco podobného jako hello následující na portál Mobile Engagement hello (Všimněte si, že hello stav = Koncept a aktivované = není k dispozici)
   
    ![][3]
5. `CreateAzMECampaign`Vytvoří oznámení kampaně a vrátí jeho Id toohello volajícího. `ActivateAzMECampaign`Toto Id vyžaduje jako hello parametr tooactivate hello kampaně. 
   
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
6. Až budete mít hello oznámení aktivovat, zobrazí se něco podobného jako následující hello na portál Mobile Engagement hello:
   
    ![][4]
7. Při aktivaci kampaně hello získá jakékoli zařízení, které splňují kritéria hello na kampaň hello začne zobrazuje oznámení. 
8. Také si všimnete, že položka seznamu hello označena s IsProcessed = false je nastavená tooTrue po vytvoření hello oznámení kampaně.  

Tato ukázka vytvořit jednoduché oznámení kampaň zadání většinou hello požadované vlastnosti. Tuto možnost můžete upravit z portálu hello co nejvíce, pomocí informací o hello [zde](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



