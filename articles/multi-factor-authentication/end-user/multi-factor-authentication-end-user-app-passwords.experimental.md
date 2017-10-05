---
title: "Jak používat hesla aplikací v Azure MFA? | Dokumentace Microsoftu"
description: "Tato stránka vám pomůže uživatelům pomoct pochopit, jaké jsou hesla aplikací a jaké se používají s ohledem na Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 1ecc2bdef5ff7ef8ed8dded7dc12428ce9657821
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a><span data-ttu-id="befe0-104">Co jsou hesla aplikací v Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="befe0-104">What are App Passwords in Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="befe0-105">Některé neprohlížečové aplikace, jako je například klienta Apple nativní e-mailu, který používá protokolu Exchange Active Sync, aktuálně nepodporují službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="befe0-105">Certain non-browser apps, such as the Apple native email client that uses Exchange Active Sync, currently do not support multi-factor authentication.</span></span> <span data-ttu-id="befe0-106">Služba Multi-Factor authentication je povoleno podle uživatelů.</span><span class="sxs-lookup"><span data-stu-id="befe0-106">Multi-factor authentication is enabled per user.</span></span>  <span data-ttu-id="befe0-107">To znamená, že uživatel nemůže používání služby Multi-Factor authentication, pokud:</span><span class="sxs-lookup"><span data-stu-id="befe0-107">This means that a user can't use multi-factor authentication if:</span></span>

- <span data-ttu-id="befe0-108">Uživatel povolen pro službu Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="befe0-108">The user has been enabled for multi-factor authentication</span></span>
- <span data-ttu-id="befe0-109">Uživatel se pokouší použít neprohlížečové aplikace.</span><span class="sxs-lookup"><span data-stu-id="befe0-109">The user is trying to use a non-browser app.</span></span>

<span data-ttu-id="befe0-110">Heslo aplikace umožňuje uživatelům používat aplikace.</span><span class="sxs-lookup"><span data-stu-id="befe0-110">An app password allows the user to use the app.</span></span>

<span data-ttu-id="befe0-111">Až budete mít heslo aplikace, použijte místo původní heslo těchto neprohlížečových aplikací.</span><span class="sxs-lookup"><span data-stu-id="befe0-111">Once you have an app password, you use it in place of your original password with these non-browser apps.</span></span> <span data-ttu-id="befe0-112">Při registraci pro dvoustupňové ověření, že informuje Microsoft nechcete libovolný uživatel přihlásit pomocí hesla, pokud nemohou také provádět druhé ověření.</span><span class="sxs-lookup"><span data-stu-id="befe0-112">When you register for two-step verification, you're telling Microsoft not to let anyone sign in with your password if they can't also perform the second verification.</span></span> <span data-ttu-id="befe0-113">Klient pro nativní e-mailové Apple na váš telefon nemůžete se přihlásit jako je vzhledem k tomu, že nemůžete žádat pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="befe0-113">The Apple native email client on your phone can't sign in as you because it can't ask for two-step verification.</span></span> <span data-ttu-id="befe0-114">Řešení tohoto problému je vytvoření každodenní bezpečnější heslo aplikace, které nepoužíváte.</span><span class="sxs-lookup"><span data-stu-id="befe0-114">The solution to this problem is to create a more secure app password that you don't use day-to-day.</span></span> <span data-ttu-id="befe0-115">Hesla aplikací jsou pouze pro tyto aplikace, které nemohou podporovat dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="befe0-115">App passwords are only for those apps that can't support two-step verification.</span></span> <span data-ttu-id="befe0-116">Používejte heslo aplikace, takže aplikace se můžou obejít ověřování Multi-Factor authentication a pokračovat v práci.</span><span class="sxs-lookup"><span data-stu-id="befe0-116">Use the app password so that apps can bypass multi-factor authentication and continue to work.</span></span>


