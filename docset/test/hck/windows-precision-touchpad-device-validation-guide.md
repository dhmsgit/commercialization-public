---
author: joshbax-msft
title: Windows Precision Touchpad Device Validation Guide
description: Windows Precision Touchpad Device Validation Guide
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: a357b1f8-e29a-4495-929d-5340be13a9c6
---

# Windows Precision Touchpad Device Validation Guide


This topic provides information about how to validate a Windows Precision Touchpad. It provides guidelines to ensure compliance with the Windows Precision Touchpad Windows Hardware Certification Kit (Windows HCK) testing.

This information applies to Windows 8.1.

Each test that is described in this topic is available through the Windows HCK.

**In this topic:**

-   [General test guidelines](#gen)

-   [PTLogo interface](#ptlogo)

-   [PTLogo command line switches](#switches)

-   [USB Selective Suspend](#selectsuspend)

**In this section:**

-   [Precision Touchpad Tests](precision-touchpad-tests.md)

-   [Precision Touchpad Test Error Messages](precision-touchpad-test-error-messages.md)

## <a href="" id="gen"></a>General test guidelines


-   Unless otherwise specified, the device should always have AC power when you are performing Windows HCK tests.

-   Unless otherwise specified, all tests should be run with “Test Signing Mode” ON:

    1.  Open a command prompt that has elevated privileges.

    2.  Run the following command: `bcdedit –set testsigning on`

    3.  Restart the test machine.

-   Unless otherwise specified, use 7mm diameter contacts for tests that require the use of the PT3 (Precision Touch Testing Tool).

-   Unless otherwise specified, all numbers in error messages that report distance or location are in himetric (0.01mm).

-   The grids in the PTLogo visualization help align vertices (for tests like Linearity), and graph distances. The distance from one line to the next is 200 in himetric, or 2mm.

-   Unless otherwise specified, multiple contacts should maintain a minimum separation distance:

    -   If vertically or horizontally aligned, contacts should be at least 11mm apart during test.

    -   If diagonally aligned, contacts should be at least 14mm apart during test.

-   The path to **GetThqaBlob.exe** on the Windows HCK controller is **C:\\Program Files (x86)\\Windows Kits\\8.1\\Hardware Certification Kit\\Tests\\x86\\digitizer\\Win8Touch**. All necessary files are copied to test machines when the Windows HCK client is installed.

-   Unless otherwise specified, you should disable Selective Suspend to validate USB PTP devices, with exceptions for Static Validation, Edge, Field Firmware Upgradeable (FFU), and Power State Transition tests.

-   For all tests that use the PT3, the PT3 must be axis-aligned to the sensor lines as closely as possible. To do this:

    -   Start with the device secured to the table, and turn the table to 0 or 90 degrees, depending on the test’s requirement.

    -   Because the touchpad might not be perfectly aligned within the device, start PTLogo and use the visualization to make sure that the contact’s path is completely parallel to the visualization grid lines.

        **Note**  
        Pressing **C** in PTLogo will clear the visualization if you start to fill up the screen. If you started PTLogo with the actual test to be run, press **R** after the sensor is aligned to start at the beginning of the test.

         

    -   If there is any drift off axis, use the fine-tune knob to slightly turn the table, as shown in *Figure 1 Fine Adjustment for Axis Alignment*.

        ![fine adjustment axis alignment](images/fineadjustmentaxisalignment.jpg)

        **Figure 1 Fine Adjustment for Axis Alignment**

    -   Repeat as needed until there is no observable drift from axis.

    -   For example, observe how the path in *Figure 2 Visualization Grid Iterations* is adjusted over multiple iterations until the contact’s path is parallel to the visualization grid:

        ![parallel gridlines](images/parallelgridlines1.jpg)

        **Figure 2 Visualization Grid Iterations**

    -   After the path is parallel to the visualization grid lines on one side of the touchpad, run it again on the other side, and make sure it still moves parallel to the grid lines.

        ![parallel gridlines no drift](images/parallelgridlines2.jpg)

        **Figure 3 Parallel Gridlines**

## <a href="" id="ptlogo"></a>PTLogo interface


-   To manually pass an iteration (where applicable), press **P** on the keyboard.

-   To manually fail an iteration (where applicable), press **F** on the keyboard.

-   To manually restart the test (where applicable), press **R** on the keyboard.

-   To manually fail the test (where applicable), press **T** on the keyboard.

-   To manually advance to the next iteration (where applicable), press **N** on the keyboard.

-   To exit PTLogo, press **E** on the keyboard.

## <a href="" id="switches"></a>PTLogo command line switches


These switches can be combined and are useful for debugging purposes only. These switches are not permissible for a certification test run.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th>Switch</th>
<th>Usage</th>
<th>Purpose</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>-startat #</strong></p></td>
<td><p><strong>Ptlogo.exe –startat # test</strong><em>&lt;testname&gt;</em><strong>.json</strong></p>
<p>Where <strong>#</strong> indicates the specific iteration to start for a given .json test, and <em>&lt;testname&gt;</em> is the name of the .json test.</p></td>
<td><p>Skips to a specific iteration in a given test.</p></td>
</tr>
<tr class="even">
<td><p><strong>-noDesktop</strong></p></td>
<td><p><strong>Ptlogo.exe –noDesktop test.</strong><em>&lt;testname&gt;</em><strong>.json</strong></p>
<p>Where <em>&lt;testname&gt;</em> is the name of the .json test.</p></td>
<td><p>Starts the test on the same input desktop from where it was launched; this is useful for running <strong>digiinfo</strong> or other debugging tools in the background.</p></td>
</tr>
<tr class="odd">
<td><p><strong>-NoHIDValidation</strong></p></td>
<td><p><strong>Ptlogo.exe –noHidValidation test.</strong><em>&lt;testname&gt;</em><strong>.json</strong></p>
<p>Where <em>&lt;testname&gt;</em> is the name of the .json test.</p></td>
<td><p>Disables background HID validation for a specific test.</p></td>
</tr>
<tr class="even">
<td><p><strong>-alliters</strong></p></td>
<td><p><strong>Ptlogo.exe –alliters test.</strong><em>&lt;testname&gt;</em><strong>.json</strong></p>
<p>Where <em>&lt;testname&gt;</em> is the name of the .json test.</p></td>
<td><p>Allows you to go through all iterations of a test even if more than the maximum number of permissible failed iterations have occurred.</p></td>
</tr>
<tr class="odd">
<td><p><strong>-EnableHIDLogging</strong></p></td>
<td><p><strong>Ptlogo.exe –enableHIDLogging test.</strong><em>&lt;testname&gt;</em><strong>.json</strong></p>
<p>Where <em>&lt;testname&gt;</em> is the name of the .json test.</p></td>
<td><p>Enables HID logging during a specific test to generate a HID.log file for detailed debugging of failures.</p></td>
</tr>
</tbody>
</table>

 

## <a href="" id="selectsuspend"></a>USB Selective Suspend


All validation should be performed with Selective Suspend DISABLED except for Static Validation, Edge, and Power State Transition tests.

In Device Manager, selective suspend can be toggled by selecting the HID Compliant touchpad device node and then viewing by connection to find its parent (a USB Input Device). Select the parent, view **Properties**, and then select the **Power Management** tab to see the screen that is shown in *Figure 4 USB Selective Suspend Toggle*.

When the box is checked, Selective Suspend is enabled; when the box is unchecked, Selective Suspend is disabled.

![usb selective suspend toggle](images/usbselectivesuspend.jpg)

**Figure 4 USB Selective Suspend Toggle**

## Related topics


[Additional Input Resources](additional-input-resources.md)

[Windows Precision Touchpad Certification Process](windows-precision-touchpad-certification-process.md)

[Windows Precision Touchpad HCK Requirements](windows-precision-touchpad-hck-requirements.md)

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bp_hck\p_hck%5D:%20Windows%20Precision%20Touchpad%20Device%20Validation%20Guide%20%20RELEASE:%20%284/27/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




