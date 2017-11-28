---
title: "Ochrana osobních údajů v přenosu pomocí šifrování v Azure | Microsoft Docs"
description: "Použití šifrování v Azure k ochraně osobních údajů"
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
ms.openlocfilehash: 99e40b8a09a2f151e7f83fbb58bdfc00ae4b1268
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a><span data-ttu-id="a541d-103">Azure šifrovací technologie: ochrana osobních údajů v přenosu s šifrováním</span><span class="sxs-lookup"><span data-stu-id="a541d-103">Azure encryption technologies: Protect personal data in transit with encryption</span></span>

<span data-ttu-id="a541d-104">Tento článek vám pomůže pochopit a používat technologie Azure šifrování k zabezpečení dat při přenosu.</span><span class="sxs-lookup"><span data-stu-id="a541d-104">This article will help you understand and use Azure encryption technologies to secure data in transit.</span></span> 

<span data-ttu-id="a541d-105">Ochrana osobních údajů osobních údajů při přenosu v síti je důležitou součástí strategie zabezpečení víceúrovňová obrany zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a541d-105">Protecting the privacy of personal data as it travels across the network is an essential part of a multi-layered defense-in-depth security strategy.</span></span> <span data-ttu-id="a541d-106">Šifrování během přenosu je navržená tak, aby útočník, který zabrání přenosů nebudou moci zobrazit nebo použít data.</span><span class="sxs-lookup"><span data-stu-id="a541d-106">Encryption in transit is designed to prevent an attacker who intercepts transmissions from being able to view or use the data.</span></span>

## <a name="scenario"></a><span data-ttu-id="a541d-107">Scénář</span><span class="sxs-lookup"><span data-stu-id="a541d-107">Scenario</span></span>

<span data-ttu-id="a541d-108">Velké výletních společnosti, centrálou ve Spojených státech amerických, je rozšířit jeho operací a nabídnout itineráře v Středomoří, Jaderského a baltský moři, jakož i Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="a541d-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="a541d-109">Pro podporu těchto úsilí, získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a Spojeném království</span><span class="sxs-lookup"><span data-stu-id="a541d-109">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span> 

<span data-ttu-id="a541d-110">Společnost používá Microsoft Azure k ukládání firemních dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="a541d-110">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="a541d-111">To zahrnuje osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a541d-111">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="a541d-112">Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako je adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti.</span><span class="sxs-lookup"><span data-stu-id="a541d-112">It also includes traditional Human Resource information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="a541d-113">Na řádku výletních také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje ke sledování vztahů se zákazníky aktuální a starší.</span><span class="sxs-lookup"><span data-stu-id="a541d-113">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

<span data-ttu-id="a541d-114">Osobní údaje zákazníků je zadána v databázi ze vzdálených pobočkách společnosti a z agentů cesta umístěné po celém světě.</span><span class="sxs-lookup"><span data-stu-id="a541d-114">Personal data of customers is entered in the database from the company’s remote offices and from travel agents located around the world.</span></span> <span data-ttu-id="a541d-115">Dokumenty, které obsahují informace o zákazníkovi se přenáší přes síť do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a541d-115">Documents containing customer information are transferred across the network to Azure storage.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="a541d-116">Popis problému</span><span class="sxs-lookup"><span data-stu-id="a541d-116">Problem statement</span></span>

<span data-ttu-id="a541d-117">Společnosti musí ochrany osobních údajů zaměstnanců a zákazníků osobních údajů, i když je při přenosu do a ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="a541d-117">The company must protect the privacy of customers’ and employees’ personal data while it is in transit to and from Azure services.</span></span>

## <a name="company-goal"></a><span data-ttu-id="a541d-118">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="a541d-118">Company goal</span></span>

