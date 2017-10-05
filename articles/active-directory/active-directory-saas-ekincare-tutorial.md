---
title: 'Kurz: Azure Active Directory integrace s eKincare | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a eKincare."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 57f56d14-83cf-4cbb-b342-fac4fc60078f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 014eaff14974bb6cd551b6fe53409ede6a6dfea1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ekincare"></a><span data-ttu-id="110d6-103">Kurz: Azure Active Directory integrace s eKincare</span><span class="sxs-lookup"><span data-stu-id="110d6-103">Tutorial: Azure Active Directory integration with eKincare</span></span>

<span data-ttu-id="110d6-104">V tomto kurzu zjistěte, jak integrovat eKincare s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="110d6-104">In this tutorial, you learn how to integrate eKincare with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="110d6-105">Integrace eKincare s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="110d6-105">Integrating eKincare with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="110d6-106">Můžete řídit ve službě Azure AD, který má přístup k eKincare</span><span class="sxs-lookup"><span data-stu-id="110d6-106">You can control in Azure AD who has access to eKincare</span></span>
- <span data-ttu-id="110d6-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k eKincare (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="110d6-107">You can enable your users to automatically get signed-on to eKincare (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="110d6-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="110d6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="110d6-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="110d6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="110d6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="110d6-110">Prerequisites</span></span>

<span data-ttu-id="110d6-111">Konfigurace integrace Azure AD s eKincare, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="110d6-111">To configure Azure AD integration with eKincare, you need the following items:</span></span>

- <span data-ttu-id="110d6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="110d6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="110d6-113">EKincare jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="110d6-113">A eKincare single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="110d6-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="110d6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="110d6-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="110d6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="110d6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="110d6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="110d6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="110d6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="110d6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="110d6-118">Scenario description</span></span>
<span data-ttu-id="110d6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="110d6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="110d6-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="110d6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="110d6-121">Přidání eKincare z Galerie</span><span class="sxs-lookup"><span data-stu-id="110d6-121">Adding eKincare from the gallery</span></span>
2. <span data-ttu-id="110d6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="110d6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ekincare-from-the-gallery"></a><span data-ttu-id="110d6-123">Přidání eKincare z Galerie</span><span class="sxs-lookup"><span data-stu-id="110d6-123">Adding eKincare from the gallery</span></span>
<span data-ttu-id="110d6-124">Při konfiguraci integrace eKincare do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS eKincare z galerie.</span><span class="sxs-lookup"><span data-stu-id="110d6-124">To configure the integration of eKincare into Azure AD, you need to add eKincare from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="110d6-125">**Pokud chcete přidat eKincare z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="110d6-125">**To add eKincare from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="110d6-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="110d6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="110d6-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="110d6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="110d6-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="110d6-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="110d6-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="110d6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="110d6-133">Do vyhledávacího pole zadejte **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="110d6-133">In the search box, type **eKincare**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_search.png)

5. <span data-ttu-id="110d6-135">Na panelu výsledků vyberte **eKincare**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="110d6-135">In the results panel, select **eKincare**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="110d6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="110d6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="110d6-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s eKincare podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="110d6-138">In this section, you configure and test Azure AD single sign-on with eKincare based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="110d6-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v eKincare je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="110d6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in eKincare is to a user in Azure AD.</span></span> <span data-ttu-id="110d6-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v eKincare musí navázat.</span><span class="sxs-lookup"><span data-stu-id="110d6-140">In other words, a link relationship between an Azure AD user and the related user in eKincare needs to be established.</span></span>

<span data-ttu-id="110d6-141">V eKincare, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="110d6-141">In eKincare, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="110d6-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s eKincare, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="110d6-142">To configure and test Azure AD single sign-on with eKincare, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="110d6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="110d6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="110d6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="110d6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="110d6-145">**[Vytvoření zkušebního uživatele eKincare](#creating-a-ekincare-test-user)**  – Pokud chcete mít protějšek Britta Simon v eKincare propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="110d6-145">**[Creating a eKincare test user](#creating-a-ekincare-test-user)** - to have a counterpart of Britta Simon in eKincare that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="110d6-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="110d6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="110d6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="110d6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="110d6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="110d6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="110d6-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci eKincare.</span><span class="sxs-lookup"><span data-stu-id="110d6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your eKincare application.</span></span>

