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
   divided, isolated, and visualized. In addition, all founders have
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

   An end-to-end network slice, such as a 5G network slice {{TS.28.530-3GPP}} may
span multiple technological and administrative domains. According to
{{?I-D.ietf-teas-ietf-network-slice-definition}}, IETF network slice provides
the required connectivity between different entities of an end-to-end
network slice, with a specific commitment. An IETF network slice may be
composed of network slices from underlying network domains that include
OTN, in a combination of hierarchical (recursive) or sequential
(stitched) manner. In this case, an OTN network slice is essentially a
realization of an IETF network slice in OTN network domains.

# Framework for OTN slicing

   An OTN slice is a collection of OTN network resources that are used
   to establish a logically dedicated OTN virtual network over one or
   more OTN networks. For example, the bandwidth of an OTN slice is
   described in terms of a total number or a specific set of OTN
tributary slots ; the labels
   may be specified as OTN tributary slots and/or tributary ports to
   allow slice users to interconnect devices with matching
   specifications.

   The relationship between an OTN slice and an IETF network slice
   {{?I-D.ietf-teas-ietf-network-slice-definition}} is for further discussions.

   To support the configuration of OTN slices, an OTN slice controller
   (OTN-SC) can be deployed either outside or within the SDN controller.
   In the former case, the OTN-SC translates an OTN slice configuration
   request into a TE topology configuration or a set of TE tunnel
   configurations, and instantiate it by using the TE topology {{!RFC8795}}
   or TE tunnel {{!I-D.ietf-teas-yang-te}} interfaces at the MPI (MDSC-to-
   PNC Interface), as defined in the ACTN framework {{?RFC8453}}.
   In the latter case, an Orchestrator or an end-to-end slice controller
   may request OTN slices directly through the OTN slicing interface
   provided by the OTN-SC. A higher-level OTN-SC may also designate the
   creation of OTN slices to a lower-level OTN-SC in a recursive manner.
   {{fig-slice-interfaces}} illustrates the OTN slicing control hierarchy and the
   positioning of the OTN slicing interfaces.

~~~~
                      +--------------------+
                      | Provider's User    |
                      +--------|-----------+
                               |CMI
       +-----------------------+----------------------------+
       |          Orchestrator / E2E Slice Controller       |
       +------------+---------------------------+-----------+
                    |                           |
                    |                           |
                    |                           | NSC-NBI
                    |                    +------+-----------+
                    |                    | IETF Network     |
                    |    +---------------+ Slice Controller |
                    |    |               +---------+--------+
         OTN-SC NBI |    |OTN-SC NBI               |
                    |    |                         |
       +------------+----+----+   OTN-SC NBI       |OTN-SC NBI
       |       OTN-SC         +---------------+    |
       +------------+---------+               |    |
                    |MPI                      |    |
       +------------|-------------------------|----|--------+
       | SDN        |                 +-------+----+-------+|
       | Controller |                 |      OTN-SC        ||
       |            |                 +-------+------------+|
       |            |                         |Internal API |
       |+-----------+-------------------------+------------+|
       ||                PNC/MDSC-L                        ||
       |+-----------------------+--------------------------+|
       +------------------------|---------------------------+
                                |SBI
                    +-----------+----------+
                    |OTN Physical Network  |
                    +----------------------+

~~~~
{: #fig-slice-interfaces title="Positioning of OTN Slicing Interfaces"}

   A particular OTN network resource, such as a port or link, may be
   sliced in two modes:

   -  Link-based slicing, where a link and its associated link
      termination points (LTPs) are dedicatedly allocated to a
      particular OTN network slice.

   -  Tributary-slot based slicing, where multiple OTN network slices
      share the same link by allocating different OTN tributary slots in
      different granularities.

   Additionally, since an OTN switch is typically designed to be fully
non-blocking switchable at the lowest ODU container granularity, , it is
desirable to specify just the total number of tributary slots in the
lowest granularity (e.g. ODU0) when configuring tributary-slot based
slicing on links and ports internal to an OTN network. In multi-domain
OTN network scenarios where separate OTN network slices are created on
each of the OTN networks and are stitched at inter-domain OTN links, it
is necessary to specify matching tributary slots at the endpoints of the
inter-domain links. In some real network scenarios, OTN network resources
including tributary slots are managed explicitly by network operators for
network maintenance considerations. Therefore an OTN slice controller
shall support configuring an OTN slice with both options.

# YANG Data Model for OTN Slicing Configuration

## OTN Slicing YANG Model for MPI

### MPI YANG Model Overview

   An SDN controller (PNC or MDSC) exposes to the OTN-SC set of
   available resources for OTN slicing in the form of an abstract TE
   topology. When the OTN-SC receives slice configuration from the NBI,
   it translates the configuration into a colored graph on the
   abstract TE topology, by marking corresponding link resources on the
   TE topology received from the SDN controller with a slice identifier
   and OTN-specific resource requirements, e.g. the number of ODU time
   slots. When the SDN controller receives the slice configuration, it
   shall create a new virtual TE link for each of the colored links to
   hold the reserved OTN time slots for time slot-based slicing. These
   resultant virtual links are then included in the TE topology
   advertisement to the OTN-SC.

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
