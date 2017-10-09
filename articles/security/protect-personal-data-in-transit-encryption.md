---
title: "aaaProtect osobních údajů v přenosu pomocí šifrování v Azure | Microsoft Docs"
description: "Použití šifrování v Azure tooprotect osobní data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 218ad3f49326e8dec299a6d92b18116da65eae71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a><span data-ttu-id="6eb53-103">Azure šifrovací technologie: ochrana osobních údajů v přenosu s šifrováním</span><span class="sxs-lookup"><span data-stu-id="6eb53-103">Azure encryption technologies: Protect personal data in transit with encryption</span></span>

<span data-ttu-id="6eb53-104">Tento článek vám pomůže pochopit a používat Azure šifrovací technologie toosecure data při přenosu.</span><span class="sxs-lookup"><span data-stu-id="6eb53-104">This article will help you understand and use Azure encryption technologies toosecure data in transit.</span></span> 

<span data-ttu-id="6eb53-105">Ochranu osobních údajů hello osobních údajů při přenosu v síti hello je důležitou součástí strategie zabezpečení víceúrovňová obrany zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6eb53-105">Protecting hello privacy of personal data as it travels across hello network is an essential part of a multi-layered defense-in-depth security strategy.</span></span> <span data-ttu-id="6eb53-106">Šifrování během přenosu je navrženou tooprevent útočník, který zabrání přenosů nebudou moct tooview nebo použijte hello data.</span><span class="sxs-lookup"><span data-stu-id="6eb53-106">Encryption in transit is designed tooprevent an attacker who intercepts transmissions from being able tooview or use hello data.</span></span>

## <a name="scenario"></a><span data-ttu-id="6eb53-107">Scénář</span><span class="sxs-lookup"><span data-stu-id="6eb53-107">Scenario</span></span>

<span data-ttu-id="6eb53-108">Velké výletních společnosti, centrálou ve Spojených státech amerických hello je rozšířit jeho itineráře toooffer operace v hello Středozemního, Jaderského a baltský moři, jakož i hello Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="6eb53-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="6eb53-109">toosupport těchto úsilí získala menší výletních Víceřádkový na základě v Itálii Německo, Dánsko a hello Spojené království</span><span class="sxs-lookup"><span data-stu-id="6eb53-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span> 

<span data-ttu-id="6eb53-110">Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="6eb53-111">To zahrnuje osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka.</span><span class="sxs-lookup"><span data-stu-id="6eb53-111">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="6eb53-112">Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako je adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti.</span><span class="sxs-lookup"><span data-stu-id="6eb53-112">It also includes traditional Human Resource information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="6eb53-113">Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje tootrack vztahů se zákazníky aktuální a starší.</span><span class="sxs-lookup"><span data-stu-id="6eb53-113">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

<span data-ttu-id="6eb53-114">Osobní údaje zákazníků je zadána v hello databáze ze vzdálených pobočkách hello společnosti a z agentů cesta nachází kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="6eb53-114">Personal data of customers is entered in hello database from hello company’s remote offices and from travel agents located around hello world.</span></span> <span data-ttu-id="6eb53-115">Dokumenty, které obsahují informace o zákazníkovi se přenesou pomocí hello síťového tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="6eb53-115">Documents containing customer information are transferred across hello network tooAzure storage.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="6eb53-116">Popis problému</span><span class="sxs-lookup"><span data-stu-id="6eb53-116">Problem statement</span></span>

<span data-ttu-id="6eb53-117">Hello společnosti musí chránit hello ochranu osobních údajů zákazníků a osobní data zaměstnanců při jeho je v tooand přenosu ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6eb53-117">hello company must protect hello privacy of customers’ and employees’ personal data while it is in transit tooand from Azure services.</span></span>

## <a name="company-goal"></a><span data-ttu-id="6eb53-118">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="6eb53-118">Company goal</span></span>

