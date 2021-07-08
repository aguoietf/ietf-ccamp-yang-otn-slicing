---
coding: utf-8

title: Framework and Data Model for OTN Network Slicing

abbrev: Framework and YANG of OTN Slices
docname: draft-zheng-ccamp-yang-otn-slicing-02
workgroup: CCAMP Working Group
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Haomian Zheng
    org: Huawei Technologies
    street: H1, Xiliu Beipo Village, Songshan Lake
    city: Dongguan
    country: China
    email: zhenghaomian@huawei.com
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Aihua Guo
    org: Futurewei Technologies
    email: aihuaguo.ietf@gmail.com
  -
    name: Luis M. Contreras
    org: Telefonica
    email: luismiguel.contrerasmurillo@telefonica.com
  -
    name: Oscar Gonzalez de Dios 
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    name: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com
  -
    name: Sergio Belotti
    org: Nokia
    email: Sergio.belotti@nokia.com
  -
    name: Dieter Beller
    org: Nokia
    email: Dieter.Beller@nokia.com
  -
    name: Reza Rokui
    org: Nokia
    email: reza.rokui@nokia.com
  -
    name: Yunbin Xu
    org: CAICT
    email: xuyunbin@caict.ca.cn
  -
    name: Yang Zhao
    org: China Mobile
    email: zhaoyangyjy@chinamobile.com

#contributor:
#  -

normative:
  TS.28.530-3GPP:
    title: 3GPP TS 28.530 V15.1.0 Technical Specification Group Services and System
           Aspects; Management and orchestration; Concepts, use cases
           and requirements (Release 15)
    author:
      org: 3rd Generation Partnership Project (3GPP)
    date: December 2018
    seriesinfo: 3GPP TS 28.530
    target: http://ftp.3gpp.org//Specs/archive/28_series/28.530/28530-f10.zip

--- abstract

   The requirement of slicing network resource with desired quality of
   service is emerging at every network technology, including the
   Optical Transport Networks (OTN). As a part of the transport network,
   OTN can provide hard pipes with guaranteed data isolation and
   deterministic low latency, which are highly demanded in the Service
   Level Agreement (SLA).

   This document describes a framework for OTN network slicing and a
   YANG data model augmentation of the OTN topology model. Additional
   YANG data model augmentations will be defined in a future version of
   this draft.

--- middle

# Introduction

   The requirement of slicing network resource with desired quality of
   service is emerging at every network technology, including the
   Optical Transport Networks (OTN). As a part of the transport network,
   OTN can provide hard pipes with guaranteed data isolation and
   deterministic low latency, which are highly demanded in the Service
   Level Agreement (SLA).
   This document describes a framework for OTN network slicing and a
   YANG data model augmentation of the OTN topology model. Additional
   YANG data model augmentations will be defined in a future version of
   this draft.

## Definition of OTN Slice
   An OTN slice is an OTN virtual network topology connecting a number
   of OTN endpoints using a set of shared or dedicated OTN network resources to
   satisfy specific service level objectives (SLOs).

   An OTN slice is a technology-specific realization of an IETF network slice 
   {{?I-D.ietf-teas-ietf-network-slices}} in the OTN domain, with the
   capability of configuring slice resources in the term of OTN technologies. 
   Therefore, all the terms and definitions with respect to network slicing as 
   defined in {{?I-D.ietf-teas-ietf-network-slices}} apply to OTN slicing.
   
   An OTN slice can span multiple OTN administrative domains, encompassing 
   access links, intra-domain paths, and inter-domain links. 
   An OTN slice may include multiple endpoints, each associated with a set of physical
   or logical resources, e.g. optical port or time slots, at the termination point (TP) of 
   an access link or inter-domain link at an OTN provider edge (PE) equipment. 

   An end-to-end OTN slice may be composed from multiple OTN segment slices in
   a hierarchical or sequential (or stitched) combination. 

   {{fig-otn-slice}} illustrates the scope of OTN slices in multi-domain 
   environment.

~~~~
        <-------------------End-to-end OTN Slice----------------->

        <-- OTN Segment Slice 1 --->  <-- OTN Segment Slice 2 --->


          +-------------------------+  +-----------------------+
          | +-----+      +-------+  |  | +-------+      +-----+|
   +----+ | | OTN |      | OTN   |  |  | | OTN   |      | OTN ||  +----+
   | CE +-+-o PE  +-...--+ Borde o--+--+-o Borde +-...--+ PE  o+--+ CE |
   +----+ |/|     |      | Node  |\ |  | | Node  |      |     ||  +----+
        | ||+-----+      +-------+ ||| | +-------+      +-----+| |
        | ||    OTN Domain 1       ||| |      OTN Domain 2     | |
        | ++-----------------------++| +-----------------------+ |
        |  |                       | |                           |
        |  +-----+    +------------+ |                           |
        |        |    |              |                           |
        V        V    V              V                           V
     Access     OTN Slice        Inter-domain                  Access
     Link       Endpoint         Link                          Link

