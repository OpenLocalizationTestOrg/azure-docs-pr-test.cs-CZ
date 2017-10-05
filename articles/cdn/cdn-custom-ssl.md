---
title: "Povolit protokol HTTPS na vlastní domény služby Azure CDN | Microsoft Docs"
description: "Zjistěte, jak povolit protokol HTTPS na koncový bod Azure CDN s vlastní doménu."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: b334ba6bbec1d0a7e23a514174bffae01c7fff05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a><span data-ttu-id="c964d-103">Povolit protokol HTTPS na vlastní domény služby Azure CDN</span><span class="sxs-lookup"><span data-stu-id="c964d-103">Enable HTTPS on an Azure CDN custom domain</span></span>

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="c964d-104">Podpora protokolu HTTPS pro vlastní domény Azure CDN umožňuje doručovat bezpečné obsahu přes SSL pomocí vlastní název domény pro zlepšení zabezpečení dat v průběhu přenosu.</span><span class="sxs-lookup"><span data-stu-id="c964d-104">HTTPS support for Azure CDN custom domains enables you to deliver secure content via SSL using your own domain name to improve the security of data while in transit.</span></span> <span data-ttu-id="c964d-105">V pracovním postupu začátku do konce povolit HTTPS pro vaše vlastní doména je zjednodušený prostřednictvím povolování jedním kliknutím, kompletní certifikát správy a všechny s bez dalších nákladů.</span><span class="sxs-lookup"><span data-stu-id="c964d-105">The end-to-end workflow to enable HTTPS for your custom domain is simplified via one-click enablement, complete certificate management, and all with no additional cost.</span></span>

<span data-ttu-id="c964d-106">Je důležité k zajištění ochrany osobních údajů a integritu dat všech s citlivými daty webové aplikace v průběhu přenosu.</span><span class="sxs-lookup"><span data-stu-id="c964d-106">It's critical to ensure the privacy and data integrity of all of your web applications sensitive data while in transit.</span></span> <span data-ttu-id="c964d-107">Pomocí protokolu HTTPS zajišťuje, aby citlivá data šifrovala při posílání přes internet.</span><span class="sxs-lookup"><span data-stu-id="c964d-107">Using the HTTPS protocol ensures that your sensitive data is encrypted when it is sent across the internet.</span></span> <span data-ttu-id="c964d-108">Poskytuje vztah důvěryhodnosti, ověřování a chrání vaše webové aplikace před útoky.</span><span class="sxs-lookup"><span data-stu-id="c964d-108">It provides trust, authentication and protects your web applications from attacks.</span></span> <span data-ttu-id="c964d-109">Azure CDN v současné době podporuje protokol HTTPS na koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="c964d-109">Currently, Azure CDN supports HTTPS on a CDN endpoint.</span></span> <span data-ttu-id="c964d-110">Například pokud vytvoříte koncový bod CDN z Azure CDN (např. https://contoso.azureedge.net), je ve výchozím nastavení povolené HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c964d-110">For example, if you create a CDN endpoint from Azure CDN (e.g. https://contoso.azureedge.net), HTTPS is enabled by default.</span></span> <span data-ttu-id="c964d-111">Nyní s vlastní doménu HTTPS, můžete povolit zabezpečené doručování pro vlastní doménu (např. https://www.contoso.com) také.</span><span class="sxs-lookup"><span data-stu-id="c964d-111">Now, with custom domain HTTPS, you can enable secure delivery for a custom domain (e.g. https://www.contoso.com) as well.</span></span> 

<span data-ttu-id="c964d-112">Mezi klíčové atributy HTTPS funkce patří:</span><span class="sxs-lookup"><span data-stu-id="c964d-112">Some of the key attributes of HTTPS feature are:</span></span>

- <span data-ttu-id="c964d-113">Bez dalších nákladů: existují neplatí pro získání certifikátů nebo obnovení a bez dalších nákladů pro komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c964d-113">No additional cost: There are no costs for certificate acquisition or renewal and no additional cost for HTTPS traffic.</span></span> <span data-ttu-id="c964d-114">Právě platíte za GB odchozí od CDN.</span><span class="sxs-lookup"><span data-stu-id="c964d-114">You just pay for GB egress from the CDN.</span></span>

