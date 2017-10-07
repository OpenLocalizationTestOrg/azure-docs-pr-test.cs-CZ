---
title: "Průvodci odstraňováním potíží Storage Explorer aaaAzure | Microsoft Docs"
description: "Přehled ladění funkce hello dvě sady Azure"
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
ms.openlocfilehash: 21705629500359222bc566c599f0864ad50036ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a><span data-ttu-id="a5dea-103">Azure Storage Explorer Průvodci odstraňováním potíží</span><span class="sxs-lookup"><span data-stu-id="a5dea-103">Azure Storage Explorer troubleshooting guide</span></span>

## <a name="introduction"></a><span data-ttu-id="a5dea-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="a5dea-104">Introduction</span></span>

<span data-ttu-id="a5dea-105">Microsoft Azure Storage Explorer (Preview) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="a5dea-105">Microsoft Azure Storage Explorer (Preview) is a stand-alone app that enables you tooeasily work with Azure Storage data on Windows, macOS and Linux.</span></span> <span data-ttu-id="a5dea-106">aplikace Hello se může připojit toStorage účty hostované na Azure, suverénní Cloudy a Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="a5dea-106">hello app can connect toStorage accounts hosted on Azure, Sovereign Clouds, and Azure Stack.</span></span>

<span data-ttu-id="a5dea-107">Tato příručka obsahuje souhrn řešení pro běžné problémy, které jsou vidět ve Storage Exploreru.</span><span class="sxs-lookup"><span data-stu-id="a5dea-107">This guide summarizes solutions for common issues seen in Storage Explorer.</span></span>

## <a name="sign-in-issues"></a><span data-ttu-id="a5dea-108">Přihlaste se problémy</span><span class="sxs-lookup"><span data-stu-id="a5dea-108">Sign in issues</span></span>

<span data-ttu-id="a5dea-109">Podporovány jsou pouze účty Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="a5dea-109">Only Azure Active Directory (AAD) accounts are supported.</span></span> <span data-ttu-id="a5dea-110">Pokud používáte účet služby AD FS, očekává se, že podepisování v tooStorage Explorer nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="a5dea-110">If you use an ADFS account, it’s expected that signing in tooStorage Explorer would not work.</span></span> <span data-ttu-id="a5dea-111">Než budete pokračovat, zkuste restartovat aplikaci a zjistit, zda lze dlouhodobý hello problémy.</span><span class="sxs-lookup"><span data-stu-id="a5dea-111">Before you continue, try restarting your application and see whether hello problems can be fixed.</span></span>

### <a name="error-self-signed-certificate-in-certificate-chain"></a><span data-ttu-id="a5dea-112">Chyba: Certifikát podepsaný svým držitelem v řetězu certifikátů</span><span class="sxs-lookup"><span data-stu-id="a5dea-112">Error: Self-Signed Certificate in Certificate Chain</span></span>

<span data-ttu-id="a5dea-113">Tady je několik důvodů, proč této chybě může dojít, a nejběžnější dva důvody hello jsou následující:</span><span class="sxs-lookup"><span data-stu-id="a5dea-113">There are several reasons why you may encounter this error, and hello most common two reasons are as follows:</span></span>

1. <span data-ttu-id="a5dea-114">aplikace Hello je připojená přes "transparentní proxy", což znamená, brání komunikaci přes protokol HTTPS, dešifrování ho a pak šifrování pomocí certifikát podepsaný svým držitelem serveru (například serveru vaší společnosti).</span><span class="sxs-lookup"><span data-stu-id="a5dea-114">hello app is connected through a “transparent proxy”, which means a server (such as your company server) is intercepting HTTPS traffic, decrypting it, and then encrypting it using a self-signed certificate.</span></span>

2. <span data-ttu-id="a5dea-115">Běží aplikace, jako je antivirový software, který je podepsaný certifikát SSL vložení do hello zpráv protokolu HTTPS, které jste dostali.</span><span class="sxs-lookup"><span data-stu-id="a5dea-115">You are running an application, such as antivirus software, which is injecting a self-signed SSL certificate into hello HTTPS messages that you receive.</span></span>

