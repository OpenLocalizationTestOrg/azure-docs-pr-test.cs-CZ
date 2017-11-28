---
title: "Brána dat aaaOn místní | Microsoft Docs"
description: "Bránu místní je nezbytný, pokud váš server služby Analysis Services v Azure se budou připojovat tooon místní datové zdroje."
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
ms.openlocfilehash: fc7b9c69e6f81b41deb7a5d6d963225593845d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-tooon-premises-data-sources-with-azure-on-premises-data-gateway"></a><span data-ttu-id="70cdf-103">Připojování tooon místní zdroje dat pomocí Azure místní brány dat</span><span class="sxs-lookup"><span data-stu-id="70cdf-103">Connecting tooon-premises data sources with Azure On-premises Data Gateway</span></span>
<span data-ttu-id="70cdf-104">Brána dat místní Hello funguje jako mostu, zajištění zabezpečených dat přenos mezi místní zdroje dat a vaše servery Azure Analysis Services v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-104">hello on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in hello cloud.</span></span> <span data-ttu-id="70cdf-105">V přidání tooworking s více servery Azure Analysis Services v hello stejné oblasti, hello nejnovější verzi této brány hello taky spolupracuje se službou Azure Logic Apps, Power BI, Power aplikace a Flow společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="70cdf-105">In addition tooworking with multiple Azure Analysis Services servers in hello same region, hello latest version of hello gateway also works with Azure Logic Apps, Power BI, Power Apps, and Microsoft Flow.</span></span> <span data-ttu-id="70cdf-106">Můžete přidružit více služeb v hello stejné oblasti s jednou bránou.</span><span class="sxs-lookup"><span data-stu-id="70cdf-106">You can associate multiple services in hello same region with a single gateway.</span></span> 

 <span data-ttu-id="70cdf-107">Azure Analysis Services vyžaduje prostředek brány v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="70cdf-107">Azure Analysis Services requires a gateway resource in hello same region.</span></span> <span data-ttu-id="70cdf-108">Například pokud máte servery Azure Analysis Services v oblasti Východ USA 2 hello, musíte prostředek brány v oblasti Východ USA 2 hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-108">For example, if you have Azure Analysis Services servers in hello East US 2 region, you need a gateway resource in hello East US 2 region.</span></span> <span data-ttu-id="70cdf-109">Několik serverů v oblasti Východ USA 2 můžete použít hello stejné bráně.</span><span class="sxs-lookup"><span data-stu-id="70cdf-109">Multiple servers in East US 2 can use hello same gateway.</span></span>

<span data-ttu-id="70cdf-110">Získávání nastavení s hello brány hello poprvé je proces, který sestávající ze čtyř částí:</span><span class="sxs-lookup"><span data-stu-id="70cdf-110">Getting setup with hello gateway hello first time is a four-part process:</span></span>

- <span data-ttu-id="70cdf-111">**Stáhněte a spusťte instalační program** – tento krok nainstaluje služba brány do počítače ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="70cdf-111">**Download and run setup** - This step installs a gateway service on a computer in your organization.</span></span>

- <span data-ttu-id="70cdf-112">**Zaregistrujte bránu** – v tomto kroku, zadejte název a obnovení klíče pro bránu a vyberte oblast, hello cloudové službě Brána pro registraci brány.</span><span class="sxs-lookup"><span data-stu-id="70cdf-112">**Register your gateway** - In this step, you specify a name and recovery key for your gateway, and select a region, registering your gateway with hello Gateway Cloud Service.</span></span>

- <span data-ttu-id="70cdf-113">**Vytvořte prostředek brány v Azure** – v tomto kroku vytvoříte bránu prostředků ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="70cdf-113">**Create a gateway resource in Azure** - In this step, you create a gateway resource in your Azure subscription.</span></span>

- <span data-ttu-id="70cdf-114">**Připojte prostředek brány tooyour vaše servery** – až budete mít bránu prostředků v rámci vašeho předplatného, můžete začít připojení tooit vaše servery.</span><span class="sxs-lookup"><span data-stu-id="70cdf-114">**Connect your servers tooyour gateway resource** - Once you have a gateway resource in your subscription, you can begin connecting your servers tooit.</span></span>

<span data-ttu-id="70cdf-115">Až budete mít bránu prostředek nakonfigurovaný pro vaše předplatné, se můžete připojit víc serverů a dalších služeb tooit.</span><span class="sxs-lookup"><span data-stu-id="70cdf-115">Once you have a gateway resource configured for your subscription, you can connect multiple servers, and other services tooit.</span></span> <span data-ttu-id="70cdf-116">Pouze potřebovat tooinstall jinou bránu a vytvářet prostředky další brány, pokud máte servery nebo jiné služby v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="70cdf-116">You only need tooinstall a different gateway and create additional gateway resources if you have servers or other services in a different region.</span></span>

