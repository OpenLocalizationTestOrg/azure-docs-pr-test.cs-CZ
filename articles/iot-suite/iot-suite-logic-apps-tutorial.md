---
title: "Azure IoT Suite a aplikacích logiky | Microsoft Docs"
description: "Kurz o tom, jak spojit Logic Apps, abyste Azure IoT Suite pro obchodní proces."
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
ms.openlocfilehash: 2e7997e2a8bdeeec6083d40acb55e653f87e140b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="4c1af-103">Kurz: Připojení k Azure IoT Suite vzdálené monitorování předkonfigurované řešení aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="4c1af-103">Tutorial: Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution</span></span>
<span data-ttu-id="4c1af-104">[Microsoft Azure IoT Suite] [ lnk-internetofthings] předkonfigurovanému řešení vzdáleného monitorování je skvělým způsobem, jak rychle začít používat sadu začátku do konce funkce, která exemplifies řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="4c1af-104">The [Microsoft Azure IoT Suite][lnk-internetofthings] remote monitoring preconfigured solution is a great way to get started quickly with an end-to-end feature set that exemplifies an IoT solution.</span></span> <span data-ttu-id="4c1af-105">Tento kurz vás provede procesem přidání aplikace logiky do vaší Microsoft Azure IoT Suite předkonfigurovanému řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="4c1af-105">This tutorial walks you through how to add Logic App to your Microsoft Azure IoT Suite remote monitoring preconfigured solution.</span></span> <span data-ttu-id="4c1af-106">Tyto kroky ukazují, jak může trvat i další řešení IoT připojením k obchodní proces.</span><span class="sxs-lookup"><span data-stu-id="4c1af-106">These steps demonstrate how you can take your IoT solution even further by connecting it to a business process.</span></span>

<span data-ttu-id="4c1af-107">*Pokud hledáte návod o tom, jak zřídit předkonfigurované řešení vzdáleného monitorování, přečtěte si téma [kurz: Začínáme s přednastavenými řešeními IoT][lnk-getstarted].*</span><span class="sxs-lookup"><span data-stu-id="4c1af-107">*If you’re looking for a walkthrough on how to provision a remote monitoring preconfigured solution, see [Tutorial: Get started with the IoT preconfigured solutions][lnk-getstarted].*</span></span>

<span data-ttu-id="4c1af-108">Než začnete tento kurz, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4c1af-108">Before you start this tutorial, you should:</span></span>

