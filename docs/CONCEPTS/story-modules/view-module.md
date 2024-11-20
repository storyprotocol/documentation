---
title: View Module
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
# Overview

The View Module, inheriting from the Module in Story Protocol, is designed for interpreting and displaying IP data. Its primary role is to function as a read-only module, focusing on the presentation of IP-related data in various accessible formats.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3c35819-Untitled.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


# Design

The View Module is uniquely tailored to aggregate data from multiple namespaces within an IPAccount. This design enables it to present a comprehensive view of an IP, encompassing different data types and sources. It is structured to prioritize interpretability and user-friendly display of information.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/53c890b-Screenshot_2024-01-29_at_22.35.55.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


# Use Cases Example

- **Core Metadata Immutability:**Utilizes CoreMetadataModule to ensure the immutability of core IP metadata.
- **User-Defined Metadata:** Employs UserMetadataModule for new metadata additions and UserMetadataViewModule for reading and showcasing data from Core and User Metadata.
- **Metadata Upgrade/Migration: **Focuses on seamless data evolution with MetadataModuleV2 and MetadataViewModuleV2, combining information from original and upgraded metadata sources without the need for migration.