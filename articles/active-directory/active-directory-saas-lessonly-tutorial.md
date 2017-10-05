---
title: 'Kurz: Azure Active Directory integrace s Lesson.ly | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lesson.ly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fc1e1b2de0a138dbe88d794f802b002321948ab8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a><span data-ttu-id="9e057-103">Kurz: Azure Active Directory integrace s Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="9e057-103">Tutorial: Azure Active Directory integration with Lesson.ly</span></span>

<span data-ttu-id="9e057-104">V tomto kurzu zjistěte, jak integrovat Lesson.ly s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9e057-104">In this tutorial, you learn how to integrate Lesson.ly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9e057-105">Integrace Lesson.ly s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9e057-105">Integrating Lesson.ly with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9e057-106">Můžete řídit ve službě Azure AD, který má přístup k Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="9e057-106">You can control in Azure AD who has access to Lesson.ly</span></span>
- <span data-ttu-id="9e057-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Lesson.ly (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e057-107">You can enable your users to automatically get signed-on to Lesson.ly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9e057-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9e057-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9e057-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9e057-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e057-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e057-110">Prerequisites</span></span>

<span data-ttu-id="9e057-111">Konfigurace integrace Azure AD s Lesson.ly, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9e057-111">To configure Azure AD integration with Lesson.ly, you need the following items:</span></span>

- <span data-ttu-id="9e057-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e057-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9e057-113">Lesson.ly jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9e057-113">A Lesson.ly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9e057-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e057-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9e057-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9e057-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9e057-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9e057-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9e057-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9e057-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9e057-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9e057-118">Scenario description</span></span>
<span data-ttu-id="9e057-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e057-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9e057-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9e057-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9e057-121">Přidání Lesson.ly z Galerie</span><span class="sxs-lookup"><span data-stu-id="9e057-121">Adding Lesson.ly from the gallery</span></span>
2. <span data-ttu-id="9e057-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e057-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lessonly-from-the-gallery"></a><span data-ttu-id="9e057-123">Přidání Lesson.ly z Galerie</span><span class="sxs-lookup"><span data-stu-id="9e057-123">Adding Lesson.ly from the gallery</span></span>
<span data-ttu-id="9e057-124">Při konfiguraci integrace Lesson.ly do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Lesson.ly z galerie.</span><span class="sxs-lookup"><span data-stu-id="9e057-124">To configure the integration of Lesson.ly into Azure AD, you need to add Lesson.ly from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9e057-125">**Pokud chcete přidat Lesson.ly z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e057-125">**To add Lesson.ly from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9e057-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9e057-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9e057-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9e057-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9e057-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e057-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9e057-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e057-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9e057-133">Do vyhledávacího pole zadejte **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="9e057-133">In the search box, type **Lesson.ly**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_search.png)

5. <span data-ttu-id="9e057-135">Na panelu výsledků vyberte **Lesson.ly**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e057-135">In the results panel, select **Lesson.ly**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9e057-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e057-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9e057-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lesson.ly podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9e057-138">In this section, you configure and test Azure AD single sign-on with Lesson.ly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9e057-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Lesson.ly je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e057-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lesson.ly is to a user in Azure AD.</span></span> <span data-ttu-id="9e057-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Lesson.ly musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9e057-140">In other words, a link relationship between an Azure AD user and the related user in Lesson.ly needs to be established.</span></span>