<span data-ttu-id="a541d-119">Cílem společnosti zajistit, že osobní data se šifrují při vypnutí disku.</span><span class="sxs-lookup"><span data-stu-id="a541d-119">The company goal to ensure that personal data is encrypted when off disk.</span></span> <span data-ttu-id="a541d-120">Pokud neoprávněné osoby intercept vypnout disku osobní data, musí být ve formuláři, který bude zobrazovat nečitelná.</span><span class="sxs-lookup"><span data-stu-id="a541d-120">If unauthorized persons intercept the off-disk personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="a541d-121">Použití šifrování by měla být snadná nebo zcela transparentní, uživatelům a správcům.</span><span class="sxs-lookup"><span data-stu-id="a541d-121">Applying encryption should be easy, or completely transparent, for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="a541d-122">Řešení</span><span class="sxs-lookup"><span data-stu-id="a541d-122">Solutions</span></span>

<span data-ttu-id="a541d-123">Azure services poskytují několik nástrojů a technologie, které vám umožní chránit osobní údaje při přenosu.</span><span class="sxs-lookup"><span data-stu-id="a541d-123">Azure services provide multiple tools and technologies to help you protect personal data in transit.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="a541d-124">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a541d-124">Azure Storage</span></span>

<span data-ttu-id="a541d-125">Data, která je uložená v cloudu musíte se dostavit z klienta, které můžou být fyzicky umístěná kdekoliv na světě, do datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="a541d-125">Data that is stored in the cloud must travel from the client, which can be physically located anywhere in the world, to the Azure data center.</span></span> <span data-ttu-id="a541d-126">Pokud tato data jsou načítána pro uživatele, přenáší se znovu, v opačném směru.</span><span class="sxs-lookup"><span data-stu-id="a541d-126">When that data is retrieved by users, it travels again, in the opposite direction.</span></span> <span data-ttu-id="a541d-127">Data, která je při přenosu prostřednictvím veřejného Internetu je vždy hrozí zachycení útočníků.</span><span class="sxs-lookup"><span data-stu-id="a541d-127">Data that is in transit over the public Internet is always at risk of interception by attackers.</span></span> <span data-ttu-id="a541d-128">Je důležité chránit osobní údaje osobních údajů s využitím šifrování transportní vrstvy je zabezpečit při jejich přesunu mezi umístěními.</span><span class="sxs-lookup"><span data-stu-id="a541d-128">It is important to protect the privacy of personal data by using transport-level encryption to secure it as it moves between locations.</span></span>

<span data-ttu-id="a541d-129">Protokol HTTPS poskytuje zabezpečené, šifrované komunikační kanál přes Internet.</span><span class="sxs-lookup"><span data-stu-id="a541d-129">The HTTPS protocol provides a secure, encrypted communications channel over the Internet.</span></span> <span data-ttu-id="a541d-130">HTTPS slouží k přístupu k objektům ve službě Azure Storage a při volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="a541d-130">HTTPS should be used to access objects in Azure Storage and when calling REST APIs.</span></span> <span data-ttu-id="a541d-131">Vynutit používání protokolu HTTPS, při použití [sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) delegovat přístup k objektům Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a541d-131">You enforce use of the HTTPS protocol when using [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) to delegate access to Azure Storage objects.</span></span> <span data-ttu-id="a541d-132">Existují dva typy SAS: SAS služby a SAS účtu.</span><span class="sxs-lookup"><span data-stu-id="a541d-132">There are two types of SAS: Service SAS and Account SAS.</span></span>

#### <a name="how-do-i-construct-a-service-sas"></a><span data-ttu-id="a541d-133">Jak vytvořit SAS služby?</span><span class="sxs-lookup"><span data-stu-id="a541d-133">How do I construct a Service SAS?</span></span>

<span data-ttu-id="a541d-134">SAS služby deleguje přístup k prostředku jen v jedné službě úložiště (služba objektů blob, fronty, tabulka nebo souboru).</span><span class="sxs-lookup"><span data-stu-id="a541d-134">A Service SAS delegates access to a resource in just one of the storage services (blob, queue, table or file service).</span></span> <span data-ttu-id="a541d-135">K vytvoření služby SAS, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a541d-135">To construct a Service SAS, do the following:</span></span>

1. <span data-ttu-id="a541d-136">Zadejte pole podepsaný verze</span><span class="sxs-lookup"><span data-stu-id="a541d-136">Specify the Signed Version Field</span></span>