<span data-ttu-id="6eb53-119">Hello tooensure cílem společnosti, když vypnout disku musí být zašifrovaná osobní data.</span><span class="sxs-lookup"><span data-stu-id="6eb53-119">hello company goal tooensure that personal data is encrypted when off disk.</span></span> <span data-ttu-id="6eb53-120">Pokud neoprávněné osoby intercept hello vypnout disku osobní data, musí být ve formuláři, který bude zobrazovat nečitelná.</span><span class="sxs-lookup"><span data-stu-id="6eb53-120">If unauthorized persons intercept hello off-disk personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="6eb53-121">Použití šifrování by měla být snadná nebo zcela transparentní, uživatelům a správcům.</span><span class="sxs-lookup"><span data-stu-id="6eb53-121">Applying encryption should be easy, or completely transparent, for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="6eb53-122">Řešení</span><span class="sxs-lookup"><span data-stu-id="6eb53-122">Solutions</span></span>

<span data-ttu-id="6eb53-123">Azure services poskytují několik nástrojů a technologií toohelp chránit osobní údaje při přenosu.</span><span class="sxs-lookup"><span data-stu-id="6eb53-123">Azure services provide multiple tools and technologies toohelp you protect personal data in transit.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="6eb53-124">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6eb53-124">Azure Storage</span></span>

<span data-ttu-id="6eb53-125">Data, která je uložená v cloudu hello musí cestují z hello klienta, který může být fyzicky umístěné kdekoli v hello, world toohello datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="6eb53-125">Data that is stored in hello cloud must travel from hello client, which can be physically located anywhere in hello world, toohello Azure data center.</span></span> <span data-ttu-id="6eb53-126">Pokud tato data jsou načítána pro uživatele, se přenáší znovu v hello opačným směrem.</span><span class="sxs-lookup"><span data-stu-id="6eb53-126">When that data is retrieved by users, it travels again, in hello opposite direction.</span></span> <span data-ttu-id="6eb53-127">Data, která je při přenosu přes hello veřejného Internetu je vždy hrozí zachycení útočníků.</span><span class="sxs-lookup"><span data-stu-id="6eb53-127">Data that is in transit over hello public Internet is always at risk of interception by attackers.</span></span> <span data-ttu-id="6eb53-128">Je důležité tooprotect hello soukromí osobní data pomocí šifrování transportní vrstvy toosecure jako ji přesune mezi umístěními.</span><span class="sxs-lookup"><span data-stu-id="6eb53-128">It is important tooprotect hello privacy of personal data by using transport-level encryption toosecure it as it moves between locations.</span></span>

