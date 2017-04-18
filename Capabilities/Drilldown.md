# Drill down in a custom visual

Custom visuals are capable of using PowerBI's drill down.

Read more about Power BI drill down [here](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-drill-down-in-a-visualization/)


## How to add drill down support to your custom visual

To support drill down in your visual, add a new field to "capabilities.json" named `drilldown` which has one property:

**roles** - the name of the dataRole you want to enable drill down on. 
> **NOTE:**  
The drill down dataRole must be of 'Grouping' type.  
"max" property in the dataRole conditions must be set to 1. 

Once you add the role to `drilldown`, users can drag multiple fields into the data role.
 
example:

![Adding Drill down](../images/addDrillDown2.png)
