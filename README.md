# DEXTDENT

For USB device manufacturers, the tried and true way of doing this is using the USB HID class driver - when the device connects to the host, it identifies itself over the USB channel as a device that supports generic USB HID. When the computer sees that device and those identifications, it immediately starts an instance of the generic USB HID driver and attaches it to that endpoint. The driver exposes a simple file-handle interface that allows you to read and write raw bytes to the device.

A user-land program uses the HID API to enumerate all HID devices, filtering the enumerated devices by their vendor ID and product ID field to find the particular device you want. Next, you can obtain a Windows Object Manager 'path' to the device driver's user-land interface. Call CreateFile on that path to open a file handle to the driver, and then read and write to your heart's content. What you read and write is just a stream of bytes that get sent to the device.

The relevant APIs you'll need to implement this:

HidD_GetHidGuid
SetupDiGetClassDevs
SetupDiEnumDeviceInterfaces
SetupDiGetDeviceInterfaceDetail
CreateFile
HidD_GetAttributes
HidD_GetPreparsedData
HidD_FreePreparsedData
HidP_GetCaps
HidD_GetSerialNumberString
HidD_... etc
There are several projects available on the internet that show to do this - the DS4Windows project does, since the DualShock4 bluetooth controllers attach as vanilla USB HID devices. Take a look at their source: https://github.com/Jays2Kings/DS4Windows, particularly their HidDevices.EnumerateDevices method:

    private static IEnumerable<DeviceInfo> EnumerateDevices()
    {
        var devices = new List<DeviceInfo>();
        var hidClass = HidClassGuid;
        var deviceInfoSet = NativeMethods.SetupDiGetClassDevs(ref hidClass, null, 0, NativeMethods.DIGCF_PRESENT | NativeMethods.DIGCF_DEVICEINTERFACE);

        if (deviceInfoSet.ToInt64() != NativeMethods.INVALID_HANDLE_VALUE)
        {
            var deviceInfoData = CreateDeviceInfoData();
            var deviceIndex = 0;

            while (NativeMethods.SetupDiEnumDeviceInfo(deviceInfoSet, deviceIndex, ref deviceInfoData))
            {
                deviceIndex += 1;

                var deviceInterfaceData = new NativeMethods.SP_DEVICE_INTERFACE_DATA();
                deviceInterfaceData.cbSize = Marshal.SizeOf(deviceInterfaceData);
                var deviceInterfaceIndex = 0;

                while (NativeMethods.SetupDiEnumDeviceInterfaces(deviceInfoSet, ref deviceInfoData, ref hidClass, deviceInterfaceIndex, ref deviceInterfaceData))
                {
                    deviceInterfaceIndex++;
                    var devicePath = GetDevicePath(deviceInfoSet, deviceInterfaceData);
                    var description = GetBusReportedDeviceDescription(deviceInfoSet, ref deviceInfoData) ?? 
                                      GetDeviceDescription(deviceInfoSet, ref deviceInfoData);
                    devices.Add(new DeviceInfo { Path = devicePath, Description = description });
                }
            }
            NativeMethods.SetupDiDestroyDeviceInfoList(deviceInfoSet);
        }
        return devices;
    }
