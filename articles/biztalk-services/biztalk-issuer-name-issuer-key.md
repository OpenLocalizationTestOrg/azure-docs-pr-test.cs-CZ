---
title: "Název vystavitele a klíč vystavitele ve službě BizTalk Services | Microsoft Docs"
description: "Zjistěte, jak načíst název vystavitele a klíč vystavitele pro Service Bus nebo řízení přístupu (ACS) ve službě BizTalk Services. MABS, WABS"
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
ms.openlocfilehash: b9fd985c23558596408e78eadae00dd0f95c4214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a><span data-ttu-id="7dee8-104">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-104">BizTalk Services: Issuer Name and Issuer Key</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="7dee8-105">Služba Azure BizTalk Services používá název vystavitele Service Bus a Issuer Key a název vystavitele řízení přístupu a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="7dee8-105">Azure BizTalk Services uses the Service Bus Issuer Name and Issuer Key, and the Access Control Issuer Name and Issuer Key.</span></span> <span data-ttu-id="7dee8-106">Zejména:</span><span class="sxs-lookup"><span data-stu-id="7dee8-106">Specifically:</span></span>

| <span data-ttu-id="7dee8-107">Úkol</span><span class="sxs-lookup"><span data-stu-id="7dee8-107">Task</span></span> | <span data-ttu-id="7dee8-108">Které název vystavitele a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-108">Which Issuer Name and Issuer Key</span></span> |
| --- | --- |
| <span data-ttu-id="7dee8-109">Nasazení aplikace z Visual Studia</span><span class="sxs-lookup"><span data-stu-id="7dee8-109">Deploying your application from Visual Studio</span></span> |<span data-ttu-id="7dee8-110">Název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-110">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="7dee8-111">Konfigurace portálu Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="7dee8-111">Configuring the Azure BizTalk Services Portal</span></span> |<span data-ttu-id="7dee8-112">Název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-112">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="7dee8-113">Vytvoření předávací LOB pomocí adaptéru služby BizTalk v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dee8-113">Creating LOB Relays with the BizTalk Adapter Services in Visual Studio</span></span> |<span data-ttu-id="7dee8-114">Název vystavitele sběrnice služby a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-114">Service Bus Issuer Name and Issuer Key</span></span> |

<span data-ttu-id="7dee8-115">Toto téma obsahuje kroky k načtení název vystavitele a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="7dee8-115">This topic lists the steps to retrieve the Issuer Name and Issuer Key.</span></span> 

## <a name="access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="7dee8-116">Název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-116">Access Control Issuer Name and Issuer Key</span></span>
<span data-ttu-id="7dee8-117">Název vystavitele řízení přístupu a klíč vystavitele se používají následující:</span><span class="sxs-lookup"><span data-stu-id="7dee8-117">The Access Control Issuer Name and Issuer Key are used by the following:</span></span>

* <span data-ttu-id="7dee8-118">Aplikace služby BizTalk Azure vytvořit v sadě Visual Studio: Pokud chcete úspěšně nasadit aplikace služby BizTalk v sadě Visual Studio do Azure, zadejte název vystavitele řízení přístupu a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="7dee8-118">Your Azure BizTalk Service application created in Visual Studio: To successfully deploy your BizTalk Service application in Visual Studio to Azure, you enter the Access Control Issuer Name and Issuer Key.</span></span> 
* <span data-ttu-id="7dee8-119">Portál služby Azure BizTalk: Při vytváření služby BizTalk a otevřete portál služby BizTalk, název vystavitele řízení přístupu a klíč vystavitele se automaticky registruje pro vaše nasazení s stejné hodnoty řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="7dee8-119">The Azure BizTalk Services  Portal: When you create a BizTalk Service and open the BizTalk Services Portal, your Access Control Issuer Name and Issuer Key are automatically registered for your deployments with the same Access Control values.</span></span>

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="7dee8-120">Získat název vystavitele řízení přístupu a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-120">Get the Access Control Issuer Name and Issuer Key</span></span>

<span data-ttu-id="7dee8-121">Pokud chcete používat pro ověřování služby ACS a získat hodnoty název vystavitele a klíč vystavitele, celkový krokům patří:</span><span class="sxs-lookup"><span data-stu-id="7dee8-121">To use ACS for authentication, and get the Issuer Name and Issuer Key values, the overall steps include:</span></span>

