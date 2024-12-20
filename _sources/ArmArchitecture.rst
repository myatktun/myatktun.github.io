================
Arm Architecture
================

1. `Architecture`_
2. `References & External Resources`_

`back to top <#arm-architecture>`_

Architecture
============

* `Component Architecture Specifications`_, `System Architecture`_, `Base System Architecture`_
* `Base Boot Requirements`_, `Micro-architecture`_, `Available Specifications`_, `Documentation`_
* `Architecture Terms`_
* Arm architecture is integrated into System-on-Chip (SoC) devices
* Programmer's model: common instruction set and workflow for software developers
* interoperability across different architecture implementation ensure that software can run on
  different Arm devices
* A-profile: applications, high performance, designed for complex operating system, e.g. Linux,
  Windows
* R-profile: real-time, for real-time systems, such as networking equipment, embedded control
  systems
* M-profile: microcontroller, small and power-efficient IoT devices
* architecture defines functional specification for a processor, and describes what
  functionality the software can provide, relying on the hardware
* Instruction set: function of each instruction and encoding, instruction's memory
  representation
* Register set: count, size, function and initial states of registers
* Exception model: privilege levels, exception types, and take or return from an exception
* Memory model: memory access order, cache behaviour, and how software must perform
  explicit maintenance
* Debug, trace, and profiling: how breakpoints set and trigger, and what information in
  different formats can be captured by trace tools

Component Architecture Specifications
-------------------------------------
    * Arm architecture, ISA compatibility, first layer that provide common programmer's model
    * basis of software compatibility
    * hardware built according to specifications ensure software can be written to match it
    * software written according to specifications ensure that it can run on compliant hardware

System Architecture
-------------------
    * BSA and BBR, kernel boot compatibility, applicable to a wide range of use cases
    * xBSA, standard distribution compatibility, specific to particular market

Base System Architecture
------------------------
    * BSA describe hardware system architecture that system software can rely on
    * cover features of processor and system architecture, e.g. interrupt controller, timers,
      and other common devices that OS need
    * can be used by other standards to provide market-specific standardisation, e.g. SBSA for
      server OS

Base Boot Requirements
----------------------
    * BBR describe requirements that Arm based systems, OS and hypervisors can rely on
    * establish firmware interface requirements, such as PSCI, SMCCC, UEFI, ACPI, SMBIOS
    * also provide instructions for target specific use cases, e.g. SBBR, EBBR, LBBR

* architecture does not tell how a processor is built or works

Micro-architecture
------------------
    * explains how a particular processor works
    * include pipeline length and layout, number and sizes of caches, cycle counts for
      individual instructions, and optional features implemented
    * e.g. Cortex-A53 and Cortex-A72 both implement Armv8-A architecture, but have very
      different micro-architectures
    * architecturally compliant software can run on any processor that implement the same
      architecture

* each architecture version builds on the previous one, e.g Armv9-A or v9-A, version 9 for
  A-profile, builds on Armv8-A
* new instructions and features are added annually

Available Specifications
------------------------
    * **Generic Interrupt Controller (GIC)**
        - standardised interrupt controller for use with Armv7-A/R and Armv8-A/R
    * **SMMU/IOMMU**
        - System Memory Management Unit, provide translation services to non-processor masters
    * **Generic Timer**
        - common reference system count to all the processors in the system
        - provide functionalities used in OS scheduler tick
        - Generic Timer is part of Arm architecture, but the system counter is a system
          component
    * **SBSA & TBSA**
        - Server Base System Architecture, and Trusted Base System Architecture
        - provide system design guidelines for SoC developers
    * **AMBA**
        - Advanced Microcontroller Bus Architecture, family of bus protocols
        - control how components are connected and the protocols of the connections

Documentation
-------------
    * Arm architecture and processor manuals are available on [Arm developer website](https://developer.arm.com/)
    * Architecture Reference Manual: describe architecture specification, and any
      implementation of that architecture
    * Technical Reference Manual (TRM): describe features specific to each Arm Cortex processor
    * Configuration and Integration Manual (CIM): describe how to integrate Arm Cortex
      processor into a system, only relevant to SoC designers, and only available to IP licenses
    * reference manuals do not guide on how to use the processor, only technical detail of what
      the architecture requires
    * documents provide more general guidance, concepts, and instructions

Architecture Terms
------------------
    * **Processing Element (PE)**
        - generic term for Arm architecture implementation, foundations for the design of a
          processor or core
        - anything that has its own program counter and can execute a program is a PE
        - PE mode: states that determine how a PE operates, include current Exception level
          and security state, and in AArch32 state
        - Cortex-A8: entire processor is a PE, as it is a single core and single-threaded
        - Cortex-A53: each core is a PE, as it is a multi-core, and each core is single thread
        - Cortext-A65AE: each thread is a PE, as it is a multi-core, and each core has two
          threads
    * **Implementation Defined (IMP DEF)**
        - IMP DEF feature is defined by specific micro-architecture
        - implementation must has consistent behaviour or value
        - e.g. cache size is IMP DEF, the architecture defined mechanism for software to query
          the cache sizes, but the size is up to the processor designer
        - a processor will or will not support the features and instructions, but the presence
          of the feature cannot change at runtime
        - IMP DEF options are documented in TRM
    * **Unpredictable and Constrained Unpredictable**
        - describe things that software should not do
        - the processor may show different behaviours, if software carry out things that are
          defined unpredictable
        - unpredictable behaviours are not usually described in TRM
    * **Deprecated**
        - feature marked for removal for performance or no longer commonly used and
          unnecessary
        - warn developers to remove the feature from their code
        - control to test the deprecated feature in legacy code will be added to the
          architecture
    * **RES0 and RES1**
        - RES0: reserved, should be zero
        - RES1: reserved, should be one
        - to describe fields that are unused and have no functional effect on the processor
        - RES0 and RES1 fields will not always read 0 and 1, only tell that the field is
          unused
        - reserved field might be used in some future version of the architecture
        - stateful: the fields read back the last written value

`back to top <#arm-architecture>`_

References & External Resources
===============================

* Arm. (2024). Architecture: Learn the Architecture Guides. Available at:
  https://www.arm.com/architecture/learn-the-architecture
* Arm Developer. (2024). Learn the Architecture: Introducing the Arm architecture. Available
  at: https://developer.arm.com/documentation/102404/latest/

`back to top <#arm-architecture>`_