2. <span data-ttu-id="a541d-137">Zadejte podepsaných prostředků (objektů Blob a pouze služba souborů)</span><span class="sxs-lookup"><span data-stu-id="a541d-137">Specify the Signed Resource (Blob and File Service Only)</span></span>

3. <span data-ttu-id="a541d-138">Zadejte parametry dotazu k přepsání hlavičky odpovědi (služby objektů Blob a pouze služba souborů)</span><span class="sxs-lookup"><span data-stu-id="a541d-138">Specify Query Parameters to Override Response Headers (Blob Service and File Service Only)</span></span>

4. <span data-ttu-id="a541d-139">Zadejte název tabulky (pouze služby Table)</span><span class="sxs-lookup"><span data-stu-id="a541d-139">Specify the Table Name (Table Service Only)</span></span>

5. <span data-ttu-id="a541d-140">Zadejte zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="a541d-140">Specify the Access Policy</span></span>

6. <span data-ttu-id="a541d-141">Zadejte Interval platnosti podpisu</span><span class="sxs-lookup"><span data-stu-id="a541d-141">Specify the Signature Validity Interval</span></span>

8. <span data-ttu-id="a541d-142">Zadejte oprávnění</span><span class="sxs-lookup"><span data-stu-id="a541d-142">Specify Permissions</span></span>

9. <span data-ttu-id="a541d-143">Zadejte IP adresu nebo rozsah IP adres</span><span class="sxs-lookup"><span data-stu-id="a541d-143">Specify IP Address or IP Range</span></span>

10. <span data-ttu-id="a541d-144">Zadejte protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="a541d-144">Specify the HTTP Protocol</span></span>

11. <span data-ttu-id="a541d-145">Zadejte rozsahy přístup tabulky</span><span class="sxs-lookup"><span data-stu-id="a541d-145">Specify Table Access Ranges</span></span>

12. <span data-ttu-id="a541d-146">Zadejte identifikátor podepsané</span><span class="sxs-lookup"><span data-stu-id="a541d-146">Specify the Signed Identifier</span></span>

13. <span data-ttu-id="a541d-147">Zadejte podpis</span><span class="sxs-lookup"><span data-stu-id="a541d-147">Specify the Signature</span></span>

