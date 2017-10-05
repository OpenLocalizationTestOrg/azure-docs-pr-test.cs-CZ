---
title: 'Kurz: Azure Active Directory integrace s IdeaScale | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a IdeaScale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 88099e942319f16dd721da83e4e69b8fcb836c0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a><span data-ttu-id="470f9-103">Kurz: Azure Active Directory integrace s IdeaScale</span><span class="sxs-lookup"><span data-stu-id="470f9-103">Tutorial: Azure Active Directory integration with IdeaScale</span></span>

<span data-ttu-id="470f9-104">V tomto kurzu zjistěte, jak integrovat IdeaScale s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="470f9-104">In this tutorial, you learn how to integrate IdeaScale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="470f9-105">Integrace IdeaScale s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="470f9-105">Integrating IdeaScale with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="470f9-106">Můžete řídit ve službě Azure AD, který má přístup k IdeaScale</span><span class="sxs-lookup"><span data-stu-id="470f9-106">You can control in Azure AD who has access to IdeaScale</span></span>
- <span data-ttu-id="470f9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k IdeaScale (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="470f9-107">You can enable your users to automatically get signed-on to IdeaScale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="470f9-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="470f9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="470f9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="470f9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="470f9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="470f9-110">Prerequisites</span></span>

<span data-ttu-id="470f9-111">Konfigurace integrace Azure AD s IdeaScale, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="470f9-111">To configure Azure AD integration with IdeaScale, you need the following items:</span></span>

- <span data-ttu-id="470f9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="470f9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="470f9-113">IdeaScale jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="470f9-113">An IdeaScale single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="470f9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="470f9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="470f9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="470f9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="470f9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="470f9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="470f9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="470f9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="470f9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="470f9-118">Scenario description</span></span>
<span data-ttu-id="470f9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="470f9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="470f9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="470f9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="470f9-121">Přidání IdeaScale z Galerie</span><span class="sxs-lookup"><span data-stu-id="470f9-121">Adding IdeaScale from the gallery</span></span>
2. <span data-ttu-id="470f9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="470f9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ideascale-from-the-gallery"></a><span data-ttu-id="470f9-123">Přidání IdeaScale z Galerie</span><span class="sxs-lookup"><span data-stu-id="470f9-123">Adding IdeaScale from the gallery</span></span>
<span data-ttu-id="470f9-124">Při konfiguraci integrace IdeaScale do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS IdeaScale z galerie.</span><span class="sxs-lookup"><span data-stu-id="470f9-124">To configure the integration of IdeaScale into Azure AD, you need to add IdeaScale from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="470f9-125">**Pokud chcete přidat IdeaScale z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="470f9-125">**To add IdeaScale from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="470f9-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="470f9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="470f9-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="470f9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="470f9-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="470f9-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="470f9-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="470f9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="470f9-133">Do vyhledávacího pole zadejte **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="470f9-133">In the search box, type **IdeaScale**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. <span data-ttu-id="470f9-135">Na panelu výsledků vyberte **IdeaScale**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="470f9-135">In the results panel, select **IdeaScale**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="470f9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="470f9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="470f9-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s IdeaScale podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="470f9-138">In this section, you configure and test Azure AD single sign-on with IdeaScale based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="470f9-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v IdeaScale je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="470f9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in IdeaScale is to a user in Azure AD.</span></span> <span data-ttu-id="470f9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v IdeaScale musí navázat.</span><span class="sxs-lookup"><span data-stu-id="470f9-140">In other words, a link relationship between an Azure AD user and the related user in IdeaScale needs to be established.</span></span>

<span data-ttu-id="470f9-141">V IdeaScale, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="470f9-141">In IdeaScale, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="470f9-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s IdeaScale, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="470f9-142">To configure and test Azure AD single sign-on with IdeaScale, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="470f9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="470f9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="470f9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="470f9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="470f9-145">**[Vytváření testovacího uživatele IdeaScale](#creating-an-ideascale-test-user)**  – Pokud chcete mít protějšek Britta Simon v IdeaScale propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="470f9-145">**[Creating an IdeaScale test user](#creating-an-ideascale-test-user)** - to have a counterpart of Britta Simon in IdeaScale that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="470f9-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="470f9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="470f9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="470f9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="470f9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="470f9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="470f9-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci IdeaScale.</span><span class="sxs-lookup"><span data-stu-id="470f9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your IdeaScale application.</span></span>

