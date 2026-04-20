<p align="center">
  <img src="images/project picture.png" alt="Azure VM Setup" width="50%" />
</p>

<h1 align="center">Network Security Groups (NSGs) &amp; Traffic Inspection Between Azure Virtual Machines</h1>

<p>
  This project demonstrates how to deploy Azure virtual machines, analyze real network traffic using Wireshark, and control communication using Network Security Groups (NSGs). The lab focuses on understanding how systems communicate and how security rules impact network behavior.
</p>

<hr>

<h2>🎯 Goals &amp; Objectives</h2>

<p>
  The goal of this project was to build a working network environment in Azure and understand how traffic flows between systems at the packet level.
</p>

<p>By the end of this lab, I aimed to:</p>

<ul>
  <li>Deploy multiple virtual machines in a shared network</li>
  <li>Capture and analyze traffic using Wireshark</li>
  <li>Observe key protocols (ICMP, SSH, DHCP, DNS, RDP)</li>
  <li>Apply and test Network Security Group rules</li>
  <li>Understand how security configurations affect connectivity</li>
  <li>Improve troubleshooting through direct observation</li>
</ul>

<hr>

<h2>📌 Overview</h2>

<p>
  In this project, I created a Windows and Ubuntu virtual machine within the same Azure Virtual Network to simulate a basic network environment. Using Wireshark, I captured and analyzed traffic between the machines while generating different types of network activity.
</p>

<p>
  I then modified Network Security Group rules to block and allow traffic, which demonstrated how cloud-level security controls influence communication between systems.
</p>

<hr>

<h2>🧰 Technologies Used</h2>

<ul>
  <li>Microsoft Azure (Virtual Machines)</li>
  <li>Network Security Groups (NSGs)</li>
  <li>Wireshark</li>
  <li>Remote Desktop Protocol (RDP)</li>
  <li>SSH</li>
  <li>PowerShell</li>
  <li>DNS</li>
  <li>DHCP</li>
  <li>ICMP</li>
</ul>

<hr>

<h2>💻 Environment</h2>

<ul>
  <li>Windows 10 Virtual Machine</li>
  <li>Ubuntu Virtual Machine</li>
  <li>Azure Virtual Network (shared)</li>
  <li>Azure Subnet</li>
  <li>Network Security Group</li>
  <li>Wireshark installed on the Windows VM</li>
</ul>

<hr>

<h2>⚙️ Implementation</h2>

<h3>1. Infrastructure Setup</h3>

<ul>
  <li>Created a Resource Group</li>
  <li>Deployed Windows and Ubuntu virtual machines</li>
  <li>Ensured both VMs were placed in the same Virtual Network and Subnet</li>
</ul>

<p>
  This setup allowed communication over private IP addresses within a controlled environment.
</p>

<p align="center">
  <img src="images/vm-setup.png" alt="Azure VM Setup" width="90%" />
</p>

<hr>

<h3>2. ICMP Traffic Observation</h3>

<ul>
  <li>Installed Wireshark on the Windows VM</li>
  <li>Captured live network traffic</li>
  <li>Generated ICMP traffic by:
    <ul>
      <li>Pinging the Ubuntu VM using its private IP address</li>
      <li>Pinging a public website such as google.com</li>
    </ul>
  </li>
</ul>

<p>
  I observed successful request and reply packets, confirming connectivity.
</p>

<p align="center">
  <img src="images/icmp-traffic.png" alt="ICMP Traffic" width="90%" />
</p>

<hr>

<h3>3. Network Security Group Testing</h3>

<ul>
  <li>Started a continuous ping from the Windows VM to the Ubuntu VM</li>
  <li>Applied an NSG rule to block inbound ICMP traffic</li>
  <li>Observed that requests continued while replies stopped</li>
  <li>Re-enabled ICMP traffic and confirmed that replies resumed</li>
</ul>

<p>
  Blocking ICMP stopped replies but not outgoing requests, creating one-way communication. Re-enabling the rule restored normal connectivity.
</p>

<table align="center">
  <tr>
    <td align="center">
      <img src="images/ping-running.png" alt="Ping Working Before NSG Rule" width="100%"><br>
      <sub>Before NSG Rule (Working)</sub>
    </td>
    <td align="center">
      <img src="images/ping-fail.png" alt="Ping Failing After NSG Rule" width="100%"><br>
      <sub>After NSG Rule (Blocked)</sub>
    </td>
  </tr>
</table>

<br>

<p align="center">
  <img src="images/nsg-block.png" alt="NSG Blocking ICMP" width="90%" />
</p>

<p align="center">
  <img src="images/ping-restored.png" alt="Ping Restored After Re-enabling ICMP" width="90%" />
</p>
<p align="center">
  <sub>Connectivity restored after re-enabling ICMP.</sub>
</p>

<hr>

<h3>4. SSH Traffic Observation</h3>

<ul>
  <li>Filtered Wireshark for SSH traffic</li>
  <li>Initiated an SSH session from Windows to Ubuntu</li>
  <li>Executed commands within the session</li>
