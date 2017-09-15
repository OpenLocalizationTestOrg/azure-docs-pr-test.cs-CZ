<span data-ttu-id="b0bc3-101">Každý klientský počítač, který se připojuje k virtuální síti pomocí připojení Point-to-Site, musí mít nainstalovaný klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-101">Each client computer that connects to a VNet using Point-to-Site must have a client certificate installed.</span></span> <span data-ttu-id="b0bc3-102">Klientský certifikát se generuje z kořenového certifikátu a nainstaluje na každý klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-102">The client certificate is generated from the root certificate and installed on each client computer.</span></span> <span data-ttu-id="b0bc3-103">Pokud není nainstalovaný platný klientský certifikát a klient se pokusí připojit k virtuální síti, ověření se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-103">If a valid client certificate is not installed and the client tries to connect to the VNet, authentication fails.</span></span>

<span data-ttu-id="b0bc3-104">Můžete buď vygenerovat jedinečný certifikát pro každého klienta, nebo můžete použít stejný certifikát pro více klientů.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-104">You can either generate a unique certificate for each client, or you can use the same certificate for multiple clients.</span></span> <span data-ttu-id="b0bc3-105">Výhodou generování jedinečných certifikátů pro klienty je možnost jednotlivý certifikát odvolat.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-105">The advantage to generating unique client certificates is the ability to revoke a single certificate.</span></span> <span data-ttu-id="b0bc3-106">V opačném případě, pokud více klientů používá stejný klientský certifikát a vy ho potřebujete odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, kteří používají tento certifikát k ověření.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-106">Otherwise, if multiple clients are using the same client certificate and you need to revoke it, you have to generate and install new certificates for all the clients that use that certificate to authenticate.</span></span>

<span data-ttu-id="b0bc3-107">Klientské certifikáty můžete vygenerovat následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="b0bc3-107">You can generate client certificates using the following methods:</span></span>

- <span data-ttu-id="b0bc3-108">**Podnikový certifikát:**</span><span class="sxs-lookup"><span data-stu-id="b0bc3-108">**Enterprise certificate:**</span></span>

  - <span data-ttu-id="b0bc3-109">Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným názvem ve formátu name@yourdomain.com (namísto formátu název_domény\uživatelské_jméno).</span><span class="sxs-lookup"><span data-stu-id="b0bc3-109">If you are using an enterprise certificate solution, generate a client certificate with the common name value format 'name@yourdomain.com', rather than the 'domain name\username' format.</span></span>
  - <span data-ttu-id="b0bc3-110">Ujistěte se, že je certifikát založený na šabloně uživatelského certifikátu, která má jako první položku v seznamu používání Ověření klienta místo Přihlášení pomocí čipové karty atd. Certifikát můžete zkontrolovat dvojím kliknutím na klientský certifikát a zobrazením **Podrobnosti > Použití rozšířeného klíče**.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-110">Make sure the client certificate is based on the 'User' certificate template that has 'Client Authentication' as the first item in the use list, rather than Smart Card Logon, etc. You can check the certificate by double-clicking the client certificate and viewing **Details > Enhanced Key Usage**.</span></span>

- <span data-ttu-id="b0bc3-111">**Kořenový certifikát podepsaný svým držitelem:** Je důležité, abyste postupovali podle pokynů v některém z níže uvedených článků věnujících se certifikátům Point-to-Site.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-111">**Self-signed root certificate:** It's important that you follow the steps in one of the P2S certificate articles below.</span></span> <span data-ttu-id="b0bc3-112">Jinak klientské certifikáty, které vytvoříte, nebudou kompatibilní s připojeními typu Point-to-Site a klienti při pokusu o připojení obdrží chybu.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-112">Otherwise, the client certificates you create won't be compatible with P2S connections and clients receive an error when trying to connect.</span></span> <span data-ttu-id="b0bc3-113">Kompatibilní klientský certifikát můžete vytvořit pomocí postupu v jednom z následujících článků:</span><span class="sxs-lookup"><span data-stu-id="b0bc3-113">The steps in either of the following articles generate a compatible client certificate:</span></span> 

  * <span data-ttu-id="b0bc3-114">[Pokyny pro Windows 10 PowerShell:](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert) Tyto pokyny pro generování certifikátů vyžadují Windows 10 a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-114">[Windows 10 PowerShell instructions](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): These instructions require Windows 10 and PowerShell to generate certificates.</span></span> <span data-ttu-id="b0bc3-115">Vygenerované certifikáty můžete nainstalovat na jakémkoli podporovaném klientu Point-to-Site.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-115">The certificates that are generated can be installed on any supported P2S client.</span></span>
  * <span data-ttu-id="b0bc3-116">[Pokyny pro MakeCert:](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) Použijte MakeCert, pokud nemáte přístup k počítači s Windows 10, který byste mohli použít ke generování certifikátů.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-116">[MakeCert instructions](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Use MakeCert if you don't have access to a Windows 10 computer to use to generate certificates.</span></span> <span data-ttu-id="b0bc3-117">Nástroj MakeCert je zastaralý, ale stále ho můžete použít ke generování certifikátů.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-117">MakeCert deprecated, but you can still use MakeCert to generate certificates.</span></span> <span data-ttu-id="b0bc3-118">Vygenerované certifikáty můžete nainstalovat na jakémkoli podporovaném klientu Point-to-Site.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-118">The certificates that are generated can be installed on any supported P2S client.</span></span>

  <span data-ttu-id="b0bc3-119">Pokud generujete klientský certifikát pomocí kořenového certifikátu podepsaného svým držitelem podle předchozích pokynů, automaticky se nainstaluje na počítač, který jste použili k jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-119">When you generate a client certificate from a self-signed root certificate using the preceding instructions, it's automatically installed on the computer that you used to generate it.</span></span> <span data-ttu-id="b0bc3-120">Pokud chcete klientský certifikát nainstalovat na jiný klientský počítač, musíte ho vyexportovat jako soubor .pfx spolu s úplným řetězem certifikátů.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-120">If you want to install a client certificate on another client computer, you need to export it as a .pfx, along with the entire certificate chain.</span></span> <span data-ttu-id="b0bc3-121">Tím se vytvoří soubor .pfx, který obsahuje informace o kořenovém certifikátu potřebné pro úspěšné ověření klienta.</span><span class="sxs-lookup"><span data-stu-id="b0bc3-121">This creates a .pfx file that contains the root certificate information that is required for the client to successfully authenticate.</span></span> <span data-ttu-id="b0bc3-122">Postup pro export certifikátu najdete v tématu [Certifikáty – export klientského certifikátu](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).</span><span class="sxs-lookup"><span data-stu-id="b0bc3-122">For steps to export a certificate, see [Certificates - export a client certificate](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).</span></span>