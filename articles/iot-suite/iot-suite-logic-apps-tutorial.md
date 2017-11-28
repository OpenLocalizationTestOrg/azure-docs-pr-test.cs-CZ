---
title: aaaAzure IoT Suite a Logic Apps | Microsoft Docs
description: "Kurz týkající se jak toohook až Logic Apps tooAzure IoT Suite pro obchodní proces."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: corywink
ms.openlocfilehash: 6ef7311ac38f4e2ddb032cff0fb73591da5f76c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-connect-logic-app-tooyour-azure-iot-suite-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="04e0f-103">Kurz: Připojte aplikace logiky tooyour Azure IoT Suite vzdálené monitorování předkonfigurované řešení</span><span class="sxs-lookup"><span data-stu-id="04e0f-103">Tutorial: Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution</span></span>
<span data-ttu-id="04e0f-104">Hello [Microsoft Azure IoT Suite] [ lnk-internetofthings] předkonfigurovanému řešení vzdáleného monitorování je skvělým způsobem tooget rychle začít s sadu začátku do konce funkce, která exemplifies řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="04e0f-104">hello [Microsoft Azure IoT Suite][lnk-internetofthings] remote monitoring preconfigured solution is a great way tooget started quickly with an end-to-end feature set that exemplifies an IoT solution.</span></span> <span data-ttu-id="04e0f-105">Tento kurz vás provede jak tooadd aplikace logiky tooyour Microsoft Azure IoT Suite předkonfigurované řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="04e0f-105">This tutorial walks you through how tooadd Logic App tooyour Microsoft Azure IoT Suite remote monitoring preconfigured solution.</span></span> <span data-ttu-id="04e0f-106">Tyto kroky ukazují, jak může trvat i další řešení IoT propojením tooa obchodní proces.</span><span class="sxs-lookup"><span data-stu-id="04e0f-106">These steps demonstrate how you can take your IoT solution even further by connecting it tooa business process.</span></span>

<span data-ttu-id="04e0f-107">*Pokud hledáte návod na tom, jak tooprovision vzdálené monitorování předkonfigurované řešení, přečtěte si téma [kurz: Začínáme s řešeními IoT předkonfigurované hello][lnk-getstarted].*</span><span class="sxs-lookup"><span data-stu-id="04e0f-107">*If you’re looking for a walkthrough on how tooprovision a remote monitoring preconfigured solution, see [Tutorial: Get started with hello IoT preconfigured solutions][lnk-getstarted].*</span></span>

<span data-ttu-id="04e0f-108">Než začnete tento kurz, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="04e0f-108">Before you start this tutorial, you should:</span></span>

