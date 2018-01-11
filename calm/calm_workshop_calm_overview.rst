*******
NuCalm 
*******


Overview
********

NuCalm allows Enterprise customers to seamlessly provision, deploy & manage their Business Apps across all of their
infrastructure. NuCalm ties together App Lifecycle, Monitoring & Remediation byproviding single pane of glass 
for heterogeneous infrastructure.

Key Terms
*********

Brief definition of key terms used in document. 

**Infrastructure**

Infrastructure is plain-jane infrastructure comprised of IaaS, consisting of Compute, Network & Storage. Infrastructure is 
dumb and does not understand the applications running on top of it. Infrastructure can be provided by multiple Providers. 
Some of these providers are in-house captive, some are pay-as-you-go utility providers. Irrespective of origin all 
infrastructure costs real dollars to run per unit-of-time. Some infrastructure comes with (practically) infinite capacity 
vs others have hard limits. A good analogy is energy consumption from Electricity companies vs having on-prem Diesel 
Generators. Examples are AWS, vCenter, Azure.

**Blueprints**

Blueprints are App Recipes. These recipes encompass App Architecture, Infrastructure choices, Provisioning & Deployment 
steps, App Bits, Command steps, Monitoring endpoints, Remediation steps, Licensing & Monetization, Policies. Every time a 
Blueprint is executed it gives rise to an App.


**App**

App is a deployed Blueprint. Every time a Blueprint runs it creates a new App instance. Apps have their own life cycle.

An App has the following life cycle steps:

1. Instantiation: A blueprint is instantiated to setup the application. Instantiation is 

i. Provision the Infrastructure components (compute, storage, network)

ii.	Fetch the App Bits
iii.	Deploy & Configure the App Bits on infrastructure components
iv.	Run the Sanity Checks

2. Running: After instantiation, the App is up and running. In running stage the application needs periodic Command steps
to keep it healthy and operational. These include upgrades, scale-up, scale-down, start, stop, backup (i.e. common App 
specific actions defined in the blueprint).

3. Destruction: At a certain point the instantiated App is no longer useful. A destruction (or delete) operation undoes 
all the creation steps, makes sure all the tied up resources (Infrastructure) is returned to the common pool


**Blueprint Components**

Important components:

1. App Architecture: App architecture specifies how the different components in the target App are connected. This comprises of nodes of different types (compute, storage, network) and the connections between them.

2. Infrastructure choices: Any useful blueprint needs Infrastructure for instantiation. A blueprint can specify the exact infrastructure needed (n AWS VM, m Nutanix VM), a predefined palette or can be completely left to user to specify at instantiation time (late binding). The blueprint developer can also specify policies (or constraints) on the type of infrastructure needed. The platform will not let a blueprint be instantiated if the policies are not met. Other additional policies can be overlaid on the blueprint specified ones later, depending on the organisation setup.

3. Provisioning steps: Provisioning is the action of creating infrastructure components (VMs, Firewalls, Containers, Storage,...). Provisioning is usually performed by calling out the Provider specific APIs or commands.

4. App Bits: App Bits are the actual software needed for the application to run. A blueprint should have URIs pointing to repositories from where the actual bits are fetched. A blueprint should not bundle the application bits, for size & IP concerns.

5. Deployment steps: Deployment steps are the commands/scripts needed to setup the App bits to run on the provisioned infrastructure. These are the steps run on each node of infrastructure to setup the node-specific software. Since some of these nodes are virtual endpoints (S3 buckets) these steps can also be specified in terms of API operations that virtual endpoint supports.

6. Command Steps: Command steps are common actions needed to maintain an application. Some of these steps run only on one node in the application while others are multi-node orchestrated flows. Examples include: upgrade, scale-up, scale-down, backup, restore, start, stop. Most of these Commands are specified by the Blueprint developer but the end consumer (with appropriate permissions) should be able to add more to simplify their common use-cases.

7. Monitoring Endpoints: A blueprint optionally includes the steps needed to configure common monitoring solutions to setup monitoring for the newly deployed App. The blueprint specifies health checks and metrics along with warning & error thresholds for each node. In addition the blueprint specifies endpoints into the NuCalm platform where monitoring should feed alerts and other data.

8. Remediation steps: Remediation steps are needed to get the App to a healthy stage after monitoring or NuCalm detects runtime errors or alerts. They are triggered by data from the underlying platform or monitoring endpoints.

