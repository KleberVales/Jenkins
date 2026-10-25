# Compute

# 1. Compute Service

The OCI Compute Service is a foundational offering that enables you to provision and manage compute hosts called instances. It provides a range of shapes—templates defining OCPUs, memory, and other resources—to suit different compute and application needs. 

OCI offers both bare metal instances, which give you dedicated physical server access, and virtual machine instances, which run as independent environments on shared physical hardware. 

A key operational detail to remember is that any changes made to an instance's local drives are lost upon termination, while changes saved to attached volumes are retained. Understanding these fundamentals is essential for effectively using OCI Compute Service in your cloud environment.

## Understanding Shapes

OCI offers a variety of shapes designed to meet a wide range of compute and application requirements. But what exactly is a shape?

> A shape is a template that defines:
> - The number of OCPUs (Oracle CPUs)
> - The amount of memory
> - Other resources allocated to an instance

In other words, when you choose a shape, you're deciding how much compute power and memory your instance will have.

---

## Types of Compute Instances

OCI Compute Service offers two main types of instances:

### Bare Metal Instances
- Provide dedicated physical server access
- You get the entire physical machine to yourself

### Virtual Machine (VM) Instances
- An independent computing environment that runs on top of physical bare metal hardware
- Multiple VMs can share the same underlying hardware

---