<span data-ttu-id="110d6-150">**Ke konfiguraci Azure AD jednotné přihlašování s eKincare, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="110d6-150">**To configure Azure AD single sign-on with eKincare, perform the following steps:**</span></span>

1. <span data-ttu-id="110d6-151">Na portálu Azure na **eKincare** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="110d6-151">In the Azure portal, on the **eKincare** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="110d6-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="110d6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_samlbase.png)

3. <span data-ttu-id="110d6-155">Na **eKincare domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="110d6-155">On the **eKincare Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_url.png)

    <span data-ttu-id="110d6-157">a.</span><span class="sxs-lookup"><span data-stu-id="110d6-157">a.</span></span> <span data-ttu-id="110d6-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.ekincare.com/`</span><span class="sxs-lookup"><span data-stu-id="110d6-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.ekincare.com/`</span></span>

    <span data-ttu-id="110d6-159">b.</span><span class="sxs-lookup"><span data-stu-id="110d6-159">b.</span></span> <span data-ttu-id="110d6-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.ekincare.com/hul/saml`</span><span class="sxs-lookup"><span data-stu-id="110d6-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<instancename>.ekincare.com/hul/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="110d6-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="110d6-161">These values are not real.</span></span> <span data-ttu-id="110d6-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="110d6-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="110d6-163">Obraťte se na [tým podpory eKincare](mailto:tech@ekincare.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="110d6-163">Contact [eKincare support team](mailto:tech@ekincare.com) to get these values.</span></span>
 
4. <span data-ttu-id="110d6-164">aplikace eKincare očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="110d6-164">eKincare application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="110d6-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="110d6-165">Configure the following claims for this application.</span></span> <span data-ttu-id="110d6-166">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="110d6-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="110d6-167">Následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="110d6-167">The following screenshot shows an example for this configuration.</span></span>

    <span data-ttu-id="110d6-168">Název deklarací bude vždy **"employeeid"** a hodnoty, které jsme jsou namapovány user.extensionattribute1, který obsahuje employeeid uživatele.</span><span class="sxs-lookup"><span data-stu-id="110d6-168">The claim name will always be **"employeeid"** and the value of which we have mapped to user.extensionattribute1, that contains the employeeid of the user.</span></span> <span data-ttu-id="110d6-169">Název jednofaktorovému další dva deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="110d6-169">The other two claims' name i.e</span></span> <span data-ttu-id="110d6-170">**"kódu organizace"** a **"název organizace"** bude vždy stejnou a jejich hodnoty obsahují podrobnosti o organizaci uživatele v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="110d6-170">**"organizationid"** and **"organizationname"** will always be same and their values contain the details of the organization of the user respectively.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/attribute.png)
    
5. <span data-ttu-id="110d6-172">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="110d6-172">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="110d6-173">Název atributu</span><span class="sxs-lookup"><span data-stu-id="110d6-173">Attribute Name</span></span> | <span data-ttu-id="110d6-174">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="110d6-174">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="110d6-175">Číslo zaměstnance</span><span class="sxs-lookup"><span data-stu-id="110d6-175">employeeid</span></span> | <span data-ttu-id="110d6-176">*User.extensionattribute1*</span><span class="sxs-lookup"><span data-stu-id="110d6-176">*user.extensionattribute1*</span></span> |
    | <span data-ttu-id="110d6-177">kódu organizace</span><span class="sxs-lookup"><span data-stu-id="110d6-177">organizationid</span></span> | <span data-ttu-id="110d6-178">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="110d6-178">*"uniquevalue"*</span></span> |
    | <span data-ttu-id="110d6-179">název organizace</span><span class="sxs-lookup"><span data-stu-id="110d6-179">organizationname</span></span> | <span data-ttu-id="110d6-180">*User.CompanyName*</span><span class="sxs-lookup"><span data-stu-id="110d6-180">*user.companyname*</span></span> |

    <span data-ttu-id="110d6-181">a.</span><span class="sxs-lookup"><span data-stu-id="110d6-181">a.</span></span> <span data-ttu-id="110d6-182">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="110d6-182">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/05.png)
    
    <span data-ttu-id="110d6-185">b.</span><span class="sxs-lookup"><span data-stu-id="110d6-185">b.</span></span> <span data-ttu-id="110d6-186">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="110d6-186">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="110d6-187">c.</span><span class="sxs-lookup"><span data-stu-id="110d6-187">c.</span></span> <span data-ttu-id="110d6-188">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="110d6-188">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="110d6-189">d.</span><span class="sxs-lookup"><span data-stu-id="110d6-189">d.</span></span> <span data-ttu-id="110d6-190">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="110d6-190">Click **Ok**</span></span>

