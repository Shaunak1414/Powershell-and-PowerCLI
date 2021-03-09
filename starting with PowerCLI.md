Few Commands to Start with
Get-VM

Get-VMHost

Get-Folder

Get-Datacenter

Get-Datastore

## Managing Tag Categories
Tag Categories allow you to group related tags together. When you define a category, you can also specify which object types its tags can be applied to and whether more than one tag in the category can be applied to an object. Creating a category with PowerCLI is easy:

`New-TagCategory –Name [name] -Description [description] -Cardinality [single/multiple] –EntityType [list of types]`

For example let’s create a category that will contain a tag for each vSphere user. We’ll name this category “Owner” and use it to tag the owners of Virtual Machines. The tags will only be applicable to Virtual Machines and a Virtual Machine can have only one owner. The following script creates such a category:

`New-TagCategory –Name “Owner” –Cardinality single –EntityType VirtualMachine`

The “-Cardinality” parameter specifies whether a single or multiple tags from this category can be applied to the same object at the same time. If you don’t specify it the default is “single”. The “-EntityType” parameter allows you to specify the object types to which you can attach tags from this category. If you omit the parameter by default the category will be applicable to all supported entity types.

The full list of supported entity types is: VirtualMachine, VM, VMHost, Folder, Datastore, DatastoreCluster, Cluster, ResourcePool, DistributedSwitch, DistributedPortGroup, VirtualPortGroup, VApp, Datacenter, All.

Modifying an existing TagCategory is done through Set-TagCategory cmdlet. You can change its name, description, cardinality (you can only extend it to “multiple”, restricting it to “single” is not possible) and add more entity types (again you can only extend the applicable entity types).

In our previous example the “Owner” category is only applicable to Virtual Machines, however VApps can also have an owner. Let’s modify the category to allow tagging VApps as well:

`Get-TagCategory “Owner” | Set-TagCategory –AddEntityType VApp`

Removing an existing category is just as easy by using the Remove-TagCategory cmdlet. Note that by removing a category you are also removing all tags in it and any assignments of these tags! Here is an example:

`Remove-TagCategory “Owner”`

Managing Tags
Once you have a tag category you are able to create new tags in it. This is done through the New-Tag cmdlet. For example in our “Owner” category let’s create a tag for John Smith:

`New-Tag –Name “jsmith” –Category “Owner”`

You can also create multiple tags – by reading the input values from CSV file or from other cmdlets. Here is how to create tags for each user in the “Example.org” domain:

# Retrieve all user accounts in the “Example.org” domain

$userList = Get-VIAccount –User –Domain “Example.org”

# For each user account create a new tag based on the user’s Id

foreach ($user in $userList) { New-Tag –Category “Owner” –Name $user.Id –Description $user.Description }

Modifying an existing tag is done through the Set-Tag cmdlet. It allows you to change the tag’s name and description:

`Get-Tag “jsmith” –Category “Owner” | Set-Tag –Description “John Smith”`

Removing a tag is done through Remove-Tag cmdlet, similar to removing a category. When removing a tag you will automatically remove any assignments of this tag.

Now that you have created some tags – it’s time to put them to use. You can assign tag to an entity using the New-TagAssignment cmdlet. Here is how to assign the “jsmith” tag to the VMs that belong to John (we assume they have “jsmith” in their name):

`Get-VM –Name *jsmith* | New-TagAssignment –Tag “jsmith”`

You can easily retrieve all tag assignments on a given entity:

`Get-VM jsmith_vm1 | Get-TagAssignment`

Or you can retrieve all VMs that have a given tag associated with them:

`Get-VM –Tag “jsmith”`