# 5. Profile Options

The Supply Chain Hub includes a set of configurable profile options that determine system behavior, UI availability, and integration with other Oracle applications. These profile options must be reviewed and set according to the organization's supply planning, release management, pricing, and performance requirements.

> **Important:** After installation, many of these options may appear blank. This is expected, and administrators are advised to populate them explicitly based on intended usage scenarios and planning sources (ASCP or MRP).

## Profile Options Reference

| Profile Option | Description | Default |
|----------------|-------------|---------|
| **Supply Chain Hub: Enable Release** | Enable Release of Planned Orders from Supply Chain Hub<br>- ASCP – supports Planned Order Release of Purchase Requisition and Work Order recommendations<br>- MRP – only supports Release of Work Order recommendations | Yes |
| **Supply Chain Hub: Enable Reschedule and Cancel Actions** | Enable Reschedule and Cancel Actions<br>- Only applicable to ASCP<br>- Allows for the schedule in/schedule out/cancel recommendations to be actioned from Supply Chain Hub<br>- This is done via the Release Button or via the Right Click Release Planned Order menu option | Yes |
| **Supply Chain Hub: Calculate Price** | Calculate Selling Price from Price List<br>- If yes a call to the Pricing Engine will be made to determine the selling Price of the Item which is displayed in the Item Details Tab | Yes |
| **Supply Chain Hub: Demand Price List** | Price list for price display on the item details tab | |
| **Supply Chain Hub: Drilldown after Release** | Drilldown to Work Order after Planned Order Release<br>- Only Applies to MRP Release to Work Orders | Yes |
| **Supply Chain Hub: Exclude Complete Work Orders** | Exclude Complete Work Orders from Supply/Demand | No |
| **Supply Chain Hub: Exclude Inventory Master Organizations** | Exclude Inventory Master Organizations from the Organizations List of Values | Yes |
| **Supply Chain Hub: Item Attachment Category** | Category for attachment title and description display in Supply Chain Hub Item Details Tab | null |
| **Supply Chain Hub: Show KPIs in Item Details Tab** | Show Item KPI Quantities in the Item Detail Tab<br>- generally only disabled if performance is an issue in the Item Details Tab | Yes |
| **Supply Chain Hub: Show KPIs in Item Search Tab** | Show Item KPI Quantities in the Item Search Tab<br>- generally only disabled if performance is an issue in the Item Search Tab | Yes |
| **Supply Chain Hub: Show Release Button** | Show the Release Button in the Supply/Demand Tab<br>- If Yes, a Release button is displayed in the left hand panel of the Supply/Demand tab<br>- If No, the Release Planned Orders action is accessible as a menu option in the Right Click Menu after selecting the Planned Orders to Release | No |
| **Supply Chain Hub: Show Tooltip Help** | Enable or disable tooltip help display for Supply Chain Hub | Yes (Enabled) |
| **Supply Chain Hub: Use MRP for Planning Source** | Tells Supply Chain Hub to use MRP instead of ASCP | No (ASCP) |
| **Supply Chain Hub: Use Quick Sales Orders Form** | Supply Chain Hub: Use Quick Sales Orders Form for Drilldowns<br>- By default Supply Chain Hub drills down to the Orders Workbench<br>- This profile can be used to direct Supply Chain Hub to drill down to the Quick Orders Sales Order form instead | No |
| **Supply Chain Hub: Use Purchasing Buyer Work Center** | Supply Chain Hub: Use Purchasing Buyer Work Center for Drilldowns<br>- By default Supply Chain Hub drills down to the Purchasing Forms<br>- This profile can be used to direct Supply Chain Hub to drill down to the Purchasing Buyers Work Center instead | No |

## Best Practices

- **Review and set profile options explicitly** based on your planning approach (MRP vs ASCP), user workflows, and UI preferences.
- **Performance tuning:** Disable KPI and tooltip options in large data environments if performance becomes a concern.
- **Drilldown configurations:** Adjust drilldown forms (e.g., Quick Sales Orders Form, Buyer Work Center) based on user roles and departmental preferences.
- **Testing:** Always validate profile option settings in a non-production environment before deploying to production.

## Notes

- Unset (blank) values may result in default behaviors, but these are not always suitable for every deployment.
- Some profile options, such as those related to drilldown forms or price lists, are environment-specific and may require coordination with functional consultants and business owners.

*Previous: [Application Setup](part3_application_setup.md) | Next: [Upgrade](part5_upgrade.md)*
