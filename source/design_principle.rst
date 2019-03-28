.. image:: images/EdenChain_Introduction/Design_Principle.png
    :width: 750px


.. rubric:: Design Principle
    :name: EdenChainIntroduction-DesignPrinciple

The design principle is important especially when it comes
to platform software because the principle resolves conflict
among many software modules as well as giving guideline to
keep architectual consistency.

The technical problems edenchain wants to deal with are 3
items.

#. To provide scalable blockchain platform where required
    performance will be delivered with ease
#. To guarantee non-stop blockchain platform to its users 
#. To accelerate blockchain business service 

So the selected design principle is supposed to meet those
requirements. Scalability, High-Availability and
Accessibility are 3 design principles adopted in Edenchain
architecture.

    .. rubric:: Scalability
        :name: EdenChainIntroduction-Scalability

    As we know, scalability means that increasing or decreasing
    capacity of processing power to handle received tasks.
    Scalability is the top priority demanded by most of
    blockchain services since existing blockchain platforms are
    not suitable to process real world workload. Basically there
    are 2 options to smooth scalability, scale up and scale out.
    Edenchain achieves the scalability through scale out.

    The basic idea for the scale out is doing parallel
    processing in distributed manner. Handling massive workload
    simultaneously needs to have multiple computing resources
    which can digest its given workload in distributed
    environment. So scalability design principle forces to have
    a design which allows workload distribution by algorithm,
    data structure and so on.

    .. rubric:: Accessibility
        :name: EdenChainIntroduction-Accessibility

    Accessibility takes important position when it comes to
    blockchain service implementation. What if the technology
    which solves blockchain's major tech problems is not easy to
    learn, and hard to find right developers. 

    The blockchain platform should be easy to use and learn, not
    hard to find developers for fast pervasiveness even though
    architecture should sacrificing flexibility and
    functionality. 

    In Edenchain, accessibility is the second important design
    principle to fulfill its vision, permissioned blockchain
    platform for enterprises.

    .. rubric:: High Availability
        :name: EdenChainIntroduction-HighAvailability

    Since Eden is a permissioned blockchain, a consideration of
    service availability is necessary. Given that an Eden server
    is operated by a small number of authorized agencies or
    companies, the server operation can be terminated when many
    hackers attack the servers or when there is a natural
    disaster such as an earthquake. Eden must be able to
    guarantee high availability in order to ensure that the
    services for users and businesses alike can continue to
    operate at all times regardless of any external threat.

    Eden utilizes cloud services to ensure a high degree of
    availability and operates an Eden system with a
    multi-datacenter pattern using a global DNS and a load
    balancer. The same system that provides the Eden service is
    configured and operated in each service zone across major
    continents such as Asia, North America, and Europe, and it
    can provide a stable service despite attacks from hackers
    and or the occurrence of natural disasters.

    A network between service zones deployed on each of the
    continents is composed of a Virtual Private Network (VPN).
    Cloud services provide connectivity between data centers
    across continents with high-speed dedicated lines, enabling
    fast networking and a data center-to-data center
    configuration. A multi-datacenter pattern is a pattern
    provided by the cloud service provider Amazon. It is used by
    a number of Internet companies such as the Apache
    Foundation, Netflix, CloudFoundry, and Attlasian, and is
    also recommended by Microsoft Azure.

    | 

    | 

    *The above image shows a configuration of an operating
    environment of Eden to which a multi data center pattern and
    a VPN are applied. The operating environment receives a data
    request from outside a global DNS, plays the role of being
    connected to an appropriate service zone, and secures
    availability by operating multiple global DNS servers.
    Endpoints of all services are designed and operated so as to
    be the global DNS. A load balancer delivers requests
    forwarded from the global DNS to Eden servers in order to be
    processed. The load balancer not only requests routing but
    also collects status information from each of the servers.
    This helps perform a more intelligent service operation than
    a round-robin service operation, which in turn allows the
    system to pinpoint servers that encounter a problem and to
    monitor the workload on each server, thereby aiding in
    capacity planning.*

    Servers running Eden are protected by an operational
    firewall. The operational firewall is a way to organize the
    Eden servers into functional groups and to apply a firewall
    policy to each of the organized functional groups. The
    operational firewall can functionally apply a
    well-abstracted security policy to a server so that a
    security policy can be flexibly designed, applied to each of
    the groups, and managed internally.  This allows the Eden
    architecture to minimize any form of potential mistake in
    setting work by users.

    If a VPN in full mesh topology is built between service
    zones, performance and management problems will arise
    because each VPN configuration becomes more complicated as
    the range of the service zone increases. The Eden operating
    system can configure a VPN in a star topology so that a VPN
    router in a service zone can be connected with a VPN gateway
    without connecting to all of the service zones and enable
    VPN networking with the other service zones.
