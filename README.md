# freebsd-system76-charge-thresholds
Simple rc script to setup the charge thresholds on the 2020 Lemur Pro (LEMP9) laptop on FreeBSD 13.0

Default settings will stop charging the battery at 80% charge and begin to charge again when the battery level dips below 50%.
This behaviour will maximize the long term battery _lifespan_ at the cost of day to day battery life.

Plenty of discussion around this can be found [here](https://github.com/pop-os/gnome-control-center/issues/101)


To install copy to `/usr/local/etc/rc.d/` and set `s76_charge_thresholds_enable=YES` in your rc.conf

you can also tweak the following values in your `rc.conf`:

`s76_charge_thresholds_stop_at` (0-100) percentage to stop charging at, default is 80.
`s76_charge_thresholds_start_at` (0-100) percentage to start charging at, default is 50.
`s76_charge_thresholds_quiet` (YES,NO) yes for less verbose output in the dmesg at startup, default is no.


This requires the [sysutils/acpi_call](https://www.freshports.org/sysutils/acpi_call/) package/port to be installed.

```
# pkg install sysutils/acpi_call
# kldload acpi_call.ko
```

The exact ACPI calls were revers engineered through reading 
[issue to add run time setting facility](https://github.com/system76/ec/issues/62) to the 
[embedded Controller firmware](https://github.com/system76/ec/commit/dabda167426644f78404fa8f459ea366b3d6a804)
and `acpidump -dt` which gives

```
        Field (ERAM, ByteAcc, Lock, Preserve)
        {
            Offset (0xBC),
            BTL0,   8,
            BTH0,   8
        }

        Method (GBCT, 1, NotSerialized)
        {
            If ((Arg0 == Zero))
            {
                Return (BTL0) /* \_SB_.PCI0.LPCB.EC0_.BTL0 */
            }

            If ((Arg0 == One))
            {
                Return (BTH0) /* \_SB_.PCI0.LPCB.EC0_.BTH0 */
            }

            Return (0xFF)
        }

        Method (SBCT, 2, NotSerialized)
        {
            If ((Arg1 <= 0x64))
            {
                If ((Arg0 == Zero))
                {
                    BTL0 = Arg1
                }

                If ((Arg0 == One))
                {
                    BTH0 = Arg1
                }
            }
        }

```