* <span data-ttu-id="04e0f-109">Zřízení hello vzdálené monitorování předkonfigurované řešení ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="04e0f-109">Provision hello remote monitoring preconfigured solution in your Azure subscription.</span></span>
* <span data-ttu-id="04e0f-110">Vytvoření tooenable účet sendgrid vám umožňuje toosend e-mailu, která aktivuje obchodní proces.</span><span class="sxs-lookup"><span data-stu-id="04e0f-110">Create a SendGrid account tooenable you toosend an email that triggers your business process.</span></span> <span data-ttu-id="04e0f-111">Můžete si zaregistrovat Bezplatný zkušební účet v [sendgrid vám umožňuje](https://sendgrid.com/) kliknutím **zkuste zdarma**.</span><span class="sxs-lookup"><span data-stu-id="04e0f-111">You can sign up for a free trial account at [SendGrid](https://sendgrid.com/) by clicking **Try for Free**.</span></span> <span data-ttu-id="04e0f-112">Po registraci pro bezplatný zkušební účet musíte toocreate [klíč rozhraní API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) v Sendgridu, která uděluje oprávnění toosend e-mailu.</span><span class="sxs-lookup"><span data-stu-id="04e0f-112">After you have registered for your free trial account, you need toocreate an [API key](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid that grants permissions toosend mail.</span></span> <span data-ttu-id="04e0f-113">Později v kurzu hello potřebujete tento klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="04e0f-113">You need this API key later in hello tutorial.</span></span>

<span data-ttu-id="04e0f-114">toocomplete tohoto kurzu budete potřebovat Visual Studio 2015 nebo Visual Studio 2017 akce hello toomodify v back-end hello předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="04e0f-114">toocomplete this tutorial, you need Visual Studio 2015 or Visual Studio 2017 toomodify hello actions in hello preconfigured solution back end.</span></span>

<span data-ttu-id="04e0f-115">Za předpokladu, že jste už zřízené vzdáleného monitorování předkonfigurované řešení, přejděte toohello skupinu prostředků pro toto řešení v hello [portál Azure][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="04e0f-115">Assuming you’ve already provisioned your remote monitoring preconfigured solution, navigate toohello resource group for that solution in hello [Azure portal][lnk-azureportal].</span></span> <span data-ttu-id="04e0f-116">Skupina prostředků Hello má hello stejný název jako hello řešení, kterou si zvolili při zřízení řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="04e0f-116">hello resource group has hello same name as hello solution name you chose when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="04e0f-117">Ve skupině prostředků hello zobrazí se všechny hello zřízení prostředků Azure pro vaše řešení s výjimkou hello aplikaci Azure Active Directory, které můžete najít v hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="04e0f-117">In hello resource group, you can see all hello provisioned Azure resources for your solution except for hello Azure Active Directory application that you can find in hello Azure Classic Portal.</span></span> <span data-ttu-id="04e0f-118">Hello následující snímek obrazovky ukazuje příklad **skupiny prostředků** okno pro vzdálené monitorování předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="04e0f-118">hello following screenshot shows an example **Resource group** blade for a remote monitoring preconfigured solution:</span></span>

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

<span data-ttu-id="04e0f-119">toobegin, nastavte hello logiku aplikace toouse s hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="04e0f-119">toobegin, set up hello logic app toouse with hello preconfigured solution.</span></span>

## <a name="set-up-hello-logic-app"></a><span data-ttu-id="04e0f-120">Nastavit hello aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="04e0f-120">Set up hello Logic App</span></span>
1. <span data-ttu-id="04e0f-121">Klikněte na tlačítko **přidat** hello horní části vaší okně skupiny prostředků v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="04e0f-121">Click **Add** at hello top of your resource group blade in hello Azure portal.</span></span>
2. <span data-ttu-id="04e0f-122">Vyhledejte **aplikace logiky**, vyberte ho a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04e0f-122">Search for **Logic App**, select it and then click **Create**.</span></span>
3. <span data-ttu-id="04e0f-123">Vyplňte hello **název** a použití hello stejné **předplatné** a **skupiny prostředků** jste použili při zřizování řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="04e0f-123">Fill out hello **Name** and use hello same **Subscription** and **Resource group** that you used when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="04e0f-124">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04e0f-124">Click **Create**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. <span data-ttu-id="04e0f-125">Po dokončení nasazení uvidíte jako prostředek hello aplikace logiky, která je uvedena ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="04e0f-125">When your deployment completes, you can see hello Logic App is listed as a resource in your resource group.</span></span>
5. <span data-ttu-id="04e0f-126">Klikněte na tlačítko hello aplikace logiky toonavigate toohello aplikace logiky okně vyberte hello **prázdné aplikace logiky** šablony tooopen hello **logiku aplikace Návrhář**.</span><span class="sxs-lookup"><span data-stu-id="04e0f-126">Click hello Logic App toonavigate toohello Logic App blade, select hello **Blank Logic App** template tooopen hello **Logic Apps Designer**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. <span data-ttu-id="04e0f-127">Vyberte **požadavku**.</span><span class="sxs-lookup"><span data-stu-id="04e0f-127">Select **Request**.</span></span> <span data-ttu-id="04e0f-128">Tuto akci Určuje, že příchozí požadavek HTTP s konkrétní JSON formátu aktivační událost jednání datové části.</span><span class="sxs-lookup"><span data-stu-id="04e0f-128">This action specifies that an incoming HTTP request with a specific JSON formatted payload acts as a trigger.</span></span>
7. <span data-ttu-id="04e0f-129">Vložte následující kód do hello požadavku schématu JSON textu hello:</span><span class="sxs-lookup"><span data-stu-id="04e0f-129">Paste hello following code into hello Request Body JSON Schema:</span></span>
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > <span data-ttu-id="04e0f-130">Po uložení aplikace logiky hello, ale nejdřív je nutné přidat akci můžete zkopírovat hello URL pro hello HTTP post.</span><span class="sxs-lookup"><span data-stu-id="04e0f-130">You can copy hello URL for hello HTTP post after you save hello logic app, but first you must add an action.</span></span>
   > 
   > 
8. <span data-ttu-id="04e0f-131">Klikněte na tlačítko **+ nový krok** pod ruční aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="04e0f-131">Click **+ New step** under your manual trigger.</span></span> <span data-ttu-id="04e0f-132">Pak klikněte na tlačítko **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="04e0f-132">Then click **Add an action**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. <span data-ttu-id="04e0f-133">Vyhledejte **sendgrid vám umožňuje - odesílání e-mailu** a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="04e0f-133">Search for **SendGrid - Send email** and click it.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. <span data-ttu-id="04e0f-134">Zadejte název hello připojení, jako například **SendGridConnection**, zadejte hello **klíč rozhraní API sendgrid vám umožňuje** jste vytvořili při nastavení účtu Sendgridu a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04e0f-134">Enter a name for hello connection, such as **SendGridConnection**, enter hello **SendGrid API Key** you created when you set up your SendGrid account, and click **Create**.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. <span data-ttu-id="04e0f-135">Přidání e-mailových adres, vlastní tooboth hello **z** a **k** pole.</span><span class="sxs-lookup"><span data-stu-id="04e0f-135">Add email addresses you own tooboth hello **From** and **To** fields.</span></span> <span data-ttu-id="04e0f-136">Přidat **vzdáleného sledování upozornění [DeviceId]** toohello **subjektu** pole.</span><span class="sxs-lookup"><span data-stu-id="04e0f-136">Add **Remote monitoring alert [DeviceId]** toohello **Subject** field.</span></span> <span data-ttu-id="04e0f-137">V hello **obsahu e-mailu** pole, přidejte **zařízení [DeviceId] ohlásil [measurementName] s hodnotou [measuredValue].**</span><span class="sxs-lookup"><span data-stu-id="04e0f-137">In hello **Email Body** field, add **Device [DeviceId] has reported [measurementName] with value [measuredValue].**</span></span> <span data-ttu-id="04e0f-138">Můžete přidat **[DeviceId]**, **[measurementName]**, a **[measuredValue]** kliknutím v hello **můžete vložit data z předchozích kroků**části.</span><span class="sxs-lookup"><span data-stu-id="04e0f-138">You can add **[DeviceId]**, **[measurementName]**, and **[measuredValue]** by clicking in hello **You can insert data from previous steps** section.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. <span data-ttu-id="04e0f-139">Klikněte na tlačítko **Uložit** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="04e0f-139">Click **Save** in hello top menu.</span></span>
13. <span data-ttu-id="04e0f-140">Klikněte na tlačítko hello **požadavku** aktivační události a zkopírujte hello **Http Post toothis URL** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="04e0f-140">Click hello **Request** trigger and copy hello **Http Post toothis URL** value.</span></span> <span data-ttu-id="04e0f-141">Později v tomto kurzu musíte tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="04e0f-141">You need this URL later in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="04e0f-142">Logic Apps umožňují toorun [mnoho různých typů akce] [ lnk-logic-apps-actions] včetně akce v Office 365.</span><span class="sxs-lookup"><span data-stu-id="04e0f-142">Logic Apps enable you toorun [many different types of action][lnk-logic-apps-actions] including actions in Office 365.</span></span> 
> 
> 

## <a name="set-up-hello-eventprocessor-web-job"></a><span data-ttu-id="04e0f-143">Nastavit hello EventProcessor webové úlohy</span><span class="sxs-lookup"><span data-stu-id="04e0f-143">Set up hello EventProcessor Web Job</span></span>
<span data-ttu-id="04e0f-144">V této části je připojit vaše toohello předkonfigurované řešení aplikace logiky, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="04e0f-144">In this section, you connect your preconfigured solution toohello Logic App you created.</span></span> <span data-ttu-id="04e0f-145">toocomplete této úlohy přidejte hello URL tootrigger hello aplikace logiky toohello akci, která aktivuje se v případě hodnotu senzor zařízení překračuje prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="04e0f-145">toocomplete this task, you add hello URL tootrigger hello Logic App toohello action that fires when a device sensor value exceeds a threshold.</span></span>

1. <span data-ttu-id="04e0f-146">Použijte váš git tooclone hello nejnovější verze klienta hello [azure-iot-remote-monitoring úložiště github][lnk-rmgithub].</span><span class="sxs-lookup"><span data-stu-id="04e0f-146">Use your git client tooclone hello latest version of hello [azure-iot-remote-monitoring github repository][lnk-rmgithub].</span></span> <span data-ttu-id="04e0f-147">Například:</span><span class="sxs-lookup"><span data-stu-id="04e0f-147">For example:</span></span>
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. <span data-ttu-id="04e0f-148">V sadě Visual Studio otevřete hello **RemoteMonitoring.sln** z místní kopie hello hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="04e0f-148">In Visual Studio, open hello **RemoteMonitoring.sln** from hello local copy of hello repository.</span></span>
3. <span data-ttu-id="04e0f-149">Otevřete hello **ActionRepository.cs** souboru v hello **infrastruktury\\úložiště** složky.</span><span class="sxs-lookup"><span data-stu-id="04e0f-149">Open hello **ActionRepository.cs** file in hello **Infrastructure\\Repository** folder.</span></span>
4. <span data-ttu-id="04e0f-150">Aktualizace hello **actionIds** slovník s hello **Http Post toothis URL** jste si poznamenali u aplikace logiky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="04e0f-150">Update hello **actionIds** dictionary with hello **Http Post toothis URL** you noted from your Logic App as follows:</span></span>
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post toothis URL>" },
        { "Raise Alarm", "<Http Post toothis URL>" }
    };
    ```
5. <span data-ttu-id="04e0f-151">Uložení změn hello v řešení a ukončení Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04e0f-151">Save hello changes in solution and exit Visual Studio.</span></span>

## <a name="deploy-from-hello-command-line"></a><span data-ttu-id="04e0f-152">Nasazení z příkazového řádku hello</span><span class="sxs-lookup"><span data-stu-id="04e0f-152">Deploy from hello command line</span></span>
<span data-ttu-id="04e0f-153">V této části nasadíte vaší aktualizovanou verzi hello vzdálené monitorování řešení tooreplace hello verzi aktuálně spuštěných v Azure.</span><span class="sxs-lookup"><span data-stu-id="04e0f-153">In this section, you deploy your updated version of hello remote monitoring solution tooreplace hello version currently running in Azure.</span></span>

1. <span data-ttu-id="04e0f-154">Následující hello [dev nastavení] [ lnk-devsetup] tooset pokyny prostředí pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="04e0f-154">Following hello [dev set-up][lnk-devsetup] instructions tooset up your environment for deployment.</span></span>
2. <span data-ttu-id="04e0f-155">toodeploy místně, postupujte podle hello [místní nasazení] [ lnk-localdeploy] pokyny.</span><span class="sxs-lookup"><span data-stu-id="04e0f-155">toodeploy locally, follow hello [local deployment][lnk-localdeploy] instructions.</span></span>
3. <span data-ttu-id="04e0f-156">toodeploy toohello cloudu a aktualizovat vaše stávající nasazení cloudu, postupujte podle hello [cloudové nasazení] [ lnk-clouddeploy] pokyny.</span><span class="sxs-lookup"><span data-stu-id="04e0f-156">toodeploy toohello cloud and update your existing cloud deployment, follow hello [cloud deployment][lnk-clouddeploy] instructions.</span></span> <span data-ttu-id="04e0f-157">Použijte název hello původní nasazení jako název nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="04e0f-157">Use hello name of your original deployment as hello deployment name.</span></span> <span data-ttu-id="04e0f-158">Například pokud byla volána původní nasazení hello **demologicapp**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="04e0f-158">For example if hello original deployment was called **demologicapp**, use hello following command:</span></span>
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   <span data-ttu-id="04e0f-159">Když hello vytvořit skript se spustí, ujistěte se, toouse hello stejný účet Azure, předplatné, oblast a instance služby Active Directory, které jste použili při zřizování řešení hello.</span><span class="sxs-lookup"><span data-stu-id="04e0f-159">When hello build script runs, be sure toouse hello same Azure account, subscription, region, and Active Directory instance you used when you provisioned hello solution.</span></span>

## <a name="see-your-logic-app-in-action"></a><span data-ttu-id="04e0f-160">V tématu aplikace logiky v akci</span><span class="sxs-lookup"><span data-stu-id="04e0f-160">See your Logic App in action</span></span>
<span data-ttu-id="04e0f-161">Hello předkonfigurovanému řešení vzdáleného monitorování má dvě pravidla ve výchozím nastavení při zřizování řešení.</span><span class="sxs-lookup"><span data-stu-id="04e0f-161">hello remote monitoring preconfigured solution has two rules set up by default when you provision a solution.</span></span> <span data-ttu-id="04e0f-162">Obě pravidla jsou na hello **SampleDevice001** zařízení:</span><span class="sxs-lookup"><span data-stu-id="04e0f-162">Both rules are on hello **SampleDevice001** device:</span></span>

* <span data-ttu-id="04e0f-163">Teplotní > 38.00</span><span class="sxs-lookup"><span data-stu-id="04e0f-163">Temperature > 38.00</span></span>
* <span data-ttu-id="04e0f-164">Vlhkosti > 48.00</span><span class="sxs-lookup"><span data-stu-id="04e0f-164">Humidity > 48.00</span></span>

<span data-ttu-id="04e0f-165">pravidlo teploty Hello aktivuje hello **vyvolat alarmů** akce a hello vlhkosti pravidlo aktivuje hello **SendMessage** akce.</span><span class="sxs-lookup"><span data-stu-id="04e0f-165">hello temperature rule triggers hello **Raise Alarm** action and hello Humidity rule triggers hello **SendMessage** action.</span></span> <span data-ttu-id="04e0f-166">Za předpokladu, že jste použili hello stejnou adresu URL pro obě akce hello **ActionRepository** třídy, vaše aplikace logiky aktivačních událostí pro buď pravidlo.</span><span class="sxs-lookup"><span data-stu-id="04e0f-166">Assuming you used hello same URL for both actions hello **ActionRepository** class, your logic app triggers for either rule.</span></span> <span data-ttu-id="04e0f-167">Obě pravidla použít toosend sendgrid vám umožňuje e-mailu toohello **k** adresu s podrobnostmi hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="04e0f-167">Both rules use SendGrid toosend an email toohello **To** address with details of hello alert.</span></span>

> [!NOTE]
> <span data-ttu-id="04e0f-168">Hello aplikace logiky pokračuje tootrigger pokaždé, když se dosáhla prahová hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="04e0f-168">hello Logic App continues tootrigger every time hello threshold is met.</span></span> <span data-ttu-id="04e0f-169">nepotřebné e-mailů tooavoid, můžete zakázat hello pravidla ve vašem řešení portálu nebo zakázat hello aplikace logiky v hello [portál Azure][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="04e0f-169">tooavoid unnecessary emails, you can either disable hello rules in your solution portal or disable hello Logic App in hello [Azure portal][lnk-azureportal].</span></span>
> 
> 

<span data-ttu-id="04e0f-170">Kromě toho tooreceiving e-mailů, můžete také zjistit spuštění hello aplikace logiky hello portálu:</span><span class="sxs-lookup"><span data-stu-id="04e0f-170">In addition tooreceiving emails, you can also see when hello Logic App runs in hello portal:</span></span>

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a><span data-ttu-id="04e0f-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04e0f-171">Next steps</span></span>
<span data-ttu-id="04e0f-172">Teď, když jste použili aplikaci logiky tooconnect hello předkonfigurované řešení tooa obchodní proces, můžete další informace o hello možností pro přizpůsobení hello předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="04e0f-172">Now that you've used a Logic App tooconnect hello preconfigured solution tooa business process, you can learn more about hello options for customizing hello preconfigured solutions:</span></span>

* <span data-ttu-id="04e0f-173">[Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="04e0f-173">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="04e0f-174">[Informace metadat zařízení v hello předkonfigurovanému řešení vzdáleného monitorování][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="04e0f-174">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo]</span></span>

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
