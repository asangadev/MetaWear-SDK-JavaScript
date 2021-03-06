.. highlight:: javascript

Settings
========
The Settings module configures Bluetooth LE parameters and reports diagnostic information about the board.  

Functions communicating with this module are defined in the `settings.h <https://mbientlab.com/docs/metawear/cpp/latest/settings_8h.html>`_ header file.

Battery State
-------------
The battery data provided by the Settings module reports both charge percentange and voltage, encapsulated by the 
`MblMwBatteryState <https://mbientlab.com/docs/metawear/cpp/latest/structMblMwBatteryState.html>`_ struct.  Unlike the battery gatt servive, this 
battery data can be utilized with MetaWear features such as the logger. ::

    var battSignal = MetaWear.mbl_mw_settings_get_battery_state_data_signal(device.board, );
    
    MetaWear.mbl_mw_datasignal_read(battSignal);

Handling Disconnects
--------------------
The disconnect event is a type of `MblMwEvent <https://mbientlab.com/docs/metawear/cpp/latest/event__fwd_8h.html#a569b89edd88766619bb41a2471743695>`_ 
that is fired when the Bluetooth connection is terminated. ::

    var disEvent = MetaWear.mbl_mw_settings_get_disconnect_event(device.board);

Transmit Power
--------------------
The default transmission power is 0db on the MetaWear. This is the Bluetooth radio signal strength when transmitting and can be changed with `mbl_mw_settings_set_tx_power <https://mbientlab.com/documents/metawear/cpp/latest/settings_8h.html#a335f712d5fc0587eff9671b8b105d3ed>`_.

Valid values are: 4, 0, -4, -8, -12, -16, -20, -30:  ::

    MetaWear.mbl_mw_settings_set_tx_power(device.board, -10);

Advertising Parameters
----------------------
Advertising parameters control how the Bluetooth radio sends its advertising data.  You can modify the device name, timeout, tx power, and scan 
response.  If you have set an timeout, you can manually begin another advertisement cycle by calling 
`mbl_mw_settings_start_advertising <https://mbientlab.com/docs/metawear/cpp/latest/settings_8h.html#aad3d9f431b6e2178dbb5a409ce14cbce>`_. ::

    MetaWear.mbl_mw_settings_set_ad_interval(device.board, 417, 180);
    MetaWear.mbl_mw_settings_start_advertising(device.board);
    
Device Name
-----------------
The advertised name of the device (default = MetaWear) can be changed with `mbl_mw_settings_set_device_name <https://mbientlab.com/documents/metawear/cpp/latest/settings_8h.html#a7b2e5239dfb56137b86cfaddb5d10333>`_.  ::

    let name = "METACOOL"
    MetaWear.mbl_mw_settings_set_device_name(device.board, name, name.length);

Connection Parameters
---------------------
Connection parameters control how BTLE devices communicate with each other.  Modifying the connection parameters are all handled at the same time by 
calling 
`mbl_mw_settings_set_connection_parameters <https://mbientlab.com/docs/metawear/cpp/latest/settings_8h.html#a1cf3cae052fe7981c26124340a41d66d>`_.  

 - Connection interval = how often devices talk - min is 7.5ms, it increases in steps of 1.25ms - recommend setting min and max to same @ 7.5ms for performance.
 - Slave latency = metawear can choose not to answer when central asks for an update (i.e metawear can sleep longer - doesn't affect transfer speeds).
 - Connection supervision timeout = determines timeout from last data exchange (tells central how long to wait to attempt to reconnect to a lost conn - if your metawear goes in and out of range often, it is better to have a short timeout)

Changing connection parameters is not guaranteed to work; Apple only accepts 15ms for example and will often default to 30ms.
	
A more detailed explanation of about BTLE connection parameters can be found on this 
`post <https://devzone.nordicsemi.com/question/60/what-is-connection-parameters/>`_ from the Nordic Developer Zone forums. ::

    MetaWear.mbl_mw_settings_set_connection_parameters(board, 10.0, 1024.0, 0, 6000);

