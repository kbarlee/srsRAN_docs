.. _enb_advanced:

Advanced Usage
==============

MIMO
****

The srsENB supports MIMO transmission modes 2, 3, and 4. You only need to set up the transmission mode and the number of eNb ports in the ``enb.conf`` file:

.. code::

  ...
  [enb]
  ...
  tm = 3
  nof_ports = 2
  ...
  
The eNb configures the UE for reporting the Rank Indicator for transmission modes 3 and 4. You can set the rank indicator periodic report in the file ``rr.conf`` field ``m_ri``. This value is multiples of CQI report period. For example, if the CQI period is 40ms and ``m_ri`` is 8, the rank indicator will be reported every 320ms.

5G NSA
******

5G Non-Standalone (NSA) mode adds 5G support on top of existing 4G infrastructure. This is the approach used for many commercial 5G network deployments to date, supporting higher data rates on a secondary 5G carrier while continuing to carry control traffic on the legacy 4G carrier. New 5G NSA-capable UEs can take advantage of 5G NSA services, but existing 4G devices on the network are not disrupted.

What is 5G NSA Mode?
--------------------

.. figure:: .imgs/5G_NSA_mode3.png
  :align: center
  
  5G NSA Mode 3

To activate the secondary 5G NSA carrier, the UE first connects to the 4G network. If both the UE and eNB support NSA, and a 5G secondary cell is present then a secondary bearer will be activated on that 5G cell. The NR node is deployed as a Secondary Node (SN) to the LTE Master Node (MN). The 4G anchor carrier is used for control plane signaling while the secondary 5G carrier is used for high-speed data plane traffic. This can result in improved data rates compared to a standard LTE connection.  

The following signaling procedure is used when initiating a 5G NSA link: 

.. figure:: .imgs/NSA_Signaling.png
  :align: center
  
  5G NSA Signaling Procedure. Modified from `here <https://www.sharetechnote.com/html/5G/5G_LTE_Interworking.html>`_. 

The above signaling procedure for secondary bearer activation occurs after the initial LTE connection between the UE and eNB is made. The NR connection is then made via the RRC Connection Reconfiguration process.  


Implementing 5G NSA with srsENB
-------------------------------

To enable a 5G NSA connection with srsENB you will need to modify the configuration files for srsENB. Namely, the *rr.conf* file, by enabling an NR secondary carrier for the existing eNB. This is covered in more detail in the relevant :ref:`app note<5g_nsa_zmq_appnote>` detailing how to use ZMQ to create an E2E 5G NSA network using srsRAN. 

5G NSA Features
---------------

  * LTE Release 15 aligned
  * FDD and TDD configuration
  * Tested bandwidths: 10 MHz
  * Transmission mode 1 (single antenna)
  * Frequency-based ZF and MMSE equalizer
  * Highly optimized LDPC and Polar encoders and decoders available in Intel SSE4.1/AVX2/AVX512
  * Detailed log system with per-layer log levels and hex dumps
  * MAC layer wireshark packet capture
  * Command-line trace metrics
  * Detailed input configuration files
  * Channel simulator for EPA, EVA, and ETU 3GPP channels
  * ZeroMQ-based fake RF driver for I/Q over IPC/network
  * User-plane encryption
  * All layers for NSA operation (no RRC, no SDAP, no NG)
  * PUCCH formats 1 and 2 supported
  * UE-specific and Common Search Spaces
  * DCI formats 0_0 and 1_0 supported
  * Physical Signals: NZP-CSI and Synchronization signals supported