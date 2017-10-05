---
title: "Práce s existujícími místními proxy servery a Azure AD | Microsoft Docs"
description: "Popisuje, jak pracovat s existující místní proxy servery."
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
ms.openlocfilehash: bdca442755507c4ffe8d43692c5b7f2aa3a746f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a><span data-ttu-id="31eee-103">Práce s existující místní proxy servery</span><span class="sxs-lookup"><span data-stu-id="31eee-103">Work with existing on-premises proxy servers</span></span>

<span data-ttu-id="31eee-104">Tento článek vysvětluje, jak konfigurovat konektory Proxy aplikace služby Azure Active Directory (Azure AD) pro práci s odchozí proxy servery.</span><span class="sxs-lookup"><span data-stu-id="31eee-104">This article explains how to configure Azure Active Directory (Azure AD) Application Proxy connectors to work with outbound proxy servers.</span></span> <span data-ttu-id="31eee-105">Je určený pro zákazníky s sítě prostředích, která mají existující proxy.</span><span class="sxs-lookup"><span data-stu-id="31eee-105">It is intended for customers with network environments that have existing proxies.</span></span>

<span data-ttu-id="31eee-106">Můžeme začít hledáním v těchto scénářích hlavní nasazení:</span><span class="sxs-lookup"><span data-stu-id="31eee-106">We start by looking at these main deployment scenarios:</span></span>
* <span data-ttu-id="31eee-107">Nakonfigurujte konektory pro vynechat vaše místní odchozí proxy.</span><span class="sxs-lookup"><span data-stu-id="31eee-107">Configure connectors to bypass your on-premises outbound proxies.</span></span>
* <span data-ttu-id="31eee-108">Nakonfigurujte konektory používat odchozího proxy serveru pro přístup k Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="31eee-108">Configure connectors to use an outbound proxy to access Azure AD Application Proxy.</span></span>

<span data-ttu-id="31eee-109">Další informace o fungování konektory najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="31eee-109">For more information about how connectors work, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

## <a name="configure-the-outbound-proxy"></a><span data-ttu-id="31eee-110">Konfigurace odchozího proxy serveru</span><span class="sxs-lookup"><span data-stu-id="31eee-110">Configure the outbound proxy</span></span>

<span data-ttu-id="31eee-111">Pokud máte ve vašem prostředí odchozího proxy serveru, použijte účet s příslušnými oprávněními ke konfiguraci odchozího proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-111">If you have an outbound proxy in your environment, use an account with appropriate permissions to configure the outbound proxy.</span></span> <span data-ttu-id="31eee-112">Vzhledem k tomu, že instalační program se spustí v kontextu uživatele, který provádí instalaci, můžete zkontrolovat konfiguraci pomocí Microsoft Edge nebo jiný prohlížeč Internetu.</span><span class="sxs-lookup"><span data-stu-id="31eee-112">Because the installer runs in the context of the user who's doing the installation, you can check the configuration by using Microsoft Edge or another Internet browser.</span></span>

<span data-ttu-id="31eee-113">Konfigurace nastavení proxy serveru v Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="31eee-113">To configure the proxy settings in Microsoft Edge:</span></span>

