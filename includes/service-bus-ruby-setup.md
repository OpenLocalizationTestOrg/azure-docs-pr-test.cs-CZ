## <a name="create-a-ruby-application"></a><span data-ttu-id="7aa6c-101">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="7aa6c-101">Create a Ruby application</span></span>
<span data-ttu-id="7aa6c-102">Pokyny najdete v tématu [vytvořit Ruby aplikace na platformě Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="7aa6c-102">For instructions, see [Create a Ruby Application on Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="7aa6c-103">Konfigurace aplikace pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="7aa6c-103">Configure Your application to Use Service Bus</span></span>
<span data-ttu-id="7aa6c-104">Použití služby Service Bus, stáhnout a použít balíček Azure Ruby, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.</span><span class="sxs-lookup"><span data-stu-id="7aa6c-104">To use Service Bus, download and use the Azure Ruby package, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="7aa6c-105">Použití RubyGems získat balíček</span><span class="sxs-lookup"><span data-stu-id="7aa6c-105">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="7aa6c-106">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="7aa6c-106">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="7aa6c-107">"Gem instalace azure" zadejte v příkazovém okně instalace gem a závislostí.</span><span class="sxs-lookup"><span data-stu-id="7aa6c-107">Type "gem install azure" in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="7aa6c-108">Import balíčku</span><span class="sxs-lookup"><span data-stu-id="7aa6c-108">Import the package</span></span>
<span data-ttu-id="7aa6c-109">Na začátek souboru Ruby, ve kterém chcete používat úložiště pomocí svém oblíbeném textovém editoru, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="7aa6c-109">Using your favorite text editor, add the following to the top of the Ruby file in which you intend to use storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="7aa6c-110">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="7aa6c-110">Set up a Service Bus connection</span></span>
<span data-ttu-id="7aa6c-111">Pomocí následujícího kódu nastavit hodnoty oboru názvů a název klíče, klíče, podepisující osoba a hostitele:</span><span class="sxs-lookup"><span data-stu-id="7aa6c-111">Use the following code to set the values of namespace, name of the key, key, signer and host:</span></span>

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

<span data-ttu-id="7aa6c-112">Nastavte hodnotu oboru názvů na hodnotu, kterou jste vytvořili místo celého adresy URL.</span><span class="sxs-lookup"><span data-stu-id="7aa6c-112">Set the namespace value to the value you created rather than the entire URL.</span></span> <span data-ttu-id="7aa6c-113">Například použít **"yourexamplenamespace"**, není "yourexamplenamespace.servicebus.windows.net".</span><span class="sxs-lookup"><span data-stu-id="7aa6c-113">For example, use **"yourexamplenamespace"**, not "yourexamplenamespace.servicebus.windows.net".</span></span>
