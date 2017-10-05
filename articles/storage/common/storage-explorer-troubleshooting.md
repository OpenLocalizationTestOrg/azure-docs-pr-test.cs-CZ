---
title: "Azure Storage Explorer Průvodci odstraňováním potíží | Microsoft Docs"
description: "Přehled dvou ladění funkcí Azure"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: delhan
ms.openlocfilehash: 470b2d87ffdc4769bb2963df7dea646901469e00
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a><span data-ttu-id="7cb8d-103">Azure Storage Explorer Průvodci odstraňováním potíží</span><span class="sxs-lookup"><span data-stu-id="7cb8d-103">Azure Storage Explorer troubleshooting guide</span></span>

## <a name="introduction"></a><span data-ttu-id="7cb8d-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="7cb8d-104">Introduction</span></span>

<span data-ttu-id="7cb8d-105">Microsoft Azure Storage Explorer (Preview) je samostatná aplikace, která umožňuje snadno pracovat s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-105">Microsoft Azure Storage Explorer (Preview) is a stand-alone app that enables you to easily work with Azure Storage data on Windows, macOS and Linux.</span></span> <span data-ttu-id="7cb8d-106">Aplikace se může připojit toStorage účty hostované na Azure, suverénní Cloudy a Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-106">The app can connect toStorage accounts hosted on Azure, Sovereign Clouds, and Azure Stack.</span></span>

<span data-ttu-id="7cb8d-107">Tato příručka obsahuje souhrn řešení pro běžné problémy, které jsou vidět ve Storage Exploreru.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-107">This guide summarizes solutions for common issues seen in Storage Explorer.</span></span>

## <a name="sign-in-issues"></a><span data-ttu-id="7cb8d-108">Přihlaste se problémy</span><span class="sxs-lookup"><span data-stu-id="7cb8d-108">Sign in issues</span></span>

<span data-ttu-id="7cb8d-109">Než budete pokračovat, zkuste restartovat aplikaci a zjistit, zda lze dlouhodobý problémy.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-109">Before you continue, try restarting your application and see whether the problems can be fixed.</span></span>

### <a name="error-self-signed-certificate-in-certificate-chain"></a><span data-ttu-id="7cb8d-110">Chyba: Certifikát podepsaný svým držitelem v řetězu certifikátů</span><span class="sxs-lookup"><span data-stu-id="7cb8d-110">Error: Self-Signed Certificate in Certificate Chain</span></span>

<span data-ttu-id="7cb8d-111">Tady je několik důvodů, proč této chybě může dojít, a nejběžnější dva důvody jsou následující:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-111">There are several reasons why you may encounter this error, and the most common two reasons are as follows:</span></span>

1. <span data-ttu-id="7cb8d-112">Aplikace je připojený prostřednictvím "transparentní proxy", což znamená, brání komunikaci přes protokol HTTPS, dešifrování ho a pak šifrování pomocí certifikát podepsaný svým držitelem serveru (například serveru vaší společnosti).</span><span class="sxs-lookup"><span data-stu-id="7cb8d-112">The app is connected through a “transparent proxy”, which means a server (such as your company server) is intercepting HTTPS traffic, decrypting it, and then encrypting it using a self-signed certificate.</span></span>

2. <span data-ttu-id="7cb8d-113">Běží aplikace, jako je antivirový software, který je podepsaný certifikát SSL vložení do zpráv protokolu HTTPS, které jste dostali.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-113">You are running an application, such as antivirus software, which is injecting a self-signed SSL certificate into the HTTPS messages that you receive.</span></span>

<span data-ttu-id="7cb8d-114">Když Storage Explorer dojde jeden z problémů, můžete již nebude vědět, jestli je úmyslně přijatou zprávu protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-114">When Storage Explorer encounters one of the issues, it can no longer know whether the received HTTPS message is tampered.</span></span> <span data-ttu-id="7cb8d-115">Pokud máte kopie certifikátu podepsaného svým držitelem, můžete je nechat Storage Explorer důvěřujete mu.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-115">If you have a copy of the self-signed certificate, you can let Storage Explorer trust it.</span></span> <span data-ttu-id="7cb8d-116">Pokud si nejste jisti kdo je vložení certifikát, použijte následující postup ji najít:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-116">If you are unsure of who is injecting the certificate, follow these steps to find it:</span></span>