<span data-ttu-id="a5dea-116">Když Storage Explorer dojde jedním z problémů hello, můžete již nebude vědět, jestli je úmyslně hello přijata zpráva protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a5dea-116">When Storage Explorer encounters one of hello issues, it can no longer know whether hello received HTTPS message is tampered.</span></span> <span data-ttu-id="a5dea-117">Pokud máte kopii hello certifikát podepsaný svým držitelem, můžete je nechat Storage Explorer důvěřujete mu.</span><span class="sxs-lookup"><span data-stu-id="a5dea-117">If you have a copy of hello self-signed certificate, you can let Storage Explorer trust it.</span></span> <span data-ttu-id="a5dea-118">Pokud si nejste jisti kdo je vložení hello certifikát, postupujte podle těchto kroků toofind ho:</span><span class="sxs-lookup"><span data-stu-id="a5dea-118">If you are unsure of who is injecting hello certificate, follow these steps toofind it:</span></span>

1. <span data-ttu-id="a5dea-119">Instalace otevřete SSL</span><span class="sxs-lookup"><span data-stu-id="a5dea-119">Install Open SSL</span></span>

    - <span data-ttu-id="a5dea-120">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (jakékoli světla verze hello by mělo být dostatečné)</span><span class="sxs-lookup"><span data-stu-id="a5dea-120">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (any of hello light versions should be sufficient)</span></span>

    - <span data-ttu-id="a5dea-121">Mac a Linux: by měly být zahrnuty s operačním systémem</span><span class="sxs-lookup"><span data-stu-id="a5dea-121">Mac and Linux: should be included with your operating system</span></span>