<span data-ttu-id="9e057-141">V Lesson.ly, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="9e057-141">In Lesson.ly, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9e057-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lesson.ly, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9e057-142">To configure and test Azure AD single sign-on with Lesson.ly, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9e057-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9e057-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9e057-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e057-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9e057-145">**[Vytvoření zkušebního uživatele Lesson.ly](#creating-a-lessonly-test-user)**  – Pokud chcete mít protějšek Britta Simon v Lesson.ly propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9e057-145">**[Creating a Lesson.ly test user](#creating-a-lessonly-test-user)** - to have a counterpart of Britta Simon in Lesson.ly that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9e057-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e057-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9e057-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9e057-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9e057-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e057-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9e057-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="9e057-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lesson.ly application.</span></span>

<span data-ttu-id="9e057-150">**Ke konfiguraci Azure AD jednotné přihlašování s Lesson.ly, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e057-150">**To configure Azure AD single sign-on with Lesson.ly, perform the following steps:**</span></span>

1. <span data-ttu-id="9e057-151">Na portálu Azure na **Lesson.ly** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9e057-151">In the Azure portal, on the **Lesson.ly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9e057-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e057-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_samlbase.png)

3. <span data-ttu-id="9e057-155">Na **Lesson.ly domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9e057-155">On the **Lesson.ly Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_url.png)

    <span data-ttu-id="9e057-157">a.</span><span class="sxs-lookup"><span data-stu-id="9e057-157">a.</span></span> <span data-ttu-id="9e057-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="9e057-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/signin`|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    ><span data-ttu-id="9e057-159">Při odkazování na obecný názvů, které **NázevSpolečnosti** potřebuje nahradit skutečným názvem.</span><span class="sxs-lookup"><span data-stu-id="9e057-159">When referencing a generic name that **companyname** needs to be replaced by an actual name.</span></span>
    
    <span data-ttu-id="9e057-160">b.</span><span class="sxs-lookup"><span data-stu-id="9e057-160">b.</span></span> <span data-ttu-id="9e057-161">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="9e057-161">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/auth/saml/metadata`|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > <span data-ttu-id="9e057-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9e057-162">These values are not real.</span></span> <span data-ttu-id="9e057-163">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9e057-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9e057-164">Obraťte se na [tým podpory Lesson.ly klienta](mailto:dev@lessonly.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="9e057-164">Contact [Lesson.ly Client support team](mailto:dev@lessonly.com) to get these values.</span></span> 

4. <span data-ttu-id="9e057-165">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9e057-165">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_certificate.png)

5. <span data-ttu-id="9e057-167">Aplikace Lesson.ly očekává SAML kontrolní výrazy ve specifickém formátu, který můžete přidat mapování vlastní atribut vyžaduje vaše **atributy tokenu SAML** konfigurace. Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="9e057-167">The Lesson.ly application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png)
           
6. <span data-ttu-id="9e057-169">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je vidět na předchozím obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9e057-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>

    | <span data-ttu-id="9e057-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="9e057-170">Attribute Name</span></span>   | <span data-ttu-id="9e057-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="9e057-171">Attribute Value</span></span> |
    | ---------------  | ----------------|
    | <span data-ttu-id="9e057-172">název urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="9e057-172">urn:oid:2.5.4.42</span></span> |<span data-ttu-id="9e057-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="9e057-173">user.givenname</span></span> |
    | <span data-ttu-id="9e057-174">název urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="9e057-174">urn:oid:2.5.4.4</span></span>  |<span data-ttu-id="9e057-175">User.Surname</span><span class="sxs-lookup"><span data-stu-id="9e057-175">user.surname</span></span> |
    | <span data-ttu-id="9e057-176">název urn: oid:0.9.2342.19200300.1001.3</span><span class="sxs-lookup"><span data-stu-id="9e057-176">urn:oid:0.9.2342.19200300.1001.3</span></span> |<span data-ttu-id="9e057-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="9e057-177">user.mail</span></span> |

    <span data-ttu-id="9e057-178">a.</span><span class="sxs-lookup"><span data-stu-id="9e057-178">a.</span></span> <span data-ttu-id="9e057-179">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e057-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9e057-182">b.</span><span class="sxs-lookup"><span data-stu-id="9e057-182">b.</span></span> <span data-ttu-id="9e057-183">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="9e057-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="9e057-184">c.</span><span class="sxs-lookup"><span data-stu-id="9e057-184">c.</span></span> <span data-ttu-id="9e057-185">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="9e057-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="9e057-186">d.</span><span class="sxs-lookup"><span data-stu-id="9e057-186">d.</span></span> <span data-ttu-id="9e057-187">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e057-187">Click **Ok**.</span></span>     

