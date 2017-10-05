---
title: "Řešení problémů Brána pro správu dat | Microsoft Docs"
description: "Tipy pro řešení potíží s Brána pro správu dat poskytuje."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
published: True
ms.openlocfilehash: b8e50a4e3c0b9c535ccc2344ff22261a356d5acc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a><span data-ttu-id="15367-103">Řešení potíží při použití Brány pro správu dat</span><span class="sxs-lookup"><span data-stu-id="15367-103">Troubleshoot issues with using Data Management Gateway</span></span>
<span data-ttu-id="15367-104">Tento článek obsahuje informace o řešení potíží s použitím brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="15367-104">This article provides information on troubleshooting issues with using Data Management Gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="15367-105">Najdete v článku [Brána pro správu dat](data-factory-data-management-gateway.md) článku podrobné informace o bráně.</span><span class="sxs-lookup"><span data-stu-id="15367-105">See the [Data Management Gateway](data-factory-data-management-gateway.md) article for detailed information about the gateway.</span></span> <span data-ttu-id="15367-106">Najdete v článku [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článku návod přesun dat z databáze SQL serveru místně do Microsoft Azure Blob storage pomocí brány.</span><span class="sxs-lookup"><span data-stu-id="15367-106">See the [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for a walkthrough of moving data from an on-premises SQL Server database to Microsoft Azure Blob storage by using the gateway.</span></span>
>
>

## <a name="failed-to-install-or-register-gateway"></a><span data-ttu-id="15367-107">Nepodařilo se nainstalovat nebo registraci brány</span><span class="sxs-lookup"><span data-stu-id="15367-107">Failed to install or register gateway</span></span>
### <a name="1-problem"></a><span data-ttu-id="15367-108">1. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-108">1. Problem</span></span>
<span data-ttu-id="15367-109">Zobrazí tato chybová zpráva při instalaci a registraci bránu, konkrétně při stahování souboru instalace brány.</span><span class="sxs-lookup"><span data-stu-id="15367-109">You see this error message when installing and registering a gateway, specifically, while downloading the gateway installation file.</span></span>

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a><span data-ttu-id="15367-110">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-110">Cause</span></span>
<span data-ttu-id="15367-111">Počítač, na který se pokoušíte nainstalovat brány se nepodařilo stáhnout nejnovější instalační soubor brány z webu Stažení softwaru kvůli problému v síti.</span><span class="sxs-lookup"><span data-stu-id="15367-111">The machine on which you are trying to install the gateway has failed to download the latest gateway installation file from the download center due to a network issue.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-112">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-112">Resolution</span></span>
<span data-ttu-id="15367-113">Zkontrolujte nastavení proxy serveru brány firewall zobrazíte, zda nastavení blokování síťové připojení z počítače na [centra stahování softwaru společnosti](https://download.microsoft.com/)a aktualizovat nastavení odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="15367-113">Check your firewall proxy server settings to see whether the settings block the network connection from the computer to the [download center](https://download.microsoft.com/), and update the settings accordingly.</span></span>

<span data-ttu-id="15367-114">Alternativně můžete stáhnout instalační soubor pro nejnovější bránu z [centra stahování softwaru společnosti](https://www.microsoft.com/download/details.aspx?id=39717) na jiných počítačích, kteří mohou přistupovat k stažení softwaru.</span><span class="sxs-lookup"><span data-stu-id="15367-114">Alternatively, you can download the installation file for the latest gateway from the [download center](https://www.microsoft.com/download/details.aspx?id=39717) on other machines that can access the download center.</span></span> <span data-ttu-id="15367-115">Pak můžete zkopírovat soubor Instalační služby systému k hostitelskému počítači brány a spouštět ručně, aby instalace a aktualizace brány.</span><span class="sxs-lookup"><span data-stu-id="15367-115">You can then copy the installer file to the gateway host computer and run it manually to install and update the gateway.</span></span>

### <a name="2-problem"></a><span data-ttu-id="15367-116">2. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-116">2. Problem</span></span>
<span data-ttu-id="15367-117">Se zobrazí tato chyba, když se pokoušíte nainstalovat bránu kliknutím **nainstalovat přímo na tomto počítači** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15367-117">You see this error when you're attempting to install a gateway by clicking **install directly on this computer** in the Azure portal.</span></span>

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a><span data-ttu-id="15367-118">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-118">Cause</span></span>
<span data-ttu-id="15367-119">Brána je už nainstalovaná na počítači.</span><span class="sxs-lookup"><span data-stu-id="15367-119">A gateway is already installed on the machine.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-120">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-120">Resolution</span></span>
<span data-ttu-id="15367-121">Odinstalujte existující bránu na počítači a klikněte na tlačítko **nainstalovat přímo na tomto počítači** propojení znovu.</span><span class="sxs-lookup"><span data-stu-id="15367-121">Uninstall the existing gateway on the machine and click the **install directly on this computer** link again.</span></span>

### <a name="3-problem"></a><span data-ttu-id="15367-122">3. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-122">3. Problem</span></span>
<span data-ttu-id="15367-123">Tato chyba může zobrazit při registraci novou bránu.</span><span class="sxs-lookup"><span data-stu-id="15367-123">You might see this error when registering a new gateway.</span></span>

`Error: The gateway has encountered an error during registration.`

#### <a name="cause"></a><span data-ttu-id="15367-124">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-124">Cause</span></span>
<span data-ttu-id="15367-125">Tato zpráva se může zobrazit pro jednu z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="15367-125">You might see this message for one of the following reasons:</span></span>

* <span data-ttu-id="15367-126">Formát klíč brány je neplatný.</span><span class="sxs-lookup"><span data-stu-id="15367-126">The format of the gateway key is invalid.</span></span>
* <span data-ttu-id="15367-127">Klíč brány byla zrušena platnost.</span><span class="sxs-lookup"><span data-stu-id="15367-127">The gateway key has been invalidated.</span></span>
* <span data-ttu-id="15367-128">Klíč brány byl znovu vygenerovat, z portálu.</span><span class="sxs-lookup"><span data-stu-id="15367-128">The gateway key has been regenerated from the portal.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="15367-129">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-129">Resolution</span></span>
<span data-ttu-id="15367-130">Ověřte, zda používáte správné brány klíč z portálu.</span><span class="sxs-lookup"><span data-stu-id="15367-130">Verify whether you are using the right gateway key from the portal.</span></span> <span data-ttu-id="15367-131">V případě potřeby znovu vygenerovat klíč a pomocí klíče k registraci brány.</span><span class="sxs-lookup"><span data-stu-id="15367-131">If needed, regenerate a key and use the key to register the gateway.</span></span>

### <a name="4-problem"></a><span data-ttu-id="15367-132">4. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-132">4. Problem</span></span>
<span data-ttu-id="15367-133">Pokud se registrace brány, může se zobrazit tato chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="15367-133">You might see the following error message when you're registering a gateway.</span></span>

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`



![Obsah nebo formát klíče je neplatný](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a><span data-ttu-id="15367-135">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-135">Cause</span></span>
<span data-ttu-id="15367-136">Obsah nebo formátu vstupní brána klíče je nesprávné.</span><span class="sxs-lookup"><span data-stu-id="15367-136">The content or format of the input gateway key is incorrect.</span></span> <span data-ttu-id="15367-137">Jedním z důvodů může být, že jste zkopírovali pouze část klíč z portálu nebo používáte neplatný klíč.</span><span class="sxs-lookup"><span data-stu-id="15367-137">One of the reasons can be that you copied only a portion of the key from the portal or you're using an invalid key.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-138">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-138">Resolution</span></span>
<span data-ttu-id="15367-139">Generovat klíč brány na portálu a použijte tlačítko Kopírovat můžete zkopírovat celý klíč.</span><span class="sxs-lookup"><span data-stu-id="15367-139">Generate a gateway key in the portal, and use the copy button to copy the whole key.</span></span> <span data-ttu-id="15367-140">Vložte jej v tomto okně k registraci brány.</span><span class="sxs-lookup"><span data-stu-id="15367-140">Then paste it in this window to register the gateway.</span></span>

### <a name="5-problem"></a><span data-ttu-id="15367-141">5. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-141">5. Problem</span></span>
<span data-ttu-id="15367-142">Pokud se registrace brány, může se zobrazit tato chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="15367-142">You might see the following error message when you're registering a gateway.</span></span>

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Klíč brány je neplatný nebo prázdný](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a><span data-ttu-id="15367-144">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-144">Cause</span></span>
<span data-ttu-id="15367-145">Byl znovu vygenerovat klíč brány nebo byl odstraněn brány na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15367-145">The gateway key has been regenerated or the gateway has been deleted in the Azure portal.</span></span> <span data-ttu-id="15367-146">Může také dojít, pokud instalační program brány pro správu dat není nejnovější.</span><span class="sxs-lookup"><span data-stu-id="15367-146">It can also happen if the Data Management Gateway setup is not latest.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-147">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-147">Resolution</span></span>
<span data-ttu-id="15367-148">Zkontrolujte, pokud instalační program brány pro správu dat je na nejnovější verzi, můžete nějakého najít na nejnovější verzi na Microsoft [centra stahování softwaru společnosti](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span><span class="sxs-lookup"><span data-stu-id="15367-148">Check if the Data Management Gateway setup is the latest version, you can find the latest version on the Microsoft [download center](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span></span>

<span data-ttu-id="15367-149">Pokud je nastavení aktuální / nejnovější a brány stále existuje na portálu, znovu vygenerovat klíč brány na portálu Azure a použijte tlačítko Kopírovat můžete zkopírovat celý klíč a vložte jej v tomto okně k registraci brány.</span><span class="sxs-lookup"><span data-stu-id="15367-149">If setup is current/ latest and gateway still exists on Portal, regenerate the gateway key in the Azure portal, and use the copy button to copy the whole key, and then paste it in this window to register the gateway.</span></span> <span data-ttu-id="15367-150">Jinak bránu znovu vytvořte a začít znovu.</span><span class="sxs-lookup"><span data-stu-id="15367-150">Otherwise, recreate the gateway and start over.</span></span>

### <a name="6-problem"></a><span data-ttu-id="15367-151">6. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-151">6. Problem</span></span>
<span data-ttu-id="15367-152">Pokud se registrace brány, může se zobrazit tato chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="15367-152">You might see the following error message when you're registering a gateway.</span></span>

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Klíč brány je neplatný nebo prázdný](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a><span data-ttu-id="15367-154">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-154">Cause</span></span>
<span data-ttu-id="15367-155">K této chybě může dojít, protože brána byla odstraněna, nebo byl znovu vygenerovat klíč přidružené brány.</span><span class="sxs-lookup"><span data-stu-id="15367-155">This error might happen because either the gateway has been deleted or the associated gateway key has been regenerated.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-156">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-156">Resolution</span></span>
<span data-ttu-id="15367-157">Pokud brána byla odstraněna, znovu vytvořit bránu z portálu, klikněte na **zaregistrovat**, zkopírujte klíč z portálu, vložte ho a zkuste registraci brány.</span><span class="sxs-lookup"><span data-stu-id="15367-157">If the gateway has been deleted, re-create the gateway from the portal, click **Register**, copy the key from the portal, paste it, and try to register the gateway.</span></span>

<span data-ttu-id="15367-158">Pokud brána stále existuje, ale byla znovu vygeneroval svůj klíč, použijte nový klíč k registraci brány.</span><span class="sxs-lookup"><span data-stu-id="15367-158">If the gateway still exists but its key has been regenerated, use the new key to register the gateway.</span></span> <span data-ttu-id="15367-159">Pokud nemáte klíč, znovu vygenerujte klíč znovu z portálu.</span><span class="sxs-lookup"><span data-stu-id="15367-159">If you don’t have the key, regenerate the key again from the portal.</span></span>

### <a name="7-problem"></a><span data-ttu-id="15367-160">7. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-160">7. Problem</span></span>
<span data-ttu-id="15367-161">Pokud se registrace brány, vám může být potřeba zadejte cestu a heslo pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="15367-161">When you're registering a gateway, you might need to enter path and password for a certificate.</span></span>

![Zadejte certifikát](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a><span data-ttu-id="15367-163">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-163">Cause</span></span>
<span data-ttu-id="15367-164">Brána je zaregistrován na jiných počítačích než.</span><span class="sxs-lookup"><span data-stu-id="15367-164">The gateway has been registered on other machines before.</span></span> <span data-ttu-id="15367-165">Při počáteční registraci brány byla přidružena brány šifrovací certifikát.</span><span class="sxs-lookup"><span data-stu-id="15367-165">During the initial registration of a gateway, an encryption certificate has been associated with the gateway.</span></span> <span data-ttu-id="15367-166">Certifikát můžete být samoobslužné generované bránu nebo zadané uživatelem.</span><span class="sxs-lookup"><span data-stu-id="15367-166">The certificate can either be self-generated by the gateway or provided by the user.</span></span>  <span data-ttu-id="15367-167">Tento certifikát se používá k šifrování přihlašovacích údajů úložiště dat (propojené služby).</span><span class="sxs-lookup"><span data-stu-id="15367-167">This certificate is used to encrypt credentials of the data store (linked service).</span></span>  

![Export certifikátu](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

<span data-ttu-id="15367-169">Při obnovování brány na jiný hostitelský počítač, Průvodce registrací požádá o tohoto certifikátu dešifrovat přihlašovací údaje dříve zašifrované pomocí tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="15367-169">When restoring the gateway on a different host machine, the registration wizard asks for this certificate to decrypt credentials previously encrypted with this certificate.</span></span>  <span data-ttu-id="15367-170">Bez tohoto certifikátu přihlašovací údaje nelze dešifrovat pomocí nové brány a další kopie aktivity spuštěních přidružené k této nové brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="15367-170">Without this certificate, the credentials cannot be decrypted by the new gateway and subsequent copy activity executions associated with this new gateway will fail.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="15367-171">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-171">Resolution</span></span>
<span data-ttu-id="15367-172">Pokud jste exportovali certifikát přihlašovacích údajů z původní počítač brány pomocí **exportovat** tlačítko **nastavení** kartě v správy Správce konfigurace brány dat, použít certifikát v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="15367-172">If you have exported the credential certificate from the original gateway machine by using the **Export** button on the **Settings** tab in Data Management Gateway Configuration Manager, use the certificate here.</span></span>

<span data-ttu-id="15367-173">Při obnovení bránu nedá Přeskočit tuto fázi.</span><span class="sxs-lookup"><span data-stu-id="15367-173">You cannot skip this stage when recovering a gateway.</span></span> <span data-ttu-id="15367-174">Pokud chybí certifikát, budete muset odstranit bránu z portálu a znovu vytvořit novou bránu.</span><span class="sxs-lookup"><span data-stu-id="15367-174">If the certificate is missing, you need to delete the gateway from the portal and re-create a new gateway.</span></span>  <span data-ttu-id="15367-175">Kromě toho aktualizujte všechny propojené služby, které se vztahují k bráně nutnosti opětovného zadávání svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="15367-175">In addition, update all linked services that are related to the gateway by reentering their credentials.</span></span>

### <a name="8-problem"></a><span data-ttu-id="15367-176">8. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-176">8. Problem</span></span>
<span data-ttu-id="15367-177">Zobrazí se následující chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="15367-177">You might see the following error message.</span></span>

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a><span data-ttu-id="15367-178">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-178">Cause</span></span>
<span data-ttu-id="15367-179">Tato chyba se stane, když vaše brána je v prostředí, které vyžaduje proxy server HTTP přístup k internetovým prostředkům nebo heslo pro ověřování vašeho proxy serveru změní, ale není příslušným způsobem aktualizuje v bránu.</span><span class="sxs-lookup"><span data-stu-id="15367-179">This error happens when your gateway is in an environment that requires an HTTP proxy to access Internet resources, or your proxy's authentication password is changed but it's not updated accordingly in your gateway.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-180">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-180">Resolution</span></span>
<span data-ttu-id="15367-181">Postupujte podle pokynů v [důležité informace o Proxy serveru](#proxy-server-considerations) části tohoto článku a konfigurace nastavení proxy serveru s Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="15367-181">Follow the instructions in the [Proxy server considerations](#proxy-server-considerations) section of this article, and configure proxy settings with Data Management Gateway Configuration Manager.</span></span>

## <a name="gateway-is-online-with-limited-functionality"></a><span data-ttu-id="15367-182">Je brána online s omezenou funkčností</span><span class="sxs-lookup"><span data-stu-id="15367-182">Gateway is online with limited functionality</span></span>
### <a name="1-problem"></a><span data-ttu-id="15367-183">1. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-183">1. Problem</span></span>
<span data-ttu-id="15367-184">Stav brány, zobrazí jako online s omezenou funkčností.</span><span class="sxs-lookup"><span data-stu-id="15367-184">You see the status of the gateway as online with limited functionality.</span></span>

#### <a name="cause"></a><span data-ttu-id="15367-185">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-185">Cause</span></span>
<span data-ttu-id="15367-186">Zobrazí stav brány jako online s omezenou funkčností pro jednu z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="15367-186">You see the status of the gateway as online with limited functionality for one of the following reasons:</span></span>

* <span data-ttu-id="15367-187">Brána se nemůže připojit ke cloudové službě přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="15367-187">Gateway cannot connect to cloud service through Azure Service Bus.</span></span>
* <span data-ttu-id="15367-188">Cloudové služby se nemůže připojit k bráně přes službu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="15367-188">Cloud service cannot connect to gateway through Service Bus.</span></span>

<span data-ttu-id="15367-189">Když je brána online s omezenou funkčností, není možné pomocí Průvodce kopírování objektu pro vytváření dat k vytvoření datových kanálů pro kopírování dat do nebo z místní datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="15367-189">When the gateway is online with limited functionality, you might not be able to use the Data Factory Copy Wizard to create data pipelines for copying data to or from on-premises data stores.</span></span> <span data-ttu-id="15367-190">Jako alternativní řešení můžete použít Editor služby Data Factory v portálu, Visual Studio nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="15367-190">As a workaround, you can use Data Factory Editor in the portal, Visual Studio, or Azure PowerShell.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-191">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-191">Resolution</span></span>
<span data-ttu-id="15367-192">Řešení tohoto problému (online s omezenou funkčností) je založená na tom, jestli brána se nemůže připojit cloudové služby nebo jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="15367-192">Resolution for this issue (online with limited functionality) is based on whether the gateway cannot connect to the cloud service or the other way.</span></span> <span data-ttu-id="15367-193">Následující části obsahují tato řešení.</span><span class="sxs-lookup"><span data-stu-id="15367-193">The following sections provide these resolutions.</span></span>

### <a name="2-problem"></a><span data-ttu-id="15367-194">2. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-194">2. Problem</span></span>
<span data-ttu-id="15367-195">Zobrazí následující chyba.</span><span class="sxs-lookup"><span data-stu-id="15367-195">You see the following error.</span></span>

`Error: Gateway cannot connect to cloud service through service bus`

![Brána se nemůže připojit ke cloudové službě](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a><span data-ttu-id="15367-197">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-197">Cause</span></span>
<span data-ttu-id="15367-198">Brána se nemůže připojit ke cloudové službě přes službu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="15367-198">Gateway cannot connect to the cloud service through Service Bus.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-199">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-199">Resolution</span></span>
<span data-ttu-id="15367-200">Postupujte podle těchto kroků bránu zpátky do online režimu:</span><span class="sxs-lookup"><span data-stu-id="15367-200">Follow these steps to get the gateway back online:</span></span>

1. <span data-ttu-id="15367-201">Povolit IP adresy odchozí pravidla na počítači s bránou a podnikové brány firewall.</span><span class="sxs-lookup"><span data-stu-id="15367-201">Allow IP address outbound rules on the gateway machine and the corporate firewall.</span></span> <span data-ttu-id="15367-202">Můžete najít IP adresy z protokolu událostí systému Windows (ID == 401): došlo pokusu o přístup k soketu způsobem, jeho přístupovými oprávněními XX. XX. XX. XX:9350.</span><span class="sxs-lookup"><span data-stu-id="15367-202">You can find IP addresses from the Windows Event Log (ID == 401): An attempt was made to access a socket in a way forbidden by its access permissions XX.XX.XX.XX:9350.</span></span>
* <span data-ttu-id="15367-203">Konfigurace nastavení proxy serveru na bráně.</span><span class="sxs-lookup"><span data-stu-id="15367-203">Configure proxy settings on the gateway.</span></span> <span data-ttu-id="15367-204">Najdete v článku [důležité informace o Proxy serveru](#proxy-server-considerations) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="15367-204">See the [Proxy server considerations](#proxy-server-considerations) section for details.</span></span>
* <span data-ttu-id="15367-205">Povolte Odchozí porty 5671 a 9350-9354 na obou bránu Windows Firewall na počítači s bránou a podnikové brány firewall.</span><span class="sxs-lookup"><span data-stu-id="15367-205">Enable outbound ports 5671 and 9350-9354 on both the Windows Firewall on the gateway machine and the corporate firewall.</span></span> <span data-ttu-id="15367-206">Najdete v článku [porty a brány firewall](#ports-and-firewall) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="15367-206">See the [Ports and firewall](#ports-and-firewall) section for details.</span></span> <span data-ttu-id="15367-207">Tento krok je volitelný, ale doporučujeme ho důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="15367-207">This step is optional, but we recommend it for performance consideration.</span></span>

### <a name="3-problem"></a><span data-ttu-id="15367-208">3. Problém</span><span class="sxs-lookup"><span data-stu-id="15367-208">3. Problem</span></span>
<span data-ttu-id="15367-209">Zobrazí následující chyba.</span><span class="sxs-lookup"><span data-stu-id="15367-209">You see the following error.</span></span>

`Error: Cloud service cannot connect to gateway through service bus.`

#### <a name="cause"></a><span data-ttu-id="15367-210">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-210">Cause</span></span>
<span data-ttu-id="15367-211">Přechodná chyba v připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="15367-211">A transient error in network connectivity.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-212">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-212">Resolution</span></span>
<span data-ttu-id="15367-213">Postupujte podle těchto kroků bránu zpátky do online režimu:</span><span class="sxs-lookup"><span data-stu-id="15367-213">Follow these steps to get the gateway back online:</span></span>

1. <span data-ttu-id="15367-214">Počkejte několik minut, připojení se automaticky obnoví při chyba byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="15367-214">Wait for a couple of minutes, the connectivity will be automatically recovered when the error is gone.</span></span>
* <span data-ttu-id="15367-215">Pokud chyba přetrvává, restartujte službu brány.</span><span class="sxs-lookup"><span data-stu-id="15367-215">If the error persists, restart the gateway service.</span></span>

## <a name="failed-to-author-linked-service"></a><span data-ttu-id="15367-216">Nepodařilo se vytvořit propojenou službu</span><span class="sxs-lookup"><span data-stu-id="15367-216">Failed to author linked service</span></span>
### <a name="problem"></a><span data-ttu-id="15367-217">Problém</span><span class="sxs-lookup"><span data-stu-id="15367-217">Problem</span></span>
<span data-ttu-id="15367-218">Tato chyba se můžete setkat, když se pokusíte použít Správce přihlašovacích údajů na portálu a zadejte přihlašovací údaje pro nová propojená služba nebo aktualizujte pověření pro existující propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="15367-218">You might see this error when you try to use Credential Manager in the portal to input credentials for a new linked service, or update credentials for an existing linked service.</span></span>

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

<span data-ttu-id="15367-219">Když se tato chyba, stránka nastavení nástroje Data Management Gateway Configuration Manager může vypadat jako na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="15367-219">When you see this error, the settings page of Data Management Gateway Configuration Manager might look like the following screenshot.</span></span>

![Databázi nelze získat přístup.](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a><span data-ttu-id="15367-221">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-221">Cause</span></span>
<span data-ttu-id="15367-222">Certifikát SSL může byla v počítači s bránou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="15367-222">The SSL certificate might have been lost on the gateway machine.</span></span> <span data-ttu-id="15367-223">Počítač brány nelze načíst aktuálně certifikát, který se používá pro šifrování SSL.</span><span class="sxs-lookup"><span data-stu-id="15367-223">The gateway computer cannot load the certificate currently that is used for SSL encryption.</span></span> <span data-ttu-id="15367-224">Můžete si také prohlédnout chybovou zprávu do protokolu událostí, která se podobá následující zprávě.</span><span class="sxs-lookup"><span data-stu-id="15367-224">You might also see an error message in the event log that is similar to the following message.</span></span>

 `Unable to get the gateway settings from cloud service. Check the gateway key and the network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a><span data-ttu-id="15367-225">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-225">Resolution</span></span>
<span data-ttu-id="15367-226">Postupujte podle těchto kroků problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="15367-226">Follow these steps to solve the problem:</span></span>

1. <span data-ttu-id="15367-227">Spusťte Správce konfigurace brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="15367-227">Start Data Management Gateway Configuration Manager.</span></span>
2. <span data-ttu-id="15367-228">Přepnout **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="15367-228">Switch to the **Settings** tab.</span></span>  
3. <span data-ttu-id="15367-229">Klikněte **změnit** tlačítko změňte certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="15367-229">Click the **Change** button to change the SSL certificate.</span></span>

   ![Tlačítko změnit certifikát](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. <span data-ttu-id="15367-231">Vyberte nový certifikát jako certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="15367-231">Select a new certificate as the SSL certificate.</span></span> <span data-ttu-id="15367-232">Můžete použít libovolný certifikát SSL, který je generován můžete nebo některé z organizací.</span><span class="sxs-lookup"><span data-stu-id="15367-232">You can use any SSL certificate that is generated by you or any organization.</span></span>

   ![Zadejte certifikát](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a><span data-ttu-id="15367-234">Aktivita kopírování se nezdaří</span><span class="sxs-lookup"><span data-stu-id="15367-234">Copy activity fails</span></span>
### <a name="problem"></a><span data-ttu-id="15367-235">Problém</span><span class="sxs-lookup"><span data-stu-id="15367-235">Problem</span></span>
<span data-ttu-id="15367-236">Možná jste si všimli následující selhání "UserErrorFailedToConnectToSqlserver" po nastavení kanál v portálu.</span><span class="sxs-lookup"><span data-stu-id="15367-236">You might notice the following "UserErrorFailedToConnectToSqlserver" failure after you set up a pipeline in the portal.</span></span>

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

#### <a name="cause"></a><span data-ttu-id="15367-237">Příčina</span><span class="sxs-lookup"><span data-stu-id="15367-237">Cause</span></span>
<span data-ttu-id="15367-238">K tomu může dojít z různých důvodů a zmírnění se liší podle toho.</span><span class="sxs-lookup"><span data-stu-id="15367-238">This can happen for different reasons, and mitigation varies accordingly.</span></span>

#### <a name="resolution"></a><span data-ttu-id="15367-239">Řešení</span><span class="sxs-lookup"><span data-stu-id="15367-239">Resolution</span></span>
<span data-ttu-id="15367-240">Povolte odchozí připojení TCP přes port TCP 1433 nebo na straně klienta Brána pro správu dat před připojením k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="15367-240">Allow outbound TCP connections over port TCP/1433 on the Data Management Gateway client side before connecting to an SQL database.</span></span>

<span data-ttu-id="15367-241">Pokud je cílová databáze je databáze Azure SQL, zkontrolujte nastavení systému SQL Server brány firewall pro Azure také.</span><span class="sxs-lookup"><span data-stu-id="15367-241">If the target database is an Azure SQL database, check SQL Server firewall settings for Azure as well.</span></span>

<span data-ttu-id="15367-242">Najdete v následující části, abyste otestovali připojení k úložišti dat na místě.</span><span class="sxs-lookup"><span data-stu-id="15367-242">See the following section to test the connection to the on-premises data store.</span></span>

## <a name="data-store-connection-or-driver-related-errors"></a><span data-ttu-id="15367-243">Připojení nebo chyby související s ovladačů úložiště dat</span><span class="sxs-lookup"><span data-stu-id="15367-243">Data store connection or driver-related errors</span></span>
<span data-ttu-id="15367-244">Pokud se zobrazí data uložit připojení nebo chyby související s ovladačem, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15367-244">If you see data store connection or driver-related errors, complete the following steps:</span></span>

1. <span data-ttu-id="15367-245">Spusťte Správce konfigurace brány pro správu dat na počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="15367-245">Start Data Management Gateway Configuration Manager on the gateway machine.</span></span>
2. <span data-ttu-id="15367-246">Přepnout **diagnostiky** kartě.</span><span class="sxs-lookup"><span data-stu-id="15367-246">Switch to the **Diagnostics** tab.</span></span>
3. <span data-ttu-id="15367-247">V **Test připojení**, přidejte hodnoty skupiny brány.</span><span class="sxs-lookup"><span data-stu-id="15367-247">In **Test Connection**, add the gateway group values.</span></span>
4. <span data-ttu-id="15367-248">Klikněte na tlačítko **Test** chcete zobrazit, pokud je můžete připojit k místnímu zdroji dat z počítače brány pomocí informací o připojení a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="15367-248">Click **Test** to see if you can connect to the on-premises data source from the gateway machine by using the connection information and credentials.</span></span> <span data-ttu-id="15367-249">Pokud se testovací připojení nepodaří ani po instalaci ovladače, restartujte bránu, aby se získaly nejnovější změny.</span><span class="sxs-lookup"><span data-stu-id="15367-249">If the test connection still fails after you install a driver, restart the gateway for it to pick up the latest change.</span></span>

![Test připojení na kartě diagnostiky](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a><span data-ttu-id="15367-251">Protokoly Gateway</span><span class="sxs-lookup"><span data-stu-id="15367-251">Gateway logs</span></span>
### <a name="send-gateway-logs-to-microsoft"></a><span data-ttu-id="15367-252">Odeslání protokolů s brány Microsoft</span><span class="sxs-lookup"><span data-stu-id="15367-252">Send gateway logs to Microsoft</span></span>
<span data-ttu-id="15367-253">Při kontaktování Microsoft Support získat nápovědu k řešení potíží s brány možná sdílet svoje protokoly brány.</span><span class="sxs-lookup"><span data-stu-id="15367-253">When you contact Microsoft Support to get help with troubleshooting gateway issues, you might be asked to share your gateway logs.</span></span> <span data-ttu-id="15367-254">S vydáním brány můžete sdílet protokoly požadované gateway s dvěma kliknutí na tlačítko v Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="15367-254">With the release of the gateway, you can share required gateway logs with two button clicks in Data Management Gateway Configuration Manager.</span></span>    

1. <span data-ttu-id="15367-255">Přepnout **diagnostiky** kartě v Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="15367-255">Switch to the **Diagnostics** tab in Data Management Gateway Configuration Manager.</span></span>

    ![Karta diagnostiku brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. <span data-ttu-id="15367-257">Klikněte na tlačítko **odeslat protokoly** zobrazíte následující dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="15367-257">Click **Send Logs** to see the following dialog box.</span></span>

    ![Data Management Gateway odeslat protokoly](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. <span data-ttu-id="15367-259">(Volitelné) Klikněte na tlačítko **zobrazit protokoly** ke kontrole protokoluje události prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="15367-259">(Optional) Click **view logs** to review logs in the event viewer.</span></span>
4. <span data-ttu-id="15367-260">(Volitelné) Klikněte na tlačítko **o ochraně osobních údajů** ke kontrole prohlášení o ochraně osobních údajů služeb Microsoft web services.</span><span class="sxs-lookup"><span data-stu-id="15367-260">(Optional) Click **privacy** to review Microsoft web services privacy statement.</span></span>
5. <span data-ttu-id="15367-261">Až budete spokojeni se chystáte se nahrát, klikněte na tlačítko **odeslat protokoly** ve skutečnosti odeslat protokoly z posledních sedmi dnů společnosti Microsoft pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="15367-261">When you are satisfied with what you are about to upload, click **Send Logs** to actually send the logs from the last seven days to Microsoft for troubleshooting.</span></span> <span data-ttu-id="15367-262">Stav operace odesílání protokolů byste měli vidět, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="15367-262">You should see the status of the send-logs operation as shown in the following screenshot.</span></span>

    ![Data Management Gateway odeslat protokoly stavu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. <span data-ttu-id="15367-264">Po dokončení operace se zobrazí dialogové okno, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="15367-264">After the operation is complete, you see a dialog box as shown in the following screenshot.</span></span>

    ![Data Management Gateway odeslat protokoly stavu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. <span data-ttu-id="15367-266">Uložit **ID sestavy** a sdílet je s Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="15367-266">Save the **Report ID** and share it with Microsoft Support.</span></span> <span data-ttu-id="15367-267">ID sestavy slouží k vyhledání brány protokoly, které jste nahráli k řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="15367-267">The report ID is used to locate the gateway logs that you uploaded for troubleshooting.</span></span>  <span data-ttu-id="15367-268">ID sestavy se také uloženy události prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="15367-268">The report ID is also saved in the event viewer.</span></span>  <span data-ttu-id="15367-269">Můžete najít prohlížením událost s ID "25" a zkontrolujte datum a čas.</span><span class="sxs-lookup"><span data-stu-id="15367-269">You can find it by looking at the event ID “25”, and check the date and time.</span></span>

    ![Data Management Gateway odeslat protokoly ID sestavy](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a><span data-ttu-id="15367-271">Archiv brány protokoly na hostitelském počítači brány</span><span class="sxs-lookup"><span data-stu-id="15367-271">Archive gateway logs on gateway host machine</span></span>
<span data-ttu-id="15367-272">Je několik scénářů, kdy máte problémy s brány a protokoly gateway nemohou sdílet přímo:</span><span class="sxs-lookup"><span data-stu-id="15367-272">There are some scenarios where you have gateway issues and you cannot share gateway logs directly:</span></span>

* <span data-ttu-id="15367-273">Ručně nainstalujte brány a zaregistrovat bránu.</span><span class="sxs-lookup"><span data-stu-id="15367-273">You manually install the gateway and register the gateway.</span></span>
* <span data-ttu-id="15367-274">Pokusíte registrovat bránu pomocí obnoveného klíče v Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="15367-274">You try to register the gateway with a regenerated key in Data Management Gateway Configuration Manager.</span></span>
* <span data-ttu-id="15367-275">Zkuste odeslat protokoly a nemůže být připojen hostitelskou službu brány.</span><span class="sxs-lookup"><span data-stu-id="15367-275">You try to send logs and the gateway host service cannot be connected.</span></span>

<span data-ttu-id="15367-276">Pro tyto scénáře můžete uložit protokoly gateway jako soubor zip a sdílet ho při kontaktujte podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="15367-276">For these scenarios, you can save gateway logs as a zip file and share it when you contact Microsoft support.</span></span> <span data-ttu-id="15367-277">Například pokud obdržíte chybu při registraci brány jako ukazuje následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="15367-277">For example, if you receive an error while you register the gateway as shown in the following screenshot.</span></span>   

![Chyba registrace brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

<span data-ttu-id="15367-279">Klikněte na tlačítko **archivu protokoly gateway** propojit archivaci a uložte protokoly a potom sdílet soubor zip s podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="15367-279">Click the **Archive gateway logs** link to archive and save logs, and then share the zip file with Microsoft support.</span></span>

![Protokoly archivu brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a><span data-ttu-id="15367-281">Najít protokoly gateway</span><span class="sxs-lookup"><span data-stu-id="15367-281">Locate gateway logs</span></span>
<span data-ttu-id="15367-282">Podrobné brány protokolu informace naleznete v protokolech událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="15367-282">You can find detailed gateway log information in the Windows event logs.</span></span>

1. <span data-ttu-id="15367-283">Spustit systém Windows **Prohlížeč událostí**.</span><span class="sxs-lookup"><span data-stu-id="15367-283">Start Windows **Event Viewer**.</span></span>
2. <span data-ttu-id="15367-284">Vyhledejte v protokolech **protokoly aplikací a služeb** > **Brána pro správu dat** složky.</span><span class="sxs-lookup"><span data-stu-id="15367-284">Locate logs in the **Application and Services Logs** > **Data Management Gateway** folder.</span></span>

 <span data-ttu-id="15367-285">Pokud se řešení potíží s problémy související s brány, vyhledejte chyby úrovně události události prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="15367-285">When you're troubleshooting gateway-related issues, look for error level events in the event viewer.</span></span>

![Brána pro správu dat protokoly v prohlížeči událostí](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
