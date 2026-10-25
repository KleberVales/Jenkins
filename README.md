# Compute

# 1. Compute Service

The OCI Compute Service is a foundational offering that enables you to provision and manage compute hosts called instances. It provides a range of shapes—templates defining OCPUs, memory, and other resources—to suit different compute and application needs. 

OCI offers both bare metal instances, which give you dedicated physical server access, and virtual machine instances, which run as independent environments on shared physical hardware. 

A key operational detail to remember is that any changes made to an instance's local drives are lost upon termination, while changes saved to attached volumes are retained. Understanding these fundamentals is essential for effectively using OCI Compute Service in your cloud environment.

# 2. Shapes

OCI offers a variety of shapes designed to meet a wide range of compute and application requirements. But what exactly is a shape?

> A shape is a template that defines:
> - The number of OCPUs (Oracle CPUs)
> - The amount of memory
> - Other resources allocated to an instance

In other words, when you choose a shape, you're deciding how much compute power and memory your instance will have.

---

## Compute Shapes

 

# 3. Compute Instances

OCI Compute Service offers two main types of instances:

### Bare Metal Instances
- Provide dedicated physical server access
- You get the entire physical machine to yourself

### Virtual Machine (VM) Instances
- An independent computing environment that runs on top of physical bare metal hardware
- Multiple VMs can share the same underlying hardware

---

# 4. Data Persistence

# 5. Images

## Platform and Custom Images

When creating a compute instance, you must specify both a shape and an image—the image acts as a template for the virtual hard drive, determining the operating system and software. 

OCI offers multiple image sources: platform images (pre-built operating systems like Oracle Linux, Ubuntu, CentOS, Windows, and Oracle Autonomous Linux), Oracle images (enterprise solutions), custom images (created from existing OCI instances), partner images (third-party images from the Marketplace), and bring-your-own-image (importing your own OS versions, with a 400 GB maximum size limit). 

When creating a custom image from a running instance, the instance shuts down temporarily and restarts once complete, and any instances launched from that image will inherit the captured customizations. 

For Windows custom images, you can choose between generalized images (cleaned of instance-specific data) and specialized images (point-in-time snapshots useful for backups).

### What is an Instance Image?

An Instance Image is a template for a virtual hard drive. It contains the operating system and any pre-installed software or custom configurations needed to launch a compute instance (virtual server) in the cloud .

- How It Works
When you create a new instance in a service like Oracle Cloud Infrastructure (OCI), you must choose an image. This image acts as the "blueprint" for the instance's boot disk, determining what OS (like Oracle Linux or Windows) and software stack the new server will start with.

### Types of Images

OCI provides several sources for images. Let's look at each one:

**1. Platform Images**

Provided directly by Oracle

Oracle provides several pre-built operating systems for OCI, including:

- **Oracle Linux**
- **Ubuntu**
- **CentOS**
- **Microsoft Windows**
- **Oracle Autonomous Linux**
  

**2. Oracle Images**
- Oracle Enterprise images and solutions enabled for OCI

**3. Custom Images**
- Created from existing instances running in OCI
- Capture your own configurations and software

**4. Partner Images**
- Trusted third-party images published in the Marketplace by Oracle partners

**5. Bring Your Own Image (BYOI)**
- Using the custom image feature, you can import custom images into the OCI environment
- Allows you to bring your own versions of operating systems to the cloud

- Requirement: The underlying hardware must support it
- Limitation: The maximum image size that can be imported is 400 GB

**Windows Custom Images**

When creating a Windows custom image, there is support for two kinds of images:

| Image Type | Description | Use Case |
|------------|-------------|----------|
| Generalized | Images that have been cleaned of instance-specific information | Standard deployments |
| Specialized | Point-in-time snapshots of the boot disk of a running instance | Creating backups of an instance |

---

---




