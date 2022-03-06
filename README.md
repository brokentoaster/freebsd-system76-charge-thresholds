# freebsd-system76-charge-thresholds
Simple Rc script to setup the charge thresholds on the 2020 Lemur Pro (LEMP9) laptop.

Default settings will stop charging the battery at 80% charge and begin to charge again when the battery level dips below 50%.
This behaviour will maximize the long term battery lifetime at the cost of day to day battery life.

To install copy to `/usr/local/etc/rc.d/` and set `s76_charge_thresholds_enable=YES` in your rc.conf

This requires the [sysutils/acpi_call](https://www.freshports.org/sysutils/acpi_call/) package/port to be installed.

```
# pkg install sysutils/acpi_call
# kldload acpi_call.ko
`
