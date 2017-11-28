---
title: "Azure Active Directory B2C: Přidáte jako poskytovatele identity SAML, používat vlastní zásady služby AD FS"
description: "Jak tooarticle na nastavení služby AD FS 2016 pomocí protokolu SAML a vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 30fb7700e7834e3d91fab1fc1b169b761584b204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a><span data-ttu-id="4286d-103">Azure Active Directory B2C: Přidáte jako poskytovatele identity SAML, používat vlastní zásady služby AD FS</span><span class="sxs-lookup"><span data-stu-id="4286d-103">Azure Active Directory B2C: Add ADFS as a SAML identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="4286d-104">Tento článek ukazuje, jak tooenable přihlášení pro uživatele z účtu služby AD FS prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="4286d-104">This article shows you how tooenable sign-in for users from ADFS account through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4286d-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4286d-105">Prerequisites</span></span>

<span data-ttu-id="4286d-106">Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4286d-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="4286d-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="4286d-107">These steps include:</span></span>

1.  <span data-ttu-id="4286d-108">Vytvoření AD FS vztah důvěryhodnosti předávající strany.</span><span class="sxs-lookup"><span data-stu-id="4286d-108">Creating an ADFS Relying Party Trust.</span></span>
2.  <span data-ttu-id="4286d-109">Přidání hello služby AD FS vztah důvěryhodnosti předávající strany certifikát tooAzure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="4286d-109">Adding hello ADFS Relying Party Trust certificate tooAzure AD B2C.</span></span>
3.  <span data-ttu-id="4286d-110">Přidání zásad tooa poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="4286d-110">Adding claims provider tooa policy.</span></span>
4.  <span data-ttu-id="4286d-111">Registrace účtu služby AD FS hello deklarací zprostředkovatele tooa uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="4286d-111">Registering hello ADFS account claims provider tooa user journey.</span></span>
5.  <span data-ttu-id="4286d-112">Odesílání hello zásad tooan Azure AD B2C klienta a otestovat.</span><span class="sxs-lookup"><span data-stu-id="4286d-112">Uploading hello policy tooan Azure AD B2C tenant and test it.</span></span>

## <a name="toocreate-a-claims-aware-relying-party-trust"></a><span data-ttu-id="4286d-113">toocreate deklaracemi vztahu důvěryhodnosti předávající strany</span><span class="sxs-lookup"><span data-stu-id="4286d-113">toocreate a claims-aware Relying Party Trust</span></span>

<span data-ttu-id="4286d-114">toouse služby AD FS jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba služby ADFS vztah důvěryhodnosti předávající strany toocreate a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="4286d-114">toouse ADFS as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate an ADFS Relying Party Trust and supply it with hello right parameters.</span></span>

<span data-ttu-id="4286d-115">tooadd nové předávající straně důvěryhodnosti pomocí modulu snap-in Správa služby FS hello AD a ručně nakonfigurujte nastavení hello, proveďte následující postup na federační server hello.</span><span class="sxs-lookup"><span data-stu-id="4286d-115">tooadd a new relying party trust by using hello AD FS Management snap-in and manually configure hello settings, perform hello following procedure on a federation server.</span></span>