</ul>

<p>
  I observed encrypted traffic on port 22, confirming secure remote communication.
</p>

<p align="center">
  <img src="images/ssh-traffic.png" alt="SSH Traffic" width="90%" />
</p>

<hr>

<h3>5. DHCP Traffic Observation</h3>

<ul>
  <li>Filtered Wireshark for DHCP traffic</li>
  <li>Issued a new IP request using:</li>
</ul>

<pre><code>ipconfig /renew</code></pre>

<p>
  I observed DHCP request and response packets, demonstrating dynamic IP assignment.
</p>

<p align="center">
  <img src="images/dhcp-traffic.png" alt="DHCP Traffic" width="90%" />
</p>

<hr>

<h3>6. DNS Traffic Observation</h3>

<ul>
  <li>Filtered Wireshark for DNS traffic</li>
  <li>Used:</li>
</ul>

<pre><code>nslookup google.com
nslookup disney.com</code></pre>

<p>
  I observed DNS queries and responses, showing how domain names are resolved to IP addresses.
</p>

<p align="center">
  <img src="images/dns-traffic.png" alt="DNS Traffic" width="90%" />
</p>

<hr>

<h3>7. RDP Traffic Observation</h3>

<ul>
  <li>Filtered Wireshark for RDP traffic (<code>tcp.port == 3389</code>)</li>
</ul>

<p>
  I observed continuous traffic because Remote Desktop constantly transmits display, input, and session data between systems.
</p>

<p align="center">
  <img src="images/rdp-traffic.png" alt="RDP Traffic" width="90%" />
</p>

<hr>

<h2>🔍 Troubleshooting</h2>

<h3>ICMP Blocking Behavior</h3>

<ul>
  <li><strong>Problem:</strong> Ping stopped receiving replies after applying the NSG rule</li>
  <li><strong>Cause:</strong> Inbound ICMP traffic was blocked at the network level</li>
  <li><strong>Fix:</strong> Re-enabled ICMP traffic in the NSG</li>
</ul>

<p>
  This demonstrated that traffic can still be sent even when responses are blocked.
</p>

<h3>Protocol Visibility</h3>

<ul>
  <li>Initially expected all traffic to behave similarly</li>
  <li>Observed that each protocol has unique patterns:
    <ul>
      <li>ICMP: request/reply</li>
      <li>SSH: encrypted session traffic</li>
      <li>DHCP: broadcast-based communication</li>
      <li>DNS: query/response</li>
      <li>RDP: continuous stream</li>
    </ul>
  </li>
</ul>

<p>
  This reinforced the importance of protocol-specific analysis.
</p>

<hr>

<h2>🧠 Design Decisions</h2>

<ul>
  <li>Placed both VMs in the same Virtual Network to enable internal communication</li>
  <li>Used private IP addresses to simulate internal enterprise traffic</li>
  <li>Used continuous ping to clearly observe changes when applying NSG rules</li>
  <li>Used Wireshark instead of relying only on Azure tools to validate actual packet behavior</li>
</ul>

<hr>

<h2>🛡️ Security Awareness</h2>

<ul>
  <li>NSGs function as cloud-level firewalls controlling inbound and outbound traffic</li>
  <li>Blocking traffic at the network level does not stop packets from being sent, only from being received</li>
  <li>Misconfigured rules can silently block communication without obvious errors</li>
  <li>Understanding traffic patterns helps identify abnormal or malicious activity</li>
</ul>

<hr>

<h2>🌍 Real-World Relevance</h2>

<ul>
  <li>Network Security Groups are widely used to control access between systems in cloud environments</li>
  <li>Packet analysis tools like Wireshark are essential for diagnosing connectivity issues</li>
  <li>Protocol-level understanding is critical for both IT support and cybersecurity roles</li>
  <li>Troubleshooting requires validating assumptions with real data</li>
</ul>

<hr>

<h2>📌 Lessons Learned</h2>

<ul>
  <li>Connectivity issues are not always obvious and require step-by-step validation</li>
  <li>Blocking traffic can create one-way communication without clear error messages</li>
  <li>Different protocols behave in fundamentally different ways</li>
  <li>Observing real traffic provides deeper understanding than following configuration steps alone</li>
  <li>Small configuration changes can significantly impact system behavior</li>
</ul>

<hr>

<h2>💭 Key Takeaways</h2>

<p>
  Before this lab, I viewed networking primarily as configuration-based. This project showed that understanding actual traffic behavior is essential for diagnosing issues and validating system functionality.
</p>

<p>
  By combining traffic visibility with security controls, I was able to better understand how communication between systems is established, interrupted, and restored.
</p>

<hr>

<h2>🧹 Cleanup</h2>

<ul>
  <li>Closed the Remote Desktop session</li>
  <li>Deleted the Resource Group</li>
  <li>Verified that all resources were removed</li>
</ul>

<p>
  This ensured no unnecessary cloud resources were left running.
</p>

<p align="center">
  <img src="images/resource-cleanup-all.png" alt="Resource Cleanup" width="90%" />
</p>
