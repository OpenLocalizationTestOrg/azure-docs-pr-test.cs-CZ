---
title: 'Kurz: Azure Active Directory integrace s Picturepark | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Picturepark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 1c009aa1fdd3140a4466cf762b6c9687e74ce4c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a><span data-ttu-id="a4d37-103">Kurz: Azure Active Directory integrace s Picturepark</span><span class="sxs-lookup"><span data-stu-id="a4d37-103">Tutorial: Azure Active Directory integration with Picturepark</span></span>

<span data-ttu-id="a4d37-104">V tomto kurzu zjistěte, jak integrovat Picturepark s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a4d37-104">In this tutorial, you learn how to integrate Picturepark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a4d37-105">Integrace Picturepark s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a4d37-105">Integrating Picturepark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a4d37-106">Můžete řídit ve službě Azure AD, který má přístup k Picturepark</span><span class="sxs-lookup"><span data-stu-id="a4d37-106">You can control in Azure AD who has access to Picturepark</span></span>
- <span data-ttu-id="a4d37-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Picturepark (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4d37-107">You can enable your users to automatically get signed-on to Picturepark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a4d37-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a4d37-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a4d37-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a4d37-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4d37-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4d37-110">Prerequisites</span></span>

<span data-ttu-id="a4d37-111">Konfigurace integrace Azure AD s Picturepark, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a4d37-111">To configure Azure AD integration with Picturepark, you need the following items:</span></span>

- <span data-ttu-id="a4d37-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4d37-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a4d37-113">Picturepark jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a4d37-113">A Picturepark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a4d37-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4d37-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a4d37-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a4d37-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a4d37-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a4d37-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a4d37-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a4d37-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a4d37-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a4d37-118">Scenario description</span></span>
<span data-ttu-id="a4d37-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4d37-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a4d37-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a4d37-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a4d37-121">Přidání Picturepark z Galerie</span><span class="sxs-lookup"><span data-stu-id="a4d37-121">Adding Picturepark from the gallery</span></span>
2. <span data-ttu-id="a4d37-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4d37-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-picturepark-from-the-gallery"></a><span data-ttu-id="a4d37-123">Přidání Picturepark z Galerie</span><span class="sxs-lookup"><span data-stu-id="a4d37-123">Adding Picturepark from the gallery</span></span>
<span data-ttu-id="a4d37-124">Při konfiguraci integrace Picturepark do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Picturepark z galerie.</span><span class="sxs-lookup"><span data-stu-id="a4d37-124">To configure the integration of Picturepark into Azure AD, you need to add Picturepark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a4d37-125">**Pokud chcete přidat Picturepark z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4d37-125">**To add Picturepark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a4d37-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4d37-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a4d37-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a4d37-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a4d37-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4d37-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a4d37-133">Do vyhledávacího pole zadejte **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-133">In the search box, type **Picturepark**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. <span data-ttu-id="a4d37-135">Na panelu výsledků vyberte **Picturepark**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4d37-135">In the results panel, select **Picturepark**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a4d37-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4d37-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a4d37-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Picturepark podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a4d37-138">In this section, you configure and test Azure AD single sign-on with Picturepark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a4d37-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Picturepark je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a4d37-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Picturepark is to a user in Azure AD.</span></span> <span data-ttu-id="a4d37-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Picturepark musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a4d37-140">In other words, a link relationship between an Azure AD user and the related user in Picturepark needs to be established.</span></span>

