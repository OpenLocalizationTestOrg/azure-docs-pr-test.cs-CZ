---
title: 'Kurz: Azure Active Directory integrace s Evidence.com | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Evidence.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: a9c474cfc648fc8a306d736c89a4813d82c133ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a><span data-ttu-id="ea5cd-103">Kurz: Azure Active Directory integrace s Evidence.com</span><span class="sxs-lookup"><span data-stu-id="ea5cd-103">Tutorial: Azure Active Directory integration with Evidence.com</span></span>

<span data-ttu-id="ea5cd-104">V tomto kurzu zjistěte, jak integrovat Evidence.com s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ea5cd-104">In this tutorial, you learn how to integrate Evidence.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ea5cd-105">Integrace Evidence.com s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ea5cd-105">Integrating Evidence.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ea5cd-106">Můžete ovládat ve službě Azure AD, který má přístup k Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-106">You can control in Azure AD who has access to Evidence.com.</span></span>
- <span data-ttu-id="ea5cd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Evidence.com (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-107">You can enable your users to automatically get signed-on to Evidence.com (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="ea5cd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="ea5cd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ea5cd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea5cd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ea5cd-110">Prerequisites</span></span>

<span data-ttu-id="ea5cd-111">Konfigurace integrace Azure AD s Evidence.com, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ea5cd-111">To configure Azure AD integration with Evidence.com, you need the following items:</span></span>

- <span data-ttu-id="ea5cd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ea5cd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ea5cd-113">Evidence.com jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ea5cd-113">A Evidence.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ea5cd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ea5cd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ea5cd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ea5cd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ea5cd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ea5cd-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ea5cd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ea5cd-118">Scenario description</span></span>
<span data-ttu-id="ea5cd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ea5cd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ea5cd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ea5cd-121">Přidání Evidence.com z Galerie</span><span class="sxs-lookup"><span data-stu-id="ea5cd-121">Adding Evidence.com from the gallery</span></span>
2. <span data-ttu-id="ea5cd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ea5cd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evidencecom-from-the-gallery"></a><span data-ttu-id="ea5cd-123">Přidání Evidence.com z Galerie</span><span class="sxs-lookup"><span data-stu-id="ea5cd-123">Adding Evidence.com from the gallery</span></span>
<span data-ttu-id="ea5cd-124">Při konfiguraci integrace Evidence.com do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Evidence.com z galerie.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-124">To configure the integration of Evidence.com into Azure AD, you need to add Evidence.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ea5cd-125">**Pokud chcete přidat Evidence.com z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-125">**To add Evidence.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ea5cd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="ea5cd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ea5cd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="ea5cd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="ea5cd-133">Do vyhledávacího pole zadejte **Evidence.com**, vyberte **Evidence.com** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-133">In the search box, type **Evidence.com**, select **Evidence.com** from result panel then click **Add** button to add the application.</span></span>

    ![Evidence.com v seznamu výsledků](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ea5cd-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ea5cd-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ea5cd-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Evidence.com podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ea5cd-136">In this section, you configure and test Azure AD single sign-on with Evidence.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ea5cd-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Evidence.com je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Evidence.com is to a user in Azure AD.</span></span> <span data-ttu-id="ea5cd-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Evidence.com musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-138">In other words, a link relationship between an Azure AD user and the related user in Evidence.com needs to be established.</span></span>

<span data-ttu-id="ea5cd-139">V Evidence.com, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-139">In Evidence.com, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ea5cd-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Evidence.com, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ea5cd-140">To configure and test Azure AD single sign-on with Evidence.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ea5cd-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ea5cd-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ea5cd-143">**[Vytvoření zkušebního uživatele Evidence.com](#create-a-evidencecom-test-user)**  – Pokud chcete mít protějšek Britta Simon v Evidence.com propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-143">**[Create a Evidence.com test user](#create-a-evidencecom-test-user)** - to have a counterpart of Britta Simon in Evidence.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ea5cd-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ea5cd-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ea5cd-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ea5cd-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ea5cd-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Evidence.com application.</span></span>

<span data-ttu-id="ea5cd-148">**Ke konfiguraci Azure AD jednotné přihlašování s Evidence.com, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-148">**To configure Azure AD single sign-on with Evidence.com, perform the following steps:**</span></span>

1. <span data-ttu-id="ea5cd-149">Na portálu Azure na **Evidence.com** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-149">In the Azure portal, on the **Evidence.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="ea5cd-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. <span data-ttu-id="ea5cd-153">Na **Evidence.com domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ea5cd-153">On the **Evidence.com Domain and URLs** section, perform the following steps:</span></span>

    ![Evidence.com domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_url.png)

    <span data-ttu-id="ea5cd-155">a.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-155">a.</span></span> <span data-ttu-id="ea5cd-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="ea5cd-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<yourtenant>.evidence.com`</span></span>

    <span data-ttu-id="ea5cd-157">b.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-157">b.</span></span> <span data-ttu-id="ea5cd-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="ea5cd-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<yourtenant>.evidence.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ea5cd-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-159">These values are not real.</span></span> <span data-ttu-id="ea5cd-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ea5cd-161">Obraťte se na [tým podpory Evidence.com klienta](https://communities.taser.com/support/SupportContactUs?typ=LE) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-161">Contact [Evidence.com Client support team](https://communities.taser.com/support/SupportContactUs?typ=LE) to get these values.</span></span> 

4. <span data-ttu-id="ea5cd-162">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. <span data-ttu-id="ea5cd-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-evidence-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ea5cd-166">Na **Evidence.com konfigurace** klikněte na tlačítko **konfigurace Evidence.com** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-166">On the **Evidence.com Configuration** section, click **Configure Evidence.com** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ea5cd-167">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace evidence.com](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_configure.png) 

7. <span data-ttu-id="ea5cd-169">V okně prohlížeče samostatné webové přihlášení k vaší Evidence.com klienta jako správce a přejděte do **správce** karta</span><span class="sxs-lookup"><span data-stu-id="ea5cd-169">In a separate web browser window, login to your Evidence.com tenant as an administrator and navigate to **Admin** Tab</span></span>

8. <span data-ttu-id="ea5cd-170">Klikněte na **jednotného přihlašování k agentura**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-170">Click on **Agency Single Sign On**</span></span>

9. <span data-ttu-id="ea5cd-171">Vyberte **SAML na základě jednotné přihlašování**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-171">Select **SAML Based Single Sign On**</span></span>

10. <span data-ttu-id="ea5cd-172">Kopírování **SAML Entity ID**, **SAML jeden přihlašování adresa URL služby** a **Sign-Out URL** hodnoty zobrazené na portálu Azure a na odpovídající pole v Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-172">Copy the **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** values shown in the Azure portal and to the corresponding fields in Evidence.com.</span></span>

11. <span data-ttu-id="ea5cd-173">Otevřete v poznámkovém bloku stažený soubor Certificate(Base64), zkopírujte obsah ho do schránky a vložte jej do **certifikát zabezpečení** pole.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-173">Open your downloaded Certificate(Base64) file in notepad, copy the content of it into your clipboard, and then paste it to the **Security Certificate** box.</span></span> 

12. <span data-ttu-id="ea5cd-174">V Evidence.com uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-174">Save the configuration in Evidence.com.</span></span>

> [!TIP]
> <span data-ttu-id="ea5cd-175">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ea5cd-175">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ea5cd-176">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-176">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ea5cd-177">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ea5cd-177">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ea5cd-178">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ea5cd-178">Create an Azure AD test user</span></span>

<span data-ttu-id="ea5cd-179">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-179">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="ea5cd-181">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ea5cd-182">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-182">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-evidence-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="ea5cd-184">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-184">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-evidence-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="ea5cd-186">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-186">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-evidence-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="ea5cd-188">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ea5cd-188">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-evidence-tutorial/create_aaduser_04.png)

    <span data-ttu-id="ea5cd-190">a.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-190">a.</span></span> <span data-ttu-id="ea5cd-191">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-191">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ea5cd-192">b.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-192">b.</span></span> <span data-ttu-id="ea5cd-193">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-193">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="ea5cd-194">c.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-194">c.</span></span> <span data-ttu-id="ea5cd-195">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-195">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="ea5cd-196">d.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-196">d.</span></span> <span data-ttu-id="ea5cd-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-197">Click **Create**.</span></span>
 
### <a name="create-a-evidencecom-test-user"></a><span data-ttu-id="ea5cd-198">Vytvoření zkušebního uživatele Evidence.com</span><span class="sxs-lookup"><span data-stu-id="ea5cd-198">Create a Evidence.com test user</span></span>

<span data-ttu-id="ea5cd-199">Azure AD uživatelé moci přihlásit musí být zřízená pro přístup k uvnitř Evidence.com aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-199">For Azure AD users to be able to sign in, they must be provisioned for access inside the Evidence.com application.</span></span> <span data-ttu-id="ea5cd-200">Tato část popisuje, jak vytvářet uživatelské účty Azure AD uvnitř Evidence.com</span><span class="sxs-lookup"><span data-stu-id="ea5cd-200">This section describes how to create Azure AD user accounts inside Evidence.com</span></span>

<span data-ttu-id="ea5cd-201">**K poskytnutí uživatelského účtu v Evidence.com:**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-201">**To provision a user account in Evidence.com:**</span></span>

1. <span data-ttu-id="ea5cd-202">V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-202">In a web browser window, log into your Evidence.com company site as an administrator.</span></span>

2. <span data-ttu-id="ea5cd-203">Přejděte na **správce** kartě.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-203">Navigate to **Admin** tab.</span></span>

3. <span data-ttu-id="ea5cd-204">Klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-204">Click on **Add User**.</span></span>

4. <span data-ttu-id="ea5cd-205">Klikněte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-205">Click the **Add** button.</span></span>

5. <span data-ttu-id="ea5cd-206">**E-mailovou adresu** uživatele, přidání musí odpovídat uživatelskému jménu uživatele ve službě Azure AD, který chcete poskytnout přístup.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-206">The **Email Address** of the added user must match the username of the users in Azure AD who you wish to give access.</span></span> <span data-ttu-id="ea5cd-207">Pokud uživatelské jméno a e-mailové adresy nejsou stejnou hodnotu ve vaší organizaci, můžete použít **Evidence.com > atributy > jednotné přihlašování** části portálu Azure, chcete-li změnit nameidenitifer posílá Evidence.com jako e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-207">If the username and email address are not the same value in your organization, you can use the **Evidence.com > Attributes > Single Sign-On** section of the Azure portal to change the nameidenitifer sent to Evidence.com to be the email address.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="ea5cd-208">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ea5cd-208">Assign the Azure AD test user</span></span>

<span data-ttu-id="ea5cd-209">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Evidence.com.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="ea5cd-211">**Pokud chcete přiřadit Britta Simon Evidence.com, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ea5cd-211">**To assign Britta Simon to Evidence.com, perform the following steps:**</span></span>

1. <span data-ttu-id="ea5cd-212">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ea5cd-214">V seznamu aplikací vyberte **Evidence.com**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-214">In the applications list, select **Evidence.com**.</span></span>

    ![V seznamu aplikací na Evidence.com odkaz](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_app.png)  

3. <span data-ttu-id="ea5cd-216">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="ea5cd-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-218">Click **Add** button.</span></span> <span data-ttu-id="ea5cd-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="ea5cd-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ea5cd-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ea5cd-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ea5cd-224">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ea5cd-224">Test single sign-on</span></span>

<span data-ttu-id="ea5cd-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ea5cd-226">Když kliknete na dlaždici Evidence.com na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="ea5cd-226">When you click the Evidence.com tile in the Access Panel, you should get automatically signed-on to your Evidence.com application.</span></span>
<span data-ttu-id="ea5cd-227">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ea5cd-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ea5cd-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ea5cd-228">Additional resources</span></span>

* [<span data-ttu-id="ea5cd-229">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ea5cd-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ea5cd-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ea5cd-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_203.png

