# ISO/IEC 19249

The International Organization for Standardization (ISO) and the International Electrotechnical Commission (IEC) have created the ISO/IEC 19249.

### Five Architectural Principles:

1. **Domain Separation** - categorizing into different domains and applying a single set of rules unique to each domain. Example: OS kernel (ring 0 - most privileged), user-mode applications (ring 3 - least privileged).
2. **Layering** - imposing different security policies at different layers, relating to defense in depth (multiple security measures to slow down/prevent attacks). Example: OSI model
3. **Encapsulation**: hiding low-level implementations to prevent direct manipulation of data. Example: designing an API to access a database
4. **Redundancy**: preventing single point of failure situations. Example: RAID 5 having multiple drives
5. **Virtualization**: sandboxing and security boundaries with the help of cloud.

### Five Design Principles:

1. **Least Privilege**: Access granted on a need-to-know basis
2. **Attack Surface Minimisation**: Disabling unused services to reduce risk
3. **Centralized Parameter Validation**: Using a centralized set of rules to validate user input
4. **Centralized General Security Services**: Centralized servers for different security processes keeping in mind redundancy
5. **Preparing for Error and Exception Handling**: Preventing silent failures and enabling failsafes during downtimes and errors