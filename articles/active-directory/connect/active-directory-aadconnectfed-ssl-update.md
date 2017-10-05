---
title: "Azure AD Connect: Aktualizovat certifikát SSL pro farmy služby Active Directory Federation Services (AD FS) | Microsoft Docs"
description: "Tato dokument podrobně popisuje postup aktualizace certifikátu SSL farmu služby AD FS pomocí Azure AD Connect."
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
ms.openlocfilehash: 87807a203d71b3abfe3e93132eb7d0b82b14b4ee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="update-the-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a><span data-ttu-id="01a1b-104">Aktualizovat certifikát SSL pro farmy služby Active Directory Federation Services (AD FS)</span><span class="sxs-lookup"><span data-stu-id="01a1b-104">Update the SSL certificate for an Active Directory Federation Services (AD FS) farm</span></span>

## <a name="overview"></a><span data-ttu-id="01a1b-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="01a1b-105">Overview</span></span>
<span data-ttu-id="01a1b-106">Tento článek popisuje, jak používat Azure AD Connect aktualizovat certifikát SSL pro farmy služby Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="01a1b-106">This article describes how you can use Azure AD Connect to update the SSL certificate for an Active Directory Federation Services (AD FS) farm.</span></span> <span data-ttu-id="01a1b-107">Nástroj Azure AD Connect můžete snadno aktualizovat certifikát SSL pro farmy služby AD FS, i když uživatel přihlásit metoda vybrané není služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="01a1b-107">You can use the Azure AD Connect tool to easily update the SSL certificate for the AD FS farm even if the user sign-in method selected is not AD FS.</span></span>

<span data-ttu-id="01a1b-108">Můžete provést celou operaci aktualizace certifikát SSL pro farmy služby AD FS ve všech federace a servery Proxy webové aplikace (WAP) ve třech jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="01a1b-108">You can perform the whole operation of updating SSL certificate for the AD FS farm across all federation and Web Application Proxy (WAP) servers in three simple steps:</span></span>

