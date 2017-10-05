---
title: 'Kurz: Azure Active Directory integrace s Zendesk | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a služby Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 51c06d838c5ed6286dfb99ea25faaaf33bad5f3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a><span data-ttu-id="d5f79-103">Kurz: Azure Active Directory integrace s Zendesk</span><span class="sxs-lookup"><span data-stu-id="d5f79-103">Tutorial: Azure Active Directory integration with Zendesk</span></span>

<span data-ttu-id="d5f79-104">V tomto kurzu zjistěte, jak integrovat Zendesk s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d5f79-104">In this tutorial, you learn how to integrate Zendesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d5f79-105">Integrace služby Zendesk s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d5f79-105">Integrating Zendesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d5f79-106">Můžete řídit ve službě Azure AD, který má přístup k této služby</span><span class="sxs-lookup"><span data-stu-id="d5f79-106">You can control in Azure AD who has access to Zendesk</span></span>
- <span data-ttu-id="d5f79-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Zendesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5f79-107">You can enable your users to automatically get signed-on to Zendesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d5f79-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d5f79-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d5f79-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d5f79-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5f79-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d5f79-110">Prerequisites</span></span>

<span data-ttu-id="d5f79-111">Konfigurace integrace Azure AD s Zendesk, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d5f79-111">To configure Azure AD integration with Zendesk, you need the following items:</span></span>

- <span data-ttu-id="d5f79-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5f79-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d5f79-113">Této služby jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d5f79-113">A Zendesk single sign-on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="d5f79-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5f79-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="d5f79-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d5f79-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d5f79-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d5f79-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d5f79-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5f79-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="d5f79-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d5f79-118">Scenario description</span></span>
<span data-ttu-id="d5f79-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5f79-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d5f79-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d5f79-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d5f79-121">Přidání Zendesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="d5f79-121">Adding Zendesk from the gallery</span></span>
2. <span data-ttu-id="d5f79-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5f79-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zendesk-from-the-gallery"></a><span data-ttu-id="d5f79-123">Přidání Zendesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="d5f79-123">Adding Zendesk from the gallery</span></span>
<span data-ttu-id="d5f79-124">Při konfiguraci integrace služby Zendesk do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Zendesk z galerie.</span><span class="sxs-lookup"><span data-stu-id="d5f79-124">To configure the integration of Zendesk into Azure AD, you need to add Zendesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d5f79-125">**Pokud chcete přidat Zendesk z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5f79-125">**To add Zendesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d5f79-126">V  **[portálu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d5f79-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d5f79-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d5f79-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d5f79-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5f79-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d5f79-133">Do vyhledávacího pole zadejte **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-133">In the search box, type **Zendesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. <span data-ttu-id="d5f79-135">Na panelu výsledků vyberte **Zendesk**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5f79-135">In the results panel, select **Zendesk**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d5f79-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5f79-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d5f79-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí služby Zendesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d5f79-138">In this section, you configure and test Azure AD single sign-on with Zendesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d5f79-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v této služby je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d5f79-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zendesk is to a user in Azure AD.</span></span> <span data-ttu-id="d5f79-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v této služby je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="d5f79-140">In other words, a link relationship between an Azure AD user and the related user in Zendesk needs to be established.</span></span>

<span data-ttu-id="d5f79-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v této služby.</span><span class="sxs-lookup"><span data-stu-id="d5f79-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zendesk.</span></span>

