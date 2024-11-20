---
title: IP Account
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
<Image align="center" src="https://files.readme.io/72cdabb-Beta_Protocol_Architecture_6.png" />

IP Account is a critical component that adheres to the EIP-6551 standard. As token-bound accounts, IP Accounts are uniquely deployed at deterministic addresses via the official 6551 account registry. These accounts, essentially smart contracts, are equipped to store comprehensive IP-related data, including metadata and ownership details of various NFTs such as License NFTs or Royalty NFTs. Serving as a centralized hub, IP Account streamlines the management and organization of intellectual property (IP) data.

The operational flexibility of IP Account is one of its key strengths. It can actively engage with various modules by initiating calls akin to a conventional transaction sender. This capability enables fluid and efficient manipulation of the state and data associated with IP. Importantly, any changes to the IP's state or data are meticulously recorded and stored within the IP Account, ensuring data integrity and traceability.

A fundamental aspect of IP Account in the **Programmable IP** framework is its role in data stewardship and module interaction. As a foundational component, IP Account not only stores IP-related data but also facilitates the utilization of this data by various modules. These modules interact with and contribute to the IP Account, creating and storing data. This symbiotic relationship enhances the functionality and efficiency of the IP management process.

One of the innovative features of IP Account is its approach to data sharing and standardization. It establishes a standardized method for storing and sharing data among modules based on namespaces. Each module is assigned its own namespace within the IP Account, wherein it has the autonomy to write data. However, in fostering a collaborative and transparent environment, each module is granted the ability to freely read data from the namespaces of other modules. This structure not only promotes data sharing but also maintains data segregation and security, ensuring that each module operates within its designated purview while benefiting from the collective intelligence of the system.
