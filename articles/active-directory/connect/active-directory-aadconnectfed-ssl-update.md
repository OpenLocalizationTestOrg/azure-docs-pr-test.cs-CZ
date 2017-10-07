---
title: "Azure AD Connect: Aktualizace hello certifikát SSL pro farmy služby Active Directory Federation Services (AD FS) | Microsoft Docs"
description: "Tento dokument podrobnosti hello kroky tooupdate hello certifikátu SSL farmu služby AD FS pomocí Azure AD Connect."
services: active-directory
keywords: "služby Azure ad connect, aktualizace ssl služby AD FS, aktualizace certifikátů služby AD FS, změnit certifikát služby AD FS, nový certifikát služby AD FS, certifikát služby AD FS, aktualizace služby AD FS certifikát ssl, aktualizace ssl certifikát služby AD FS, nakonfigurovat certifikát ssl služby AD FS, služba AD FS, ssl, certifikát, certifikát komunikace služby AD FS, federation aktualizace, konfigurace federace, aad connect"
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: anandy
ms.openlocfilehash: bce7f75aab83b6abacb8472a6895054d137e10e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-hello-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a><span data-ttu-id="58bd5-104">Aktualizovat certifikát SSL hello pro farmu služby Active Directory Federation Services (AD FS)</span><span class="sxs-lookup"><span data-stu-id="58bd5-104">Update hello SSL certificate for an Active Directory Federation Services (AD FS) farm</span></span>

## <a name="overview"></a><span data-ttu-id="58bd5-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="58bd5-105">Overview</span></span>
<span data-ttu-id="58bd5-106">Tento článek popisuje, jak můžete použít certifikát SSL hello tooupdate Azure AD Connect pro farmu služby Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="58bd5-106">This article describes how you can use Azure AD Connect tooupdate hello SSL certificate for an Active Directory Federation Services (AD FS) farm.</span></span> <span data-ttu-id="58bd5-107">Můžete certifikát SSL hello tooeasily hello Azure AD Connect nástroje aktualizace pro farmu hello AD FS, i v případě, že není vybraná metoda přihlašovací hello uživatele služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="58bd5-107">You can use hello Azure AD Connect tool tooeasily update hello SSL certificate for hello AD FS farm even if hello user sign-in method selected is not AD FS.</span></span>

<span data-ttu-id="58bd5-108">Můžete provést hello celou operaci aktualizace certifikát SSL pro farmy hello AD FS ve všech federace a servery Proxy webové aplikace (WAP) ve třech jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="58bd5-108">You can perform hello whole operation of updating SSL certificate for hello AD FS farm across all federation and Web Application Proxy (WAP) servers in three simple steps:</span></span>

