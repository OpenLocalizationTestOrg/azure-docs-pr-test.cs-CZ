Azure nabízí hello následující SKU služby VPN gateway:

|**SKU**   | **Tunely<br> S2S/VNet-to-VNet** | **Připojení<br>P2S** | **Srovnávací test<br>agregované propustnosti** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Max. 30                         | Max. 128               | 650 Mb/s                    |
|**VpnGw2**| Max. 30                         | Max. 128               | 1 Gb/s                      |
|**VpnGw3**| Max. 30                         | Max. 128               | 1,25 Gb/s                   |
|**Basic** | Max. 10                         | Max. 128               | 100 Mb/s                    | 
|          |                                 |                        |                             | 

- Srovnávací test agregované propustnosti je založen na měření více tunelů agregovaných prostřednictvím jedné brány. Není zaručenou propustnost kvůli tooInternet provoz podmínky a chování vaší aplikace.

- Informace o cenách najdete na hello [cenová](https://azure.microsoft.com/pricing/details/vpn-gateway) stránky.

- Informace o dokumentu SLA (smlouvy o úrovni služeb) můžete najít na hello [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) stránky.
