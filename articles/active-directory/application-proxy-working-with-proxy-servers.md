---
title: "aaaWork s existujícími místními proxy servery a Azure AD | Microsoft Docs"
description: "Popisuje, jak toowork s existujícími místními proxy servery."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 7f8cec4f676f99bead5211bcbcf23056bd7f211f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a><span data-ttu-id="2d72f-103">Práce s existující místní proxy servery</span><span class="sxs-lookup"><span data-stu-id="2d72f-103">Work with existing on-premises proxy servers</span></span>

<span data-ttu-id="2d72f-104">Tento článek vysvětluje, jak toowork konektory tooconfigure Proxy aplikace služby Azure Active Directory (Azure AD) s odchozí proxy servery.</span><span class="sxs-lookup"><span data-stu-id="2d72f-104">This article explains how tooconfigure Azure Active Directory (Azure AD) Application Proxy connectors toowork with outbound proxy servers.</span></span> <span data-ttu-id="2d72f-105">Je určený pro zákazníky s sítě prostředích, která mají existující proxy.</span><span class="sxs-lookup"><span data-stu-id="2d72f-105">It is intended for customers with network environments that have existing proxies.</span></span>

<span data-ttu-id="2d72f-106">Můžeme začít hledáním v těchto scénářích hlavní nasazení:</span><span class="sxs-lookup"><span data-stu-id="2d72f-106">We start by looking at these main deployment scenarios:</span></span>
* <span data-ttu-id="2d72f-107">Nakonfigurujte konektory toobypass vaše místní odchozí proxy.</span><span class="sxs-lookup"><span data-stu-id="2d72f-107">Configure connectors toobypass your on-premises outbound proxies.</span></span>
* <span data-ttu-id="2d72f-108">Nakonfigurujte konektory toouse tooaccess odchozího proxy serveru proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d72f-108">Configure connectors toouse an outbound proxy tooaccess Azure AD Application Proxy.</span></span>

<span data-ttu-id="2d72f-109">Další informace o fungování konektory najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="2d72f-109">For more information about how connectors work, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

## <a name="configure-hello-outbound-proxy"></a><span data-ttu-id="2d72f-110">Konfigurace odchozího proxy serveru hello</span><span class="sxs-lookup"><span data-stu-id="2d72f-110">Configure hello outbound proxy</span></span>

<span data-ttu-id="2d72f-111">Pokud máte ve vašem prostředí odchozího proxy serveru, použijte účet s příslušnými oprávněními tooconfigure hello odchozí proxy.</span><span class="sxs-lookup"><span data-stu-id="2d72f-111">If you have an outbound proxy in your environment, use an account with appropriate permissions tooconfigure hello outbound proxy.</span></span> <span data-ttu-id="2d72f-112">Protože hello instalační program se spustí v kontextu hello hello uživatele, který provádí instalaci hello, můžete zkontrolovat konfiguraci hello pomocí Microsoft Edge nebo jiný prohlížeč Internetu.</span><span class="sxs-lookup"><span data-stu-id="2d72f-112">Because hello installer runs in hello context of hello user who's doing hello installation, you can check hello configuration by using Microsoft Edge or another Internet browser.</span></span>

<span data-ttu-id="2d72f-113">nastavení proxy serveru tooconfigure hello v Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="2d72f-113">tooconfigure hello proxy settings in Microsoft Edge:</span></span>