![Tři kroky](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
><span data-ttu-id="58bd5-110">toolearn Další informace o certifikáty, které používají službu AD FS, najdete v části [Principy certifikátů používaných službou AD FS](https://technet.microsoft.com/library/cc730660.aspx).</span><span class="sxs-lookup"><span data-stu-id="58bd5-110">toolearn more about certificates that are used by AD FS, see [Understanding certificates used by AD FS](https://technet.microsoft.com/library/cc730660.aspx).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58bd5-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="58bd5-111">Prerequisites</span></span>

* <span data-ttu-id="58bd5-112">**Farma služby AD FS**: Ujistěte se, že farmu služby AD FS je založená na Windows Server 2012 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="58bd5-112">**AD FS Farm**: Make sure that your AD FS farm is Windows Server 2012 R2-based or later.</span></span>
* <span data-ttu-id="58bd5-113">**Azure AD Connect**: Ujistěte se, že hello verze služby Azure AD Connect je 1.1.443.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="58bd5-113">**Azure AD Connect**: Ensure that hello version of Azure AD Connect is 1.1.443.0 or later.</span></span> <span data-ttu-id="58bd5-114">Budete používat hello úloh **certifikát SSL služby FS aktualizace AD**.</span><span class="sxs-lookup"><span data-stu-id="58bd5-114">You'll use hello task **Update AD FS SSL certificate**.</span></span>

![Úloha aktualizace SSL](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a><span data-ttu-id="58bd5-116">Krok 1: Zadání informací farmy služby AD FS</span><span class="sxs-lookup"><span data-stu-id="58bd5-116">Step 1: Provide AD FS farm information</span></span>

<span data-ttu-id="58bd5-117">Azure AD Connect automaticky pokusí tooobtain informace o farmu hello AD FS:</span><span class="sxs-lookup"><span data-stu-id="58bd5-117">Azure AD Connect attempts tooobtain information about hello AD FS farm automatically by:</span></span>
1. <span data-ttu-id="58bd5-118">Dotaz na informace farmy hello ze služby AD FS (Windows Server 2016 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="58bd5-118">Querying hello farm information from AD FS (Windows Server 2016 or later).</span></span>
2. <span data-ttu-id="58bd5-119">Odkazování na hello informace z předchozích spuštění, které jsou uloženy místně službou Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="58bd5-119">Referencing hello information from previous runs, which are stored locally with Azure AD Connect.</span></span>

<span data-ttu-id="58bd5-120">Můžete upravit hello seznam serverů, které se zobrazují přidáním nebo odebráním hello servery tooreflect hello aktuální konfiguraci farmy hello AD FS.</span><span class="sxs-lookup"><span data-stu-id="58bd5-120">You can modify hello list of servers that are displayed by adding or removing hello servers tooreflect hello current configuration of hello AD FS farm.</span></span> <span data-ttu-id="58bd5-121">Jakmile je poskytována informace o serveru hello, Azure AD Connect zobrazí hello připojení a aktuální stav certifikátu protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="58bd5-121">As soon as hello server information is provided, Azure AD Connect displays hello connectivity and current SSL certificate status.</span></span>

![Informace o serveru AD FS](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

<span data-ttu-id="58bd5-123">Pokud seznam hello obsahuje server, který je už součástí farmy hello AD FS, klikněte na tlačítko **odebrat** toodelete hello server hello seznamu serverů ve farmě služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="58bd5-123">If hello list contains a server that's no longer part of hello AD FS farm, click **Remove** toodelete hello server from hello list of servers in your AD FS farm.</span></span>

![Offline serveru v seznamu](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> <span data-ttu-id="58bd5-125">Odebrání serveru z hello seznam serverů služby AD FS farmy v Azure AD Connect je místní operace a aktualizace hello informace pro hello farmu služby AD FS, která udržuje Azure AD Connect místně.</span><span class="sxs-lookup"><span data-stu-id="58bd5-125">Removing a server from hello list of servers for an AD FS farm in Azure AD Connect is a local operation and updates hello information for hello AD FS farm that Azure AD Connect maintains locally.</span></span> <span data-ttu-id="58bd5-126">Azure AD Connect nezmění konfigurace hello při změně hello tooreflect služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="58bd5-126">Azure AD Connect doesn't modify hello configuration on AD FS tooreflect hello change.</span></span>    

## <a name="step-2-provide-a-new-ssl-certificate"></a><span data-ttu-id="58bd5-127">Krok 2: Zadejte nový certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="58bd5-127">Step 2: Provide a new SSL certificate</span></span>

<span data-ttu-id="58bd5-128">Když jste potvrdili hello informace o serverech farmy služby AD FS, Azure AD Connect požádá o nový certifikát SSL hello.</span><span class="sxs-lookup"><span data-stu-id="58bd5-128">After you've confirmed hello information about AD FS farm servers, Azure AD Connect asks for hello new SSL certificate.</span></span> <span data-ttu-id="58bd5-129">Zadejte chráněný heslem PFX toocontinue hello instalaci certifikátu.</span><span class="sxs-lookup"><span data-stu-id="58bd5-129">Provide a password-protected PFX certificate toocontinue hello installation.</span></span>

![Certifikát SSL](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

<span data-ttu-id="58bd5-131">Po zadání hello certifikát Azure AD Connect projde řadu požadavky.</span><span class="sxs-lookup"><span data-stu-id="58bd5-131">After you provide hello certificate, Azure AD Connect goes through a series of prerequisites.</span></span> <span data-ttu-id="58bd5-132">Ověřte, zda je správný pro farmu služby AD FS hello tooensure hello certifikát, který hello certifikátu:</span><span class="sxs-lookup"><span data-stu-id="58bd5-132">Verify hello certificate tooensure that hello certificate is correct for hello AD FS farm:</span></span>

-   <span data-ttu-id="58bd5-133">Hello subjektu název nebo alternativní název subjektu certifikátu hello je stejný jako název federační služby hello hello, nebo je certifikát se zástupným znakem.</span><span class="sxs-lookup"><span data-stu-id="58bd5-133">hello subject name/alternate subject name for hello certificate is either hello same as hello federation service name, or it's a wildcard certificate.</span></span>
-   <span data-ttu-id="58bd5-134">Hello certifikát je platný pro více než 30 dní.</span><span class="sxs-lookup"><span data-stu-id="58bd5-134">hello certificate is valid for more than 30 days.</span></span>
-   <span data-ttu-id="58bd5-135">řetěz certifikátů Hello certifikát je platný.</span><span class="sxs-lookup"><span data-stu-id="58bd5-135">hello certificate trust chain is valid.</span></span>
-   <span data-ttu-id="58bd5-136">Hello certifikát je chráněný heslem.</span><span class="sxs-lookup"><span data-stu-id="58bd5-136">hello certificate is password protected.</span></span>

## <a name="step-3-select-servers-for-hello-update"></a><span data-ttu-id="58bd5-137">Krok 3: Vyberte servery pro aktualizaci hello</span><span class="sxs-lookup"><span data-stu-id="58bd5-137">Step 3: Select servers for hello update</span></span>

<span data-ttu-id="58bd5-138">V dalším kroku hello vyberte hello servery, které je třeba certifikát SSL hello toohave aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="58bd5-138">In hello next step, select hello servers that need toohave hello SSL certificate updated.</span></span> <span data-ttu-id="58bd5-139">Servery, které jsou offline nelze vybrat pro aktualizaci hello.</span><span class="sxs-lookup"><span data-stu-id="58bd5-139">Servers that are offline can't be selected for hello update.</span></span>

![Vyberte servery tooupdate](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

<span data-ttu-id="58bd5-141">Po dokončení konfigurace hello Azure AD Connect zobrazí hello zprávu, která označuje stav hello hello aktualizace a poskytuje možnost tooverify hello služby AD FS přihlášení.</span><span class="sxs-lookup"><span data-stu-id="58bd5-141">After you complete hello configuration, Azure AD Connect displays hello message that indicates hello status of hello update and provides an option tooverify hello AD FS sign-in.</span></span>

![Dokončení konfigurace](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a><span data-ttu-id="58bd5-143">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="58bd5-143">FAQs</span></span>

* <span data-ttu-id="58bd5-144">**Co by měla být hello název subjektu certifikátu hello pro nový certifikát SSL služby AD FS hello?**</span><span class="sxs-lookup"><span data-stu-id="58bd5-144">**What should be hello subject name of hello certificate for hello new AD FS SSL certificate?**</span></span>

    <span data-ttu-id="58bd5-145">Azure AD Connect kontroluje, zda subjektu název nebo alternativní název subjektu hello hello certifikátu obsahuje název federační služby hello.</span><span class="sxs-lookup"><span data-stu-id="58bd5-145">Azure AD Connect checks if hello subject name/alternate subject name of hello certificate contains hello federation service name.</span></span> <span data-ttu-id="58bd5-146">Například pokud název vaší služby federačního serveru fs.contoso.com, musí být název subjektu název nebo alternativní subjektu hello fs.contoso.com.  Certifikáty se zástupnými znaky, jsou také přijaty.</span><span class="sxs-lookup"><span data-stu-id="58bd5-146">For example, if your federation service name is fs.contoso.com, hello subject name/alternate subject name must be fs.contoso.com.  Wildcard certificates are also accepted.</span></span>

* <span data-ttu-id="58bd5-147">**Proč se zobrazuje zpráva pro přihlašovací údaje znovu na stránce server hello WAP?**</span><span class="sxs-lookup"><span data-stu-id="58bd5-147">**Why am I asked for credentials again on hello WAP server page?**</span></span>

    <span data-ttu-id="58bd5-148">Pokud hello přihlašovacích údajů, které zadáte pro připojení serverů služby FS tooAD také nemají hello oprávnění toomanage hello WAP servery, pak Azure AD Connect vyzve k zadání přihlašovacích údajů, které mají oprávnění správce na serverech WAP hello.</span><span class="sxs-lookup"><span data-stu-id="58bd5-148">If hello credentials you provide for connecting tooAD FS servers don't also have hello privilege toomanage hello WAP servers, then Azure AD Connect asks for credentials that have administrative privilege on hello WAP servers.</span></span>

* <span data-ttu-id="58bd5-149">**Hello server se zobrazuje v režimu offline. Co bych měl/a dělat?**</span><span class="sxs-lookup"><span data-stu-id="58bd5-149">**hello server is shown as offline. What should I do?**</span></span>

    <span data-ttu-id="58bd5-150">Azure AD Connect nelze provést všechny operace, pokud je hello server offline.</span><span class="sxs-lookup"><span data-stu-id="58bd5-150">Azure AD Connect can't perform any operation if hello server is offline.</span></span> <span data-ttu-id="58bd5-151">Pokud je hello server součástí hello farmu služby AD FS, zkontrolujte hello připojení toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="58bd5-151">If hello server is part of hello AD FS farm, then check hello connectivity toohello server.</span></span> <span data-ttu-id="58bd5-152">Poté, co jste hello problém vyřešili, stiskněte tlačítko hello aktualizace ikonu tooupdate hello stav v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="58bd5-152">After you've resolved hello issue, press hello refresh icon tooupdate hello status in hello wizard.</span></span> <span data-ttu-id="58bd5-153">Pokud hello server byl součástí z hello farmy dříve, ale teď už existuje, klikněte na **odebrat** toodelete hello seznamu serverů, které Azure AD Connect udržuje.</span><span class="sxs-lookup"><span data-stu-id="58bd5-153">If hello server was part of hello farm earlier but now no longer exists, click **Remove** toodelete it from hello list of servers that Azure AD Connect maintains.</span></span> <span data-ttu-id="58bd5-154">Odebrání serveru hello hello seznamu ve službě Azure AD Connect není změnit hello konfigurace služby AD FS, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="58bd5-154">Removing hello server from hello list in Azure AD Connect doesn't alter hello AD FS configuration itself.</span></span> <span data-ttu-id="58bd5-155">Pokud používáte služby AD FS v systému Windows Server 2016 nebo novější, zůstanou server hello v nastavení konfigurace hello a zobrazí se znovu hello příštím hello úloha je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="58bd5-155">If you're using AD FS in Windows Server 2016 or later, hello server remains in hello configuration settings and will be shown again hello next time hello task is run.</span></span>

* <span data-ttu-id="58bd5-156">**Můžete aktualizovat podmnožinu Moje servery ve farmě s hello nový certifikát SSL?**</span><span class="sxs-lookup"><span data-stu-id="58bd5-156">**Can I update a subset of my farm servers with hello new SSL certificate?**</span></span>

    <span data-ttu-id="58bd5-157">Ano.</span><span class="sxs-lookup"><span data-stu-id="58bd5-157">Yes.</span></span> <span data-ttu-id="58bd5-158">Hello úlohu lze spustit vždy **certifikát SSL aktualizace** znovu tooupdate hello zbývající servery.</span><span class="sxs-lookup"><span data-stu-id="58bd5-158">You can always run hello task **Update SSL Certificate** again tooupdate hello remaining servers.</span></span> <span data-ttu-id="58bd5-159">Na hello **vyberte servery pro protokol SSL certifikát aktualizace** stránky, můžete řadit hello seznam serverů na **datum vypršení platnosti SSL** tooeasily přístup hello servery, které ještě nejsou aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="58bd5-159">On hello **Select servers for SSL certificate update** page, you can sort hello list of servers on **SSL Expiry date** tooeasily access hello servers that aren't updated yet.</span></span>

* <span data-ttu-id="58bd5-160">**Po odebrání hello server hello předchozí, spuštění, ale stále se zobrazuje jako offline a uvedené na stránce hello AD FS servery. Proč je hello offline serveru stále existují i po jeho po odebrání?**</span><span class="sxs-lookup"><span data-stu-id="58bd5-160">**I removed hello server in hello previous run, but it's still being shown as offline and listed on hello AD FS Servers page. Why is hello offline server still there even after I removed it?**</span></span>

    <span data-ttu-id="58bd5-161">Odebrání serveru hello hello seznamu ve službě Azure AD Connect neodstraní v hello konfigurace služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="58bd5-161">Removing hello server from hello list in Azure AD Connect doesn't remove it in hello AD FS configuration.</span></span> <span data-ttu-id="58bd5-162">Azure AD Connect odkazuje služby AD FS (Windows Server 2016 nebo vyšší) pro žádné informace o hello farmy.</span><span class="sxs-lookup"><span data-stu-id="58bd5-162">Azure AD Connect references AD FS (Windows Server 2016 or higher) for any information about hello farm.</span></span> <span data-ttu-id="58bd5-163">Pokud hello server se stále nachází na hello konfigurace služby AD FS, objeví se zpět v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="58bd5-163">If hello server is still present in hello AD FS configuration, it will be listed back in hello list.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="58bd5-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="58bd5-164">Next steps</span></span>

- [<span data-ttu-id="58bd5-165">Azure AD Connect a federace</span><span class="sxs-lookup"><span data-stu-id="58bd5-165">Azure AD Connect and federation</span></span>](active-directory-aadconnectfed-whatis.md)
- [<span data-ttu-id="58bd5-166">Přizpůsobení službou Azure AD Connect a správy služby Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="58bd5-166">Active Directory Federation Services management and customization with Azure AD Connect</span></span>](active-directory-aadconnect-federation-management.md)
