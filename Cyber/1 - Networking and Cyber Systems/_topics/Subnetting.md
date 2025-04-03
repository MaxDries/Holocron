---
tags:
  - Cyber
  - yr11
---


> [!info] Written by Google Gemini


## **Subnetting: Dividing the Digital Neighborhood**

Subnetting takes the concept of network and host sections in IP addresses a step further. It allows you to **subdivide a larger network into smaller, logical subnetworks**, much like dividing a large apartment building into separate floors or wings. Here's how it works:

**Imagine:**

- You have a large office building with hundreds of employees.
- Instead of everyone sharing the same "building address," you want to create separate networks for different departments (e.g., marketing, HR, IT).

**Subnetting lets you achieve this by:**

1. **Borrowing bits:** You take some bits from the **host section** of the IP address and use them to define the subnetwork address. This effectively reduces the number of available host addresses within each subnet.
2. **Subnet mask:** A special 32-bit number (for IPv4) or 128-bit number (for IPv6) acts like a mask. It defines which bits belong to the network, subnet, and host sections. Devices use this mask to understand how to interpret IP addresses and route data correctly.

**Benefits of Subnetting:**

- **Improved security:** Separating departments into subnetworks restricts communication flow and reduces the risk of unauthorised access across the entire network.
- **Enhanced efficiency:** Smaller subnetworks experience less broadcast traffic, leading to better network performance and reduced congestion.
- **Flexible addressing:** You can create subnetworks with the exact number of devices each department needs, avoiding wasted IP addresses.

**Things to Consider:**

- **Planning:** Choosing the right subnet mask and network size requires careful planning based on the number of devices and security requirements.
- **Complexity:** Subnetting can become complex, especially for large networks. You need proper tools and understanding to configure and manage it effectively.

**Analogy:**

Think of subnetting like creating virtual floors within a large building. Each floor has its own set of residents (devices) with controlled access, improving organisation, security, and overall network health.

**I hope this explanation clarifies the concept of subnetting. Feel free to ask if you have any further questions or want to explore specific examples!**


