# Tabulator Data Grid

- **Widget ID:** `conventsystems.tabulatordatagrid.TabulatorDatagrid`
- **Type:** PLUGGABLEWIDGET
- **Version:** 1.0.0

## MDL Example

```sql
PLUGGABLEWIDGET 'conventsystems.tabulatordatagrid.TabulatorDatagrid' widget1 {
  toolbarwidgets slot1 {
    -- widgets for `toolbarWidgets`
  }
  detailcontent slot2 {
    -- widgets for `detailContent`
  }
  column item1   -- one entry of `columns`
  spreadsheetsheet item2   -- one entry of `spreadsheetSheets`
}
```

## Properties

| Property | Type | Required | Default | Values / notes | Group | Description |
|----------|------|----------|---------|----------------|-------|-------------|
| `enableSpreadsheet` | boolean | Yes | false |  | General::Spreadsheet mode | Show an embedded spreadsheet instead of the data grid. When enabled, Data source and Columns are not used. |
| `datasource` | datasource |  |  | list | General::Data source | Mendix object list to display in the grid |
| `columns` | object |  |  | list; 23 sub-properties below | General::Columns | Define one column per attribute or custom content |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `showContentAs` | enumeration | Yes | attribute | `attribute` \| `dynamicText` \| `customContent` |  | How to display data in this column |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `attribute` | attribute |  |  |  |  | The attribute to show in this column |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `dynamicText` | textTemplate |  |  |  |  | Text template per row (active when Show = Dynamic text) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `content` | widgets |  |  |  |  | Widgets rendered per row (active when Show = Custom content) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `width` | integer | Yes | 150 |  |  | Column width in pixels (0 = auto) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `minWidth` | integer | Yes | 50 |  |  | Minimum column width in pixels |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `header` | textTemplate |  |  |  |  | Column header text. Auto-filled with the attribute name when an attribute is selected. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `sortable` | boolean | Yes | true |  |  | Allow sorting by clicking the column header |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `headerFilter` | boolean | Yes | false |  |  | Show a filter input below the column header |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `editable` | boolean | Yes | false |  |  | Allow the user to edit this cell value inline. The attribute must be writable in Mendix. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `onCellEditAction` | action |  |  |  |  | Action executed in the context of the row after the user changes a cell value |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `resizable` | boolean | Yes | true |  |  | Allow the user to resize this column |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `freeze` | enumeration | Yes | none | `none` \| `left` |  | Pin this column to the left edge of the grid |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `hozAlign` | enumeration | Yes | auto | `auto` \| `left` \| `center` \| `right` |  | Horizontal alignment (auto = right for numbers, center for boolean, left otherwise) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `cellClass` | expression |  |  |  |  | Expression returning a CSS class name per row, e.g. if $currentObject/Priority = 'High' then 'tab-danger' else ''. Built-in: tab-danger, tab-warning, tab-success, tab-info, tab-muted. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `formatter` | enumeration | Yes | value | `value` \| `progressBar` \| `star` \| `tickCross` \| `money` \| `html` \| `color` \| `link` \| `toggle` \| `textarea` \| `image` \| `datetimediff` \| `traffic` \| `buttonTick` \| `buttonCross` |  | How to render the cell value. Value = plain text; ProgressBar = 0–100 bar; Star = star rating (0–5); TickCross = checkbox; Money = currency; HTML = raw HTML; Color = color swatch; Link = hyperlink; Toggle = on/off toggle; Textarea = multi-line text; Image = img tag; DateTimeDiff = time-ago; Traffic = traffic light; ButtonTick / ButtonCross = action buttons. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `decimalPlaces` | integer | Yes | -1 |  |  | Fixed decimal places for numeric values (-1 = use Mendix default) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `currencySymbol` | string |  |  |  |  | Prefix symbol, e.g. €, $, £ (applied when Decimal places ≥ 0 or Formatter = Money) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `dateFormat` | string |  |  |  |  | Pattern, e.g. dd-MM-yyyy \| dd-MM-yyyy HH:mm (empty = Mendix default) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `aggregateFunction` | enumeration | Yes | none | `none` \| `sum` \| `count` \| `average` \| `max` \| `min` |  | Show an aggregate in the summary row. Sum/Average/Max/Min require a numeric attribute column. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `groupBy` | boolean | Yes | false |  |  | Include this column in the Group-by toolbar. Multiple columns create nested groups. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `xpathName` | string |  |  |  |  | Attribute name, e.g. Amount or Description. Required for: (1) XPath filter writing, (2) reliable inline cell editing — the widget uses this name to write the new value directly to the Mendix object so the nanoflow receives the updated value. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `tooltip` | boolean | Yes | false |  |  | Show the cell value as a tooltip on hover |
| `itemSelection` | selection | Yes |  |  | Row Selection | None = no selection. Single = one row at a time. Multi = multiple rows via checkboxes. |
| `enableSelectionColumn` | boolean | Yes | true |  | Row Selection | Show a checkbox column for multi-selection mode |
| `autoSelectRow` | integer | Yes | 0 |  | Row Selection | Automatically select this row when the grid loads. 0 = no auto-selection, 1 = first row, 2 = second row, etc. |
| `onRowSelectAction` | action |  |  |  | Row Selection | Fires in the context of a row when it is added to the selection |
| `onRowDeselectAction` | action |  |  |  | Row Selection | Fires in the context of a row when it is removed from the selection |
| `enableCsvExport` | boolean | Yes | false |  | Export | Show an Export CSV button in the toolbar |
| `enableExcelExport` | boolean | Yes | false |  | Export | Show an Export Excel button (downloads .xlsx) |
| `enableJsonExport` | boolean | Yes | false |  | Export | Show an Export JSON button |
| `enablePdfExport` | boolean | Yes | false |  | Export | Show a Print/PDF button (uses browser print dialog) |
| `enableClipboardCopy` | boolean | Yes | false |  | Export | Show a Copy button that copies the selected (or all) rows to clipboard |
| `exportFilename` | string |  | export |  | Export | Filename for downloaded files (without extension) |
| `toolbarWidgets` | widgets |  |  |  | Toolbar | Additional widgets shown in the toolbar (e.g. import button) |
| `onRowClickAction` | action |  |  |  | Behavior | Action executed when the user clicks a row |
| `onRowDblClickAction` | action |  |  |  | Behavior | Action executed when the user double-clicks a row |
| `emptyPlaceholder` | string |  | No data |  | Behavior | Text shown when the data source returns no rows |
| `enableRowReorder` | boolean | Yes | false |  | Behavior | Show a drag handle so the user can reorder rows |
| `onRowReorderAction` | action |  |  |  | Behavior | Action executed in the context of the moved row after it is dropped |
| `reorderTargetIdAttribute` | attribute |  |  |  | Behavior | String attribute that receives the ID of the row now below the moved row (empty = moved to last position) |
| `enableMasterDetail` | boolean | Yes | false |  | Master / Detail | Show an expand toggle on each row to reveal a detail panel |
| `detailContent` | widgets |  |  |  | Master / Detail | Widgets rendered in the collapsible detail panel below each row |
| `detailHeight` | integer | Yes | 200 |  | Master / Detail | Height of the expanded detail panel in pixels |
| `configAttribute` | attribute |  |  |  | Personalization | Unlimited String attribute to persist sort, filter and column order. Surround the widget with a Data view to select an attribute. |
| `onConfigChangeAction` | action |  |  |  | Personalization | Action executed after the configuration attribute is updated (e.g. to commit the object) |
| `filterXpathAttribute` | attribute |  |  |  | Personalization | String attribute where the widget writes the current filter as an XPath constraint. Requires XPath attribute name per column. |
| `enableFilters` | boolean | Yes | false |  | Filtering | Show filter inputs in column headers (must also enable Header filter per column) |
| `enableFilterBar` | boolean | Yes | false |  | Filtering | Show a filter bar above the grid with column, comparison type and value selectors (setFilter API) |
| `summaryPosition` | enumeration | Yes | none | `none` \| `top` \| `bottom` | Summary row | Show a row with aggregate values (sum, count, average, max, min) above or below the data |
| `enableGroupBy` | boolean | Yes | false |  | Appearance | Show a Group by panel. Columns with Group by enabled can be toggled as grouping levels. |
| `enableColumnChooser` | boolean | Yes | false |  | Appearance | Show a Columns button that lets users show or hide individual columns |
| `enableColumnReorder` | boolean | Yes | false |  | Appearance | Allow users to drag column headers to reorder columns |
| `rowStriping` | boolean | Yes | false |  | Appearance | Alternate row background color for even rows |
| `rowHeight` | integer | Yes | 35 |  | Appearance | Height of each data row in pixels |
| `gridHeight` | integer | Yes | 400 |  | Appearance | Total height of the grid in pixels |
| `layout` | enumeration | Yes | fitData | `fitData` \| `fitColumns` \| `fitDataFill` \| `fitDataStretch` | Appearance | fitData = each column is its configured width; fitColumns = stretch to fill grid width; fitDataFill = like fitData but fills remaining space; fitDataStretch = like fitData but last column stretches. |
| `enableResponsiveLayout` | boolean | Yes | false |  | Appearance | Automatically hide columns when the grid is too narrow. Columns are hidden right-to-left. |
| `enablePagination` | boolean | Yes | false |  | Pagination | Split data into pages with navigation controls |
| `pageSize` | integer | Yes | 20 |  | Pagination | Number of rows per page |
| `paginationPosition` | enumeration | Yes | bottom | `top` \| `bottom` | Pagination | Show pagination controls above or below the grid |
| `paginationSizeSelector` | boolean | Yes | false |  | Pagination | Allow users to change the number of rows per page |
| `spreadsheetSource` | enumeration | Yes | attribute | `attribute` \| `datasource` | Spreadsheet | JSON attribute = load and save sheet data as JSON in the Data attribute. Data source = fill the sheet from the Data source and Columns (General tab); edits are not written back to the objects, but are still saved to the Data attribute as JSON when one is set. Data source mode uses a single sheet. |
| `spreadsheetHeaderRow` | boolean | Yes | true |  | Spreadsheet | Data source mode: put the column headers in the first sheet row |
| `spreadsheetRows` | integer | Yes | 50 |  | Spreadsheet | Number of rows in the spreadsheet grid |
| `spreadsheetColumns` | integer | Yes | 26 |  | Spreadsheet | Number of columns in the spreadsheet grid |
| `spreadsheetDataAttribute` | attribute |  |  |  | Spreadsheet | Unlimited String attribute where spreadsheet data is stored and loaded as JSON. Wrap the widget in a Data view to select an attribute. |
| `onSpreadsheetChangeAction` | action |  |  |  | Spreadsheet | Action executed after each cell edit (e.g. to commit the object) |
| `spreadsheetSheetTabs` | boolean | Yes | false |  | Spreadsheet | Show tabs at the bottom to switch between sheets (requires Sheets to be configured) |
| `spreadsheetSheets` | object |  |  | list; 4 sub-properties below | Spreadsheet | Define multiple sheets. When empty a single unnamed sheet is used. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `sheetTitle` | string | Yes | Sheet |  |  | Display name shown in the sheet tab |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `sheetKey` | string | Yes |  |  |  | Unique identifier for this sheet (used as key in the stored JSON) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `sheetRows` | integer | Yes | 0 |  |  | Number of rows for this sheet (0 = use global Rows setting) |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `sheetColumns` | integer | Yes | 0 |  |  | Number of columns for this sheet (0 = use global Columns setting) |
| `spreadsheetClipboard` | boolean | Yes | true |  | Spreadsheet | Allow Ctrl+C / Ctrl+V to copy and paste cell ranges |