9. Licensing & Monetization: A blueprint needs to include machine-readable bits on its licensing restrictions. This informs NuCalm if the blueprint is editable or shareable by the consumer. NuCalm can hide the actual scripts from the consumer if  so specified. Monetization decides if the blueprint publisher charges a cost for using it. See Chargeback.

10. Policies: Policies are requirements for other different components for a blueprint. Policies specify what meta-objectives have to be met for a successful instantiation and use. For example, a policy can specify that the desired App can be instantiated on on-prem Infrastructure, or that a specific node type always requires more than 4 GB RAM.


**AppStore**

An AppStore is essentially a classical economics Marketplace. Marketplace is the exchange channel between blueprint publishers and consumers. Publishers upload or publish their blueprints to the Marketplace to make it available for Consumers. Consumers search/browse the Marketplace to find desired Blueprints and then (depending on other considerations) download and use them


Key Actors / Dramatis Persona
*****************************

1.	Publisher / Producer: The publisher is responsible for developing Blueprints. 

2.	Consumer / Customer: The consumer uses the Blueprints to deploy and manage desired Apps. 

3.	Infrastructure Admin (Admin): The Infrastructure Admin is responsible for buying, setting up and maintaining the IaaS. This includes one or more people in the IT group that maintain and run the Infrastructure Platforms. Examples are the vCenter Admin team, the Xi Admin team, The inhouse AWS Admin team.

4.	IT Admin (DevOps): The IT Admin manages Apps deployed on the Infrastructure (in contrast to Infrastructure Admins that manage the pure Infrastructure). The IT Admins also set organization IT policies to meet business goals.

5.	OOB Users: These are users who do not exist in the system but are needed for approvals, notifications


AppStore / Marketplace
**********************

In designing our App Store we have two main choices, with different mix-n-match possible:

1.	Vertically Integrated / Walled Garden Only Nutanix (and carefully vetted partners) are allowed to publish Blueprints (heavy regulation).

2.	Two-sided Open Market Third party publishers (ISV ) can publish Blueprints, subject to meeting objective criteria (lightweight regulation).

Two sided markets are notoriously hard to bootstrap. The usual approach is to create a high quality walled garden to build a customer base and then getting more third party producers in. This avoids the chicken and egg problem of bringing of both producers and consumers onboard at the same time.

We have an additional wrinkle in that NuCalm can be deployed in a completely isolated on-prem installations where the users might want to publish Blueprints for internal consumption. 

|image0|

Functions of an AppStore
************************

**Discovery**

An AppStore allows consumers to discover needed services. In our case customers should be able to search by various criteria and recommendations to find blueprints they are interested in.

**Reputation Metrics**

AppStore keeps track of reputation, ratings & feedback of both producers and consumers. This greatly aids Discovery. 

**Transaction Guarantees**

AppStore provides transaction guarantees to producers and consumers when they enter into an exchange (when Blueprints are consumed or updated). If we allow monetization this guarantees the producer gets paid (in whatever virtual currency). 

**Enforceable Property Rights**

AppStore provides platform enforced intellectual property rights. This includes controls over if a Blueprint is shareable, editable, internals visible. Producers desire these guarantees for their IP.

**Support Forums**

Support forums provide a channel for the producers and consumers to interact outside of the produce-consume cycle. This helps in building communities and feeds into the reputation metrics.

**Costing and Chargeback / Monetization**

AppStore lets consumers see the costs associated with a Blueprint, including upfront costs and ongoing running costs.

**Curation and Approvals**

AppStore provides curation and approvals for consuming blueprints, enforced by the competent authorities. The competent authorities here include: AppStore owners (Nutanix & on-prem admin), IT Admins & Platform Admins.


Publishers
**********

Publishers produce the Blueprints for use by Consumers. 

**Publisher personas**

1.	Nutanix team
2.	Customer IT-Ops/DevOps team
3.	Customer Developers (for inhouse apps)
4.	Third Parties (ISV)

**Publisher Incentives**

Publishers have various overlapping incentives to build Blueprints.

1.	Enable Self Service for consumers within organization to reduce workload
2.	Promote ease-of-use of the platform (probably only true for Nutanix team)
3.	Get paid for know-how in Blueprint
4.	Social Standing

**Publisher Concerns**

1.	Loss of control over usage
2.	Intellectual property leakage
3.	Security / Secret Sauce leakage

**Publisher Workflow**

|image1|

.. |image0| image:: ./media/image1.png
.. |image1| image:: ./media/image2.png

