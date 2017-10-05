---
title: "Místní data brána | Microsoft Docs"
description: "Bránu místní je nezbytný, pokud váš server služby Analysis Services v Azure se připojí k místní datové zdroje."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: 514b5404e8cbfa0baa657eb41736e20cad502638
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a><span data-ttu-id="ee741-103">Připojení k místní zdroje dat s Azure místní brány dat</span><span class="sxs-lookup"><span data-stu-id="ee741-103">Connecting to on-premises data sources with Azure On-premises Data Gateway</span></span>
<span data-ttu-id="ee741-104">Místní data brána funguje jako mostu, zajištění zabezpečených dat přenos mezi místní zdroje dat a vaše servery Azure Analysis Services v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ee741-104">The on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in the cloud.</span></span> <span data-ttu-id="ee741-105">Kromě práce s více servery Azure Analysis Services ve stejné oblasti, nejnovější verzi brány taky spolupracuje se službou Azure Logic Apps, Power BI, Power aplikace a Flow společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ee741-105">In addition to working with multiple Azure Analysis Services servers in the same region, the latest version of the gateway also works with Azure Logic Apps, Power BI, Power Apps, and Microsoft Flow.</span></span> <span data-ttu-id="ee741-106">S jednou bránou můžete přidružit více služeb ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="ee741-106">You can associate multiple services in the same region with a single gateway.</span></span> 

 <span data-ttu-id="ee741-107">Azure Analysis Services vyžaduje prostředek brány ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="ee741-107">Azure Analysis Services requires a gateway resource in the same region.</span></span> <span data-ttu-id="ee741-108">Například pokud máte servery Azure Analysis Services v oblasti Východ USA 2, musíte prostředek brány v oblasti Východ USA 2.</span><span class="sxs-lookup"><span data-stu-id="ee741-108">For example, if you have Azure Analysis Services servers in the East US 2 region, you need a gateway resource in the East US 2 region.</span></span> <span data-ttu-id="ee741-109">Několik serverů v oblasti Východ USA 2 můžete použít stejné bráně.</span><span class="sxs-lookup"><span data-stu-id="ee741-109">Multiple servers in East US 2 can use the same gateway.</span></span>

<span data-ttu-id="ee741-110">Získání Instalační program s bránou prvním je proces, který sestávající ze čtyř částí:</span><span class="sxs-lookup"><span data-stu-id="ee741-110">Getting setup with the gateway the first time is a four-part process:</span></span>

- <span data-ttu-id="ee741-111">**Stáhněte a spusťte instalační program** – tento krok nainstaluje služba brány do počítače ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="ee741-111">**Download and run setup** - This step installs a gateway service on a computer in your organization.</span></span>

- <span data-ttu-id="ee741-112">**Zaregistrujte bránu** – v tomto kroku, zadejte název a obnovení klíče pro bránu a vyberte oblast, registrace brány cloudové službě brány.</span><span class="sxs-lookup"><span data-stu-id="ee741-112">**Register your gateway** - In this step, you specify a name and recovery key for your gateway, and select a region, registering your gateway with the Gateway Cloud Service.</span></span>

- <span data-ttu-id="ee741-113">**Vytvořte prostředek brány v Azure** – v tomto kroku vytvoříte bránu prostředků ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="ee741-113">**Create a gateway resource in Azure** - In this step, you create a gateway resource in your Azure subscription.</span></span>

- <span data-ttu-id="ee741-114">**Připojte své servery k prostředku brány** – až budete mít bránu prostředků v rámci vašeho předplatného, můžete začít vaše servery k němu připojuje.</span><span class="sxs-lookup"><span data-stu-id="ee741-114">**Connect your servers to your gateway resource** - Once you have a gateway resource in your subscription, you can begin connecting your servers to it.</span></span>

<span data-ttu-id="ee741-115">Když máte brány prostředek nakonfigurovaný pro vaše předplatné, můžete k nim mohla připojit víc serverů a dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="ee741-115">Once you have a gateway resource configured for your subscription, you can connect multiple servers, and other services to it.</span></span> <span data-ttu-id="ee741-116">Stačí nainstalovat jinou bránu a vytvářet prostředky další brány, pokud máte servery nebo jiné služby v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="ee741-116">You only need to install a different gateway and create additional gateway resources if you have servers or other services in a different region.</span></span>