1. <span data-ttu-id="31eee-114">Přejděte na **nastavení** > **zobrazení upřesňující nastavení** > **otevřete nastavení proxy serveru** > **instalace ruční Proxy**.</span><span class="sxs-lookup"><span data-stu-id="31eee-114">Go to **Settings** > **View Advanced Settings** > **Open Proxy Settings** > **Manual Proxy Setup**.</span></span>
2. <span data-ttu-id="31eee-115">Nastavit **použít proxy server** k **na**, vyberte **nechcete používat proxy server pro místní adresy** zaškrtněte políčko a potom změňte adresu a port, aby odrážela místní proxy server.</span><span class="sxs-lookup"><span data-stu-id="31eee-115">Set **Use a proxy server** to **On**, select the **Don’t use the proxy server for local (intranet) addresses** check box, and then change the address and port to reflect your local proxy server.</span></span>
3. <span data-ttu-id="31eee-116">Zadejte nastavení potřebné proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-116">Fill in the necessary proxy settings.</span></span>

   ![Dialogové okno pro nastavení proxy serveru](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a><span data-ttu-id="31eee-118">Odchozí proxy jednorázové přihlášení</span><span class="sxs-lookup"><span data-stu-id="31eee-118">Bypass outbound proxies</span></span>

<span data-ttu-id="31eee-119">Konektory mít základní součásti operačního systému, které odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="31eee-119">Connectors have underlying OS components that make outbound requests.</span></span> <span data-ttu-id="31eee-120">Tyto součásti se automaticky pokusí vyhledat proxy server v síti.</span><span class="sxs-lookup"><span data-stu-id="31eee-120">These components automatically attempt to locate a proxy server on the network.</span></span> <span data-ttu-id="31eee-121">Používají Proxy Auto-Discovery WPAD (Web), pokud je povolena v prostředí.</span><span class="sxs-lookup"><span data-stu-id="31eee-121">They use Web Proxy Auto-Discovery (WPAD), if it's enabled in the environment.</span></span>

<span data-ttu-id="31eee-122">Součásti operačního systému se pokusí vyhledat proxy server pomocí vyhledávání DNS pro wpad.domainsuffix.</span><span class="sxs-lookup"><span data-stu-id="31eee-122">The OS components attempt to locate a proxy server by carrying out a DNS lookup for wpad.domainsuffix.</span></span> <span data-ttu-id="31eee-123">Pokud to řeší ve službě DNS, je IP adresa pro wpad.dat potom k požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="31eee-123">If this resolves in DNS, an HTTP request is then made to the IP address for wpad.dat.</span></span> <span data-ttu-id="31eee-124">Tento požadavek se změní na skript konfigurace proxy serveru ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="31eee-124">This request becomes the proxy configuration script in your environment.</span></span> <span data-ttu-id="31eee-125">Konektor používá tento skript k vyberte odchozí proxy server.</span><span class="sxs-lookup"><span data-stu-id="31eee-125">The connector uses this script to select an outbound proxy server.</span></span> <span data-ttu-id="31eee-126">Ale provoz konektor nemusí stále projít, z důvodu nastavení konfigurace na proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-126">However, connector traffic might still not go through, because of additional configuration settings needed on the proxy.</span></span>

<span data-ttu-id="31eee-127">Můžete nakonfigurovat konektor Nepoužívat proxy místní zajistit, že používá přímé připojení ke službám Azure.</span><span class="sxs-lookup"><span data-stu-id="31eee-127">You can configure the connector to bypass your on-premises proxy to ensure that it uses direct connectivity to the Azure services.</span></span> <span data-ttu-id="31eee-128">Doporučujeme, abyste tento přístup (Pokud je pro něj umožňuje zásady sítě), protože znamená, že máte jeden menší konfiguraci k údržbě.</span><span class="sxs-lookup"><span data-stu-id="31eee-128">We recommend this approach (if your network policy allows for it), because it means that you have one less configuration to maintain.</span></span>

<span data-ttu-id="31eee-129">Zakázat používání odchozího proxy serveru pro konektor, upravte soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config a přidat *system.net* části uvedené v této ukázce kódu:</span><span class="sxs-lookup"><span data-stu-id="31eee-129">To disable outbound proxy usage for the connector, edit the C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add the *system.net* section shown in this code sample:</span></span>

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
<span data-ttu-id="31eee-130">Zajistit, že službu Connector Updater také obchází proxy server, změňte podobné ApplicationProxyConnectorUpdaterService.exe.config umístěného v C:\Program Files\Microsoft AAD aplikace Proxy Connector Updater.</span><span class="sxs-lookup"><span data-stu-id="31eee-130">To ensure that the Connector Updater service also bypasses the proxy, make a similar change to the ApplicationProxyConnectorUpdaterService.exe.config file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span></span>

<span data-ttu-id="31eee-131">Ujistěte se, aby byly kopie aplikace původních souborů, v případě, že potřebujete obnovit výchozí soubory .config.</span><span class="sxs-lookup"><span data-stu-id="31eee-131">Be sure to make copies of the original files, in case you need to revert to the default .config files.</span></span>

## <a name="use-the-outbound-proxy-server"></a><span data-ttu-id="31eee-132">Použít odchozí proxy server</span><span class="sxs-lookup"><span data-stu-id="31eee-132">Use the outbound proxy server</span></span>

<span data-ttu-id="31eee-133">Některé prostředí vyžaduje všechny odchozí přenosy projít odchozího proxy serveru, bez výjimky.</span><span class="sxs-lookup"><span data-stu-id="31eee-133">Some environments require all outbound traffic to go through an outbound proxy, without exception.</span></span> <span data-ttu-id="31eee-134">V důsledku toho obcházení proxy serveru není možné.</span><span class="sxs-lookup"><span data-stu-id="31eee-134">As a result, bypassing the proxy is not an option.</span></span>

<span data-ttu-id="31eee-135">Konektor provoz projít odchozího proxy serveru, můžete nakonfigurovat, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="31eee-135">You can configure the connector traffic to go through the outbound proxy, as shown in the following diagram:</span></span>

 ![Konfigurace konektoru provoz projít odchozího proxy serveru, na proxy aplikace služby Azure AD](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

<span data-ttu-id="31eee-137">V důsledku s pouze odchozí přenosy, není nutné konfigurovat příchozí přístup přes vaší brány firewall.</span><span class="sxs-lookup"><span data-stu-id="31eee-137">As a result of having only outbound traffic, there's no need to configure inbound access through your firewalls.</span></span>

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a><span data-ttu-id="31eee-138">Krok 1: Konfigurace konektoru a související služby projít odchozího proxy serveru</span><span class="sxs-lookup"><span data-stu-id="31eee-138">Step 1: Configure the connector and related services to go through the outbound proxy</span></span>

<span data-ttu-id="31eee-139">Jak je popsané dříve, pokud je povoleno v prostředí a správně nakonfigurována WPAD, konektor automaticky zjišťovat odchozí proxy server a pokus o použití ho.</span><span class="sxs-lookup"><span data-stu-id="31eee-139">As covered earlier, if WPAD is enabled in the environment and configured appropriately, the connector will automatically discover the outbound proxy server and attempt to use it.</span></span> <span data-ttu-id="31eee-140">Můžete však explicitně nakonfigurovat konektor projít odchozího proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-140">However, you can explicitly configure the connector to go through an outbound proxy.</span></span>

<span data-ttu-id="31eee-141">Uděláte to tak, upravte soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config a přidat *system.net* části uvedené v této ukázce kódu.</span><span class="sxs-lookup"><span data-stu-id="31eee-141">To do so, edit the C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add the *system.net* section shown in this code sample.</span></span> <span data-ttu-id="31eee-142">Změna *proxyserver:8080* tak, aby odrážela název místní proxy serveru nebo IP adresu a port, který naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="31eee-142">Change *proxyserver:8080* to reflect your local proxy server name or IP address, and the port that it's listening on.</span></span>

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

<span data-ttu-id="31eee-143">V dalším kroku nakonfigurujte službu Connector Updater na používání proxy serveru tak, že podobné změny do souboru, složce C:\Program Files\Microsoft AAD aplikace proxy serveru konektoru Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span><span class="sxs-lookup"><span data-stu-id="31eee-143">Next, configure the Connector Updater service to use the proxy by making a similar change to the file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span></span>

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a><span data-ttu-id="31eee-144">Krok 2: Konfigurace proxy serveru, který chcete povolit přenosy z konektoru a související služby k procházet skrz</span><span class="sxs-lookup"><span data-stu-id="31eee-144">Step 2: Configure the proxy to allow traffic from the connector and related services to flow through</span></span>

<span data-ttu-id="31eee-145">Existují čtyři aspekty, které je třeba zvážit v odchozího proxy serveru:</span><span class="sxs-lookup"><span data-stu-id="31eee-145">There are four aspects to consider at the outbound proxy:</span></span>
* <span data-ttu-id="31eee-146">Odchozí pravidla proxy</span><span class="sxs-lookup"><span data-stu-id="31eee-146">Proxy outbound rules</span></span>
* <span data-ttu-id="31eee-147">Ověřování proxy serverem</span><span class="sxs-lookup"><span data-stu-id="31eee-147">Proxy authentication</span></span>
* <span data-ttu-id="31eee-148">Porty proxy</span><span class="sxs-lookup"><span data-stu-id="31eee-148">Proxy ports</span></span>
* <span data-ttu-id="31eee-149">Kontrolu SSL</span><span class="sxs-lookup"><span data-stu-id="31eee-149">SSL inspection</span></span>

#### <a name="proxy-outbound-rules"></a><span data-ttu-id="31eee-150">Odchozí pravidla proxy</span><span class="sxs-lookup"><span data-stu-id="31eee-150">Proxy outbound rules</span></span>
<span data-ttu-id="31eee-151">Povolit přístup k vytvoření následujících koncových bodů pro přístup k službě konektor:</span><span class="sxs-lookup"><span data-stu-id="31eee-151">Allow access to the following endpoints for connector service access:</span></span>

* <span data-ttu-id="31eee-152">*. msappproxy.net</span><span class="sxs-lookup"><span data-stu-id="31eee-152">*.msappproxy.net</span></span>
* <span data-ttu-id="31eee-153">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="31eee-153">*.servicebus.windows.net</span></span>

<span data-ttu-id="31eee-154">Pro počáteční registraci povolte přístup k vytvoření následujících koncových bodů:</span><span class="sxs-lookup"><span data-stu-id="31eee-154">For initial registration, allow access to the following endpoints:</span></span>

* <span data-ttu-id="31eee-155">login.windows.net</span><span class="sxs-lookup"><span data-stu-id="31eee-155">login.windows.net</span></span>
* <span data-ttu-id="31eee-156">Login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="31eee-156">login.microsoftonline.com</span></span>

<span data-ttu-id="31eee-157">Pokud nemůžete povolit připojení ve plně kvalifikovaný název domény a je nutné místo toho zadat rozsahy IP adres, použijte tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="31eee-157">If you can't allow connectivity by FQDN and need to specify IP ranges instead, use these options:</span></span>

* <span data-ttu-id="31eee-158">Povolí odchozí přístup konektoru na všechna místa určení.</span><span class="sxs-lookup"><span data-stu-id="31eee-158">Allow the connector outbound access to all destinations.</span></span>
* <span data-ttu-id="31eee-159">Povolit konektor odchozí přístup k [rozsahy IP adres Azure datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="31eee-159">Allow the connector outbound access to [Azure datacenter IP ranges](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span></span> <span data-ttu-id="31eee-160">Problém s použitím seznamu rozsahů IP adres Azure datacenter je, že je aktualizovaný týdně.</span><span class="sxs-lookup"><span data-stu-id="31eee-160">The challenge with using the list of Azure datacenter IP ranges is that it's updated weekly.</span></span> <span data-ttu-id="31eee-161">Musíte uvést proces k zajištění, že jsou vaše pravidla přístupu k příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="31eee-161">You need to put a process in place to ensure that your access rules are updated accordingly.</span></span>

#### <a name="proxy-authentication"></a><span data-ttu-id="31eee-162">Ověřování proxy serverem</span><span class="sxs-lookup"><span data-stu-id="31eee-162">Proxy authentication</span></span>

<span data-ttu-id="31eee-163">Ověřování proxy není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="31eee-163">Proxy authentication is not currently supported.</span></span> <span data-ttu-id="31eee-164">Naše doporučení aktuální je umožnit konektor anonymní přístup k Internetu cíle.</span><span class="sxs-lookup"><span data-stu-id="31eee-164">Our current recommendation is to allow the connector anonymous access to the Internet destinations.</span></span>

#### <a name="proxy-ports"></a><span data-ttu-id="31eee-165">Porty proxy</span><span class="sxs-lookup"><span data-stu-id="31eee-165">Proxy ports</span></span>

<span data-ttu-id="31eee-166">Konektor umožňuje odchozí připojení založené na protokolu SSL pomocí metody připojení.</span><span class="sxs-lookup"><span data-stu-id="31eee-166">The connector makes outbound SSL-based connections by using the CONNECT method.</span></span> <span data-ttu-id="31eee-167">Tato metoda je v podstatě nastavuje tunelové propojení prostřednictvím odchozího proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-167">This method essentially sets up a tunnel through the outbound proxy.</span></span> <span data-ttu-id="31eee-168">Konfigurace proxy serveru tak, aby tunelového propojení pro porty 443 a 80.</span><span class="sxs-lookup"><span data-stu-id="31eee-168">Configure the proxy server to allow tunneling to ports 443 and 80.</span></span>

>[!NOTE]
><span data-ttu-id="31eee-169">Spuštění služby Service Bus přes protokol HTTPS používá port 443.</span><span class="sxs-lookup"><span data-stu-id="31eee-169">When Service Bus runs over HTTPS, it uses port 443.</span></span> <span data-ttu-id="31eee-170">Ve výchozím nastavení, ale Service Bus pokusí přímé připojení TCP a spadne zpět na HTTPS pouze v případě, že přímé připojení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="31eee-170">However, by default, Service Bus attempts direct TCP connections and falls back to HTTPS only if direct connectivity fails.</span></span>

<span data-ttu-id="31eee-171">Aby se zajistilo, že Service Bus se také odesílá přes odchozí proxy serveru, ověřte, zda konektor nelze připojit přímo ke službám Azure pro porty 9350, 9352 a 5671.</span><span class="sxs-lookup"><span data-stu-id="31eee-171">To ensure that the Service Bus traffic is also sent through the outbound proxy server, ensure that the connector cannot directly connect to the Azure services for ports 9350, 9352, and 5671.</span></span>

#### <a name="ssl-inspection"></a><span data-ttu-id="31eee-172">Kontrolu SSL</span><span class="sxs-lookup"><span data-stu-id="31eee-172">SSL inspection</span></span>
<span data-ttu-id="31eee-173">Pro provoz konektor, nepoužívejte kontrolu SSL, protože způsobuje problémy týkající se provozu konektor.</span><span class="sxs-lookup"><span data-stu-id="31eee-173">Do not use SSL inspection for the connector traffic, because it causes problems for the connector traffic.</span></span>

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a><span data-ttu-id="31eee-174">Řešení potíží s konektor proxy problémy a potíže s připojením služby</span><span class="sxs-lookup"><span data-stu-id="31eee-174">Troubleshoot connector proxy problems and service connectivity issues</span></span>
<span data-ttu-id="31eee-175">Nyní byste měli vidět veškerý provoz prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-175">Now you should see all traffic flowing through the proxy.</span></span> <span data-ttu-id="31eee-176">Pokud máte potíže, by měly pomoci následující informace o odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="31eee-176">If you have problems, the following troubleshooting information should help.</span></span>

<span data-ttu-id="31eee-177">Nejlepší způsob, jak identifikovat a řešit problémy s připojením k konektor je převést zachycení dat ze sítě na službu konektoru při spouštění služby konektoru.</span><span class="sxs-lookup"><span data-stu-id="31eee-177">The best way to identify and troubleshoot connector connectivity issues is to take a network capture on the connector service while starting the connector service.</span></span> <span data-ttu-id="31eee-178">To může být složitý úkol, takže Podíváme se na rychlé tipy k zaznamenání a filtrování trasování sítě.</span><span class="sxs-lookup"><span data-stu-id="31eee-178">This can be a daunting task, so let’s look at quick tips on capturing and filtering network traces.</span></span>

<span data-ttu-id="31eee-179">Můžete použít nástroj monitorování podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="31eee-179">You can use the monitoring tool of your choice.</span></span> <span data-ttu-id="31eee-180">Pro účely tohoto článku použili jsme Microsoft Network Monitor 3.4.</span><span class="sxs-lookup"><span data-stu-id="31eee-180">For the purposes of this article, we used Microsoft Network Monitor 3.4.</span></span> <span data-ttu-id="31eee-181">Můžete [ji stáhnout z Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span><span class="sxs-lookup"><span data-stu-id="31eee-181">You can [download it from Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span></span>

<span data-ttu-id="31eee-182">Příklady a filtry, které používáme v následujících částech jsou specifické pro sledování sítě, ale zásady lze použít pro nástroj pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="31eee-182">The examples and filters that we use in the following sections are specific to Network Monitor, but the principles can be applied to any analysis tool.</span></span>

### <a name="take-a-capture-by-using-network-monitor"></a><span data-ttu-id="31eee-183">Proveďte zachycení pomocí sledování sítě</span><span class="sxs-lookup"><span data-stu-id="31eee-183">Take a capture by using Network Monitor</span></span>

<span data-ttu-id="31eee-184">Spuštění zachycení:</span><span class="sxs-lookup"><span data-stu-id="31eee-184">To start a capture:</span></span>

1. <span data-ttu-id="31eee-185">Otevřete sledování sítě a klikněte na tlačítko **nové zachycení**.</span><span class="sxs-lookup"><span data-stu-id="31eee-185">Open Network Monitor and click **New Capture**.</span></span>
2. <span data-ttu-id="31eee-186">Klikněte **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="31eee-186">Click the **Start** button.</span></span>

   ![Okno monitorování sítě](./media/application-proxy-working-with-proxy-servers/network-capture.png)

<span data-ttu-id="31eee-188">Po dokončení zachycení, klikněte **Zastavit** tlačítko ukončete ji.</span><span class="sxs-lookup"><span data-stu-id="31eee-188">After you complete a capture, click the **Stop** button to end it.</span></span>

### <a name="take-a-capture-of-connector-traffic"></a><span data-ttu-id="31eee-189">Proveďte zachycení provozu konektoru</span><span class="sxs-lookup"><span data-stu-id="31eee-189">Take a capture of connector traffic</span></span>

<span data-ttu-id="31eee-190">Pro počáteční řešení potíží, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="31eee-190">For initial troubleshooting, perform the following steps:</span></span>

1. <span data-ttu-id="31eee-191">Ze souboru services.msc zastavte službu konektoru Proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="31eee-191">From services.msc, stop the Azure AD Application Proxy Connector service.</span></span>
2. <span data-ttu-id="31eee-192">Spusťte zachycení dat ze sítě.</span><span class="sxs-lookup"><span data-stu-id="31eee-192">Start the network capture.</span></span>
3. <span data-ttu-id="31eee-193">Spusťte službu konektoru Proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="31eee-193">Start the Azure AD Application Proxy Connector service.</span></span>
4. <span data-ttu-id="31eee-194">Zastavte zachycení dat ze sítě.</span><span class="sxs-lookup"><span data-stu-id="31eee-194">Stop the network capture.</span></span>

   ![Služba Azure AD konektor Proxy aplikace v services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-the-requests-from-the-connector-to-the-proxy-server"></a><span data-ttu-id="31eee-196">Podívejte se na žádosti z konektoru k proxy serveru</span><span class="sxs-lookup"><span data-stu-id="31eee-196">Look at the requests from the connector to the proxy server</span></span>

<span data-ttu-id="31eee-197">Teď, když máte k dispozici zachycení dat ze sítě, jste připravení ho filtrovat.</span><span class="sxs-lookup"><span data-stu-id="31eee-197">Now that you’ve got a network capture, you're ready to filter it.</span></span> <span data-ttu-id="31eee-198">Klíč k prohlížení trasování je pochopení, jak filtrovat zachytávání.</span><span class="sxs-lookup"><span data-stu-id="31eee-198">The key to looking at the trace is understanding how to filter the capture.</span></span>

<span data-ttu-id="31eee-199">Jeden filtr je následující (kde 8080 je port proxy serveru služby):</span><span class="sxs-lookup"><span data-stu-id="31eee-199">One filter is as follows (where 8080 is the proxy service port):</span></span>

<span data-ttu-id="31eee-200">**(http. Požadavek nebo http. Odpověď) a tcp.port==8080**</span><span class="sxs-lookup"><span data-stu-id="31eee-200">**(http.Request or http.Response) and tcp.port==8080**</span></span>

<span data-ttu-id="31eee-201">Pokud zadáte tento filtr v **filtru zobrazení** a vyberte **použít**, filtruje zachycená data podle filtru.</span><span class="sxs-lookup"><span data-stu-id="31eee-201">If you enter this filter in the **Display Filter** window and select **Apply**, it filters the captured traffic based on the filter.</span></span>

<span data-ttu-id="31eee-202">Předchozí filtr zobrazuje jenom požadavky a odpovědi HTTP z port proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-202">The preceding filter shows just the HTTP requests and responses to/from the proxy port.</span></span> <span data-ttu-id="31eee-203">Pro spuštění konektoru konfigurovaným konektor používat proxy server by filtr zobrazit přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="31eee-203">For a connector startup where the connector is configured to use a proxy server, the filter would show something like this:</span></span>

 ![Příklad seznamu Filtrované požadavky a odpovědi HTTP](./media/application-proxy-working-with-proxy-servers/http-requests.png)

<span data-ttu-id="31eee-205">Nyní konkrétně díváte pro požadavky připojení, které se zobrazí komunikace se serverem proxy.</span><span class="sxs-lookup"><span data-stu-id="31eee-205">You're now specifically looking for the CONNECT requests that show communication with the proxy server.</span></span> <span data-ttu-id="31eee-206">Po úspěšné zobrazí se odpověď HTTP OK (200).</span><span class="sxs-lookup"><span data-stu-id="31eee-206">Upon success, you get an HTTP OK (200) response.</span></span>

<span data-ttu-id="31eee-207">Pokud se zobrazí další kódy odpovědí, jako je například 407 nebo 502, proxy server je vyžadování ověřování nebo že nepovolí provoz z jiného důvodu.</span><span class="sxs-lookup"><span data-stu-id="31eee-207">If you see other response codes, such as 407 or 502, the proxy is requiring authentication or not allowing the traffic for some other reason.</span></span> <span data-ttu-id="31eee-208">V tomto okamžiku zaujmout váš tým podpory proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="31eee-208">At this point, you engage your proxy server support team.</span></span>

### <a name="identify-failed-tcp-connection-attempts"></a><span data-ttu-id="31eee-209">Identifikovat neúspěšné pokusy o připojení protokolu TCP</span><span class="sxs-lookup"><span data-stu-id="31eee-209">Identify failed TCP connection attempts</span></span>

<span data-ttu-id="31eee-210">Další běžné scénáře, který vás může zajímat je když tento konektor se snaží připojit přímo, ale selhává.</span><span class="sxs-lookup"><span data-stu-id="31eee-210">The other common scenario that you may be interested in is when the connector is trying to connect directly, but it's failing.</span></span>

<span data-ttu-id="31eee-211">Jiný filtr sledování sítě, který vám umožní snadno identifikovat tento problém je:</span><span class="sxs-lookup"><span data-stu-id="31eee-211">Another Network Monitor filter that helps you to easily identify this problem is:</span></span>

<span data-ttu-id="31eee-212">**Vlastnost. TCPSynRetransmit**</span><span class="sxs-lookup"><span data-stu-id="31eee-212">**property.TCPSynRetransmit**</span></span>

<span data-ttu-id="31eee-213">Paket SYN je první paket odeslaný k navázání připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="31eee-213">A SYN packet is the first packet sent to establish a TCP connection.</span></span> <span data-ttu-id="31eee-214">Pokud tomuto paketu nevrací odpověď, je reattempted SYN.</span><span class="sxs-lookup"><span data-stu-id="31eee-214">If this packet doesn’t return a response, the SYN is reattempted.</span></span> <span data-ttu-id="31eee-215">Předchozí filtrem můžete zobrazit všechny požadavky SYN opakovaně.</span><span class="sxs-lookup"><span data-stu-id="31eee-215">You can use the preceding filter to see any retransmitted SYNs.</span></span> <span data-ttu-id="31eee-216">Potom můžete zkontrolovat, zda tyto požadavky SYN odpovídají přenosy dat souvisejících s konektoru.</span><span class="sxs-lookup"><span data-stu-id="31eee-216">Then, you can check whether these SYNs correspond to any connector-related traffic.</span></span>

<span data-ttu-id="31eee-217">Následující příklad ukazuje, pokus o selhání připojení k Service Bus port 9352:</span><span class="sxs-lookup"><span data-stu-id="31eee-217">The following example shows a failed connection attempt to Service Bus port 9352:</span></span>

 ![Příklad odpověď pro pokus o připojení se nezdařilo](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

<span data-ttu-id="31eee-219">Pokud se něco podobného jako předchozí odpovědi, konektor se pokouší komunikovat přímo se službou Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="31eee-219">If you see something like the preceding response, the connector is trying to communicate directly with the Azure Service Bus service.</span></span> <span data-ttu-id="31eee-220">Pokud očekáváte, konektor navázat přímé připojení ke službám Azure, je tato odezva jasný náznak, že máte síť nebo brána firewall problému.</span><span class="sxs-lookup"><span data-stu-id="31eee-220">If you expect the connector to make direct connections to the Azure services, this response is a clear indication that you have a network or firewall problem.</span></span>

>[!NOTE]
><span data-ttu-id="31eee-221">Pokud je nakonfigurována k používání proxy serveru, tato odpověď může znamenat, že Service Bus se pokouší o přímé připojení TCP před přepnutím do pokusu o připojení přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="31eee-221">If you are configured to use a proxy server, this response might mean that Service Bus is attempting a direct TCP connection before switching to attempting a connection over HTTPS.</span></span>
>

<span data-ttu-id="31eee-222">Analýzy trasování sítě není u všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="31eee-222">Network trace analysis is not for everyone.</span></span> <span data-ttu-id="31eee-223">Ale může být cenným nástrojem pro rychlé informace o co se děje s vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="31eee-223">But it can be a valuable tool to get quick information about what's going on with your network.</span></span>

<span data-ttu-id="31eee-224">Pokud budete pokračovat, potíže se čtením s problémy s připojením k konektor, vytvořte lístek s náš tým podpory.</span><span class="sxs-lookup"><span data-stu-id="31eee-224">If you continue to struggle with connector connectivity issues, create a ticket with our support team.</span></span> <span data-ttu-id="31eee-225">Tým vám mohou pomoci s další informace o řešení.</span><span class="sxs-lookup"><span data-stu-id="31eee-225">The team can assist you with further troubleshooting.</span></span>

<span data-ttu-id="31eee-226">Informace o řešení chyb s konektor Proxy aplikace najdete v tématu [Poradce při potížích s Proxy aplikace](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="31eee-226">For information about resolving errors with Application Proxy Connector, see [Troubleshoot Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span></span>

## <a name="next-steps"></a><span data-ttu-id="31eee-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31eee-227">Next steps</span></span>

[<span data-ttu-id="31eee-228">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="31eee-228">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)<br><span data-ttu-id="31eee-229">
[Postup při bezobslužné instalaci konektoru Proxy aplikace Azure AD](active-directory-application-proxy-silent-installation.md)</span><span class="sxs-lookup"><span data-stu-id="31eee-229">
[How to silently install the Azure AD Application Proxy Connector ](active-directory-application-proxy-silent-installation.md)</span></span>