2. <span data-ttu-id="a5dea-122">Spustit otevřete SSL</span><span class="sxs-lookup"><span data-stu-id="a5dea-122">Run Open SSL</span></span>

    - <span data-ttu-id="a5dea-123">Windows: Otevřete hello instalační adresář, klikněte na **/bin/**a potom dvakrát klikněte na **openssl.exe**.</span><span class="sxs-lookup"><span data-stu-id="a5dea-123">Windows: open hello installation directory, click **/bin/**, and then double-click **openssl.exe**.</span></span>
    - <span data-ttu-id="a5dea-124">Mac a Linux: Spusťte **openssl** z terminálu.</span><span class="sxs-lookup"><span data-stu-id="a5dea-124">Mac and Linux: run **openssl** from a terminal.</span></span>

3. <span data-ttu-id="a5dea-125">Spuštění s_client - showcerts-připojení microsoft.com:443</span><span class="sxs-lookup"><span data-stu-id="a5dea-125">Execute s_client -showcerts -connect microsoft.com:443</span></span>

4. <span data-ttu-id="a5dea-126">Hledejte certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="a5dea-126">Look for self-signed certificates.</span></span> <span data-ttu-id="a5dea-127">Pokud si nejste jistí, které jsou podepsané svým držitelem, vyhledejte kdekoli hello subjektu ("s:") a vystavitele ("i") jsou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="a5dea-127">If you are unsure which are self-signed, look for anywhere hello subject ("s:") and issuer ("i:") are hello same.</span></span>

5. <span data-ttu-id="a5dea-128">Po nalezení všechny certifikáty podepsané svým držitelem pro každé z nich, zkopírujte a vložte všechno z a to včetně **---BEGIN CERTIFICATE---** příliš**---END CERTIFICATE---** tooa nový soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="a5dea-128">When you have found any self-signed certificates, for each one, copy and paste everything from and including **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** tooa new .cer file.</span></span>

6. <span data-ttu-id="a5dea-129">Otevřete Storage Explorer, klikněte na **upravit** > **certifikáty SSL** > **importu certifikátů**a pak použijte hello souboru výběr toofind, vyberte možnost, a otevřete hello .cer soubory, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a5dea-129">Open Storage Explorer, click **Edit** > **SSL Certificates** > **Import Certificates**, and then use hello file picker toofind, select, and open hello .cer files that you created.</span></span>

<span data-ttu-id="a5dea-130">Pokud nenajdete žádné certifikáty podepsané svým držitelem pomocí hello výše uvedené kroky, kontaktujte nás pomocí nástroje hello zpětnou vazbu o další pomoc.</span><span class="sxs-lookup"><span data-stu-id="a5dea-130">If you cannot find any self-signed certificates using hello above steps, contact us through hello feedback tool for more help.</span></span>

### <a name="unable-tooretrieve-subscriptions"></a><span data-ttu-id="a5dea-131">Odběry nelze tooretrieve</span><span class="sxs-lookup"><span data-stu-id="a5dea-131">Unable tooretrieve subscriptions</span></span>

<span data-ttu-id="a5dea-132">Pokud jste nelze tooretrieve vašich předplatných po můžete úspěšně přihlásit, postupujte tento problém tootroubleshoot tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="a5dea-132">If you are unable tooretrieve your subscriptions after you successfully sign in, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="a5dea-133">Ověřte, zda má váš účet přístup toohello odběry po přihlášení k portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="a5dea-133">Verify that your account has access toohello subscriptions by signing into hello Azure portal.</span></span>

- <span data-ttu-id="a5dea-134">Ujistěte se, že jste se přihlásili pomocí správné prostředí hello (Azure, Azure China, Azure v Německu, Azure US Government nebo vlastní prostředí nebo Azure zásobníku).</span><span class="sxs-lookup"><span data-stu-id="a5dea-134">Make sure that you have signed in using hello correct environment (Azure, Azure China, Azure Germany, Azure US Government, or Custom Environment/Azure Stack).</span></span>

- <span data-ttu-id="a5dea-135">Pokud se nacházíte za proxy, ujistěte se, které jste nakonfigurovali proxy hello Storage Explorer správně.</span><span class="sxs-lookup"><span data-stu-id="a5dea-135">If you are behind a proxy, make sure that you have configured hello Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="a5dea-136">Zkuste odebrat a nové přidání účtu hello.</span><span class="sxs-lookup"><span data-stu-id="a5dea-136">Try removing and readding hello account.</span></span>

- <span data-ttu-id="a5dea-137">Zkuste odstranit hello následující soubory z kořenového adresáře (tedy C:\Users\ContosoUser) a potom znovu přidat účet hello:</span><span class="sxs-lookup"><span data-stu-id="a5dea-137">Try deleting hello following files from your root directory (that is, C:\Users\ContosoUser), and then re-adding hello account:</span></span>

    - <span data-ttu-id="a5dea-138">.adalcache</span><span class="sxs-lookup"><span data-stu-id="a5dea-138">.adalcache</span></span>

    - <span data-ttu-id="a5dea-139">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="a5dea-139">.devaccounts</span></span>

    - <span data-ttu-id="a5dea-140">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="a5dea-140">.extaccounts</span></span>

- <span data-ttu-id="a5dea-141">Sledování konzoly vývojářských nástrojů hello (stisknutím klávesy F12) Pokud se přihlašujete všechny chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="a5dea-141">Watch hello developer tools console (by pressing F12) when you are signing in for any error messages:</span></span>

![Nástroje pro vývojáře](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-toosee-hello-authentication-page"></a><span data-ttu-id="a5dea-143">Stránku nelze toosee hello ověřování</span><span class="sxs-lookup"><span data-stu-id="a5dea-143">Unable toosee hello authentication page</span></span>

<span data-ttu-id="a5dea-144">Pokud jste nelze toosee hello ověřovací stránku, postupujte tento problém tootroubleshoot tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="a5dea-144">If you are unable toosee hello authentication page, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="a5dea-145">V závislosti na rychlosti hello připojení může chvíli trvat, než tooload přihlašovací stránku hello, počkejte před jeho zavřením dialogové okno ověřování hello alespoň jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="a5dea-145">Depending on hello speed of your connection, it may take a while for hello sign-in page tooload, wait at least one minute before closing hello authentication dialog box.</span></span>

- <span data-ttu-id="a5dea-146">Pokud se nacházíte za proxy, ujistěte se, které jste nakonfigurovali proxy hello Storage Explorer správně.</span><span class="sxs-lookup"><span data-stu-id="a5dea-146">If you are behind a proxy, make sure that you have configured hello Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="a5dea-147">Zobrazení hello vývojářské konzole stisknutím klávesy F12 hello.</span><span class="sxs-lookup"><span data-stu-id="a5dea-147">View hello developer console by pressing hello F12 key.</span></span> <span data-ttu-id="a5dea-148">Sledovat hello odpovědí z vývojářské konzole hello a zjistit, jestli můžete najít všechny potvrzením pro důvod, proč ověřování nepracuje.</span><span class="sxs-lookup"><span data-stu-id="a5dea-148">Watch hello responses from hello developer console and see whether you can find any clue for why authentication not working.</span></span>

### <a name="cannot-remove-account"></a><span data-ttu-id="a5dea-149">Nelze odebrat účet</span><span class="sxs-lookup"><span data-stu-id="a5dea-149">Cannot remove account</span></span>

<span data-ttu-id="a5dea-150">Pokud jste nelze tooremove účet nebo pokud hello znovu ověřit propojení nemá žádný, postupujte podle těchto kroků tootroubleshoot tento problém:</span><span class="sxs-lookup"><span data-stu-id="a5dea-150">If you are unable tooremove an account, or if hello reauthenticate link does not do anything, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="a5dea-151">Zkuste odstranit hello následující soubory z kořenového adresáře a pak nové přidání účtu hello:</span><span class="sxs-lookup"><span data-stu-id="a5dea-151">Try deleting hello following files from your root directory, and then readding hello account:</span></span>

    - <span data-ttu-id="a5dea-152">.adalcache</span><span class="sxs-lookup"><span data-stu-id="a5dea-152">.adalcache</span></span>

    - <span data-ttu-id="a5dea-153">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="a5dea-153">.devaccounts</span></span>

    - <span data-ttu-id="a5dea-154">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="a5dea-154">.extaccounts</span></span>

- <span data-ttu-id="a5dea-155">Pokud chcete, aby byl tooremove SAS připojen prostředky úložiště, odstraňte hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="a5dea-155">If you want tooremove SAS attached Storage resources, delete hello following files:</span></span>

    - <span data-ttu-id="a5dea-156">%AppData%/StorageExplorer složky pro Windows</span><span class="sxs-lookup"><span data-stu-id="a5dea-156">%AppData%/StorageExplorer folder for Windows</span></span>

    - <span data-ttu-id="a5dea-157">/Users/ < vaše_jméno >/knihovny/Express podporu nebo StorageExplorer pro Mac</span><span class="sxs-lookup"><span data-stu-id="a5dea-157">/Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer for Mac</span></span>

    - <span data-ttu-id="a5dea-158">~/.config/StorageExplorer pro Linux</span><span class="sxs-lookup"><span data-stu-id="a5dea-158">~/.config/StorageExplorer for Linux</span></span>

> [!NOTE]
>  <span data-ttu-id="a5dea-159">Tooreenter bude mít všechny svoje přihlašovací údaje, pokud odstraníte tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="a5dea-159">You will have tooreenter all your credentials if you delete these files.</span></span>

## <a name="proxy-issues"></a><span data-ttu-id="a5dea-160">Problémy s proxy</span><span class="sxs-lookup"><span data-stu-id="a5dea-160">Proxy issues</span></span>

<span data-ttu-id="a5dea-161">Nejprve zkontrolujte, zda jsou všechny správné této hello následující informace, které jste zadali:</span><span class="sxs-lookup"><span data-stu-id="a5dea-161">First, make sure that hello following information you entered are all correct:</span></span>

- <span data-ttu-id="a5dea-162">Hello adresa URL proxy serveru a port číslo</span><span class="sxs-lookup"><span data-stu-id="a5dea-162">hello proxy URL and port number</span></span>

- <span data-ttu-id="a5dea-163">Uživatelské jméno a heslo, pokud to vyžaduje proxy server hello</span><span class="sxs-lookup"><span data-stu-id="a5dea-163">Username and password if required by hello proxy</span></span>

### <a name="common-solutions"></a><span data-ttu-id="a5dea-164">Běžná řešení</span><span class="sxs-lookup"><span data-stu-id="a5dea-164">Common solutions</span></span>

<span data-ttu-id="a5dea-165">Pokud pořád dochází k problémům, postupujte podle těchto kroků tootroubleshoot je:</span><span class="sxs-lookup"><span data-stu-id="a5dea-165">If you are still experiencing issues, follow these steps tootroubleshoot them:</span></span>

- <span data-ttu-id="a5dea-166">Pokud se můžete připojit toohello Internetu bez použití proxy, ověřte, že Storage Explorer funguje bez povolené nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="a5dea-166">If you can connect toohello Internet without using your proxy, verify that Storage Explorer works without proxy settings enabled.</span></span> <span data-ttu-id="a5dea-167">Pokud je to hello případ, může být problém se vaše nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="a5dea-167">If this is hello case, there may be an issue with your proxy settings.</span></span> <span data-ttu-id="a5dea-168">Spolupracovat s proxy správce tooidentify hello problémů.</span><span class="sxs-lookup"><span data-stu-id="a5dea-168">Work with your proxy administrator tooidentify hello problems.</span></span>

- <span data-ttu-id="a5dea-169">Ověřte, zda jiné aplikace pomocí serveru proxy hello fungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="a5dea-169">Verify that other applications using hello proxy server work as expected.</span></span>

- <span data-ttu-id="a5dea-170">Ověřte, zda se můžete připojit toohello portálu Microsoft Azure pomocí webového prohlížeče</span><span class="sxs-lookup"><span data-stu-id="a5dea-170">Verify that you can connect toohello Microsoft Azure portal using your web browser</span></span>

- <span data-ttu-id="a5dea-171">Ověřte, zda se zobrazila odpovědí z koncových bodů služby.</span><span class="sxs-lookup"><span data-stu-id="a5dea-171">Verify that you can receive responses from your service endpoints.</span></span> <span data-ttu-id="a5dea-172">Zadejte jednu z váš koncový bod adresy URL do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a5dea-172">Enter one of your endpoint URLs into your browser.</span></span> <span data-ttu-id="a5dea-173">Pokud se můžete připojit, měli byste obdržet InvalidQueryParameterValue nebo podobné odpovědi ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="a5dea-173">If you can connect, you should receive an InvalidQueryParameterValue or similar XML response.</span></span>

- <span data-ttu-id="a5dea-174">Pokud někdo jiný používá také Storage Explorer proxy serveru, ověřte, zda se můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="a5dea-174">If someone else is also using Storage Explorer with your proxy server, verify that they can connect.</span></span> <span data-ttu-id="a5dea-175">Pokud se mohou připojovat, bude pravděpodobně toocontact správce proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="a5dea-175">If they can connect, you may have toocontact your proxy server admin.</span></span>

### <a name="tools-for-diagnosing-issues"></a><span data-ttu-id="a5dea-176">Nástroje pro diagnostiku problémů</span><span class="sxs-lookup"><span data-stu-id="a5dea-176">Tools for diagnosing issues</span></span>

<span data-ttu-id="a5dea-177">Pokud máte síťové nástroje, například aplikaci Fiddler pro Windows, může být schopný toodiagnose hello problémy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a5dea-177">If you have networking tools, such as Fiddler for Windows, you may be able toodiagnose hello problems as follows:</span></span>

- <span data-ttu-id="a5dea-178">Pokud máte toowork prostřednictvím proxy, můžete mít tooconfigure vaší sítě tooconnect nástroj přes proxy server hello.</span><span class="sxs-lookup"><span data-stu-id="a5dea-178">If you have toowork through your proxy, you may have tooconfigure your networking tool tooconnect through hello proxy.</span></span>

- <span data-ttu-id="a5dea-179">Zkontrolujte číslo portu hello používané nástrojem pro vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="a5dea-179">Check hello port number used by your networking tool.</span></span>

- <span data-ttu-id="a5dea-180">Zadejte adresu URL místního hostitele hello a hello sítě číslo portu je nástroj jako nastavení proxy serveru v Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="a5dea-180">Enter hello local host URL and hello networking tool's port number as proxy settings in Storage Explorer.</span></span> <span data-ttu-id="a5dea-181">Když toto dokončíte správně, vaše síťové nástroje spustí se protokolování provedené koncové body toomanagement a služby Storage Explorer síťové požadavky.</span><span class="sxs-lookup"><span data-stu-id="a5dea-181">If this is done correctly, your networking tool starts logging network requests made by Storage Explorer toomanagement and service endpoints.</span></span> <span data-ttu-id="a5dea-182">Zadejte například https://cawablobgrs.blob.core.windows.net/ pro koncový bod služby objektů blob v prohlížeči a zobrazí se odpověď se podobá následující text hello, který naznačuje hello prostředek existuje, i když nelze k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="a5dea-182">For example, enter https://cawablobgrs.blob.core.windows.net/ for your blob endpoint in a browser, and you will receive a response resembles hello following, which suggests hello resource exists, although you cannot access it.</span></span>

![Ukázka kódu](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a><span data-ttu-id="a5dea-184">Obraťte se na správce serveru proxy</span><span class="sxs-lookup"><span data-stu-id="a5dea-184">Contact proxy server admin</span></span>

<span data-ttu-id="a5dea-185">Pokud vaše nastavení proxy serveru jsou správné, bude pravděpodobně toocontact správce serveru proxy a</span><span class="sxs-lookup"><span data-stu-id="a5dea-185">If your proxy settings are correct, you may have toocontact your proxy server admin, and</span></span>

- <span data-ttu-id="a5dea-186">Ujistěte se, že proxy neblokuje přenosy tooAzure správy nebo prostředků koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="a5dea-186">Make sure that your proxy does not block traffic tooAzure management or resource endpoints.</span></span>

- <span data-ttu-id="a5dea-187">Zkontrolujte protokol ověřování hello používá proxy server.</span><span class="sxs-lookup"><span data-stu-id="a5dea-187">Verify hello authentication protocol used by your proxy server.</span></span> <span data-ttu-id="a5dea-188">Storage Explorer v současné době nepodporuje proxy protokolu NTLM.</span><span class="sxs-lookup"><span data-stu-id="a5dea-188">Storage Explorer does not currently support NTLM proxies.</span></span>

## <a name="unable-tooretrieve-children-error-message"></a><span data-ttu-id="a5dea-189">Chybová zpráva "Nelze tooRetrieve podřízené objekty"</span><span class="sxs-lookup"><span data-stu-id="a5dea-189">"Unable tooRetrieve Children" error message</span></span>

<span data-ttu-id="a5dea-190">Pokud jste připojené tooAzure prostřednictvím proxy serveru, ověřte správnost nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="a5dea-190">If you are connected tooAzure through a proxy, verify that your proxy settings are correct.</span></span> <span data-ttu-id="a5dea-191">Pokud byla udělena prostředků tooa přístup od vlastníka hello hello předplatné nebo účet, ověřte, zda přečetli nebo seznamu oprávnění pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="a5dea-191">If you were granted access tooa resource from hello owner of hello subscription or account, verify that you have read or list permissions for that resource.</span></span>

### <a name="issues-with-sas-url"></a><span data-ttu-id="a5dea-192">Problémy s adresou URL SAS</span><span class="sxs-lookup"><span data-stu-id="a5dea-192">Issues with SAS URL</span></span>
<span data-ttu-id="a5dea-193">Pokud se připojujete tooa služby pomocí SAS adresa URL a hlásí tuto chybu:</span><span class="sxs-lookup"><span data-stu-id="a5dea-193">If you are connecting tooa service using a SAS URL and experiencing this error:</span></span>

- <span data-ttu-id="a5dea-194">Ověřte, že adresa URL hello poskytuje hello potřebná oprávnění tooread nebo seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a5dea-194">Verify that hello URL provides hello necessary permissions tooread or list resources.</span></span>

- <span data-ttu-id="a5dea-195">Ověřte, že hello nevypršela platnost adresy URL.</span><span class="sxs-lookup"><span data-stu-id="a5dea-195">Verify that hello URL has not expired.</span></span>

- <span data-ttu-id="a5dea-196">Pokud hello SAS adresa URL je založená na zásadách přístupu, ověřte nebyl odvolaný hello zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="a5dea-196">If hello SAS URL is based on an access policy, verify that hello access policy has not been revoked.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5dea-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5dea-197">Next steps</span></span>

<span data-ttu-id="a5dea-198">Pokud žádná z hello řešení fungovat pro vás, odešlete svůj problém pomocí nástroje hello zpětnou vazbu s e-mailu a tolik podrobnosti o problému hello zahrnuty jako je možné, tak, aby budeme vás moc kontaktovat o nápravě problému hello.</span><span class="sxs-lookup"><span data-stu-id="a5dea-198">If none of hello solutions work for you, submit your issue through hello feedback tool with your email and as many details about hello issue included as you can, so that we can contact you for fixing hello issue.</span></span>

<span data-ttu-id="a5dea-199">toodo tento, klikněte na tlačítko **pomoci** nabídce a pak klikněte na tlačítko **odeslat zpětnou vazbu**.</span><span class="sxs-lookup"><span data-stu-id="a5dea-199">toodo this, click **Help** menu, and then click **Send Feedback**.</span></span>

![Váš názor](./media/storage-explorer-troubleshooting/4022503_en_1.png)
