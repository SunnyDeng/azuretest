<properties
   pageTitle="Big Compute: Technical resources for batch and high performance computing (HPC) | Microsoft Azure"
   description="Lists technical resources to help you run your large-scale parallel, batch, and HPC workloads in Azure."
   services="batch, cloud-services, virtual-machines"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="09/02/2015"
   ms.author="danlep"/>

# Big Compute in Azure: Technical resources for batch and high performance computing (HPC)
This is a guide to technical resources to help you run your large-scale parallel, batch, and HPC workloads in Azure. Extend your existing batch or HPC workloads to the Azure cloud, or build new Big Compute solutions in Azure using a range of Azure services.

## Solutions options

Learn about Big Compute options in Azure, and choose the right approach for your workload and business need.

* [Batch and HPC solutions](batch-hpc-solutions.md)

* [Video: Big Compute in the cloud with Azure and HPC](http://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)


## Azure Batch

[Batch](http://azure.microsoft.com/services/batch/) is a platform service that makes it easy to cloud-enable your applications and run jobs without setting up and managing a cluster and job scheduler. Use the SDK to integrate client applications with Azure Batch through a variety of languages, stage data to Azure, and build job execution pipelines.

* [Documentation](http://azure.microsoft.com/documentation/services/batch/)

* [API reference](https://msdn.microsoft.com/library/azure/dn820177.aspx)

* [Tutorial: Getting started with Azure Batch library for .NET](batch-dotnet-get-started.md)

* [Batch forum](https://social.msdn.microsoft.com/Forums/home?forum=azurebatch)

* [Batch videos](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## HPC cluster solutions

Deploy or extend your existing Windows or Linux HPC cluster to Azure to run your compute intensive workloads.  

### Microsoft HPC Pack

HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies.  

* [Download HPC Pack 2012 R2 Update 2](https://www.microsoft.com/en-us/download/details.aspx?id=47755)

* [Documentation](https://technet.microsoft.com/library/jj899572.aspx)

* **Azure VM images**

    * [HPC Pack on Windows Server 2012 R2](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

    * [HPC Pack compute node on Windows Server 2012 R2](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

    * [HPC Pack compute node with Excel on Windows Server 2012 R2](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

* **Azure quickstart templates and scripts**

    * [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/)

    * [Create an HPC cluster with a custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/)

    * [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

    * [HPC Pack IaaS deployment PowerShell script](https://www.microsoft.com/download/details.aspx?id=44949)

* **Azure tutorials**

    * [Tutorial: Get started with Linux compute nodes in an HPC Pack cluster in Azure](../virtual-machines/virtual-machines-linux-cluster-hpcpack.md)

    * [Tutorial: Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure](../virtual-machines/virtual-machines-linux-cluster-hpcpack-namd.md)

    * [Tutorial: Get started with an HPC Pack cluster in Azure to run Excel and SOA workloads](../virtual-machines/virtual-machines-excel-cluster-hpcpack.md)

    * [Tutorial: Set up a hybrid cluster with HPC Pack in Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

    * [HPC Pack in Azure VMs](https://msdn.microsoft.com/library/azure/dn518135.aspx)

    * [Burst to Azure with HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)


* [Windows HPC forums](https://social.microsoft.com/Forums/home?category=windowshpc)

### Linux and OSS cluster solutions

Use these Azure quickstart templates to deploy Linux HPC clusters with open source tools.

* [Spin up a SLURM cluster](http://azure.microsoft.com/documentation/templates/slurm/)
 and [blog post](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)

* [Spin up a Torque cluster](http://azure.microsoft.com/documentation/templates/torque-cluster/)

## Microsoft MPI

[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) is a Microsoft implementation of the Message Passing Interface standard for developing and running parallel applications on the Windows platform.


* [Download MS-MPI](http://go.microsoft.com/FWLink/p/?LinkID=389556)

* [MS-MPI reference](https://msdn.microsoft.com/library/dn473458.aspx)

* [MPI forum](https://social.microsoft.com/Forums/home?forum=windowshpcmpi)


## Compute intensive instances

Azure offers a [range of sizes](../virtual-machines/virtual-machines-size-specs.md), including compute intensive [A8, A9, A10, and A11 instances](../virtual-machines/virtual-machines-a8-a9-a10-a11-specs.md), to run your Linux and Windows HPC workloads.

* [Set up a Linux RDMA cluster to run MPI applications](../virtual-machines/virtual-machines-linux-cluster-rdma.md)

* [A8 and A9 instances: Quick start with HPC Pack](https://msdn.microsoft.com/library/azure/dn594431.aspx)

* [Run MPI applications on A8 and A9 instances](https://msdn.microsoft.com/library/azure/dn592104.aspx)

## Architecture blueprints

* [Large-scale computing - financial services](http://go.microsoft.com/fwlink/?LinkId=536378) (PDF) shows how to operationalize and orchestrate large-scale computation and data analysis in the cloud for risk management, reporting, and simulations.

## Samples and demos

* [Azure Batch code samples](https://github.com/Azure/azure-batch-samples)

* [Batch Apps Blender sample](https://github.com/Azure/azure-batch-apps-blender) and [blog post](http://azure.microsoft.com/blog/2015/01/26/blender-on-azure-batch/)

## Related Azure services

* [Data Factory](http://azure.microsoft.com/documentation/services/data-factory/)

* [Machine Learning](http://azure.microsoft.com/documentation/services/machine-learning/)

* [HDInsight](http://azure.microsoft.com/documentation/services/hdinsight/)

* [Virtual Machines](http://azure.microsoft.com/documentation/services/virtual-machines/)

* [Cloud Services](http://azure.microsoft.com/documentation/services/cloud-services/)

* [Media Services](http://azure.microsoft.com/documentation/services/media-services/)



## Next steps

* For the latest announcements, see the [Microsoft HPC and Batch team blog](http://blogs.technet.com/b/windowshpc/) and the [Azure blog](http://azure.microsoft.com/blog/tag/hpc/).
* Also see [what's new in Batch](http://azure.microsoft.com/updates/?service=batch) or subscribe to the [RSS feed](http://azure.microsoft.com/updates/feed/?service=batch).

test
