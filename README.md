# compute-availabilitySets
Reference Template
```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "StorageAccountName": {
      "type": "string"
    },
    "ContainerName": {
      "type": "string"
    },
    "SasToken": {
      "type": "string"
    }
  },
  "variables": {
    "Provider": "/Microsoft.Compute",
    "Resource": "/availabilitySets",
    "templateUri": "[concat('https://',parameters('StorageAccountName'),'.blob.core.windows.net/',parameters('ContainerName'),variables('Provider'),variables('Resource'))]"
  },
  "resources": [
    {
      "name": "DeployAvailabilitySet",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2016-09-01",
      "dependsOn": [ ],
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[concat(variables('templateUri'), '/availabilitySets.json', parameters('SasToken'))]",
          "contentVersion": "2017.09.01.0"
        },
        "parameters": {
          "name": {
            "value": "avSet"
          },
          "skuName": {
            "value": "Aligned"
          },
          "tags": {
            "value": "[json('{\"TagName\": \"TagValue\"}')]"
          }
        }
      }
    }
  ],
  "outputs": {
    "availabilitySets": {
      "type": "object",
      "value": "[reference('DeployAvailabilitySet').outputs.availabilitySets.value]"
    }
  }
}
```