1. <span data-ttu-id="7cb8d-117">Instalace otevřete SSL</span><span class="sxs-lookup"><span data-stu-id="7cb8d-117">Install Open SSL</span></span>

    - <span data-ttu-id="7cb8d-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (jakékoli světla verze by mělo být dostatečné)</span><span class="sxs-lookup"><span data-stu-id="7cb8d-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (any of the light versions should be sufficient)</span></span>

    - <span data-ttu-id="7cb8d-119">Mac a Linux: by měly být zahrnuty s operačním systémem</span><span class="sxs-lookup"><span data-stu-id="7cb8d-119">Mac and Linux: should be included with your operating system</span></span>

2. <span data-ttu-id="7cb8d-120">Spustit otevřete SSL</span><span class="sxs-lookup"><span data-stu-id="7cb8d-120">Run Open SSL</span></span>

    - <span data-ttu-id="7cb8d-121">Windows: otevřete instalační adresář, klikněte na **/bin/**a potom dvakrát klikněte na **openssl.exe**.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-121">Windows: open the installation directory, click **/bin/**, and then double-click **openssl.exe**.</span></span>
    - <span data-ttu-id="7cb8d-122">Mac a Linux: Spusťte **openssl** z terminálu.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-122">Mac and Linux: run **openssl** from a terminal.</span></span>

3. <span data-ttu-id="7cb8d-123">Spuštění s_client - showcerts-připojení microsoft.com:443</span><span class="sxs-lookup"><span data-stu-id="7cb8d-123">Execute s_client -showcerts -connect microsoft.com:443</span></span>

4. <span data-ttu-id="7cb8d-124">Hledejte certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-124">Look for self-signed certificates.</span></span> <span data-ttu-id="7cb8d-125">Pokud si nejste jistí, které jsou podepsané svým držitelem, vyhledejte kdekoli subjektu ("s:") a vystavitele ("i") jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-125">If you are unsure which are self-signed, look for anywhere the subject ("s:") and issuer ("i:") are the same.</span></span>

5. <span data-ttu-id="7cb8d-126">Po nalezení všechny certifikáty podepsané svým držitelem pro každé z nich, zkopírujte a vložte všechno z a to včetně **---BEGIN CERTIFICATE---** k **---END CERTIFICATE---** do nového souboru .cer.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-126">When you have found any self-signed certificates, for each one, copy and paste everything from and including **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** to a new .cer file.</span></span>

6. <span data-ttu-id="7cb8d-127">Otevřete Storage Explorer, klikněte na **upravit** > **certifikáty SSL** > **importu certifikátů**a potom pomocí nástroje pro výběr souborů najít, vyberte a otevřete .cer soubory, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-127">Open Storage Explorer, click **Edit** > **SSL Certificates** > **Import Certificates**, and then use the file picker to find, select, and open the .cer files that you created.</span></span>

<span data-ttu-id="7cb8d-128">Pokud nenajdete žádné certifikáty podepsané svým držitelem pomocí výše uvedené kroky, kontaktujte nás pomocí nástroje zpětnou vazbu o další pomoc.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-128">If you cannot find any self-signed certificates using the above steps, contact us through the feedback tool for more help.</span></span>

### <a name="unable-to-retrieve-subscriptions"></a><span data-ttu-id="7cb8d-129">Nelze načíst předplatná</span><span class="sxs-lookup"><span data-stu-id="7cb8d-129">Unable to retrieve subscriptions</span></span>

<span data-ttu-id="7cb8d-130">Pokud jste se nepodařilo načíst vašich předplatných, po úspěšném přihlášení, použijte následující postup řešení tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-130">If you are unable to retrieve your subscriptions after you successfully sign in, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="7cb8d-131">Ověřte, že má váš účet přístup k předplatným po přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-131">Verify that your account has access to the subscriptions by signing into the Azure Portal.</span></span>

- <span data-ttu-id="7cb8d-132">Ujistěte se, že jste se zaregistrovali pomocí správné prostředí (Azure, Azure China, Azure v Německu, Azure US Government nebo vlastní prostředí nebo Azure zásobníku).</span><span class="sxs-lookup"><span data-stu-id="7cb8d-132">Make sure that you have signed in using the correct environment (Azure, Azure China, Azure Germany, Azure US Government, or Custom Environment/Azure Stack).</span></span>

- <span data-ttu-id="7cb8d-133">Pokud se nacházíte za proxy, ujistěte se, že jste nakonfigurovali proxy Storage Explorer správně.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-133">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="7cb8d-134">Zkuste odebrat a nové přidání účtu.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-134">Try removing and readding the account.</span></span>

- <span data-ttu-id="7cb8d-135">Pokuste se odstranit následující soubory z kořenového adresáře (tedy C:\Users\ContosoUser) a potom znovu přidat účet:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-135">Try deleting the following files from your root directory (that is, C:\Users\ContosoUser), and then re-adding the account:</span></span>

    - <span data-ttu-id="7cb8d-136">.adalcache</span><span class="sxs-lookup"><span data-stu-id="7cb8d-136">.adalcache</span></span>

    - <span data-ttu-id="7cb8d-137">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="7cb8d-137">.devaccounts</span></span>

    - <span data-ttu-id="7cb8d-138">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="7cb8d-138">.extaccounts</span></span>

- <span data-ttu-id="7cb8d-139">Kukátko nástrojů pro vývojáře, (podle stisknutím klávesy F12), pokud se přihlašujete všechny chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-139">Watch the developer tools console (by pressing F12) when you are signing in for any error messages:</span></span>

![Nástroje pro vývojáře](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a><span data-ttu-id="7cb8d-141">Nelze zobrazit stránku ověřování</span><span class="sxs-lookup"><span data-stu-id="7cb8d-141">Unable to see the authentication page</span></span>

<span data-ttu-id="7cb8d-142">Pokud nelze zobrazit stránku ověřování, použijte následující postup řešení tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-142">If you are unable to see the authentication page, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="7cb8d-143">V závislosti na rychlosti připojení může trvat nějakou dobu stránku přihlášení a načíst, počkejte před jeho zavřením dialogové okno ověřování alespoň jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-143">Depending on the speed of your connection, it may take a while for the sign-in page to load, wait at least one minute before closing the authentication dialog box.</span></span>

- <span data-ttu-id="7cb8d-144">Pokud se nacházíte za proxy, ujistěte se, že jste nakonfigurovali proxy Storage Explorer správně.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-144">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="7cb8d-145">Zobrazení konzole pro vývojáře stisknutím klávesy F12.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-145">View the developer console by pressing the F12 key.</span></span> <span data-ttu-id="7cb8d-146">Podívejte se na odpovědi z konzole pro vývojáře a zobrazit, zda můžete najít všechny potvrzením pro důvod, proč ověřování nepracuje.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-146">Watch the responses from the developer console and see whether you can find any clue for why authentication not working.</span></span>

### <a name="cannot-remove-account"></a><span data-ttu-id="7cb8d-147">Nelze odebrat účet</span><span class="sxs-lookup"><span data-stu-id="7cb8d-147">Cannot remove account</span></span>

<span data-ttu-id="7cb8d-148">Pokud se nepodařilo odebrat účet, nebo pokud znovu ověřit propojení nemá žádný, použijte následující postup řešení tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-148">If you are unable to remove an account, or if the reauthenticate link does not do anything, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="7cb8d-149">Pokuste se odstranit následující soubory z kořenového adresáře a pak nové přidání účtu:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-149">Try deleting the following files from your root directory, and then readding the account:</span></span>

    - <span data-ttu-id="7cb8d-150">.adalcache</span><span class="sxs-lookup"><span data-stu-id="7cb8d-150">.adalcache</span></span>

    - <span data-ttu-id="7cb8d-151">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="7cb8d-151">.devaccounts</span></span>

    - <span data-ttu-id="7cb8d-152">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="7cb8d-152">.extaccounts</span></span>

- <span data-ttu-id="7cb8d-153">Pokud chcete odebrat SAS připojená prostředky úložiště, odstraňte následující soubory:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-153">If you want to remove SAS attached Storage resources, delete the following files:</span></span>

    - <span data-ttu-id="7cb8d-154">%AppData%/StorageExplorer složky pro Windows</span><span class="sxs-lookup"><span data-stu-id="7cb8d-154">%AppData%/StorageExplorer folder for Windows</span></span>

    - <span data-ttu-id="7cb8d-155">/Users/ < vaše_jméno >/knihovny/Express podporu nebo StorageExplorer pro Mac</span><span class="sxs-lookup"><span data-stu-id="7cb8d-155">/Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer for Mac</span></span>

    - <span data-ttu-id="7cb8d-156">~/.config/StorageExplorer pro Linux</span><span class="sxs-lookup"><span data-stu-id="7cb8d-156">~/.config/StorageExplorer for Linux</span></span>

> [!NOTE]
>  <span data-ttu-id="7cb8d-157">Budete muset znovu zadat všechny přihlašovací údaje, pokud odstraníte tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-157">You will have to reenter all your credentials if you delete these files.</span></span>

## <a name="proxy-issues"></a><span data-ttu-id="7cb8d-158">Problémy s proxy</span><span class="sxs-lookup"><span data-stu-id="7cb8d-158">Proxy issues</span></span>

<span data-ttu-id="7cb8d-159">První zajistěte, aby byla následující informace, které jste zadali správně:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-159">First, make sure that the following information you entered are all correct:</span></span>

- <span data-ttu-id="7cb8d-160">Adresa URL proxy serveru a číslo portu</span><span class="sxs-lookup"><span data-stu-id="7cb8d-160">The proxy URL and port number</span></span>

- <span data-ttu-id="7cb8d-161">Uživatelské jméno a heslo, pokud to vyžaduje proxy server</span><span class="sxs-lookup"><span data-stu-id="7cb8d-161">Username and password if required by the proxy</span></span>

### <a name="common-solutions"></a><span data-ttu-id="7cb8d-162">Běžná řešení</span><span class="sxs-lookup"><span data-stu-id="7cb8d-162">Common solutions</span></span>

<span data-ttu-id="7cb8d-163">Pokud stále dochází k problémům, použijte následující postup řešení potíží s je:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-163">If you are still experiencing issues, follow these steps to troubleshoot them:</span></span>

- <span data-ttu-id="7cb8d-164">Pokud se můžete připojit k Internetu bez použití proxy, ověřte, že Storage Explorer funguje bez povolené nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-164">If you can connect to the Internet without using your proxy, verify that Storage Explorer works without proxy settings enabled.</span></span> <span data-ttu-id="7cb8d-165">Pokud je to tento případ, může být problém se vaše nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-165">If this is the case, there may be an issue with your proxy settings.</span></span> <span data-ttu-id="7cb8d-166">Práce se na správce serveru proxy a identifikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-166">Work with your proxy administrator to identify the problems.</span></span>

- <span data-ttu-id="7cb8d-167">Ověřte, že jiných aplikací pomocí proxy serveru fungovat podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-167">Verify that other applications using the proxy server work as expected.</span></span>

- <span data-ttu-id="7cb8d-168">Ověřte, zda se můžete připojit k portálu Microsoft Azure pomocí webového prohlížeče</span><span class="sxs-lookup"><span data-stu-id="7cb8d-168">Verify that you can connect to the Microsoft Azure portal using your web browser</span></span>

- <span data-ttu-id="7cb8d-169">Ověřte, zda se zobrazila odpovědí z koncových bodů služby.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-169">Verify that you can receive responses from your service endpoints.</span></span> <span data-ttu-id="7cb8d-170">Zadejte jednu z váš koncový bod adresy URL do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-170">Enter one of your endpoint URLs into your browser.</span></span> <span data-ttu-id="7cb8d-171">Pokud se můžete připojit, měli byste obdržet InvalidQueryParameterValue nebo podobné odpovědi ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-171">If you can connect, you should receive an InvalidQueryParameterValue or similar XML response.</span></span>

- <span data-ttu-id="7cb8d-172">Pokud někdo jiný používá také Storage Explorer proxy serveru, ověřte, zda se můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-172">If someone else is also using Storage Explorer with your proxy server, verify that they can connect.</span></span> <span data-ttu-id="7cb8d-173">Pokud se můžete připojit, možná budete muset obrátit na správce serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-173">If they can connect, you may have to contact your proxy server admin.</span></span>

### <a name="tools-for-diagnosing-issues"></a><span data-ttu-id="7cb8d-174">Nástroje pro diagnostiku problémů</span><span class="sxs-lookup"><span data-stu-id="7cb8d-174">Tools for diagnosing issues</span></span>

<span data-ttu-id="7cb8d-175">Pokud máte síťové nástroje, například aplikaci Fiddler pro Windows, bude pravděpodobně možné diagnostikovat problémy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-175">If you have networking tools, such as Fiddler for Windows, you may be able to diagnose the problems as follows:</span></span>

- <span data-ttu-id="7cb8d-176">Pokud máte fungovat prostřednictvím proxy, možná budete muset nakonfigurovat vaše síťové nástroje pro připojení prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-176">If you have to work through your proxy, you may have to configure your networking tool to connect through the proxy.</span></span>

- <span data-ttu-id="7cb8d-177">Zkontrolujte číslo portu používané nástrojem pro vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-177">Check the port number used by your networking tool.</span></span>

- <span data-ttu-id="7cb8d-178">Zadejte adresu URL místního hostitele a číslo portu sítě nástroj jako nastavení proxy serveru v Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-178">Enter the local host URL and the networking tool's port number as proxy settings in Storage Explorer.</span></span> <span data-ttu-id="7cb8d-179">Pokud tento isdone správně, vaší sítě nástroj spustí protokolování síťové požadavky provedené Storage Explorer na koncové body správy a služby.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-179">If this isdone correctly, your networking tool starts logging network requests made by Storage Explorer to management and service endpoints.</span></span> <span data-ttu-id="7cb8d-180">Zadejte například https://cawablobgrs.blob.core.windows.net/ pro koncový bod služby objektů blob v prohlížeči a zobrazí se odpověď se podobá následující text, který naznačuje prostředek existuje, i když nelze k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-180">For example, enter https://cawablobgrs.blob.core.windows.net/ for your blob endpoint in a browser, and you will receive a response resembles the following, which suggests the resource exists, although you cannot access it.</span></span>

![Ukázka kódu](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a><span data-ttu-id="7cb8d-182">Obraťte se na správce serveru proxy</span><span class="sxs-lookup"><span data-stu-id="7cb8d-182">Contact proxy server admin</span></span>

<span data-ttu-id="7cb8d-183">Pokud vaše nastavení proxy serveru jsou správné, možná budete muset obrátit na správce serveru proxy a</span><span class="sxs-lookup"><span data-stu-id="7cb8d-183">If your proxy settings are correct, you may have to contact your proxy server admin, and</span></span>

- <span data-ttu-id="7cb8d-184">Ujistěte se, že proxy neblokovala přenosy na koncové body Azure pro správu nebo prostředků.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-184">Make sure that your proxy does not block traffic to Azure management or resource endpoints.</span></span>

- <span data-ttu-id="7cb8d-185">Zkontrolujte protokol ověřování používá proxy server.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-185">Verify the authentication protocol used by your proxy server.</span></span> <span data-ttu-id="7cb8d-186">Storage Explorer v současné době nepodporuje proxy protokolu NTLM.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-186">Storage Explorer does not currently support NTLM proxies.</span></span>

## <a name="unable-to-retrieve-children-error-message"></a><span data-ttu-id="7cb8d-187">Chybová zpráva "Nelze načíst, děti"</span><span class="sxs-lookup"><span data-stu-id="7cb8d-187">"Unable to Retrieve Children" error message</span></span>

<span data-ttu-id="7cb8d-188">Pokud jste připojeni k Azure prostřednictvím proxy serveru, ověřte správnost nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-188">If you are connected to Azure through a proxy, verify that your proxy settings are correct.</span></span> <span data-ttu-id="7cb8d-189">Pokud byly udělen přístup k prostředku z vlastník předplatného nebo účet, ověřte, zda přečetli nebo seznamu oprávnění pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-189">If you were granted access to a resource from the owner of the subscription or account, verify that you have read or list permissions for that resource.</span></span>

### <a name="issues-with-sas-url"></a><span data-ttu-id="7cb8d-190">Problémy s adresou URL SAS</span><span class="sxs-lookup"><span data-stu-id="7cb8d-190">Issues with SAS URL</span></span>
<span data-ttu-id="7cb8d-191">Pokud se připojujete ke službě pomocí adresy URL SAS a hlásí tuto chybu:</span><span class="sxs-lookup"><span data-stu-id="7cb8d-191">If you are connecting to a service using a SAS URL and experiencing this error:</span></span>

- <span data-ttu-id="7cb8d-192">Ověřte, že adresa URL poskytuje potřebná oprávnění ke čtení nebo seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-192">Verify that the URL provides the necessary permissions to read or list resources.</span></span>

- <span data-ttu-id="7cb8d-193">Ověřte, zda nevypršela platnost adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-193">Verify that the URL has not expired.</span></span>

- <span data-ttu-id="7cb8d-194">Pokud SAS adresa URL je založená na zásadách přístupu, ověřte, že nebyl odvolaný zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-194">If the SAS URL is based on an access policy, verify that the access policy has not been revoked.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cb8d-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7cb8d-195">Next steps</span></span>

<span data-ttu-id="7cb8d-196">Pokud žádná z řešení fungovat pro vás, odešlete svůj problém prostřednictvím nástroje zpětnou vazbu s e-mailu a tolik podrobnosti o problému zahrnuty jako je možné, tak, aby budeme vás moc kontaktovat o nápravě problému.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-196">If none of the solutions work for you, submit your issue through the feedback tool with your email and as many details about the issue included as you can, so that we can contact you for fixing the issue.</span></span>

<span data-ttu-id="7cb8d-197">Chcete-li to provést, klikněte na tlačítko **pomoci** nabídce a pak klikněte na tlačítko **odeslat zpětnou vazbu**.</span><span class="sxs-lookup"><span data-stu-id="7cb8d-197">To do this, click **Help** menu, and then click **Send Feedback**.</span></span>

![Váš názor](./media/storage-explorer-troubleshooting/4022503_en_1.png)
