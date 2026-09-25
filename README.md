# mqmaker-witi-sata-wifi-pcie-x3

Hallo,

Eine Lösung für das Poblem das die zwei sata Anschlüsse nicht gehen.

**mt7621-pci 1e140000.pcie: PCIE0 enabled	Wlan ac		gpio 19 PERST_N
  mt7621-pci 1e140000.pcie: PCIE1 enabled	Wlan bgn	gpio 8  RXD3 
  mt7621-pci 1e140000.pcie: PCIE2 enabled	SATA		gpio 7  TXD3** 

target/linux/ramips/dts/mt7621.dtsi
------------------------------------------------------------------
dtschosen {
    bootargs = "console=ttyS0,57600 pci=realloc";
};
/* GPIO's steuert hier den Reset des externen wlan-Chips sata-chips und evtl gesamte pcie*/
reset-gpios = <&gpio 19 GPIO_ACTIVE_LOW>, <&gpio 8 GPIO_ACTIVE_LOW>, <&gpio 7 GPIO_ACTIVE_LOW>;
-------------------------------------------------------------------




.....
weitere pcie und rom ???

target/linux/ramips/dts/mt7621_mqmaker_witi.dts

-------------------------------------------------------------------
partition@50000 {
				compatible = "denx,uimage";
				label = "firmware";
				reg = <0x50000 0x1fb0000>; /*+++geändert von 0xfb0000 16M -> 32M*/
			};

&pcie0 {
	/* GPIO 19 steuert hier den Reset des externen ac-wlan-Chips und evtl gesamte pcie*/
	reset-gpios = <&gpio 19 GPIO_ACTIVE_LOW>;
	wifi@0,0 {
		compatible = "mediatek,mt76";
		reg = <0x0000 0 0 0 0>;
		ieee80211-freq-limit = <5000000 6000000>;
		nvmem-cells = <&eeprom_factory_8000>, <&macaddr_factory_e000 0>;
		nvmem-cell-names = "eeprom", "mac-address";
	};
};

&pcie1 {
	reset-gpios = <&gpio 8 GPIO_ACTIVE_LOW>;
	/* GPIO 8 steuert hier den Reset des externen bgn-wlan Chips */
	wifi@0,0 {
		compatible = "mediatek,mt76";
		reg = <0x0000 0 0 0 0>;
		ieee80211-freq-limit = <2400000 2500000>;
		nvmem-cells = <&eeprom_factory_0>, <&macaddr_factory_e000 0>;
		nvmem-cell-names = "eeprom", "mac-address";
	};
};

&pcie2 {
	reset-gpios = <&gpio 7 GPIO_ACTIVE_LOW>;
	status = "okay";
	/* GPIO 7 steuert hier den Reset des externen SATA-Chips */ 
};

--------------------------------------------------------------------

.....
16M -> 32M

/target/linux/ramips/image/mt7621.mk

--------------------------------------------------------------------
define Device/mqmaker_witi
  $(Device/dsa-migration)
  IMAGE_SIZE := 32448k
  DEVICE_VENDOR := MQmaker
  DEVICE_MODEL := WiTi
  DEVICE_PACKAGES := kmod-ata-ahci kmod-mt76x2 kmod-sdhci-mt7620 kmod-usb3 \
        kmod-usb-ledtrig-usbport
  SUPPORTED_DEVICES += witi mqmaker,witi-256m mqmaker,witi-512m
endef
TARGET_DEVICES += mqmaker_witi
-------------------------------------------------------------------------------
