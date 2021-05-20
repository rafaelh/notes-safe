# AZURE RESOURCE MANAGER
  ======================

A resource can have up to 15 tags, with each name being limited to 512 characters, with the exception of storage resources which have a limit of 128 characters. The tag value is limited to 256 characters for all resource. Resources don't inherit tags from their parents, and not all resources support tags, such as 'classic' resources.

``` sh
az resource tag --tags Department=Finance \
                --resource-group msftlearn-core-infrastructure-rg \
		--name msftlearn-vnet1 \
		--resource-type "Microsoft.Network/virtualNetworks"
```

Tags can be added automatically by Azure Policy.