1. <span data-ttu-id="7dee8-122">Nainstalujte [rutin prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="7dee8-122">Install the [Azure Powershell cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
2. <span data-ttu-id="7dee8-123">Přidání účtu Azure:`Add-AzureAccount`</span><span class="sxs-lookup"><span data-stu-id="7dee8-123">Add your Azure account: `Add-AzureAccount`</span></span>
3. <span data-ttu-id="7dee8-124">Vrátí název předplatného:`get-azuresubscription`</span><span class="sxs-lookup"><span data-stu-id="7dee8-124">Return your subscription name: `get-azuresubscription`</span></span>
4. <span data-ttu-id="7dee8-125">Vyberte předplatné:`select-azuresubscription <name of your subscription>`</span><span class="sxs-lookup"><span data-stu-id="7dee8-125">Select your subscription: `select-azuresubscription <name of your subscription>`</span></span> 
5. <span data-ttu-id="7dee8-126">Vytvořte nový obor názvů:`new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="7dee8-126">Create a new namespace: `new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>

    <span data-ttu-id="7dee8-127">Příklad:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="7dee8-127">Example:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>
      
5. <span data-ttu-id="7dee8-128">Když je vytvořen nový obor názvů služby ACS, (který může trvat několik minut), jsou uvedené hodnoty název vystavitele a klíč vystavitele v připojovacím řetězci:</span><span class="sxs-lookup"><span data-stu-id="7dee8-128">When the new ACS namespace is created (which can take several minutes), the Issuer Name and Issuer Key values are listed in the connection string:</span></span> 

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

<span data-ttu-id="7dee8-129">Shrnutí:</span><span class="sxs-lookup"><span data-stu-id="7dee8-129">To summarize:</span></span>  
<span data-ttu-id="7dee8-130">Název vystavitele = SharedSecretIssuer</span><span class="sxs-lookup"><span data-stu-id="7dee8-130">Issuer Name = SharedSecretIssuer</span></span>  
<span data-ttu-id="7dee8-131">Klíče vystavitele = SharedSecretKey</span><span class="sxs-lookup"><span data-stu-id="7dee8-131">Issuer Key = SharedSecretKey</span></span>

<span data-ttu-id="7dee8-132">Více na [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7dee8-132">More on the [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet.</span></span> 

## <a name="service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="7dee8-133">Název vystavitele sběrnice služby a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-133">Service Bus Issuer Name and Issuer Key</span></span>
<span data-ttu-id="7dee8-134">Název vystavitele sběrnice služby a klíč vystavitele jsou používány adaptér služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="7dee8-134">Service Bus Issuer Name and Issuer Key are used by BizTalk Adapter Services.</span></span> <span data-ttu-id="7dee8-135">V projektu služby BizTalk v sadě Visual Studio používáte adaptér služby BizTalk se připojit k-obchodní (LOB) v místním systému.</span><span class="sxs-lookup"><span data-stu-id="7dee8-135">In your BizTalk Services project in Visual Studio, you use the BizTalk Adapter Services to connect to an on-premises Line-of-Business (LOB) system.</span></span> <span data-ttu-id="7dee8-136">Pokud chcete připojit, vytvořit obchodní předávání a zadejte podrobnosti o systémové vaše obchodní.</span><span class="sxs-lookup"><span data-stu-id="7dee8-136">To connect, you create the LOB Relay and enter your LOB system details.</span></span> <span data-ttu-id="7dee8-137">Při této činnosti, můžete taky zadat název vystavitele sběrnice služby a klíč vystavitele.</span><span class="sxs-lookup"><span data-stu-id="7dee8-137">When doing this, you also enter the Service Bus Issuer Name and Issuer Key.</span></span>

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="7dee8-138">Načíst název vystavitele sběrnice služby a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-138">To retrieve the Service Bus Issuer Name and Issuer Key</span></span>
1. <span data-ttu-id="7dee8-139">Přihlaste se do [portál Azure Classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="7dee8-139">Sign in to the [Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="7dee8-140">V levém navigačním podokně, vyberte **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="7dee8-140">In the left navigation pane, select **Service Bus**.</span></span>
3. <span data-ttu-id="7dee8-141">Zvolte svůj obor názvů.</span><span class="sxs-lookup"><span data-stu-id="7dee8-141">Select your namespace.</span></span> <span data-ttu-id="7dee8-142">Na hlavním panelu vyberte **informace o připojení**.</span><span class="sxs-lookup"><span data-stu-id="7dee8-142">In the task bar, select **Connection Information**.</span></span> <span data-ttu-id="7dee8-143">Zobrazí se **výchozí vystavitele** (název vystavitele) a **výchozí klíč** (Issuer Key).</span><span class="sxs-lookup"><span data-stu-id="7dee8-143">This displays the **Default Issuer** (Issuer Name) and **Default Key** (Issuer Key).</span></span> <span data-ttu-id="7dee8-144">Jejich hodnoty lze zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="7dee8-144">Their values can be copied.</span></span>  

<span data-ttu-id="7dee8-145">Shrnutí:</span><span class="sxs-lookup"><span data-stu-id="7dee8-145">To summarize:</span></span>  
<span data-ttu-id="7dee8-146">Název vystavitele = výchozí vystavitele</span><span class="sxs-lookup"><span data-stu-id="7dee8-146">Issuer Name = Default Issuer</span></span>  
<span data-ttu-id="7dee8-147">Klíče vystavitele = výchozí klíč</span><span class="sxs-lookup"><span data-stu-id="7dee8-147">Issuer Key = Default Key</span></span>

## <a name="next"></a><span data-ttu-id="7dee8-148">Další</span><span class="sxs-lookup"><span data-stu-id="7dee8-148">Next</span></span>
<span data-ttu-id="7dee8-149">Další témata týkající se služby Azure BizTalk Services:</span><span class="sxs-lookup"><span data-stu-id="7dee8-149">Additional Azure BizTalk Services topics:</span></span>

* [<span data-ttu-id="7dee8-150">Instalace služby Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="7dee8-150">Installing the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="7dee8-151">Kurzy: Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="7dee8-151">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="7dee8-152">Jak začít používat sadu SDK Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="7dee8-152">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="7dee8-153">Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="7dee8-153">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="7dee8-154">Viz také</span><span class="sxs-lookup"><span data-stu-id="7dee8-154">See Also</span></span>
* [<span data-ttu-id="7dee8-155">Postupy: Konfigurace identity služby pomocí služby správy služby ACS</span><span class="sxs-lookup"><span data-stu-id="7dee8-155">How to: Use ACS Management Service to Configure Service Identities</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [<span data-ttu-id="7dee8-156">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="7dee8-156">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="7dee8-157">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="7dee8-157">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="7dee8-158">BizTalk Services: Tabulka stavů zřízení</span><span class="sxs-lookup"><span data-stu-id="7dee8-158">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="7dee8-159">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="7dee8-159">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="7dee8-160">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="7dee8-160">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="7dee8-161">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="7dee8-161">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

