---
title: "Kurz: Azure Active Directory integrace s spolupráce inovací | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a spolupráce inovace."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 5706ba9f4e7c92de77a0edc5146aa150de379c9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a><span data-ttu-id="5c8d0-103">Kurz: Azure Active Directory integrace s inovací spolupráce</span><span class="sxs-lookup"><span data-stu-id="5c8d0-103">Tutorial: Azure Active Directory integration with Collaborative Innovation</span></span>

<span data-ttu-id="5c8d0-104">V tomto kurzu zjistěte, jak integrovat spolupráce inovace v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5c8d0-104">In this tutorial, you learn how to integrate Collaborative Innovation with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5c8d0-105">Integrace spolupráce inovací s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5c8d0-105">Integrating Collaborative Innovation with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5c8d0-106">Můžete řídit ve službě Azure AD, který má přístup ke spolupráci inovací</span><span class="sxs-lookup"><span data-stu-id="5c8d0-106">You can control in Azure AD who has access to Collaborative Innovation</span></span>
- <span data-ttu-id="5c8d0-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k spolupráce inovací (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c8d0-107">You can enable your users to automatically get signed-on to Collaborative Innovation (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5c8d0-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5c8d0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5c8d0-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5c8d0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c8d0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5c8d0-110">Prerequisites</span></span>

<span data-ttu-id="5c8d0-111">Konfigurace integrace Azure AD s spolupráce inovací, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5c8d0-111">To configure Azure AD integration with Collaborative Innovation, you need the following items:</span></span>

- <span data-ttu-id="5c8d0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c8d0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5c8d0-113">Spolupráce inovací jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5c8d0-113">A Collaborative Innovation single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5c8d0-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5c8d0-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5c8d0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5c8d0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5c8d0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c8d0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5c8d0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5c8d0-118">Scenario description</span></span>
<span data-ttu-id="5c8d0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5c8d0-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5c8d0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5c8d0-121">Přidání spolupráce inovací z Galerie</span><span class="sxs-lookup"><span data-stu-id="5c8d0-121">Adding Collaborative Innovation from the gallery</span></span>
2. <span data-ttu-id="5c8d0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5c8d0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-collaborative-innovation-from-the-gallery"></a><span data-ttu-id="5c8d0-123">Přidání spolupráce inovací z Galerie</span><span class="sxs-lookup"><span data-stu-id="5c8d0-123">Adding Collaborative Innovation from the gallery</span></span>
<span data-ttu-id="5c8d0-124">Při konfiguraci integrace spolupráce inovací do služby Azure AD potřebujete přidat spolupráce inovací z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-124">To configure the integration of Collaborative Innovation into Azure AD, you need to add Collaborative Innovation from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5c8d0-125">**Pokud chcete přidat spolupráce inovací z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5c8d0-125">**To add Collaborative Innovation from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5c8d0-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5c8d0-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5c8d0-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5c8d0-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5c8d0-133">Do vyhledávacího pole zadejte **spolupráce inovací**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-133">In the search box, type **Collaborative Innovation**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. <span data-ttu-id="5c8d0-135">Na panelu výsledků vyberte **spolupráce inovací**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-135">In the results panel, select **Collaborative Innovation**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5c8d0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5c8d0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5c8d0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s spolupráce inovací podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5c8d0-138">In this section, you configure and test Azure AD single sign-on with Collaborative Innovation based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5c8d0-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v spolupráce inovací je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Collaborative Innovation is to a user in Azure AD.</span></span> <span data-ttu-id="5c8d0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v spolupráce inovací musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-140">In other words, a link relationship between an Azure AD user and the related user in Collaborative Innovation needs to be established.</span></span>

<span data-ttu-id="5c8d0-141">V spolupráce inovací přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-141">In Collaborative Innovation, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5c8d0-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s spolupráce inovací, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5c8d0-142">To configure and test Azure AD single sign-on with Collaborative Innovation, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5c8d0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5c8d0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5c8d0-145">**[Vytvoření zkušebního uživatele spolupráce inovací](#creating-a-collaborative-innovation-test-user)**  – Pokud chcete mít protějšek Britta Simon v spolupráce novinka, kterou je propojena k reprezentaci Azure AD uživatele.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-145">**[Creating a Collaborative Innovation test user](#creating-a-collaborative-innovation-test-user)** - to have a counterpart of Britta Simon in Collaborative Innovation that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5c8d0-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5c8d0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5c8d0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5c8d0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5c8d0-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci spolupráce inovace.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Collaborative Innovation application.</span></span>

<span data-ttu-id="5c8d0-150">**Ke konfiguraci Azure AD jednotné přihlašování s spolupráce inovací, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5c8d0-150">**To configure Azure AD single sign-on with Collaborative Innovation, perform the following steps:**</span></span>

1. <span data-ttu-id="5c8d0-151">Na portálu Azure na **spolupráce inovací** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-151">In the Azure portal, on the **Collaborative Innovation** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5c8d0-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. <span data-ttu-id="5c8d0-155">Na **spolupráce inovací domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5c8d0-155">On the **Collaborative Innovation Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    <span data-ttu-id="5c8d0-157">a.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-157">a.</span></span> <span data-ttu-id="5c8d0-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.foundry.<companyname>.com/`</span><span class="sxs-lookup"><span data-stu-id="5c8d0-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.foundry.<companyname>.com/`</span></span>

    <span data-ttu-id="5c8d0-159">b.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-159">b.</span></span> <span data-ttu-id="5c8d0-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.foundry.<companyname>.com`</span><span class="sxs-lookup"><span data-stu-id="5c8d0-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.foundry.<companyname>.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="5c8d0-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-161">These values are not real.</span></span> <span data-ttu-id="5c8d0-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5c8d0-163">Obraťte se na [tým podpory spolupráce klienta inovací](https://www.unilever.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-163">Contact [Collaborative Innovation Client support team](https://www.unilever.com/contact/) to get these values.</span></span>  

4. <span data-ttu-id="5c8d0-164">Spolupráce aplikace inovací očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-164">Collaborative Innovation application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="5c8d0-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-165">Please configure the following claims for this application.</span></span> <span data-ttu-id="5c8d0-166">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="5c8d0-167">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-167">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. <span data-ttu-id="5c8d0-169">Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtnout políčko **uživatelské atributy** rozbalte atributy.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-169">Click **View and edit all other user attributes** checkbox in the **User Attributes** section to expand the attributes.</span></span> <span data-ttu-id="5c8d0-170">Proveďte následující kroky na každém z zobrazených atributů-</span><span class="sxs-lookup"><span data-stu-id="5c8d0-170">Perform the following steps on each of the displayed attributes-</span></span>

    | <span data-ttu-id="5c8d0-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="5c8d0-171">Attribute Name</span></span> | <span data-ttu-id="5c8d0-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="5c8d0-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="5c8d0-173">givenName</span><span class="sxs-lookup"><span data-stu-id="5c8d0-173">givenname</span></span> | <span data-ttu-id="5c8d0-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="5c8d0-174">user.givenname</span></span> |
    | <span data-ttu-id="5c8d0-175">Příjmení</span><span class="sxs-lookup"><span data-stu-id="5c8d0-175">surname</span></span> | <span data-ttu-id="5c8d0-176">User.Surname</span><span class="sxs-lookup"><span data-stu-id="5c8d0-176">user.surname</span></span> |
    | <span data-ttu-id="5c8d0-177">EmailAddress</span><span class="sxs-lookup"><span data-stu-id="5c8d0-177">emailaddress</span></span> | <span data-ttu-id="5c8d0-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="5c8d0-178">user.userprincipalname</span></span> |
    | <span data-ttu-id="5c8d0-179">jméno</span><span class="sxs-lookup"><span data-stu-id="5c8d0-179">name</span></span> | <span data-ttu-id="5c8d0-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="5c8d0-180">user.userprincipalname</span></span> |

    <span data-ttu-id="5c8d0-181">a.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-181">a.</span></span> <span data-ttu-id="5c8d0-182">Klikněte na atribut, který se otevře **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-182">Click the attribute to open the **Edit Attribute** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    <span data-ttu-id="5c8d0-184">b.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-184">b.</span></span> <span data-ttu-id="5c8d0-185">Odstranit hodnotu adresy URL **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-185">Delete the URL value from the **Namespace**.</span></span>
    
    <span data-ttu-id="5c8d0-186">c.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-186">c.</span></span> <span data-ttu-id="5c8d0-187">Klikněte na tlačítko **Ok** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-187">Click **Ok** to save the setting.</span></span>

6. <span data-ttu-id="5c8d0-188">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-188">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. <span data-ttu-id="5c8d0-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="5c8d0-192">Konfigurace jednotného přihlašování na **spolupráce inovací** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory spolupráce inovací](https://www.unilever.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="5c8d0-192">To configure single sign-on on **Collaborative Innovation** side, you need to send the downloaded **Metadata XML** to [Collaborative Innovation support team](https://www.unilever.com/contact/).</span></span> <span data-ttu-id="5c8d0-193">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-193">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5c8d0-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5c8d0-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5c8d0-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5c8d0-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5c8d0-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5c8d0-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c8d0-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="5c8d0-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5c8d0-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5c8d0-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5c8d0-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5c8d0-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5c8d0-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5c8d0-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5c8d0-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5c8d0-209">a.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-209">a.</span></span> <span data-ttu-id="5c8d0-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5c8d0-211">b.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-211">b.</span></span> <span data-ttu-id="5c8d0-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5c8d0-213">c.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-213">c.</span></span> <span data-ttu-id="5c8d0-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5c8d0-215">d.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-215">d.</span></span> <span data-ttu-id="5c8d0-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-216">Click **Create**.</span></span>
 
### <a name="creating-a-collaborative-innovation-test-user"></a><span data-ttu-id="5c8d0-217">Vytvoření zkušebního uživatele spolupráce inovací</span><span class="sxs-lookup"><span data-stu-id="5c8d0-217">Creating a Collaborative Innovation test user</span></span>

<span data-ttu-id="5c8d0-218">Povolit uživatelům Azure AD přihlášení do spolupráce inovací, musí být zřízená do spolupráce inovace.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-218">To enable Azure AD users to log in to Collaborative Innovation, they must be provisioned into Collaborative Innovation.</span></span>  

<span data-ttu-id="5c8d0-219">Zřizování je automaticky v případě této aplikace jako aplikace podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-219">In case of this application provisioning is automatic as the application supports just in time user provisioning.</span></span> <span data-ttu-id="5c8d0-220">Proto není nutné provádět žádné kroky v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-220">So there is no need to perform any steps here.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5c8d0-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c8d0-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5c8d0-222">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu spolupráce inovace.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Collaborative Innovation.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5c8d0-224">**Pokud chcete přiřadit Britta Simon spolupráce inovací, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5c8d0-224">**To assign Britta Simon to Collaborative Innovation, perform the following steps:**</span></span>

1. <span data-ttu-id="5c8d0-225">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5c8d0-227">V seznamu aplikací vyberte **spolupráce inovací**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-227">In the applications list, select **Collaborative Innovation**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. <span data-ttu-id="5c8d0-229">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5c8d0-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-231">Click **Add** button.</span></span> <span data-ttu-id="5c8d0-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5c8d0-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5c8d0-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5c8d0-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5c8d0-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5c8d0-237">Testing single sign-on</span></span>

<span data-ttu-id="5c8d0-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5c8d0-239">Když kliknete na dlaždici spolupráce inovací na přístupovém panelu, měli byste obdržet přihlašovací stránku aplikace spolupráce inovace.</span><span class="sxs-lookup"><span data-stu-id="5c8d0-239">When you click the Collaborative Innovation tile in the Access Panel, you should get login page of Collaborative Innovation application.</span></span>
<span data-ttu-id="5c8d0-240">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5c8d0-240">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5c8d0-241">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5c8d0-241">Additional resources</span></span>

* [<span data-ttu-id="5c8d0-242">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c8d0-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c8d0-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5c8d0-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