~~~~
{: #fig-otn-slice title="OTN Slice"}

   OTN slices may be pre-configured by the management plane and presented to 
   the customer via the northbound interface (NBI), or they may be dynamically 
   provisioned by a higher layer slice controller, e.g. 
   an IETF network slice controller (IETF NSC) through the NBI. The OTN slice is 
   provided by a service provider to a customer to be used as though it was part
   of the customer's own networks.
         
# Use Cases for OTN Network Slicing

## Leased Line Services with OTN

   For end business customers (like OTT or enterprises), leased lines
   have the advantage of providing high-speed connections with low
   costs. On the other hand, the traffic control of leased lines is very
   challenging due to rapid changes in service demands. Carriers are
   recommended to provide network-level slicing capabilities to meet
   this demand. Based on such capabilities, private network users have
   full control over the sliced resources which have allocated to them
   and which could be used to support their leased lines, when needed.
   Users may formulate policies based on the demand for services and
   time to schedule the resources from the entire network's perspective
   flexibly. For example, the bandwidth between any two points may be
   established or released based on the time or monitored traffic
   characteristics. The routing and bandwidth may be adjusted at a
   specific time interval to maximize network resource utilization
   efficiency.

## Co-construction and Sharing

   Co-construction and sharing of a network are becoming a popular means
   among service providers to reduce networking building CAPEX. For Co-
   construction and sharing case, there are typically multiple co-
   founders for the same network. For example, one founder may provide
   optical fibres and another founder may provide OTN equipment, while
   each occupies a certain percentage of the usage rights of the network
   resources. In this scenario, the network O&M is performed by a
   certain founder in each region, where the same founder usually
   deploys an independent management and control system. The other
   founders of the network use each other's management and control
   system to provision services remotely. In this scenario, different
   founders' network resources need to be automatically (associated)
   divided, isolated, and visualized. All founders may share or have
   independent O&M capabilities, and should be able to perform service-
   level provisioning in their respective slices.

## Wholesale of optical resources

   In the optical resource wholesale market, smaller, local carriers and
   wireless carriers may rent resources from larger carriers, or
   infrastructure carriers instead of building their networks. Likewise,
   international carriers may rent resources from respective local
   carriers and local carriers may lease their owned networks to each
   other to achieve better network utilization efficiency.
   From the perspective of a resource provider, it is crucial that a
   network slice is timely configured to meet traffic matrix
   requirements requested by its tenants. The support for multi-tenancy
   within the resource provider's network demands that the network
   slices are qualitatively isolated from each other to meet the
   requirements for transparency, non-interference, and security.
   Typically, a resource purchaser expects to use the leased network
   resources flexibly, just like they are self-constructed. Therefore,
   the purchaser is not only provided with a network slice, but also the
   full set of functionalities for operating and maintaining the network
   slice.  The purchaser also expects to, in a flexible and independent
   manner, schedule and maintain physical resources to support their own
   end-to-end automation using both leased and self-constructed network
   resources.

## Vertical dedicated network with OTN

   Vertical industry slicing is an emerging category of network slicing
   due to the high demand for private high-speed network interconnects
   for industrial applications.
   In this scenario, the biggest challenge is to implement
   differentiated optical network slices based on the requirements from
   different industries. For example, in the financial industry, to
   support high-frequency transactions, the slice must ensure to provide
   the minimum latency along with the mechanism for latency management.
   For the healthcare industry, online diagnosis network and software
   capabilities to ensure the delivery of HD video without frame loss.
   For bulk data migration in data centers, the network needs to support
   on-demand, large-bandwidth allocation. In each of the aforementioned
   vertical industry scenarios, the bandwidth shall be adjusted as
   required to ensure flexible and efficient network resource usage.

## End-to-end network slicing

   In an end-to-end network slicing scenario such as 5G network slicing
   {{TS.28.530-3GPP}}, an IETF network slice {{?I-D.ietf-teas-ietf-network-slices}}
   provides the required connectivity between other different segments 
   of an end-to-end network slice, such as the Radio Access Network 
   (RAN) and the Core Network (CN) segments, with a specific 
   performance commitment. An IETF network slice could be composed of 
   network slices from multiple technological and administrative 
   domains. An IETF network slice can be realized by using or combining
   multiple underlying OTN slices with OTN resources, e.g. ODU time 
   slots or ODU containers,to achieve end-to-end slicing across the transport
   domain.

# Framework for OTN slicing

   OTN slices may be abstracted differently depending on the requirement contained
   in the configuration provided by the slice customer. Whereas the customer requests
   an OTN slice to provide connectivities between specified endpoints, an OTN slice 
   can be abstracted as a set of endpoint-to-endpoint links, with each link formed 
   by an end-to-end tunnel across the underlying OTN networks. The resources
   associated with each link of the slice is reserved and commissioned in the underlying
   physical network upon the completion of configuring the OTN slice, and all the 
   links are active. 
   
   An OTN slice can also be abstracted as an abstract topology when the customer reqeuests
   the slice to share resources between multiple endpoints and to use the resources on demand.
   The abstract topology may consist of virtual nodes and virtual links, and their associated
   resources are reserved but not comissioned across the underlying OTN networks. The 
   customer can later commission resources within the slice dynamically using the NBI provided
   by the service provider. Those means to abstract an OTN slice is similar to the Virtual 
   Network (VN) concept defined for higher-level interfaces in {{!I-D.ietf-teas-actn-vn-yang}}, where
   the connectivity-based slice abstraction corresponds to VN Type 1 and the resource-based slice
   abstraction corresponds to VN type 2.

   A particular resource in an OTN network, such as a port or link, may be
   sliced with one of the two granularity levels:

   -  Link-based slicing, where a link and its associated link
      termination points (LTPs) are dedicatedly allocated to a
      particular OTN slice.

   -  Tributary-slot based slicing, where multiple OTN slices
      share the same link by allocating different OTN tributary slots in
      different granularities.

   Furthemore, an OTN switch is typically fully non-blockable switching 
   at the lowest ODU container granularity, it is
desirable to specify just the total number of tributary slots in the
lowest granularity (e.g. ODU0), when configuring tributary-slot based
slicing on links and ports internal to an OTN network. In multi-domain
OTN network scenarios where separate OTN slices are created on
each of the OTN networks and are stitched at inter-domain OTN links, it
is necessary to specify matching tributary slots at the endpoints of the
inter-domain links. In some real network scenarios, OTN network resources
including tributary slots are managed explicitly by network operators for
network maintenance considerations. Therefore an OTN slice controller
shall support configuring an OTN slice with both options.

   An OTN slice controller (OTN-SC) is a logical function responsible for
   the life-cycle management of OTN slices instantiated within the 
   corresponding OTN network domains. The OTN-SC provides technology-specific 
   interfaces at its north bound (OTN-SC NBI) to allow a higher-layer slice 
   controller, such as an IETF network slice controller (NSC), or an orchestrator, 
   to configure OTN slices with OTN-specific 
   network resources. The OTN-SC interfaces at the south bound using the MDSC-to-PNC 
   interface (MPI) with a Physical Network Controller (PNC) or Multi-Domain Service Orchestrator (MDSC),
   as defined in the ACTN control framework {{?RFC8453}}. The logical function 
   within the OTN-SC is responsible for translating OTN slice configurations 
   received into concrete slice realization which can be understood and 
   provisioned at the south bound by the PNC or MDSC.
   
   When realizing OTN slices, the OTN-SC may translate a connectivity-based OTN slice 
   into a set of end-to-end tunnels using the Traffic-engineering(TE) tunnel interface defined in 
   {{!I-D.ietf-teas-yang-te}}. For a resource-based OTN slices, the 
   OTN-SC may transate the abstract topology representing the slice into a colored graph on an 
   abstract TE topology using the TE topology interface defined in {{!RFC8795}}.
   
   The OTN-SC NBI is technology-specific, while the IETF NSC-NBI is technology-
   agnostic. An IETF NSC may translate its customer's technology-agnostic slice
   request into an OTN slice request and utilize the OTN-SC NBI to realize 
   the IETF network slice. Alternatively, the IETF NSC may translate the slicing
   request into tunnel or topology configuration commands and communicate directly
   with the underlying PNC or MDSC to provision the IETF network slice.

   {{fig-slice-interfaces}} illustrates the OTN slicing control hierarchy 
   and the positioning of the OTN slicing interfaces.


~~~~
                      +--------------------+
                      | Provider's User    |
                      +--------|-----------+
                               | CMI
       +-----------------------+----------------------------+
       |          Orchestrator / E2E Slice Controller       | 
       +------------+-----------------------------+---------+
                    |                             | NSC-NBI
                    |       +---------------------+---------+
                    |       | IETF Network Slice Controller |
                    |       +-----+---------------+---------+
                    |             |               |
                    | OTN-SC NBI  |OTN-SC NBI     |               
       +------------+-------------+--------+      |
       |               OTN-SC              |      |
       +--------------------------+--------+      |      
                                  | MPI           | MPI                           
       +--------------------------+---------------+---------+ 
       |                         PNC                        | 
       +--------------------------+-------------------------+ 
                                  | SBI
                      +-----------+----------+
                      |OTN Physical Network  |
                      +----------------------+

~~~~
{: #fig-slice-interfaces title="Positioning of OTN Slicing Interfaces"}

   OTN-SC functionalities may be recursive such that a higher-level
   OTN-SC may designate the creation of OTN slices to a lower-level
   OTN-SC in a recursive manner. This scenario may apply to the
   creation of OTN slices in multi-domain OTN networks, where 
   multiple domain-wide OTN slices provisioned by lower-layer
   OTN-SCs are stitched to support a multi-domain OTN slice
   provisioned by the higer-level OTN-SC.  Alternatively, the OTN-SC
   may interface with an MDSC, which in turn interfaces with multiple 
   PNCs through the MPI to realize OTN slices in multi-domain without OTN-SC recursion. 
   {{fig-otn-sc-recursion}} illustrates both options for OTN slicing
   in multi-domain.

~~~~
    +-------------------+                    +-------------------+
    |      OTN-SC       |                    |      OTN-SC       |
    +--------|----------+                    +---|----------|----+
             |MPI                                |OTN-SC NBI|
    +--------|----------+                    +---|----+ +---|----+
    |      MDSC         |                    | OTN-SC | | OTN-SC |
    +---|----------|----+                    +---|----+ +---|----+
        |MPI       |MPI                          |MPI       |MPI
    +---|----+ +---|----+                    +---|----+ +---|----+
    |   PNC  | |   PNC  |                    |   PNC  | |   PNC  |
    +--------+ +--------+                    +--------+ +--------+
    Multi-domain Option 1                    Multi-domain Option 2
~~~~
{: #fig-otn-sc-recursion title="OTN-SC for multi-domain"}

   OTN-SC functionalities are logically independent and may be deployed in 
   different means to cater to the realization needs. In reference with the 
   ACTN control framework {{?RFC8453}}, an OTN-SC may be deployed
    - as an independent network function;
    - together with a Physical Network Controller (PNC) for single domain
      or with a Multi-Domain Service Orchestrator (MDSC)for multi domain;
    - together with a higher-level network slice controller to support 
      end-to-end network slicing;

# YANG Data Model for OTN Slicing Configuration

## OTN Slicing YANG Model for MPI

### MPI YANG Model Overview

   When realizing OTN slices, the OTN-SC may translate a connectivity-based OTN slice 
   into a set of end-to-end tunnels using the Traffic-engineering(TE) tunnel interface defined in 
   {{!I-D.ietf-teas-yang-te}}. For a resource-based OTN slices, the 
   OTN-SC may transate the abstract topology representing the slice into a colored graph on an 
   abstract TE topology using the TE topology interface defined in {{!RFC8795}}.
   
   For the configuration of connectivity-based OTN slices, existing models such as 
   the TE tunnel interface {{!I-D.ietf-teas-yang-te}} may be used and no addition is 
   needed. This model is addressing the case for configuring resource-based OTN slices,
   where the model permits to reserve resources exploiting on the common knowledge of an underlying 
   virtual topology between the OTN-SC and the subtended network controller (MDSC or PNC). The slice
   is configured by by marking corresponding link resources on the TE topology received from the 
   underlying MDSC or PNC with a slice identifier and OTN-specific resource requirements, 
   e.g. the number of ODU time slots or type/number of ODU container. The MDSC or PNC, based on the 
   marked resources by OTN-SC, will update the underlying TE topology with new TE link for each of 
   the colored links to keep booked the reserved OTN resources e.g. time slots or ODU containers.

### MPI YANG Model Tree

~~~~
{::include ./ietf-otn-slice.tree}
~~~~
{: #fig-otn-slice-tree title="OTN network slicing tree diagram"}

### MPI YANG Code

~~~~
   <CODE BEGINS>file "ietf-otn-slice@2021-02-22.yang"
{::include ./ietf-otn-slice.yang}
   <CODE ENDS>
~~~~
{: #fig-otn-slice-yang title="OTN network slicing YANG model"}

## OTN Slicing YANG Model for OTN-SC NBI

   TBD.

# Manageability Considerations

   To ensure the security and controllability of physical resource
   isolation, slice-based independent operation and management are
   required to achieve management isolation.
   Each optical slice typically requires dedicated accounts,
   permissions, and resources for independent access and O&M. This
   mechanism is to guarantee the information isolation among slice
   tenants and to avoid resource conflicts. The access to slice
   management functions will only be permitted after successful security
   checks.

# Security Considerations

   \<Add any security considerations>

# IANA Considerations

   \<Add any IANA considerations>

--- back

{: numbered="false"}

# Acknowledgments

   This document was prepared using kramdown.

   Previous versions of this document was prepared using 2-Word-v2.0.template.dot.

{: numbered="false"}

# Contributors' Addresses

    Henry Yu
    Huawei Technologies Canada
    
    Email: henry.yu@huawei.com