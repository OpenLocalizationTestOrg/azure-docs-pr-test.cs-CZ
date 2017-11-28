---
title: "AAA \"Povolit protokol HTTPS na vlastní domény služby Azure CDN | Microsoft Docs\""
description: "Zjistěte, jak tooenable HTTPS na koncový bod Azure CDN s vlastní doménu."
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
ms.openlocfilehash: 93746222616c9ed7977ec3b22c38ac1d43b118f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a><span data-ttu-id="64954-103">Povolit protokol HTTPS na vlastní domény služby Azure CDN</span><span class="sxs-lookup"><span data-stu-id="64954-103">Enable HTTPS on an Azure CDN custom domain</span></span>

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="64954-104">Podpora protokolu HTTPS pro vlastní domény Azure CDN vám umožní bezpečné obsahu toodeliver přes SSL používá vlastní domény název tooimprove hello zabezpečení dat v průběhu přenosu.</span><span class="sxs-lookup"><span data-stu-id="64954-104">HTTPS support for Azure CDN custom domains enables you toodeliver secure content via SSL using your own domain name tooimprove hello security of data while in transit.</span></span> <span data-ttu-id="64954-105">Hello-komplexní pracovní postup tooenable HTTPS pro vaše vlastní doména je zjednodušený prostřednictvím povolování jedním kliknutím, kompletní certifikát správy a všechny s bez dalších nákladů.</span><span class="sxs-lookup"><span data-stu-id="64954-105">hello end-to-end workflow tooenable HTTPS for your custom domain is simplified via one-click enablement, complete certificate management, and all with no additional cost.</span></span>

<span data-ttu-id="64954-106">Je důležité tooensure hello ochrany osobních údajů a integritu dat všech s citlivými daty webové aplikace v průběhu přenosu.</span><span class="sxs-lookup"><span data-stu-id="64954-106">It's critical tooensure hello privacy and data integrity of all of your web applications sensitive data while in transit.</span></span> <span data-ttu-id="64954-107">Pomocí protokolu HTTPS zajišťuje, aby citlivá data šifrovala při posílání přes hello hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="64954-107">Using hello HTTPS protocol ensures that your sensitive data is encrypted when it is sent across hello internet.</span></span> <span data-ttu-id="64954-108">Poskytuje vztah důvěryhodnosti, ověřování a chrání vaše webové aplikace před útoky.</span><span class="sxs-lookup"><span data-stu-id="64954-108">It provides trust, authentication and protects your web applications from attacks.</span></span> <span data-ttu-id="64954-109">Azure CDN v současné době podporuje protokol HTTPS na koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="64954-109">Currently, Azure CDN supports HTTPS on a CDN endpoint.</span></span> <span data-ttu-id="64954-110">Například pokud vytvoříte koncový bod CDN z Azure CDN (např. https://contoso.azureedge.net), je ve výchozím nastavení povolené HTTPS.</span><span class="sxs-lookup"><span data-stu-id="64954-110">For example, if you create a CDN endpoint from Azure CDN (e.g. https://contoso.azureedge.net), HTTPS is enabled by default.</span></span> <span data-ttu-id="64954-111">Nyní s vlastní doménu HTTPS, můžete povolit zabezpečené doručování pro vlastní doménu (např. https://www.contoso.com) také.</span><span class="sxs-lookup"><span data-stu-id="64954-111">Now, with custom domain HTTPS, you can enable secure delivery for a custom domain (e.g. https://www.contoso.com) as well.</span></span> 

<span data-ttu-id="64954-112">Mezi klíčové atributy hello HTTPS funkce patří:</span><span class="sxs-lookup"><span data-stu-id="64954-112">Some of hello key attributes of HTTPS feature are:</span></span>

- <span data-ttu-id="64954-113">Bez dalších nákladů: existují neplatí pro získání certifikátů nebo obnovení a bez dalších nákladů pro komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="64954-113">No additional cost: There are no costs for certificate acquisition or renewal and no additional cost for HTTPS traffic.</span></span> <span data-ttu-id="64954-114">Platíte jenom odchozí GB z hello CDN.</span><span class="sxs-lookup"><span data-stu-id="64954-114">You just pay for GB egress from hello CDN.</span></span>

