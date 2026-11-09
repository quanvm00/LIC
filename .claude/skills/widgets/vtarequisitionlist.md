# VTA Requisition List

- **Widget ID:** `vta.vtarequisitionlist.VtaRequisitionList`
- **Type:** PLUGGABLEWIDGET
- **Version:** 0.1.0

## MDL Example

```sql
PLUGGABLEWIDGET 'vta.vtarequisitionlist.VtaRequisitionList' widget1 {
  multipleactions slot1 {
    -- widgets for `multipleActions`
  }
  multiplepanelactions slot2 {
    -- widgets for `multiplePanelActions`
  }
  rowactions slot3 {
    -- widgets for `rowActions`
  }
  menuactions slot4 {
    -- widgets for `menuActions`
  }
  detailextra slot5 {
    -- widgets for `detailExtra`
  }
  panelactions slot6 {
    -- widgets for `panelActions`
  }
}
```

## Properties

| Property | Type | Required | Default | Values / notes | Group | Description |
|----------|------|----------|---------|----------------|-------|-------------|
| `datasource` | datasource | Yes |  | list | Data::Source | List of requisitions, e.g. microflow ACT_Get_Requisition_By_Current_User with the HelperSearchForm parameter. |
| `attrCode` | attribute | Yes |  |  | Data::Fields | Request.Code. |
| `attrTitle` | attribute | Yes |  |  | Data::Fields | Request.RequestTitle. |
| `attrType` | attribute |  |  |  | Data::Fields | RequisitionRequest.RequisitionType. Also drives the All / Single / Multiple switch in the toolbar. |
| `attrDepartment` | attribute |  |  |  | Data::Fields | Request_RootBudgetItem / BudgetItem.OriginalCode. Also drives the department filter, shown when the list has two or more departments. |
| `attrAmount` | attribute |  |  |  | Data::Fields | Stored amount, shown with grouping and 2 decimals. Never recomputed. |
| `attrProcessStatus` | attribute | Yes |  |  | Data::Status | Request.ProcessStatus. The enum key drives the status family; the caption is displayed. |
| `attrWorkflowStatus` | attribute | Yes |  |  | Data::Status | Request.WorkflowStatus. The enum key drives the status family; the caption is always shown next to the badge. |
| `attrRequestStatus` | attribute |  |  |  | Data::Status | Request.RequestStatus. Shown under "Additional status" in the expanded panel. |
| `textPic` | textTemplate |  |  |  | Data::People | Text template, e.g. "{1} — {2}" with Request_Data_Owner_Account / Account Name and FullName. Shown in the focus line and the expanded panel. |
| `exprPicIsMe` | expression |  |  |  | Data::People | Optional, e.g. $currentObject/BudgetControl.Request_Data_Owner_Account = [%CurrentUser%]. Adds "(You)" after the PIC. |
| `textApprover` | textTemplate |  |  |  | Data::People | Text template, e.g. "{1} — {2}" with CurrentApprover / Account Name and FullName. |
| `exprApproverIsMe` | expression |  |  |  | Data::People | Optional, e.g. $currentObject/BudgetControl.CurrentApprover/Administration.Account = [%CurrentUser%]. Adds "(You)" and the "Awaiting your approval" chip. |
| `textCreatedBy` | textTemplate |  |  |  | Data::People | Text template, e.g. "{1} — {2}" with the creator Name and FullName, or Request.CreatedBy alone. |
| `exprCreatedByIsMe` | expression |  |  |  | Data::People | Optional. Adds "(You)" after the creator. |
| `attrStartDate` | attribute |  |  |  | Data::Dates | Request.StartDate. |
| `attrEndDate` | attribute |  |  |  | Data::Dates | Request.EndDate. |
| `attrPaymentDate` | attribute |  |  |  | Data::Dates | ExpectedPaymentDate. Not treated as a deadline. |
| `attrExtendDate` | attribute |  |  |  | Data::Dates | ExtendDate. |
| `attrRequestTypeName` | attribute |  |  |  | Data::Related information | Request_RequestType / RequestType.RequestTypeName. |
| `attrRequestTypeCode` | attribute |  |  |  | Data::Related information | Request_RequestType / RequestType.RequestTypeCode. Shown under "Additional status". |
| `attrEventCode` | attribute |  |  |  | Data::Related information | BudgetItemRequisitionRequestHdr_BudgetItemRequisitionEventRequest / Code. Rows sharing a code are grouped under one Multiple header. Use the same entity on the path as the other Multiple fields (BudgetItemRequisitionEventRequest, not its specialization RequisitionEventRequest); mixing them makes the runtime fail with "Associated object should not have already been retrieved over schema". |
| `attrEventAmount` | attribute |  |  |  | Data::Related information | BudgetItemRequisitionRequestHdr_BudgetItemRequisitionEventRequest / Amount. Stored total shown on the group header; never recomputed. Use the same entity on the path as the other Multiple fields (BudgetItemRequisitionEventRequest, not its specialization RequisitionEventRequest); mixing them makes the runtime fail with "Associated object should not have already been retrieved over schema". |
| `attrEventTitle` | attribute |  |  |  | Data::Related information | BudgetItemRequisitionRequestHdr_BudgetItemRequisitionEventRequest / RequestTitle. The row is hidden when empty. Use the same entity on the path as the other Multiple fields (BudgetItemRequisitionEventRequest, not its specialization RequisitionEventRequest); mixing them makes the runtime fail with "Associated object should not have already been retrieved over schema". |
| `attrRootBudgetCode` | attribute |  |  |  | Data::Related information | Request_RootBudgetItem / BudgetItem.ParentBudgetItemCode. |
| `attrEndBudgetCode` | attribute |  |  |  | Data::Related information | Request_EndBudgetItem / BudgetItem.ParentBudgetItemCode. |
| `multipleSource` | datasource |  |  | list | Multiple requisitions::Source | Optional. Multiple requisitions, e.g. microflow MF_ACT_Get_Requisition_Event_Request_List_By_Role with the HelperSearchForm parameter. Each Multiple becomes the header of its department rows (matched on code) with its own actions, and a Multiple the user sees without any department row still shows. Leave empty to group rows by the Multiple requisition code only. |
| `multipleCode` | attribute |  |  |  | Multiple requisitions::Source | Request.Code. Matched with the rows' "Multiple requisition code". Required when the Multiple data source is set. |
| `multipleTitle` | attribute |  |  |  | Multiple requisitions::Source | Request.RequestTitle. |
| `multipleAmount` | attribute |  |  |  | Multiple requisitions::Source | BudgetItemRequisitionEventRequest.Amount. Stored total, never recomputed. |
| `multipleProcessStatus` | attribute |  |  |  | Multiple requisitions::Source | Request.ProcessStatus. Gives a Multiple without visible department rows its status family. |
| `multipleWorkflowStatus` | attribute |  |  |  | Multiple requisitions::Source | Request.WorkflowStatus. |
| `multipleRequestStatus` | attribute |  |  |  | Multiple requisitions::Source | Request.RequestStatus. Shown in the Multiple's details. |
| `multipleTextPic` | textTemplate |  |  |  | Multiple requisitions::People and dates | Text template, e.g. "{1} — {2}" with Request_Data_Owner_Account / Account Name and FullName. |
| `multipleExprPicIsMe` | expression |  |  |  | Multiple requisitions::People and dates | Optional. Adds "(You)" after the PIC. |
| `multipleTextCreatedBy` | textTemplate |  |  |  | Multiple requisitions::People and dates | Text template with the creator's Name and FullName. |
| `multipleExprCreatedByIsMe` | expression |  |  |  | Multiple requisitions::People and dates | Optional. Adds "(You)" after the creator. |
| `multipleStartDate` | attribute |  |  |  | Multiple requisitions::People and dates | Request.StartDate. |
| `multipleEndDate` | attribute |  |  |  | Multiple requisitions::People and dates | Request.EndDate. |
| `multiplePaymentDate` | attribute |  |  |  | Multiple requisitions::People and dates | BudgetItemRequisitionEventRequest.ExpectedPaymentDate. |
| `multipleExtendDate` | attribute |  |  |  | Multiple requisitions::People and dates | BudgetItemRequisitionEventRequest.ExtendDate. |
| `onOpenMultiple` | action |  |  |  | Multiple requisitions::Actions | Runs from the Multiple code link, e.g. show page EventRequisition_Detail. |
| `multipleActions` | widgets |  |  |  | Multiple requisitions::Actions | Rendered on every Multiple header of the current page: View and Edit, with the same button classes as the Single rows. Visibility rules keep working. |
| `multiplePanelActions` | widgets |  |  |  | Multiple requisitions::Actions | Action bar under the Multiple's details, mounted only when the group is opened: Change PIC and Delete. |
| `filterMine` | attribute |  |  |  | Filters::Server filters (context object) | HelperSearchForm.IsMyRequest. The widget writes it and reloads both data sources (owner OR PIC, evaluated server-side). |
| `onOpen` | action |  |  |  | Actions::Row | Runs from the "Open" button and the code link, e.g. show page Requisition_Detail. |
| `showOpenButton` | boolean | Yes | true |  | Actions::Row | Shows "Open →" on each row. Turn it off when the inline actions already hold their own view button; the code link still runs On open. |
| `rowActions` | widgets |  |  |  | Actions::Slots | Rendered on every row of the current page, like the old Actions column. Move the data view MF_SUB_Create_DS_RequestEditPermission with its View, Change PIC, Edit, Delete and Admin Delete buttons here; their visibility rules keep working. |
| `menuActions` | widgets |  |  |  | Actions::Slots | Mounted only when the row menu opens. Place Change PIC, Delete and Admin Delete with their existing visibility rules. |
| `detailExtra` | widgets |  |  |  | Actions::Slots | Mounted only when a row is expanded, e.g. SNIP_Next_Approvers. |
| `panelActions` | widgets |  |  |  | Actions::Slots | Action bar at the bottom of the expanded panel, mounted only when the row is expanded. Place a copy of the permission data view with Change PIC, Delete and Admin Delete here, and keep only View and Edit in Inline actions. |
| `pageSize` | integer | Yes | 20 |  | Display::List | Rows per page. Paging is client-side over the loaded list. |
| `defaultView` | enumeration | Yes | board | `board` \| `cards` | Display::List | Board shows one collapsed row per requisition, like a price board; Cards shows the full card. Users can switch, and the choice is remembered for the browser tab. |
| `emptyMessage` | textTemplate |  |  |  | Display::List | Shown when no row matches the filters. Defaults to "No requisitions match the filters". |
| `showToolbar` | boolean | Yes | true |  | Display::List | Type switch, view switch, search box, department filter and status-family chips with counts above the rows. |
| `perspectiveAccess` | datasource |  |  | list | Display::Perspective | The "View as" switch shows only when this data source returns at least one object; otherwise everyone sees the default perspective. For administrators only, use Database System.UserRole with XPath [System.UserRoles = '[%CurrentUser%]'][Name = 'Administrator' or Name = 'BudgetControl_Administrator']. Presentation only: it never changes actions or permissions. |
| `defaultPerspective` | enumeration | Yes | owner | `manager` \| `owner` \| `admin` | Display::Perspective | Focus line for everyone who cannot switch, and the starting point for those who can. |

## Child Slots (curly-brace blocks)

| MDL keyword | Widget property |
|-------------|----------------|
| `multipleactions` | `multipleActions` |
| `multiplepanelactions` | `multiplePanelActions` |
| `rowactions` | `rowActions` |
| `menuactions` | `menuActions` |
| `detailextra` | `detailExtra` |
| `panelactions` | `panelActions` |

---

Regenerated by `mxcli widget docs` and by `refresh catalog`. For the same data live from the `.mpk` — including anything added by a widget upgrade since this file was written — run `mxcli widget describe vtarequisitionlist -p <app.mpr>`.
