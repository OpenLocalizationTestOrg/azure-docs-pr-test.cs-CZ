---
title: "Azure Active Directory B2C: Přidáte jako poskytovatele identity SAML, používat vlastní zásady služby AD FS"
description: "Postupy: článek o nastavení služby AD FS 2016 pomocí protokolu SAML a vlastní zásady"
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
ms.openlocfilehash: ef0495460b5652dd6052a49ab9c722381e93458b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a><span data-ttu-id="d25dd-103">Azure Active Directory B2C: Přidáte jako poskytovatele identity SAML, používat vlastní zásady služby AD FS</span><span class="sxs-lookup"><span data-stu-id="d25dd-103">Azure Active Directory B2C: Add ADFS as a SAML identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="d25dd-104">Tento článek ukazuje, jak povolit přihlášení pro uživatele z účtu služby AD FS prostřednictvím [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d25dd-104">This article shows you how to enable sign-in for users from ADFS account through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d25dd-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d25dd-105">Prerequisites</span></span>

<span data-ttu-id="d25dd-106">Proveďte kroky v [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d25dd-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="d25dd-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="d25dd-107">These steps include:</span></span>

1.  <span data-ttu-id="d25dd-108">Vytvoření AD FS vztah důvěryhodnosti předávající strany.</span><span class="sxs-lookup"><span data-stu-id="d25dd-108">Creating an ADFS Relying Party Trust.</span></span>
2.  <span data-ttu-id="d25dd-109">Certifikát služby AD FS vztah důvěryhodnosti předávající strany se přidává do Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d25dd-109">Adding the ADFS Relying Party Trust certificate to Azure AD B2C.</span></span>
3.  <span data-ttu-id="d25dd-110">Přidání poskytovatele deklarací identity k zásadě.</span><span class="sxs-lookup"><span data-stu-id="d25dd-110">Adding claims provider to a policy.</span></span>
4.  <span data-ttu-id="d25dd-111">Registrace služby AD FS účet deklarací zprostředkovatele, který se cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="d25dd-111">Registering the ADFS account claims provider to a user journey.</span></span>
5.  <span data-ttu-id="d25dd-112">Odeslání zásady do Azure AD B2C klienta a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="d25dd-112">Uploading the policy to an Azure AD B2C tenant and test it.</span></span>

## <a name="to-create-a-claims-aware-relying-party-trust"></a><span data-ttu-id="d25dd-113">Chcete-li vytvořit deklaracemi identity vztah důvěryhodnosti předávající strany</span><span class="sxs-lookup"><span data-stu-id="d25dd-113">To create a claims-aware Relying Party Trust</span></span>

<span data-ttu-id="d25dd-114">Použití služby AD FS jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte k vytvoření AD FS vztah důvěryhodnosti předávající strany a poskytne mu se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="d25dd-114">To use ADFS as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create an ADFS Relying Party Trust and supply it with the right parameters.</span></span>

<span data-ttu-id="d25dd-115">Chcete-li přidat nový vztah důvěryhodnosti předávající strany pomocí modulu snap-in Správa služby AD FS a ručně nakonfigurovat nastavení, proveďte následující postup na federačním serveru.</span><span class="sxs-lookup"><span data-stu-id="d25dd-115">To add a new relying party trust by using the AD FS Management snap-in and manually configure the settings, perform the following procedure on a federation server.</span></span>

<span data-ttu-id="d25dd-116">Členství ve skupině **správci**, nebo ekvivalentní v místním počítači je minimálním předpokladem pro dokončení tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="d25dd-116">Membership in **Administrators**, or equivalent, on the local computer is the minimum required to complete this procedure.</span></span> <span data-ttu-id="d25dd-117">Podrobnosti o používání příslušných účtů a členství ve skupinách v [místní a doménové výchozí skupiny](http://go.microsoft.com/fwlink/?LinkId=83477)</span><span class="sxs-lookup"><span data-stu-id="d25dd-117">Review details about using the appropriate accounts and group memberships at [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)</span></span>

1.  <span data-ttu-id="d25dd-118">Ve Správci serveru klikněte na tlačítko **nástroje**a potom vyberte **správu služby AD FS**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-118">In Server Manager, click **Tools**, and then select **ADFS Management**.</span></span>

2.  <span data-ttu-id="d25dd-119">Klikněte na **přidat vztah důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-119">Click on **Add Relying Party Trust**.</span></span>
    <span data-ttu-id="d25dd-120">![Přidat vztah důvěryhodnosti předávající strany](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-120">![Add Relying Party Trust](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span></span>

3.  <span data-ttu-id="d25dd-121">Na **úvodní** vyberte **deklarace identity vědět** a klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-121">On the **Welcome** page, choose **Claims aware** and click **Start**.</span></span>
    <span data-ttu-id="d25dd-122">![Na úvodní stránce zvolte vědět deklarace identity](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-122">![On the Welcome page, choose Claims aware](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span></span>
4.  <span data-ttu-id="d25dd-123">Na **vybrat zdroj dat** klikněte na tlačítko **zadat data o předávající straně ručně**a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-123">On the **Select Data Source** page, click **Enter data about the relying party manually**, and then click **Next**.</span></span>
    <span data-ttu-id="d25dd-124">![Zadat data o předávající strany](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-124">![Enter data about the relying party](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span></span>

5.  <span data-ttu-id="d25dd-125">Na **zadat zobrazovaný název** stránky, zadejte název do **zobrazovaný název**v části **poznámky** zadejte popis tohoto vztahu důvěryhodnosti předávající strany a pak klikněte na tlačítko **další** .</span><span class="sxs-lookup"><span data-stu-id="d25dd-125">On the **Specify Display Name** page, type a name in **Display name**, under **Notes** type a description for this relying party trust, and then click **Next**.</span></span>
    <span data-ttu-id="d25dd-126">![Zadejte zobrazovaný název a poznámky](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-126">![Specify Display Name and notes](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span></span>
6.  <span data-ttu-id="d25dd-127">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="d25dd-127">Optional.</span></span> <span data-ttu-id="d25dd-128">Pokud máte volitelné tokenu šifrovací certifikát, pak na **konfigurace certifikátu** klikněte na tlačítko **Procházet** vyhledejte soubor certifikátu, a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-128">If you have an optional token encryption certificate, then on the **Configure Certificate** page, click **Browse** to locate your certificate file, and then click **Next**.</span></span>
    <span data-ttu-id="d25dd-129">![Konfigurace certifikátu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-129">![Configure Certificate](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span></span>
7.  <span data-ttu-id="d25dd-130">Na **konfigurace adresy URL** vyberte **povolit podporu protokolu SAML 2.0 WebSSO** zaškrtněte políčko.</span><span class="sxs-lookup"><span data-stu-id="d25dd-130">On the **Configure URL** page, select the **Enable support for the SAML 2.0 WebSSO protocol** check box.</span></span> <span data-ttu-id="d25dd-131">V části **URL služby SAML 2.0 SSO předávající strany**, zadejte adresu URL koncového bodu služby Security (Assertion Markup Language SAML) pro tento vztah důvěryhodnosti předávající strany a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-131">Under **Relying party SAML 2.0 SSO service URL**, type the Security Assertion Markup Language (SAML) service endpoint URL for this relying party trust, and then click **Next**.</span></span>  <span data-ttu-id="d25dd-132">Pro **URL služby SAML 2.0 SSO předávající strany**, vložte `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span><span class="sxs-lookup"><span data-stu-id="d25dd-132">For the **Relying party SAML 2.0 SSO service URL**, paste the `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span></span> <span data-ttu-id="d25dd-133">Nahraďte názvem vašeho klienta (například contosob2c.onmicrosoft.com) {klient} a {zásady} nahraďte název zásady přípony (například B2C_1A_TrustFrameworkExtensions).</span><span class="sxs-lookup"><span data-stu-id="d25dd-133">Replace {tenant} with your tenant's name (for example, contosob2c.onmicrosoft.com), and replace the {policy} with your extensions policy name (for example, B2C_1A_TrustFrameworkExtensions).</span></span>
    > [!IMPORTANT]
    ><span data-ttu-id="d25dd-134">Název zásady je ten, který signup_or_signin zásad zdědí v tomto případě je: `B2C_1A_TrustFrameworkExtensions`.</span><span class="sxs-lookup"><span data-stu-id="d25dd-134">The policy name  is the one that signup_or_signin policy inherits from, in this case it is: `B2C_1A_TrustFrameworkExtensions`.</span></span>
    ><span data-ttu-id="d25dd-135">Adresa URL může být například: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-135">For example the URL could be:   https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span></span>

    ![Předávající strany adresa URL služby SAML 2.0 SSO](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. <span data-ttu-id="d25dd-137">Na **konfigurovat identifikátory** stránky, zadejte stejnou adresu URL jako v předchozím kroku, klikněte na **přidat** je přidejte do seznamu, a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-137">On the **Configure Identifiers** page, specify the same URL as the previous step, click **Add** to add them to the list, and then click **Next**.</span></span>
    <span data-ttu-id="d25dd-138">![Předávající strany důvěryhodnosti identifikátory](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-138">![Relying party trust identifiers](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span></span>
9.  <span data-ttu-id="d25dd-139">Na **zvolte zásad řízení přístupu** vyberte zásadu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-139">On the **Choose Access Control Policy** select a policy and click **Next**.</span></span>
    <span data-ttu-id="d25dd-140">![Zvolte zásad řízení přístupu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-140">![Choose Access Control Policy](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span></span>
10.  <span data-ttu-id="d25dd-141">Na **připravené k přidání vztahu důvěryhodnosti** stránka, zkontrolujte nastavení a pak klikněte na tlačítko **Další** předávající strany uložit informace o vztahu důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="d25dd-141">On the **Ready to Add Trust** page, review the settings, and then click **Next** to save your relying party trust information.</span></span>
    <span data-ttu-id="d25dd-142">![Uložit informace o vztazích důvěryhodnosti předávající strany](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-142">![Save your relying party trust information](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span></span>
11.  <span data-ttu-id="d25dd-143">Na **Dokončit** klikněte na tlačítko **Zavřít**, tato akce se automaticky zobrazí **upravit pravidla deklarací identity** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d25dd-143">On the **Finish** page, click **Close**, this action automatically displays the **Edit Claim Rules** dialog box.</span></span>
    <span data-ttu-id="d25dd-144">![Upravit pravidla deklarace identity](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-144">![Edit Claim Rules](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span></span>
12. <span data-ttu-id="d25dd-145">Klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-145">Click **Add Rule**.</span></span>  
      <span data-ttu-id="d25dd-146">![Přidat nové pravidlo](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-146">![Add new rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span></span>
13.  <span data-ttu-id="d25dd-147">V **šablony pravidla deklarace identity**, vyberte **odesílat atributy LDAP jako deklarace identity**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-147">In **Claim rule template**, select **Send LDAP attributes as claims**.</span></span>
    <span data-ttu-id="d25dd-148">![Vyberte možnost odesílat atributy LDAP jako deklarace šablony](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-148">![Select Send LDAP attributes as claims template rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span></span>
14.  <span data-ttu-id="d25dd-149">Zadejte **název pravidla deklarací**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-149">Provide **Claim rule name**.</span></span> <span data-ttu-id="d25dd-150">Pro **úložiště atributů** vyberte **služby Active Directory vyberte** přidejte následující deklarace identity a pak klikněte na tlačítko **Dokončit** a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-150">For the **Attribute store** select **Select Active Directory** Add the following claims, then click **Finish** and **OK**.</span></span>
    <span data-ttu-id="d25dd-151">![Umožňuje nastavit vlastnosti pravidla](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-151">![Set rule properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span></span>
15.  <span data-ttu-id="d25dd-152">Ve Správci serveru vyberte **vztahy důvěryhodnosti předávající strany** pak vyberte předávající straně důvěryhodnosti jste vytvořili a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-152">In Server Manager, select **Relying Party Trusts** then select the relying party trust you created and click **Properties**.</span></span>
    <span data-ttu-id="d25dd-153">![Předávající strany upravit vlastnosti](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-153">![Relying party edit properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span></span>
16.  <span data-ttu-id="d25dd-154">Jeden okno Vlastnosti vztahu důvěryhodnosti (B2C ukázku) předávající strany, klikněte na tlačítko **podpis** a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-154">One the relying party trust (B2C Demo) properties window click **Signature** tab and click **Add**.</span></span>  
    <span data-ttu-id="d25dd-155">![Sada podpis](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-155">![Set signature](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span></span>
17.  <span data-ttu-id="d25dd-156">Přidání vaší podpis certifikátu (bez soukromého klíče soubor .cert).</span><span class="sxs-lookup"><span data-stu-id="d25dd-156">Add your signature certificate (.cert file, without private key).</span></span>  
    ![Přidejte svůj certifikát podpisu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  <span data-ttu-id="d25dd-158">V okně Vlastnosti vztahu důvěryhodnosti (B2C ukázku) předávající strany klikněte na tlačítko **Upřesnit** kartě a změňte **zabezpečený algoritmus hash** k **SHA-1**, klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-158">On the relying party trust (B2C Demo) properties window click **Advanced** tab and change the **Secure hash algorithm** to **SHA-1**, Click **Ok**.</span></span>  
    <span data-ttu-id="d25dd-159">![Nastavit zabezpečený algoritmus hash SHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-159">![Set secure hash algorithm to SHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span></span>

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a><span data-ttu-id="d25dd-160">Přidat klíč služby AD FS účet aplikace do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d25dd-160">Add the ADFS account application key to Azure AD B2C</span></span>
<span data-ttu-id="d25dd-161">Federace s účty služby AD FS vyžaduje tajný klíč klienta pro účet služby AD FS do vztahu důvěryhodnosti Azure AD B2C jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="d25dd-161">Federation with ADFS accounts requires a client secret for ADFS account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="d25dd-162">Potřebujete k uložení certifikátu služby AD FS v klientovi služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d25dd-162">You need to store your ADFS certificate in your Azure AD B2C tenant.</span></span> 

1.  <span data-ttu-id="d25dd-163">Přejděte ke klientovi Azure AD B2C a vyberte **nastavení B2C** > **Identity rozhraní Framework**</span><span class="sxs-lookup"><span data-stu-id="d25dd-163">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="d25dd-164">Vyberte **zásad klíče** zobrazíte klíče, které jsou k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="d25dd-164">Select **Policy Keys** to view the keys available in your tenant.</span></span>
3.  <span data-ttu-id="d25dd-165">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-165">Click **+Add**.</span></span>
4.  <span data-ttu-id="d25dd-166">Pro **možnosti**, použijte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-166">For **Options**, use **Upload**.</span></span>
5.  <span data-ttu-id="d25dd-167">Pro **název**, použijte `ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="d25dd-167">For **Name**, use `ADFSSamlCert`.</span></span>  
    <span data-ttu-id="d25dd-168">Předpona `B2C_1A_` může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="d25dd-168">The prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="d25dd-169">V samotné nahrávání souborů ** vyberte soubor .pfx certifikátu s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="d25dd-169">In the File upload,** select your certificate .pfx file with private key.</span></span> <span data-ttu-id="d25dd-170">Poznámka: Tento certifikát (s privátním klíčem) by měl být stejný jako ten, který vydala a používat pro předávající strany služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="d25dd-170">Note: this certificate (with the private key) should be the same one that issued and used for the ADFS relying party.</span></span>
<span data-ttu-id="d25dd-171">![Odešlete klíč zásad](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span><span class="sxs-lookup"><span data-stu-id="d25dd-171">![Upload policy key](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span></span>
7.  <span data-ttu-id="d25dd-172">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="d25dd-172">Click **Create**</span></span>
8.  <span data-ttu-id="d25dd-173">Potvrďte, že jste vytvořili klíč `B2C_1A_ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="d25dd-173">Confirm that you've created the key `B2C_1A_ADFSSamlCert`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="d25dd-174">Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="d25dd-174">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="d25dd-175">Pokud chcete uživatelům přihlášení pomocí účtu služby AD FS, musíte zadat účet služby AD FS jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="d25dd-175">If you want users to sign in by using ADFS account, you need to define ADFS account as a claims provider.</span></span> <span data-ttu-id="d25dd-176">Jinými slovy budete muset zadat koncový bod, který komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d25dd-176">In other words, you need to specify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="d25dd-177">Koncový bod poskytuje sadu deklarací identity, které používají Azure AD B2C k ověření, že byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="d25dd-177">The endpoint provides a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="d25dd-178">Definování služby AD FS jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="d25dd-178">Define ADFS as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1. <span data-ttu-id="d25dd-179">Otevřete soubor rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="d25dd-179">Open the extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="d25dd-180">Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="d25dd-180">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2. <span data-ttu-id="d25dd-181">Najít `<ClaimsProviders>` části</span><span class="sxs-lookup"><span data-stu-id="d25dd-181">Find the `<ClaimsProviders>` section</span></span>
3. <span data-ttu-id="d25dd-182">Přidejte následující fragment kódu XML v části `ClaimsProviders` elementu a nahraďte `identityProvider` s DNS (libovolná hodnota, kterou určuje doménu) a uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="d25dd-182">Add the following XML snippet under the `ClaimsProviders` element and replace `identityProvider` with your DNS (Arbitrary value that indicates your domain), and save the file.</span></span> 

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

## <a name="register-the-adfs-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a><span data-ttu-id="d25dd-183">Zaregistrujte zprostředkovatele deklarací identity účtu služby AD FS k přihlášení až nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="d25dd-183">Register the ADFS account claims provider to Sign up or Sign in user journey</span></span>
<span data-ttu-id="d25dd-184">V tomto okamžiku zprostředkovatele identity má nastaven.</span><span class="sxs-lookup"><span data-stu-id="d25dd-184">At this point, the identity provider has been set up.</span></span>  <span data-ttu-id="d25dd-185">Však není k dispozici v žádném z obrazovky registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d25dd-185">However, it is not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="d25dd-186">Nyní je nutné přidat zprostředkovatele identity účtu služby AD FS pro vaše uživatele `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="d25dd-186">Now you need to add the ADFS account identity provider to your user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="d25dd-187">Chcete-li k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="d25dd-187">To make it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="d25dd-188">Potom jsme upravit ji tak, aby zahrnovala zprostředkovatele identity služby AD FS:</span><span class="sxs-lookup"><span data-stu-id="d25dd-188">Then, we modify it so it includes the ADFS identity provider:</span></span>
    >[!NOTE]
    >If you previously copied the `<UserJourneys>` element from base file of your policy to the extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  <span data-ttu-id="d25dd-189">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="d25dd-189">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="d25dd-190">Najít `<UserJourneys>` elementu a zkopírujte celý obsah `<UserJourneys>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="d25dd-190">Find the `<UserJourneys>` element and copy the entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="d25dd-191">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml) a najděte `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d25dd-191">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="d25dd-192">Pokud element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="d25dd-192">If the element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="d25dd-193">Vložte celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d25dd-193">Paste the entire content of `<UserJournesy>` node that you copied as a child of the `<UserJourneys>` element.</span></span>

### <a name="display-the-button"></a><span data-ttu-id="d25dd-194">Zobrazení tlačítka</span><span class="sxs-lookup"><span data-stu-id="d25dd-194">Display the button</span></span>
<span data-ttu-id="d25dd-195">`<ClaimsProviderSelections>` Element definuje seznam možnosti výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="d25dd-195">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span>  <span data-ttu-id="d25dd-196">`<ClaimsProviderSelection>`Element je obdobou tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d25dd-196">`<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="d25dd-197">Pokud přidáte `<ClaimsProviderSelection>` element pro účet služby AD FS, nové tlačítko se zobrazí při pojmenováváme uživatele na stránce.</span><span class="sxs-lookup"><span data-stu-id="d25dd-197">If you add a `<ClaimsProviderSelection>` element for  ADFS account, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="d25dd-198">Chcete-li přidat tento element:</span><span class="sxs-lookup"><span data-stu-id="d25dd-198">To add this element:</span></span>

1.  <span data-ttu-id="d25dd-199">Najít `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="d25dd-199">Find the `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>
2.  <span data-ttu-id="d25dd-200">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="d25dd-200">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="d25dd-201">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="d25dd-201">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-the-button-to-an-action"></a><span data-ttu-id="d25dd-202">Tlačítko propojit akce</span><span class="sxs-lookup"><span data-stu-id="d25dd-202">Link the button to an action</span></span>

<span data-ttu-id="d25dd-203">Nyní když máte tlačítka na místě, budete muset propojit akce.</span><span class="sxs-lookup"><span data-stu-id="d25dd-203">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="d25dd-204">Akce, v takovém případě je pro Azure AD B2C ke komunikaci s účtem služby AD FS přijmout token.</span><span class="sxs-lookup"><span data-stu-id="d25dd-204">The action, in this case, is for Azure AD B2C to communicate with ADFS account to receive a token.</span></span> <span data-ttu-id="d25dd-205">Odkaz na tlačítko akce pomocí propojení technické profil pro poskytovatele deklarací identity účtu služby AD FS:</span><span class="sxs-lookup"><span data-stu-id="d25dd-205">Link the button to an action by linking the technical profile for your ADFS account claims provider:</span></span>

1.  <span data-ttu-id="d25dd-206">Najít `<OrchestrationStep>` , který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="d25dd-206">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="d25dd-207">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="d25dd-207">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * <span data-ttu-id="d25dd-208">Ujistěte se, `Id` mají stejnou hodnotu jako `TargetClaimsExchangeId` v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="d25dd-208">Ensure the `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section.</span></span>
> * <span data-ttu-id="d25dd-209">Ujistěte se, `TechnicalProfileReferenceId` je nastaven na technické profil vytvoříte starší (Contoso-typu SAML2).</span><span class="sxs-lookup"><span data-stu-id="d25dd-209">Ensure `TechnicalProfileReferenceId` is set to the technical profile you created earlier (Contoso-SAML2).</span></span>

## <a name="upload-the-policy-to-your-tenant"></a><span data-ttu-id="d25dd-210">Nahrát zásady klienta</span><span class="sxs-lookup"><span data-stu-id="d25dd-210">Upload the policy to your tenant</span></span>
1.  <span data-ttu-id="d25dd-211">V [portál Azure](https://portal.azure.com), přepněte do [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="d25dd-211">In the [Azure portal](https://portal.azure.com), switch into the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open the **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="d25dd-212">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-212">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="d25dd-213">Otevřete **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="d25dd-213">Open the **All Policies** blade.</span></span>
4.  <span data-ttu-id="d25dd-214">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-214">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="d25dd-215">Zkontrolujte **přepsat zásady, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="d25dd-215">Check **Overwrite the policy if it exists** box.</span></span>
6.  <span data-ttu-id="d25dd-216">**Nahrát** TrustFrameworkExtensions.xml a ujistěte se, že neselže ověření</span><span class="sxs-lookup"><span data-stu-id="d25dd-216">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail the validation</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="d25dd-217">Otestovat vlastní zásady pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="d25dd-217">Test the custom policy by using Run Now</span></span>
1.  <span data-ttu-id="d25dd-218">Otevřete **nastavení Azure AD B2C** a přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-218">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="d25dd-219">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="d25dd-219">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="d25dd-220">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-220">Select **Run now**.</span></span>
3.  <span data-ttu-id="d25dd-221">Nyní byste měli mít k přihlášení pomocí účtu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="d25dd-221">You should be able to sign in using ADFS account.</span></span>

## <a name="optional-register-the-adfs-account-claims-provider-to-profile-edit-user-journey"></a><span data-ttu-id="d25dd-222">[Nepovinné] Zaregistrujte poskytovatele deklarací identity účtu služby AD FS pro úpravy profilu uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="d25dd-222">[Optional] Register the ADFS account claims provider to Profile-Edit user journey</span></span>
<span data-ttu-id="d25dd-223">Můžete také přidat zprostředkovatele identity účtu služby AD FS pro vaše uživatele `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="d25dd-223">You may want to add the ADFS account identity provider also to your user `ProfileEdit` user journey.</span></span> <span data-ttu-id="d25dd-224">Chcete-li k dispozici, jsme zopakujte poslední dva kroky:</span><span class="sxs-lookup"><span data-stu-id="d25dd-224">To make it available, we repeat the last two steps:</span></span>

### <a name="display-the-button"></a><span data-ttu-id="d25dd-225">Zobrazení tlačítka</span><span class="sxs-lookup"><span data-stu-id="d25dd-225">Display the button</span></span>
1.  <span data-ttu-id="d25dd-226">Otevřete soubor rozšíření zásad (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="d25dd-226">Open the extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="d25dd-227">Najít `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="d25dd-227">Find the `<UserJourney>` node that includes `Id="ProfileEdit"` in the user journey that you copied.</span></span>
3.  <span data-ttu-id="d25dd-228">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="d25dd-228">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="d25dd-229">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="d25dd-229">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="d25dd-230">Tlačítko propojit akce</span><span class="sxs-lookup"><span data-stu-id="d25dd-230">Link the button to an action</span></span>
1.  <span data-ttu-id="d25dd-231">Najít `<OrchestrationStep>` , který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="d25dd-231">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="d25dd-232">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="d25dd-232">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="d25dd-233">Otestovat vlastní úpravy profilu zásady pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="d25dd-233">Test the custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="d25dd-234">Otevřete **nastavení Azure AD B2C** a přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-234">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="d25dd-235">Otevřete **B2C_1A_ProfileEdit**, předávající stranu vlastních zásad, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="d25dd-235">Open **B2C_1A_ProfileEdit**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="d25dd-236">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="d25dd-236">Select **Run now**.</span></span>
3.  <span data-ttu-id="d25dd-237">Nyní byste měli mít k přihlášení pomocí účtu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="d25dd-237">You should be able to sign in using ADFS account.</span></span>

## <a name="download-the-complete-policy-files"></a><span data-ttu-id="d25dd-238">Stáhnout soubory dokončení zásad</span><span class="sxs-lookup"><span data-stu-id="d25dd-238">Download the complete policy files</span></span>
<span data-ttu-id="d25dd-239">Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní vlastní zásady, které provede soubory po dokončení Začínáme s vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="d25dd-239">Optional: We recommend you build your scenario using your own Custom policy files after completing the Getting Started with Custom Policies walk through.</span></span> [<span data-ttu-id="d25dd-240">Ukázkové soubory zásad jenom jako reference</span><span class="sxs-lookup"><span data-stu-id="d25dd-240">Policy sample files for reference only</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