- <span data-ttu-id="64954-115">Jednoduché povolování: jeden zřizování klikněte na tlačítko je k dispozici z hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="64954-115">Simple enablement: One click provisioning is available from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="64954-116">Můžete také použít rozhraní REST API nebo jiné funkce hello tooenable nástroje pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="64954-116">You can also use REST API or other developer tools tooenable hello feature.</span></span>

- <span data-ttu-id="64954-117">Dokončení správy certifikátů: všechna certifikátů nákup a můžete je zajištěná Správa.</span><span class="sxs-lookup"><span data-stu-id="64954-117">Complete certificate management: All certificate procurement and management is handled for you.</span></span> <span data-ttu-id="64954-118">Certifikáty jsou automaticky zřízený a obnovit předchozí tooexpiration.</span><span class="sxs-lookup"><span data-stu-id="64954-118">Certificates are automatically provisioned and renewed prior tooexpiration.</span></span> <span data-ttu-id="64954-119">Tím se úplně odebere hello riziky přerušení poskytování služeb v důsledku vypršení platnosti certifikátu.</span><span class="sxs-lookup"><span data-stu-id="64954-119">This completely removes hello risks of service interruption as a result of a certificate expiring.</span></span>

>[!NOTE] 
><span data-ttu-id="64954-120">Podpora HTTPS předchozí tooenabling, musí jste již vytvořili [vlastní doménu Azure CDN](./cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="64954-120">Prior tooenabling HTTPS support, you must have already established an [Azure CDN custom domain](./cdn-map-content-to-custom-domain.md).</span></span>

## <a name="step-1-enabling-hello-feature"></a><span data-ttu-id="64954-121">Krok 1: Povolení funkce hello</span><span class="sxs-lookup"><span data-stu-id="64954-121">Step 1: Enabling hello feature</span></span> 

1. <span data-ttu-id="64954-122">V hello [portál Azure](https://portal.azure.com), procházet tooyour Verizon standard nebo premium profil CDN.</span><span class="sxs-lookup"><span data-stu-id="64954-122">In hello [Azure portal](https://portal.azure.com), browse tooyour Verizon standard or premium CDN profile.</span></span>

2. <span data-ttu-id="64954-123">Hello seznamu koncových bodů klikněte na koncový bod hello obsahující vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="64954-123">In hello list of endpoints, click hello endpoint containing your custom domain.</span></span>

3. <span data-ttu-id="64954-124">Klikněte na tlačítko hello vlastní doménu, pro které chcete tooenable HTTPS.</span><span class="sxs-lookup"><span data-stu-id="64954-124">Click hello custom domain for which you want tooenable HTTPS.</span></span>

    ![Okno koncový bod](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. <span data-ttu-id="64954-126">Klikněte na tlačítko **na** tooenable HTTPS a uložte změnu hello.</span><span class="sxs-lookup"><span data-stu-id="64954-126">Click **On** tooenable HTTPS and save hello change.</span></span>

    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a><span data-ttu-id="64954-128">Krok 2: Ověření domény</span><span class="sxs-lookup"><span data-stu-id="64954-128">Step 2: Domain validation</span></span>

>[!IMPORTANT] 
><span data-ttu-id="64954-129">Předtím, než HTTPS active na vaši vlastní doménu, je třeba provést ověření domény.</span><span class="sxs-lookup"><span data-stu-id="64954-129">You must complete domain validation before HTTPS will be active on your custom domain.</span></span> <span data-ttu-id="64954-130">Máte 6 obchodní dnů tooapprove hello domény.</span><span class="sxs-lookup"><span data-stu-id="64954-130">You have 6 business days tooapprove hello domain.</span></span> <span data-ttu-id="64954-131">Žádosti budou zrušeny pomocí žádná schválení do 6 pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="64954-131">Request will be canceled with no approval within 6 business days.</span></span>  

<span data-ttu-id="64954-132">Po povolení HTTPS na vaši vlastní doménu, bude naše HTTPS poskytovatel certifikátu DigiCert ověřit vlastnictví vaší domény kontaktováním hello osob žádajících o registraci pro vaši doménu, na základě informací o osob žádajících o registraci WHOIS, e-mailem (ve výchozím nastavení) nebo telefon.</span><span class="sxs-lookup"><span data-stu-id="64954-132">After enabling HTTPS on your custom domain, our HTTPS certificate provider DigiCert will validate ownership of your domain by contacting hello registrant for your domain, based on WHOIS registrant information, via email (by default) or phone.</span></span> <span data-ttu-id="64954-133">DigiCert odešle také hello ověření e-mailu toohello níže adresy.</span><span class="sxs-lookup"><span data-stu-id="64954-133">DigiCert will also send hello verification email toohello below addresses.</span></span> <span data-ttu-id="64954-134">Pokud je privátní WHOIS osob žádajících o registraci informace, zkontrolujte, zda že můžete schválit přímo z těchto adres.</span><span class="sxs-lookup"><span data-stu-id="64954-134">If WHOIS registrant information is private, make sure you can approve directly from one of these addresses.</span></span>

><span data-ttu-id="64954-135">Správce @ správce < vaše domain-name.com > @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="64954-135">admin@<your-domain-name.com> administrator@<your-domain-name.com></span></span>  
><span data-ttu-id="64954-136">správce webového serveru @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="64954-136">webmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="64954-137">hostmaster @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="64954-137">hostmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="64954-138">správce pošty @< vaše domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="64954-138">postmaster@<your-domain-name.com></span></span>


<span data-ttu-id="64954-139">Po přijetí e-mailu hello, máte dvě možnosti ověřování:</span><span class="sxs-lookup"><span data-stu-id="64954-139">Upon receiving hello email, you have two verification options:</span></span>

1. <span data-ttu-id="64954-140">Můžete schválit všechny budoucí objednávky zadané prostřednictvím hello stejný účet pro hello stejné kořenové domény, například consoto.com. Toto je doporučený postup, pokud plánujete tooadd další vlastní domény v budoucnu pro hello hello stejné kořenové domény.</span><span class="sxs-lookup"><span data-stu-id="64954-140">You can approve all future orders placed through hello same account for hello same root domain, e.g. consoto.com. This is a recommended approach if you are planning tooadd additional custom domains in hello future for hello same root domain.</span></span>
 
2. <span data-ttu-id="64954-141">Právě můžete schválit hello konkrétní název hostitele použít v této žádosti.</span><span class="sxs-lookup"><span data-stu-id="64954-141">You can just approve hello specific host name used in this request.</span></span> <span data-ttu-id="64954-142">Dodatečné schválení se bude vyžadovat pro následné požadavky.</span><span class="sxs-lookup"><span data-stu-id="64954-142">Additional approval will be required for subsequent requests.</span></span>

    <span data-ttu-id="64954-143">Příklad e-mailu:</span><span class="sxs-lookup"><span data-stu-id="64954-143">Example email:</span></span>
    
    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

<span data-ttu-id="64954-145">Po schválení DigiCert přidá certifikát SAN toohello název vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="64954-145">After approval, DigiCert will add your custom domain name toohello SAN certificate.</span></span> <span data-ttu-id="64954-146">certifikát Hello bude platný jeden rok a bude automaticky obnoveno před jeho platnost vypršela.</span><span class="sxs-lookup"><span data-stu-id="64954-146">hello certificate will be valid for one year and will be auto renewed before it's expired.</span></span>

## <a name="step-3-wait-for-hello-propagation-then-start-using-your-feature"></a><span data-ttu-id="64954-147">Krok 3: Počkejte hello šíření pak spusťte pomocí vaší funkce</span><span class="sxs-lookup"><span data-stu-id="64954-147">Step 3: Wait for hello propagation then start using your feature</span></span>

<span data-ttu-id="64954-148">Po ověření názvu domény hello to bude trvat až too6-8 hodin pro hello vlastní domény HTTPS funkce toobe aktivní.</span><span class="sxs-lookup"><span data-stu-id="64954-148">After hello domain name is validated it will take up too6-8 hours for hello custom domain HTTPS feature toobe active.</span></span> <span data-ttu-id="64954-149">Po dokončení procesu hello stav "vlastní HTTPS" hello v hello portál Azure bude nastaven na příliš "povoleno".</span><span class="sxs-lookup"><span data-stu-id="64954-149">After hello process is complete, hello "custom HTTPS" status in hello Azure portal will be set too"Enabled".</span></span> <span data-ttu-id="64954-150">HTTPS s vaše vlastní doména je nyní připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="64954-150">HTTPS with your custom domain is now ready for your use.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="64954-151">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="64954-151">Frequently asked questions</span></span>

1. <span data-ttu-id="64954-152">*Kdo je poskytovatel certifikátu hello a jaký typ certifikátu se použije?*</span><span class="sxs-lookup"><span data-stu-id="64954-152">*Who is hello certificate provider and what type of certificate is used?*</span></span>

    <span data-ttu-id="64954-153">Používáme alternativní názvy předmětu (SAN) certifikátu od DigiCert.</span><span class="sxs-lookup"><span data-stu-id="64954-153">We use Subject Alternative Names (SAN) certificate provided by DigiCert.</span></span> <span data-ttu-id="64954-154">Certifikát SAN můžete zabezpečit více plně kvalifikované názvy domény s jedním certifikátem.</span><span class="sxs-lookup"><span data-stu-id="64954-154">A SAN certificate can secure multiple fully qualifIed domain names with one certificate.</span></span>

2. <span data-ttu-id="64954-155">*Můžete používat vyhrazené certifikát?*</span><span class="sxs-lookup"><span data-stu-id="64954-155">*Can I use my dedicated certificate?*</span></span>
    
    <span data-ttu-id="64954-156">Aktuálně, ale jeho na hello plán.</span><span class="sxs-lookup"><span data-stu-id="64954-156">Not currently, but it's on hello roadmap.</span></span>

3. <span data-ttu-id="64954-157">*Co když hello domény ověřovací e-mail neobdrželi z DigiCert?*</span><span class="sxs-lookup"><span data-stu-id="64954-157">*What if I don't receive hello domain verification email from DigiCert?*</span></span>

    <span data-ttu-id="64954-158">Pokud jste neobdrželi e-mailu do 24 hodin, obraťte se na společnost Microsoft.</span><span class="sxs-lookup"><span data-stu-id="64954-158">Please contact Microsoft if you don't receive an email within 24 hours.</span></span>

4. <span data-ttu-id="64954-159">*Používá certifikát SAN méně bezpečné než vyhrazené certifikát?*</span><span class="sxs-lookup"><span data-stu-id="64954-159">*Is using a SAN certificate less secure than a dedicated certificate?*</span></span>
    
    <span data-ttu-id="64954-160">Certifikát sítě SAN, že způsobem hello stejné standardy zabezpečení a šifrování jako vyhrazené certifikátu. Všechny vystavené certifikáty SSL používají algoritmus SHA-256 pro server rozšířené zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="64954-160">A SAN cert follows hello same encryption and security standards as a dedicated cert. All issued SSL certificates are using SHA-256 for enhanced server security.</span></span>

5. <span data-ttu-id="64954-161">*Můžete použít vlastní doménu HTTPS s Azure CDN společnosti Akamai?*</span><span class="sxs-lookup"><span data-stu-id="64954-161">*Can I use custom domain HTTPS with Azure CDN from Akamai?*</span></span>

    <span data-ttu-id="64954-162">Tato funkce je v současné době pouze s Azure CDN společnosti Verizon k dispozici.</span><span class="sxs-lookup"><span data-stu-id="64954-162">Currently, this feature is only available with Azure CDN from Verizon.</span></span> <span data-ttu-id="64954-163">Pracujeme na podporu této funkce s Azure CDN společnosti Akamai v nadcházejících měsících hello.</span><span class="sxs-lookup"><span data-stu-id="64954-163">We are working on supporting this feature with Azure CDN from Akamai in hello coming months.</span></span>


## <a name="next-steps"></a><span data-ttu-id="64954-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64954-164">Next steps</span></span>

- <span data-ttu-id="64954-165">Zjistěte, jak tooset nahoru [vlastní doménu na koncový bod Azure CDN](./cdn-map-content-to-custom-domain.md)</span><span class="sxs-lookup"><span data-stu-id="64954-165">Learn how tooset up a [custom domain on your Azure CDN endpoint](./cdn-map-content-to-custom-domain.md)</span></span>