<span data-ttu-id="6eb53-129">Hello protokol HTTPS nabízí v porovnání s hello Internet zabezpečené, šifrované komunikační kanál.</span><span class="sxs-lookup"><span data-stu-id="6eb53-129">hello HTTPS protocol provides a secure, encrypted communications channel over hello Internet.</span></span> <span data-ttu-id="6eb53-130">HTTPS musí být použité tooaccess objekty ve službě Azure Storage a při volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="6eb53-130">HTTPS should be used tooaccess objects in Azure Storage and when calling REST APIs.</span></span> <span data-ttu-id="6eb53-131">Vynutit používání protokolu HTTPS hello při použití [sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate přístup tooAzure úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="6eb53-131">You enforce use of hello HTTPS protocol when using [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate access tooAzure Storage objects.</span></span> <span data-ttu-id="6eb53-132">Existují dva typy SAS: SAS služby a SAS účtu.</span><span class="sxs-lookup"><span data-stu-id="6eb53-132">There are two types of SAS: Service SAS and Account SAS.</span></span>

#### <a name="how-do-i-construct-a-service-sas"></a><span data-ttu-id="6eb53-133">Jak vytvořit SAS služby?</span><span class="sxs-lookup"><span data-stu-id="6eb53-133">How do I construct a Service SAS?</span></span>

<span data-ttu-id="6eb53-134">Prostředek služby SAS delegáti přístup tooa jen v jedné služby úložiště hello (služba objektů blob, fronty, tabulka nebo souboru).</span><span class="sxs-lookup"><span data-stu-id="6eb53-134">A Service SAS delegates access tooa resource in just one of hello storage services (blob, queue, table or file service).</span></span> <span data-ttu-id="6eb53-135">tooconstruct Service SAS, hello následující:</span><span class="sxs-lookup"><span data-stu-id="6eb53-135">tooconstruct a Service SAS, do hello following:</span></span>

1. <span data-ttu-id="6eb53-136">Zadejte hello pole verze Signed</span><span class="sxs-lookup"><span data-stu-id="6eb53-136">Specify hello Signed Version Field</span></span>

2. <span data-ttu-id="6eb53-137">Zadejte hello podepsaná prostředků (objektů Blob a služby pouze souboru)</span><span class="sxs-lookup"><span data-stu-id="6eb53-137">Specify hello Signed Resource (Blob and File Service Only)</span></span>

3. <span data-ttu-id="6eb53-138">Zadejte parametry dotazu tooOverride hlavičky odpovědi (služby objektů Blob a služby pouze souboru)</span><span class="sxs-lookup"><span data-stu-id="6eb53-138">Specify Query Parameters tooOverride Response Headers (Blob Service and File Service Only)</span></span>

4. <span data-ttu-id="6eb53-139">Zadejte název tabulky (pouze služby Table) hello</span><span class="sxs-lookup"><span data-stu-id="6eb53-139">Specify hello Table Name (Table Service Only)</span></span>

5. <span data-ttu-id="6eb53-140">Zadejte hello zásad přístupu</span><span class="sxs-lookup"><span data-stu-id="6eb53-140">Specify hello Access Policy</span></span>

6. <span data-ttu-id="6eb53-141">Zadejte hello Interval platnosti podpisu</span><span class="sxs-lookup"><span data-stu-id="6eb53-141">Specify hello Signature Validity Interval</span></span>

8. <span data-ttu-id="6eb53-142">Zadejte oprávnění</span><span class="sxs-lookup"><span data-stu-id="6eb53-142">Specify Permissions</span></span>

9. <span data-ttu-id="6eb53-143">Zadejte IP adresu nebo rozsah IP adres</span><span class="sxs-lookup"><span data-stu-id="6eb53-143">Specify IP Address or IP Range</span></span>

10. <span data-ttu-id="6eb53-144">Zadejte hello protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="6eb53-144">Specify hello HTTP Protocol</span></span>

11. <span data-ttu-id="6eb53-145">Zadejte rozsahy přístup tabulky</span><span class="sxs-lookup"><span data-stu-id="6eb53-145">Specify Table Access Ranges</span></span>

12. <span data-ttu-id="6eb53-146">Zadejte identifikátor podepsaná hello</span><span class="sxs-lookup"><span data-stu-id="6eb53-146">Specify hello Signed Identifier</span></span>

13. <span data-ttu-id="6eb53-147">Zadejte hello podpis</span><span class="sxs-lookup"><span data-stu-id="6eb53-147">Specify hello Signature</span></span>

<span data-ttu-id="6eb53-148">Další podrobné pokyny naleznete v tématu [vytváření SAS služby](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span><span class="sxs-lookup"><span data-stu-id="6eb53-148">For more detailed instructions, see [Constructing a Service SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span></span>

#### <a name="how-do-i-construct-an-account-sas"></a><span data-ttu-id="6eb53-149">Jak vytvořit SAS účtu?</span><span class="sxs-lookup"><span data-stu-id="6eb53-149">How do I construct an Account SAS?</span></span>

<span data-ttu-id="6eb53-150">SAS účtu deleguje přístup tooresources v jednom nebo více služeb úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-150">An Account SAS delegates access tooresources in one or more of hello storage services.</span></span> <span data-ttu-id="6eb53-151">Můžete taky delegovat přístup tooread, zápisu a operace odstranění pro kontejnery objektů blob, tabulky, fronty a sdílených složek, které se nedá vymezit přes SAS služby.</span><span class="sxs-lookup"><span data-stu-id="6eb53-151">You can also delegate access tooread, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="6eb53-152">Konstrukce SAS účtu je podobné toothat SAS služby.</span><span class="sxs-lookup"><span data-stu-id="6eb53-152">Construction of an Account SAS is similar toothat of a Service SAS.</span></span> <span data-ttu-id="6eb53-153">Podrobné pokyny najdete v tématu [vytváření SAS účtu.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span><span class="sxs-lookup"><span data-stu-id="6eb53-153">For detailed instructions, see [Constructing an Account SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span></span>

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a><span data-ttu-id="6eb53-154">Jak se při volání rozhraní REST API vynutit HTTPS?</span><span class="sxs-lookup"><span data-stu-id="6eb53-154">How do I enforce HTTPS when calling REST APIs?</span></span>

<span data-ttu-id="6eb53-155">tooenforce hello použití protokolu HTTPS při volání rozhraní REST API tooaccess objekty v účtech úložiště, můžete povolit zabezpečené přenosu požadované pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-155">tooenforce hello use of HTTPS when calling REST APIs tooaccess objects in storage accounts, you can enable Secure Transfer Required for hello storage account.</span></span> 

1. <span data-ttu-id="6eb53-156">V hello portálu Azure, vyberte **vytvořit účet úložiště**, nebo pro stávající účet úložiště, vyberte **nastavení** a potom **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="6eb53-156">In hello Azure portal, select **Create Storage Account**, or for an existing storage account, select **Settings** and then **Configuration**.</span></span>

2. <span data-ttu-id="6eb53-157">V části **zabezpečení přenosu požadované**, vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="6eb53-157">Under **Secure Transfer Required**, select **Enabled**.</span></span>

![Vytvoření účtu úložiště](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

<span data-ttu-id="6eb53-159">Podrobné pokyny, včetně způsobu tooenable zabezpečení přenosu požadované prostřednictvím kódu programu, najdete v části [vyžadují zabezpečení přenosu](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span><span class="sxs-lookup"><span data-stu-id="6eb53-159">For more detailed instructions, including how tooenable Secure Transfer Required programmatically, see [Require Secure Transfer](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span></span>

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a><span data-ttu-id="6eb53-160">Jak šifrují data v Azure File Storage?</span><span class="sxs-lookup"><span data-stu-id="6eb53-160">How do I encrypt data in Azure File Storage?</span></span>

<span data-ttu-id="6eb53-161">tooencrypt dat během přenosu s [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), můžete použít protokol SMB 3.x s Windows 10, 8 a 8.1 a Windows Server 2012 R2 a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="6eb53-161">tooencrypt data in transit with [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), you can use SMB 3.x with Windows 8, 8.1, and 10 and with Windows Server 2012 R2 and Windows Server 2016.</span></span> <span data-ttu-id="6eb53-162">Pokud používáte službu Azure souborů hello, žádné připojení bez šifrování se nezdaří, pokud "Zabezpečení přenosu požadované" je povolena.</span><span class="sxs-lookup"><span data-stu-id="6eb53-162">When you are using hello Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="6eb53-163">To zahrnuje scénáře s využitím protokolu SMB 2.1, protokolu SMB 3.0 bez šifrování a některých typů hello klient Linux SMB.</span><span class="sxs-lookup"><span data-stu-id="6eb53-163">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of hello Linux SMB client.</span></span>

#### <a name="azure-client-side-encryption"></a><span data-ttu-id="6eb53-164">Azure šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="6eb53-164">Azure Client-Side Encryption</span></span>

<span data-ttu-id="6eb53-165">Další možnost ochrany osobních údajů při přenášena mezi klientská aplikace a Azure Storage je [šifrování na straně klienta](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span><span class="sxs-lookup"><span data-stu-id="6eb53-165">Another option for protecting personal data while it’s being transferred between a client application and Azure Storage is [Client-side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span></span> <span data-ttu-id="6eb53-166">Hello data jsou zašifrována před přenášením do úložiště Azure a pokud načítáte hello data ze služby Azure Storage, hello data se dešifrují po přijetí na straně klienta hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-166">hello data is encrypted before being transferred into Azure Storage and when you retrieve hello data from Azure Storage, hello data is decrypted after it is received on hello client side.</span></span>

### <a name="azure-site-to-site-vpn"></a><span data-ttu-id="6eb53-167">Azure Site-to-Site VPN</span><span class="sxs-lookup"><span data-stu-id="6eb53-167">Azure Site-to-Site VPN</span></span>

<span data-ttu-id="6eb53-168">Efektivní způsob tooprotect osobních dat během přenosu mezi podnikovou sítí nebo uživatele a hello virtuální síť Azure je toouse [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) nebo [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuální privátní sítě (VPN).</span><span class="sxs-lookup"><span data-stu-id="6eb53-168">An effective way tooprotect personal data in transit between a corporate network or user and hello Azure virtual network is toouse a [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) or [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) Virtual Private Network (VPN).</span></span> <span data-ttu-id="6eb53-169">Připojení k síti VPN vytvoří zabezpečené šifrované tunelové propojení mezi hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="6eb53-169">A VPN connection creates a secure encrypted tunnel across hello Internet.</span></span>

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a><span data-ttu-id="6eb53-170">Vytvoření připojení site-to-site VPN</span><span class="sxs-lookup"><span data-stu-id="6eb53-170">How do I create a site-to-site VPN connection?</span></span>

<span data-ttu-id="6eb53-171">Síť site-to-site VPN připojí více uživateli v podnikové síti tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-171">A site-to-site VPN connects multiple users on hello corporate network tooAzure.</span></span> <span data-ttu-id="6eb53-172">toocreate připojení site-to-site v hello portál Azure, hello následující:</span><span class="sxs-lookup"><span data-stu-id="6eb53-172">toocreate a site-to-site connection in hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="6eb53-173">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6eb53-173">Create a virtual network.</span></span>

2. <span data-ttu-id="6eb53-174">Určení serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="6eb53-174">Specify a DNS server.</span></span>

3. <span data-ttu-id="6eb53-175">Vytvořte podsíť brány hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-175">Create hello gateway subnet.</span></span>

4. <span data-ttu-id="6eb53-176">Vytvoření brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-176">Create hello VPN gateway.</span></span> 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. <span data-ttu-id="6eb53-177">Vytvoření brány místní sítě hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-177">Create hello local network gateway.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. <span data-ttu-id="6eb53-178">Konfigurace zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="6eb53-178">Configure your VPN device.</span></span>

7. <span data-ttu-id="6eb53-179">Vytvořte připojení VPN hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-179">Create hello VPN connection.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. <span data-ttu-id="6eb53-180">Ověřte připojení k síti VPN hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-180">Verify hello VPN connection.</span></span>

<span data-ttu-id="6eb53-181">Podrobnější pokyny o tom, jak připojení toocreate site-to-site v hello Azure portálu, najdete v tématu [vytvořit Site-to-Site připojení v hello portálu Azure.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="6eb53-181">For more detailed instructions on how toocreate a site-to-site connection in hello Azure portal, see [Create a Site-to-Site  connection in hello Azure Portal.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span></span>

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a><span data-ttu-id="6eb53-182">Vytvoření připojení VPN typu point-to-site?</span><span class="sxs-lookup"><span data-stu-id="6eb53-182">How do I create a point-to-site VPN connection?</span></span>

<span data-ttu-id="6eb53-183">Síť VPN Point-to-Site vytvoří zabezpečené připojení z virtuální sítě jednotlivých klientských počítačů tooa.</span><span class="sxs-lookup"><span data-stu-id="6eb53-183">A Point-to-Site VPN creates a secure connection from an individual client computer tooa virtual network.</span></span> <span data-ttu-id="6eb53-184">To je užitečné, pokud chcete tooconnect tooAzure ze vzdáleného umístění, například z domova nebo hotelů nebo konference center.</span><span class="sxs-lookup"><span data-stu-id="6eb53-184">This is useful when you  want tooconnect tooAzure from a remote location, such as from home or a hotel or conference center.</span></span> <span data-ttu-id="6eb53-185">toocreate připojení point-to-site v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6eb53-185">toocreate a point-to-site  connection in hello Azure portal,</span></span>

1. <span data-ttu-id="6eb53-186">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6eb53-186">Create a virtual network.</span></span>

2. <span data-ttu-id="6eb53-187">Přidáte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="6eb53-187">Add a gateway subnet.</span></span>

3. <span data-ttu-id="6eb53-188">Určení serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="6eb53-188">Specify a DNS server.</span></span> <span data-ttu-id="6eb53-189">(volitelné)</span><span class="sxs-lookup"><span data-stu-id="6eb53-189">(optional)</span></span>

4. <span data-ttu-id="6eb53-190">Vytvoření brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6eb53-190">Create a virtual network gateway.</span></span>

5. <span data-ttu-id="6eb53-191">Generovat certifikáty.</span><span class="sxs-lookup"><span data-stu-id="6eb53-191">Generate certificates.</span></span>

6. <span data-ttu-id="6eb53-192">Přidejte fond adres klienta hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-192">Add hello client address pool.</span></span>

7. <span data-ttu-id="6eb53-193">Nahrajte data hello kořenového certifikátu veřejného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6eb53-193">Upload hello root certificate public certificate data.</span></span>

8. <span data-ttu-id="6eb53-194">Generování a nainstalujte balíček konfigurace klienta VPN hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-194">Generate and install hello VPN client configuration package.</span></span>

9. <span data-ttu-id="6eb53-195">Nainstalujte certifikát exportovaného klienta.</span><span class="sxs-lookup"><span data-stu-id="6eb53-195">Install an exported client certificate.</span></span>

10. <span data-ttu-id="6eb53-196">Připojte tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6eb53-196">Connect tooAzure.</span></span>

11. <span data-ttu-id="6eb53-197">Ověřte své propojení.</span><span class="sxs-lookup"><span data-stu-id="6eb53-197">Verify your connection.</span></span>

<span data-ttu-id="6eb53-198">Další podrobné pokyny naleznete v tématu [konfigurace tooa připojení Point-to-Site virtuální síť ověřování pomocí certifikátů: portál Azure.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="6eb53-198">For more detailed instructions, see [Configure a Point-to-Site connection tooa VNet using certificate authentication: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span></span>

### <a name="ssltls"></a><span data-ttu-id="6eb53-199">SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="6eb53-199">SSL/TLS</span></span>

<span data-ttu-id="6eb53-200">Společnost Microsoft doporučuje vždy použít data tooexchange protokoly SSL/TLS v různých umístěních.</span><span class="sxs-lookup"><span data-stu-id="6eb53-200">Microsoft recommends that you always use SSL/TLS protocols tooexchange data across different locations.</span></span> <span data-ttu-id="6eb53-201">Organizace, které zvolte toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove velkých datových sad přes vyhrazené vysokorychlostní propojení WAN můžete šifrovat taky hello data na úrovni aplikace pro zvýšení ochrany pomocí protokolu SSL/TLS nebo jiné protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-201">Organizations that choose toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove large data sets over a dedicated high-speed WAN link can also encrypt hello data at hello application-level using SSL/TLS or other protocols for added protection.</span></span>

### <a name="encryption-by-default"></a><span data-ttu-id="6eb53-202">Ve výchozím nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="6eb53-202">Encryption by default</span></span>

<span data-ttu-id="6eb53-203">Společnost Microsoft používá data tooprotect šifrování během přenosu mezi zákazníky a cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6eb53-203">Microsoft uses encryption tooprotect data in transit between customers and Azure cloud services.</span></span> <span data-ttu-id="6eb53-204">Pokud jsou interakci s Azure Storage prostřednictvím hello portálu Azure, všechny transakce dojít přes HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6eb53-204">If you are interacting with Azure Storage through hello Azure Portal, all transactions occur via HTTPS.</span></span>

<span data-ttu-id="6eb53-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) je protokol hello že datových Center společnosti Microsoft se pokusí toonegotiate s klientské systémy, které se připojují tooMicrosoft cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6eb53-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) is hello protocol that Microsoft data centers will attempt toonegotiate with client systems that connect tooMicrosoft cloud services.</span></span> <span data-ttu-id="6eb53-206">Protokol TLS poskytuje silné ověřování, ochrana soukromí zpráv a integritu (umožňuje detekovat zpráva manipulaci, zachycení a padělání), vzájemná funkční spolupráce, flexibilita algoritmu, snadné nasazení a používání.</span><span class="sxs-lookup"><span data-stu-id="6eb53-206">TLS provides strong authentication, message privacy, and integrity (enables detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, ease of deployment and use.</span></span>

<span data-ttu-id="6eb53-207">[Ideální Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) také zaměstnání tak, aby každé připojení mezi systémy klienta zákazníků a cloudových služeb Microsoftu používat jedinečné klíče.</span><span class="sxs-lookup"><span data-stu-id="6eb53-207">[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is also employed so that each connection between customers’ client systems and Microsoft’s cloud services use unique keys.</span></span> <span data-ttu-id="6eb53-208">Připojení tooMicrosoft cloudové služby také využít výhod šifrování RSA na základě 2 048 bitů délky klíčů.</span><span class="sxs-lookup"><span data-stu-id="6eb53-208">Connections tooMicrosoft cloud services also take advantage of RSA based 2,048-bit encryption key lengths.</span></span> <span data-ttu-id="6eb53-209">Hello kombinace protokolu TLS, délky klíčů RSA 2 048 bitů a PFS usnadňuje mnohem obtížnější někdo toointercept a přístup data, která je při přenosu mezi cloudové služby společnosti Microsoft a zákazníků.</span><span class="sxs-lookup"><span data-stu-id="6eb53-209">hello combination of TLS, RSA 2,048-bit key lengths, and PFS makes it much  more difficult for someone toointercept and access data that is in transit between Microsoft cloud services and customers.</span></span>

<span data-ttu-id="6eb53-210">Přenášená data zašifrována vždy v [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span><span class="sxs-lookup"><span data-stu-id="6eb53-210">Data in transit is always encrypted in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span></span> <span data-ttu-id="6eb53-211">Kromě toho tooencrypting data předchozí toostoring toopersistent média, hello je také vždy zabezpečená data při přenosu pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6eb53-211">In addition tooencrypting data prior toostoring toopersistent media, hello data is also always secured in transit by using HTTPS.</span></span> <span data-ttu-id="6eb53-212">HTTPS je hello pouze protokol, který je podporován pro rozhraní Data Lake Store REST hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-212">HTTPS is hello only protocol that is supported for hello Data Lake Store REST interfaces.</span></span>

## <a name="summary"></a><span data-ttu-id="6eb53-213">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6eb53-213">Summary</span></span>

<span data-ttu-id="6eb53-214">Hello společnosti můžete provádět svůj cíl ochrany osobní data a hello ochrany osobních údajů těchto údajů vynucování tooAzure připojení HTTPS úložiště, pomocí sdílené přístupové podpisy a povolení zabezpečení přenosu vyžaduje na účty úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="6eb53-214">hello company can accomplish its goal of protecting personal data and hello privacy of such data by enforcing HTTPS connections tooAzure Storage, using Shared Access Signatures and enabling Secure Transfer Required on hello storage accounts.</span></span> <span data-ttu-id="6eb53-215">Pomocí připojení protokolu SMB 3.0 a implementaci šifrování na straně klienta taky chránit osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="6eb53-215">They can also protect personal data by using SMB 3.0 connections and implementing client-side encryption.</span></span> <span data-ttu-id="6eb53-216">Připojení VPN typu Site-to-site z hello toohello podnikové sítě virtuální síť Azure. připojení point-to-site VPN od jednotlivých uživatelů se vytvoří zabezpečené tunelové propojení, pomocí kterého můžete bezpečně cestují osobní data.</span><span class="sxs-lookup"><span data-stu-id="6eb53-216">Site-to-site VPN connections from hello corporate network toohello Azure virtual network and point-to-site VPN connections from individual users will create a secure tunnel through which personal data can securely travel.</span></span> <span data-ttu-id="6eb53-217">Postupy šifrování výchozí společnosti Microsoft, budou další ochrany osobních údajů hello osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="6eb53-217">Microsoft’s default encryption practices will further protect hello privacy of personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6eb53-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6eb53-218">Next steps</span></span>

- [<span data-ttu-id="6eb53-219">Doporučené postupy zabezpečení služby Azure Data a šifrování</span><span class="sxs-lookup"><span data-stu-id="6eb53-219">Azure Data Security and Encryption Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [<span data-ttu-id="6eb53-220">Plánování a návrh pro VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="6eb53-220">Planning and design for VPN Gateway</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [<span data-ttu-id="6eb53-221">VPN Gateway – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="6eb53-221">VPN Gateway FAQ</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [<span data-ttu-id="6eb53-222">Zakoupení a konfigurace certifikátu protokolu SSL pro Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6eb53-222">Buy and configure an SSL Certificate for your Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
