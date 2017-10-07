---
title: "Vytvoření hybridní kolekce vzdálené aplikace RemoteApp aaaTroubleshoot | Microsoft Docs"
description: "Zjistěte, jak selhání vytvoření tootroubleshoot vzdálené aplikace RemoteApp hybridní kolekce"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: b32033ee-8d52-4e74-bb78-86ca873c34e2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: cc426f24bd0c349a8862d54acbafa9cf84446f4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Řešení potíží s vytvářením hybridních kolekcí Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Hybridní kolekce je hostován v a ukládá data v hello cloudu Azure, ale také umožňuje uživatelům přistupovat k datům a prostředkům uloženým v místní síti. Uživatelé mohou získat přístup k aplikacím přihlášením pomocí svých podnikových přihlašovacích údajů synchronizovaných nebo sdružených se službou Azure Active Directory. Můžete nasadit hybridní kolekci, která použije existující virtuální síť Azure, nebo můžete vytvořit novou virtuální síť. Doporučujeme vytvořit nebo použít podsíť virtuální sítě s rozsahem CIDR dostatečně velký pro očekávané budoucímu růstu pro Azure RemoteApp.

Vaší kolekci ještě nevytvořili? V tématu [vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md) hello kroky.

Pokud máte potíže při vytvoření kolekce, nebo pokud hello kolekce nepracuje hello způsob, jak si myslíte, že by měl, podívejte se na hello následující informace.

## <a name="your-image-is-invalid"></a>Bitové kopie je neplatný
Pokud při čekání Azure tooprovision kolekce se zobrazí zpráva podobná "GoldImageInvalid", znamená to, že image šablony nesplňuje hello [definované požadavky na image](remoteapp-imagereqs.md). Ano, přejděte číst ty [požadavky](remoteapp-imagereqs.md)opravte bitové kopie a akci toocreate kolekci znovu.

## <a name="does-your-vnet-have-network-security-groups-defined"></a>Má vaše virtuální sítě definované skupiny zabezpečení sítě?
Pokud máte skupiny zabezpečení sítě definované v podsíti hello používáte pro svoji kolekci, ujistěte se, tyto [adres URL a portů](remoteapp-ports.md) jsou k dispozici v rámci dané podsítě.

Můžete přidat další síťové zabezpečení skupiny toohello nasazené virtuální počítače od vás v hello podsíť pro náročnější ovládacího prvku.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Používáte vlastní servery DNS? A že jsou přístupné z podsíť virtuální sítě?
> [!NOTE]
> Máte toomake zda hello, servery DNS ve vaší virtuální síti jsou vždy nahoru a vždy možné tooresolve hello virtuální počítače hostované v hello virtuální sítě. Nepoužívejte Google DNS pro tento.
> 
> 

Pro hybridní kolekce použijete vlastní servery DNS. Můžete je zadat ve schématu konfigurace vaší sítě nebo prostřednictvím portálu pro správu hello při vytváření virtuální sítě. Servery DNS se používají v pořadí hello, že jsou zadané způsobem převzetí služeb při selhání (jako názvem na rozdíl od tooround robin).  
Naleznete příliš[překlad názvů pro virtuální počítače a instance rolí](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) toomake se vaše servery DNS jsou nakonfigurované correcly.

Ujistěte se, že nejsou servery DNS hello kolekce přístupný a dostupný z podsítě virtuální sítě hello, které zadáte pro tuto kolekci.

Například:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definování DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Používáte řadič domény služby Active Directory v kolekci?
Aktuálně jen jednu doménu služby Active Directory může být přidružen Azure RemoteApp. Hello hybridní kolekce podporuje pouze účty Azure Active Directory, které byly synchronizovány pomocí nástroje DirSync z nasazení systému Windows Server Active Directory; Konkrétně buď synchronizované s možností synchronizace hesel hello nebo synchronizaci se službou federation Active Directory Federation Services (AD FS) nakonfigurované. Potřebujete toocreate vlastní doménu, která odpovídá příponu domény hello UPN pro místní doméně a nastavení integrace adresáře.

V tématu [konfigurace služby Active Directory pro Azure RemoteApp](remoteapp-ad.md) Další informace.

Zajistěte, aby podrobnosti hello domény zadali jsou platné a je hello řadič domény dostupný z hello vytvoření virtuálního počítače v podsíti hello používá pro vzdálenou aplikaci Azure. Také zkontrolujte, zda účet služby hello pověření mít oprávnění tooadd počítače toohello zadaný domény a zda text hello název Active Directory zadaný lze vyřešit z hello DNS součástí hello virtuální sítě.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Zadejte název domény byl zadán při vytváření kolekce?
název domény Hello vytvořit nebo přidat musí být název interní domény (není název domény služby Azure AD) a musí být ve formátu DNS je možné přeložit (contoso.local). Například máte interní název služby Active Directory (contoso.local) a Active Directory UPN (contoso.com) - máte toouse hello interní název při vytváření kolekce.