<span data-ttu-id="70cdf-117">tooget spustit hned, najdete v části [nainstalujte a nakonfigurujte místní brána dat](analysis-services-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="70cdf-117">tooget started right away, see [Install and configure on-premises data gateway](analysis-services-gateway-install.md).</span></span>

## <span data-ttu-id="70cdf-118"><a name="how-it-works"></a>Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="70cdf-118"><a name="how-it-works"> </a>How it works</span></span>
<span data-ttu-id="70cdf-119">Brána Hello nainstalujete na počítač ve vaší organizaci používá jako služby systému Windows, **místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="70cdf-119">hello gateway you install on a computer in your organization runs as a Windows service, **On-premises data gateway**.</span></span> <span data-ttu-id="70cdf-120">Tato místní služba není zaregistrována hello cloudové službě Brána pro přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="70cdf-120">This local service is registered with hello Gateway Cloud Service through Azure Service Bus.</span></span> <span data-ttu-id="70cdf-121">Pak vytvořte prostředek brány cloudové službě brány pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="70cdf-121">You then create a gateway resource Gateway Cloud Service for your Azure subscription.</span></span> <span data-ttu-id="70cdf-122">Vaše Azure Analysis Services, které jsou potom servery připojené tooyour prostředek brány.</span><span class="sxs-lookup"><span data-stu-id="70cdf-122">Your Azure Analysis Services servers are then connected tooyour gateway resource.</span></span> <span data-ttu-id="70cdf-123">Když modely na váš server nutné tooconnect tooyour místní zdroje dat pro dotazy nebo zpracování, hello dotaz a datové toku traverses hello brány prostředku, Azure Service Bus, místní služba Brána dat a zdrojům dat.</span><span class="sxs-lookup"><span data-stu-id="70cdf-123">When models on your server need tooconnect tooyour on-premises data sources for queries or processing, a query and data flow traverses hello gateway resource, Azure Service Bus, hello local on-premises data gateway service, and your data sources.</span></span> 

![Jak to funguje](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

<span data-ttu-id="70cdf-125">Tok dotazy a data:</span><span class="sxs-lookup"><span data-stu-id="70cdf-125">Queries and data flow:</span></span>

1. <span data-ttu-id="70cdf-126">Dotaz byl vytvořený hello cloudové služby s hello šifrovat přihlašovací údaje pro zdroj dat pro místní hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-126">A query is created by hello cloud service with hello encrypted credentials for hello on-premises data source.</span></span> <span data-ttu-id="70cdf-127">Potom odeslal tooa fronty pro tooprocess hello brány.</span><span class="sxs-lookup"><span data-stu-id="70cdf-127">It's then sent tooa queue for hello gateway tooprocess.</span></span>
2. <span data-ttu-id="70cdf-128">cloudové službě Brána pro Hello analyzuje hello dotazu a nabízených oznámení hello požadavek toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="70cdf-128">hello gateway cloud service analyzes hello query and pushes hello request toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span></span>
3. <span data-ttu-id="70cdf-129">Brána dat místní Hello dotazuje hello Azure Service Bus pro žádosti čekající na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="70cdf-129">hello on-premises data gateway polls hello Azure Service Bus for pending requests.</span></span>
4. <span data-ttu-id="70cdf-130">Brána Hello získá hello dotazu, dešifruje hello přihlašovací údaje a připojí toohello zdroje dat pomocí těchto přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="70cdf-130">hello gateway gets hello query, decrypts hello credentials, and connects toohello data sources with those credentials.</span></span>
5. <span data-ttu-id="70cdf-131">Brána Hello odešle datový zdroj toohello hello dotazu pro provedení.</span><span class="sxs-lookup"><span data-stu-id="70cdf-131">hello gateway sends hello query toohello data source for execution.</span></span>
6. <span data-ttu-id="70cdf-132">výsledky Hello se odesílají ze zdroje dat hello, back toohello brány a pak na hello Cloudová služba a serverem.</span><span class="sxs-lookup"><span data-stu-id="70cdf-132">hello results are sent from hello data source, back toohello gateway, and then onto hello cloud service and your server.</span></span>

## <span data-ttu-id="70cdf-133"><a name="windows-service-account"></a>Účtu služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="70cdf-133"><a name="windows-service-account"> </a>Windows Service account</span></span>
<span data-ttu-id="70cdf-134">Hello místní brána dat je nakonfigurované toouse *NT SERVICE\PBIEgwService* pro přihlašovací pověření služby systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-134">hello on-premises data gateway is configured toouse *NT SERVICE\PBIEgwService* for hello Windows service logon credential.</span></span> <span data-ttu-id="70cdf-135">Ve výchozím nastavení má hello vpravo přihlášení jako služba; v kontextu hello hello počítače, který instalujete na hello brány.</span><span class="sxs-lookup"><span data-stu-id="70cdf-135">By default, it has hello right of Logon as a service; in hello context of hello machine that you are installing hello gateway on.</span></span> <span data-ttu-id="70cdf-136">Tento přihlašovací údaj není hello stejné zdroje dat tooon místní účet používaný tooconnect nebo účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="70cdf-136">This credential is not hello same account used tooconnect tooon-premises data sources or your Azure account.</span></span>  

<span data-ttu-id="70cdf-137">Pokud narazíte na potíže s proxy serveru z důvodu tooauthentication, může být vhodné toochange hello Windows služby účet uživatele domény tooa nebo spravovaný účet služby.</span><span class="sxs-lookup"><span data-stu-id="70cdf-137">If you encounter issues with your proxy server due tooauthentication, you may want toochange hello Windows service account tooa domain user or managed service account.</span></span>

## <span data-ttu-id="70cdf-138"><a name="ports"></a>Porty</span><span class="sxs-lookup"><span data-stu-id="70cdf-138"><a name="ports"> </a>Ports</span></span>
<span data-ttu-id="70cdf-139">Hello brány vytvoří tooAzure odchozí připojení k Service Bus.</span><span class="sxs-lookup"><span data-stu-id="70cdf-139">hello gateway creates an outbound connection tooAzure Service Bus.</span></span> <span data-ttu-id="70cdf-140">Komunikuje na odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 prostřednictvím 9354.</span><span class="sxs-lookup"><span data-stu-id="70cdf-140">It communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span>  <span data-ttu-id="70cdf-141">Brána Hello nevyžaduje příchozí porty.</span><span class="sxs-lookup"><span data-stu-id="70cdf-141">hello gateway does not require inbound ports.</span></span>

<span data-ttu-id="70cdf-142">Doporučujeme povolených hello IP adresy pro vaši oblast dat v bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="70cdf-142">We recommend you whitelist hello IP addresses for your data region in your firewall.</span></span> <span data-ttu-id="70cdf-143">Můžete si stáhnout hello [seznamu Microsoft Azure Datacenter IP](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="70cdf-143">You can download hello [Microsoft Azure Datacenter IP list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="70cdf-144">Tento seznam je aktualizovaný týdně.</span><span class="sxs-lookup"><span data-stu-id="70cdf-144">This list is updated weekly.</span></span>

> [!NOTE]
> <span data-ttu-id="70cdf-145">v notaci CIDR, jsou uvedené v seznamu IP Datacentra Azure hello Hello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="70cdf-145">hello IP Addresses listed in hello Azure Datacenter IP list are in CIDR notation.</span></span> <span data-ttu-id="70cdf-146">Například 10.0.0.0/24 neznamená 10.0.0.0 prostřednictvím 10.0.0.24.</span><span class="sxs-lookup"><span data-stu-id="70cdf-146">For example, 10.0.0.0/24 does not mean 10.0.0.0 through 10.0.0.24.</span></span> <span data-ttu-id="70cdf-147">Další informace o hello [notaci CIDR](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="70cdf-147">Learn more about hello [CIDR notation](http://whatismyipaddress.com/cidr).</span></span>
>
>

<span data-ttu-id="70cdf-148">Hello následují hello plně kvalifikovaný názvy domén používat hello gateway.</span><span class="sxs-lookup"><span data-stu-id="70cdf-148">hello following are hello fully qualified domain names used by hello gateway.</span></span>

| <span data-ttu-id="70cdf-149">Názvy domén</span><span class="sxs-lookup"><span data-stu-id="70cdf-149">Domain names</span></span> | <span data-ttu-id="70cdf-150">Odchozí porty</span><span class="sxs-lookup"><span data-stu-id="70cdf-150">Outbound ports</span></span> | <span data-ttu-id="70cdf-151">Popis</span><span class="sxs-lookup"><span data-stu-id="70cdf-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="70cdf-152">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="70cdf-152">*.powerbi.com</span></span> |<span data-ttu-id="70cdf-153">80</span><span class="sxs-lookup"><span data-stu-id="70cdf-153">80</span></span> |<span data-ttu-id="70cdf-154">Instalační program toodownload hello používá protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="70cdf-154">HTTP used toodownload hello installer.</span></span> |
| <span data-ttu-id="70cdf-155">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="70cdf-155">*.powerbi.com</span></span> |<span data-ttu-id="70cdf-156">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-156">443</span></span> |<span data-ttu-id="70cdf-157">HTTPS</span><span class="sxs-lookup"><span data-stu-id="70cdf-157">HTTPS</span></span> |
| <span data-ttu-id="70cdf-158">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="70cdf-158">*.analysis.windows.net</span></span> |<span data-ttu-id="70cdf-159">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-159">443</span></span> |<span data-ttu-id="70cdf-160">HTTPS</span><span class="sxs-lookup"><span data-stu-id="70cdf-160">HTTPS</span></span> |
| <span data-ttu-id="70cdf-161">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="70cdf-161">*.login.windows.net</span></span> |<span data-ttu-id="70cdf-162">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-162">443</span></span> |<span data-ttu-id="70cdf-163">HTTPS</span><span class="sxs-lookup"><span data-stu-id="70cdf-163">HTTPS</span></span> |
| <span data-ttu-id="70cdf-164">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="70cdf-164">*.servicebus.windows.net</span></span> |<span data-ttu-id="70cdf-165">5671-5672</span><span class="sxs-lookup"><span data-stu-id="70cdf-165">5671-5672</span></span> |<span data-ttu-id="70cdf-166">Pokročilé zpráv služby Řízení front Protocol (AMQP)</span><span class="sxs-lookup"><span data-stu-id="70cdf-166">Advanced Message Queuing Protocol (AMQP)</span></span> |
| <span data-ttu-id="70cdf-167">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="70cdf-167">*.servicebus.windows.net</span></span> |<span data-ttu-id="70cdf-168">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="70cdf-168">443, 9350-9354</span></span> |<span data-ttu-id="70cdf-169">Moduly pro naslouchání na předávání přes Service Bus přes TCP (vyžaduje 443 pro získání tokenu řízení přístupu)</span><span class="sxs-lookup"><span data-stu-id="70cdf-169">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> |
| <span data-ttu-id="70cdf-170">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="70cdf-170">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="70cdf-171">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-171">443</span></span> |<span data-ttu-id="70cdf-172">HTTPS</span><span class="sxs-lookup"><span data-stu-id="70cdf-172">HTTPS</span></span> |
| <span data-ttu-id="70cdf-173">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="70cdf-173">*.core.windows.net</span></span> |<span data-ttu-id="70cdf-174">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-174">443</span></span> |<span data-ttu-id="70cdf-175">HTTPS</span><span class="sxs-lookup"><span data-stu-id="70cdf-175">HTTPS</span></span> |
| <span data-ttu-id="70cdf-176">Login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="70cdf-176">login.microsoftonline.com</span></span> |<span data-ttu-id="70cdf-177">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-177">443</span></span> |<span data-ttu-id="70cdf-178">HTTPS</span><span class="sxs-lookup"><span data-stu-id="70cdf-178">HTTPS</span></span> |
| <span data-ttu-id="70cdf-179">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="70cdf-179">*.msftncsi.com</span></span> |<span data-ttu-id="70cdf-180">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-180">443</span></span> |<span data-ttu-id="70cdf-181">Připojení k Internetu tootest použít, pokud brána hello nedostupná pro hello služby Power BI.</span><span class="sxs-lookup"><span data-stu-id="70cdf-181">Used tootest internet connectivity if hello gateway is unreachable by hello Power BI service.</span></span> |
| <span data-ttu-id="70cdf-182">*.microsoftonline p.com</span><span class="sxs-lookup"><span data-stu-id="70cdf-182">*.microsoftonline-p.com</span></span> |<span data-ttu-id="70cdf-183">443</span><span class="sxs-lookup"><span data-stu-id="70cdf-183">443</span></span> |<span data-ttu-id="70cdf-184">Slouží k ověření v závislosti na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="70cdf-184">Used for authentication depending on configuration.</span></span> |

### <span data-ttu-id="70cdf-185"><a name="force-https"></a>Vynucení komunikaci přes protokol HTTPS s Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="70cdf-185"><a name="force-https"></a>Forcing HTTPS communication with Azure Service Bus</span></span>
<span data-ttu-id="70cdf-186">Hello brány toocommunicate s Azure Service Bus můžete vynutit pomocí protokolu HTTPS místo přímé TCP; ale to tak může výrazně snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="70cdf-186">You can force hello gateway toocommunicate with Azure Service Bus by using HTTPS instead of direct TCP; however, doing so can greatly reduce performance.</span></span> <span data-ttu-id="70cdf-187">Můžete upravit hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* souboru tak, že změníte hodnotu hello z `AutoDetect` příliš`Https`.</span><span class="sxs-lookup"><span data-stu-id="70cdf-187">You can modify hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* file by changing hello value from `AutoDetect` too`Https`.</span></span> <span data-ttu-id="70cdf-188">Tento soubor se obvykle nachází ve *brána dat Files\On místní C:\Program*.</span><span class="sxs-lookup"><span data-stu-id="70cdf-188">This file is typically located at *C:\Program Files\On-premises data gateway*.</span></span>

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <span data-ttu-id="70cdf-189"><a name="faq"></a>Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="70cdf-189"><a name="faq"></a>Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="70cdf-190">Obecné</span><span class="sxs-lookup"><span data-stu-id="70cdf-190">General</span></span>

<span data-ttu-id="70cdf-191">**Q**: Potřebuji bránu pro zdroje dat v cloudu hello, jako je například Azure SQL Database?</span><span class="sxs-lookup"><span data-stu-id="70cdf-191">**Q**: Do I need a gateway for data sources in hello cloud, such as Azure SQL Database?</span></span> <br/><span data-ttu-id="70cdf-192">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="70cdf-192">
**A**: No.</span></span> <span data-ttu-id="70cdf-193">Brána se připojuje pouze zdroje dat tooon místní.</span><span class="sxs-lookup"><span data-stu-id="70cdf-193">A gateway connects tooon-premises data sources only.</span></span>

<span data-ttu-id="70cdf-194">**Q**: nemá hello bránu nainstalovat na stejný počítač jako zdroj dat hello hello toobe?</span><span class="sxs-lookup"><span data-stu-id="70cdf-194">**Q**: Does hello gateway have toobe installed on hello same machine as hello data source?</span></span> <br/><span data-ttu-id="70cdf-195">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="70cdf-195">
**A**: No.</span></span> <span data-ttu-id="70cdf-196">Brána Hello připojí toohello zdroje dat pomocí hello informace o připojení, který byl poskytnut.</span><span class="sxs-lookup"><span data-stu-id="70cdf-196">hello gateway connects toohello data source using hello connection information that was provided.</span></span> <span data-ttu-id="70cdf-197">Vezměte v úvahu hello brány jako klientskou aplikaci v tomto smyslu.</span><span class="sxs-lookup"><span data-stu-id="70cdf-197">Consider hello gateway as a client application in this sense.</span></span> <span data-ttu-id="70cdf-198">Hello právě musí brána hello schopností tooconnect toohello název serveru, který byl poskytnut, obvykle na hello stejné síti.</span><span class="sxs-lookup"><span data-stu-id="70cdf-198">hello gateway just needs hello capability tooconnect toohello server name that was provided, typically on hello same network.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="70cdf-199">**Q**: Proč potřebovat toouse pracovní nebo školní účet toosign v?</span><span class="sxs-lookup"><span data-stu-id="70cdf-199">**Q**: Why do I need toouse a work or school account toosign in?</span></span> <br/><span data-ttu-id="70cdf-200">
**A**: pouze můžete použít Azure pracovní nebo školní účet, když instalujete hello místní data gateway.</span><span class="sxs-lookup"><span data-stu-id="70cdf-200">
**A**: You can only use an Azure work or school account when you install hello on-premises data gateway.</span></span> <span data-ttu-id="70cdf-201">Váš přihlašovací účet je uložený v klientovi, který je spravován pomocí služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="70cdf-201">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="70cdf-202">Obvykle účet Azure AD hlavní název uživatele (UPN) odpovídá hello e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="70cdf-202">Usually, your Azure AD account's user principal name (UPN) matches hello email address.</span></span>

<span data-ttu-id="70cdf-203">**Q**: uložení pověření?</span><span class="sxs-lookup"><span data-stu-id="70cdf-203">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="70cdf-204">
**A**: hello přihlašovací údaje, které zadáte pro zdroj dat jsou zašifrovány a uloženy v hello cloudové službě Brána pro.</span><span class="sxs-lookup"><span data-stu-id="70cdf-204">
**A**: hello credentials that you enter for a data source are encrypted and stored in hello Gateway Cloud Service.</span></span> <span data-ttu-id="70cdf-205">přihlašovací údaje Hello se dešifrují v hello místní data gateway.</span><span class="sxs-lookup"><span data-stu-id="70cdf-205">hello credentials are decrypted at hello on-premises data gateway.</span></span>

<span data-ttu-id="70cdf-206">**Q**: existují všechny požadavky pro šířku pásma sítě?</span><span class="sxs-lookup"><span data-stu-id="70cdf-206">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="70cdf-207">
**A**: ho má doporučujeme síti připojení má dobrou propustnost.</span><span class="sxs-lookup"><span data-stu-id="70cdf-207">
**A**: It's recommend your network connection has good throughput.</span></span> <span data-ttu-id="70cdf-208">Každé prostředí je jiné a hello množství dat odesílaných ovlivňuje hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="70cdf-208">Every environment is different, and hello amount of data being sent affects hello results.</span></span> <span data-ttu-id="70cdf-209">Pomocí ExpressRoute může pomoct tooguarantee úrovní propustnosti mezi místními a hello datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="70cdf-209">Using ExpressRoute could help tooguarantee a level of throughput between on-premises and hello Azure datacenters.</span></span>
<span data-ttu-id="70cdf-210">Můžete vytvořit hello nástroj třetí strany Azure rychlost testovací aplikace toohelp měřidla vaší propustnost.</span><span class="sxs-lookup"><span data-stu-id="70cdf-210">You can use hello third-party tool Azure Speed Test app toohelp gauge your throughput.</span></span>

<span data-ttu-id="70cdf-211">**Q**: co je hello latence pro zdroj dat tooa spuštěné dotazy z brány hello?</span><span class="sxs-lookup"><span data-stu-id="70cdf-211">**Q**: What is hello latency for running queries tooa data source from hello gateway?</span></span> <span data-ttu-id="70cdf-212">Co je nejlepší architektura hello?</span><span class="sxs-lookup"><span data-stu-id="70cdf-212">What is hello best architecture?</span></span> <br/><span data-ttu-id="70cdf-213">
**A**: tooreduce latence sítě, brána hello instalace jako zdroj dat zavřít toohello nejdříve.</span><span class="sxs-lookup"><span data-stu-id="70cdf-213">
**A**: tooreduce network latency, install hello gateway as close toohello data source as possible.</span></span> <span data-ttu-id="70cdf-214">Pokud nainstalujete hello brány na zdroj dat skutečné hello tento blízkosti minimalizuje latenci hello zavedená.</span><span class="sxs-lookup"><span data-stu-id="70cdf-214">If you can install hello gateway on hello actual data source, this proximity minimizes hello latency introduced.</span></span> <span data-ttu-id="70cdf-215">Zvažte příliš hello datových centrech.</span><span class="sxs-lookup"><span data-stu-id="70cdf-215">Consider hello datacenters too.</span></span> <span data-ttu-id="70cdf-216">Například pokud používá službu hello západní USA datacenter a vy musíte SQL Server hostované ve virtuálním počítači Azure, svého virtuálního počítače Azure musí být v hello západní USA příliš.</span><span class="sxs-lookup"><span data-stu-id="70cdf-216">For example, if your service uses hello West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in hello West US too.</span></span> <span data-ttu-id="70cdf-217">Tato blízkosti minimalizuje latenci a zabraňuje s nimi spojeným nákladům na hello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="70cdf-217">This proximity minimizes latency and avoids egress charges on hello Azure VM.</span></span>

<span data-ttu-id="70cdf-218">**Q**: jak jsou výsledky odeslána zpět toohello cloudu?</span><span class="sxs-lookup"><span data-stu-id="70cdf-218">**Q**: How are results sent back toohello cloud?</span></span> <br/><span data-ttu-id="70cdf-219">
**A**: výsledky se odesílají přes hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="70cdf-219">
**A**: Results are sent through hello Azure Service Bus.</span></span>

<span data-ttu-id="70cdf-220">**Q**: existují jakékoli brány toohello příchozí připojení z cloudu hello?</span><span class="sxs-lookup"><span data-stu-id="70cdf-220">**Q**: Are there any inbound connections toohello gateway from hello cloud?</span></span> <br/><span data-ttu-id="70cdf-221">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="70cdf-221">
**A**: No.</span></span> <span data-ttu-id="70cdf-222">Brána Hello používá tooAzure odchozí připojení služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="70cdf-222">hello gateway uses outbound connections tooAzure Service Bus.</span></span>

<span data-ttu-id="70cdf-223">**Q**: Co když blokovat odchozí připojení?</span><span class="sxs-lookup"><span data-stu-id="70cdf-223">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="70cdf-224">Co dělat, je potřeba tooopen?</span><span class="sxs-lookup"><span data-stu-id="70cdf-224">What do I need tooopen?</span></span> <br/><span data-ttu-id="70cdf-225">
**A**: najdete v části hello porty a hostitelů, které hello používá bránu.</span><span class="sxs-lookup"><span data-stu-id="70cdf-225">
**A**: See hello ports and hosts that hello gateway uses.</span></span>

<span data-ttu-id="70cdf-226">**Q**: co je skutečný služba systému Windows hello volána?</span><span class="sxs-lookup"><span data-stu-id="70cdf-226">**Q**: What is hello actual Windows service called?</span></span><br/><span data-ttu-id="70cdf-227">
**A**: V služeb hello brána nazývá služba brány pro místní data.</span><span class="sxs-lookup"><span data-stu-id="70cdf-227">
**A**: In Services, hello gateway is called On-premises data gateway service.</span></span>

<span data-ttu-id="70cdf-228">**Q**: můžete hello služba Windows Brána pro spuštění pomocí účtu Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="70cdf-228">**Q**: Can hello gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="70cdf-229">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="70cdf-229">
**A**: No.</span></span> <span data-ttu-id="70cdf-230">služba systému Windows Hello musí mít platný účet systému Windows.</span><span class="sxs-lookup"><span data-stu-id="70cdf-230">hello Windows service must have a valid Windows account.</span></span> <span data-ttu-id="70cdf-231">Ve výchozím nastavení spouští hello služby s hello SID služby NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="70cdf-231">By default, hello service runs with hello Service SID, NT SERVICE\PBIEgwService.</span></span>

### <span data-ttu-id="70cdf-232"><a name="high-availability"></a>Vysoká dostupnost a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="70cdf-232"><a name="high-availability"></a>High availability and disaster recovery</span></span>

<span data-ttu-id="70cdf-233">**Q**: jaké možnosti jsou dostupné pro zotavení po havárii?</span><span class="sxs-lookup"><span data-stu-id="70cdf-233">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="70cdf-234">
**A**: můžete použít hello obnovení klíče toorestore nebo přesunout bránu.</span><span class="sxs-lookup"><span data-stu-id="70cdf-234">
**A**: You can use hello recovery key toorestore or move a gateway.</span></span> <span data-ttu-id="70cdf-235">Když instalujete hello brány, zadejte hello obnovovací klíč.</span><span class="sxs-lookup"><span data-stu-id="70cdf-235">When you install hello gateway, specify hello recovery key.</span></span>

<span data-ttu-id="70cdf-236">**Q**: co je hello výhodou hello obnovovací klíč?</span><span class="sxs-lookup"><span data-stu-id="70cdf-236">**Q**: What is hello benefit of hello recovery key?</span></span> <br/><span data-ttu-id="70cdf-237">
**A**: hello obnovovací klíč poskytuje způsob toomigrate nebo obnovení po havárii nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="70cdf-237">
**A**: hello recovery key provides a way toomigrate or recover your gateway settings after a disaster.</span></span>

## <span data-ttu-id="70cdf-238"><a name="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="70cdf-238"><a name="troubleshooting"> </a>Troubleshooting</span></span>

<span data-ttu-id="70cdf-239">**Q**: Jak můžu zjistit, jaké dotazy jsou odesílány zdroj dat pro místní toohello?</span><span class="sxs-lookup"><span data-stu-id="70cdf-239">**Q**: How can I see what queries are being sent toohello on-premises data source?</span></span> <br/><span data-ttu-id="70cdf-240">
**A**: můžete povolit trasování dotazů, což zahrnuje hello dotazy, které se odesílají.</span><span class="sxs-lookup"><span data-stu-id="70cdf-240">
**A**: You can enable query tracing, which includes hello queries that are sent.</span></span> <span data-ttu-id="70cdf-241">Mějte na paměti, toochange dotazu trasování zpět toohello původní hodnotu po dokončení odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="70cdf-241">Remember toochange query tracing back toohello original value when done troubleshooting.</span></span> <span data-ttu-id="70cdf-242">Trasování dotazů, které jsou zapnuté vytvoří větší protokoly.</span><span class="sxs-lookup"><span data-stu-id="70cdf-242">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="70cdf-243">Můžete také zobrazit nástroje, které má váš zdroj dat pro trasování dotazů.</span><span class="sxs-lookup"><span data-stu-id="70cdf-243">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="70cdf-244">Můžete například použít rozšířených událostí nebo profileru SQL pro SQL Server a služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="70cdf-244">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="70cdf-245">**Q**: kde jsou protokoly hello gateway?</span><span class="sxs-lookup"><span data-stu-id="70cdf-245">**Q**: Where are hello gateway logs?</span></span> <br/><span data-ttu-id="70cdf-246">
**A**: viz protokoly později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="70cdf-246">
**A**: See Logs later in this topic.</span></span>

### <span data-ttu-id="70cdf-247"><a name="update"></a>Aktualizace toohello nejnovější verzi</span><span class="sxs-lookup"><span data-stu-id="70cdf-247"><a name="update"></a>Update toohello latest version</span></span>

<span data-ttu-id="70cdf-248">Mnohé problémy můžete surface při verze brány hello stanou zastaralými.</span><span class="sxs-lookup"><span data-stu-id="70cdf-248">Many issues can surface when hello gateway version becomes outdated.</span></span> <span data-ttu-id="70cdf-249">Jako vhodné obecné Ujistěte se, že používáte nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-249">As good general practice, make sure that you use hello latest version.</span></span> <span data-ttu-id="70cdf-250">Pokud jste neprovedli aktualizaci brány hello dobu jednoho měsíce nebo déle, můžete zvažte instalaci hello nejnovější verzi této brány hello a v tématu, pokud jste reprodukujte problém hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-250">If you haven't updated hello gateway for a month or longer, you might consider installing hello latest version of hello gateway, and see if you can reproduce hello issue.</span></span>

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="70cdf-251">Chyba: Nepodařilo toogroup tooadd uživatele.</span><span class="sxs-lookup"><span data-stu-id="70cdf-251">Error: Failed tooadd user toogroup.</span></span> <span data-ttu-id="70cdf-252">(-2147463168 PBIEgwService výkonu protokolu uživatelů)</span><span class="sxs-lookup"><span data-stu-id="70cdf-252">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="70cdf-253">Může se tato chyba, pokud se pokusíte tooinstall hello brány na řadiči domény, což není podporováno.</span><span class="sxs-lookup"><span data-stu-id="70cdf-253">You might get this error if you try tooinstall hello gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="70cdf-254">Ujistěte se, že nasazujete hello brána na počítači, který není řadičem domény.</span><span class="sxs-lookup"><span data-stu-id="70cdf-254">Make sure that you deploy hello gateway on a machine that isn't a domain controller.</span></span>

## <span data-ttu-id="70cdf-255"><a name="logs"></a>Protokoly</span><span class="sxs-lookup"><span data-stu-id="70cdf-255"><a name="logs"></a>Logs</span></span>

<span data-ttu-id="70cdf-256">Soubory protokolu jsou důležité prostředků při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="70cdf-256">Log files are an important resource when troubleshooting.</span></span>

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="70cdf-257">Protokoly služby Enterprise gateway</span><span class="sxs-lookup"><span data-stu-id="70cdf-257">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a><span data-ttu-id="70cdf-258">Konfigurace protokolů</span><span class="sxs-lookup"><span data-stu-id="70cdf-258">Configuration logs</span></span>

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a><span data-ttu-id="70cdf-259">Protokoly událostí</span><span class="sxs-lookup"><span data-stu-id="70cdf-259">Event logs</span></span>

<span data-ttu-id="70cdf-260">Můžete najít hello protokoly Brána pro správu dat a PowerBIGateway pod **protokoly aplikací a služeb**.</span><span class="sxs-lookup"><span data-stu-id="70cdf-260">You can find hello Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>


## <span data-ttu-id="70cdf-261"><a name="telemetry"></a>Telemetrie</span><span class="sxs-lookup"><span data-stu-id="70cdf-261"><a name="telemetry"></a>Telemetry</span></span>
<span data-ttu-id="70cdf-262">Telemetrická data lze použít pro monitorování a řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="70cdf-262">Telemetry can be used for monitoring and troubleshooting.</span></span> <span data-ttu-id="70cdf-263">Ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="70cdf-263">By default</span></span>

<span data-ttu-id="70cdf-264">**tooturn na telemetrie**</span><span class="sxs-lookup"><span data-stu-id="70cdf-264">**tooturn on telemetry**</span></span>

1.  <span data-ttu-id="70cdf-265">Zkontrolujte hello místní data brány klienta adresář v počítači hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-265">Check hello On-premises data gateway client directory on hello computer.</span></span> <span data-ttu-id="70cdf-266">Obvykle je **%systemdrive%\Program Files\On místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="70cdf-266">Typically, it is **%systemdrive%\Program Files\On-premises data gateway**.</span></span> <span data-ttu-id="70cdf-267">Nebo můžete spustit konzolu služby a zkontrolujte tooexecutable cesta hello: vlastnost služba brány pro hello místní data.</span><span class="sxs-lookup"><span data-stu-id="70cdf-267">Or, you can open a Services console and check hello Path tooexecutable: A property of hello On-premises data gateway service.</span></span>
2.  <span data-ttu-id="70cdf-268">V souboru Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config hello z adresáře klienta.</span><span class="sxs-lookup"><span data-stu-id="70cdf-268">In hello Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config file from client directory.</span></span> <span data-ttu-id="70cdf-269">Změňte tootrue nastavení SendTelemetry hello.</span><span class="sxs-lookup"><span data-stu-id="70cdf-269">Change hello SendTelemetry setting tootrue.</span></span>
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  <span data-ttu-id="70cdf-270">Uložte změny a restartovat službu systému Windows hello: místní služba brány pro data.</span><span class="sxs-lookup"><span data-stu-id="70cdf-270">Save your changes and restart hello Windows service: On-premises data gateway service.</span></span>




## <a name="next-steps"></a><span data-ttu-id="70cdf-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70cdf-271">Next steps</span></span>
* [<span data-ttu-id="70cdf-272">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="70cdf-272">Manage Analysis Services</span></span>](analysis-services-manage.md)
* [<span data-ttu-id="70cdf-273">Získání dat z Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="70cdf-273">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
