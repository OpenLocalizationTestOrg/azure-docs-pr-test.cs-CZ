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
ms.date: 08/09/2017
ms.author: delhan
ms.openlocfilehash: e9b833b07556378f17d9aaff0912c7d73dff44eb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a><span data-ttu-id="7692c-103">Azure Storage Explorer Průvodci odstraňováním potíží</span><span class="sxs-lookup"><span data-stu-id="7692c-103">Azure Storage Explorer troubleshooting guide</span></span>

## <a name="introduction"></a><span data-ttu-id="7692c-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="7692c-104">Introduction</span></span>

<span data-ttu-id="7692c-105">Microsoft Azure Storage Explorer (Preview) je samostatná aplikace, která umožňuje snadno pracovat s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="7692c-105">Microsoft Azure Storage Explorer (Preview) is a stand-alone app that enables you to easily work with Azure Storage data on Windows, macOS and Linux.</span></span> <span data-ttu-id="7692c-106">Aplikace se může připojit toStorage účty hostované na Azure, suverénní Cloudy a Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="7692c-106">The app can connect toStorage accounts hosted on Azure, Sovereign Clouds, and Azure Stack.</span></span>

<span data-ttu-id="7692c-107">Tato příručka obsahuje souhrn řešení pro běžné problémy, které jsou vidět ve Storage Exploreru.</span><span class="sxs-lookup"><span data-stu-id="7692c-107">This guide summarizes solutions for common issues seen in Storage Explorer.</span></span>

## <a name="sign-in-issues"></a><span data-ttu-id="7692c-108">Přihlaste se problémy</span><span class="sxs-lookup"><span data-stu-id="7692c-108">Sign in issues</span></span>

<span data-ttu-id="7692c-109">Podporovány jsou pouze účty Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="7692c-109">Only Azure Active Directory (AAD) accounts are supported.</span></span> <span data-ttu-id="7692c-110">Pokud používáte účet služby AD FS, očekává se, že přihlášení do služby Storage Explorer nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="7692c-110">If you use an ADFS account, it’s expected that signing in to Storage Explorer would not work.</span></span> <span data-ttu-id="7692c-111">Než budete pokračovat, zkuste restartovat aplikaci a zjistit, zda lze dlouhodobý problémy.</span><span class="sxs-lookup"><span data-stu-id="7692c-111">Before you continue, try restarting your application and see whether the problems can be fixed.</span></span>

### <a name="error-self-signed-certificate-in-certificate-chain"></a><span data-ttu-id="7692c-112">Chyba: Certifikát podepsaný svým držitelem v řetězu certifikátů</span><span class="sxs-lookup"><span data-stu-id="7692c-112">Error: Self-Signed Certificate in Certificate Chain</span></span>

<span data-ttu-id="7692c-113">Tady je několik důvodů, proč této chybě může dojít, a nejběžnější dva důvody jsou následující:</span><span class="sxs-lookup"><span data-stu-id="7692c-113">There are several reasons why you may encounter this error, and the most common two reasons are as follows:</span></span>

1. <span data-ttu-id="7692c-114">Aplikace je připojený prostřednictvím "transparentní proxy", což znamená, brání komunikaci přes protokol HTTPS, dešifrování ho a pak šifrování pomocí certifikát podepsaný svým držitelem serveru (například serveru vaší společnosti).</span><span class="sxs-lookup"><span data-stu-id="7692c-114">The app is connected through a “transparent proxy”, which means a server (such as your company server) is intercepting HTTPS traffic, decrypting it, and then encrypting it using a self-signed certificate.</span></span>

2. <span data-ttu-id="7692c-115">Běží aplikace, jako je antivirový software, který je podepsaný certifikát SSL vložení do zpráv protokolu HTTPS, které jste dostali.</span><span class="sxs-lookup"><span data-stu-id="7692c-115">You are running an application, such as antivirus software, which is injecting a self-signed SSL certificate into the HTTPS messages that you receive.</span></span>

