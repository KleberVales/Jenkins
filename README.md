# Compute ‐ advanced

Burstable instances are a cost-effective virtual machine option for workloads with low average CPU usage and occasional spikes. They provide a guaranteed baseline of CPU performance—either 12.5% or 50% of each core—and allow bursting above that baseline when the host has available resources. 

However, bursting is not guaranteed and depends on the instance's average CPU usage over the past 24 hours. Memory does not burst, only CPU. Supported shapes include VM.Standard3.Flex, VM.Standard.E3.Flex, and VM.Standard.F4.Flex. 

Creating a burstable instance involves selecting a supported shape, specifying the OCPU count, enabling the burstable option, and setting the baseline. Pricing is based solely on the baseline OCPU, making burstable instances an economical choice for microservices, dev/test environments, CI/CD tools, static websites, and monitoring systems.