7. <span data-ttu-id="9e057-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e057-188">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9e057-190">Na **Lesson.ly konfigurace** klikněte na tlačítko **konfigurace Lesson.ly** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9e057-190">On the **Lesson.ly Configuration** section, click **Configure Lesson.ly** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9e057-191">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9e057-191">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_configure.png)

9. <span data-ttu-id="9e057-193">Konfigurace jednotného přihlašování na **Lesson.ly** straně, budete muset odeslat stažené **Certificate(Base64)** a **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory Lesson.ly](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="9e057-193">To configure single sign-on on **Lesson.ly** side, you need to send the downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

> [!TIP]
> <span data-ttu-id="9e057-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9e057-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9e057-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9e057-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9e057-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9e057-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9e057-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e057-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="9e057-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e057-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9e057-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e057-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9e057-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9e057-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9e057-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9e057-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9e057-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e057-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9e057-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9e057-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9e057-209">a.</span><span class="sxs-lookup"><span data-stu-id="9e057-209">a.</span></span> <span data-ttu-id="9e057-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9e057-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9e057-211">b.</span><span class="sxs-lookup"><span data-stu-id="9e057-211">b.</span></span> <span data-ttu-id="9e057-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9e057-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9e057-213">c.</span><span class="sxs-lookup"><span data-stu-id="9e057-213">c.</span></span> <span data-ttu-id="9e057-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9e057-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9e057-215">d.</span><span class="sxs-lookup"><span data-stu-id="9e057-215">d.</span></span> <span data-ttu-id="9e057-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9e057-216">Click **Create**.</span></span>
 
### <a name="creating-a-lessonly-test-user"></a><span data-ttu-id="9e057-217">Vytvoření zkušebního uživatele Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="9e057-217">Creating a Lesson.ly test user</span></span>

<span data-ttu-id="9e057-218">Cílem této části je vytvoření uživatele v Lesson.ly nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e057-218">The objective of this section is to create a user called Britta Simon in Lesson.ly.</span></span> <span data-ttu-id="9e057-219">Lesson.ly podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="9e057-219">Lesson.ly supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="9e057-220">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="9e057-220">There is no action item for you in this section.</span></span> <span data-ttu-id="9e057-221">Vytvoří se nový uživatel během pokusu o přístup k Lesson.ly, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="9e057-221">A new user will be created during an attempt to access Lesson.ly if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="9e057-222">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktovat [tým podpory Lesson.ly](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="9e057-222">If you need to create an user manually, you need to contact the [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9e057-223">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e057-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9e057-224">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="9e057-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lesson.ly.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9e057-226">**Pokud chcete přiřadit Britta Simon Lesson.ly, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e057-226">**To assign Britta Simon to Lesson.ly, perform the following steps:**</span></span>

1. <span data-ttu-id="9e057-227">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e057-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9e057-229">V seznamu aplikací vyberte **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="9e057-229">In the applications list, select **Lesson.ly**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_app.png) 

3. <span data-ttu-id="9e057-231">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9e057-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9e057-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e057-233">Click **Add** button.</span></span> <span data-ttu-id="9e057-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e057-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9e057-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9e057-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9e057-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e057-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9e057-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e057-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9e057-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e057-239">Testing single sign-on</span></span>

<span data-ttu-id="9e057-240">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9e057-240">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9e057-241">Když kliknete na dlaždici Lesson.ly na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="9e057-241">When you click the Lesson.ly tile in the Access Panel, you should get automatically signed-on to your Lesson.ly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e057-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9e057-242">Additional resources</span></span>

* [<span data-ttu-id="9e057-243">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9e057-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9e057-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9e057-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png