<span data-ttu-id="7692c-116">Když Storage Explorer dojde jeden z problémů, můžete již nebude vědět, jestli je úmyslně přijatou zprávu protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7692c-116">When Storage Explorer encounters one of the issues, it can no longer know whether the received HTTPS message is tampered.</span></span> <span data-ttu-id="7692c-117">Pokud máte kopie certifikátu podepsaného svým držitelem, můžete je nechat Storage Explorer důvěřujete mu.</span><span class="sxs-lookup"><span data-stu-id="7692c-117">If you have a copy of the self-signed certificate, you can let Storage Explorer trust it.</span></span> <span data-ttu-id="7692c-118">Pokud si nejste jisti kdo je vložení certifikát, použijte následující postup ji najít:</span><span class="sxs-lookup"><span data-stu-id="7692c-118">If you are unsure of who is injecting the certificate, follow these steps to find it:</span></span>

1. <span data-ttu-id="7692c-119">Instalace otevřete SSL</span><span class="sxs-lookup"><span data-stu-id="7692c-119">Install Open SSL</span></span>

    - <span data-ttu-id="7692c-120">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (jakékoli světla verze by mělo být dostatečné)</span><span class="sxs-lookup"><span data-stu-id="7692c-120">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (any of the light versions should be sufficient)</span></span>

    - <span data-ttu-id="7692c-121">Mac a Linux: by měly být zahrnuty s operačním systémem</span><span class="sxs-lookup"><span data-stu-id="7692c-121">Mac and Linux: should be included with your operating system</span></span>