1. <span data-ttu-id="2d72f-114">Přejděte příliš**nastavení** > **zobrazení Upřesnit nastavení** > **otevřete nastavení proxy serveru** > **ruční instalace Proxy** .</span><span class="sxs-lookup"><span data-stu-id="2d72f-114">Go too**Settings** > **View Advanced Settings** > **Open Proxy Settings** > **Manual Proxy Setup**.</span></span>
2. <span data-ttu-id="2d72f-115">Nastavit **použít proxy server** příliš**na**, vyberte hello **nepoužívejte hello proxy serveru pro místní adresy** zaškrtávací políčko a potom změnu hello adresu a port tooreflect váš místní proxy server.</span><span class="sxs-lookup"><span data-stu-id="2d72f-115">Set **Use a proxy server** too**On**, select hello **Don’t use hello proxy server for local (intranet) addresses** check box, and then change hello address and port tooreflect your local proxy server.</span></span>
3. <span data-ttu-id="2d72f-116">Zadejte nastavení proxy serveru potřebné hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-116">Fill in hello necessary proxy settings.</span></span>

   ![Dialogové okno pro nastavení proxy serveru](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a><span data-ttu-id="2d72f-118">Odchozí proxy jednorázové přihlášení</span><span class="sxs-lookup"><span data-stu-id="2d72f-118">Bypass outbound proxies</span></span>

<span data-ttu-id="2d72f-119">Konektory mít základní součásti operačního systému, které odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="2d72f-119">Connectors have underlying OS components that make outbound requests.</span></span> <span data-ttu-id="2d72f-120">Automaticky se pokusí tyto součásti toolocate proxy server v síti hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-120">These components automatically attempt toolocate a proxy server on hello network.</span></span> <span data-ttu-id="2d72f-121">Používají Proxy Auto-Discovery WPAD (Web), pokud je povoleno v prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-121">They use Web Proxy Auto-Discovery (WPAD), if it's enabled in hello environment.</span></span>

<span data-ttu-id="2d72f-122">součásti operačního systému Hello pokusit toolocate proxy server pomocí uskutečňují vyhledávání DNS pro wpad.domainsuffix.</span><span class="sxs-lookup"><span data-stu-id="2d72f-122">hello OS components attempt toolocate a proxy server by carrying out a DNS lookup for wpad.domainsuffix.</span></span> <span data-ttu-id="2d72f-123">Pokud to řeší ve službě DNS, požadavek HTTP je potom k toohello IP adresu pro wpad.dat.</span><span class="sxs-lookup"><span data-stu-id="2d72f-123">If this resolves in DNS, an HTTP request is then made toohello IP address for wpad.dat.</span></span> <span data-ttu-id="2d72f-124">Tento požadavek se změní na skript konfigurace proxy serveru hello ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d72f-124">This request becomes hello proxy configuration script in your environment.</span></span> <span data-ttu-id="2d72f-125">konektor Hello používá tento skript tooselect serveru odchozího proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="2d72f-125">hello connector uses this script tooselect an outbound proxy server.</span></span> <span data-ttu-id="2d72f-126">Ale provoz konektor nemusí stále projít, z důvodu nastavení konfigurace na hello proxy.</span><span class="sxs-lookup"><span data-stu-id="2d72f-126">However, connector traffic might still not go through, because of additional configuration settings needed on hello proxy.</span></span>

<span data-ttu-id="2d72f-127">Můžete nakonfigurovat toobypass konektor hello vaší tooensure místní proxy server, který používá přímé připojení toohello Azure services.</span><span class="sxs-lookup"><span data-stu-id="2d72f-127">You can configure hello connector toobypass your on-premises proxy tooensure that it uses direct connectivity toohello Azure services.</span></span> <span data-ttu-id="2d72f-128">Doporučujeme, abyste tento přístup (Pokud je pro něj umožňuje zásady sítě), protože znamená, že máte jeden menší toomaintain konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2d72f-128">We recommend this approach (if your network policy allows for it), because it means that you have one less configuration toomaintain.</span></span>

<span data-ttu-id="2d72f-129">využití toodisable odchozího proxy serveru pro konektor hello, upravte soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config hello a přidejte hello *system.net* části uvedené v této ukázce kódu :</span><span class="sxs-lookup"><span data-stu-id="2d72f-129">toodisable outbound proxy usage for hello connector, edit hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add hello *system.net* section shown in this code sample:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
<span data-ttu-id="2d72f-130">tooensure service Connector Updater hello také obchází hello proxy, ujistěte se, podobně jako změnu toohello ApplicationProxyConnectorUpdaterService.exe.config soubor nacházející se v C:\Program Files\Microsoft AAD aplikace Proxy Connector Updater.</span><span class="sxs-lookup"><span data-stu-id="2d72f-130">tooensure that hello Connector Updater service also bypasses hello proxy, make a similar change toohello ApplicationProxyConnectorUpdaterService.exe.config file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span></span>

<span data-ttu-id="2d72f-131">Zda toomake kopie hello původní soubory, se v případě, že potřebujete toorevert toohello výchozí .config soubory.</span><span class="sxs-lookup"><span data-stu-id="2d72f-131">Be sure toomake copies of hello original files, in case you need toorevert toohello default .config files.</span></span>

## <a name="use-hello-outbound-proxy-server"></a><span data-ttu-id="2d72f-132">Použít hello odchozí proxy server</span><span class="sxs-lookup"><span data-stu-id="2d72f-132">Use hello outbound proxy server</span></span>

<span data-ttu-id="2d72f-133">Některé prostředí vyžaduje všechny odchozí přenosy toogo prostřednictvím odchozího proxy serveru, bez výjimky.</span><span class="sxs-lookup"><span data-stu-id="2d72f-133">Some environments require all outbound traffic toogo through an outbound proxy, without exception.</span></span> <span data-ttu-id="2d72f-134">V důsledku toho obcházení hello proxy není možné.</span><span class="sxs-lookup"><span data-stu-id="2d72f-134">As a result, bypassing hello proxy is not an option.</span></span>

<span data-ttu-id="2d72f-135">Hello konektor provoz toogo prostřednictvím hello odchozího proxy serveru, můžete nakonfigurovat, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="2d72f-135">You can configure hello connector traffic toogo through hello outbound proxy, as shown in hello following diagram:</span></span>

 ![Konfigurace konektoru toogo provoz prostřednictvím tooAzure odchozího proxy serveru AD Proxy aplikace](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

<span data-ttu-id="2d72f-137">V důsledku toho, že pouze odchozí přenosy, neexistuje žádný tooconfigure nutné příchozí přístup přes vaší brány firewall.</span><span class="sxs-lookup"><span data-stu-id="2d72f-137">As a result of having only outbound traffic, there's no need tooconfigure inbound access through your firewalls.</span></span>

### <a name="step-1-configure-hello-connector-and-related-services-toogo-through-hello-outbound-proxy"></a><span data-ttu-id="2d72f-138">Krok 1: Konfigurace hello konektoru a související služby toogo prostřednictvím hello odchozího proxy serveru</span><span class="sxs-lookup"><span data-stu-id="2d72f-138">Step 1: Configure hello connector and related services toogo through hello outbound proxy</span></span>

<span data-ttu-id="2d72f-139">Jak je popsané dříve, pokud je povoleno v prostředí hello a správně nakonfigurována WPAD, konektor hello automaticky zjistit hello odchozí proxy server a pokus o toouse ho.</span><span class="sxs-lookup"><span data-stu-id="2d72f-139">As covered earlier, if WPAD is enabled in hello environment and configured appropriately, hello connector will automatically discover hello outbound proxy server and attempt toouse it.</span></span> <span data-ttu-id="2d72f-140">Můžete však explicitně nakonfigurovat konektor toogo hello prostřednictvím odchozího proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="2d72f-140">However, you can explicitly configure hello connector toogo through an outbound proxy.</span></span>

<span data-ttu-id="2d72f-141">toodo tedy upravit soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config hello a přidat hello *system.net* části uvedené v této ukázce kódu.</span><span class="sxs-lookup"><span data-stu-id="2d72f-141">toodo so, edit hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add hello *system.net* section shown in this code sample.</span></span> <span data-ttu-id="2d72f-142">Změna *proxyserver:8080* tooreflect vaše místní proxy server název nebo IP adresa a hello portu naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="2d72f-142">Change *proxyserver:8080* tooreflect your local proxy server name or IP address, and hello port that it's listening on.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

<span data-ttu-id="2d72f-143">V dalším kroku nakonfigurujte proxy server hello Connector Updater služby toouse hello tím, že v C:\Program Files\Microsoft AAD aplikace proxy serveru konektoru Updater\ApplicationProxyConnectorUpdaterService.exe.config podobné soubor toohello změnu.</span><span class="sxs-lookup"><span data-stu-id="2d72f-143">Next, configure hello Connector Updater service toouse hello proxy by making a similar change toohello file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span></span>

### <a name="step-2-configure-hello-proxy-tooallow-traffic-from-hello-connector-and-related-services-tooflow-through"></a><span data-ttu-id="2d72f-144">Krok 2: Konfigurace hello proxy tooallow provoz z hello konektoru a související služby tooflow prostřednictvím</span><span class="sxs-lookup"><span data-stu-id="2d72f-144">Step 2: Configure hello proxy tooallow traffic from hello connector and related services tooflow through</span></span>

<span data-ttu-id="2d72f-145">Existují čtyři aspekty tooconsider v hello odchozího proxy serveru:</span><span class="sxs-lookup"><span data-stu-id="2d72f-145">There are four aspects tooconsider at hello outbound proxy:</span></span>
* <span data-ttu-id="2d72f-146">Odchozí pravidla proxy</span><span class="sxs-lookup"><span data-stu-id="2d72f-146">Proxy outbound rules</span></span>
* <span data-ttu-id="2d72f-147">Ověřování proxy serverem</span><span class="sxs-lookup"><span data-stu-id="2d72f-147">Proxy authentication</span></span>
* <span data-ttu-id="2d72f-148">Porty proxy</span><span class="sxs-lookup"><span data-stu-id="2d72f-148">Proxy ports</span></span>
* <span data-ttu-id="2d72f-149">Kontrolu SSL</span><span class="sxs-lookup"><span data-stu-id="2d72f-149">SSL inspection</span></span>

#### <a name="proxy-outbound-rules"></a><span data-ttu-id="2d72f-150">Odchozí pravidla proxy</span><span class="sxs-lookup"><span data-stu-id="2d72f-150">Proxy outbound rules</span></span>
<span data-ttu-id="2d72f-151">Povolit přístup toohello následující koncové body pro přístup k službě konektor:</span><span class="sxs-lookup"><span data-stu-id="2d72f-151">Allow access toohello following endpoints for connector service access:</span></span>

* <span data-ttu-id="2d72f-152">*. msappproxy.net</span><span class="sxs-lookup"><span data-stu-id="2d72f-152">*.msappproxy.net</span></span>
* <span data-ttu-id="2d72f-153">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="2d72f-153">*.servicebus.windows.net</span></span>

<span data-ttu-id="2d72f-154">Pro počáteční registraci povolit přístup toohello následující koncové body:</span><span class="sxs-lookup"><span data-stu-id="2d72f-154">For initial registration, allow access toohello following endpoints:</span></span>

* <span data-ttu-id="2d72f-155">login.windows.net</span><span class="sxs-lookup"><span data-stu-id="2d72f-155">login.windows.net</span></span>
* <span data-ttu-id="2d72f-156">Login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="2d72f-156">login.microsoftonline.com</span></span>

<span data-ttu-id="2d72f-157">Pokud nelze povolit připojení ve plně kvalifikovaný název domény a místo toho musí toospecify rozsahy IP adres, použijte tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="2d72f-157">If you can't allow connectivity by FQDN and need toospecify IP ranges instead, use these options:</span></span>

* <span data-ttu-id="2d72f-158">Povolí odchozí přístup hello konektor tooall cíle.</span><span class="sxs-lookup"><span data-stu-id="2d72f-158">Allow hello connector outbound access tooall destinations.</span></span>
* <span data-ttu-id="2d72f-159">Povolit odchozí přístup hello konektor příliš[rozsahy IP adres Azure datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="2d72f-159">Allow hello connector outbound access too[Azure datacenter IP ranges](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span></span> <span data-ttu-id="2d72f-160">Hello výzvy s použitím hello seznam rozsahů IP adres Azure datacenter je, že je aktualizovaný týdně.</span><span class="sxs-lookup"><span data-stu-id="2d72f-160">hello challenge with using hello list of Azure datacenter IP ranges is that it's updated weekly.</span></span> <span data-ttu-id="2d72f-161">Je nutné tooput procesu v místě tooensure, že jsou vaše pravidla přístupu k příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="2d72f-161">You need tooput a process in place tooensure that your access rules are updated accordingly.</span></span>

#### <a name="proxy-authentication"></a><span data-ttu-id="2d72f-162">Ověřování proxy serverem</span><span class="sxs-lookup"><span data-stu-id="2d72f-162">Proxy authentication</span></span>

<span data-ttu-id="2d72f-163">Ověřování proxy není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="2d72f-163">Proxy authentication is not currently supported.</span></span> <span data-ttu-id="2d72f-164">Naše doporučení aktuální je tooallow hello konektor anonymní přístup toohello Internet cíle.</span><span class="sxs-lookup"><span data-stu-id="2d72f-164">Our current recommendation is tooallow hello connector anonymous access toohello Internet destinations.</span></span>

#### <a name="proxy-ports"></a><span data-ttu-id="2d72f-165">Porty proxy</span><span class="sxs-lookup"><span data-stu-id="2d72f-165">Proxy ports</span></span>

<span data-ttu-id="2d72f-166">Hello konektor umožňuje odchozí připojení založené na protokolu SSL pomocí metody CONNECT hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-166">hello connector makes outbound SSL-based connections by using hello CONNECT method.</span></span> <span data-ttu-id="2d72f-167">Tato metoda je v podstatě nastavuje tunelové propojení prostřednictvím hello odchozího proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="2d72f-167">This method essentially sets up a tunnel through hello outbound proxy.</span></span> <span data-ttu-id="2d72f-168">Nakonfigurujte hello proxy server tooallow tunelování tooports 443 a 80.</span><span class="sxs-lookup"><span data-stu-id="2d72f-168">Configure hello proxy server tooallow tunneling tooports 443 and 80.</span></span>

>[!NOTE]
><span data-ttu-id="2d72f-169">Spuštění služby Service Bus přes protokol HTTPS používá port 443.</span><span class="sxs-lookup"><span data-stu-id="2d72f-169">When Service Bus runs over HTTPS, it uses port 443.</span></span> <span data-ttu-id="2d72f-170">Ve výchozím nastavení, ale Service Bus pokusí přímé připojení TCP a spadne zpět tooHTTPS pouze v případě, že přímé připojení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="2d72f-170">However, by default, Service Bus attempts direct TCP connections and falls back tooHTTPS only if direct connectivity fails.</span></span>

<span data-ttu-id="2d72f-171">tooensure, který hello Service Bus se odesílá i přes hello odchozí proxy server, ujistěte se, že tento hello connector se nemůže připojit přímo toohello Azure services pro porty 9350, 9352 a 5671.</span><span class="sxs-lookup"><span data-stu-id="2d72f-171">tooensure that hello Service Bus traffic is also sent through hello outbound proxy server, ensure that hello connector cannot directly connect toohello Azure services for ports 9350, 9352, and 5671.</span></span>

#### <a name="ssl-inspection"></a><span data-ttu-id="2d72f-172">Kontrolu SSL</span><span class="sxs-lookup"><span data-stu-id="2d72f-172">SSL inspection</span></span>
<span data-ttu-id="2d72f-173">Nepoužívejte kontrolu SSL pro provoz hello konektoru, protože způsobuje problémy týkající se provozu konektor hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-173">Do not use SSL inspection for hello connector traffic, because it causes problems for hello connector traffic.</span></span>

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a><span data-ttu-id="2d72f-174">Řešení potíží s konektor proxy problémy a potíže s připojením služby</span><span class="sxs-lookup"><span data-stu-id="2d72f-174">Troubleshoot connector proxy problems and service connectivity issues</span></span>
<span data-ttu-id="2d72f-175">Nyní byste měli vidět veškerý provoz přes proxy server hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-175">Now you should see all traffic flowing through hello proxy.</span></span> <span data-ttu-id="2d72f-176">Pokud máte potíže, by měly pomoci hello následující informace o odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="2d72f-176">If you have problems, hello following troubleshooting information should help.</span></span>

<span data-ttu-id="2d72f-177">Hello nejlepší způsob, jak tooidentify a řešení potíží s připojením konektor problémy je tootake síť zaznamenat na službu konektoru hello při spouštění služby konektoru hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-177">hello best way tooidentify and troubleshoot connector connectivity issues is tootake a network capture on hello connector service while starting hello connector service.</span></span> <span data-ttu-id="2d72f-178">To může být složitý úkol, takže Podíváme se na rychlé tipy k zaznamenání a filtrování trasování sítě.</span><span class="sxs-lookup"><span data-stu-id="2d72f-178">This can be a daunting task, so let’s look at quick tips on capturing and filtering network traces.</span></span>

<span data-ttu-id="2d72f-179">Můžete použít hello monitorování nástroj podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="2d72f-179">You can use hello monitoring tool of your choice.</span></span> <span data-ttu-id="2d72f-180">Pro účely hello tohoto článku použili jsme Microsoft Network Monitor 3.4.</span><span class="sxs-lookup"><span data-stu-id="2d72f-180">For hello purposes of this article, we used Microsoft Network Monitor 3.4.</span></span> <span data-ttu-id="2d72f-181">Můžete [ji stáhnout z Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span><span class="sxs-lookup"><span data-stu-id="2d72f-181">You can [download it from Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span></span>

<span data-ttu-id="2d72f-182">Příklady Hello a filtry, které používáme v hello následující části jsou konkrétní tooNetwork monitorování, ale hello zásad může být použité tooany nástroj pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="2d72f-182">hello examples and filters that we use in hello following sections are specific tooNetwork Monitor, but hello principles can be applied tooany analysis tool.</span></span>

### <a name="take-a-capture-by-using-network-monitor"></a><span data-ttu-id="2d72f-183">Proveďte zachycení pomocí sledování sítě</span><span class="sxs-lookup"><span data-stu-id="2d72f-183">Take a capture by using Network Monitor</span></span>

<span data-ttu-id="2d72f-184">toostart zachycení:</span><span class="sxs-lookup"><span data-stu-id="2d72f-184">toostart a capture:</span></span>

1. <span data-ttu-id="2d72f-185">Otevřete sledování sítě a klikněte na tlačítko **nové zachycení**.</span><span class="sxs-lookup"><span data-stu-id="2d72f-185">Open Network Monitor and click **New Capture**.</span></span>
2. <span data-ttu-id="2d72f-186">Klikněte na tlačítko hello **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2d72f-186">Click hello **Start** button.</span></span>

   ![Okno monitorování sítě](./media/application-proxy-working-with-proxy-servers/network-capture.png)

<span data-ttu-id="2d72f-188">Po dokončení zachycení, klikněte na tlačítko hello **Zastavit** tooend tlačítko ji.</span><span class="sxs-lookup"><span data-stu-id="2d72f-188">After you complete a capture, click hello **Stop** button tooend it.</span></span>

### <a name="take-a-capture-of-connector-traffic"></a><span data-ttu-id="2d72f-189">Proveďte zachycení provozu konektoru</span><span class="sxs-lookup"><span data-stu-id="2d72f-189">Take a capture of connector traffic</span></span>

<span data-ttu-id="2d72f-190">Pro počáteční řešení potíží s proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2d72f-190">For initial troubleshooting, perform hello following steps:</span></span>

1. <span data-ttu-id="2d72f-191">Z services.msc zastavte službu konektoru Proxy aplikace služby Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-191">From services.msc, stop hello Azure AD Application Proxy Connector service.</span></span>
2. <span data-ttu-id="2d72f-192">Spusťte hello zachycení dat ze sítě.</span><span class="sxs-lookup"><span data-stu-id="2d72f-192">Start hello network capture.</span></span>
3. <span data-ttu-id="2d72f-193">Spusťte službu konektoru Proxy aplikace služby Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-193">Start hello Azure AD Application Proxy Connector service.</span></span>
4. <span data-ttu-id="2d72f-194">Zastavte hello zachycení dat ze sítě.</span><span class="sxs-lookup"><span data-stu-id="2d72f-194">Stop hello network capture.</span></span>

   ![Služba Azure AD konektor Proxy aplikace v services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-hello-requests-from-hello-connector-toohello-proxy-server"></a><span data-ttu-id="2d72f-196">Podívejte se na požadavky hello z hello konektor toohello proxy serveru</span><span class="sxs-lookup"><span data-stu-id="2d72f-196">Look at hello requests from hello connector toohello proxy server</span></span>

<span data-ttu-id="2d72f-197">Teď, když máte k dispozici zachycení dat ze sítě, jste připravené toofilter ho.</span><span class="sxs-lookup"><span data-stu-id="2d72f-197">Now that you’ve got a network capture, you're ready toofilter it.</span></span> <span data-ttu-id="2d72f-198">Hello klíče toolooking na hello trasování je pochopení, jak zachytit toofilter hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-198">hello key toolooking at hello trace is understanding how toofilter hello capture.</span></span>

<span data-ttu-id="2d72f-199">Jeden filtr je následující (kde 8080 je port služby proxy hello):</span><span class="sxs-lookup"><span data-stu-id="2d72f-199">One filter is as follows (where 8080 is hello proxy service port):</span></span>

<span data-ttu-id="2d72f-200">**(http. Požadavek nebo http. Odpověď) a tcp.port==8080**</span><span class="sxs-lookup"><span data-stu-id="2d72f-200">**(http.Request or http.Response) and tcp.port==8080**</span></span>

<span data-ttu-id="2d72f-201">Pokud zadáte tento filtr v hello **filtru zobrazení** a vyberte **použít**, filtruje hello zachycení provozu na základě filtru hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-201">If you enter this filter in hello **Display Filter** window and select **Apply**, it filters hello captured traffic based on hello filter.</span></span>

<span data-ttu-id="2d72f-202">Hello předchozí filtr zobrazuje jenom hello požadavky a odpovědi HTTP z hello port proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="2d72f-202">hello preceding filter shows just hello HTTP requests and responses to/from hello proxy port.</span></span> <span data-ttu-id="2d72f-203">Pro spuštění konektoru kde hello konektor je nakonfigurované toouse proxy server hello filtr by zobrazit přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="2d72f-203">For a connector startup where hello connector is configured toouse a proxy server, hello filter would show something like this:</span></span>

 ![Příklad seznamu Filtrované požadavky a odpovědi HTTP](./media/application-proxy-working-with-proxy-servers/http-requests.png)

<span data-ttu-id="2d72f-205">Nyní konkrétně hledáte hello CONNECT požadavky, které zobrazit komunikaci se serverem proxy hello.</span><span class="sxs-lookup"><span data-stu-id="2d72f-205">You're now specifically looking for hello CONNECT requests that show communication with hello proxy server.</span></span> <span data-ttu-id="2d72f-206">Po úspěšné zobrazí se odpověď HTTP OK (200).</span><span class="sxs-lookup"><span data-stu-id="2d72f-206">Upon success, you get an HTTP OK (200) response.</span></span>

<span data-ttu-id="2d72f-207">Pokud se zobrazí další kódy odpovědí, jako je například 407 nebo 502, hello proxy je vyžadování ověřování nebo není umožňuje hello provoz z jiného důvodu.</span><span class="sxs-lookup"><span data-stu-id="2d72f-207">If you see other response codes, such as 407 or 502, hello proxy is requiring authentication or not allowing hello traffic for some other reason.</span></span> <span data-ttu-id="2d72f-208">V tomto okamžiku zaujmout váš tým podpory proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="2d72f-208">At this point, you engage your proxy server support team.</span></span>

### <a name="identify-failed-tcp-connection-attempts"></a><span data-ttu-id="2d72f-209">Identifikovat neúspěšné pokusy o připojení protokolu TCP</span><span class="sxs-lookup"><span data-stu-id="2d72f-209">Identify failed TCP connection attempts</span></span>

<span data-ttu-id="2d72f-210">Hello další běžné scénáře, který vás může zajímat je při hello konektoru se pokouší tooconnect přímo, ale selhává.</span><span class="sxs-lookup"><span data-stu-id="2d72f-210">hello other common scenario that you may be interested in is when hello connector is trying tooconnect directly, but it's failing.</span></span>

<span data-ttu-id="2d72f-211">Jiné sledování sítě filtr, který pomáhá tooeasily identifikovat tento problém je:</span><span class="sxs-lookup"><span data-stu-id="2d72f-211">Another Network Monitor filter that helps you tooeasily identify this problem is:</span></span>

<span data-ttu-id="2d72f-212">**Vlastnost. TCPSynRetransmit**</span><span class="sxs-lookup"><span data-stu-id="2d72f-212">**property.TCPSynRetransmit**</span></span>

<span data-ttu-id="2d72f-213">Paket SYN je odeslán paket první hello tooestablish připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="2d72f-213">A SYN packet is hello first packet sent tooestablish a TCP connection.</span></span> <span data-ttu-id="2d72f-214">Pokud tomuto paketu nevrací odpověď, je reattempted hello SYN.</span><span class="sxs-lookup"><span data-stu-id="2d72f-214">If this packet doesn’t return a response, hello SYN is reattempted.</span></span> <span data-ttu-id="2d72f-215">Hello předcházející toosee filtru můžete použít všechny požadavky SYN opakovaně.</span><span class="sxs-lookup"><span data-stu-id="2d72f-215">You can use hello preceding filter toosee any retransmitted SYNs.</span></span> <span data-ttu-id="2d72f-216">Potom můžete zkontrolovat, zda tyto požadavky SYN odpovídají tooany související konektor provoz.</span><span class="sxs-lookup"><span data-stu-id="2d72f-216">Then, you can check whether these SYNs correspond tooany connector-related traffic.</span></span>

<span data-ttu-id="2d72f-217">Hello následující příklad ukazuje, pokus o připojení se nezdařilo tooService sběrnice port 9352:</span><span class="sxs-lookup"><span data-stu-id="2d72f-217">hello following example shows a failed connection attempt tooService Bus port 9352:</span></span>

 ![Příklad odpověď pro pokus o připojení se nezdařilo](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

<span data-ttu-id="2d72f-219">Pokud se zobrazí přibližně hello předcházející odpovědi, konektor hello se pokouší toocommunicate přímo s hello služby Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="2d72f-219">If you see something like hello preceding response, hello connector is trying toocommunicate directly with hello Azure Service Bus service.</span></span> <span data-ttu-id="2d72f-220">Pokud očekáváte hello konektor toomake přímé připojení toohello Azure services, tato odpověď není jasný náznak, že máte síť nebo brána firewall problému.</span><span class="sxs-lookup"><span data-stu-id="2d72f-220">If you expect hello connector toomake direct connections toohello Azure services, this response is a clear indication that you have a network or firewall problem.</span></span>

>[!NOTE]
><span data-ttu-id="2d72f-221">Pokud jste nakonfigurované toouse proxy server, tato odpověď může znamenat, že Service Bus se pokouší o přímé připojení TCP před přepnutím tooattempting připojení přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2d72f-221">If you are configured toouse a proxy server, this response might mean that Service Bus is attempting a direct TCP connection before switching tooattempting a connection over HTTPS.</span></span>
>

<span data-ttu-id="2d72f-222">Analýzy trasování sítě není u všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2d72f-222">Network trace analysis is not for everyone.</span></span> <span data-ttu-id="2d72f-223">Ale může být a cenným nástrojem tooget rychlé informace o co se děje s vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="2d72f-223">But it can be a valuable tool tooget quick information about what's going on with your network.</span></span>

<span data-ttu-id="2d72f-224">Pokud budete pokračovat toostruggle s problémy s připojením k konektor, vytvořte lístek s náš tým podpory.</span><span class="sxs-lookup"><span data-stu-id="2d72f-224">If you continue toostruggle with connector connectivity issues, create a ticket with our support team.</span></span> <span data-ttu-id="2d72f-225">Hello tým pomoct s další informace o řešení.</span><span class="sxs-lookup"><span data-stu-id="2d72f-225">hello team can assist you with further troubleshooting.</span></span>

<span data-ttu-id="2d72f-226">Informace o řešení chyb s konektor Proxy aplikace najdete v tématu [Poradce při potížích s Proxy aplikace](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="2d72f-226">For information about resolving errors with Application Proxy Connector, see [Troubleshoot Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d72f-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d72f-227">Next steps</span></span>

[<span data-ttu-id="2d72f-228">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d72f-228">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)<br><span data-ttu-id="2d72f-229">
[Jak toosilently instalovat hello konektoru Proxy aplikace služby Azure AD](active-directory-application-proxy-silent-installation.md)</span><span class="sxs-lookup"><span data-stu-id="2d72f-229">
[How toosilently install hello Azure AD Application Proxy Connector ](active-directory-application-proxy-silent-installation.md)</span></span>