<span data-ttu-id="4286d-116">Členství ve skupině **správci**, nebo ekvivalentní na místním počítači hello je minimální požadovaná toocomplete hello tento postup.</span><span class="sxs-lookup"><span data-stu-id="4286d-116">Membership in **Administrators**, or equivalent, on hello local computer is hello minimum required toocomplete this procedure.</span></span> <span data-ttu-id="4286d-117">Podrobnosti o používání hello příslušných účtů a členství ve skupinách v [místní a doménové výchozí skupiny](http://go.microsoft.com/fwlink/?LinkId=83477)</span><span class="sxs-lookup"><span data-stu-id="4286d-117">Review details about using hello appropriate accounts and group memberships at [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)</span></span>

1.  <span data-ttu-id="4286d-118">Ve Správci serveru klikněte na tlačítko **nástroje**a potom vyberte **správu služby AD FS**.</span><span class="sxs-lookup"><span data-stu-id="4286d-118">In Server Manager, click **Tools**, and then select **ADFS Management**.</span></span>

2.  <span data-ttu-id="4286d-119">Klikněte na **přidat vztah důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="4286d-119">Click on **Add Relying Party Trust**.</span></span>
    <span data-ttu-id="4286d-120">![Přidat vztah důvěryhodnosti předávající strany](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-120">![Add Relying Party Trust](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span></span>

3.  <span data-ttu-id="4286d-121">Na hello **úvodní** vyberte **deklarace identity vědět** a klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4286d-121">On hello **Welcome** page, choose **Claims aware** and click **Start**.</span></span>
    <span data-ttu-id="4286d-122">![Na úvodní stránku hello zvolte vědět deklarace identity](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-122">![On hello Welcome page, choose Claims aware](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span></span>
4.  <span data-ttu-id="4286d-123">Na hello **vybrat zdroj dat** klikněte na tlačítko **ručně zadat data o předávající straně hello**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="4286d-123">On hello **Select Data Source** page, click **Enter data about hello relying party manually**, and then click **Next**.</span></span>
    <span data-ttu-id="4286d-124">![Zadat data o předávající straně hello](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-124">![Enter data about hello relying party](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span></span>

5.  <span data-ttu-id="4286d-125">Na hello **zadat zobrazovaný název** stránky, zadejte název do **zobrazovaný název**v části **poznámky** zadejte popis tohoto vztahu důvěryhodnosti předávající strany a pak klikněte na tlačítko **další **.</span><span class="sxs-lookup"><span data-stu-id="4286d-125">On hello **Specify Display Name** page, type a name in **Display name**, under **Notes** type a description for this relying party trust, and then click **Next**.</span></span>
    <span data-ttu-id="4286d-126">![Zadejte zobrazovaný název a poznámky](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-126">![Specify Display Name and notes](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span></span>
6.  <span data-ttu-id="4286d-127">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4286d-127">Optional.</span></span> <span data-ttu-id="4286d-128">Pokud máte volitelné tokenu šifrovací certifikát, pak na hello **konfigurace certifikátu** klikněte na tlačítko **Procházet** toolocate soubor certifikátu a pak klikněte na tlačítko **Další** .</span><span class="sxs-lookup"><span data-stu-id="4286d-128">If you have an optional token encryption certificate, then on hello **Configure Certificate** page, click **Browse** toolocate your certificate file, and then click **Next**.</span></span>
    <span data-ttu-id="4286d-129">![Konfigurace certifikátu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-129">![Configure Certificate](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span></span>
7.  <span data-ttu-id="4286d-130">Na hello **konfigurace adresy URL** stránky, vyberte hello **povolit podporu protokolu SAML 2.0 WebSSO hello** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4286d-130">On hello **Configure URL** page, select hello **Enable support for hello SAML 2.0 WebSSO protocol** check box.</span></span> <span data-ttu-id="4286d-131">V části **URL služby SAML 2.0 SSO předávající strany**hello adresu URL koncového bodu služby Security (Assertion Markup Language SAML) pro tento vztah důvěryhodnosti předávající strany a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="4286d-131">Under **Relying party SAML 2.0 SSO service URL**, type hello Security Assertion Markup Language (SAML) service endpoint URL for this relying party trust, and then click **Next**.</span></span>  <span data-ttu-id="4286d-132">Pro hello **URL služby SAML 2.0 SSO předávající strany**, vložte hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span><span class="sxs-lookup"><span data-stu-id="4286d-132">For hello **Relying party SAML 2.0 SSO service URL**, paste hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span></span> <span data-ttu-id="4286d-133">{Klient} nahraďte názvem vašeho klienta (například contosob2c.onmicrosoft.com) a nahraďte název zásady přípony (například B2C_1A_TrustFrameworkExtensions) hello {zásad}.</span><span class="sxs-lookup"><span data-stu-id="4286d-133">Replace {tenant} with your tenant's name (for example, contosob2c.onmicrosoft.com), and replace hello {policy} with your extensions policy name (for example, B2C_1A_TrustFrameworkExtensions).</span></span>
    > [!IMPORTANT]
    ><span data-ttu-id="4286d-134">Název zásady Hello je hello jeden, který dědí zásady signup_or_signin, v tomto případě je: `B2C_1A_TrustFrameworkExtensions`.</span><span class="sxs-lookup"><span data-stu-id="4286d-134">hello policy name  is hello one that signup_or_signin policy inherits from, in this case it is: `B2C_1A_TrustFrameworkExtensions`.</span></span>
    ><span data-ttu-id="4286d-135">Například může být adresa URL hello: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span><span class="sxs-lookup"><span data-stu-id="4286d-135">For example hello URL could be:   https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span></span>

    ![Předávající strany adresa URL služby SAML 2.0 SSO](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. <span data-ttu-id="4286d-137">Na hello **konfigurovat identifikátory** hello zadejte stejnou adresu URL jako předchozí krok text hello, klikněte na tlačítko **přidat** tooadd je toohello seznamu a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4286d-137">On hello **Configure Identifiers** page, specify hello same URL as hello previous step, click **Add** tooadd them toohello list, and then click **Next**.</span></span>
    <span data-ttu-id="4286d-138">![Předávající strany důvěryhodnosti identifikátory](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-138">![Relying party trust identifiers](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span></span>
9.  <span data-ttu-id="4286d-139">Na hello **zvolte zásad řízení přístupu** vyberte zásadu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4286d-139">On hello **Choose Access Control Policy** select a policy and click **Next**.</span></span>
    <span data-ttu-id="4286d-140">![Zvolte zásad řízení přístupu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-140">![Choose Access Control Policy](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span></span>
10.  <span data-ttu-id="4286d-141">Na hello **připraveni tooAdd důvěryhodnosti** zkontrolujte hello nastavení a pak klikněte na tlačítko **Další** toosave informace o vztahu důvěryhodnosti předávající strany.</span><span class="sxs-lookup"><span data-stu-id="4286d-141">On hello **Ready tooAdd Trust** page, review hello settings, and then click **Next** toosave your relying party trust information.</span></span>
    <span data-ttu-id="4286d-142">![Uložit informace o vztazích důvěryhodnosti předávající strany](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-142">![Save your relying party trust information](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span></span>
11.  <span data-ttu-id="4286d-143">Na hello **Dokončit** klikněte na tlačítko **Zavřít**, tato akce se automaticky zobrazí hello **upravit pravidla deklarací identity** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4286d-143">On hello **Finish** page, click **Close**, this action automatically displays hello **Edit Claim Rules** dialog box.</span></span>
    <span data-ttu-id="4286d-144">![Upravit pravidla deklarace identity](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-144">![Edit Claim Rules](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span></span>
12. <span data-ttu-id="4286d-145">Klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="4286d-145">Click **Add Rule**.</span></span>  
      <span data-ttu-id="4286d-146">![Přidat nové pravidlo](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-146">![Add new rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span></span>
13.  <span data-ttu-id="4286d-147">V **šablony pravidla deklarace identity**, vyberte **odesílat atributy LDAP jako deklarace identity**.</span><span class="sxs-lookup"><span data-stu-id="4286d-147">In **Claim rule template**, select **Send LDAP attributes as claims**.</span></span>
    <span data-ttu-id="4286d-148">![Vyberte možnost odesílat atributy LDAP jako deklarace šablony](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-148">![Select Send LDAP attributes as claims template rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span></span>
14.  <span data-ttu-id="4286d-149">Zadejte **název pravidla deklarací**.</span><span class="sxs-lookup"><span data-stu-id="4286d-149">Provide **Claim rule name**.</span></span> <span data-ttu-id="4286d-150">Pro hello **úložiště atributů** vyberte **služby Active Directory vyberte** přidejte následující deklarace identity hello a pak klikněte na tlačítko **Dokončit** a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4286d-150">For hello **Attribute store** select **Select Active Directory** Add hello following claims, then click **Finish** and **OK**.</span></span>
    <span data-ttu-id="4286d-151">![Umožňuje nastavit vlastnosti pravidla](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-151">![Set rule properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span></span>
15.  <span data-ttu-id="4286d-152">Ve Správci serveru vyberte **vztahy důvěryhodnosti předávající strany** poté vyberte hello vztahu důvěryhodnosti předávající strany jste vytvořili a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4286d-152">In Server Manager, select **Relying Party Trusts** then select hello relying party trust you created and click **Properties**.</span></span>
    <span data-ttu-id="4286d-153">![Předávající strany upravit vlastnosti](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-153">![Relying party edit properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span></span>
16.  <span data-ttu-id="4286d-154">Jeden hello předávající strany vztahu důvěryhodnosti (B2C ukázku) vlastnosti klikněte na **podpis** a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4286d-154">One hello relying party trust (B2C Demo) properties window click **Signature** tab and click **Add**.</span></span>  
    <span data-ttu-id="4286d-155">![Sada podpis](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-155">![Set signature](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span></span>
17.  <span data-ttu-id="4286d-156">Přidání vaší podpis certifikátu (bez soukromého klíče soubor .cert).</span><span class="sxs-lookup"><span data-stu-id="4286d-156">Add your signature certificate (.cert file, without private key).</span></span>  
    ![Přidejte svůj certifikát podpisu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  <span data-ttu-id="4286d-158">V okně Vlastnosti vztahu důvěryhodnosti (B2C ukázku) předávající strany hello klikněte na tlačítko **Upřesnit** kartě a změňte hello **zabezpečený algoritmus hash** příliš**SHA-1**, klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="4286d-158">On hello relying party trust (B2C Demo) properties window click **Advanced** tab and change hello **Secure hash algorithm** too**SHA-1**, Click **Ok**.</span></span>  
    <span data-ttu-id="4286d-159">![Algoritmus hash zabezpečené sady tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-159">![Set secure hash algorithm tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span></span>

## <a name="add-hello-adfs-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="4286d-160">Přidat hello služby AD FS účet aplikace klíče tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="4286d-160">Add hello ADFS account application key tooAzure AD B2C</span></span>
<span data-ttu-id="4286d-161">Federace s účty služby AD FS vyžaduje tajný klíč klienta služby AD FS tootrust účet Azure AD B2C jménem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4286d-161">Federation with ADFS accounts requires a client secret for ADFS account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="4286d-162">Budete potřebovat toostore vaší služby AD FS certifikát v klientovi služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="4286d-162">You need toostore your ADFS certificate in your Azure AD B2C tenant.</span></span> 

1.  <span data-ttu-id="4286d-163">Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework**</span><span class="sxs-lookup"><span data-stu-id="4286d-163">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="4286d-164">Vyberte **zásad klíče** tooview hello klíče k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="4286d-164">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="4286d-165">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="4286d-165">Click **+Add**.</span></span>
4.  <span data-ttu-id="4286d-166">Pro **možnosti**, použijte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="4286d-166">For **Options**, use **Upload**.</span></span>
5.  <span data-ttu-id="4286d-167">Pro **název**, použijte `ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="4286d-167">For **Name**, use `ADFSSamlCert`.</span></span>  
    <span data-ttu-id="4286d-168">Předpona Hello `B2C_1A_` může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="4286d-168">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="4286d-169">V hello nahrávání souborů ** vyberte soubor .pfx certifikátu s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="4286d-169">In hello File upload,** select your certificate .pfx file with private key.</span></span> <span data-ttu-id="4286d-170">Poznámka: Tento certifikát (s privátním klíčem hello) by měl být hello stejné, který vydala a použít hello služby AD FS, předávající strany.</span><span class="sxs-lookup"><span data-stu-id="4286d-170">Note: this certificate (with hello private key) should be hello same one that issued and used for hello ADFS relying party.</span></span>
<span data-ttu-id="4286d-171">![Odešlete klíč zásad](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span><span class="sxs-lookup"><span data-stu-id="4286d-171">![Upload policy key](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span></span>
7.  <span data-ttu-id="4286d-172">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="4286d-172">Click **Create**</span></span>
8.  <span data-ttu-id="4286d-173">Potvrďte, že jste vytvořili klíč hello `B2C_1A_ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="4286d-173">Confirm that you've created hello key `B2C_1A_ADFSSamlCert`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="4286d-174">Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="4286d-174">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="4286d-175">Pokud chcete uživatelům toosign pomocí účtu služby AD FS, potřebujete účet služby AD FS toodefine jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="4286d-175">If you want users toosign in by using ADFS account, you need toodefine ADFS account as a claims provider.</span></span> <span data-ttu-id="4286d-176">Jinými slovy budete potřebovat toospecify koncový bod, který komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="4286d-176">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="4286d-177">koncový bod Hello obsahuje sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4286d-177">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="4286d-178">Definování služby AD FS jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="4286d-178">Define ADFS as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1. <span data-ttu-id="4286d-179">Otevřete soubor hello rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="4286d-179">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="4286d-180">Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="4286d-180">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2. <span data-ttu-id="4286d-181">Najde hello `<ClaimsProviders>` části</span><span class="sxs-lookup"><span data-stu-id="4286d-181">Find hello `<ClaimsProviders>` section</span></span>
3. <span data-ttu-id="4286d-182">Přidejte následující fragment kódu XML v části hello hello `ClaimsProviders` elementu a nahraďte `identityProvider` s DNS (libovolná hodnota, kterou určuje doménu) a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="4286d-182">Add hello following XML snippet under hello `ClaimsProviders` element and replace `identityProvider` with your DNS (Arbitrary value that indicates your domain), and save hello file.</span></span> 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-adfs-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="4286d-183">Zaregistrovat hello služby AD FS účet deklarace identity zprostředkovatele tooSign nahoru nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="4286d-183">Register hello ADFS account claims provider tooSign up or Sign in user journey</span></span>
<span data-ttu-id="4286d-184">V tomto okamžiku zprostředkovatele identity hello má nastaven.</span><span class="sxs-lookup"><span data-stu-id="4286d-184">At this point, hello identity provider has been set up.</span></span>  <span data-ttu-id="4286d-185">Však není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky.</span><span class="sxs-lookup"><span data-stu-id="4286d-185">However, it is not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="4286d-186">Nyní je třeba tooyour uživatele zprostředkovatele účtu identity tooadd hello služby AD FS `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="4286d-186">Now you need tooadd hello ADFS account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="4286d-187">toomake je k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="4286d-187">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="4286d-188">Potom jsme upravit ji tak, aby zahrnovala hello zprostředkovatele identity služby AD FS:</span><span class="sxs-lookup"><span data-stu-id="4286d-188">Then, we modify it so it includes hello ADFS identity provider:</span></span>
    >[!NOTE]
    >If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  <span data-ttu-id="4286d-189">Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="4286d-189">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="4286d-190">Najde hello `<UserJourneys>` elementu a zkopírujte hello celý obsah `<UserJourneys>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="4286d-190">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="4286d-191">Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4286d-191">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="4286d-192">Pokud hello element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="4286d-192">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="4286d-193">Vložte hello celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4286d-193">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="4286d-194">Tlačítko hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="4286d-194">Display hello button</span></span>
<span data-ttu-id="4286d-195">Hello `<ClaimsProviderSelections>` element definuje hello seznam možností výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="4286d-195">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="4286d-196">`<ClaimsProviderSelection>`Element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4286d-196">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="4286d-197">Pokud přidáte `<ClaimsProviderSelection>` element pro účet služby AD FS, nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello.</span><span class="sxs-lookup"><span data-stu-id="4286d-197">If you add a `<ClaimsProviderSelection>` element for  ADFS account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="4286d-198">tooadd tento element:</span><span class="sxs-lookup"><span data-stu-id="4286d-198">tooadd this element:</span></span>

1.  <span data-ttu-id="4286d-199">Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty hello uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="4286d-199">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="4286d-200">Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="4286d-200">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="4286d-201">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="4286d-201">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="4286d-202">Akce tooan tlačítka hello odkaz</span><span class="sxs-lookup"><span data-stu-id="4286d-202">Link hello button tooan action</span></span>

<span data-ttu-id="4286d-203">Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce.</span><span class="sxs-lookup"><span data-stu-id="4286d-203">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="4286d-204">pro Azure AD B2C toocommunicate s tooreceive účet služby AD FS Hello akce v tomto případě je token.</span><span class="sxs-lookup"><span data-stu-id="4286d-204">hello action, in this case, is for Azure AD B2C toocommunicate with ADFS account tooreceive a token.</span></span> <span data-ttu-id="4286d-205">Odkaz hello tlačítko tooan akce pomocí propojení hello technické profil pro poskytovatele deklarací identity účtu služby AD FS:</span><span class="sxs-lookup"><span data-stu-id="4286d-205">Link hello button tooan action by linking hello technical profile for your ADFS account claims provider:</span></span>

1.  <span data-ttu-id="4286d-206">Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="4286d-206">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="4286d-207">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="4286d-207">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * <span data-ttu-id="4286d-208">Ujistěte se, hello `Id` má stejnou hodnotu jako hello `TargetClaimsExchangeId` v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="4286d-208">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section.</span></span>
> * <span data-ttu-id="4286d-209">Ujistěte se, `TechnicalProfileReferenceId` nastavena toohello technické profil vytvoříte starší (Contoso-typu SAML2).</span><span class="sxs-lookup"><span data-stu-id="4286d-209">Ensure `TechnicalProfileReferenceId` is set toohello technical profile you created earlier (Contoso-SAML2).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="4286d-210">Nahrát hello zásad tooyour klienta</span><span class="sxs-lookup"><span data-stu-id="4286d-210">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="4286d-211">V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete hello **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="4286d-211">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="4286d-212">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="4286d-212">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="4286d-213">Otevřete hello **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="4286d-213">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="4286d-214">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="4286d-214">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="4286d-215">Zkontrolujte **přepsat zásady hello, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="4286d-215">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="4286d-216">**Nahrát** TrustFrameworkExtensions.xml a ujistěte se, zda text hello ověření neselže</span><span class="sxs-lookup"><span data-stu-id="4286d-216">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="4286d-217">Otestovat vlastní zásady hello pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="4286d-217">Test hello custom policy by using Run Now</span></span>
1.  <span data-ttu-id="4286d-218">Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="4286d-218">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="4286d-219">Otevřete **B2C_1A_signup_signin**, hello předávající vlastní zásady stranu, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="4286d-219">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="4286d-220">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="4286d-220">Select **Run now**.</span></span>
3.  <span data-ttu-id="4286d-221">Musí být schopný toosign pomocí účtu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="4286d-221">You should be able toosign in using ADFS account.</span></span>

## <a name="optional-register-hello-adfs-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="4286d-222">[Nepovinné] Zaregistrovat hello služby AD FS účet deklarace identity zprostředkovatele tooProfile upravit uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="4286d-222">[Optional] Register hello ADFS account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="4286d-223">Může být vhodné zprostředkovatele identity účtu služby AD FS hello tooadd také tooyour uživatele `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="4286d-223">You may want tooadd hello ADFS account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="4286d-224">toomake je k dispozici, jsme opakujte hello poslední dva kroky:</span><span class="sxs-lookup"><span data-stu-id="4286d-224">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="4286d-225">Tlačítko hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="4286d-225">Display hello button</span></span>
1.  <span data-ttu-id="4286d-226">Otevřete soubor hello rozšíření zásad (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="4286d-226">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="4286d-227">Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty hello uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="4286d-227">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="4286d-228">Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="4286d-228">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="4286d-229">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="4286d-229">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="4286d-230">Akce tooan tlačítka hello odkaz</span><span class="sxs-lookup"><span data-stu-id="4286d-230">Link hello button tooan action</span></span>
1.  <span data-ttu-id="4286d-231">Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="4286d-231">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="4286d-232">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="4286d-232">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="4286d-233">Otestovat vlastní zásady pro úpravy profilu hello pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="4286d-233">Test hello custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="4286d-234">Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="4286d-234">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="4286d-235">Otevřete **B2C_1A_ProfileEdit**, hello předávající vlastní zásady stranu, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="4286d-235">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="4286d-236">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="4286d-236">Select **Run now**.</span></span>
3.  <span data-ttu-id="4286d-237">Musí být schopný toosign pomocí účtu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="4286d-237">You should be able toosign in using ADFS account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="4286d-238">Stáhnout soubory dokončení zásad hello</span><span class="sxs-lookup"><span data-stu-id="4286d-238">Download hello complete policy files</span></span>
<span data-ttu-id="4286d-239">Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní soubory zásad vlastní po dokončení hello Začínáme se zásadami vlastní provede.</span><span class="sxs-lookup"><span data-stu-id="4286d-239">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through.</span></span> [<span data-ttu-id="4286d-240">Ukázkové soubory zásad jenom jako reference</span><span class="sxs-lookup"><span data-stu-id="4286d-240">Policy sample files for reference only</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
