---
title: 'Kurz: Azure Active Directory integrace s Neota logiku Studio | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Neota logiku Studio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.openlocfilehash: 99018277392cab44a6b579ad45b4611739a803d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a><span data-ttu-id="491b7-103">Kurz: Azure Active Directory integrace s Neota logiku Studio</span><span class="sxs-lookup"><span data-stu-id="491b7-103">Tutorial: Azure Active Directory integration with Neota Logic Studio</span></span>

<span data-ttu-id="491b7-104">V tomto kurzu zjistěte, jak integrovat Studio logiku Neota s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="491b7-104">In this tutorial, you learn how to integrate Neota Logic Studio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="491b7-105">Integrace s Azure AD Neota logiku Studio poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="491b7-105">Integrating Neota Logic Studio with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="491b7-106">Můžete řídit ve službě Azure AD, který má přístup k Neota logiku Studio</span><span class="sxs-lookup"><span data-stu-id="491b7-106">You can control in Azure AD who has access to Neota Logic Studio</span></span>
- <span data-ttu-id="491b7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Neota logiku Studio (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="491b7-107">You can enable your users to automatically get signed-on to Neota Logic Studio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="491b7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="491b7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="491b7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="491b7-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="491b7-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="491b7-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="491b7-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="491b7-111">Prerequisites</span></span>

<span data-ttu-id="491b7-112">Konfigurace integrace Azure AD s Neota logiku Studio, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="491b7-112">To configure Azure AD integration with Neota Logic Studio, you need the following items:</span></span>

- <span data-ttu-id="491b7-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="491b7-113">An Azure AD subscription</span></span>
- <span data-ttu-id="491b7-114">Neota logiku Studio jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="491b7-114">A Neota Logic Studio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="491b7-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="491b7-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="491b7-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="491b7-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="491b7-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="491b7-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="491b7-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="491b7-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="491b7-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="491b7-119">Scenario description</span></span>

<span data-ttu-id="491b7-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="491b7-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="491b7-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="491b7-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="491b7-122">Přidání logiky Studio Neota z Galerie</span><span class="sxs-lookup"><span data-stu-id="491b7-122">Adding Neota Logic Studio from the gallery</span></span>
2. <span data-ttu-id="491b7-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="491b7-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-neota-logic-studio-from-the-gallery"></a><span data-ttu-id="491b7-124">Přidání logiky Studio Neota z Galerie</span><span class="sxs-lookup"><span data-stu-id="491b7-124">Adding Neota Logic Studio from the gallery</span></span>

<span data-ttu-id="491b7-125">Při konfiguraci integrace Neota logiku Studio do služby Azure AD potřebujete přidat logiku Studio Neota z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="491b7-125">To configure the integration of Neota Logic Studio into Azure AD, you need to add Neota Logic Studio from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="491b7-126">**Pokud chcete přidat logiku Studio Neota z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="491b7-126">**To add Neota Logic Studio from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="491b7-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="491b7-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="491b7-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="491b7-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="491b7-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="491b7-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="491b7-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="491b7-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="491b7-134">Do vyhledávacího pole zadejte **Neota logiku Studio**.</span><span class="sxs-lookup"><span data-stu-id="491b7-134">In the search box, type **Neota Logic Studio**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

5. <span data-ttu-id="491b7-136">Na panelu výsledků vyberte **Neota logiku Studio**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="491b7-136">In the results panel, select **Neota Logic Studio**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="491b7-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="491b7-138">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="491b7-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Neota Studio logiku podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="491b7-139">In this section, you configure and test Azure AD single sign-on with Neota Logic Studio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="491b7-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v Neota logiku Studio je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="491b7-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Neota Logic Studio is to a user in Azure AD.</span></span> <span data-ttu-id="491b7-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Neota logiku Studio musí navázat.</span><span class="sxs-lookup"><span data-stu-id="491b7-141">In other words, a link relationship between an Azure AD user and the related user in Neota Logic Studio needs to be established.</span></span>

<span data-ttu-id="491b7-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Neota logiku Studio.</span><span class="sxs-lookup"><span data-stu-id="491b7-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Neota Logic Studio.</span></span>

<span data-ttu-id="491b7-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Neota logiku Studio, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="491b7-143">To configure and test Azure AD single sign-on with Neota Logic Studio, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="491b7-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="491b7-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="491b7-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="491b7-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="491b7-146">**[Vytvoření zkušebního uživatele Studio logiku Neota](#creating-a-neota-logic-studio-test-user)**  – Pokud chcete mít protějšek Britta Simon v Studio Neota logiku, která je propojený s Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="491b7-146">**[Creating a Neota Logic Studio test user](#creating-a-neota-logic-studio-test-user)** - to have a counterpart of Britta Simon in Neota Logic Studio that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="491b7-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="491b7-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="491b7-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="491b7-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="491b7-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="491b7-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="491b7-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci logiky Studio Neota.</span><span class="sxs-lookup"><span data-stu-id="491b7-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Neota Logic Studio application.</span></span>

<span data-ttu-id="491b7-151">**Ke konfiguraci Azure AD jednotné přihlašování s Studio Neota logiku, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="491b7-151">**To configure Azure AD single sign-on with Neota Logic Studio, perform the following steps:**</span></span>

1. <span data-ttu-id="491b7-152">Na portálu Azure na **Neota logiku Studio** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="491b7-152">In the Azure portal, on the **Neota Logic Studio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="491b7-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="491b7-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

