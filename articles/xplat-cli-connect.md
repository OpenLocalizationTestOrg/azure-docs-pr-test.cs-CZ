---
title: "Přihlaste se k Azure z rozhraní příkazového řádku | Microsoft Docs"
description: "Připojení k předplatnému Azure z rozhraní příkazového řádku (Azure CLI) pro Mac, Linux a Windows"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 31efab60690b54faf7992251fcd01e307c4464f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="log-in-to-azure-from-the-azure-cli"></a><span data-ttu-id="69013-103">Přihlaste se k Azure z rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="69013-103">Log in to Azure from the Azure CLI</span></span>
<span data-ttu-id="69013-104">Rozhraní příkazového řádku Azure je sada příkazů open source, a platformy pro práci s prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="69013-104">The Azure CLI is a set of open-source, cross-platform commands for working with Azure resources.</span></span> <span data-ttu-id="69013-105">Tento článek popisuje různé způsoby, jak poskytnout přihlašovací údaje účtu Azure připojit rozhraní příkazového řádku Azure k předplatnému Azure:</span><span class="sxs-lookup"><span data-stu-id="69013-105">This article describes the different ways to provide your Azure account credentials to connect the Azure CLI to your Azure subscription:</span></span>

* <span data-ttu-id="69013-106">Spustit `azure login` rozhraní příkazového řádku příkaz k ověření pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="69013-106">Run the `azure login` CLI command to authenticate through Azure Active Directory.</span></span> <span data-ttu-id="69013-107">Tato metoda umožňuje přístup k rozhraní příkazového řádku v obou [příkaz režimy](#cli-command-modes).</span><span class="sxs-lookup"><span data-stu-id="69013-107">This method gives you access to CLI commands in both [command modes](#cli-command-modes).</span></span> <span data-ttu-id="69013-108">Při spuštění příkazu bez dalších možností, `azure login` vás vyzve, abyste pokračovat interaktivní přihlašování prostřednictvím webového portálu.</span><span class="sxs-lookup"><span data-stu-id="69013-108">When you run the command without additional options, `azure login` prompts you to continue logging in interactively through a web portal.</span></span> <span data-ttu-id="69013-109">Pro další `azure login` příkaz Možnosti najdete v tématu scénáře v tomto článku nebo typ `azure login --help`.</span><span class="sxs-lookup"><span data-stu-id="69013-109">For additional `azure login` command options, see the scenarios in this article, or type `azure login --help`.</span></span>
* <span data-ttu-id="69013-110">Pokud potřebujete použít příkazy rozhraní příkazového řádku v režimu Azure Service Management (nedoporučuje se většina nových nasazení), můžete stáhnout a nainstalovat soubor nastavení publikování ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="69013-110">If you only need to use Azure Service Management mode CLI commands (not recommended for most new deployments), you can download and install a publish settings file on your computer.</span></span>

<span data-ttu-id="69013-111">Pokud jste ještě nenainstalovali rozhraní příkazového řádku, přečtěte si téma [nainstalovat Azure CLI](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="69013-111">If you haven't already installed the CLI, see [Install the Azure CLI](cli-install-nodejs.md).</span></span> <span data-ttu-id="69013-112">Pokud nemáte předplatné Azure, můžete si [bezplatně vytvořit účet](http://azure.microsoft.com/free/) během několika minut.</span><span class="sxs-lookup"><span data-stu-id="69013-112">If you don't have an Azure subscription, you can create a [free account](http://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="69013-113">Informace o jiný účet identity a předplatná Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="69013-113">For background about different account identities and Azure subscriptions, see [How Azure subscriptions are associated with Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span></span>

## <a name="scenario-1-azure-login-with-interactive-login"></a><span data-ttu-id="69013-114">Scénář 1: přihlášení k azure s interaktivní přihlášení</span><span class="sxs-lookup"><span data-stu-id="69013-114">Scenario 1: azure login with interactive login</span></span>
<span data-ttu-id="69013-115">S určitým účtům rozhraní příkazového řádku vyžaduje, abyste spustili `azure login` a potom pokračujte v procesu přihlášení s webovým prohlížečem, přes webový portál, procesu označovaného jako *interaktivní přihlášení*.</span><span class="sxs-lookup"><span data-stu-id="69013-115">With certain accounts, the CLI requires you to run `azure login` and then continue the login process with a web browser through a web portal, a process called *interactive login*.</span></span> <span data-ttu-id="69013-116">Obvyklým důvodem je, když máte pracovní nebo školní účet (také nazývané *účet organizace*), je nastavena na požadavek vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="69013-116">A common reason is when you have a work or school account (also called an *organizational account*) that is set up to require multifactor authentication.</span></span> <span data-ttu-id="69013-117">Pokud chcete používat příkazy v režimu Resource Manager také používejte interaktivní přihlášení pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="69013-117">Also use interactive login with your Microsoft account, when you want to use Resource Manager mode commands.</span></span>

<span data-ttu-id="69013-118">Interaktivní přihlášení je snadné: typ `azure login` – bez jakékoli možnosti – jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="69013-118">Interactive login is easy: type `azure login` -- without any options -- as shown in the following example:</span></span>

```
azure login
```                                                                                             

<span data-ttu-id="69013-119">Výstup vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="69013-119">The output appears something like the following:</span></span>

```         
info:    Executing command login
info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate.
```
<span data-ttu-id="69013-120">Zkopírujte kód nabízena ve výstupu příkazu a otevřete prohlížeč na http://aka.ms/devicelogin nebo jinou stránku, pokud zadaný.</span><span class="sxs-lookup"><span data-stu-id="69013-120">Copy the code offered to you in the command output, and open a browser to http://aka.ms/devicelogin, or other page if specified.</span></span> <span data-ttu-id="69013-121">(Můžete otevřít prohlížeč na stejném počítači nebo na jiný počítač nebo zařízení.) Zadejte kód a potom zobrazí výzva k zadání uživatelského jména a hesla pro identitu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="69013-121">(You can open a browser on the same computer, or on a different computer or device.) Enter the code, and then you are prompted to enter the username and password for the identity you want to use.</span></span> <span data-ttu-id="69013-122">Po dokončení tohoto procesu příkazové okno dokončení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="69013-122">When that process completes, the command shell completes the login.</span></span> <span data-ttu-id="69013-123">Ho může vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="69013-123">It might look something like:</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> <span data-ttu-id="69013-124">S interaktivní přihlášení ověřování a autorizace se provádí pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="69013-124">With interactive login, authentication and authorization are performed using Azure Active Directory.</span></span> <span data-ttu-id="69013-125">Pokud chcete použít identitu účtu Microsoft, přistupuje k procesu přihlášení výchozí doménu služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="69013-125">If you use a Microsoft account identity, the login process accesses your Azure Active Directory default domain.</span></span> <span data-ttu-id="69013-126">(Pokud jste zaregistrovali bezplatný účet Azure, Azure Active Directory automaticky vytvoří výchozí doménu pro účet.)</span><span class="sxs-lookup"><span data-stu-id="69013-126">(If you signed up for a free Azure account, Azure Active Directory automatically created a default domain for your account.)</span></span>
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a><span data-ttu-id="69013-127">Scénář 2: azure přihlášení pomocí uživatelského jména a hesla</span><span class="sxs-lookup"><span data-stu-id="69013-127">Scenario 2: azure login with a username and password</span></span>
<span data-ttu-id="69013-128">Použít `azure login` s uživatelské jméno (`-u`) parametr k ověření, když chcete použít pracovní nebo školní účet, který nevyžaduje vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="69013-128">Use the `azure login` command with the username (`-u`) parameter to authenticate when you want to use a work or school account that doesn't require multifactor authentication.</span></span> <span data-ttu-id="69013-129">Zobrazení výzvy na příkazovém řádku hesla (nebo můžete volitelně předat heslo jako další parametr `azure login` příkaz).</span><span class="sxs-lookup"><span data-stu-id="69013-129">You are prompted at the command line for the password (or you can optionally pass the password as an additional parameter of the `azure login` command).</span></span> <span data-ttu-id="69013-130">Následující příklad předá uživatelské jméno účtu organizace:</span><span class="sxs-lookup"><span data-stu-id="69013-130">The following example passes the username of an organizational account:</span></span>

    azure login -u myUserName@contoso.onmicrosoft.com

<span data-ttu-id="69013-131">Potom budete vyzváni k zadání hesla:</span><span class="sxs-lookup"><span data-stu-id="69013-131">You are then prompted to enter your password:</span></span>

    info:    Executing command login
    Password: *********

<span data-ttu-id="69013-132">Potom dokončí proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="69013-132">The login process then completes.</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

<span data-ttu-id="69013-133">Pokud je vaše první protokolování v čase se pomocí těchto přihlašovacích údajů, zobrazí se výzva k ověření, které chcete ověřovací token do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="69013-133">If this is your first time logging in with these credentials, you are asked to verify that you wish to cache an authentication token.</span></span> <span data-ttu-id="69013-134">Tato výzva také nastat, pokud jste dříve používali `azure logout` příkazu (popsaný v článku později).</span><span class="sxs-lookup"><span data-stu-id="69013-134">This prompt also occurs if you previously used the `azure logout` command (described later in the article).</span></span> <span data-ttu-id="69013-135">Chcete-li vynechat tuto výzvu pro scénáře automatizace, spusťte `azure login` s `-q` parametr.</span><span class="sxs-lookup"><span data-stu-id="69013-135">To bypass this prompt for automation scenarios, run `azure login` with the `-q` parameter.</span></span>

## <a name="scenario-3-azure-login-with-a-service-principal"></a><span data-ttu-id="69013-136">Scénář 3: přihlášení k azure pomocí objektu služby</span><span class="sxs-lookup"><span data-stu-id="69013-136">Scenario 3: azure login with a service principal</span></span>
<span data-ttu-id="69013-137">Pokud vytvoříte objekt služby pro aplikaci služby Active Directory a objektu služby má oprávnění pro vaše předplatné, můžete použít `azure login` příkaz k ověření objektu služby.</span><span class="sxs-lookup"><span data-stu-id="69013-137">If you create a service principal for an Active Directory application, and the service principal has permissions on your subscription, you can use the `azure login` command to authenticate the service principal.</span></span> <span data-ttu-id="69013-138">V závislosti na scénáři, je možné zadat přihlašovací údaje objektu služby jako explicitní parametry `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="69013-138">Depending on your scenario, you could provide the credentials of the service principal as explicit parameters of the `azure login` command.</span></span> <span data-ttu-id="69013-139">Například následující příkaz předává hlavní název služby a služby Active Directory ID klienta:</span><span class="sxs-lookup"><span data-stu-id="69013-139">For example, the following command passes the service principal name and Active Directory tenant ID:</span></span>

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

<span data-ttu-id="69013-140">Zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="69013-140">You are then prompted to provide the password.</span></span> <span data-ttu-id="69013-141">Můžete také zadat přihlašovací údaje prostřednictvím rozhraní příkazového řádku skriptu nebo aplikace kódu nebo používat certifikát pro ověření instanční objekt neinteraktivně pro scénáře automatizace.</span><span class="sxs-lookup"><span data-stu-id="69013-141">You can also provide the credentials through a CLI script or application code, or use a certificate to authenticate the service principal non-interactively for automation scenarios.</span></span> <span data-ttu-id="69013-142">Podrobnosti a příklady naleznete v tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="69013-142">For details and examples, see [Authenticating a service principal with Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).</span></span>

## <a name="scenario-4-use-a-publish-settings-file"></a><span data-ttu-id="69013-143">Scénář 4: Použijte soubor nastavení publikování</span><span class="sxs-lookup"><span data-stu-id="69013-143">Scenario 4: Use a publish settings file</span></span>
<span data-ttu-id="69013-144">Pokud potřebujete použít příkazy rozhraní příkazového řádku režimu Azure Service Management (například k nasazení virtuálních počítačů Azure v modelu nasazení classic), se můžete připojit pomocí soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="69013-144">If you only need to use the Azure Service Management mode CLI commands (for example, to deploy Azure VMs in the classic deployment model), you can connect using a publish settings file.</span></span> <span data-ttu-id="69013-145">Tato metoda nainstaluje certifikát na místním počítači, který umožňuje provádět úlohy správy pro, dokud jsou platné předplatné a certifikát.</span><span class="sxs-lookup"><span data-stu-id="69013-145">This method installs a certificate on your local computer that allows you to perform management tasks for as long as the subscription and the certificate are valid.</span></span>

* <span data-ttu-id="69013-146">**Chcete-li stáhnout soubor nastavení publikování** pro váš účet, zajistěte, aby rozhraní příkazového řádku v režimu správy služby zadáním `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="69013-146">**To download the publish settings file** for your account, ensure that the CLI is in Service Management mode by typing `azure config mode asm`.</span></span> <span data-ttu-id="69013-147">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="69013-147">Then run the following command:</span></span>

        azure account download

<span data-ttu-id="69013-148">To otevře výchozí prohlížeč a zobrazí výzvu pro přihlášení k aplikaci [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="69013-148">This opens your default browser and prompts you to sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="69013-149">Po přihlášení, `.publishsettings` stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="69013-149">After you sign in, a `.publishsettings` file downloads.</span></span> <span data-ttu-id="69013-150">Poznamenejte si, kam se soubor uloží.</span><span class="sxs-lookup"><span data-stu-id="69013-150">Make note of where this file is saved.</span></span>

> [!NOTE]
> <span data-ttu-id="69013-151">Pokud váš účet je spojen s více klienty Azure Active Directory, budete vyzváni k výběru služby Active Directory, které chcete stáhnout soubor nastavení publikování pro.</span><span class="sxs-lookup"><span data-stu-id="69013-151">If your account is associated with multiple Azure Active Directory tenants, you may be prompted to select which Active Directory you wish to download a publish settings file for.</span></span>
>
>

<span data-ttu-id="69013-152">Po výběru pomocí stránce pro stahování, nebo pomocí portálu Azure classic, vybrané služby Active Directory se změní na výchozí hodnotu použitou classic stránka portálu a stahování.</span><span class="sxs-lookup"><span data-stu-id="69013-152">Once selected using the download page, or by visiting the Azure classic portal, the selected Active Directory becomes the default used by the classic portal and download page.</span></span> <span data-ttu-id="69013-153">Po vytvoření výchozí uvidíte text '**kliknutím sem se vrátíte na stránku výběr**se v horní části stránky pro stažení.</span><span class="sxs-lookup"><span data-stu-id="69013-153">Once a default has been established, you see the text '**click here to return to the selection page**' at the top of the download page.</span></span> <span data-ttu-id="69013-154">Použijte zadaný odkaz a návrat na stránku výběr.</span><span class="sxs-lookup"><span data-stu-id="69013-154">Use the provided link to return to the selection page.</span></span>

* <span data-ttu-id="69013-155">**Chcete-li importovat soubor nastavení publikování**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="69013-155">**To import the publish settings file**, run the following command:</span></span>

        azure account import <path to your .publishsettings file>

> [!IMPORTANT]
> <span data-ttu-id="69013-156">Po importu vaše nastavení publikování, měli byste odstranit `.publishsettings` souboru.</span><span class="sxs-lookup"><span data-stu-id="69013-156">After importing your publish settings, you should delete the `.publishsettings` file.</span></span> <span data-ttu-id="69013-157">To už není nutné pomocí rozhraní příkazového řádku Azure a představuje bezpečnostní riziko, protože ho může použít k získání přístupu k vašemu předplatnému.</span><span class="sxs-lookup"><span data-stu-id="69013-157">It is no longer required by the Azure CLI and presents a security risk as it could be used to gain access to your subscription.</span></span>
>
>

## <a name="cli-command-modes"></a><span data-ttu-id="69013-158">Režimy rozhraní příkazového řádku příkaz</span><span class="sxs-lookup"><span data-stu-id="69013-158">CLI command modes</span></span>
<span data-ttu-id="69013-159">Rozhraní příkazového řádku Azure obsahuje dva režimy příkaz pro práci s prostředky Azure, se sadami jiný příkaz:</span><span class="sxs-lookup"><span data-stu-id="69013-159">The Azure CLI provides two command modes for working with Azure resources, with different command sets:</span></span>

* <span data-ttu-id="69013-160">**Režimu Resource Manager** – pro práci s prostředky Azure v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="69013-160">**Resource Manager mode** - for working with Azure resources in the Resource Manager deployment model.</span></span> <span data-ttu-id="69013-161">Pokud chcete nastavit tento režim, spusťte `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="69013-161">To set this mode, run `azure config mode arm`.</span></span>
* <span data-ttu-id="69013-162">**Režim správy služby** – pro práci s prostředky Azure v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="69013-162">**Service Management mode** - for working with Azure resources in the classic deployment model.</span></span> <span data-ttu-id="69013-163">Pokud chcete nastavit tento režim, spusťte `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="69013-163">To set this mode, run `azure config mode asm`.</span></span>

<span data-ttu-id="69013-164">Při první instalaci aktuální verzi rozhraní příkazového řádku je v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="69013-164">When first installed, the current release of the CLI is in Resource Manager mode.</span></span>

> [!NOTE]
> <span data-ttu-id="69013-165">Režim Resource Manager a Service Management režim se vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="69013-165">The Resource Manager mode and Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="69013-166">To znamená nelze spravovat prostředky vytvořené v jednom režimu z jiných režimu.</span><span class="sxs-lookup"><span data-stu-id="69013-166">That is, resources created in one mode cannot be managed from the other mode.</span></span>
>
>

## <a name="multiple-subscriptions"></a><span data-ttu-id="69013-167">Více předplatných</span><span class="sxs-lookup"><span data-stu-id="69013-167">Multiple subscriptions</span></span>
<span data-ttu-id="69013-168">Pokud máte víc předplatných Azure, připojení k Azure udělí přístup k Všechna předplatná spojená s přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="69013-168">If you have multiple Azure subscriptions, connecting to Azure grants access to all subscriptions associated with your credentials.</span></span> <span data-ttu-id="69013-169">Jedno předplatné, je vybrána jako výchozí a používat rozhraní příkazového řádku Azure při provádění operací.</span><span class="sxs-lookup"><span data-stu-id="69013-169">One subscription is selected as the default, and used by the Azure CLI when performing operations.</span></span> <span data-ttu-id="69013-170">Můžete zobrazit předplatná, včetně aktuální výchozí předplatné, pomocí `azure account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="69013-170">You can view the subscriptions, including the current default subscription, using the `azure account list` command.</span></span> <span data-ttu-id="69013-171">Tento příkaz vrátí informace podobná této:</span><span class="sxs-lookup"><span data-stu-id="69013-171">This command returns information similar to the following:</span></span>

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

<span data-ttu-id="69013-172">V předchozím seznamu **aktuální** sloupec zobrazuje informace o aktuálním předplatném výchozí jako Azure-sub-1.</span><span class="sxs-lookup"><span data-stu-id="69013-172">In the preceding list, the **Current** column indicates the current default subscription as Azure-sub-1.</span></span> <span data-ttu-id="69013-173">Chcete-li změnit výchozí předplatné, použijte `azure account set` příkaz a určete předplatné, které chcete nastavit jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="69013-173">To change the default subscription, use the `azure account set` command, and specify the subscription that you wish to be the default.</span></span> <span data-ttu-id="69013-174">Například:</span><span class="sxs-lookup"><span data-stu-id="69013-174">For example:</span></span>

    azure account set Azure-sub-2

<span data-ttu-id="69013-175">Tato operace změní výchozí předplatné na Azure-sub-2.</span><span class="sxs-lookup"><span data-stu-id="69013-175">This changes the default subscription to Azure-sub-2.</span></span>

> [!NOTE]
> <span data-ttu-id="69013-176">Změna výchozí předplatné se okamžitě projeví a globální změně; nové příkazy rozhraní příkazového řádku Azure, zda je spouštět z příkazového řádku stejnou instanci nebo jinou instanci, použijte nové předplatné výchozí.</span><span class="sxs-lookup"><span data-stu-id="69013-176">Changing the default subscription takes effect immediately, and is a global change; new Azure CLI commands, whether you run them from the same command-line instance or a different instance, use the new default subscription.</span></span>
>
>

<span data-ttu-id="69013-177">Pokud chcete použít jiné než výchozí předplatné s Azure CLI, ale nechcete změnit aktuální výchozí nastavení, můžete použít `--subscription` možnost pro příkaz a zadejte název odběru, které chcete použít pro operaci.</span><span class="sxs-lookup"><span data-stu-id="69013-177">If you wish to use a non-default subscription with the Azure CLI, but don't want to change the current default, you can use the `--subscription` option for the command and provide the name of the subscription you wish to use for the operation.</span></span>

<span data-ttu-id="69013-178">Jakmile se připojíte ke svému předplatnému Azure, budete moct začít, používat příkazy rozhraní příkazového řádku Azure pro práci s prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="69013-178">Once you are connected to your Azure subscription, you can start using the Azure CLI commands to work with Azure resources.</span></span>

## <a name="storage-of-cli-settings"></a><span data-ttu-id="69013-179">Ukládání nastavení rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="69013-179">Storage of CLI settings</span></span>
<span data-ttu-id="69013-180">Jestli přihlášení s `azure login` příkaz nebo importu nastavení publikování, profil rozhraní příkazového řádku a protokoly jsou uloženy v `.azure` adresář umístěný ve vaší `user` adresáře.</span><span class="sxs-lookup"><span data-stu-id="69013-180">Whether you log in with the `azure login` command or import publish settings, your CLI profile and logs are stored in a `.azure` directory located in your `user` directory.</span></span> <span data-ttu-id="69013-181">Vaše `user` adresář je chráněn váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="69013-181">Your `user` directory is protected by your operating system.</span></span> <span data-ttu-id="69013-182">Doporučujeme však, že je provést další kroky k šifrování vašeho `user` adresáře.</span><span class="sxs-lookup"><span data-stu-id="69013-182">However, we recommend that you take additional steps to encrypt your `user` directory.</span></span> <span data-ttu-id="69013-183">Můžete tak učinit následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="69013-183">You can do so in the following ways:</span></span>

* <span data-ttu-id="69013-184">V systému Windows upravit vlastnosti directory nebo pomocí nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="69013-184">On Windows, modify the directory properties or use BitLocker.</span></span>
* <span data-ttu-id="69013-185">V systému Mac zapněte FileVault pro adresář.</span><span class="sxs-lookup"><span data-stu-id="69013-185">On Mac, turn on FileVault for the directory.</span></span>
* <span data-ttu-id="69013-186">V Ubuntu použijte funkci Šifrovaný domovský adresář.</span><span class="sxs-lookup"><span data-stu-id="69013-186">On Ubuntu, use the Encrypted Home directory feature.</span></span> <span data-ttu-id="69013-187">Další Linuxových distribucích nabízí podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="69013-187">Other Linux distributions offer similar features.</span></span>

## <a name="logging-out"></a><span data-ttu-id="69013-188">Protokolování se</span><span class="sxs-lookup"><span data-stu-id="69013-188">Logging out</span></span>
<span data-ttu-id="69013-189">Chcete-li odhlášení, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="69013-189">To log out, use the following command:</span></span>

    azure logout -u <username>

<span data-ttu-id="69013-190">Pokud předplatné přidružené k účtu jsou pouze ověření pomocí služby Active Directory, protokolování se odstraní informace o předplatném z místního profilu.</span><span class="sxs-lookup"><span data-stu-id="69013-190">If the subscriptions associated with the account are only authenticated with Active Directory, logging out deletes the subscription information from the local profile.</span></span> <span data-ttu-id="69013-191">Ale pokud soubor nastavení publikování byl importován také pro odběry, protokolování jen odstranění služby Active Directory související informace z místního profilu.</span><span class="sxs-lookup"><span data-stu-id="69013-191">However, if a publish settings file was also imported for the subscriptions, logging out only deletes Active Directory related information from the local profile.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69013-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69013-192">Next steps</span></span>
* <span data-ttu-id="69013-193">Pomocí rozhraní příkazového řádku Azure naleznete v části [rozhraní příkazového řádku Azure v režimu Resource Manager](virtual-machines/azure-cli-arm-commands.md) a [rozhraní příkazového řádku Azure v režimu správy služby](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="69013-193">To use Azure CLI commands, see [Azure CLI commands in Resource Manager mode](virtual-machines/azure-cli-arm-commands.md) and [Azure CLI commands in Service Management mode](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>
* <span data-ttu-id="69013-194">Další informace o Azure CLI, stažení zdrojový kód, problémy sestavy, nebo můžete přispět k projektu, najdete [úložiště GitHub pro rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="69013-194">To learn more about the Azure CLI, download source code, report problems, or contribute to the project, visit the [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="69013-195">Pokud narazíte na problémy pomocí rozhraní příkazového řádku Azure nebo Azure, přejděte [fóra Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="69013-195">If you encounter problems using the Azure CLI, or Azure, visit the [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>