<span data-ttu-id="ee741-117">Pokud chcete začít hned, najdete v části [nainstalujte a nakonfigurujte místní brána dat](analysis-services-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="ee741-117">To get started right away, see [Install and configure on-premises data gateway](analysis-services-gateway-install.md).</span></span>

## <span data-ttu-id="ee741-118"><a name="how-it-works"></a>Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="ee741-118"><a name="how-it-works"> </a>How it works</span></span>
<span data-ttu-id="ee741-119">Brána je nainstalujte do počítače ve vaší organizaci používá jako služby systému Windows, **místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="ee741-119">The gateway you install on a computer in your organization runs as a Windows service, **On-premises data gateway**.</span></span> <span data-ttu-id="ee741-120">Tato místní služba není zaregistrována cloudové službě brány přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ee741-120">This local service is registered with the Gateway Cloud Service through Azure Service Bus.</span></span> <span data-ttu-id="ee741-121">Pak vytvořte prostředek brány cloudové službě brány pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ee741-121">You then create a gateway resource Gateway Cloud Service for your Azure subscription.</span></span> <span data-ttu-id="ee741-122">Vaše servery Azure Analysis Services jsou připojena k vaší brány prostředku.</span><span class="sxs-lookup"><span data-stu-id="ee741-122">Your Azure Analysis Services servers are then connected to your gateway resource.</span></span> <span data-ttu-id="ee741-123">Při modely na serveru je nutné se připojit k místním datům zdroje pro dotazy nebo zpracování, prochází přes prostředek brány, Azure Service Bus, místní služba brány dat a zdroje dat dotazu a datový tok.</span><span class="sxs-lookup"><span data-stu-id="ee741-123">When models on your server need to connect to your on-premises data sources for queries or processing, a query and data flow traverses the gateway resource, Azure Service Bus, the local on-premises data gateway service, and your data sources.</span></span> 

![Jak to funguje](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

<span data-ttu-id="ee741-125">Tok dotazy a data:</span><span class="sxs-lookup"><span data-stu-id="ee741-125">Queries and data flow:</span></span>

1. <span data-ttu-id="ee741-126">Dotaz byl vytvořený Cloudová služba se zašifrované přihlašovací údaje pro zdroj dat na místě.</span><span class="sxs-lookup"><span data-stu-id="ee741-126">A query is created by the cloud service with the encrypted credentials for the on-premises data source.</span></span> <span data-ttu-id="ee741-127">Pak se odešlou do fronty pro bránu ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="ee741-127">It's then sent to a queue for the gateway to process.</span></span>
2. <span data-ttu-id="ee741-128">Cloudová služba brány analyzuje dotaz a nabízených oznámení požadavku [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="ee741-128">The gateway cloud service analyzes the query and pushes the request to the [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span></span>
3. <span data-ttu-id="ee741-129">Bránu dat místní dotáže Azure Service Bus pro žádosti čekající na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="ee741-129">The on-premises data gateway polls the Azure Service Bus for pending requests.</span></span>
4. <span data-ttu-id="ee741-130">Brána získá dotaz, dešifruje přihlašovací údaje a připojuje ke zdroji dat pomocí těchto přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="ee741-130">The gateway gets the query, decrypts the credentials, and connects to the data sources with those credentials.</span></span>
5. <span data-ttu-id="ee741-131">Brána odešle dotaz na zdroj dat pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="ee741-131">The gateway sends the query to the data source for execution.</span></span>
6. <span data-ttu-id="ee741-132">Výsledky jsou odesílány ze zdroje dat zpět na bránu a pak do cloudové služby a vaším serverem.</span><span class="sxs-lookup"><span data-stu-id="ee741-132">The results are sent from the data source, back to the gateway, and then onto the cloud service and your server.</span></span>

## <span data-ttu-id="ee741-133"><a name="windows-service-account"></a>Účtu služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="ee741-133"><a name="windows-service-account"> </a>Windows Service account</span></span>
<span data-ttu-id="ee741-134">Místní brána dat je nakonfigurován pro použití *NT SERVICE\PBIEgwService* pro přihlašovací pověření služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ee741-134">The on-premises data gateway is configured to use *NT SERVICE\PBIEgwService* for the Windows service logon credential.</span></span> <span data-ttu-id="ee741-135">Ve výchozím nastavení má právo přihlášení jako služba; v kontextu na počítač, který instalujete na bránu.</span><span class="sxs-lookup"><span data-stu-id="ee741-135">By default, it has the right of Logon as a service; in the context of the machine that you are installing the gateway on.</span></span> <span data-ttu-id="ee741-136">Tento přihlašovací údaj není stejný účet používaný pro připojení k místním zdrojům dat nebo účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="ee741-136">This credential is not the same account used to connect to on-premises data sources or your Azure account.</span></span>  

<span data-ttu-id="ee741-137">Pokud narazíte na potíže s proxy serveru z důvodu ověřování, můžete změnit účet služby systému Windows na uživatele domény nebo účet spravované služby.</span><span class="sxs-lookup"><span data-stu-id="ee741-137">If you encounter issues with your proxy server due to authentication, you may want to change the Windows service account to a domain user or managed service account.</span></span>

## <span data-ttu-id="ee741-138"><a name="ports"></a>Porty</span><span class="sxs-lookup"><span data-stu-id="ee741-138"><a name="ports"> </a>Ports</span></span>
<span data-ttu-id="ee741-139">Brána vytvoří odchozí připojení k Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ee741-139">The gateway creates an outbound connection to Azure Service Bus.</span></span> <span data-ttu-id="ee741-140">Komunikuje na odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 prostřednictvím 9354.</span><span class="sxs-lookup"><span data-stu-id="ee741-140">It communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span>  <span data-ttu-id="ee741-141">Brána nevyžaduje příchozí porty.</span><span class="sxs-lookup"><span data-stu-id="ee741-141">The gateway does not require inbound ports.</span></span>

<span data-ttu-id="ee741-142">Doporučujeme povolených IP adres pro vaši oblast dat v bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="ee741-142">We recommend you whitelist the IP addresses for your data region in your firewall.</span></span> <span data-ttu-id="ee741-143">Si můžete stáhnout [seznamu Microsoft Azure Datacenter IP](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="ee741-143">You can download the [Microsoft Azure Datacenter IP list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="ee741-144">Tento seznam je aktualizovaný týdně.</span><span class="sxs-lookup"><span data-stu-id="ee741-144">This list is updated weekly.</span></span>

> [!NOTE]
> <span data-ttu-id="ee741-145">V notaci CIDR, jsou uvedené v seznamu IP Datacentra Azure IP adresy.</span><span class="sxs-lookup"><span data-stu-id="ee741-145">The IP Addresses listed in the Azure Datacenter IP list are in CIDR notation.</span></span> <span data-ttu-id="ee741-146">Například 10.0.0.0/24 neznamená 10.0.0.0 prostřednictvím 10.0.0.24.</span><span class="sxs-lookup"><span data-stu-id="ee741-146">For example, 10.0.0.0/24 does not mean 10.0.0.0 through 10.0.0.24.</span></span> <span data-ttu-id="ee741-147">Další informace o [notaci CIDR](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="ee741-147">Learn more about the [CIDR notation](http://whatismyipaddress.com/cidr).</span></span>
>
>

<span data-ttu-id="ee741-148">Níže jsou uvedeny plně kvalifikované názvy domény používá bránu.</span><span class="sxs-lookup"><span data-stu-id="ee741-148">The following are the fully qualified domain names used by the gateway.</span></span>

| <span data-ttu-id="ee741-149">Názvy domén</span><span class="sxs-lookup"><span data-stu-id="ee741-149">Domain names</span></span> | <span data-ttu-id="ee741-150">Odchozí porty</span><span class="sxs-lookup"><span data-stu-id="ee741-150">Outbound ports</span></span> | <span data-ttu-id="ee741-151">Popis</span><span class="sxs-lookup"><span data-stu-id="ee741-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee741-152">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="ee741-152">*.powerbi.com</span></span> |<span data-ttu-id="ee741-153">80</span><span class="sxs-lookup"><span data-stu-id="ee741-153">80</span></span> |<span data-ttu-id="ee741-154">HTTP používaný ke stahování instalační služby.</span><span class="sxs-lookup"><span data-stu-id="ee741-154">HTTP used to download the installer.</span></span> |
| <span data-ttu-id="ee741-155">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="ee741-155">*.powerbi.com</span></span> |<span data-ttu-id="ee741-156">443</span><span class="sxs-lookup"><span data-stu-id="ee741-156">443</span></span> |<span data-ttu-id="ee741-157">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee741-157">HTTPS</span></span> |
| <span data-ttu-id="ee741-158">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="ee741-158">*.analysis.windows.net</span></span> |<span data-ttu-id="ee741-159">443</span><span class="sxs-lookup"><span data-stu-id="ee741-159">443</span></span> |<span data-ttu-id="ee741-160">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee741-160">HTTPS</span></span> |
| <span data-ttu-id="ee741-161">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="ee741-161">*.login.windows.net</span></span> |<span data-ttu-id="ee741-162">443</span><span class="sxs-lookup"><span data-stu-id="ee741-162">443</span></span> |<span data-ttu-id="ee741-163">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee741-163">HTTPS</span></span> |
| <span data-ttu-id="ee741-164">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="ee741-164">*.servicebus.windows.net</span></span> |<span data-ttu-id="ee741-165">5671-5672</span><span class="sxs-lookup"><span data-stu-id="ee741-165">5671-5672</span></span> |<span data-ttu-id="ee741-166">Pokročilé zpráv služby Řízení front Protocol (AMQP)</span><span class="sxs-lookup"><span data-stu-id="ee741-166">Advanced Message Queuing Protocol (AMQP)</span></span> |
| <span data-ttu-id="ee741-167">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="ee741-167">*.servicebus.windows.net</span></span> |<span data-ttu-id="ee741-168">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="ee741-168">443, 9350-9354</span></span> |<span data-ttu-id="ee741-169">Moduly pro naslouchání na předávání přes Service Bus přes TCP (vyžaduje 443 pro získání tokenu řízení přístupu)</span><span class="sxs-lookup"><span data-stu-id="ee741-169">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> |
| <span data-ttu-id="ee741-170">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="ee741-170">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="ee741-171">443</span><span class="sxs-lookup"><span data-stu-id="ee741-171">443</span></span> |<span data-ttu-id="ee741-172">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee741-172">HTTPS</span></span> |
| <span data-ttu-id="ee741-173">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="ee741-173">*.core.windows.net</span></span> |<span data-ttu-id="ee741-174">443</span><span class="sxs-lookup"><span data-stu-id="ee741-174">443</span></span> |<span data-ttu-id="ee741-175">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee741-175">HTTPS</span></span> |
| <span data-ttu-id="ee741-176">Login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="ee741-176">login.microsoftonline.com</span></span> |<span data-ttu-id="ee741-177">443</span><span class="sxs-lookup"><span data-stu-id="ee741-177">443</span></span> |<span data-ttu-id="ee741-178">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee741-178">HTTPS</span></span> |
| <span data-ttu-id="ee741-179">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="ee741-179">*.msftncsi.com</span></span> |<span data-ttu-id="ee741-180">443</span><span class="sxs-lookup"><span data-stu-id="ee741-180">443</span></span> |<span data-ttu-id="ee741-181">Použít k testování připojení k Internetu, pokud brána nedostupná pro službu Power BI.</span><span class="sxs-lookup"><span data-stu-id="ee741-181">Used to test internet connectivity if the gateway is unreachable by the Power BI service.</span></span> |
| <span data-ttu-id="ee741-182">*.microsoftonline p.com</span><span class="sxs-lookup"><span data-stu-id="ee741-182">*.microsoftonline-p.com</span></span> |<span data-ttu-id="ee741-183">443</span><span class="sxs-lookup"><span data-stu-id="ee741-183">443</span></span> |<span data-ttu-id="ee741-184">Slouží k ověření v závislosti na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ee741-184">Used for authentication depending on configuration.</span></span> |

### <span data-ttu-id="ee741-185"><a name="force-https"></a>Vynucení komunikaci přes protokol HTTPS s Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="ee741-185"><a name="force-https"></a>Forcing HTTPS communication with Azure Service Bus</span></span>
<span data-ttu-id="ee741-186">Můžete vynutit bránu a komunikovat s Azure Service Bus pomocí protokolu HTTPS místo přímé TCP; ale to tak může výrazně snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="ee741-186">You can force the gateway to communicate with Azure Service Bus by using HTTPS instead of direct TCP; however, doing so can greatly reduce performance.</span></span> <span data-ttu-id="ee741-187">Můžete upravit *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* souboru změnou hodnoty z `AutoDetect` k `Https`.</span><span class="sxs-lookup"><span data-stu-id="ee741-187">You can modify the *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* file by changing the value from `AutoDetect` to `Https`.</span></span> <span data-ttu-id="ee741-188">Tento soubor se obvykle nachází ve *brána dat Files\On místní C:\Program*.</span><span class="sxs-lookup"><span data-stu-id="ee741-188">This file is typically located at *C:\Program Files\On-premises data gateway*.</span></span>

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <span data-ttu-id="ee741-189"><a name="faq"></a>Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="ee741-189"><a name="faq"></a>Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="ee741-190">Obecné</span><span class="sxs-lookup"><span data-stu-id="ee741-190">General</span></span>

<span data-ttu-id="ee741-191">**Q**: Potřebuji bránu pro zdroje dat v cloudu, jako je například Azure SQL Database?</span><span class="sxs-lookup"><span data-stu-id="ee741-191">**Q**: Do I need a gateway for data sources in the cloud, such as Azure SQL Database?</span></span> <br/><span data-ttu-id="ee741-192">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="ee741-192">
**A**: No.</span></span> <span data-ttu-id="ee741-193">Bránu se připojí k místním zdrojům dat pouze.</span><span class="sxs-lookup"><span data-stu-id="ee741-193">A gateway connects to on-premises data sources only.</span></span>

<span data-ttu-id="ee741-194">**Q**: nemá brány musí být instalovány na stejném počítači jako zdroj dat?</span><span class="sxs-lookup"><span data-stu-id="ee741-194">**Q**: Does the gateway have to be installed on the same machine as the data source?</span></span> <br/><span data-ttu-id="ee741-195">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="ee741-195">
**A**: No.</span></span> <span data-ttu-id="ee741-196">Se brána připojí ke zdroji dat pomocí informací o připojení, který byl poskytnut.</span><span class="sxs-lookup"><span data-stu-id="ee741-196">The gateway connects to the data source using the connection information that was provided.</span></span> <span data-ttu-id="ee741-197">Vezměte v úvahu brány jako klientskou aplikaci v tomto smyslu.</span><span class="sxs-lookup"><span data-stu-id="ee741-197">Consider the gateway as a client application in this sense.</span></span> <span data-ttu-id="ee741-198">Brány musí právě schopnost připojit k názvu serveru, který byl poskytnut, obvykle ve stejné síti.</span><span class="sxs-lookup"><span data-stu-id="ee741-198">The gateway just needs the capability to connect to the server name that was provided, typically on the same network.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="ee741-199">**Q**: Proč je nutné použít pracovní nebo školní účet pro přihlášení?</span><span class="sxs-lookup"><span data-stu-id="ee741-199">**Q**: Why do I need to use a work or school account to sign in?</span></span> <br/><span data-ttu-id="ee741-200">
**A**: pouze můžete použít Azure pracovní nebo školní účet, když instalujete místní brána data.</span><span class="sxs-lookup"><span data-stu-id="ee741-200">
**A**: You can only use an Azure work or school account when you install the on-premises data gateway.</span></span> <span data-ttu-id="ee741-201">Váš přihlašovací účet je uložený v klientovi, který je spravován pomocí služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ee741-201">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="ee741-202">Obvykle účet Azure AD hlavní název uživatele (UPN) odpovídá e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="ee741-202">Usually, your Azure AD account's user principal name (UPN) matches the email address.</span></span>

<span data-ttu-id="ee741-203">**Q**: uložení pověření?</span><span class="sxs-lookup"><span data-stu-id="ee741-203">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="ee741-204">
**A**: přihlašovací údaje, které zadáte pro zdroj dat jsou zašifrovány a uložené v cloudové službě brány.</span><span class="sxs-lookup"><span data-stu-id="ee741-204">
**A**: The credentials that you enter for a data source are encrypted and stored in the Gateway Cloud Service.</span></span> <span data-ttu-id="ee741-205">Přihlašovací údaje jsou v bráně místní data dešifrovat.</span><span class="sxs-lookup"><span data-stu-id="ee741-205">The credentials are decrypted at the on-premises data gateway.</span></span>

<span data-ttu-id="ee741-206">**Q**: existují všechny požadavky pro šířku pásma sítě?</span><span class="sxs-lookup"><span data-stu-id="ee741-206">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="ee741-207">
**A**: ho má doporučujeme síti připojení má dobrou propustnost.</span><span class="sxs-lookup"><span data-stu-id="ee741-207">
**A**: It's recommend your network connection has good throughput.</span></span> <span data-ttu-id="ee741-208">Každé prostředí je jiné a množství dat odesílaných ovlivňuje výsledky.</span><span class="sxs-lookup"><span data-stu-id="ee741-208">Every environment is different, and the amount of data being sent affects the results.</span></span> <span data-ttu-id="ee741-209">Pomocí ExpressRoute může pomoct zajistit úrovní propustnosti mezi místními a datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="ee741-209">Using ExpressRoute could help to guarantee a level of throughput between on-premises and the Azure datacenters.</span></span>
<span data-ttu-id="ee741-210">Můžete aplikaci Azure rychlost testovací nástroj třetí strany ke měřidla vaší propustnost.</span><span class="sxs-lookup"><span data-stu-id="ee741-210">You can use the third-party tool Azure Speed Test app to help gauge your throughput.</span></span>

<span data-ttu-id="ee741-211">**Q**: co je latence spouštět dotazy na zdroj dat z brány?</span><span class="sxs-lookup"><span data-stu-id="ee741-211">**Q**: What is the latency for running queries to a data source from the gateway?</span></span> <span data-ttu-id="ee741-212">Co je nejlepší architektura?</span><span class="sxs-lookup"><span data-stu-id="ee741-212">What is the best architecture?</span></span> <br/><span data-ttu-id="ee741-213">
**A**: Chcete-li snížit latenci sítě, nainstalujte bránu co nejblíže zdroji dat jako možné.</span><span class="sxs-lookup"><span data-stu-id="ee741-213">
**A**: To reduce network latency, install the gateway as close to the data source as possible.</span></span> <span data-ttu-id="ee741-214">Pokud nainstalujete brány na skutečná data zdroj tento blízkosti minimalizuje latenci zavedená.</span><span class="sxs-lookup"><span data-stu-id="ee741-214">If you can install the gateway on the actual data source, this proximity minimizes the latency introduced.</span></span> <span data-ttu-id="ee741-215">Příliš zvažte v datacentrech.</span><span class="sxs-lookup"><span data-stu-id="ee741-215">Consider the datacenters too.</span></span> <span data-ttu-id="ee741-216">Například pokud používá službu západní USA datacenter a vy musíte SQL Server hostované ve virtuálním počítači Azure, svého virtuálního počítače Azure musí být v západní USA příliš.</span><span class="sxs-lookup"><span data-stu-id="ee741-216">For example, if your service uses the West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in the West US too.</span></span> <span data-ttu-id="ee741-217">Tato blízkosti minimalizuje latenci a zabraňuje s nimi spojeným nákladům na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="ee741-217">This proximity minimizes latency and avoids egress charges on the Azure VM.</span></span>

<span data-ttu-id="ee741-218">**Q**: jak se výsledky odesílají zpět do cloudu?</span><span class="sxs-lookup"><span data-stu-id="ee741-218">**Q**: How are results sent back to the cloud?</span></span> <br/><span data-ttu-id="ee741-219">
**A**: výsledky se odesílají přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ee741-219">
**A**: Results are sent through the Azure Service Bus.</span></span>

<span data-ttu-id="ee741-220">**Q**: existují všechna příchozí připojení k bráně z cloudu?</span><span class="sxs-lookup"><span data-stu-id="ee741-220">**Q**: Are there any inbound connections to the gateway from the cloud?</span></span> <br/><span data-ttu-id="ee741-221">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="ee741-221">
**A**: No.</span></span> <span data-ttu-id="ee741-222">Brána používá odchozí připojení k Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ee741-222">The gateway uses outbound connections to Azure Service Bus.</span></span>

<span data-ttu-id="ee741-223">**Q**: Co když blokovat odchozí připojení?</span><span class="sxs-lookup"><span data-stu-id="ee741-223">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="ee741-224">Co je potřeba otevřít?</span><span class="sxs-lookup"><span data-stu-id="ee741-224">What do I need to open?</span></span> <br/><span data-ttu-id="ee741-225">
**A**: najdete v části Nastavení portů a hostitelů, které používá bránu.</span><span class="sxs-lookup"><span data-stu-id="ee741-225">
**A**: See the ports and hosts that the gateway uses.</span></span>

<span data-ttu-id="ee741-226">**Q**: co je aktuální služby Windows volána?</span><span class="sxs-lookup"><span data-stu-id="ee741-226">**Q**: What is the actual Windows service called?</span></span><br/><span data-ttu-id="ee741-227">
**A**: V služby brány se nazývá služba brány pro místní data.</span><span class="sxs-lookup"><span data-stu-id="ee741-227">
**A**: In Services, the gateway is called On-premises data gateway service.</span></span>

<span data-ttu-id="ee741-228">**Q**: můžete bránu služby systému Windows spustit pomocí účtu Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ee741-228">**Q**: Can the gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="ee741-229">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="ee741-229">
**A**: No.</span></span> <span data-ttu-id="ee741-230">Služba systému Windows musí mít platný účet systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ee741-230">The Windows service must have a valid Windows account.</span></span> <span data-ttu-id="ee741-231">Ve výchozím nastavení spouští službu s SID služby, NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="ee741-231">By default, the service runs with the Service SID, NT SERVICE\PBIEgwService.</span></span>

### <span data-ttu-id="ee741-232"><a name="high-availability"></a>Vysoká dostupnost a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="ee741-232"><a name="high-availability"></a>High availability and disaster recovery</span></span>

<span data-ttu-id="ee741-233">**Q**: jaké možnosti jsou dostupné pro zotavení po havárii?</span><span class="sxs-lookup"><span data-stu-id="ee741-233">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="ee741-234">
**A**: obnovovací klíč můžete použít k obnovení nebo přesunout bránu.</span><span class="sxs-lookup"><span data-stu-id="ee741-234">
**A**: You can use the recovery key to restore or move a gateway.</span></span> <span data-ttu-id="ee741-235">Při instalaci brány, zadejte obnovovací klíč.</span><span class="sxs-lookup"><span data-stu-id="ee741-235">When you install the gateway, specify the recovery key.</span></span>

<span data-ttu-id="ee741-236">**Q**: co je výhodou obnovovací klíč?</span><span class="sxs-lookup"><span data-stu-id="ee741-236">**Q**: What is the benefit of the recovery key?</span></span> <br/><span data-ttu-id="ee741-237">
**A**: obnovovací klíč poskytuje způsob, jak migrovat nebo obnovení po havárii nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="ee741-237">
**A**: The recovery key provides a way to migrate or recover your gateway settings after a disaster.</span></span>

## <span data-ttu-id="ee741-238"><a name="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ee741-238"><a name="troubleshooting"> </a>Troubleshooting</span></span>

<span data-ttu-id="ee741-239">**Q**: jak můžete zjistit, co jsou dotazy se posílá místní zdroj dat?</span><span class="sxs-lookup"><span data-stu-id="ee741-239">**Q**: How can I see what queries are being sent to the on-premises data source?</span></span> <br/><span data-ttu-id="ee741-240">
**A**: můžete povolit trasování dotazů, který obsahuje dotazy, které se odesílají.</span><span class="sxs-lookup"><span data-stu-id="ee741-240">
**A**: You can enable query tracing, which includes the queries that are sent.</span></span> <span data-ttu-id="ee741-241">Nezapomeňte změnit dotaz trasování zpět na původní hodnotu po dokončení odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="ee741-241">Remember to change query tracing back to the original value when done troubleshooting.</span></span> <span data-ttu-id="ee741-242">Trasování dotazů, které jsou zapnuté vytvoří větší protokoly.</span><span class="sxs-lookup"><span data-stu-id="ee741-242">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="ee741-243">Můžete také zobrazit nástroje, které má váš zdroj dat pro trasování dotazů.</span><span class="sxs-lookup"><span data-stu-id="ee741-243">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="ee741-244">Můžete například použít rozšířených událostí nebo profileru SQL pro SQL Server a služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="ee741-244">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="ee741-245">**Q**: kde jsou protokoly gateway?</span><span class="sxs-lookup"><span data-stu-id="ee741-245">**Q**: Where are the gateway logs?</span></span> <br/><span data-ttu-id="ee741-246">
**A**: viz protokoly později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="ee741-246">
**A**: See Logs later in this topic.</span></span>

### <span data-ttu-id="ee741-247"><a name="update"></a>Aktualizovat na nejnovější verzi</span><span class="sxs-lookup"><span data-stu-id="ee741-247"><a name="update"></a>Update to the latest version</span></span>

<span data-ttu-id="ee741-248">Mnohé problémy můžete surface, když se stane zastaralé verze brány.</span><span class="sxs-lookup"><span data-stu-id="ee741-248">Many issues can surface when the gateway version becomes outdated.</span></span> <span data-ttu-id="ee741-249">Jako vhodné obecné Ujistěte se, že používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="ee741-249">As good general practice, make sure that you use the latest version.</span></span> <span data-ttu-id="ee741-250">Pokud jste neprovedli aktualizaci brány pro měsíc i déle, můžete zvažte instalaci nejnovější verze brány a v tématu, pokud jste reprodukujte problém.</span><span class="sxs-lookup"><span data-stu-id="ee741-250">If you haven't updated the gateway for a month or longer, you might consider installing the latest version of the gateway, and see if you can reproduce the issue.</span></span>

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="ee741-251">Chyba: Nepodařilo se přidat uživatele do skupiny.</span><span class="sxs-lookup"><span data-stu-id="ee741-251">Error: Failed to add user to group.</span></span> <span data-ttu-id="ee741-252">(-2147463168 PBIEgwService výkonu protokolu uživatelů)</span><span class="sxs-lookup"><span data-stu-id="ee741-252">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="ee741-253">Může se tato chyba, pokud se pokusíte nainstalovat bránu na řadič domény, což není podporováno.</span><span class="sxs-lookup"><span data-stu-id="ee741-253">You might get this error if you try to install the gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="ee741-254">Ujistěte se, která můžete nasadit bránu na počítači, který není řadičem domény.</span><span class="sxs-lookup"><span data-stu-id="ee741-254">Make sure that you deploy the gateway on a machine that isn't a domain controller.</span></span>

## <span data-ttu-id="ee741-255"><a name="logs"></a>Protokoly</span><span class="sxs-lookup"><span data-stu-id="ee741-255"><a name="logs"></a>Logs</span></span>

<span data-ttu-id="ee741-256">Soubory protokolu jsou důležité prostředků při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="ee741-256">Log files are an important resource when troubleshooting.</span></span>

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="ee741-257">Protokoly služby Enterprise gateway</span><span class="sxs-lookup"><span data-stu-id="ee741-257">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a><span data-ttu-id="ee741-258">Konfigurace protokolů</span><span class="sxs-lookup"><span data-stu-id="ee741-258">Configuration logs</span></span>

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a><span data-ttu-id="ee741-259">Protokoly událostí</span><span class="sxs-lookup"><span data-stu-id="ee741-259">Event logs</span></span>

<span data-ttu-id="ee741-260">Můžete najít v protokolech Brána pro správu dat a PowerBIGateway pod **protokoly aplikací a služeb**.</span><span class="sxs-lookup"><span data-stu-id="ee741-260">You can find the Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>


## <span data-ttu-id="ee741-261"><a name="telemetry"></a>Telemetrie</span><span class="sxs-lookup"><span data-stu-id="ee741-261"><a name="telemetry"></a>Telemetry</span></span>
<span data-ttu-id="ee741-262">Telemetrická data lze použít pro monitorování a řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="ee741-262">Telemetry can be used for monitoring and troubleshooting.</span></span> <span data-ttu-id="ee741-263">Ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="ee741-263">By default</span></span>

<span data-ttu-id="ee741-264">**Chcete-li zapnout telemetrii**</span><span class="sxs-lookup"><span data-stu-id="ee741-264">**To turn on telemetry**</span></span>

1.  <span data-ttu-id="ee741-265">Zkontrolujte místní adresář klienta brány dat v počítači.</span><span class="sxs-lookup"><span data-stu-id="ee741-265">Check the On-premises data gateway client directory on the computer.</span></span> <span data-ttu-id="ee741-266">Obvykle je **%systemdrive%\Program Files\On místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="ee741-266">Typically, it is **%systemdrive%\Program Files\On-premises data gateway**.</span></span> <span data-ttu-id="ee741-267">Nebo můžete spustit konzolu služby a zkontrolujte cestu ke spustitelnému souboru: vlastnost službu brány místní data.</span><span class="sxs-lookup"><span data-stu-id="ee741-267">Or, you can open a Services console and check the Path to executable: A property of the On-premises data gateway service.</span></span>
2.  <span data-ttu-id="ee741-268">V souboru Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config z adresáře klienta.</span><span class="sxs-lookup"><span data-stu-id="ee741-268">In the Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config file from client directory.</span></span> <span data-ttu-id="ee741-269">Nastavení SendTelemetry změňte na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="ee741-269">Change the SendTelemetry setting to true.</span></span>
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  <span data-ttu-id="ee741-270">Uložte změny a restartovat službu systému Windows: místní služba brány pro data.</span><span class="sxs-lookup"><span data-stu-id="ee741-270">Save your changes and restart the Windows service: On-premises data gateway service.</span></span>




## <a name="next-steps"></a><span data-ttu-id="ee741-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee741-271">Next steps</span></span>
* [<span data-ttu-id="ee741-272">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="ee741-272">Manage Analysis Services</span></span>](analysis-services-manage.md)
* [<span data-ttu-id="ee741-273">Získání dat z Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="ee741-273">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
