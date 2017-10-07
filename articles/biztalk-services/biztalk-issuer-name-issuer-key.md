---
title: "aaaIssuer název a klíč vystavitele ve službě BizTalk Services | Microsoft Docs"
description: "Zjistěte, jak tooretrieve název vystavitele a klíč vystavitele pro Service Bus nebo řízení přístupu (ACS) ve službě BizTalk Services. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: cc84c2820724ae3e7fc7c40ddbcd83a169add911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a><span data-ttu-id="928f4-104">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-104">BizTalk Services: Issuer Name and Issuer Key</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="928f4-105">Služba Azure BizTalk Services používá hello název vystavitele sběrnice služby a klíč vystavitele a hello název vystavitele řízení přístupu a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="928f4-105">Azure BizTalk Services uses hello Service Bus Issuer Name and Issuer Key, and hello Access Control Issuer Name and Issuer Key.</span></span> <span data-ttu-id="928f4-106">Zejména:</span><span class="sxs-lookup"><span data-stu-id="928f4-106">Specifically:</span></span>

| <span data-ttu-id="928f4-107">Úkol</span><span class="sxs-lookup"><span data-stu-id="928f4-107">Task</span></span> | <span data-ttu-id="928f4-108">Které název vystavitele a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-108">Which Issuer Name and Issuer Key</span></span> |
| --- | --- |
| <span data-ttu-id="928f4-109">Nasazení aplikace z Visual Studia</span><span class="sxs-lookup"><span data-stu-id="928f4-109">Deploying your application from Visual Studio</span></span> |<span data-ttu-id="928f4-110">Název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-110">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="928f4-111">Konfigurace hello portálu služby Azure BizTalk</span><span class="sxs-lookup"><span data-stu-id="928f4-111">Configuring hello Azure BizTalk Services Portal</span></span> |<span data-ttu-id="928f4-112">Název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-112">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="928f4-113">Vytvoření předávací LOB pomocí hello adaptér služby BizTalk v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="928f4-113">Creating LOB Relays with hello BizTalk Adapter Services in Visual Studio</span></span> |<span data-ttu-id="928f4-114">Název vystavitele sběrnice služby a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-114">Service Bus Issuer Name and Issuer Key</span></span> |

<span data-ttu-id="928f4-115">Toto téma uvádí hello kroky tooretrieve hello název vystavitele a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="928f4-115">This topic lists hello steps tooretrieve hello Issuer Name and Issuer Key.</span></span> 

## <a name="access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="928f4-116">Název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-116">Access Control Issuer Name and Issuer Key</span></span>
<span data-ttu-id="928f4-117">Hello název vystavitele řízení přístupu a klíč vystavitele používá hello následující:</span><span class="sxs-lookup"><span data-stu-id="928f4-117">hello Access Control Issuer Name and Issuer Key are used by hello following:</span></span>

* <span data-ttu-id="928f4-118">Aplikace služby BizTalk Azure vytvořit v sadě Visual Studio: toosuccessfully nasazení aplikace služby BizTalk v sadě Visual Studio tooAzure, zadejte hello název vystavitele řízení přístupu a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="928f4-118">Your Azure BizTalk Service application created in Visual Studio: toosuccessfully deploy your BizTalk Service application in Visual Studio tooAzure, you enter hello Access Control Issuer Name and Issuer Key.</span></span> 
* <span data-ttu-id="928f4-119">Hello portálu Azure BizTalk Services: při vytváření služby BizTalk a otevřete hello portál služby BizTalk, název vystavitele řízení přístupu a klíč vystavitele se automaticky registruje pro vaše nasazení s hello stejné hodnoty řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="928f4-119">hello Azure BizTalk Services  Portal: When you create a BizTalk Service and open hello BizTalk Services Portal, your Access Control Issuer Name and Issuer Key are automatically registered for your deployments with hello same Access Control values.</span></span>

### <a name="get-hello-access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="928f4-120">Získat hello název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-120">Get hello Access Control Issuer Name and Issuer Key</span></span>

<span data-ttu-id="928f4-121">toouse ACS pro ověřování a získání hello hodnoty název vystavitele a klíč vystavitele, hello celkové krokům patří:</span><span class="sxs-lookup"><span data-stu-id="928f4-121">toouse ACS for authentication, and get hello Issuer Name and Issuer Key values, hello overall steps include:</span></span>

