---
title: "sys.dm_os_hosts (Transact-SQL) | Microsoft Docs"
ms.custom: ""
ms.date: "08/18/2017"
ms.prod: "sql"
ms.prod_service: "database-engine"
ms.service: ""
ms.component: "dmv's"
ms.reviewer: ""
ms.suite: "sql"
ms.technology: 
  - "database-engine"
ms.tgt_pltfrm: ""
ms.topic: "language-reference"
f1_keywords: 
  - "sys.dm_os_hosts_TSQL"
  - "dm_os_hosts"
  - "dm_os_hosts_TSQL"
  - "sys.dm_os_hosts"
dev_langs: 
  - "TSQL"
helpviewer_keywords: 
  - "sys.dm_os_hosts dynamic management view"
ms.assetid: a313ff3b-1fe9-421e-b94b-cea19c43b0e5
caps.latest.revision: 35
author: "stevestein"
ms.author: "sstein"
manager: "craigg"
---
# sys.dm_os_hosts (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Returns all the hosts currently registered in an instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. This view also returns the resources that are used by these hosts.  
  
> [!NOTE]  
>  To call this from [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] or [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], use the name **sys.dm_pdw_nodes_os_hosts**.  
  
|Column name|Data type|Description|  
|-----------------|---------------|-----------------|  
|**host_address**|**varbinary(8)**|Internal memory address of the host object.|  
|**type**|**nvarchar(60)**|Type of hosted component. For example,<br /><br /> SOSHOST_CLIENTID_SERVERSNI= SQL Server Native Interface<br /><br /> SOSHOST_CLIENTID_SQLOLEDB = SQL Server Native Client OLE DB Provider<br /><br /> SOSHOST_CLIENTID_MSDART = Microsoft Data Access Run Time|  
|**name**|**nvarchar(32)**|Name of the host.|  
|**enqueued_tasks_count**|**int**|Total number of tasks that this host has placed onto queues in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**active_tasks_count**|**int**|Number of currently running tasks that this host has placed onto queues.|  
|**completed_ios_count**|**int**|Total number of I/Os issued and completed through this host.|  
|**completed_ios_in_bytes**|**bigint**|Total byte count of the I/Os completed through this host.|  
|**active_ios_count**|**int**|Total number of I/O requests related to this host that are currently waiting to complete.|  
|**default_memory_clerk_address**|**varbinary(8)**|Memory address of the memory clerk object associated with this host. For more information, see [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**pdw_node_id**|**int**|**Applies to**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> The identifier for the node that this distribution is on.|  
  
## Permissions

On [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], requires `VIEW SERVER STATE` permission.   
On [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], requires the `VIEW DATABASE STATE` permission in the database.   

## Remarks  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] allows components, such as an OLE DB provider, that are not part of the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] executable to allocate memory and participate in non-preemptive scheduling. These components are hosted by [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], and all resources allocated by these components are tracked. Hosting allows [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] to better account for resources used by components external to the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] executable.  
  
## Relationship Cardinalities  
  
|From|To|Relationship|  
|----------|--------|------------------|  
|sys.dm_os_hosts. default_memory_clerk_address|sys.dm_os_memory_clerks. memory_clerk_address|one to one|  
|sys.dm_os_hosts. host_address|sys.dm_os_memory_clerks. host_address|one to one|  
  
## Examples  
 The following example determines the total amount of memory committed by a hosted component.  
  
||  
|-|  
|**Applies to**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] through [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
  
```  
SELECT h.type, SUM(mc.pages_kb) AS commited_memory  
FROM sys.dm_os_memory_clerks AS mc   
INNER JOIN sys.dm_os_hosts AS h   
    ON mc.memory_clerk_address = h.default_memory_clerk_address  
GROUP BY h.type;  
```  
  
## See Also  

 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)   
 [SQL Server Operating System Related Dynamic Management Views &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


