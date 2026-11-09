# VTA Work Dashboard

- **Widget ID:** `vta.vtaworkdashboard.VtaWorkDashboard`
- **Type:** PLUGGABLEWIDGET
- **Version:** 0.1.0

## MDL Example

```sql
PLUGGABLEWIDGET 'vta.vtaworkdashboard.VtaWorkDashboard' widget1 {
  headeractions slot1 {
    -- widgets for `headerActions`
  }
}
```

## Properties

| Property | Type | Required | Default | Values / notes | Group | Description |
|----------|------|----------|---------|----------------|-------|-------------|
| `ctxGeneratedAt` | attribute | Yes |  |  | Summary::Context | MyWorkContext.GeneratedAt, used as "now". |
| `ctxSlaDays` | attribute | Yes |  |  | Summary::Context | MyWorkContext.SlaDays. |
| `ctxLoadLimit` | attribute | Yes |  |  | Summary::Context | MyWorkContext.LoadLimit. |
| `ctxWindowDays` | attribute |  |  |  | Summary::Context | MyWorkContext.WindowDays. |
| `ctxPersonText` | attribute |  |  |  | Summary::Context | MyWorkContext.PersonText. |
| `ctxOrphanTaskCount` | attribute |  |  |  | Summary::Context | MyWorkContext.OrphanTaskCount (admins only). |
| `ctxDone7d` | attribute |  |  |  | Summary::Context | MyWorkContext.Done7d. |
| `ctxDonePrev7d` | attribute |  |  |  | Summary::Context | MyWorkContext.DonePrev7d. |
| `ctxApproved` | attribute |  |  |  | Summary::Context | MyWorkContext.ApprovedCount. |
| `ctxReturned` | attribute |  |  |  | Summary::Context | MyWorkContext.ReturnedCount. |
| `ctxOtherOutcome` | attribute |  |  |  | Summary::Context | MyWorkContext.OtherOutcomeCount. |
| `ctxMedianHours` | attribute |  |  |  | Summary::Context | MyWorkContext.MedianHandlingHours. |
| `ctxMineWaitingPeople` | attribute |  |  |  | Summary::Context | MyWorkContext.MineWaitingPeople. |
| `headerActions` | widgets |  |  |  | Summary::Header | Create Single / Create Multiple buttons. |
| `onRefresh` | action |  |  |  | Summary::Header | ACT_MyWork_Refresh. |
| `showGreeting` | boolean | Yes | true |  | Summary::Display | Show "Hello, ..." above the updated line. Turn it off when the page already has a welcome bar. |
| `queueLimit` | integer | Yes | 8 |  | Summary::Display | Rows shown before "Show all". |
| `paymentDueDays` | integer | Yes | 7 |  | Summary::Display | Approved requisitions paid within this many days, today included. |
| `items` | datasource | Yes |  | list | Queue::Source | DS_MyWork_Items with the MyWorkContext parameter. |
| `onOpenTask` | action |  |  |  | Queue::Source | ACT_MyWork_OpenTask. |
| `onOpenRequest` | action |  |  |  | Queue::Source | ACT_MyWork_OpenRequest; shown only when Has request. |
| `itemTaskName` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.TaskName. |
| `itemKind` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.Kind. |
| `itemCode` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.Code. |
| `itemTitle` | attribute |  |  |  | Queue::Fields | MyWorkItem.Title. |
| `itemDepartment` | attribute |  |  |  | Queue::Fields | MyWorkItem.Department. |
| `itemRequester` | attribute |  |  |  | Queue::Fields | MyWorkItem.RequesterText. |
| `itemEventCode` | attribute |  |  |  | Queue::Fields | MyWorkItem.EventCode. |
| `itemObjectKey` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.ObjectKey. |
| `itemAmount` | attribute |  |  |  | Queue::Fields | MyWorkItem.Amount. |
| `itemStartedAt` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.StartedAt. |
| `itemTargetCount` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.TargetCount. |
| `itemAssignedToMe` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.AssignedToMe. |
| `itemContextClosed` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.ContextClosed. |
| `itemHasRequest` | attribute | Yes |  |  | Queue::Fields | MyWorkItem.HasRequest. |
| `mine` | datasource |  |  | list | Mine::Source | DS_MyWork_Mine with the MyWorkContext parameter. |
| `onOpenMine` | action |  |  |  | Mine::Source | ACT_MyWork_OpenMine. |
| `mineCode` | attribute |  |  |  | Mine::Fields | MyWorkMine.Code. |
| `mineTitle` | attribute |  |  |  | Mine::Fields | MyWorkMine.Title. |
| `mineAmount` | attribute |  |  |  | Mine::Fields | MyWorkMine.Amount. |
| `mineProcessStatus` | attribute |  |  |  | Mine::Fields | MyWorkMine.ProcessStatus. |
| `mineWorkflowStatus` | attribute |  |  |  | Mine::Fields | MyWorkMine.WorkflowStatus. |
| `mineWaitingOn` | attribute |  |  |  | Mine::Fields | MyWorkMine.WaitingOnText. |
| `mineWaitingOnCount` | attribute |  |  |  | Mine::Fields | MyWorkMine.WaitingOnCount. |
| `mineWaitingSince` | attribute |  |  |  | Mine::Fields | MyWorkMine.WaitingSince. |
| `minePaymentDate` | attribute |  |  |  | Mine::Fields | MyWorkMine.PaymentDate. |
| `mineChangedAt` | attribute |  |  |  | Mine::Fields | MyWorkMine.ChangedAt. |
| `weeks` | datasource |  |  | list | Weeks::Source | DS_MyWork_Weeks with the MyWorkContext parameter. |
| `weekStart` | attribute |  |  |  | Weeks::Fields | MyWorkWeek.WeekStart. |
| `weekIsCurrent` | attribute |  |  |  | Weeks::Fields | MyWorkWeek.IsCurrent. |
| `weekIncoming` | attribute |  |  |  | Weeks::Fields | MyWorkWeek.Incoming. |
| `weekDone` | attribute |  |  |  | Weeks::Fields | MyWorkWeek.Done. |
| `weekApproved` | attribute |  |  |  | Weeks::Fields | MyWorkWeek.Approved. |
| `weekReturned` | attribute |  |  |  | Weeks::Fields | MyWorkWeek.Returned. |
| `weekClosedByOthers` | attribute |  |  |  | Weeks::Fields | MyWorkWeek.ClosedByOthers. |
| `team` | datasource |  |  | list | Team::Source | DS_MyWork_Team with the MyWorkContext parameter. Empty for people without a team. |
| `teamPerson` | attribute |  |  |  | Team::Fields | MyWorkTeamMember.PersonText. |
| `teamOpen` | attribute |  |  |  | Team::Fields | MyWorkTeamMember.OpenCount. |
| `teamPastSla` | attribute |  |  |  | Team::Fields | MyWorkTeamMember.PastSlaCount. |
| `teamOldestHours` | attribute |  |  |  | Team::Fields | MyWorkTeamMember.OldestHours. |
| `teamDone7d` | attribute |  |  |  | Team::Fields | MyWorkTeamMember.Done7d. |

## Child Slots (curly-brace blocks)

| MDL keyword | Widget property |
|-------------|----------------|
| `headeractions` | `headerActions` |

---

Regenerated by `mxcli widget docs` and by `refresh catalog`. For the same data live from the `.mpk` — including anything added by a widget upgrade since this file was written — run `mxcli widget describe vtaworkdashboard -p <app.mpr>`.
