# mqmaker-witi-sata-wifi-pcie-x3

Hallo,

Eine Lösung für das Poblem das die zwei sata Anschlüsse nicht gehen.

**mt7621-pci 1e140000.pcie: PCIE0 enabled	Wlan ac		gpio 19 PERST_N  
  mt7621-pci 1e140000.pcie: PCIE1 enabled	Wlan bgn	gpio 8  RXD3  
  mt7621-pci 1e140000.pcie: PCIE2 enabled	SATA		gpio 7  TXD3** 
  
# target/linux/ramips/dts/mt7621.dtsi

------------------------------------------------------------------  

dtschosen {  bootargs = "console=ttyS0,57600 pci=realloc";};  

/* GPIO's steuert hier den Reset des externen wlan-Chips sata-chips und evtl gesamte pcie*/  

reset-gpios = <&gpio 19 GPIO_ACTIVE_LOW>, <&gpio 8 GPIO_ACTIVE_LOW>, <&gpio 7 GPIO_ACTIVE_LOW>;  

-------------------------------------------------------------------  




.....
weitere pcie und rom ???

-------------------------------------------------------------------