## Child Slots (curly-brace blocks)

| MDL keyword | Widget property |
|-------------|----------------|
| `toolbarwidgets` | `toolbarWidgets` |
| `detailcontent` | `detailContent` |

## Object Lists (repeating child entries)

### `column` → property `columns`

Item properties:

| Property | Operation |
|----------|-----------|
| `showContentAs` | primitive |
| `attribute` | attribute |
| `dynamicText` | texttemplate |
| `width` | primitive |
| `minWidth` | primitive |
| `header` | texttemplate |
| `sortable` | primitive |
| `headerFilter` | primitive |
| `editable` | primitive |
| `onCellEditAction` | action |
| `resizable` | primitive |
| `freeze` | primitive |
| `hozAlign` | primitive |
| `cellClass` | expression |
| `formatter` | primitive |
| `decimalPlaces` | primitive |
| `currencySymbol` | primitive |
| `dateFormat` | primitive |
| `aggregateFunction` | primitive |
| `groupBy` | primitive |
| `xpathName` | primitive |
| `tooltip` | primitive |

Item child slots:

| MDL keyword | Widget property |
|-------------|----------------|
| `content` | `content` |

### `spreadsheetsheet` → property `spreadsheetSheets`

Item properties:

| Property | Operation |
|----------|-----------|
| `sheetTitle` | primitive |
| `sheetKey` | primitive |
| `sheetRows` | primitive |
| `sheetColumns` | primitive |

---

Regenerated by `mxcli widget docs` and by `refresh catalog`. For the same data live from the `.mpk` — including anything added by a widget upgrade since this file was written — run `mxcli widget describe tabulatordatagrid -p <app.mpr>`.
