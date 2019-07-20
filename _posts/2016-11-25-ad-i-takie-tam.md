---
layout: post
title: AD i takie tam
type: post
categories:
- Bez kategorii
- Uniwersalne
tags:
- Active Directory
- AD
- Windows Server
permalink: "/ad-i-takie-tam/"
image: 'https://sienicki.dev/assets/tiles/drzewko-LDAP.jpg'
category: 'blog' 
introduction: Ostatnio w domowych pieleszach "bawię" się z windows server.
---
Bry!

Ostatnio w domowych pieleszach "bawię" się z windows server.

Będę dodawał do tego posta howTo dla siebie i być może kogoś jeszcze na przyszłości.

**[1] Wymuszenie synchronizacji pomiędzy dwoma kontrolerami domeny:**

1. Open Active Directory Sites and Services: On the **Start** menu, point to **Administrative Tools** and then click **Active Directory Sites and Services**.
2. In the console tree, expand **Sites**, and then expand the site to which you want to force replication from the updated server.
3. Expand the **Servers** container to display the list of servers that are currently configured for that site.
4. Expand the server objects and click their **NTDS Settings** objects to display their connection objects in the details pane. Find a server that has a connection object from the server on which you made the updates.
5. Click **NTDS Settings** below the server object. In the details pane, right-click the connection object whose **From Server** is the domain controller that has the updates that you want to replicate, and then click **Replicate Now**.
6. When the **Replicate Now** message box appears, review the information, and then click **OK**.

http://technet.microsoft.com/pl-pl/library/cc816926(v=ws.10).aspx

**[2] Generowanie nowego SID dla nowej maszyny - kopii**

C:\Windows\System32\Sysprep\Sysprep.exe

![sysprep-exe]({{ site.baseurl }}/assets/images/sysprep.exe_.png)