<span data-ttu-id="a4d37-141">V Picturepark, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a4d37-141">In Picturepark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a4d37-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Picturepark, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a4d37-142">To configure and test Azure AD single sign-on with Picturepark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a4d37-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a4d37-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a4d37-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4d37-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a4d37-145">**[Vytvoření zkušebního uživatele Picturepark](#creating-a-picturepark-test-user)**  – Pokud chcete mít protějšek Britta Simon v Picturepark propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4d37-145">**[Creating a Picturepark test user](#creating-a-picturepark-test-user)** - to have a counterpart of Britta Simon in Picturepark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a4d37-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4d37-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a4d37-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a4d37-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a4d37-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4d37-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a4d37-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Picturepark.</span><span class="sxs-lookup"><span data-stu-id="a4d37-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Picturepark application.</span></span>

<span data-ttu-id="a4d37-150">**Ke konfiguraci Azure AD jednotné přihlašování s Picturepark, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4d37-150">**To configure Azure AD single sign-on with Picturepark, perform the following steps:**</span></span>

1. <span data-ttu-id="a4d37-151">Na portálu Azure na **Picturepark** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-151">In the Azure portal, on the **Picturepark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a4d37-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4d37-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. <span data-ttu-id="a4d37-155">Na **Picturepark domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4d37-155">On the **Picturepark Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    <span data-ttu-id="a4d37-157">a.</span><span class="sxs-lookup"><span data-stu-id="a4d37-157">a.</span></span> <span data-ttu-id="a4d37-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.picturepark.com`</span><span class="sxs-lookup"><span data-stu-id="a4d37-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.picturepark.com`</span></span>

    <span data-ttu-id="a4d37-159">b.</span><span class="sxs-lookup"><span data-stu-id="a4d37-159">b.</span></span> <span data-ttu-id="a4d37-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a4d37-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span> 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > <span data-ttu-id="a4d37-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a4d37-161">These values are not real.</span></span> <span data-ttu-id="a4d37-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a4d37-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a4d37-163">Obraťte se na [tým podpory Picturepark klienta](https://picturepark.com/about/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a4d37-163">Contact [Picturepark Client support team](https://picturepark.com/about/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="a4d37-164">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a4d37-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. <span data-ttu-id="a4d37-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4d37-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a4d37-168">Na **Picturepark konfigurace** klikněte na tlačítko **konfigurace Picturepark** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a4d37-168">On the **Picturepark Configuration** section, click **Configure Picturepark** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a4d37-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a4d37-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. <span data-ttu-id="a4d37-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Picturepark.</span><span class="sxs-lookup"><span data-stu-id="a4d37-171">In a different web browser window, log into your Picturepark company site as an administrator.</span></span>

8. <span data-ttu-id="a4d37-172">Na panelu nástrojů v horní části klikněte na tlačítko **nástroje pro správu**a potom klikněte na **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-172">In the toolbar on the top, click **Administrative tools**, and then click **Management Console**.</span></span>
   
    <span data-ttu-id="a4d37-173">![Konzola pro správu](./media/active-directory-saas-picturepark-tutorial/ic795062.png "konzoly pro správu")</span><span class="sxs-lookup"><span data-stu-id="a4d37-173">![Management Console](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span></span>

9. <span data-ttu-id="a4d37-174">Klikněte na tlačítko **ověřování**a potom klikněte na **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-174">Click **Authentication**, and then click **Identity providers**.</span></span>
   
    <span data-ttu-id="a4d37-175">![Ověřování](./media/active-directory-saas-picturepark-tutorial/ic795063.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="a4d37-175">![Authentication](./media/active-directory-saas-picturepark-tutorial/ic795063.png "Authentication")</span></span>

10. <span data-ttu-id="a4d37-176">V **konfigurace zprostředkovatele Identity** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4d37-176">In the **Identity provider configuration** section, perform the following steps:</span></span>
   
    <span data-ttu-id="a4d37-177">![Konfigurace zprostředkovatele identity](./media/active-directory-saas-picturepark-tutorial/ic795064.png "konfigurace zprostředkovatele Identity")</span><span class="sxs-lookup"><span data-stu-id="a4d37-177">![Identity provider configuration](./media/active-directory-saas-picturepark-tutorial/ic795064.png "Identity provider configuration")</span></span>
   
    <span data-ttu-id="a4d37-178">a.</span><span class="sxs-lookup"><span data-stu-id="a4d37-178">a.</span></span> <span data-ttu-id="a4d37-179">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-179">Click **Add**.</span></span>
  
    <span data-ttu-id="a4d37-180">b.</span><span class="sxs-lookup"><span data-stu-id="a4d37-180">b.</span></span> <span data-ttu-id="a4d37-181">Zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a4d37-181">Type a name for your configuration.</span></span>
   
    <span data-ttu-id="a4d37-182">c.</span><span class="sxs-lookup"><span data-stu-id="a4d37-182">c.</span></span> <span data-ttu-id="a4d37-183">Vyberte **nastavit jako výchozí**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-183">Select **Set as default**.</span></span>
   
    <span data-ttu-id="a4d37-184">d.</span><span class="sxs-lookup"><span data-stu-id="a4d37-184">d.</span></span> <span data-ttu-id="a4d37-185">V **vystavitele URI** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a4d37-185">In **Issuer URI** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="a4d37-186">e.</span><span class="sxs-lookup"><span data-stu-id="a4d37-186">e.</span></span> <span data-ttu-id="a4d37-187">V **důvěryhodné vystavitele jezdec tiskových** textovému poli, vložte hodnotu **kryptografický otisk** který jste zkopírovali z **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="a4d37-187">In **Trusted Issuer Thumb Print** textbox, paste the value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span> 

11. <span data-ttu-id="a4d37-188">Klikněte na tlačítko **JoinDefaultUsersGroup**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-188">Click **JoinDefaultUsersGroup**.</span></span>

12. <span data-ttu-id="a4d37-189">Nastavit **Emailaddress** atribut **deklarace identity** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-189">To set the **Emailaddress** attribute in the **Claim** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` and click **Save**.</span></span>

      <span data-ttu-id="a4d37-190">![Konfigurace](./media/active-directory-saas-picturepark-tutorial/ic795065.png "konfigurace")</span><span class="sxs-lookup"><span data-stu-id="a4d37-190">![Configuration](./media/active-directory-saas-picturepark-tutorial/ic795065.png "Configuration")</span></span>

> [!TIP]
> <span data-ttu-id="a4d37-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a4d37-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a4d37-192">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a4d37-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a4d37-193">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a4d37-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a4d37-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4d37-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="a4d37-195">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4d37-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a4d37-197">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4d37-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a4d37-198">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4d37-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a4d37-200">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a4d37-202">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4d37-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a4d37-204">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4d37-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a4d37-206">a.</span><span class="sxs-lookup"><span data-stu-id="a4d37-206">a.</span></span> <span data-ttu-id="a4d37-207">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a4d37-208">b.</span><span class="sxs-lookup"><span data-stu-id="a4d37-208">b.</span></span> <span data-ttu-id="a4d37-209">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a4d37-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a4d37-210">c.</span><span class="sxs-lookup"><span data-stu-id="a4d37-210">c.</span></span> <span data-ttu-id="a4d37-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a4d37-212">d.</span><span class="sxs-lookup"><span data-stu-id="a4d37-212">d.</span></span> <span data-ttu-id="a4d37-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-213">Click **Create**.</span></span>
 
### <a name="creating-a-picturepark-test-user"></a><span data-ttu-id="a4d37-214">Vytvoření zkušebního uživatele Picturepark</span><span class="sxs-lookup"><span data-stu-id="a4d37-214">Creating a Picturepark test user</span></span>

<span data-ttu-id="a4d37-215">Pokud chcete povolit uživatelům Azure AD přihlášení do Picturepark, musí být zřízená do Picturepark.</span><span class="sxs-lookup"><span data-stu-id="a4d37-215">In order to enable Azure AD users to log into Picturepark, they must be provisioned into Picturepark.</span></span> <span data-ttu-id="a4d37-216">V případě Picturepark zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a4d37-216">In the case of Picturepark, provisioning is a manual task.</span></span>

<span data-ttu-id="a4d37-217">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4d37-217">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="a4d37-218">Přihlaste se k vaší **Picturepark** klienta.</span><span class="sxs-lookup"><span data-stu-id="a4d37-218">Log in to your **Picturepark** tenant.</span></span>

2. <span data-ttu-id="a4d37-219">Na panelu nástrojů v horní části klikněte na tlačítko **nástroje pro správu**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-219">In the toolbar on the top, click **Administrative tools**, and then click **Users**.</span></span>
   
    <span data-ttu-id="a4d37-220">![Uživatelé](./media/active-directory-saas-picturepark-tutorial/ic795067.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a4d37-220">![Users](./media/active-directory-saas-picturepark-tutorial/ic795067.png "Users")</span></span>

3. <span data-ttu-id="a4d37-221">V **přehled uživatelů** , klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-221">In the **Users overview** tab, click **New**.</span></span>
   
    <span data-ttu-id="a4d37-222">![Správa uživatelů](./media/active-directory-saas-picturepark-tutorial/ic795068.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a4d37-222">![User management](./media/active-directory-saas-picturepark-tutorial/ic795068.png "User management")</span></span>

4. <span data-ttu-id="a4d37-223">Na **vytvořit uživatele** dialogové okno, proveďte následující kroky platný Azure Active Directory uživatele, který chcete zřídit:</span><span class="sxs-lookup"><span data-stu-id="a4d37-223">On the **Create User** dialog, perform the following steps of a valid Azure Active Directory User you want to provision:</span></span>
   
    <span data-ttu-id="a4d37-224">![Vytvoření uživatele](./media/active-directory-saas-picturepark-tutorial/ic795069.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="a4d37-224">![Create User](./media/active-directory-saas-picturepark-tutorial/ic795069.png "Create User")</span></span>
   
    <span data-ttu-id="a4d37-225">a.</span><span class="sxs-lookup"><span data-stu-id="a4d37-225">a.</span></span> <span data-ttu-id="a4d37-226">V **e-mailovou adresu** textovému poli, typ **e-mailová adresa** uživatele  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a4d37-226">In the **Email Address** textbox, type the **email address** of the user **BrittaSimon@contoso.com**.</span></span>  
   
    <span data-ttu-id="a4d37-227">b.</span><span class="sxs-lookup"><span data-stu-id="a4d37-227">b.</span></span> <span data-ttu-id="a4d37-228">V **heslo** a **Potvrdit heslo** textová pole, zadejte **heslo** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a4d37-228">In the **Password** and **Confirm Password** textboxes, type the **password** of BrittaSimon.</span></span> 
   
    <span data-ttu-id="a4d37-229">c.</span><span class="sxs-lookup"><span data-stu-id="a4d37-229">c.</span></span> <span data-ttu-id="a4d37-230">V **křestní jméno** textovému poli, typ **křestní jméno** uživatele **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-230">In the **First Name** textbox, type the **First Name** of the user **Britta**.</span></span> 
   
    <span data-ttu-id="a4d37-231">d.</span><span class="sxs-lookup"><span data-stu-id="a4d37-231">d.</span></span> <span data-ttu-id="a4d37-232">V **příjmení** textovému poli, typ **příjmení** uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-232">In the **Last Name** textbox, type the **Last Name** of the user **Simon**.</span></span>
   
    <span data-ttu-id="a4d37-233">e.</span><span class="sxs-lookup"><span data-stu-id="a4d37-233">e.</span></span> <span data-ttu-id="a4d37-234">V **společnosti** textovému poli, typ **název společnosti** uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4d37-234">In the **Company** textbox, type the **Company name** of the user.</span></span> 
   
    <span data-ttu-id="a4d37-235">f.</span><span class="sxs-lookup"><span data-stu-id="a4d37-235">f.</span></span> <span data-ttu-id="a4d37-236">V **země** textovému poli, vyberte **země** uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4d37-236">In the **Country** textbox, select the **Country** of the user.</span></span>
  
    <span data-ttu-id="a4d37-237">g.</span><span class="sxs-lookup"><span data-stu-id="a4d37-237">g.</span></span> <span data-ttu-id="a4d37-238">V **ZIP** textovému poli, typ **PSČ** města.</span><span class="sxs-lookup"><span data-stu-id="a4d37-238">In the **ZIP** textbox, type the **ZIP code** of the city.</span></span>
   
    <span data-ttu-id="a4d37-239">h.</span><span class="sxs-lookup"><span data-stu-id="a4d37-239">h.</span></span> <span data-ttu-id="a4d37-240">V **města** textovému poli, typ **název města** uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4d37-240">In the **City** textbox, type the **City name** of the user.</span></span>

    <span data-ttu-id="a4d37-241">i.</span><span class="sxs-lookup"><span data-stu-id="a4d37-241">i.</span></span> <span data-ttu-id="a4d37-242">Vyberte **jazyk**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-242">Select a **Language**.</span></span>
   
    <span data-ttu-id="a4d37-243">j.</span><span class="sxs-lookup"><span data-stu-id="a4d37-243">j.</span></span> <span data-ttu-id="a4d37-244">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-244">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="a4d37-245">Můžete použít všechny ostatní Picturepark uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Picturepark ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a4d37-245">You can use any other Picturepark user account creation tools or APIs provided by Picturepark to provision Azure AD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a4d37-246">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4d37-246">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a4d37-247">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Picturepark.</span><span class="sxs-lookup"><span data-stu-id="a4d37-247">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Picturepark.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a4d37-249">**Pokud chcete přiřadit Britta Simon Picturepark, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4d37-249">**To assign Britta Simon to Picturepark, perform the following steps:**</span></span>

1. <span data-ttu-id="a4d37-250">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-250">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a4d37-252">V seznamu aplikací vyberte **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-252">In the applications list, select **Picturepark**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. <span data-ttu-id="a4d37-254">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a4d37-254">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a4d37-256">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4d37-256">Click **Add** button.</span></span> <span data-ttu-id="a4d37-257">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4d37-257">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a4d37-259">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a4d37-259">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a4d37-260">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4d37-260">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a4d37-261">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4d37-261">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a4d37-262">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4d37-262">Testing single sign-on</span></span>

<span data-ttu-id="a4d37-263">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a4d37-263">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a4d37-264">Když kliknete na dlaždici Picturepark na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Picturepark.</span><span class="sxs-lookup"><span data-stu-id="a4d37-264">When you click the Picturepark tile in the Access Panel, you should get automatically signed-on to your Picturepark application.</span></span> <span data-ttu-id="a4d37-265">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a4d37-265">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4d37-266">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a4d37-266">Additional resources</span></span>

* [<span data-ttu-id="a4d37-267">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a4d37-267">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a4d37-268">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a4d37-268">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