<span data-ttu-id="470f9-150">**Ke konfiguraci Azure AD jednotné přihlašování s IdeaScale, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="470f9-150">**To configure Azure AD single sign-on with IdeaScale, perform the following steps:**</span></span>

1. <span data-ttu-id="470f9-151">Na portálu Azure na **IdeaScale** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="470f9-151">In the Azure portal, on the **IdeaScale** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="470f9-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="470f9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. <span data-ttu-id="470f9-155">Na **IdeaScale domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="470f9-155">On the **IdeaScale Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    <span data-ttu-id="470f9-157">a.</span><span class="sxs-lookup"><span data-stu-id="470f9-157">a.</span></span> <span data-ttu-id="470f9-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.ideascale.com`</span><span class="sxs-lookup"><span data-stu-id="470f9-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.ideascale.com`</span></span>

    <span data-ttu-id="470f9-159">b.</span><span class="sxs-lookup"><span data-stu-id="470f9-159">b.</span></span> <span data-ttu-id="470f9-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="470f9-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > <span data-ttu-id="470f9-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="470f9-161">These values are not real.</span></span> <span data-ttu-id="470f9-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="470f9-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="470f9-163">Obraťte se na [tým podpory IdeaScale klienta](http://support.ideascale.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="470f9-163">Contact [IdeaScale Client support team](http://support.ideascale.com/) to get these values.</span></span> 
 
4. <span data-ttu-id="470f9-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="470f9-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. <span data-ttu-id="470f9-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="470f9-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="470f9-168">Na **IdeaScale konfigurace** klikněte na tlačítko **konfigurace IdeaScale** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="470f9-168">On the **IdeaScale Configuration** section, click **Configure IdeaScale** to open **Configure sign-on** window.</span></span> <span data-ttu-id="470f9-169">Kopírování **Sign-Out adresy URL a SAML Entity ID** z **Stručná referenční příručka části**.</span><span class="sxs-lookup"><span data-stu-id="470f9-169">Copy the **Sign-Out URL, and SAML Entity ID** from the **Quick Reference section**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. <span data-ttu-id="470f9-171">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti IdeaScale jako správce.</span><span class="sxs-lookup"><span data-stu-id="470f9-171">In a different web browser window, log in to your IdeaScale company site as an administrator.</span></span>

8. <span data-ttu-id="470f9-172">Přejděte na **komunity nastavení**.</span><span class="sxs-lookup"><span data-stu-id="470f9-172">Go to **Community Settings**.</span></span>
   
    <span data-ttu-id="470f9-173">![Nastavení komunity](./media/active-directory-saas-ideascale-tutorial/ic790847.png "nastavení komunity")</span><span class="sxs-lookup"><span data-stu-id="470f9-173">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

9. <span data-ttu-id="470f9-174">Přejděte na **zabezpečení \> jednoduchého nastavení Sign-on**.</span><span class="sxs-lookup"><span data-stu-id="470f9-174">Go to **Security \> Single Signon Settings**.</span></span>
   
    <span data-ttu-id="470f9-175">![Jednoduchého nastavení Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790848.png "jednoduchého nastavení Sign-on")</span><span class="sxs-lookup"><span data-stu-id="470f9-175">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790848.png "Single Signon Settings")</span></span>

10. <span data-ttu-id="470f9-176">Jako **typu Single-Sign-on**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="470f9-176">As **Single-Signon Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="470f9-177">![Jeden typ Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790849.png "jeden typ Sign-on")</span><span class="sxs-lookup"><span data-stu-id="470f9-177">![Single Signon Type](./media/active-directory-saas-ideascale-tutorial/ic790849.png "Single Signon Type")</span></span>

11. <span data-ttu-id="470f9-178">Na **nastavení jednotného Sign-on** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="470f9-178">On the **Single Signon Settings** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="470f9-179">![Jednoduchého nastavení Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790850.png "jednoduchého nastavení Sign-on")</span><span class="sxs-lookup"><span data-stu-id="470f9-179">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790850.png "Single Signon Settings")</span></span>
   
    <span data-ttu-id="470f9-180">a.</span><span class="sxs-lookup"><span data-stu-id="470f9-180">a.</span></span> <span data-ttu-id="470f9-181">V **SAML IdP Entity ID** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="470f9-181">In **SAML IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="470f9-182">b.</span><span class="sxs-lookup"><span data-stu-id="470f9-182">b.</span></span> <span data-ttu-id="470f9-183">Kopírovat obsah souboru metadat stažený z portálu Azure a vložte ji do **SAML IdP Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="470f9-183">Copy the content of your downloaded metadata file from Azure portal, and paste it into the **SAML IdP Metadata** textbox.</span></span>

    <span data-ttu-id="470f9-184">c.</span><span class="sxs-lookup"><span data-stu-id="470f9-184">c.</span></span> <span data-ttu-id="470f9-185">V **adresy URL odhlašovací úspěch** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="470f9-185">In **Logout Success URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="470f9-186">d.</span><span class="sxs-lookup"><span data-stu-id="470f9-186">d.</span></span> <span data-ttu-id="470f9-187">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="470f9-187">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="470f9-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="470f9-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="470f9-189">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="470f9-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="470f9-190">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="470f9-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="470f9-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="470f9-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="470f9-192">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="470f9-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="470f9-194">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="470f9-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="470f9-195">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="470f9-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="470f9-197">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="470f9-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="470f9-199">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="470f9-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="470f9-201">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="470f9-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="470f9-203">a.</span><span class="sxs-lookup"><span data-stu-id="470f9-203">a.</span></span> <span data-ttu-id="470f9-204">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="470f9-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="470f9-205">b.</span><span class="sxs-lookup"><span data-stu-id="470f9-205">b.</span></span> <span data-ttu-id="470f9-206">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="470f9-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="470f9-207">c.</span><span class="sxs-lookup"><span data-stu-id="470f9-207">c.</span></span> <span data-ttu-id="470f9-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="470f9-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="470f9-209">d.</span><span class="sxs-lookup"><span data-stu-id="470f9-209">d.</span></span> <span data-ttu-id="470f9-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="470f9-210">Click **Create**.</span></span>
 
### <a name="creating-an-ideascale-test-user"></a><span data-ttu-id="470f9-211">Vytváření testovacího uživatele IdeaScale</span><span class="sxs-lookup"><span data-stu-id="470f9-211">Creating an IdeaScale test user</span></span>

<span data-ttu-id="470f9-212">Pokud chcete povolit uživatelům Azure AD přihlášení do IdeaScale, se musí být zřízená v k IdeaScale.</span><span class="sxs-lookup"><span data-stu-id="470f9-212">To enable Azure AD users to log into IdeaScale, they must be provisioned in to IdeaScale.</span></span> <span data-ttu-id="470f9-213">V případě IdeaScale zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="470f9-213">In the case of IdeaScale, provisioning is a manual task.</span></span>

<span data-ttu-id="470f9-214">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="470f9-214">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="470f9-215">Přihlaste se k vaší **IdeaScale** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="470f9-215">Log in to your **IdeaScale** company site as administrator.</span></span>

2. <span data-ttu-id="470f9-216">Přejděte na **komunity nastavení**.</span><span class="sxs-lookup"><span data-stu-id="470f9-216">Go to **Community Settings**.</span></span>
   
    <span data-ttu-id="470f9-217">![Nastavení komunity](./media/active-directory-saas-ideascale-tutorial/ic790847.png "nastavení komunity")</span><span class="sxs-lookup"><span data-stu-id="470f9-217">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

3. <span data-ttu-id="470f9-218">Přejděte na **základní nastavení \> člen správu**.</span><span class="sxs-lookup"><span data-stu-id="470f9-218">Go to **Basic Settings \> Member Management**.</span></span>

4. <span data-ttu-id="470f9-219">Klikněte na tlačítko **přidat člena**.</span><span class="sxs-lookup"><span data-stu-id="470f9-219">Click **Add Member**.</span></span>
   
    <span data-ttu-id="470f9-220">![Člen správu](./media/active-directory-saas-ideascale-tutorial/ic790852.png "člen správy")</span><span class="sxs-lookup"><span data-stu-id="470f9-220">![Member Management](./media/active-directory-saas-ideascale-tutorial/ic790852.png "Member Management")</span></span>

5. <span data-ttu-id="470f9-221">V části Přidání nového člena proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="470f9-221">In the Add New Member section, perform the following steps:</span></span>
   
    <span data-ttu-id="470f9-222">![Přidání nového člena](./media/active-directory-saas-ideascale-tutorial/ic790853.png "přidání nového člena")</span><span class="sxs-lookup"><span data-stu-id="470f9-222">![Add New Member](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Add New Member")</span></span>
   
    <span data-ttu-id="470f9-223">a.</span><span class="sxs-lookup"><span data-stu-id="470f9-223">a.</span></span> <span data-ttu-id="470f9-224">V **e-mailové adresy** textovému poli, zadejte e-mailovou adresu chcete zřídit platného účtu AAD.</span><span class="sxs-lookup"><span data-stu-id="470f9-224">In the **Email Addresses** textbox, type the email address of a valid AAD account you want to provision.</span></span>
   
    <span data-ttu-id="470f9-225">b.</span><span class="sxs-lookup"><span data-stu-id="470f9-225">b.</span></span> <span data-ttu-id="470f9-226">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="470f9-226">Click **Save Changes**.</span></span> 
   
    >[!NOTE]
    ><span data-ttu-id="470f9-227">Držitel účtu Azure Active Directory získá e-mail s odkazem pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="470f9-227">The Azure Active Directory account holder gets an email with a link to confirm the account before it becomes active.</span></span>
      
>[!NOTE]
><span data-ttu-id="470f9-228">Můžete použít všechny ostatní IdeaScale uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované IdeaScale zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="470f9-228">You can use any other IdeaScale user account creation tools or APIs provided by IdeaScale to provision AAD user accounts.</span></span>
 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="470f9-229">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="470f9-229">Assigning the Azure AD test user</span></span>

<span data-ttu-id="470f9-230">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu IdeaScale.</span><span class="sxs-lookup"><span data-stu-id="470f9-230">In this section, you enable Britta Simon to use Azure single sign-on by granting access to IdeaScale.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="470f9-232">**Pokud chcete přiřadit Britta Simon IdeaScale, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="470f9-232">**To assign Britta Simon to IdeaScale, perform the following steps:**</span></span>

1. <span data-ttu-id="470f9-233">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="470f9-233">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="470f9-235">V seznamu aplikací vyberte **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="470f9-235">In the applications list, select **IdeaScale**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. <span data-ttu-id="470f9-237">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="470f9-237">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="470f9-239">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="470f9-239">Click **Add** button.</span></span> <span data-ttu-id="470f9-240">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="470f9-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="470f9-242">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="470f9-242">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="470f9-243">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="470f9-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="470f9-244">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="470f9-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="470f9-245">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="470f9-245">Testing single sign-on</span></span>


<span data-ttu-id="470f9-246">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="470f9-246">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="470f9-247">Když kliknete na dlaždici IdeaScale na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci IdeaScale.</span><span class="sxs-lookup"><span data-stu-id="470f9-247">When you click the IdeaScale tile in the Access Panel, you should get automatically signed-on to your IdeaScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="470f9-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="470f9-248">Additional resources</span></span>

* [<span data-ttu-id="470f9-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="470f9-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="470f9-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="470f9-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