1. <span data-ttu-id="928f4-122">Nainstalujte hello [rutin prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="928f4-122">Install hello [Azure Powershell cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
2. <span data-ttu-id="928f4-123">Přidání účtu Azure:`Add-AzureAccount`</span><span class="sxs-lookup"><span data-stu-id="928f4-123">Add your Azure account: `Add-AzureAccount`</span></span>
3. <span data-ttu-id="928f4-124">Vrátí název předplatného:`get-azuresubscription`</span><span class="sxs-lookup"><span data-stu-id="928f4-124">Return your subscription name: `get-azuresubscription`</span></span>
4. <span data-ttu-id="928f4-125">Vyberte předplatné:`select-azuresubscription <name of your subscription>`</span><span class="sxs-lookup"><span data-stu-id="928f4-125">Select your subscription: `select-azuresubscription <name of your subscription>`</span></span> 
5. <span data-ttu-id="928f4-126">Vytvořte nový obor názvů:`new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="928f4-126">Create a new namespace: `new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>

    <span data-ttu-id="928f4-127">Příklad:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="928f4-127">Example:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>
      
5. <span data-ttu-id="928f4-128">Když hello nový obor názvů služby ACS je vytvořen (který může trvat několik minut), hodnoty název vystavitele a klíč vystavitele hello jsou uvedené v hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="928f4-128">When hello new ACS namespace is created (which can take several minutes), hello Issuer Name and Issuer Key values are listed in hello connection string:</span></span> 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

<span data-ttu-id="928f4-129">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="928f4-129">toosummarize:</span></span>  
<span data-ttu-id="928f4-130">Název vystavitele = SharedSecretIssuer</span><span class="sxs-lookup"><span data-stu-id="928f4-130">Issuer Name = SharedSecretIssuer</span></span>  
<span data-ttu-id="928f4-131">Klíče vystavitele = SharedSecretKey</span><span class="sxs-lookup"><span data-stu-id="928f4-131">Issuer Key = SharedSecretKey</span></span>

<span data-ttu-id="928f4-132">Další informace o hello [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="928f4-132">More on hello [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet.</span></span> 

## <a name="service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="928f4-133">Název vystavitele sběrnice služby a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-133">Service Bus Issuer Name and Issuer Key</span></span>
<span data-ttu-id="928f4-134">Název vystavitele sběrnice služby a klíč vystavitele jsou používány adaptér služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="928f4-134">Service Bus Issuer Name and Issuer Key are used by BizTalk Adapter Services.</span></span> <span data-ttu-id="928f4-135">V projektu služby BizTalk v sadě Visual Studio používáte hello BizTalk Services adaptér tooconnect tooan v místním obchodní (LOB) systému.</span><span class="sxs-lookup"><span data-stu-id="928f4-135">In your BizTalk Services project in Visual Studio, you use hello BizTalk Adapter Services tooconnect tooan on-premises Line-of-Business (LOB) system.</span></span> <span data-ttu-id="928f4-136">tooconnect, vytvořit hello obchodní předávání a zadejte podrobnosti o systémové vaše obchodní.</span><span class="sxs-lookup"><span data-stu-id="928f4-136">tooconnect, you create hello LOB Relay and enter your LOB system details.</span></span> <span data-ttu-id="928f4-137">Pokud v takovém případě zadáte také hello název vystavitele sběrnice služby a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="928f4-137">When doing this, you also enter hello Service Bus Issuer Name and Issuer Key.</span></span>

### <a name="tooretrieve-hello-service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="928f4-138">tooretrieve hello název vystavitele sběrnice služby a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-138">tooretrieve hello Service Bus Issuer Name and Issuer Key</span></span>
1. <span data-ttu-id="928f4-139">Přihlaste se toohello [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="928f4-139">Sign in toohello [Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="928f4-140">V levém navigačním podokně hello vyberte **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="928f4-140">In hello left navigation pane, select **Service Bus**.</span></span>
3. <span data-ttu-id="928f4-141">Zvolte svůj obor názvů.</span><span class="sxs-lookup"><span data-stu-id="928f4-141">Select your namespace.</span></span> <span data-ttu-id="928f4-142">Hello hlavním panelu vyberte **informace o připojení**.</span><span class="sxs-lookup"><span data-stu-id="928f4-142">In hello task bar, select **Connection Information**.</span></span> <span data-ttu-id="928f4-143">Zobrazí se hello **výchozí vystavitele** (název vystavitele) a **výchozí klíč** (Issuer Key).</span><span class="sxs-lookup"><span data-stu-id="928f4-143">This displays hello **Default Issuer** (Issuer Name) and **Default Key** (Issuer Key).</span></span> <span data-ttu-id="928f4-144">Jejich hodnoty lze zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="928f4-144">Their values can be copied.</span></span>  

<span data-ttu-id="928f4-145">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="928f4-145">toosummarize:</span></span>  
<span data-ttu-id="928f4-146">Název vystavitele = výchozí vystavitele</span><span class="sxs-lookup"><span data-stu-id="928f4-146">Issuer Name = Default Issuer</span></span>  
<span data-ttu-id="928f4-147">Klíče vystavitele = výchozí klíč</span><span class="sxs-lookup"><span data-stu-id="928f4-147">Issuer Key = Default Key</span></span>

## <a name="next"></a><span data-ttu-id="928f4-148">Další</span><span class="sxs-lookup"><span data-stu-id="928f4-148">Next</span></span>
<span data-ttu-id="928f4-149">Další témata týkající se služby Azure BizTalk Services:</span><span class="sxs-lookup"><span data-stu-id="928f4-149">Additional Azure BizTalk Services topics:</span></span>

* [<span data-ttu-id="928f4-150">Instalace hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="928f4-150">Installing hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="928f4-151">Kurzy: Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="928f4-151">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="928f4-152">Jak začít používat hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="928f4-152">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="928f4-153">Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="928f4-153">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="928f4-154">Viz také</span><span class="sxs-lookup"><span data-stu-id="928f4-154">See Also</span></span>
* [<span data-ttu-id="928f4-155">Postupy: použití služby ACS Management Service tooConfigure identity služby</span><span class="sxs-lookup"><span data-stu-id="928f4-155">How to: Use ACS Management Service tooConfigure Service Identities</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [<span data-ttu-id="928f4-156">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="928f4-156">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="928f4-157">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="928f4-157">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="928f4-158">BizTalk Services: Tabulka stavů zřízení</span><span class="sxs-lookup"><span data-stu-id="928f4-158">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="928f4-159">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="928f4-159">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="928f4-160">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="928f4-160">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="928f4-161">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="928f4-161">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

