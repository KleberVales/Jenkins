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

Compute shapes are available with three processor types:

- AMD processors
- Intel processors
- ARM-based processors

---

## Compute Shapes

A shape is a template that defines the number of OCPUs, amount of memory, and other resources allocated to an instance, and shapes are available with AMD, Intel, and ARM-based processors. 

There are two categories of shapes: fixed shapes, where you cannot customize OCPUs or memory and can create either bare metal or virtual machine instances; and flexible shapes, where you can customize OCPUs and memory but can only create virtual machine instances. 

Flexible shapes allow you to adjust resources at launch or when resizing, with network bandwidth and vNIC count scaling proportionately with OCPUs. 

## Two Options for Shapes

OCI offers two categories of shapes, each with different capabilities.

### 1. Fixed Shapes

- You cannot customize the number of OCPUs or the amount of memory
- You can create either a bare metal instance or a virtual machine instance using fixed shapes

### 2. Flexible Shapes

- You can customize the number of OCPUs and the amount of memory
- You can only create a virtual machine instance using flexible shapes
- The maximum number of OCPUs and maximum amount of memory depend on the processor type

Flexible shapes give you control over the resources allocated to your VM at launch or when resizing.

**Key Characteristics**

- Lets you customize the number of OCPUs and amount of memory when launching or resizing your VM
- Network bandwidth and the number of vNICs scale proportionately with the number of OCPUs

**Naming Convention**

Flexible shapes follow a naming pattern, for example:

- `VM.Standard.E3.Flex`
- `VM.Standard3.Flex`
---

## GPU Shapes

GPU shapes are designed for hardware-accelerated workloads—where applications offload certain tasks to specialized hardware to accelerate performance. 

### What are GPU Shapes?

Oracle Cloud Infrastructure offers a variety of shapes designed to meet a range of compute and application requirements. One such shape is the GPU shape.

> GPU shapes are designed for hardware-accelerated workloads.

**What is Hardware Acceleration?**

Hardware acceleration is a process where applications offload certain tasks to hardware in your system.

Purpose: To accelerate that task, achieving better performance than software alone could provide.

---

### NVIDIA GPU Support

OCI offers the highest value and performance for bare metal and virtual machine compute instances that are powered by NVIDIA GPUs.

**GPU Models Available**

Some of the NVIDIA GPU models offered include:

- A100
- A10
- V100
- P100

---

## Bare Metal vs Virtual Machine vs Dedicated Hosts
 

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

## Import Export and BYOI

Image import/export allows you to share custom images across tenancies and regions, using OCI Object Storage as the mechanism for replication. 

When importing images, you can choose from three launch modes: emulation mode (for VMs without paravirtualized drivers created outside OCI), paravirtualized mode (for VMs with paravirtualized drivers created outside OCI), and native mode (for images exported from OCI itself). 

To replicate an image across regions, you export it to a bucket in the source region, copy it to a bucket in the destination region, and then import it using the object's URL path. 

The Bring Your Own Image feature lets you export VMs from your existing virtualization environment and import them directly into OCI as custom images in either qcow2 or VMDK format, after which you can launch instances from them. 

BYOI supports cloud migration projects, works with both old and new operating systems, encourages experimentation, and increases infrastructure flexibility—though you must always comply with licensing requirements.

### Image Import/Export

You can **share custom images across tenancies and regions** using image import/export. This is a powerful capability that allows you to distribute your customized images wherever you need them.

### Region Replication and Object Storage

When you **replicate an image across a region**, you leverage **OCI Object Storage** as the transport mechanism. The image is exported to object storage, copied to the destination region's bucket, and then imported there.

---

### Launch Modes for Importing Images

Importing an image supports three different launch modes. Choosing the right mode depends on where the image came from and what drivers it supports.

| Launch Mode | When to Use It |
|-------------|----------------|
| Emulation Mode | For virtual machines that don't support paravirtualized drivers and were created outside of OCI — such as older on-premises physical or virtual machines |
| Paravirtualized Mode | For virtual machines that support paravirtualized drivers but were created outside of the OCI environment |
| Native Mode | For images that were exported from Oracle Cloud Infrastructure |

---

### Steps to Replicate an Image Across Regions

If you want to replicate an image from one region to another, follow these three steps:

1. Export the image to an object storage bucket in the same region as the image
2. Copy the image to an object storage bucket in the destination region
3. Import the image in the destination region using the URL path to the image object

---




