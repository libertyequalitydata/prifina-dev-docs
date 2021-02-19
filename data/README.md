# User Data Access

At Prifina's core is the goal of improving the quality of data-driven user experiences on the web. A key aspect of this is providing accessible user data to developers, while maintaining the user's ultimate control of their own privacy and data. Prifina provides interfaces allowing developers to leverage user data, creating the potential for a new breed of data-driven applications and services.   

Current interfaces include:  

![Data Flow](./data-access-diagram.png)
- [Aggregated data](aggregated) - Local data and widget applications built on Prifina can access the powerful Aggregated Data API. At its core, this is a GraphQL API supplying aggregated user data. This data is tightly controlled, and the user must grant permission to access subsets of it and generally speaking, it must remain within the user's personal data cloud.
- [Profile data](profile) - Data which can be provided to external applications and services (those not running within the user's personal cloud) at the user's discretion. The user can manage the scope of available profile data, and access permissions on a per-service basis, with Profile Cards in their Prifina console.