<span data-ttu-id="d5f79-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zendesk, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d5f79-142">To configure and test Azure AD single sign-on with Zendesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d5f79-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d5f79-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d5f79-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5f79-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d5f79-145">**[Vytvoření zkušebního uživatele Zendesk](#creating-a-zendesk-test-user)**  – Pokud chcete mít protějšek Britta Simon v Zendesku propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d5f79-145">**[Creating a Zendesk test user](#creating-a-zendesk-test-user)** - to have a counterpart of Britta Simon in Zendesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d5f79-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d5f79-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d5f79-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d5f79-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d5f79-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5f79-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d5f79-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci služby Zendesk.</span><span class="sxs-lookup"><span data-stu-id="d5f79-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zendesk application.</span></span>

<span data-ttu-id="d5f79-150">**Ke konfiguraci Azure AD jednotné přihlašování s Zendesk, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5f79-150">**To configure Azure AD single sign-on with Zendesk, perform the following steps:**</span></span>

1. <span data-ttu-id="d5f79-151">Na portálu Azure na **Zendesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-151">In the Azure portal, on the **Zendesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d5f79-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d5f79-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. <span data-ttu-id="d5f79-155">Na **Zendesk domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5f79-155">On the **Zendesk Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    <span data-ttu-id="d5f79-157">a.</span><span class="sxs-lookup"><span data-stu-id="d5f79-157">a.</span></span> <span data-ttu-id="d5f79-158">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="d5f79-158">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<subdomain>.zendesk.com`</span></span>

    <span data-ttu-id="d5f79-159">b.</span><span class="sxs-lookup"><span data-stu-id="d5f79-159">b.</span></span> <span data-ttu-id="d5f79-160">V **identifikátor** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="d5f79-160">In the **Identifier** textbox, type the value using the following pattern: `https://<subdomain>.zendesk.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d5f79-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d5f79-161">These values are not real.</span></span> <span data-ttu-id="d5f79-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d5f79-162">Update these values with the actual Sign-on URL and Identifier URL.</span></span> <span data-ttu-id="d5f79-163">Obraťte se na [tým podpory služby Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d5f79-163">Contact [Zendesk support team](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) to get these values.</span></span> 

4. <span data-ttu-id="d5f79-164">Zendesk očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="d5f79-164">Zendesk expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="d5f79-165">Neexistují žádné povinné atributy SAML, ale můžete volitelně přidat atribut z **uživatelské atributy** části podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d5f79-165">There are no mandatory SAML attributes but optionally you can add an attribute from **User Attributes** section by following the below steps:</span></span> 

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    <span data-ttu-id="d5f79-167">a.</span><span class="sxs-lookup"><span data-stu-id="d5f79-167">a.</span></span> <span data-ttu-id="d5f79-168">Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="d5f79-168">Click the **View and edit all the other attributes** check box.</span></span>
     
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    <span data-ttu-id="d5f79-170">b.</span><span class="sxs-lookup"><span data-stu-id="d5f79-170">b.</span></span> <span data-ttu-id="d5f79-171">Klikněte **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5f79-171">Click the **Add Attribute** to open **Add attribute** dialog.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="d5f79-173">c.</span><span class="sxs-lookup"><span data-stu-id="d5f79-173">c.</span></span> <span data-ttu-id="d5f79-174">V **název** textovému poli, zadejte název atributu (například **emailaddress**).</span><span class="sxs-lookup"><span data-stu-id="d5f79-174">In the **Name** textbox, type the attribute name (for example **emailaddress**).</span></span>
    
    <span data-ttu-id="d5f79-175">d.</span><span class="sxs-lookup"><span data-stu-id="d5f79-175">d.</span></span> <span data-ttu-id="d5f79-176">Z **hodnotu** vyberte hodnotu atributu (jako **user.mail**).</span><span class="sxs-lookup"><span data-stu-id="d5f79-176">From the **Value** list, select the attribute value (as **user.mail**).</span></span>
    
    <span data-ttu-id="d5f79-177">e.</span><span class="sxs-lookup"><span data-stu-id="d5f79-177">e.</span></span> <span data-ttu-id="d5f79-178">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="d5f79-178">Click **Ok**</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="d5f79-179">Atributy rozšíření je použít k přidání atributů, které nejsou ve službě Azure AD ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d5f79-179">You use extension attributes to add attributes that are not in Azure AD by default.</span></span> <span data-ttu-id="d5f79-180">Klikněte na tlačítko [atributy uživatele, které lze nastavit v SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) získat úplný seznam SAML atributy, které **Zendesk** přijímá.</span><span class="sxs-lookup"><span data-stu-id="d5f79-180">Click [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) to get the complete list of SAML attributes that **Zendesk** accepts.</span></span> 

5. <span data-ttu-id="d5f79-181">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d5f79-181">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. <span data-ttu-id="d5f79-183">Na **Zendesk konfigurace** klikněte na tlačítko **konfigurace Zendesk** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d5f79-183">On the **Zendesk Configuration** section, click **Configure Zendesk** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d5f79-184">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d5f79-184">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. <span data-ttu-id="d5f79-186">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zendesk.</span><span class="sxs-lookup"><span data-stu-id="d5f79-186">In a different web browser window, log into your Zendesk company site as an administrator.</span></span>

8. <span data-ttu-id="d5f79-187">Klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-187">Click **Admin**.</span></span>

9. <span data-ttu-id="d5f79-188">V levém navigačním podokně klikněte na tlačítko **nastavení**a potom klikněte na **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-188">In the left navigation pane, click **Settings**, and then click **Security**.</span></span>

10. <span data-ttu-id="d5f79-189">Na **zabezpečení** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5f79-189">On the **Security** page, perform the following steps:</span></span> 
   
     <span data-ttu-id="d5f79-190">![Zabezpečení](./media/active-directory-saas-zendesk-tutorial/ic773089.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="d5f79-190">![Security](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Security")</span></span>

    <span data-ttu-id="d5f79-191">![Jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/ic773090.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d5f79-191">![Single sign-on](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Single sign-on")</span></span>

     <span data-ttu-id="d5f79-192">a.</span><span class="sxs-lookup"><span data-stu-id="d5f79-192">a.</span></span> <span data-ttu-id="d5f79-193">Klikněte **správce & agenty** kartě.</span><span class="sxs-lookup"><span data-stu-id="d5f79-193">Click the **Admin & Agents** tab.</span></span>

     <span data-ttu-id="d5f79-194">b.</span><span class="sxs-lookup"><span data-stu-id="d5f79-194">b.</span></span> <span data-ttu-id="d5f79-195">Vyberte **jednotné přihlašování (SSO) a SAML**a potom vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-195">Select **Single sign-on (SSO) and SAML**, and then select **SAML**.</span></span>

     <span data-ttu-id="d5f79-196">c.</span><span class="sxs-lookup"><span data-stu-id="d5f79-196">c.</span></span> <span data-ttu-id="d5f79-197">V **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5f79-197">In **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

     <span data-ttu-id="d5f79-198">d.</span><span class="sxs-lookup"><span data-stu-id="d5f79-198">d.</span></span> <span data-ttu-id="d5f79-199">V **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5f79-199">In **Remote Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
        
     <span data-ttu-id="d5f79-200">e.</span><span class="sxs-lookup"><span data-stu-id="d5f79-200">e.</span></span> <span data-ttu-id="d5f79-201">V **otisků prstů certifikátů** textovému poli, Vložit **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5f79-201">In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="d5f79-202">f.</span><span class="sxs-lookup"><span data-stu-id="d5f79-202">f.</span></span> <span data-ttu-id="d5f79-203">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-203">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d5f79-204">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5f79-204">Creating an Azure AD test user</span></span>
<span data-ttu-id="d5f79-205">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5f79-205">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d5f79-207">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5f79-207">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d5f79-208">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d5f79-208">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d5f79-210">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-210">To display the list of users go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d5f79-212">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5f79-212">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d5f79-214">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5f79-214">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d5f79-216">a.</span><span class="sxs-lookup"><span data-stu-id="d5f79-216">a.</span></span> <span data-ttu-id="d5f79-217">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-217">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d5f79-218">b.</span><span class="sxs-lookup"><span data-stu-id="d5f79-218">b.</span></span> <span data-ttu-id="d5f79-219">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d5f79-219">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d5f79-220">c.</span><span class="sxs-lookup"><span data-stu-id="d5f79-220">c.</span></span> <span data-ttu-id="d5f79-221">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-221">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d5f79-222">d.</span><span class="sxs-lookup"><span data-stu-id="d5f79-222">d.</span></span> <span data-ttu-id="d5f79-223">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-223">Click **Create**.</span></span> 

### <a name="creating-a-zendesk-test-user"></a><span data-ttu-id="d5f79-224">Vytvoření zkušebního uživatele Zendesk</span><span class="sxs-lookup"><span data-stu-id="d5f79-224">Creating a Zendesk test user</span></span>

<span data-ttu-id="d5f79-225">Povolit uživatelům přihlášení do Azure AD **Zendesk**, musí být zřízená do **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-225">To enable Azure AD users to log into **Zendesk**, they must be provisioned into **Zendesk**.</span></span>  
<span data-ttu-id="d5f79-226">V závislosti na role přiřazené v aplikace je očekávané chování:</span><span class="sxs-lookup"><span data-stu-id="d5f79-226">Depending on the role assigned in the apps, it's the expected behavior:</span></span>

 1. <span data-ttu-id="d5f79-227">**Koncový uživatel** účty jsou zřízené automaticky při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d5f79-227">**End-user** accounts are automatically provisioned when signing in.</span></span>
 2. <span data-ttu-id="d5f79-228">**Agent** a **správce** účty muset ručně zřídit v **Zendesk** před přihlášením.</span><span class="sxs-lookup"><span data-stu-id="d5f79-228">**Agent** and **Admin** accounts need to be manually provisioned in **Zendesk** before signing in.</span></span>
 
<span data-ttu-id="d5f79-229">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5f79-229">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="d5f79-230">Přihlaste se k vaší **Zendesk** klienta.</span><span class="sxs-lookup"><span data-stu-id="d5f79-230">Log in to your **Zendesk** tenant.</span></span>

2. <span data-ttu-id="d5f79-231">Vyberte **seznamu zákazníků** kartě.</span><span class="sxs-lookup"><span data-stu-id="d5f79-231">Select the **Customer List** tab.</span></span>

3. <span data-ttu-id="d5f79-232">Vyberte **uživatele** a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-232">Select the **User** tab, and click **Add**.</span></span>
   
    <span data-ttu-id="d5f79-233">![Přidat uživatele](./media/active-directory-saas-zendesk-tutorial/ic773632.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="d5f79-233">![Add user](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Add user")</span></span>
4. <span data-ttu-id="d5f79-234">Zadejte e-mailovou adresu stávajícího účtu Azure AD určené ke zřízení a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-234">Type the email address of an existing Azure AD account you want to provision, and then click **Save**.</span></span>
   
    <span data-ttu-id="d5f79-235">![Nový uživatel](./media/active-directory-saas-zendesk-tutorial/ic773633.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="d5f79-235">![New user](./media/active-directory-saas-zendesk-tutorial/ic773633.png "New user")</span></span>

> [!NOTE]
> <span data-ttu-id="d5f79-236">Můžete použít všechny ostatní Zendesk uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Zendesk zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d5f79-236">You can use any other Zendesk user account creation tools or APIs provided by Zendesk to provision AAD user accounts.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d5f79-237">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5f79-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d5f79-238">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k této služby.</span><span class="sxs-lookup"><span data-stu-id="d5f79-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zendesk.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d5f79-240">**Pokud chcete přiřadit Britta Simon Zendesk, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5f79-240">**To assign Britta Simon to Zendesk, perform the following steps:**</span></span>

1. <span data-ttu-id="d5f79-241">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d5f79-243">V seznamu aplikací vyberte **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-243">In the applications list, select **Zendesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. <span data-ttu-id="d5f79-245">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d5f79-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d5f79-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d5f79-247">Click **Add** button.</span></span> <span data-ttu-id="d5f79-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5f79-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d5f79-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d5f79-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d5f79-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5f79-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d5f79-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5f79-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d5f79-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5f79-253">Testing single sign-on</span></span>

<span data-ttu-id="d5f79-254">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d5f79-254">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d5f79-255">Když kliknete na dlaždici služby Zendesk na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci služby Zendesk.</span><span class="sxs-lookup"><span data-stu-id="d5f79-255">When you click the Zendesk tile in the Access Panel, you should get automatically signed-on to your Zendesk application.</span></span>
<span data-ttu-id="d5f79-256">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d5f79-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5f79-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d5f79-257">Additional resources</span></span>

* [<span data-ttu-id="d5f79-258">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d5f79-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d5f79-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d5f79-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
