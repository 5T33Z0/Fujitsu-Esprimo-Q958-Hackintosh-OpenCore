# Fujitsu Esprimo Q958 Mini Hackintosh

## USB Port Mapping (v5)

<img width="1111" height="779" alt="Ports_v5" src="https://github.com/user-attachments/assets/ea0fd2a5-019e-4df8-a0db-b08372983a78" />

## SSDT-XHUB

```asl

DefinitionBlock ("", "SSDT", 2, "OCLT", "XHUB", 0x00000000)
{
    External (_SB_.PCI0.XHC_.RHUB, DeviceObj)
    External (OSDW, MethodObj)    // 0 Arguments

    Scope (\_SB.PCI0.XHC.RHUB)
    {
        Method (_STA, 0, NotSerialized)  // _STA: Status
        {
            If (OSDW ())
            {
                Return (Zero)
            }
            Else
            {
                Return (0x0F)
            }
        }
    }

    Device (\_SB.PCI0.XHC.XHUB)
    {
        Name (_ADR, Zero)  // _ADR: Address
        Method (_STA, 0, NotSerialized)  // _STA: Status
        {
            If (OSDW ())
            {
                Return (0x0F)
            }
            Else
            {
                Return (Zero)
            }
        }

        // HS01: Enabled — USB 2.0
        Device (HS01)
        {
            Name (_ADR, One)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS02: Enabled — USB 2.0
        Device (HS02)
        {
            Name (_ADR, 0x02)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS03: Disabled
        Device (HS03)
        {
            Name (_ADR, 0x03)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    Zero, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS04: Disabled
        Device (HS04)
        {
            Name (_ADR, 0x04)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    Zero, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS05: Enabled — USB 2.0
        Device (HS05)
        {
            Name (_ADR, 0x05)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS06: Enabled — USB 2.0
        Device (HS06)
        {
            Name (_ADR, 0x06)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS07: Disabled
        Device (HS07)
        {
            Name (_ADR, 0x07)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    Zero, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS08: Enabled — USB 2.0
        Device (HS08)
        {
            Name (_ADR, 0x08)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    Zero, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS10: Enabled — Internal
        Device (HS10)
        {
            Name (_ADR, 0x0A)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0x0A, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // HS14: Enabled — Internal/Bluetooth
        Device (HS14)
        {
            Name (_ADR, 0x0E)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0xFF, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS01: Enabled — USB 3.x
        Device (SS01)
        {
            Name (_ADR, 0x11)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x11)
                {
                    0xFF, 
                    0x03, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS02: Enabled — USB 3.x
        Device (SS02)
        {
            Name (_ADR, 0x12)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x12)
                {
                    0xFF, 
                    0x03, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS03: Enabled — USB 3.x
        Device (SS03)
        {
            Name (_ADR, 0x13)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0x03, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS04: Enabled — USB 3.x
        Device (SS04)
        {
            Name (_ADR, 0x14)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0x03, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS05: Enabled — Internal
        Device (SS05)
        {
            Name (_ADR, 0x15)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0x0A, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS06: Enabled — Internal
        Device (SS06)
        {
            Name (_ADR, 0x16)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0x0A, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS07: Enabled — USB 3.x
        Device (SS07)
        {
            Name (_ADR, 0x17)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0x03, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }

        // SS08: Enabled — USB 3.x
        Device (SS08)
        {
            Name (_ADR, 0x18)  // _ADR: Address
            Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
            {
                Return (Package (0x04)
                {
                    0xFF, 
                    0x03, 
                    Zero, 
                    Zero
                })
            }

            Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
            {
                Return (Package (0x01)
                {
                    Buffer (0x10)
                    {
                        /* 0000 */  0x81, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                        /* 0008 */  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00   // ........
                    }
                })
            }
        }
    }
}