* <span data-ttu-id="4c1af-109">Zřídit předkonfigurovaného řešení vzdáleného monitorování ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="4c1af-109">Provision the remote monitoring preconfigured solution in your Azure subscription.</span></span>
* <span data-ttu-id="4c1af-110">Vytvořte účet sendgrid vám umožňuje povolit odesílání e-mailu, která aktivuje obchodní proces.</span><span class="sxs-lookup"><span data-stu-id="4c1af-110">Create a SendGrid account to enable you to send an email that triggers your business process.</span></span> <span data-ttu-id="4c1af-111">Můžete si zaregistrovat Bezplatný zkušební účet v [sendgrid vám umožňuje](https://sendgrid.com/) kliknutím **zkuste zdarma**.</span><span class="sxs-lookup"><span data-stu-id="4c1af-111">You can sign up for a free trial account at [SendGrid](https://sendgrid.com/) by clicking **Try for Free**.</span></span> <span data-ttu-id="4c1af-112">Po registraci pro bezplatný zkušební účet je potřeba vytvořit [klíč rozhraní API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) v Sendgridu, která uděluje oprávnění k odesílání pošty.</span><span class="sxs-lookup"><span data-stu-id="4c1af-112">After you have registered for your free trial account, you need to create an [API key](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid that grants permissions to send mail.</span></span> <span data-ttu-id="4c1af-113">Později v tomto kurzu potřebujete tento klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4c1af-113">You need this API key later in the tutorial.</span></span>

<span data-ttu-id="4c1af-114">K dokončení tohoto kurzu, musíte Visual Studio 2015 nebo Visual Studio 2017 chtěli změnit nastavení akcí v back-end předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="4c1af-114">To complete this tutorial, you need Visual Studio 2015 or Visual Studio 2017 to modify the actions in the preconfigured solution back end.</span></span>

<span data-ttu-id="4c1af-115">Za předpokladu, že jste už zřízené vzdáleného monitorování předkonfigurované řešení, přejděte do skupiny prostředků pro toto řešení v [portál Azure][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="4c1af-115">Assuming you’ve already provisioned your remote monitoring preconfigured solution, navigate to the resource group for that solution in the [Azure portal][lnk-azureportal].</span></span> <span data-ttu-id="4c1af-116">Skupina prostředků má stejný název jako název řešení jste si zvolili při zřízení řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="4c1af-116">The resource group has the same name as the solution name you chose when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="4c1af-117">Ve skupině prostředků uvidíte všechny zřízené prostředky Azure pro vaše řešení s výjimkou aplikace Azure Active Directory, které můžete najít na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="4c1af-117">In the resource group, you can see all the provisioned Azure resources for your solution except for the Azure Active Directory application that you can find in the Azure Classic Portal.</span></span> <span data-ttu-id="4c1af-118">Následující snímek obrazovky ukazuje příklad **skupiny prostředků** okno pro vzdálené monitorování předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="4c1af-118">The following screenshot shows an example **Resource group** blade for a remote monitoring preconfigured solution:</span></span>

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

<span data-ttu-id="4c1af-119">Pokud chcete začít, nastavte pro použití s předkonfigurované řešení aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="4c1af-119">To begin, set up the logic app to use with the preconfigured solution.</span></span>

## <a name="set-up-the-logic-app"></a><span data-ttu-id="4c1af-120">Nastavit aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="4c1af-120">Set up the Logic App</span></span>
1. <span data-ttu-id="4c1af-121">Klikněte na tlačítko **přidat** v horní části vaší okně skupiny prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4c1af-121">Click **Add** at the top of your resource group blade in the Azure portal.</span></span>
2. <span data-ttu-id="4c1af-122">Vyhledejte **aplikace logiky**, vyberte ho a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4c1af-122">Search for **Logic App**, select it and then click **Create**.</span></span>
3. <span data-ttu-id="4c1af-123">Vyplňte **název** a používat stejné **předplatné** a **skupiny prostředků** jste použili při zřizování řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="4c1af-123">Fill out the **Name** and use the same **Subscription** and **Resource group** that you used when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="4c1af-124">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4c1af-124">Click **Create**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. <span data-ttu-id="4c1af-125">Po dokončení nasazení uvidíte, že aplikace logiky je uveden jako prostředků ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="4c1af-125">When your deployment completes, you can see the Logic App is listed as a resource in your resource group.</span></span>
5. <span data-ttu-id="4c1af-126">Klikněte na aplikaci logiky a přejděte do okna aplikace logiky, vyberte **prázdné aplikace logiky** šablony otevřete **logiku aplikace Návrhář**.</span><span class="sxs-lookup"><span data-stu-id="4c1af-126">Click the Logic App to navigate to the Logic App blade, select the **Blank Logic App** template to open the **Logic Apps Designer**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. <span data-ttu-id="4c1af-127">Vyberte **požadavku**.</span><span class="sxs-lookup"><span data-stu-id="4c1af-127">Select **Request**.</span></span> <span data-ttu-id="4c1af-128">Tuto akci Určuje, že příchozí požadavek HTTP s konkrétní JSON formátu aktivační událost jednání datové části.</span><span class="sxs-lookup"><span data-stu-id="4c1af-128">This action specifies that an incoming HTTP request with a specific JSON formatted payload acts as a trigger.</span></span>
7. <span data-ttu-id="4c1af-129">Do schématu JSON textu žádosti vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="4c1af-129">Paste the following code into the Request Body JSON Schema:</span></span>
   
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
   > <span data-ttu-id="4c1af-130">Po uložení aplikaci logiky, ale nejdřív je nutné přidat akci, můžete zkopírovat adresu URL pro metodu post protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="4c1af-130">You can copy the URL for the HTTP post after you save the logic app, but first you must add an action.</span></span>
   > 
   > 
8. <span data-ttu-id="4c1af-131">Klikněte na tlačítko **+ nový krok** pod ruční aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="4c1af-131">Click **+ New step** under your manual trigger.</span></span> <span data-ttu-id="4c1af-132">Pak klikněte na tlačítko **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="4c1af-132">Then click **Add an action**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. <span data-ttu-id="4c1af-133">Vyhledejte **sendgrid vám umožňuje - odesílání e-mailu** a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="4c1af-133">Search for **SendGrid - Send email** and click it.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. <span data-ttu-id="4c1af-134">Zadejte název připojení, jako například **SendGridConnection**, zadejte **klíč rozhraní API sendgrid vám umožňuje** jste vytvořili při nastavení účtu Sendgridu a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4c1af-134">Enter a name for the connection, such as **SendGridConnection**, enter the **SendGrid API Key** you created when you set up your SendGrid account, and click **Create**.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. <span data-ttu-id="4c1af-135">Přidání e-mailové adresy, které vlastníte na obojí **z** a **k** pole.</span><span class="sxs-lookup"><span data-stu-id="4c1af-135">Add email addresses you own to both the **From** and **To** fields.</span></span> <span data-ttu-id="4c1af-136">Přidat **vzdáleného sledování upozornění [DeviceId]** k **subjektu** pole.</span><span class="sxs-lookup"><span data-stu-id="4c1af-136">Add **Remote monitoring alert [DeviceId]** to the **Subject** field.</span></span> <span data-ttu-id="4c1af-137">V **obsahu e-mailu** pole, přidejte **zařízení [DeviceId] ohlásil [measurementName] s hodnotou [measuredValue].**</span><span class="sxs-lookup"><span data-stu-id="4c1af-137">In the **Email Body** field, add **Device [DeviceId] has reported [measurementName] with value [measuredValue].**</span></span> <span data-ttu-id="4c1af-138">Můžete přidat **[DeviceId]**, **[measurementName]**, a **[measuredValue]** kliknutím v **můžete vložit data z předchozích kroků** části.</span><span class="sxs-lookup"><span data-stu-id="4c1af-138">You can add **[DeviceId]**, **[measurementName]**, and **[measuredValue]** by clicking in the **You can insert data from previous steps** section.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. <span data-ttu-id="4c1af-139">Klikněte na tlačítko **Uložit** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="4c1af-139">Click **Save** in the top menu.</span></span>
13. <span data-ttu-id="4c1af-140">Klikněte na tlačítko **požadavku** aktivační události a zkopírujte **Http Post na tuto adresu URL** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4c1af-140">Click the **Request** trigger and copy the **Http Post to this URL** value.</span></span> <span data-ttu-id="4c1af-141">Později v tomto kurzu musíte tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4c1af-141">You need this URL later in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="4c1af-142">Logic Apps umožňují spustit [mnoho různých typů akce] [ lnk-logic-apps-actions] včetně akce v Office 365.</span><span class="sxs-lookup"><span data-stu-id="4c1af-142">Logic Apps enable you to run [many different types of action][lnk-logic-apps-actions] including actions in Office 365.</span></span> 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a><span data-ttu-id="4c1af-143">Nastavit webovou úlohu EventProcessor</span><span class="sxs-lookup"><span data-stu-id="4c1af-143">Set up the EventProcessor Web Job</span></span>
<span data-ttu-id="4c1af-144">V této části se připojit k aplikaci logiky můžete vytvořit předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="4c1af-144">In this section, you connect your preconfigured solution to the Logic App you created.</span></span> <span data-ttu-id="4c1af-145">Pro dokončení této úlohy, přidejte adresu URL pro aktivaci aplikace logiky k akci, která aktivuje se v případě hodnotu senzor zařízení překračuje prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4c1af-145">To complete this task, you add the URL to trigger the Logic App to the action that fires when a device sensor value exceeds a threshold.</span></span>

1. <span data-ttu-id="4c1af-146">Pomocí vašeho klienta git clone nejnovější verzi [azure-iot-remote-monitoring úložiště github][lnk-rmgithub].</span><span class="sxs-lookup"><span data-stu-id="4c1af-146">Use your git client to clone the latest version of the [azure-iot-remote-monitoring github repository][lnk-rmgithub].</span></span> <span data-ttu-id="4c1af-147">Například:</span><span class="sxs-lookup"><span data-stu-id="4c1af-147">For example:</span></span>
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. <span data-ttu-id="4c1af-148">V sadě Visual Studio, otevřete **RemoteMonitoring.sln** z místní kopie úložiště.</span><span class="sxs-lookup"><span data-stu-id="4c1af-148">In Visual Studio, open the **RemoteMonitoring.sln** from the local copy of the repository.</span></span>
3. <span data-ttu-id="4c1af-149">Otevřete **ActionRepository.cs** v soubor **infrastruktury\\úložiště** složky.</span><span class="sxs-lookup"><span data-stu-id="4c1af-149">Open the **ActionRepository.cs** file in the **Infrastructure\\Repository** folder.</span></span>
4. <span data-ttu-id="4c1af-150">Aktualizace **actionIds** slovníku pomocí **Http Post na tuto adresu URL** jste si poznamenali u aplikace logiky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4c1af-150">Update the **actionIds** dictionary with the **Http Post to this URL** you noted from your Logic App as follows:</span></span>
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. <span data-ttu-id="4c1af-151">Uložte změny v řešení a ukončení Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c1af-151">Save the changes in solution and exit Visual Studio.</span></span>

## <a name="deploy-from-the-command-line"></a><span data-ttu-id="4c1af-152">Nasazení z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4c1af-152">Deploy from the command line</span></span>
<span data-ttu-id="4c1af-153">V této části nasadíte vaší aktualizovanou verzi řešení vzdáleného monitorování nahradit verzi aktuálně spuštěných v Azure.</span><span class="sxs-lookup"><span data-stu-id="4c1af-153">In this section, you deploy your updated version of the remote monitoring solution to replace the version currently running in Azure.</span></span>

1. <span data-ttu-id="4c1af-154">Následující [dev nastavení] [ lnk-devsetup] pokyny pro nastavení prostředí pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="4c1af-154">Following the [dev set-up][lnk-devsetup] instructions to set up your environment for deployment.</span></span>
2. <span data-ttu-id="4c1af-155">Chcete-li nasadit místně, postupujte [místní nasazení] [ lnk-localdeploy] pokyny.</span><span class="sxs-lookup"><span data-stu-id="4c1af-155">To deploy locally, follow the [local deployment][lnk-localdeploy] instructions.</span></span>
3. <span data-ttu-id="4c1af-156">Chcete-li nasadit do cloudu a aktualizovat vaše stávající nasazení cloudu, postupujte [cloudové nasazení] [ lnk-clouddeploy] pokyny.</span><span class="sxs-lookup"><span data-stu-id="4c1af-156">To deploy to the cloud and update your existing cloud deployment, follow the [cloud deployment][lnk-clouddeploy] instructions.</span></span> <span data-ttu-id="4c1af-157">Název původního nasazení použijte jako název nasazení.</span><span class="sxs-lookup"><span data-stu-id="4c1af-157">Use the name of your original deployment as the deployment name.</span></span> <span data-ttu-id="4c1af-158">Například pokud byla volána původního nasazení **demologicapp**, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4c1af-158">For example if the original deployment was called **demologicapp**, use the following command:</span></span>
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   <span data-ttu-id="4c1af-159">Při spuštění skriptu sestavení, nezapomeňte použít stejný účet Azure, předplatné, oblast a instanci Active Directory, které jste použili při zřizování řešení.</span><span class="sxs-lookup"><span data-stu-id="4c1af-159">When the build script runs, be sure to use the same Azure account, subscription, region, and Active Directory instance you used when you provisioned the solution.</span></span>

## <a name="see-your-logic-app-in-action"></a><span data-ttu-id="4c1af-160">V tématu aplikace logiky v akci</span><span class="sxs-lookup"><span data-stu-id="4c1af-160">See your Logic App in action</span></span>
<span data-ttu-id="4c1af-161">Předkonfigurované řešení vzdáleného monitorování má dvě pravidla ve výchozím nastavení při zřizování řešení.</span><span class="sxs-lookup"><span data-stu-id="4c1af-161">The remote monitoring preconfigured solution has two rules set up by default when you provision a solution.</span></span> <span data-ttu-id="4c1af-162">Obě pravidla jsou na **SampleDevice001** zařízení:</span><span class="sxs-lookup"><span data-stu-id="4c1af-162">Both rules are on the **SampleDevice001** device:</span></span>

* <span data-ttu-id="4c1af-163">Teplotní > 38.00</span><span class="sxs-lookup"><span data-stu-id="4c1af-163">Temperature > 38.00</span></span>
* <span data-ttu-id="4c1af-164">Vlhkosti > 48.00</span><span class="sxs-lookup"><span data-stu-id="4c1af-164">Humidity > 48.00</span></span>

<span data-ttu-id="4c1af-165">Aktivační události pravidlo teploty **vyvolat alarmů** akce a vlhkost pravidla aktivační události **SendMessage** akce.</span><span class="sxs-lookup"><span data-stu-id="4c1af-165">The temperature rule triggers the **Raise Alarm** action and the Humidity rule triggers the **SendMessage** action.</span></span> <span data-ttu-id="4c1af-166">Za předpokladu, že používá stejnou adresu URL pro obě akce **ActionRepository** třídy, vaše aplikace logiky aktivačních událostí pro buď pravidlo.</span><span class="sxs-lookup"><span data-stu-id="4c1af-166">Assuming you used the same URL for both actions the **ActionRepository** class, your logic app triggers for either rule.</span></span> <span data-ttu-id="4c1af-167">Obě pravidla sendgrid vám umožňuje používat k odesílání e-mailu na **k** adresu podrobné informace o této výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4c1af-167">Both rules use SendGrid to send an email to the **To** address with details of the alert.</span></span>

> [!NOTE]
> <span data-ttu-id="4c1af-168">Aplikace logiky i nadále aktivovat pokaždé, když je splněna prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4c1af-168">The Logic App continues to trigger every time the threshold is met.</span></span> <span data-ttu-id="4c1af-169">Aby se zabránilo zbytečným e-mailů, můžete buď vypněte pravidla na portálu řešení nebo zakázat aplikaci logiky v [portál Azure][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="4c1af-169">To avoid unnecessary emails, you can either disable the rules in your solution portal or disable the Logic App in the [Azure portal][lnk-azureportal].</span></span>
> 
> 

<span data-ttu-id="4c1af-170">Kromě doručování e-mailů, můžete také zjistit při spuštění aplikace logiky na portálu:</span><span class="sxs-lookup"><span data-stu-id="4c1af-170">In addition to receiving emails, you can also see when the Logic App runs in the portal:</span></span>

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a><span data-ttu-id="4c1af-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c1af-171">Next steps</span></span>
<span data-ttu-id="4c1af-172">Teď, když jste použili aplikaci logiky pro připojení k obchodní proces předkonfigurované řešení, můžete se další informace o možnostech přizpůsobení předkonfigurovaných řešení:</span><span class="sxs-lookup"><span data-stu-id="4c1af-172">Now that you've used a Logic App to connect the preconfigured solution to a business process, you can learn more about the options for customizing the preconfigured solutions:</span></span>

* <span data-ttu-id="4c1af-173">[Použití dynamické telemetrie s předkonfigurovaného řešení vzdáleného monitorování][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="4c1af-173">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="4c1af-174">[Informace metadat zařízení v předkonfigurovaného řešení vzdáleného monitorování][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="4c1af-174">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo]</span></span>

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