<span data-ttu-id="a541d-148">Další podrobné pokyny naleznete v tématu [vytváření SAS služby](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span><span class="sxs-lookup"><span data-stu-id="a541d-148">For more detailed instructions, see [Constructing a Service SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span></span>

#### <a name="how-do-i-construct-an-account-sas"></a><span data-ttu-id="a541d-149">Jak vytvořit SAS účtu?</span><span class="sxs-lookup"><span data-stu-id="a541d-149">How do I construct an Account SAS?</span></span>

<span data-ttu-id="a541d-150">SAS účtu deleguje přístup k prostředkům v jedné nebo více službách úložiště.</span><span class="sxs-lookup"><span data-stu-id="a541d-150">An Account SAS delegates access to resources in one or more of the storage services.</span></span> <span data-ttu-id="a541d-151">Můžete taky delegovat přístup k operacím čtení, zápis a odstranění pro kontejnery objektů blob, tabulky a sdílené složky, který se nedá vymezit přes SAS služby.</span><span class="sxs-lookup"><span data-stu-id="a541d-151">You can also delegate access to read, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="a541d-152">Konstrukce SAS účtu je podobná SAS služby.</span><span class="sxs-lookup"><span data-stu-id="a541d-152">Construction of an Account SAS is similar to that of a Service SAS.</span></span> <span data-ttu-id="a541d-153">Podrobné pokyny najdete v tématu [vytváření SAS účtu.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span><span class="sxs-lookup"><span data-stu-id="a541d-153">For detailed instructions, see [Constructing an Account SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span></span>

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a><span data-ttu-id="a541d-154">Jak se při volání rozhraní REST API vynutit HTTPS?</span><span class="sxs-lookup"><span data-stu-id="a541d-154">How do I enforce HTTPS when calling REST APIs?</span></span>

<span data-ttu-id="a541d-155">Pokud chcete vynutit použití protokolu HTTPS, při volání rozhraní REST API pro přístup k objektům v účtech úložiště, můžete povolit zabezpečení přenosu požadované pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a541d-155">To enforce the use of HTTPS when calling REST APIs to access objects in storage accounts, you can enable Secure Transfer Required for the storage account.</span></span> 

1. <span data-ttu-id="a541d-156">Na portálu Azure vyberte **vytvořit účet úložiště**, nebo pro stávající účet úložiště, vyberte **nastavení** a potom **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="a541d-156">In the Azure portal, select **Create Storage Account**, or for an existing storage account, select **Settings** and then **Configuration**.</span></span>

2. <span data-ttu-id="a541d-157">V části **zabezpečení přenosu požadované**, vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="a541d-157">Under **Secure Transfer Required**, select **Enabled**.</span></span>

![Vytvoření účtu úložiště](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

<span data-ttu-id="a541d-159">Podrobné pokyny, včetně toho, jak povolit zabezpečení přenosu požadované prostřednictvím kódu programu, najdete v článku [vyžadují zabezpečení přenosu](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span><span class="sxs-lookup"><span data-stu-id="a541d-159">For more detailed instructions, including how to enable Secure Transfer Required programmatically, see [Require Secure Transfer](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span></span>

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a><span data-ttu-id="a541d-160">Jak šifrují data v Azure File Storage?</span><span class="sxs-lookup"><span data-stu-id="a541d-160">How do I encrypt data in Azure File Storage?</span></span>

<span data-ttu-id="a541d-161">K šifrování dat během přenosu s [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), můžete použít protokol SMB 3.x s Windows 10, 8 a 8.1 a Windows Server 2012 R2 a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="a541d-161">To encrypt data in transit with [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), you can use SMB 3.x with Windows 8, 8.1, and 10 and with Windows Server 2012 R2 and Windows Server 2016.</span></span> <span data-ttu-id="a541d-162">Pokud používáte službu Azure soubory, žádné připojení bez šifrování se nezdaří, pokud "Zabezpečení přenosu požadované" je povolena.</span><span class="sxs-lookup"><span data-stu-id="a541d-162">When you are using the Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="a541d-163">To zahrnuje scénáře s využitím protokolu SMB 2.1, protokolu SMB 3.0 bez šifrování a některých typů klient Linux SMB.</span><span class="sxs-lookup"><span data-stu-id="a541d-163">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of the Linux SMB client.</span></span>

#### <a name="azure-client-side-encryption"></a><span data-ttu-id="a541d-164">Azure šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="a541d-164">Azure Client-Side Encryption</span></span>

<span data-ttu-id="a541d-165">Další možnost ochrany osobních údajů při přenášena mezi klientská aplikace a Azure Storage je [šifrování na straně klienta](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span><span class="sxs-lookup"><span data-stu-id="a541d-165">Another option for protecting personal data while it’s being transferred between a client application and Azure Storage is [Client-side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span></span> <span data-ttu-id="a541d-166">Data se zašifrují před přenášením do úložiště Azure a při načítání dat z Azure Storage, data se dešifrují po přijetí na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="a541d-166">The data is encrypted before being transferred into Azure Storage and when you retrieve the data from Azure Storage, the data is decrypted after it is received on the client side.</span></span>

### <a name="azure-site-to-site-vpn"></a><span data-ttu-id="a541d-167">Azure Site-to-Site VPN</span><span class="sxs-lookup"><span data-stu-id="a541d-167">Azure Site-to-Site VPN</span></span>

<span data-ttu-id="a541d-168">Efektivní způsob, jak chránit osobní údaje při přenosu mezi podnikovou sítí nebo uživatele a virtuální síť Azure má používat [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) nebo [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuální privátní sítě (VPN).</span><span class="sxs-lookup"><span data-stu-id="a541d-168">An effective way to protect personal data in transit between a corporate network or user and the Azure virtual network is to use a [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) or [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) Virtual Private Network (VPN).</span></span> <span data-ttu-id="a541d-169">Připojení k síti VPN vytvoří zabezpečené tunelové propojení šifrované přes Internet.</span><span class="sxs-lookup"><span data-stu-id="a541d-169">A VPN connection creates a secure encrypted tunnel across the Internet.</span></span>

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a><span data-ttu-id="a541d-170">Vytvoření připojení site-to-site VPN</span><span class="sxs-lookup"><span data-stu-id="a541d-170">How do I create a site-to-site VPN connection?</span></span>

<span data-ttu-id="a541d-171">Síť site-to-site VPN připojí více uživatelů v podnikové síti do Azure.</span><span class="sxs-lookup"><span data-stu-id="a541d-171">A site-to-site VPN connects multiple users on the corporate network to Azure.</span></span> <span data-ttu-id="a541d-172">Pokud chcete vytvořit připojení site-to-site na portálu Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a541d-172">To create a site-to-site connection in the Azure portal, do the following:</span></span>

1. <span data-ttu-id="a541d-173">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a541d-173">Create a virtual network.</span></span>

2. <span data-ttu-id="a541d-174">Určení serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a541d-174">Specify a DNS server.</span></span>

3. <span data-ttu-id="a541d-175">Vytvořte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="a541d-175">Create the gateway subnet.</span></span>

4. <span data-ttu-id="a541d-176">Vytvoření brány VPN.</span><span class="sxs-lookup"><span data-stu-id="a541d-176">Create the VPN gateway.</span></span> 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. <span data-ttu-id="a541d-177">Vytvořte bránu místní sítě.</span><span class="sxs-lookup"><span data-stu-id="a541d-177">Create the local network gateway.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. <span data-ttu-id="a541d-178">Konfigurace zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="a541d-178">Configure your VPN device.</span></span>

7. <span data-ttu-id="a541d-179">Vytvoření připojení k síti VPN.</span><span class="sxs-lookup"><span data-stu-id="a541d-179">Create the VPN connection.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. <span data-ttu-id="a541d-180">Ověření připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="a541d-180">Verify the VPN connection.</span></span>

<span data-ttu-id="a541d-181">Podrobnější pokyny o tom, jak vytvořit připojení site-to-site na portálu Azure najdete v tématu [vytvořte připojení Site-to-Site na portálu Azure.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="a541d-181">For more detailed instructions on how to create a site-to-site connection in the Azure portal, see [Create a Site-to-Site  connection in the Azure Portal.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span></span>

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a><span data-ttu-id="a541d-182">Vytvoření připojení VPN typu point-to-site?</span><span class="sxs-lookup"><span data-stu-id="a541d-182">How do I create a point-to-site VPN connection?</span></span>

<span data-ttu-id="a541d-183">Síť VPN Point-to-Site vytvoří zabezpečené připojení z jednotlivých klientských počítačů k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="a541d-183">A Point-to-Site VPN creates a secure connection from an individual client computer to a virtual network.</span></span> <span data-ttu-id="a541d-184">To je užitečné, pokud chcete připojit k Azure ze vzdáleného umístění, například z domova nebo hotelů nebo konference center.</span><span class="sxs-lookup"><span data-stu-id="a541d-184">This is useful when you  want to connect to Azure from a remote location, such as from home or a hotel or conference center.</span></span> <span data-ttu-id="a541d-185">Na portálu Azure vytvoříte připojení typu point-to-site</span><span class="sxs-lookup"><span data-stu-id="a541d-185">To create a point-to-site  connection in the Azure portal,</span></span>

1. <span data-ttu-id="a541d-186">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a541d-186">Create a virtual network.</span></span>

2. <span data-ttu-id="a541d-187">Přidáte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="a541d-187">Add a gateway subnet.</span></span>

3. <span data-ttu-id="a541d-188">Určení serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a541d-188">Specify a DNS server.</span></span> <span data-ttu-id="a541d-189">(volitelné)</span><span class="sxs-lookup"><span data-stu-id="a541d-189">(optional)</span></span>

4. <span data-ttu-id="a541d-190">Vytvoření brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a541d-190">Create a virtual network gateway.</span></span>

5. <span data-ttu-id="a541d-191">Generovat certifikáty.</span><span class="sxs-lookup"><span data-stu-id="a541d-191">Generate certificates.</span></span>

6. <span data-ttu-id="a541d-192">Přidejte fond adres klienta.</span><span class="sxs-lookup"><span data-stu-id="a541d-192">Add the client address pool.</span></span>

7. <span data-ttu-id="a541d-193">Nahrajte data certifikátu veřejného kořenového certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a541d-193">Upload the root certificate public certificate data.</span></span>

8. <span data-ttu-id="a541d-194">Generování a nainstalujte balíček konfigurace klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="a541d-194">Generate and install the VPN client configuration package.</span></span>

9. <span data-ttu-id="a541d-195">Nainstalujte certifikát exportovaného klienta.</span><span class="sxs-lookup"><span data-stu-id="a541d-195">Install an exported client certificate.</span></span>

10. <span data-ttu-id="a541d-196">Připojte k Azure.</span><span class="sxs-lookup"><span data-stu-id="a541d-196">Connect to Azure.</span></span>

11. <span data-ttu-id="a541d-197">Ověřte své propojení.</span><span class="sxs-lookup"><span data-stu-id="a541d-197">Verify your connection.</span></span>

<span data-ttu-id="a541d-198">Další podrobné pokyny naleznete v tématu [konfigurace připojení typu Point-to-Site k virtuální síti ověřování pomocí certifikátů: portál Azure.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="a541d-198">For more detailed instructions, see [Configure a Point-to-Site connection to a VNet using certificate authentication: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span></span>

### <a name="ssltls"></a><span data-ttu-id="a541d-199">SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="a541d-199">SSL/TLS</span></span>

<span data-ttu-id="a541d-200">Společnost Microsoft doporučuje vždy používat protokoly SSL/TLS pro výměnu dat v různých umístěních.</span><span class="sxs-lookup"><span data-stu-id="a541d-200">Microsoft recommends that you always use SSL/TLS protocols to exchange data across different locations.</span></span> <span data-ttu-id="a541d-201">Organizace, které se rozhodnete použít [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) přesunout velké sady dat přes síť WAN vyhrazené vysokorychlostní můžete odkaz taky šifrování dat na úrovni aplikace pomocí protokolu SSL/TLS nebo jiné protokoly pro přidání ochrany.</span><span class="sxs-lookup"><span data-stu-id="a541d-201">Organizations that choose to use [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) to move large data sets over a dedicated high-speed WAN link can also encrypt the data at the application-level using SSL/TLS or other protocols for added protection.</span></span>

### <a name="encryption-by-default"></a><span data-ttu-id="a541d-202">Ve výchozím nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="a541d-202">Encryption by default</span></span>

<span data-ttu-id="a541d-203">Společnost Microsoft používá šifrování k ochraně dat během přenosu mezi zákazníky a cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="a541d-203">Microsoft uses encryption to protect data in transit between customers and Azure cloud services.</span></span> <span data-ttu-id="a541d-204">Pokud jsou interakci s Azure Storage prostřednictvím portálu Azure, všechny transakce dojít přes HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a541d-204">If you are interacting with Azure Storage through the Azure Portal, all transactions occur via HTTPS.</span></span>

<span data-ttu-id="a541d-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) je protokol, který datových Center společnosti Microsoft se pokusí o vyjednávání s klientské systémy, které se připojují ke cloudovým službám Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="a541d-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) is the protocol that Microsoft data centers will attempt to negotiate with client systems that connect to Microsoft cloud services.</span></span> <span data-ttu-id="a541d-206">Protokol TLS poskytuje silné ověřování, ochrana soukromí zpráv a integritu (umožňuje detekovat zpráva manipulaci, zachycení a padělání), vzájemná funkční spolupráce, flexibilita algoritmu, snadné nasazení a používání.</span><span class="sxs-lookup"><span data-stu-id="a541d-206">TLS provides strong authentication, message privacy, and integrity (enables detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, ease of deployment and use.</span></span>

<span data-ttu-id="a541d-207">[Ideální Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) také zaměstnání tak, aby každé připojení mezi systémy klienta zákazníků a cloudových služeb Microsoftu používat jedinečné klíče.</span><span class="sxs-lookup"><span data-stu-id="a541d-207">[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is also employed so that each connection between customers’ client systems and Microsoft’s cloud services use unique keys.</span></span> <span data-ttu-id="a541d-208">Připojení ke cloudovým službám Microsoftu taky využít výhod šifrování RSA na základě 2 048 bitů délky klíčů.</span><span class="sxs-lookup"><span data-stu-id="a541d-208">Connections to Microsoft cloud services also take advantage of RSA based 2,048-bit encryption key lengths.</span></span> <span data-ttu-id="a541d-209">Kombinace protokolu TLS, délky klíčů RSA 2 048 bitů a PFS usnadňuje mnohem obtížnější někdo k zachycení a přístup data, která je při přenosu mezi cloudové služby společnosti Microsoft a zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a541d-209">The combination of TLS, RSA 2,048-bit key lengths, and PFS makes it much  more difficult for someone to intercept and access data that is in transit between Microsoft cloud services and customers.</span></span>

<span data-ttu-id="a541d-210">Přenášená data zašifrována vždy v [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span><span class="sxs-lookup"><span data-stu-id="a541d-210">Data in transit is always encrypted in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span></span> <span data-ttu-id="a541d-211">Kromě šifrování dat před uložením na trvalé médium se také vždy šifrují přenášená data pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a541d-211">In addition to encrypting data prior to storing to persistent media, the data is also always secured in transit by using HTTPS.</span></span> <span data-ttu-id="a541d-212">HTTPS je jediný protokol, který se podporuje v rozhraních REST služby Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a541d-212">HTTPS is the only protocol that is supported for the Data Lake Store REST interfaces.</span></span>

## <a name="summary"></a><span data-ttu-id="a541d-213">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a541d-213">Summary</span></span>

<span data-ttu-id="a541d-214">Společnost můžete provést jeho cílem ochrany osobních údajů a ochrana osobních údajů těchto údajů vynucování připojení prostřednictvím protokolu HTTPS do služby Azure Storage, pomocí sdílené přístupové podpisy a povolení zabezpečení přenosu vyžaduje na účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="a541d-214">The company can accomplish its goal of protecting personal data and the privacy of such data by enforcing HTTPS connections to Azure Storage, using Shared Access Signatures and enabling Secure Transfer Required on the storage accounts.</span></span> <span data-ttu-id="a541d-215">Pomocí připojení protokolu SMB 3.0 a implementaci šifrování na straně klienta taky chránit osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="a541d-215">They can also protect personal data by using SMB 3.0 connections and implementing client-side encryption.</span></span> <span data-ttu-id="a541d-216">Připojení Site-to-site VPN z podnikové sítě pro virtuální síť Azure a připojení VPN typu point-to-site z jednotlivých uživatelů vytvoří zabezpečené tunelové propojení, pomocí kterého můžete bezpečně cestují osobní data.</span><span class="sxs-lookup"><span data-stu-id="a541d-216">Site-to-site VPN connections from the corporate network to the Azure virtual network and point-to-site VPN connections from individual users will create a secure tunnel through which personal data can securely travel.</span></span> <span data-ttu-id="a541d-217">Postupy šifrování výchozí společnosti Microsoft bude další ochrany osobních údajů osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="a541d-217">Microsoft’s default encryption practices will further protect the privacy of personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a541d-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a541d-218">Next steps</span></span>

- [<span data-ttu-id="a541d-219">Doporučené postupy zabezpečení služby Azure Data a šifrování</span><span class="sxs-lookup"><span data-stu-id="a541d-219">Azure Data Security and Encryption Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [<span data-ttu-id="a541d-220">Plánování a návrh pro VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="a541d-220">Planning and design for VPN Gateway</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [<span data-ttu-id="a541d-221">VPN Gateway – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a541d-221">VPN Gateway FAQ</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [<span data-ttu-id="a541d-222">Zakoupení a konfigurace certifikátu protokolu SSL pro Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a541d-222">Buy and configure an SSL Certificate for your Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