6. <span data-ttu-id="110d6-191">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="110d6-191">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_certificate.png) 

7. <span data-ttu-id="110d6-193">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="110d6-193">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="110d6-195">Konfigurace jednotného přihlašování na **eKincare** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory eKincare](mailto:tech@ekincare.com).</span><span class="sxs-lookup"><span data-stu-id="110d6-195">To configure single sign-on on **eKincare** side, you need to send the downloaded **Metadata XML** to [eKincare support team](mailto:tech@ekincare.com).</span></span> <span data-ttu-id="110d6-196">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="110d6-196">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="110d6-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="110d6-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="110d6-198">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="110d6-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="110d6-199">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="110d6-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="110d6-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="110d6-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="110d6-201">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="110d6-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="110d6-203">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="110d6-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="110d6-204">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="110d6-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="110d6-206">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="110d6-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="110d6-208">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="110d6-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="110d6-210">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="110d6-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="110d6-212">a.</span><span class="sxs-lookup"><span data-stu-id="110d6-212">a.</span></span> <span data-ttu-id="110d6-213">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="110d6-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="110d6-214">b.</span><span class="sxs-lookup"><span data-stu-id="110d6-214">b.</span></span> <span data-ttu-id="110d6-215">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="110d6-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="110d6-216">c.</span><span class="sxs-lookup"><span data-stu-id="110d6-216">c.</span></span> <span data-ttu-id="110d6-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="110d6-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="110d6-218">d.</span><span class="sxs-lookup"><span data-stu-id="110d6-218">d.</span></span> <span data-ttu-id="110d6-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="110d6-219">Click **Create**.</span></span>
 
### <a name="creating-a-ekincare-test-user"></a><span data-ttu-id="110d6-220">Vytvoření zkušebního uživatele eKincare</span><span class="sxs-lookup"><span data-stu-id="110d6-220">Creating a eKincare test user</span></span>

<span data-ttu-id="110d6-221">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="110d6-221">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="110d6-222">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="110d6-222">Assigning the Azure AD test user</span></span>

<span data-ttu-id="110d6-223">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k eKincare.</span><span class="sxs-lookup"><span data-stu-id="110d6-223">In this section, you enable Britta Simon to use Azure single sign-on by granting access to eKincare.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="110d6-225">**Pokud chcete přiřadit Britta Simon eKincare, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="110d6-225">**To assign Britta Simon to eKincare, perform the following steps:**</span></span>

1. <span data-ttu-id="110d6-226">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="110d6-226">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="110d6-228">V seznamu aplikací vyberte **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="110d6-228">In the applications list, select **eKincare**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_app.png) 

3. <span data-ttu-id="110d6-230">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="110d6-230">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="110d6-232">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="110d6-232">Click **Add** button.</span></span> <span data-ttu-id="110d6-233">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="110d6-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="110d6-235">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="110d6-235">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="110d6-236">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="110d6-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="110d6-237">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="110d6-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="110d6-238">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="110d6-238">Testing single sign-on</span></span>

<span data-ttu-id="110d6-239">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="110d6-239">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="110d6-240">Když kliknete na dlaždici eKincare na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci eKincare.</span><span class="sxs-lookup"><span data-stu-id="110d6-240">When you click the eKincare tile in the Access Panel, you should get automatically signed-on to your eKincare application.</span></span>
<span data-ttu-id="110d6-241">Další informace o na přístupovém panelu najdete v tématu [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="110d6-241">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="110d6-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="110d6-242">Additional resources</span></span>

* [<span data-ttu-id="110d6-243">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="110d6-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="110d6-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="110d6-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_203.png