> [!NOTE]
> <span data-ttu-id="befe0-117">Klienti Office 2013 (včetně Outlook) podporují nové protokoly pro ověřování a lze použít s dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="befe0-117">Office 2013 clients (including Outlook) support new authentication protocols and can be used with two-step verification.</span></span> <span data-ttu-id="befe0-118">Hesla aplikací nejsou potřeba použít s klienty Office 2013.</span><span class="sxs-lookup"><span data-stu-id="befe0-118">App passwords are not required for use with Office 2013 clients.</span></span>  <span data-ttu-id="befe0-119">Další informace najdete v tématu [Office 2013 veřejné Preview verze moderního ověřování oznámeno](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span><span class="sxs-lookup"><span data-stu-id="befe0-119">For more information, see [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span></span>


## <a name="how-to-use-app-passwords"></a><span data-ttu-id="befe0-120">Postupy pro používání hesel aplikací</span><span class="sxs-lookup"><span data-stu-id="befe0-120">How to use app passwords</span></span>
<span data-ttu-id="befe0-121">Zde jsou některé věci vědět o heslech aplikací:</span><span class="sxs-lookup"><span data-stu-id="befe0-121">Here are some things to know about app passwords:</span></span>

* <span data-ttu-id="befe0-122">Nevytvářejte vlastní hesla aplikací.</span><span class="sxs-lookup"><span data-stu-id="befe0-122">You don't create your own app passwords.</span></span> <span data-ttu-id="befe0-123">Jsou automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="befe0-123">They are automatically generated.</span></span>
* <span data-ttu-id="befe0-124">Aktuálně je omezení 40 hesel na uživatele.</span><span class="sxs-lookup"><span data-stu-id="befe0-124">Currently there is a limit of 40 passwords per user.</span></span> 
* <span data-ttu-id="befe0-125">Pokud se pokusíte po dosažení limitu vytvořit heslo aplikace, budete muset před vytvořením nové odstraňte jednu z vašich dosavadních hesel aplikací.</span><span class="sxs-lookup"><span data-stu-id="befe0-125">If you try to create an app password after you have reached the limit, you'll have to delete one of your existing app passwords before you create a new one.</span></span>
* <span data-ttu-id="befe0-126">Používejte jedno heslo aplikace na zařízení, nikoliv podle aplikací.</span><span class="sxs-lookup"><span data-stu-id="befe0-126">Use one app password per device, not per application.</span></span> <span data-ttu-id="befe0-127">Můžete například vytvořit jedno heslo aplikace pro přenosný počítač a používat takové heslo aplikace pro všechny aplikace v tomto přenosném počítači.</span><span class="sxs-lookup"><span data-stu-id="befe0-127">For example, you can create one app password for your laptop and use that app password for all of your applications on that laptop.</span></span> <span data-ttu-id="befe0-128">Pak vytvořte druhý heslo aplikace se používat pro všechny aplikace na ploše.</span><span class="sxs-lookup"><span data-stu-id="befe0-128">Then, create a second app password to use for all your apps on your desktop.</span></span> 
* <span data-ttu-id="befe0-129">Jedno heslo aplikace jsou uvedeny poprvé, můžete zaregistrovat pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="befe0-129">You are given one app password the first time you register for two-step verification.</span></span>  <span data-ttu-id="befe0-130">Pokud potřebujete další těch, můžete je vytvořit.</span><span class="sxs-lookup"><span data-stu-id="befe0-130">If you need additional ones, you can create them.</span></span>



## <a name="creating-and-deleting-app-passwords"></a><span data-ttu-id="befe0-131">Vytvoření nebo odstranění hesla aplikace</span><span class="sxs-lookup"><span data-stu-id="befe0-131">Creating and deleting app passwords</span></span>
<span data-ttu-id="befe0-132">Během vaše počáteční přihlášení budete mít heslo aplikace, kterou můžete použít.</span><span class="sxs-lookup"><span data-stu-id="befe0-132">During your initial sign-in, you are given an app password that you can use.</span></span>  <span data-ttu-id="befe0-133">Můžete také vytvořit a později odstranit hesla aplikací.</span><span class="sxs-lookup"><span data-stu-id="befe0-133">You can also create and delete app passwords later on.</span></span> <span data-ttu-id="befe0-134">Jak odstranit hesla aplikací závisí na tom, jak používat ověřování Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="befe0-134">How you delete app passwords depends on how you use multi-factor authentication.</span></span> <span data-ttu-id="befe0-135">Odpovězte si na tyto otázky k určení, kde by měli přejít ke správě hesel aplikací:</span><span class="sxs-lookup"><span data-stu-id="befe0-135">Answer the following questions to determine where you should go to manage app passwords:</span></span> 

1. <span data-ttu-id="befe0-136">Používáte dvoustupňové ověřování pro svůj osobní účet Microsoft?</span><span class="sxs-lookup"><span data-stu-id="befe0-136">Do you use two-step verification for your personal Microsoft account?</span></span> <span data-ttu-id="befe0-137">Pokud ano, se seznamte s [hesla aplikací a dvoustupňové ověření](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) článek nápovědy.</span><span class="sxs-lookup"><span data-stu-id="befe0-137">If yes, you should refer to the [App passwords and two-step verification](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) article for help.</span></span> <span data-ttu-id="befe0-138">Pokud ne, přejděte k otázka, na dvě.</span><span class="sxs-lookup"><span data-stu-id="befe0-138">If no, continue to question two.</span></span>

2. <span data-ttu-id="befe0-139">OK, abyste používat dvoustupňové ověřování pro svůj pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="befe0-139">Ok, so you use two-step verification for your work or school account.</span></span> <span data-ttu-id="befe0-140">Používáte ji k přihlášení do aplikací Office 365?</span><span class="sxs-lookup"><span data-stu-id="befe0-140">Do you use it to sign in to Office 365 apps?</span></span> <span data-ttu-id="befe0-141">Pokud ano, se seznamte s [vytvořit heslo aplikace pro Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) nápovědu.</span><span class="sxs-lookup"><span data-stu-id="befe0-141">If yes, you should refer to [Create an app password for Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) for help.</span></span> <span data-ttu-id="befe0-142">Pokud ne, přejděte k otázku tři.</span><span class="sxs-lookup"><span data-stu-id="befe0-142">If no, continue to question three.</span></span> 

3. <span data-ttu-id="befe0-143">Používáte dvoustupňové ověření pomocí Microsoft Azure?</span><span class="sxs-lookup"><span data-stu-id="befe0-143">Do you use two-step verification with Microsoft Azure?</span></span> <span data-ttu-id="befe0-144">Pokud ano, pokračovat [spravovat hesla aplikací na portálu Azure](#manage-app-passwords-in-the-Azure-portal) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="befe0-144">If yes, continue to the [Manage app passwords in the Azure portal](#manage-app-passwords-in-the-Azure-portal) section of this article.</span></span> <span data-ttu-id="befe0-145">Pokud ne, přejděte k otázka čtyři.</span><span class="sxs-lookup"><span data-stu-id="befe0-145">If no, continue to question four.</span></span>

4. <span data-ttu-id="befe0-146">Nejste si jisti, kde používáte dvoustupňové ověřování?</span><span class="sxs-lookup"><span data-stu-id="befe0-146">Not sure where you use two-step verification?</span></span> <span data-ttu-id="befe0-147">Pokračujte [spravovat hesla aplikací s portálem MyApps](#manage-app-passwords-with-the-myapps-portal) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="befe0-147">Continue to the [Manage app passwords with the MyApps portal](#manage-app-passwords-with-the-myapps-portal) section of this article.</span></span> 


## <a name="manage-app-passwords-in-the-azure-portal"></a><span data-ttu-id="befe0-148">Správa hesel aplikací na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="befe0-148">Manage app passwords in the Azure portal</span></span>
<span data-ttu-id="befe0-149">Pokud používáte dvoustupňové ověřování s Azure, budete chtít vytvořit hesla aplikací prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="befe0-149">If you use two-step verification with Azure, you want to create app passwords through the Azure portal.</span></span>

### <a name="to-create-app-passwords-in-the-azure-portal"></a><span data-ttu-id="befe0-150">Chcete-li vytvořit hesla aplikací na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="befe0-150">To create app passwords in the Azure portal</span></span>
1. <span data-ttu-id="befe0-151">Přihlaste se k portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="befe0-151">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="befe0-152">V horní části klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="befe0-152">At the top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="befe0-153">Na stránce ověření nahoře vyberte možnost hesla aplikací</span><span class="sxs-lookup"><span data-stu-id="befe0-153">On the proofup page, at the top, select app passwords</span></span>
4. <span data-ttu-id="befe0-154">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="befe0-154">Click **Create**.</span></span>
5. <span data-ttu-id="befe0-155">Zadejte název hesla aplikace a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="befe0-155">Enter a name for the app password and click **Next**</span></span>
6. <span data-ttu-id="befe0-156">Kopírovat heslo aplikace do schránky a vložte jej do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="befe0-156">Copy the app password to the clipboard and paste it into your app.</span></span>
   
   ![Cloud](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="to-delete-app-passwords-in-the-azure-portal"></a><span data-ttu-id="befe0-158">Chcete-li odstranit hesla aplikací na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="befe0-158">To delete app passwords in the Azure portal</span></span>
1. <span data-ttu-id="befe0-159">Přihlaste se k portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="befe0-159">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="befe0-160">V horní části klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="befe0-160">At the top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="befe0-161">V horní části vedle dalšího ověření zabezpečení, vyberte **hesla aplikací.**</span><span class="sxs-lookup"><span data-stu-id="befe0-161">At the top, next to additional security verification, select **app passwords.**</span></span>
4. <span data-ttu-id="befe0-162">Vedle heslo aplikace, které chcete odstranit, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="befe0-162">Next to the app password you want to delete, select **Delete**.</span></span>
5. <span data-ttu-id="befe0-163">Potvrďte odstranění kliknutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="befe0-163">Confirm the deletion by clicking **yes**.</span></span>
6. <span data-ttu-id="befe0-164">Po odstranění hesla aplikace, můžete kliknout na **zavřete**.</span><span class="sxs-lookup"><span data-stu-id="befe0-164">Once the app password is deleted, you can click **close**.</span></span>


## <a name="manage-app-passwords-with-the-myapps-portal"></a><span data-ttu-id="befe0-165">Spravujte hesla aplikací s portálem MyApps.</span><span class="sxs-lookup"><span data-stu-id="befe0-165">Manage app passwords with the MyApps portal.</span></span>
<span data-ttu-id="befe0-166">Pokud si nejste jisti, jak používat ověřování Multi-Factor authentication, pak můžete vždy vytvořit a odstranit hesla aplikací prostřednictvím portálu myapps.</span><span class="sxs-lookup"><span data-stu-id="befe0-166">If you are not sure how you use multi-factor authentication, then you can always create and delete app passwords through the myapps portal.</span></span>

### <a name="to-create-an-app-password-using-the-myapps-portal"></a><span data-ttu-id="befe0-167">Chcete-li vytvořit heslo aplikace pomocí portálu Myapps</span><span class="sxs-lookup"><span data-stu-id="befe0-167">To create an app password using the Myapps portal</span></span>
1. <span data-ttu-id="befe0-168">Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="befe0-168">Sign in to [https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="befe0-169">Klikněte na název v pravé horní a zvolte **profil**.</span><span class="sxs-lookup"><span data-stu-id="befe0-169">Click your name at the top right, and choose **Profile**.</span></span>
3. <span data-ttu-id="befe0-170">Vyberte **dalšího ověření zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="befe0-170">Select **Additional Security Verification**.</span></span>
   <span data-ttu-id="befe0-171">![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span><span class="sxs-lookup"><span data-stu-id="befe0-171">![Select additional security verification - screenshot](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span></span>

4. <span data-ttu-id="befe0-172">Vyberte **hesla aplikací**.</span><span class="sxs-lookup"><span data-stu-id="befe0-172">Select **app passwords**.</span></span>
   <span data-ttu-id="befe0-173">![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span><span class="sxs-lookup"><span data-stu-id="befe0-173">![Select app passwords - screenshot](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span></span>

5. <span data-ttu-id="befe0-174">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="befe0-174">Click **Create**.</span></span>
6. <span data-ttu-id="befe0-175">Zadejte název hesla aplikace a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="befe0-175">Enter a name for the app password and click **Next**.</span></span>
7. <span data-ttu-id="befe0-176">Kopírovat heslo aplikace do schránky a vložte jej do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="befe0-176">Copy the app password to the clipboard and paste it into your app.</span></span>
   <span data-ttu-id="befe0-177">![Vytvořit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span><span class="sxs-lookup"><span data-stu-id="befe0-177">![Create an app password](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span></span>

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a><span data-ttu-id="befe0-178">Chcete-li odstranit heslo aplikace pomocí portálu Myapps</span><span class="sxs-lookup"><span data-stu-id="befe0-178">To delete an app password using the Myapps portal</span></span>
1. <span data-ttu-id="befe0-179">Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="befe0-179">Sign in to [https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="befe0-180">V horní části vyberte profil.</span><span class="sxs-lookup"><span data-stu-id="befe0-180">At the top, select profile.</span></span>
3. <span data-ttu-id="befe0-181">Vyberte **dalšího ověření zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="befe0-181">Select **Additional Security Verification**.</span></span>

   ![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. <span data-ttu-id="befe0-183">Vyberte **hesla aplikací**.</span><span class="sxs-lookup"><span data-stu-id="befe0-183">Select **app passwords**.</span></span>

   ![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. <span data-ttu-id="befe0-185">Vedle heslo aplikace, které chcete odstranit, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="befe0-185">Next to the app password you want to delete, click **Delete**.</span></span>

   ![Odstranit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. <span data-ttu-id="befe0-187">Potvrďte, že chcete odstranit toto heslo kliknutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="befe0-187">Confirm that you want to delete that password by clicking **yes**.</span></span>
7. <span data-ttu-id="befe0-188">Po odstranění hesla aplikace, můžete kliknout na **zavřete**.</span><span class="sxs-lookup"><span data-stu-id="befe0-188">Once the app password is deleted, you can click **close**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="befe0-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="befe0-189">Next steps</span></span>

- [<span data-ttu-id="befe0-190">Spravovat nastavení dvoustupňového ověřování</span><span class="sxs-lookup"><span data-stu-id="befe0-190">Manage your two-step verification settings</span></span>](multi-factor-authentication-end-user-manage-settings.md)

- <span data-ttu-id="befe0-191">Vyzkoušení [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) ověření vašeho přihlášení s oznámeními aplikace místo přijetí texty nebo volání.</span><span class="sxs-lookup"><span data-stu-id="befe0-191">Try out the [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) to verify your sign-ins with app notifications, instead of receiving texts or calls.</span></span> 