3. <span data-ttu-id="491b7-156">Na **Neota logiku Studio domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="491b7-156">On the **Neota Logic Studio Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    <span data-ttu-id="491b7-158">a.</span><span class="sxs-lookup"><span data-stu-id="491b7-158">a.</span></span> <span data-ttu-id="491b7-159">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<sub domain>.neotalogic.com/a/<sub application>`</span><span class="sxs-lookup"><span data-stu-id="491b7-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub domain>.neotalogic.com/a/<sub application>`</span></span>

    <span data-ttu-id="491b7-160">b.</span><span class="sxs-lookup"><span data-stu-id="491b7-160">b.</span></span> <span data-ttu-id="491b7-161">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<sub domain>.neotalogic.com/wb`</span><span class="sxs-lookup"><span data-stu-id="491b7-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<sub domain>.neotalogic.com/wb`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="491b7-162">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="491b7-162">These values are not the real.</span></span> <span data-ttu-id="491b7-163">Tyto hodnoty aktualizujte s skutečná adresa URL přihlašování & identifikátor.</span><span class="sxs-lookup"><span data-stu-id="491b7-163">Update these values with the actual Sign-On URL & Identifier.</span></span> <span data-ttu-id="491b7-164">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="491b7-164">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="491b7-165">Obraťte se na [tým podpory klienta studia logiku Neota](https://www.neotalogic.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="491b7-165">Contact [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="491b7-166">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="491b7-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

5. <span data-ttu-id="491b7-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="491b7-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="491b7-170">K získání jednotného přihlašování nakonfigurovaný pro vaše aplikace, obraťte se [Neota logiku Studio podpora](https://www.neotalogic.com/contact-us/) týmu a poskytněte s Stáhnout **soubor XML s metadaty** souboru.</span><span class="sxs-lookup"><span data-stu-id="491b7-170">To get SSO configured for your application, Contact [Neota Logic Studio support](https://www.neotalogic.com/contact-us/) team and provide them with downloaded **Metadata XML** file.</span></span>

> [!TIP]
> <span data-ttu-id="491b7-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="491b7-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="491b7-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="491b7-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="491b7-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="491b7-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="491b7-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="491b7-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="491b7-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="491b7-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="491b7-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="491b7-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="491b7-178">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="491b7-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="491b7-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="491b7-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="491b7-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="491b7-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="491b7-184">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="491b7-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="491b7-186">a.</span><span class="sxs-lookup"><span data-stu-id="491b7-186">a.</span></span> <span data-ttu-id="491b7-187">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="491b7-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="491b7-188">b.</span><span class="sxs-lookup"><span data-stu-id="491b7-188">b.</span></span> <span data-ttu-id="491b7-189">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="491b7-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="491b7-190">c.</span><span class="sxs-lookup"><span data-stu-id="491b7-190">c.</span></span> <span data-ttu-id="491b7-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="491b7-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="491b7-192">d.</span><span class="sxs-lookup"><span data-stu-id="491b7-192">d.</span></span> <span data-ttu-id="491b7-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="491b7-193">Click **Create**.</span></span>
 
### <a name="creating-a-neota-logic-studio-test-user"></a><span data-ttu-id="491b7-194">Vytvoření zkušebního uživatele Neota logiku Studio</span><span class="sxs-lookup"><span data-stu-id="491b7-194">Creating a Neota Logic Studio test user</span></span>

<span data-ttu-id="491b7-195">V této části vytvoříte uživatele volal Britta Simon v Neota logiku Studio.</span><span class="sxs-lookup"><span data-stu-id="491b7-195">In this section, you create a user called Britta Simon in Neota Logic Studio.</span></span> <span data-ttu-id="491b7-196">Práce s [tým podpory klienta studia logiku Neota](https://www.neotalogic.com/contact-us/) v platformě Neota logiku Studio přidejte uživatele.</span><span class="sxs-lookup"><span data-stu-id="491b7-196">work with [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to add the users in the Neota Logic Studio platform.</span></span> <span data-ttu-id="491b7-197">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="491b7-197">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="491b7-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="491b7-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="491b7-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Neota logiku Studio.</span><span class="sxs-lookup"><span data-stu-id="491b7-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Neota Logic Studio.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="491b7-201">**Pokud chcete přiřadit Britta Simon Studio Neota logiku, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="491b7-201">**To assign Britta Simon to Neota Logic Studio, perform the following steps:**</span></span>

1. <span data-ttu-id="491b7-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="491b7-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="491b7-204">V seznamu aplikací vyberte **Neota logiku Studio**.</span><span class="sxs-lookup"><span data-stu-id="491b7-204">In the applications list, select **Neota Logic Studio**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

3. <span data-ttu-id="491b7-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="491b7-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="491b7-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="491b7-208">Click **Add** button.</span></span> <span data-ttu-id="491b7-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="491b7-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="491b7-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="491b7-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="491b7-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="491b7-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="491b7-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="491b7-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="491b7-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="491b7-214">Testing single sign-on</span></span>

<span data-ttu-id="491b7-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="491b7-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="491b7-216">Klikněte na dlaždici Neota logiku Studio na přístupovém panelu, budete přesměrováni na stránku přihlášení organizace.</span><span class="sxs-lookup"><span data-stu-id="491b7-216">Click the Neota Logic Studio tile in the Access Panel, you will be redirected to Organization sign-on page.</span></span> <span data-ttu-id="491b7-217">Po úspěšném přihlášení můžete se být přihlášení k aplikaci logiky Studio Neota.</span><span class="sxs-lookup"><span data-stu-id="491b7-217">After successful login, you will be signed-on to your Neota Logic Studio application.</span></span> <span data-ttu-id="491b7-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="491b7-218">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

<span data-ttu-id="491b7-219">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="491b7-219">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="491b7-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="491b7-220">Additional resources</span></span>

* [<span data-ttu-id="491b7-221">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="491b7-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="491b7-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="491b7-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_203.png

