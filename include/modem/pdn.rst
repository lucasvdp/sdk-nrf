.. _pdn_readme:

PDN library
###########

.. contents::
   :local:
   :depth: 2

The PDN library can be used to manage Packet Data Protocol (PDP) contexts and PDN connections.

It provides an API for creating and configuring PDP contexts, receiving events pertaining to their state and connectivity, and manage PDN connections.

The library uses several AT commands and it relies on two AT types of notifications to work, which must be subscribed to by the application for the library to work correctly:
* Packet Domain Events notifications (CGEV), subscribed via ``AT+CGEREP=1``, sa. https://infocenter.nordicsemi.com/topic/ref_at_commands/REF/at_commands/packet_domain/cgerep.html
* notifications for unsolicited reporting of error codes sent by the network (CNEC), subscribed via ``AT+CNEC=16``, sa. https://infocenter.nordicsemi.com/topic/ref_at_commands/REF/at_commands/mobile_termination_errors/cnec.html

The following AT commands are used by the library:
* AT+CGDCONT, for context creation, configuration and destruction
* AT+CGACT, for PDN activation and decactivation
* AT%XGETPDNID, to retrieve the PDN ID for a given PDP context
* AT+CGAUTH, to set a PDN connection authentication parameters.

PDP contexts can be created via :c:func:`pdn_cid_create`, and a callback can be assigned to receive events pertaining to its state and connectivity.
PDP Context can be configured with a family, access point name, and optionally authentication parameters via :c:func:`pdn_cid_configure`.
A PDN connection for a PDP context can be activated using :c:func:`pdn_activate` and are identified by their ID, as reported via AT%XGETPDNID which is distinct from the PDP context ID (CID).
The modem creates a PDN connection for a PDP context as necessary; multiple PDP contexts may share the same PDN connection if they are configured similarly.

More information can be found on the documentation page for Packet Domain AT commands: https://infocenter.nordicsemi.com/index.jsp?topic=%2Fref_at_commands%2FREF%2Fat_commands%2Fpacket_domain%2Fpacket_domain.html

Configuration
*************

The library can override the default PDP context configuration via :option:`CONFIG_PDN_DEFAULTS_OVERRIDE`, which include:
* access point name
* family
* authentication method
* authentication credentials

Limitations
***********

The callback for the default PDP context must be set before the device is put to function mode 1 (CFUN=1) to receive the first activation event.

API documentation
*****************

| Header file: :file:`include/modem/pdn.h`
| Source file: :file:`lib/pdn/pdn.c`

.. doxygengroup:: pdn
   :project: nrf
   :members:
