---
title: "aaaRetrain Classic webové služby | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically přeučování modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: d3ba21ed75f02868535cb2fcac607643303a9554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-classic-web-service"></a>Přeučování webové služby Classic
Hello prediktivní webové služby, které jste nasadili je výchozí hello vyhodnocovací koncový bod. Výchozí koncové body jsou synchronizovány s hello původní školení a vyhodnocování experimenty, a proto hello trained model pro výchozí koncový bod hello nejde nahradit. tooretrain hello webové služby, je nutné přidat novou webovou službu toohello koncový bod. 

## <a name="prerequisites"></a>Požadavky
Musí mít nastavíte výukový experiment a prediktivní experiment jak je znázorněno v [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md). 

> [!IMPORTANT]
> Prediktivní experiment Hello musí být nasazený jako Classic strojového učení webové služby. 
> 
> 

Další informace o nasazení webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Přidat nový koncový bod
Hello prediktivní webové služby, který jste nasadili obsahuje výchozí vyhodnocovací koncový bod, který je uložen synchronizována s hello původní školení a vyhodnocování experimenty trained model. tooupdate vaše toowith nového modulu trained model služby web musíte vytvořit nový koncový bod vyhodnocování. 

toocreate nový vyhodnocovací koncový bod, na hello prediktivní webová služba, která mohou být aktualizovány s hello trained model:

> [!NOTE]
> Ujistěte se, že přidáváte hello koncový bod toohello prediktivní webové služby, hello školení webové služby. Pokud jste nasadili správně školení a prediktivní webové služby, měli byste vidět dvě samostatné webové služby, které jsou uvedené. Hello prediktivní webové služby musí končit "[prediktivní exp.]".
> 
> 

Existují tři způsoby, ve které můžete přidat novou webovou službu tooa koncového bodu:

1. Prostřednictvím kódu programu
2. Pomocí portálu služby Microsoft Azure Web hello
3. Hello použijte portál Azure classic

### <a name="programmatically-add-an-endpoint"></a>Přidání koncového bodu prostřednictvím kódu programu
Můžete přidat vyhodnocování koncové body pomocí hello ukázkový kód zadaný v tomto [úložiště github](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).

### <a name="use-hello-microsoft-azure-web-services-portal-tooadd-an-endpoint"></a>Použití portálu Microsoft Azure Web Services hello – tooadd koncový bod
1. Machine Learning Studio v levém navigačním sloupec hello, klikněte na možnost webové služby.
2. V dolní části hello řídicího panelu, hello webové služby, klikněte na tlačítko **spravovat koncové body preview**.
3. Klikněte na tlačítko **Přidat**.
4. Zadejte název a popis pro nový koncový bod hello. Vyberte úroveň protokolování hello a určuje, jestli je povolená ukázková data. Další informace o protokolování naleznete v tématu [povolení protokolování pro webové služby Machine Learning](machine-learning-web-services-logging.md).

### <a name="use-hello-azure-classic-portal-tooadd-an-endpoint"></a>Použití hello Azure classic portálu tooadd koncový bod
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levé nabídce hello, klikněte na **Machine Learning**.
3. Pod názvem, klikněte na pracovní prostor a potom klikněte na **webové služby**.
4. Pod názvem, klikněte na **modelu sčítání [prediktivní exp.]** .
5. V dolní části hello hello stránky, klikněte na tlačítko **přidat koncový bod**. Další informace o přidání koncové body, najdete v části [vytváření koncových bodů](machine-learning-create-endpoint.md). 

## <a name="update-hello-added-endpoints-trained-model"></a>Aktualizace hello přidat Trained Model pro koncový bod
toocomplete hello retraining procesu, je nutné aktualizovat hello trained model nový koncový bod hello, kterou jste přidali.

* Pokud jste přidali hello nový koncový bod pomocí portálu Azure classic hello, můžete kliknutím na hello nový název koncového bodu na portálu hello a pak hello **operace UpdateResource** odkaz tooget hello – adresa URL potřebovali byste tooupdate hello endpoint modelu.
* Pokud jste přidali hello koncového bodu pomocí hello ukázkový kód, jedná se o umístění adresa URL nápovědy hello identifikovaný hello *HelpLocationURL* hodnotu ve výstupu hello.

tooretrieve hello cestu adresy URL:

1. Zkopírujte a vložte adresu URL hello do prohlížeče.
2. Kliknutím na odkaz aktualizace prostředků hello.
3. Zkopírujte hello POST URL požadavku PATCH hello. Například:
   
     ADRESA URL OPRAVY: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2

Teď můžete použít hello trained model tooupdate hello vyhodnocovací koncový bod, který jste vytvořili dříve.

Hello následující vzorový kód ukazuje, jak toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*a oprava URL tooupdate hello koncového bodu.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from hello output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from hello output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

Hello *apiKey* a hello *endpointUrl* pro volání hello můžete získat z řídicího panelu koncový bod.

Hello hodnotu hello *název* parametr v *prostředky* mají shodu hello název prostředků hello uloženy Trained Model prediktivní experiment hello. tooget hello název prostředku:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levé nabídce hello, klikněte na **Machine Learning**.
3. Pod názvem, klikněte na pracovní prostor a potom klikněte na **webové služby**.
4. Pod názvem, klikněte na **modelu sčítání [prediktivní exp.]** .
5. Klikněte na tlačítko Nový koncový bod hello, které jste přidali.
6. Na řídicím panelu endpoint hello, klikněte na tlačítko **aktualizace prostředků**.
7. Na stránce dokumentace k API prostředku aktualizace hello hello webové služby, můžete najít hello **název prostředku** pod **aktualizovat prostředky**.

Pokud před dokončíte, aktualizuje se koncový bod hello vyprší platnost vašeho tokenu SAS, je třeba provést GET s Id úlohy tooobtain hello nový token.

Při hello kód byl úspěšně spuštěn, začněte hello nový koncový bod pomocí modelu hello retrained přibližně 30 sekund.

## <a name="summary"></a>Souhrn
Pomocí hello Retraining API, můžete aktualizovat hello trained model prediktivní webové služby, například povolení scénáře:

* Pravidelné model retraining s nová data.
* Distribuce toocustomers modelu s cílem hello aby přeučování hello model pomocí svá vlastní data.

## <a name="next-steps"></a>Další kroky
[Řešení potíží s hello retraining classic webové služby Azure Machine Learning](machine-learning-troubleshooting-retraining-models.md)