- <span data-ttu-id="c964d-115">Jednoduché povolování: jeden zřizování klikněte na tlačítko je k dispozici z [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c964d-115">Simple enablement: One click provisioning is available from the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c964d-116">K povolení této funkce můžete také použít rozhraní REST API nebo jiné nástroje pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="c964d-116">You can also use REST API or other developer tools to enable the feature.</span></span>

- <span data-ttu-id="c964d-117">Dokončení správy certifikátů: všechna certifikátů nákup a můžete je zajištěná Správa.</span><span class="sxs-lookup"><span data-stu-id="c964d-117">Complete certificate management: All certificate procurement and management is handled for you.</span></span> <span data-ttu-id="c964d-118">Certifikáty jsou automaticky zřizovat a obnovit před vypršením platnosti.</span><span class="sxs-lookup"><span data-stu-id="c964d-118">Certificates are automatically provisioned and renewed prior to expiration.</span></span> <span data-ttu-id="c964d-119">Tím se úplně odebere rizika přerušení poskytování služeb v důsledku vypršení platnosti certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c964d-119">This completely removes the risks of service interruption as a result of a certificate expiring.</span></span>

>[!NOTE] 
><span data-ttu-id="c964d-120">Před povolením podpora protokolu HTTPS, musí jste již vytvořili [vlastní doménu Azure CDN](./cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="c964d-120">Prior to enabling HTTPS support, you must have already established an [Azure CDN custom domain](./cdn-map-content-to-custom-domain.md).</span></span>

## <a name="step-1-enabling-the-feature"></a><span data-ttu-id="c964d-121">Krok 1: Povolení funkce</span><span class="sxs-lookup"><span data-stu-id="c964d-121">Step 1: Enabling the feature</span></span> 

1. <span data-ttu-id="c964d-122">V [portál Azure](https://portal.azure.com), přejděte na svůj profil Verizon standard nebo premium CDN.</span><span class="sxs-lookup"><span data-stu-id="c964d-122">In the [Azure portal](https://portal.azure.com), browse to your Verizon standard or premium CDN profile.</span></span>

2. <span data-ttu-id="c964d-123">V seznamu koncových bodů klikněte na koncový bod, který obsahuje vaše vlastní doména.</span><span class="sxs-lookup"><span data-stu-id="c964d-123">In the list of endpoints, click the endpoint containing your custom domain.</span></span>

3. <span data-ttu-id="c964d-124">Klikněte na tlačítko vlastní doménu, pro který chcete povolit protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c964d-124">Click the custom domain for which you want to enable HTTPS.</span></span>

    ![Okno koncový bod](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. <span data-ttu-id="c964d-126">Klikněte na tlačítko **na** povolit protokol HTTPS a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="c964d-126">Click **On** to enable HTTPS and save the change.</span></span>

    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a><span data-ttu-id="c964d-128">Krok 2: Ověření domény</span><span class="sxs-lookup"><span data-stu-id="c964d-128">Step 2: Domain validation</span></span>

>[!IMPORTANT] 
><span data-ttu-id="c964d-129">Předtím, než HTTPS active na vaši vlastní doménu, je třeba provést ověření domény.</span><span class="sxs-lookup"><span data-stu-id="c964d-129">You must complete domain validation before HTTPS will be active on your custom domain.</span></span> <span data-ttu-id="c964d-130">Máte 6 pracovních dnů ke schválení domény.</span><span class="sxs-lookup"><span data-stu-id="c964d-130">You have 6 business days to approve the domain.</span></span> <span data-ttu-id="c964d-131">Žádosti budou zrušeny pomocí žádná schválení do 6 pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="c964d-131">Request will be canceled with no approval within 6 business days.</span></span>  

<span data-ttu-id="c964d-132">Po povolení HTTPS na vaši vlastní doménu, bude naše HTTPS poskytovatel certifikátu DigiCert ověřit vlastnictví vaší domény kontaktováním osob žádajících o registraci pro vaši doménu, na základě informací o osob žádajících o registraci WHOIS, e-mailem (ve výchozím nastavení) nebo telefon.</span><span class="sxs-lookup"><span data-stu-id="c964d-132">After enabling HTTPS on your custom domain, our HTTPS certificate provider DigiCert will validate ownership of your domain by contacting the registrant for your domain, based on WHOIS registrant information, via email (by default) or phone.</span></span> <span data-ttu-id="c964d-133">DigiCert taky odesílat v ověřovací e-mailu následující adresy.</span><span class="sxs-lookup"><span data-stu-id="c964d-133">DigiCert will also send the verification email to the below addresses.</span></span> <span data-ttu-id="c964d-134">Pokud je privátní WHOIS osob žádajících o registraci informace, zkontrolujte, zda že můžete schválit přímo z těchto adres.</span><span class="sxs-lookup"><span data-stu-id="c964d-134">If WHOIS registrant information is private, make sure you can approve directly from one of these addresses.</span></span>

><span data-ttu-id="c964d-135">Správce @ správce < vaše domain-name.com > @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="c964d-135">admin@<your-domain-name.com> administrator@<your-domain-name.com></span></span>  
><span data-ttu-id="c964d-136">správce webového serveru @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="c964d-136">webmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="c964d-137">hostmaster @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="c964d-137">hostmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="c964d-138">správce pošty @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="c964d-138">postmaster@<your-domain-name.com></span></span>


<span data-ttu-id="c964d-139">Po přijetí e-mailu, máte dvě možnosti ověřování:</span><span class="sxs-lookup"><span data-stu-id="c964d-139">Upon receiving the email, you have two verification options:</span></span>

1. <span data-ttu-id="c964d-140">Můžete schválit všechny budoucí objednávky zadané přes stejný účet pro stejnou kořenovou doménu, například consoto.com.</span><span class="sxs-lookup"><span data-stu-id="c964d-140">You can approve all future orders placed through the same account for the same root domain, e.g. consoto.com.</span></span> <span data-ttu-id="c964d-141">Toto je doporučený postup, pokud plánujete přidat další vlastní domény v budoucnosti pro stejnou kořenovou doménu.</span><span class="sxs-lookup"><span data-stu-id="c964d-141">This is a recommended approach if you are planning to add additional custom domains in the future for the same root domain.</span></span>
 
2. <span data-ttu-id="c964d-142">Právě můžete schválit na konkrétního hostitele použitý v této žádosti.</span><span class="sxs-lookup"><span data-stu-id="c964d-142">You can just approve the specific host name used in this request.</span></span> <span data-ttu-id="c964d-143">Dodatečné schválení se bude vyžadovat pro následné požadavky.</span><span class="sxs-lookup"><span data-stu-id="c964d-143">Additional approval will be required for subsequent requests.</span></span>

    <span data-ttu-id="c964d-144">Příklad e-mailu:</span><span class="sxs-lookup"><span data-stu-id="c964d-144">Example email:</span></span>
    
    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

<span data-ttu-id="c964d-146">Po schválení DigiCert přidá vlastní název domény na síti SAN certifikát.</span><span class="sxs-lookup"><span data-stu-id="c964d-146">After approval, DigiCert will add your custom domain name to the SAN certificate.</span></span> <span data-ttu-id="c964d-147">Tento certifikát bude platný jeden rok a bude automaticky obnoveno před jeho platnost vypršela.</span><span class="sxs-lookup"><span data-stu-id="c964d-147">The certificate will be valid for one year and will be auto renewed before it's expired.</span></span>

## <a name="step-3-wait-for-the-propagation-then-start-using-your-feature"></a><span data-ttu-id="c964d-148">Krok 3: Počkejte šíření pak spusťte pomocí vaší funkce</span><span class="sxs-lookup"><span data-stu-id="c964d-148">Step 3: Wait for the propagation then start using your feature</span></span>

<span data-ttu-id="c964d-149">Po ověření názvu domény se bude trvat až 6 až 8 hodin pro funkci HTTPS vlastní domény tak, aby byl aktivní.</span><span class="sxs-lookup"><span data-stu-id="c964d-149">After the domain name is validated it will take up to 6-8 hours for the custom domain HTTPS feature to be active.</span></span> <span data-ttu-id="c964d-150">Po dokončení procesu "vlastní HTTPS" stav na portálu Azure bude nastaven na "Povoleno".</span><span class="sxs-lookup"><span data-stu-id="c964d-150">After the process is complete, the "custom HTTPS" status in the Azure portal will be set to "Enabled".</span></span> <span data-ttu-id="c964d-151">HTTPS s vaše vlastní doména je nyní připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="c964d-151">HTTPS with your custom domain is now ready for your use.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="c964d-152">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="c964d-152">Frequently asked questions</span></span>

1. <span data-ttu-id="c964d-153">*Kdo je poskytovatel certifikátu a jaký typ certifikátu se použije?*</span><span class="sxs-lookup"><span data-stu-id="c964d-153">*Who is the certificate provider and what type of certificate is used?*</span></span>

    <span data-ttu-id="c964d-154">Používáme alternativní názvy předmětu (SAN) certifikátu od DigiCert.</span><span class="sxs-lookup"><span data-stu-id="c964d-154">We use Subject Alternative Names (SAN) certificate provided by DigiCert.</span></span> <span data-ttu-id="c964d-155">Certifikát SAN můžete zabezpečit více plně kvalifikované názvy domény s jedním certifikátem.</span><span class="sxs-lookup"><span data-stu-id="c964d-155">A SAN certificate can secure multiple fully qualifIed domain names with one certificate.</span></span>

2. <span data-ttu-id="c964d-156">*Můžete používat vyhrazené certifikát?*</span><span class="sxs-lookup"><span data-stu-id="c964d-156">*Can I use my dedicated certificate?*</span></span>
    
    <span data-ttu-id="c964d-157">Aktuálně ale je plánovaná.</span><span class="sxs-lookup"><span data-stu-id="c964d-157">Not currently, but it's on the roadmap.</span></span>

3. <span data-ttu-id="c964d-158">*Co když neobdrželi ověření domény e-mailu z DigiCert?*</span><span class="sxs-lookup"><span data-stu-id="c964d-158">*What if I don't receive the domain verification email from DigiCert?*</span></span>

    <span data-ttu-id="c964d-159">Pokud jste neobdrželi e-mailu do 24 hodin, obraťte se na společnost Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c964d-159">Please contact Microsoft if you don't receive an email within 24 hours.</span></span>

4. <span data-ttu-id="c964d-160">*Používá certifikát SAN méně bezpečné než vyhrazené certifikát?*</span><span class="sxs-lookup"><span data-stu-id="c964d-160">*Is using a SAN certificate less secure than a dedicated certificate?*</span></span>
    
    <span data-ttu-id="c964d-161">Certifikát SAN standardům stejné šifrování a zabezpečení jako vyhrazené certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c964d-161">A SAN cert follows the same encryption and security standards as a dedicated cert.</span></span> <span data-ttu-id="c964d-162">Všechny vystavené certifikáty SSL používají algoritmus SHA-256 pro server rozšířené zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c964d-162">All issued SSL certificates are using SHA-256 for enhanced server security.</span></span>

5. <span data-ttu-id="c964d-163">*Můžete použít vlastní doménu HTTPS s Azure CDN společnosti Akamai?*</span><span class="sxs-lookup"><span data-stu-id="c964d-163">*Can I use custom domain HTTPS with Azure CDN from Akamai?*</span></span>

    <span data-ttu-id="c964d-164">Tato funkce je v současné době pouze s Azure CDN společnosti Verizon k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c964d-164">Currently, this feature is only available with Azure CDN from Verizon.</span></span> <span data-ttu-id="c964d-165">Pracujeme na podporu této funkce s Azure CDN společnosti Akamai v nadcházejících měsících.</span><span class="sxs-lookup"><span data-stu-id="c964d-165">We are working on supporting this feature with Azure CDN from Akamai in the coming months.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c964d-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c964d-166">Next steps</span></span>

- <span data-ttu-id="c964d-167">Zjistěte, jak nastavit [vlastní doménu na koncový bod Azure CDN](./cdn-map-content-to-custom-domain.md)</span><span class="sxs-lookup"><span data-stu-id="c964d-167">Learn how to set up a [custom domain on your Azure CDN endpoint](./cdn-map-content-to-custom-domain.md)</span></span>