2. <span data-ttu-id="7692c-122">Spustit otevřete SSL</span><span class="sxs-lookup"><span data-stu-id="7692c-122">Run Open SSL</span></span>

    - <span data-ttu-id="7692c-123">Windows: otevřete instalační adresář, klikněte na **/bin/**a potom dvakrát klikněte na **openssl.exe**.</span><span class="sxs-lookup"><span data-stu-id="7692c-123">Windows: open the installation directory, click **/bin/**, and then double-click **openssl.exe**.</span></span>
    - <span data-ttu-id="7692c-124">Mac a Linux: Spusťte **openssl** z terminálu.</span><span class="sxs-lookup"><span data-stu-id="7692c-124">Mac and Linux: run **openssl** from a terminal.</span></span>

3. <span data-ttu-id="7692c-125">Spuštění s_client - showcerts-připojení microsoft.com:443</span><span class="sxs-lookup"><span data-stu-id="7692c-125">Execute s_client -showcerts -connect microsoft.com:443</span></span>

4. <span data-ttu-id="7692c-126">Hledejte certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="7692c-126">Look for self-signed certificates.</span></span> <span data-ttu-id="7692c-127">Pokud si nejste jistí, které jsou podepsané svým držitelem, vyhledejte kdekoli subjektu ("s:") a vystavitele ("i") jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="7692c-127">If you are unsure which are self-signed, look for anywhere the subject ("s:") and issuer ("i:") are the same.</span></span>

5. <span data-ttu-id="7692c-128">Po nalezení všechny certifikáty podepsané svým držitelem pro každé z nich, zkopírujte a vložte všechno z a to včetně **---BEGIN CERTIFICATE---** k **---END CERTIFICATE---** do nového souboru .cer.</span><span class="sxs-lookup"><span data-stu-id="7692c-128">When you have found any self-signed certificates, for each one, copy and paste everything from and including **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** to a new .cer file.</span></span>

6. <span data-ttu-id="7692c-129">Otevřete Storage Explorer, klikněte na **upravit** > **certifikáty SSL** > **importu certifikátů**a potom pomocí nástroje pro výběr souborů najít, vyberte a otevřete .cer soubory, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7692c-129">Open Storage Explorer, click **Edit** > **SSL Certificates** > **Import Certificates**, and then use the file picker to find, select, and open the .cer files that you created.</span></span>

<span data-ttu-id="7692c-130">Pokud nenajdete žádné certifikáty podepsané svým držitelem pomocí výše uvedené kroky, kontaktujte nás pomocí nástroje zpětnou vazbu o další pomoc.</span><span class="sxs-lookup"><span data-stu-id="7692c-130">If you cannot find any self-signed certificates using the above steps, contact us through the feedback tool for more help.</span></span>

### <a name="unable-to-retrieve-subscriptions"></a><span data-ttu-id="7692c-131">Nelze načíst předplatná</span><span class="sxs-lookup"><span data-stu-id="7692c-131">Unable to retrieve subscriptions</span></span>

<span data-ttu-id="7692c-132">Pokud jste se nepodařilo načíst vašich předplatných, po úspěšném přihlášení, použijte následující postup řešení tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="7692c-132">If you are unable to retrieve your subscriptions after you successfully sign in, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="7692c-133">Ověřte, že má váš účet přístup k předplatným po přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7692c-133">Verify that your account has access to the subscriptions by signing into the Azure portal.</span></span>

- <span data-ttu-id="7692c-134">Ujistěte se, že jste se zaregistrovali pomocí správné prostředí (Azure, Azure China, Azure v Německu, Azure US Government nebo vlastní prostředí nebo Azure zásobníku).</span><span class="sxs-lookup"><span data-stu-id="7692c-134">Make sure that you have signed in using the correct environment (Azure, Azure China, Azure Germany, Azure US Government, or Custom Environment/Azure Stack).</span></span>

- <span data-ttu-id="7692c-135">Pokud se nacházíte za proxy, ujistěte se, že jste nakonfigurovali proxy Storage Explorer správně.</span><span class="sxs-lookup"><span data-stu-id="7692c-135">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="7692c-136">Zkuste odebrat a nové přidání účtu.</span><span class="sxs-lookup"><span data-stu-id="7692c-136">Try removing and readding the account.</span></span>

- <span data-ttu-id="7692c-137">Pokuste se odstranit následující soubory z kořenového adresáře (tedy C:\Users\ContosoUser) a potom znovu přidat účet:</span><span class="sxs-lookup"><span data-stu-id="7692c-137">Try deleting the following files from your root directory (that is, C:\Users\ContosoUser), and then re-adding the account:</span></span>

    - <span data-ttu-id="7692c-138">.adalcache</span><span class="sxs-lookup"><span data-stu-id="7692c-138">.adalcache</span></span>

    - <span data-ttu-id="7692c-139">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="7692c-139">.devaccounts</span></span>

    - <span data-ttu-id="7692c-140">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="7692c-140">.extaccounts</span></span>

- <span data-ttu-id="7692c-141">Kukátko nástrojů pro vývojáře, (podle stisknutím klávesy F12), pokud se přihlašujete všechny chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="7692c-141">Watch the developer tools console (by pressing F12) when you are signing in for any error messages:</span></span>

![Nástroje pro vývojáře](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a><span data-ttu-id="7692c-143">Nelze zobrazit stránku ověřování</span><span class="sxs-lookup"><span data-stu-id="7692c-143">Unable to see the authentication page</span></span>

<span data-ttu-id="7692c-144">Pokud nelze zobrazit stránku ověřování, použijte následující postup řešení tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="7692c-144">If you are unable to see the authentication page, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="7692c-145">V závislosti na rychlosti připojení může trvat nějakou dobu stránku přihlášení a načíst, počkejte před jeho zavřením dialogové okno ověřování alespoň jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="7692c-145">Depending on the speed of your connection, it may take a while for the sign-in page to load, wait at least one minute before closing the authentication dialog box.</span></span>

- <span data-ttu-id="7692c-146">Pokud se nacházíte za proxy, ujistěte se, že jste nakonfigurovali proxy Storage Explorer správně.</span><span class="sxs-lookup"><span data-stu-id="7692c-146">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="7692c-147">Zobrazení konzole pro vývojáře stisknutím klávesy F12.</span><span class="sxs-lookup"><span data-stu-id="7692c-147">View the developer console by pressing the F12 key.</span></span> <span data-ttu-id="7692c-148">Podívejte se na odpovědi z konzole pro vývojáře a zobrazit, zda můžete najít všechny potvrzením pro důvod, proč ověřování nepracuje.</span><span class="sxs-lookup"><span data-stu-id="7692c-148">Watch the responses from the developer console and see whether you can find any clue for why authentication not working.</span></span>

### <a name="cannot-remove-account"></a><span data-ttu-id="7692c-149">Nelze odebrat účet</span><span class="sxs-lookup"><span data-stu-id="7692c-149">Cannot remove account</span></span>

<span data-ttu-id="7692c-150">Pokud se nepodařilo odebrat účet, nebo pokud znovu ověřit propojení nemá žádný, použijte následující postup řešení tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="7692c-150">If you are unable to remove an account, or if the reauthenticate link does not do anything, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="7692c-151">Pokuste se odstranit následující soubory z kořenového adresáře a pak nové přidání účtu:</span><span class="sxs-lookup"><span data-stu-id="7692c-151">Try deleting the following files from your root directory, and then readding the account:</span></span>

    - <span data-ttu-id="7692c-152">.adalcache</span><span class="sxs-lookup"><span data-stu-id="7692c-152">.adalcache</span></span>

    - <span data-ttu-id="7692c-153">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="7692c-153">.devaccounts</span></span>

    - <span data-ttu-id="7692c-154">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="7692c-154">.extaccounts</span></span>

- <span data-ttu-id="7692c-155">Pokud chcete odebrat SAS připojená prostředky úložiště, odstraňte následující soubory:</span><span class="sxs-lookup"><span data-stu-id="7692c-155">If you want to remove SAS attached Storage resources, delete the following files:</span></span>

    - <span data-ttu-id="7692c-156">%AppData%/StorageExplorer složky pro Windows</span><span class="sxs-lookup"><span data-stu-id="7692c-156">%AppData%/StorageExplorer folder for Windows</span></span>

    - <span data-ttu-id="7692c-157">/Users/ < vaše_jméno >/knihovny/Express podporu nebo StorageExplorer pro Mac</span><span class="sxs-lookup"><span data-stu-id="7692c-157">/Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer for Mac</span></span>

    - <span data-ttu-id="7692c-158">~/.config/StorageExplorer pro Linux</span><span class="sxs-lookup"><span data-stu-id="7692c-158">~/.config/StorageExplorer for Linux</span></span>

> [!NOTE]
>  <span data-ttu-id="7692c-159">Budete muset znovu zadat všechny přihlašovací údaje, pokud odstraníte tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="7692c-159">You will have to reenter all your credentials if you delete these files.</span></span>

## <a name="proxy-issues"></a><span data-ttu-id="7692c-160">Problémy s proxy</span><span class="sxs-lookup"><span data-stu-id="7692c-160">Proxy issues</span></span>

<span data-ttu-id="7692c-161">První zajistěte, aby byla následující informace, které jste zadali správně:</span><span class="sxs-lookup"><span data-stu-id="7692c-161">First, make sure that the following information you entered are all correct:</span></span>

- <span data-ttu-id="7692c-162">Adresa URL proxy serveru a číslo portu</span><span class="sxs-lookup"><span data-stu-id="7692c-162">The proxy URL and port number</span></span>

- <span data-ttu-id="7692c-163">Uživatelské jméno a heslo, pokud to vyžaduje proxy server</span><span class="sxs-lookup"><span data-stu-id="7692c-163">Username and password if required by the proxy</span></span>

### <a name="common-solutions"></a><span data-ttu-id="7692c-164">Běžná řešení</span><span class="sxs-lookup"><span data-stu-id="7692c-164">Common solutions</span></span>

<span data-ttu-id="7692c-165">Pokud stále dochází k problémům, použijte následující postup řešení potíží s je:</span><span class="sxs-lookup"><span data-stu-id="7692c-165">If you are still experiencing issues, follow these steps to troubleshoot them:</span></span>

- <span data-ttu-id="7692c-166">Pokud se můžete připojit k Internetu bez použití proxy, ověřte, že Storage Explorer funguje bez povolené nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7692c-166">If you can connect to the Internet without using your proxy, verify that Storage Explorer works without proxy settings enabled.</span></span> <span data-ttu-id="7692c-167">Pokud je to tento případ, může být problém se vaše nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7692c-167">If this is the case, there may be an issue with your proxy settings.</span></span> <span data-ttu-id="7692c-168">Práce se na správce serveru proxy a identifikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="7692c-168">Work with your proxy administrator to identify the problems.</span></span>

- <span data-ttu-id="7692c-169">Ověřte, že jiných aplikací pomocí proxy serveru fungovat podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="7692c-169">Verify that other applications using the proxy server work as expected.</span></span>

- <span data-ttu-id="7692c-170">Ověřte, zda se můžete připojit k portálu Microsoft Azure pomocí webového prohlížeče</span><span class="sxs-lookup"><span data-stu-id="7692c-170">Verify that you can connect to the Microsoft Azure portal using your web browser</span></span>

- <span data-ttu-id="7692c-171">Ověřte, zda se zobrazila odpovědí z koncových bodů služby.</span><span class="sxs-lookup"><span data-stu-id="7692c-171">Verify that you can receive responses from your service endpoints.</span></span> <span data-ttu-id="7692c-172">Zadejte jednu z váš koncový bod adresy URL do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7692c-172">Enter one of your endpoint URLs into your browser.</span></span> <span data-ttu-id="7692c-173">Pokud se můžete připojit, měli byste obdržet InvalidQueryParameterValue nebo podobné odpovědi ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="7692c-173">If you can connect, you should receive an InvalidQueryParameterValue or similar XML response.</span></span>

- <span data-ttu-id="7692c-174">Pokud někdo jiný používá také Storage Explorer proxy serveru, ověřte, zda se můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="7692c-174">If someone else is also using Storage Explorer with your proxy server, verify that they can connect.</span></span> <span data-ttu-id="7692c-175">Pokud se můžete připojit, možná budete muset obrátit na správce serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="7692c-175">If they can connect, you may have to contact your proxy server admin.</span></span>

### <a name="tools-for-diagnosing-issues"></a><span data-ttu-id="7692c-176">Nástroje pro diagnostiku problémů</span><span class="sxs-lookup"><span data-stu-id="7692c-176">Tools for diagnosing issues</span></span>

<span data-ttu-id="7692c-177">Pokud máte síťové nástroje, například aplikaci Fiddler pro Windows, bude pravděpodobně možné diagnostikovat problémy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7692c-177">If you have networking tools, such as Fiddler for Windows, you may be able to diagnose the problems as follows:</span></span>

- <span data-ttu-id="7692c-178">Pokud máte fungovat prostřednictvím proxy, možná budete muset nakonfigurovat vaše síťové nástroje pro připojení prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7692c-178">If you have to work through your proxy, you may have to configure your networking tool to connect through the proxy.</span></span>

- <span data-ttu-id="7692c-179">Zkontrolujte číslo portu používané nástrojem pro vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="7692c-179">Check the port number used by your networking tool.</span></span>

- <span data-ttu-id="7692c-180">Zadejte adresu URL místního hostitele a číslo portu sítě nástroj jako nastavení proxy serveru v Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="7692c-180">Enter the local host URL and the networking tool's port number as proxy settings in Storage Explorer.</span></span> <span data-ttu-id="7692c-181">Když toto dokončíte správně, vaše síťové nástroje spustí se protokolování síťové požadavky provedené Průzkumník úložišť pro koncové body služby a správu.</span><span class="sxs-lookup"><span data-stu-id="7692c-181">If this is done correctly, your networking tool starts logging network requests made by Storage Explorer to management and service endpoints.</span></span> <span data-ttu-id="7692c-182">Zadejte například https://cawablobgrs.blob.core.windows.net/ pro koncový bod služby objektů blob v prohlížeči a zobrazí se odpověď se podobá následující text, který naznačuje prostředek existuje, i když nelze k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="7692c-182">For example, enter https://cawablobgrs.blob.core.windows.net/ for your blob endpoint in a browser, and you will receive a response resembles the following, which suggests the resource exists, although you cannot access it.</span></span>

![Ukázka kódu](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a><span data-ttu-id="7692c-184">Obraťte se na správce serveru proxy</span><span class="sxs-lookup"><span data-stu-id="7692c-184">Contact proxy server admin</span></span>

<span data-ttu-id="7692c-185">Pokud vaše nastavení proxy serveru jsou správné, možná budete muset obrátit na správce serveru proxy a</span><span class="sxs-lookup"><span data-stu-id="7692c-185">If your proxy settings are correct, you may have to contact your proxy server admin, and</span></span>

- <span data-ttu-id="7692c-186">Ujistěte se, že proxy neblokovala přenosy na koncové body Azure pro správu nebo prostředků.</span><span class="sxs-lookup"><span data-stu-id="7692c-186">Make sure that your proxy does not block traffic to Azure management or resource endpoints.</span></span>

- <span data-ttu-id="7692c-187">Zkontrolujte protokol ověřování používá proxy server.</span><span class="sxs-lookup"><span data-stu-id="7692c-187">Verify the authentication protocol used by your proxy server.</span></span> <span data-ttu-id="7692c-188">Storage Explorer v současné době nepodporuje proxy protokolu NTLM.</span><span class="sxs-lookup"><span data-stu-id="7692c-188">Storage Explorer does not currently support NTLM proxies.</span></span>

## <a name="unable-to-retrieve-children-error-message"></a><span data-ttu-id="7692c-189">Chybová zpráva "Nelze načíst, děti"</span><span class="sxs-lookup"><span data-stu-id="7692c-189">"Unable to Retrieve Children" error message</span></span>

<span data-ttu-id="7692c-190">Pokud jste připojeni k Azure prostřednictvím proxy serveru, ověřte správnost nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="7692c-190">If you are connected to Azure through a proxy, verify that your proxy settings are correct.</span></span> <span data-ttu-id="7692c-191">Pokud byly udělen přístup k prostředku z vlastník předplatného nebo účet, ověřte, zda přečetli nebo seznamu oprávnění pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="7692c-191">If you were granted access to a resource from the owner of the subscription or account, verify that you have read or list permissions for that resource.</span></span>

### <a name="issues-with-sas-url"></a><span data-ttu-id="7692c-192">Problémy s adresou URL SAS</span><span class="sxs-lookup"><span data-stu-id="7692c-192">Issues with SAS URL</span></span>
<span data-ttu-id="7692c-193">Pokud se připojujete ke službě pomocí adresy URL SAS a hlásí tuto chybu:</span><span class="sxs-lookup"><span data-stu-id="7692c-193">If you are connecting to a service using a SAS URL and experiencing this error:</span></span>

- <span data-ttu-id="7692c-194">Ověřte, že adresa URL poskytuje potřebná oprávnění ke čtení nebo seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7692c-194">Verify that the URL provides the necessary permissions to read or list resources.</span></span>

- <span data-ttu-id="7692c-195">Ověřte, zda nevypršela platnost adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7692c-195">Verify that the URL has not expired.</span></span>

- <span data-ttu-id="7692c-196">Pokud SAS adresa URL je založená na zásadách přístupu, ověřte, že nebyl odvolaný zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="7692c-196">If the SAS URL is based on an access policy, verify that the access policy has not been revoked.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7692c-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7692c-197">Next steps</span></span>

<span data-ttu-id="7692c-198">Pokud žádná z řešení fungovat pro vás, odešlete svůj problém prostřednictvím nástroje zpětnou vazbu s e-mailu a tolik podrobnosti o problému zahrnuty jako je možné, tak, aby budeme vás moc kontaktovat o nápravě problému.</span><span class="sxs-lookup"><span data-stu-id="7692c-198">If none of the solutions work for you, submit your issue through the feedback tool with your email and as many details about the issue included as you can, so that we can contact you for fixing the issue.</span></span>

<span data-ttu-id="7692c-199">Chcete-li to provést, klikněte na tlačítko **pomoci** nabídce a pak klikněte na tlačítko **odeslat zpětnou vazbu**.</span><span class="sxs-lookup"><span data-stu-id="7692c-199">To do this, click **Help** menu, and then click **Send Feedback**.</span></span>

![Váš názor](./media/storage-explorer-troubleshooting/4022503_en_1.png)