![Tři kroky](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
><span data-ttu-id="01a1b-110">Další informace o certifikátech, které používají služby AD FS najdete v tématu [Principy certifikátů používaných službou AD FS](https://technet.microsoft.com/library/cc730660.aspx).</span><span class="sxs-lookup"><span data-stu-id="01a1b-110">To learn more about certificates that are used by AD FS, see [Understanding certificates used by AD FS](https://technet.microsoft.com/library/cc730660.aspx).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01a1b-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01a1b-111">Prerequisites</span></span>

* <span data-ttu-id="01a1b-112">**Farma služby AD FS**: Ujistěte se, že farmu služby AD FS je založená na Windows Server 2012 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="01a1b-112">**AD FS Farm**: Make sure that your AD FS farm is Windows Server 2012 R2-based or later.</span></span>
* <span data-ttu-id="01a1b-113">**Azure AD Connect**: Zkontrolujte, zda je verze služby Azure AD Connect 1.1.443.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="01a1b-113">**Azure AD Connect**: Ensure that the version of Azure AD Connect is 1.1.443.0 or later.</span></span> <span data-ttu-id="01a1b-114">Použijete-li úlohu **certifikát SSL služby FS aktualizace AD**.</span><span class="sxs-lookup"><span data-stu-id="01a1b-114">You'll use the task **Update AD FS SSL certificate**.</span></span>

![Úloha aktualizace SSL](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a><span data-ttu-id="01a1b-116">Krok 1: Zadání informací farmy služby AD FS</span><span class="sxs-lookup"><span data-stu-id="01a1b-116">Step 1: Provide AD FS farm information</span></span>

<span data-ttu-id="01a1b-117">Azure AD Connect se pokusí získat informace o farmu služby AD FS automaticky:</span><span class="sxs-lookup"><span data-stu-id="01a1b-117">Azure AD Connect attempts to obtain information about the AD FS farm automatically by:</span></span>
1. <span data-ttu-id="01a1b-118">Dotaz na informace farmy služby AD FS (Windows Server 2016 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="01a1b-118">Querying the farm information from AD FS (Windows Server 2016 or later).</span></span>
2. <span data-ttu-id="01a1b-119">Odkazy na informace z předchozích spuštění, které jsou uložené místně službou Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="01a1b-119">Referencing the information from previous runs, which are stored locally with Azure AD Connect.</span></span>

<span data-ttu-id="01a1b-120">Můžete upravit seznam serverů, které se zobrazují přidáním nebo odebráním servery tak, aby odrážela aktuální konfiguraci farmy služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="01a1b-120">You can modify the list of servers that are displayed by adding or removing the servers to reflect the current configuration of the AD FS farm.</span></span> <span data-ttu-id="01a1b-121">Jakmile se poskytuje informace o serveru, Azure AD Connect zobrazí připojení a aktuální stav certifikátu protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="01a1b-121">As soon as the server information is provided, Azure AD Connect displays the connectivity and current SSL certificate status.</span></span>

![Informace o serveru AD FS](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

<span data-ttu-id="01a1b-123">Pokud seznam obsahuje server, který je už součástí farmy služby AD FS, klikněte na tlačítko **odebrat** se odstranit server ze seznamu serverů ve farmě služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="01a1b-123">If the list contains a server that's no longer part of the AD FS farm, click **Remove** to delete the server from the list of servers in your AD FS farm.</span></span>

![Offline serveru v seznamu](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> <span data-ttu-id="01a1b-125">Odebrání serveru ze seznamu serverů pro farmu služby AD FS ve službě Azure AD Connect je místní operace a aktualizuje informace pro farmu služby AD FS, která udržuje Azure AD Connect místně.</span><span class="sxs-lookup"><span data-stu-id="01a1b-125">Removing a server from the list of servers for an AD FS farm in Azure AD Connect is a local operation and updates the information for the AD FS farm that Azure AD Connect maintains locally.</span></span> <span data-ttu-id="01a1b-126">Azure AD Connect nezmění konfigurace ve službě AD FS, aby odrážely změny.</span><span class="sxs-lookup"><span data-stu-id="01a1b-126">Azure AD Connect doesn't modify the configuration on AD FS to reflect the change.</span></span>    

## <a name="step-2-provide-a-new-ssl-certificate"></a><span data-ttu-id="01a1b-127">Krok 2: Zadejte nový certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="01a1b-127">Step 2: Provide a new SSL certificate</span></span>

<span data-ttu-id="01a1b-128">Když jste potvrdili informace o servery ve farmě služby AD FS, Azure AD Connect požádá o nový certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="01a1b-128">After you've confirmed the information about AD FS farm servers, Azure AD Connect asks for the new SSL certificate.</span></span> <span data-ttu-id="01a1b-129">Zadejte certifikát PFX chráněný heslem pokračujte v instalaci.</span><span class="sxs-lookup"><span data-stu-id="01a1b-129">Provide a password-protected PFX certificate to continue the installation.</span></span>

![Certifikát SSL](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

<span data-ttu-id="01a1b-131">Po zadání certifikát Azure AD Connect projde řadu požadavky.</span><span class="sxs-lookup"><span data-stu-id="01a1b-131">After you provide the certificate, Azure AD Connect goes through a series of prerequisites.</span></span> <span data-ttu-id="01a1b-132">Ověření certifikátu k zajištění, že certifikát je správný pro farmu služby AD FS:</span><span class="sxs-lookup"><span data-stu-id="01a1b-132">Verify the certificate to ensure that the certificate is correct for the AD FS farm:</span></span>

-   <span data-ttu-id="01a1b-133">Předmět názvu nebo alternativní název subjektu certifikátu je buď stejný jako název federační služby, nebo je certifikát se zástupným znakem.</span><span class="sxs-lookup"><span data-stu-id="01a1b-133">The subject name/alternate subject name for the certificate is either the same as the federation service name, or it's a wildcard certificate.</span></span>
-   <span data-ttu-id="01a1b-134">Certifikát je platný pro více než 30 dní.</span><span class="sxs-lookup"><span data-stu-id="01a1b-134">The certificate is valid for more than 30 days.</span></span>
-   <span data-ttu-id="01a1b-135">Řetězu certifikátů je platný.</span><span class="sxs-lookup"><span data-stu-id="01a1b-135">The certificate trust chain is valid.</span></span>
-   <span data-ttu-id="01a1b-136">Certifikát je chráněný heslem.</span><span class="sxs-lookup"><span data-stu-id="01a1b-136">The certificate is password protected.</span></span>

## <a name="step-3-select-servers-for-the-update"></a><span data-ttu-id="01a1b-137">Krok 3: Vyberte servery pro aktualizaci</span><span class="sxs-lookup"><span data-stu-id="01a1b-137">Step 3: Select servers for the update</span></span>

<span data-ttu-id="01a1b-138">V dalším kroku vyberte servery, které je potřeba aktualizovat certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="01a1b-138">In the next step, select the servers that need to have the SSL certificate updated.</span></span> <span data-ttu-id="01a1b-139">Servery, které jsou offline nelze vybrat pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="01a1b-139">Servers that are offline can't be selected for the update.</span></span>

![Vyberte servery a aktualizace](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

<span data-ttu-id="01a1b-141">Po dokončení konfigurace Azure AD Connect zobrazí zprávu, která označuje stav aktualizace a nabízí možnost ověření přihlášení služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="01a1b-141">After you complete the configuration, Azure AD Connect displays the message that indicates the status of the update and provides an option to verify the AD FS sign-in.</span></span>

![Dokončení konfigurace](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a><span data-ttu-id="01a1b-143">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="01a1b-143">FAQs</span></span>

* <span data-ttu-id="01a1b-144">**Název subjektu certifikátu pro nový certifikát SSL služby AD FS, co by měla být?**</span><span class="sxs-lookup"><span data-stu-id="01a1b-144">**What should be the subject name of the certificate for the new AD FS SSL certificate?**</span></span>

    <span data-ttu-id="01a1b-145">Azure AD Connect kontroluje, zda subjektu název nebo alternativní název subjektu certifikátu obsahuje název federační služby.</span><span class="sxs-lookup"><span data-stu-id="01a1b-145">Azure AD Connect checks if the subject name/alternate subject name of the certificate contains the federation service name.</span></span> <span data-ttu-id="01a1b-146">Například pokud je název vaší služby federačního serveru fs.contoso.com, předmět názvu nebo alternativní název subjektu musí být fs.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="01a1b-146">For example, if your federation service name is fs.contoso.com, the subject name/alternate subject name must be fs.contoso.com.</span></span>  <span data-ttu-id="01a1b-147">Certifikáty se zástupnými znaky, jsou také přijaty.</span><span class="sxs-lookup"><span data-stu-id="01a1b-147">Wildcard certificates are also accepted.</span></span>

* <span data-ttu-id="01a1b-148">**Proč se zobrazuje zpráva pro přihlašovací údaje znovu na stránce server WAP?**</span><span class="sxs-lookup"><span data-stu-id="01a1b-148">**Why am I asked for credentials again on the WAP server page?**</span></span>

    <span data-ttu-id="01a1b-149">Pokud přihlašovacích údajů, které zadáte pro připojení k serverům služby AD FS taky nemáte oprávnění ke správě serverů WAP, pak Azure AD Connect vyzve k zadání přihlašovacích údajů, které mají oprávnění správce na serverech WAP.</span><span class="sxs-lookup"><span data-stu-id="01a1b-149">If the credentials you provide for connecting to AD FS servers don't also have the privilege to manage the WAP servers, then Azure AD Connect asks for credentials that have administrative privilege on the WAP servers.</span></span>

* <span data-ttu-id="01a1b-150">**Serveru se zobrazuje v režimu offline. Co bych měl/a dělat?**</span><span class="sxs-lookup"><span data-stu-id="01a1b-150">**The server is shown as offline. What should I do?**</span></span>

    <span data-ttu-id="01a1b-151">Azure AD Connect nelze provádět všechny operace, pokud je server offline.</span><span class="sxs-lookup"><span data-stu-id="01a1b-151">Azure AD Connect can't perform any operation if the server is offline.</span></span> <span data-ttu-id="01a1b-152">Pokud je server součástí farmy služby AD FS, zkontrolujte připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="01a1b-152">If the server is part of the AD FS farm, then check the connectivity to the server.</span></span> <span data-ttu-id="01a1b-153">Poté, co jste vyřešit problém, stiskněte ikonu aktualizace se bude aktualizovat stav v průvodci.</span><span class="sxs-lookup"><span data-stu-id="01a1b-153">After you've resolved the issue, press the refresh icon to update the status in the wizard.</span></span> <span data-ttu-id="01a1b-154">Pokud byl server součástí farmy dříve, ale teď už existuje, klikněte na tlačítko **odebrat** odstranit ze seznamu serverů, že Azure AD Connect udržuje.</span><span class="sxs-lookup"><span data-stu-id="01a1b-154">If the server was part of the farm earlier but now no longer exists, click **Remove** to delete it from the list of servers that Azure AD Connect maintains.</span></span> <span data-ttu-id="01a1b-155">Odebrání serveru ze seznamu ve službě Azure AD Connect není změnit konfiguraci služby AD FS, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="01a1b-155">Removing the server from the list in Azure AD Connect doesn't alter the AD FS configuration itself.</span></span> <span data-ttu-id="01a1b-156">Pokud používáte služby AD FS v systému Windows Server 2016 nebo novější, zůstanou serveru v nastavení konfigurace a se zobrazí při příštím spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="01a1b-156">If you're using AD FS in Windows Server 2016 or later, the server remains in the configuration settings and will be shown again the next time the task is run.</span></span>

* <span data-ttu-id="01a1b-157">**Můžete aktualizovat podmnožinu Moje servery ve farmě s novým certifikátem SSL?**</span><span class="sxs-lookup"><span data-stu-id="01a1b-157">**Can I update a subset of my farm servers with the new SSL certificate?**</span></span>

    <span data-ttu-id="01a1b-158">Ano.</span><span class="sxs-lookup"><span data-stu-id="01a1b-158">Yes.</span></span> <span data-ttu-id="01a1b-159">Možné vždy spouštět úlohy **aktualizovat certifikát SSL** aktualizovat na ostatní servery.</span><span class="sxs-lookup"><span data-stu-id="01a1b-159">You can always run the task **Update SSL Certificate** again to update the remaining servers.</span></span> <span data-ttu-id="01a1b-160">Na **vyberte servery pro protokol SSL certifikát aktualizace** stránky, lze seřadit seznam serverů na **datum vypršení platnosti SSL** snadný přístup k serveru, které ještě nejsou aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="01a1b-160">On the **Select servers for SSL certificate update** page, you can sort the list of servers on **SSL Expiry date** to easily access the servers that aren't updated yet.</span></span>

* <span data-ttu-id="01a1b-161">**Po odebrání serveru při předchozím spuštění, ale stále se zobrazuje jako offline a uvedené na stránce servery služby AD FS. Proč je offline serveru stále existují i po jeho po odebrání?**</span><span class="sxs-lookup"><span data-stu-id="01a1b-161">**I removed the server in the previous run, but it's still being shown as offline and listed on the AD FS Servers page. Why is the offline server still there even after I removed it?**</span></span>

    <span data-ttu-id="01a1b-162">Odebrání serveru ze seznamu ve službě Azure AD Connect, neodstraní se v konfiguraci služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="01a1b-162">Removing the server from the list in Azure AD Connect doesn't remove it in the AD FS configuration.</span></span> <span data-ttu-id="01a1b-163">Azure AD Connect odkazuje služby AD FS (Windows Server 2016 nebo vyšší) pro žádné informace o farmy.</span><span class="sxs-lookup"><span data-stu-id="01a1b-163">Azure AD Connect references AD FS (Windows Server 2016 or higher) for any information about the farm.</span></span> <span data-ttu-id="01a1b-164">Pokud je server stále existuje v konfiguraci služby AD FS, objeví se zpět v seznamu.</span><span class="sxs-lookup"><span data-stu-id="01a1b-164">If the server is still present in the AD FS configuration, it will be listed back in the list.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="01a1b-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01a1b-165">Next steps</span></span>

- [<span data-ttu-id="01a1b-166">Azure AD Connect a federace</span><span class="sxs-lookup"><span data-stu-id="01a1b-166">Azure AD Connect and federation</span></span>](active-directory-aadconnectfed-whatis.md)
- [<span data-ttu-id="01a1b-167">Přizpůsobení službou Azure AD Connect a správy služby Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="01a1b-167">Active Directory Federation Services management and customization with Azure AD Connect</span></span>](active-directory-aadconnect-federation-management.md)
