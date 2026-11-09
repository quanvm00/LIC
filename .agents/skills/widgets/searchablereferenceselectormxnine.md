# Searchable Selector

- **Widget ID:** `mendix.searchablereferenceselectormxnine.SearchableReferenceSelectorMxNine`
- **Type:** PLUGGABLEWIDGET
- **Version:** 5.4.1

## MDL Example

```sql
PLUGGABLEWIDGET 'mendix.searchablereferenceselectormxnine.SearchableReferenceSelectorMxNine' widget1 {
  optioncustomcontent slot1 {
    -- widgets for `optionCustomContent`
  }
  referencesetvaluecontent slot2 {
    -- widgets for `referenceSetValueContent`
  }
  searchattribute item1   -- one entry of `searchAttributes`
}
```

## Properties

| Property | Type | Required | Default | Values / notes | Group | Description |
|----------|------|----------|---------|----------------|-------|-------------|
| `placeholder` | textTemplate | Yes |  |  | General::Label | Placeholder |
| `selectionType` | enumeration | Yes | enumeration | `enumeration` \| `boolean` \| `reference` \| `referenceSet` | General::Data source | Selection type |
| `reference` | association | Yes |  |  | General::Data source | Reference |
| `referenceSet` | association | Yes |  |  | General::Data source | Reference set |
| `selectableObjects` | datasource | Yes |  | list | General::Data source | Selectable objects |
| `selectableCondition` | expression | Yes | true |  | General::Data source | Option selectable? |
| `enumAttribute` | attribute | Yes |  |  | General::Data source | Enum attribute |
| `enumFilterType` | enumeration | Yes | OFF | `OFF` \| `INCLUDE` \| `EXCLUDE` | General::Data source | Enum filter type |
| `enumFilterList` | expression | Yes |  |  | General::Data source | Comma seperated list of the Enum's keys to include/exclude them |
| `booleanAttribute` | attribute | Yes |  |  | General::Data source | Boolean attribute |
| `trueLabel` | textTemplate | Yes |  |  | General::Data source | True label |
| `falseLabel` | textTemplate | Yes |  |  | General::Data source | False label |
| `optionTextType` | enumeration | Yes | text | `text` \| `html` \| `expression` \| `custom` | General::Data source | Option content type |
| `displayAttribute` | attribute | Yes |  |  | General::Data source | Option content |
| `optionExpression` | expression | Yes |  |  | General::Data source | Option content |
| `optionCustomContent` | widgets | Yes |  |  | General::Data source | Option content |
| `isSearchable` | boolean | Yes | true |  | General::Searching | Searchable? |
| `filterMode` | enumeration | Yes | SERVER | `SERVER` \| `CLIENT` \| `OFF` | General::Searching | Search mode |
| `filterDelay` | integer | Yes | 500 |  | General::Searching | Search delay (ms) |
| `filterType` | enumeration | Yes | auto | `auto` \| `manual` | General::Searching | Search type |
| `filterFunction` | enumeration | Yes | contains | `contains` \| `startsWith` | General::Searching | Search function |
| `searchAttributes` | object |  |  | list; 1 sub-properties below | General::Searching | Search attributes |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `searchAttribute` | attribute | Yes |  |  |  | Search attribute |
| `searchText` | attribute | Yes |  |  | General::Searching | Search text |
| `hasMoreResultsManual` | expression | Yes |  |  | General::Searching | Expression to determine when to show the More Results Text (Easiest to include a boolean on your Search Text's object) |
| `onClickMoreResultsText` | action |  |  |  | General::Searching | Mx Action that can be used to increase the page size on your data source when there are more results |
| `selectStyle` | enumeration | Yes | dropdown | `dropdown` \| `list` | Style::General | Select style |
| `loadDataMode` | enumeration | Yes | INITIAL | `INITIAL` \| `OPEN` | Style::General | Load data on |
| `optionsStyleSingle` | enumeration | Yes | cell | `cell` \| `radio` | Style::General | Option style |
| `optionsStyleSet` | enumeration | Yes | cell | `cell` \| `checkbox` | Style::General | Option style |
| `maxItems` | expression | Yes | 20 |  | Style::General | Limits the items shown. If 0, no limit is set. |
| `allowLoadingSelect` | boolean | Yes | true |  | Style::General | Allow the user to select an option while the data source is loading? (i.e. time between typing and the data source returning results) |
| `maxMenuHeight` | expression |  |  |  | Style::General | Custom CSS Height for the select menu (default 15em) |
| `dropdownIcon` | icon |  |  |  | Style::General | Dropdown icon |
| `isCompact` | boolean | Yes | true |  | Style::Reference set options | Displays the badges and comma's in-line with the search input |
| `clearSearchOnSelect` | boolean | Yes | true |  | Style::Reference set options | Should the search text be cleared when the user selects an option? |
| `referenceSetStyle` | enumeration | Yes | badges | `badges` \| `commas` | Style::Reference set options | How the selected items are displayed |
| `referenceSetValue` | enumeration | Yes | SAME | `SAME` \| `CUSTOM` | Style::Reference set options | The text displayed in the badge or comma |
| `badgeColor` | enumeration | Yes | default | `default` \| `info` \| `primary` \| `inverse` \| `success` \| `warning` \| `danger` | Style::Reference set options | Badge color |
| `referenceSetValueContent` | widgets | Yes |  |  | Style::Reference set options | The value displayed in the badge or comma |
| `maxReferenceDisplay` | integer | Yes | 0 |  | Style::Reference set options | How many many badges / items are displayed (0 = no limit) ~ remainder is displayed as "(+n)" |
| `moreResultsText` | textTemplate | Yes |  |  | Customization::Text | Text shown if the there are more results than what is being displayed. Clicking on the text will show more results. |
| `noResultsText` | textTemplate | Yes |  |  | Customization::Text | Message shown if no results were found |
| `loadingText` | textTemplate | Yes |  |  | Customization::Text | Text shown in the dropdown while waiting for the data source |
| `autoFocusMode` | enumeration | Yes | OFF | `OFF` \| `FOCUS_SELECTED` \| `FOCUS_OPTION` | Customization::Auto Focus | Auto focus mode |
| `autoFocusOption_Enum` | expression | Yes |  |  | Customization::Auto Focus | Finds the first Enum key that matches this text and auto focuses it |
| `autoFocusOption_Obj` | expression | Yes |  |  | Customization::Auto Focus | Auto focus the first option that makes this expression true |
| `showSelectAll` | boolean | Yes | true |  | Customization::Select all | Should the textbox have an icon to select all options? |
| `selectAllIconTitle` | textTemplate | Yes |  |  | Customization::Select all | Text shown when hovering on the select all icon |
| `selectAllIcon` | icon |  |  |  | Customization::Select all | Select all icon |
| `isClearable` | boolean | Yes | true |  | Customization::Clearing | Should the textbox have an icon to clear the search text or current value? |
| `clearIconTitle` | textTemplate | Yes |  |  | Customization::Clearing | Text shown when hovering on the clear icon |
| `clearAllIconTitle` | textTemplate | Yes |  |  | Customization::Clearing | Text shown when hovering on the clear all icon |
| `clearIcon` | icon |  |  |  | Customization::Clearing | Clear icon |
| `clearAllIcon` | icon |  |  |  | Customization::Clearing | Clear all icon |
| `onChange` | action |  |  |  | Events::Events | On change |
| `onEnter` | action |  |  |  | Events::Events | On enter |
| `onLeave` | action |  |  |  | Events::Events | On leave |
| `onBadgeClick` | action |  |  |  | Events::Events | Action called when the user clicks on one of the currently selected values |
| `onExtraClick` | action |  |  |  | Events::Events | Action called when the user clicks on the (+n) text |
| `ariaRequired` | expression | Yes | false |  | Accessibility::Accessibility | The screen reader will announce this field is required |
| `ariaLabel` | textTemplate |  |  |  | Accessibility::Accessibility | The text read by a screen reader in place of a label on the screen |
| `ariaLiveText` | textTemplate |  |  |  | Accessibility::Accessibility | The text read by a screen reader while focused on an option |
| `ariaSelectedText` | textTemplate |  |  |  | Accessibility::Accessibility | The text read by a screen reader when annoucing the selected value(s) |
| `ariaSearchText` | textTemplate |  |  |  | Accessibility::Accessibility | The text read by a screen reader when announcing the search text |
| `valueAriaLabel` | textTemplate |  |  |  | Accessibility::Accessibility | Aria label for the text of the current value(s) |
| `extraAriaLabel` | textTemplate |  |  |  | Accessibility::Accessibility | Aria label for the extra text |
| `ariaArrowKeyInstructions` | textTemplate |  |  |  | Accessibility::Instructions | The text read aloud by a screen reader when there is already a value selected |

## Child Slots (curly-brace blocks)

| MDL keyword | Widget property |
|-------------|----------------|
| `optioncustomcontent` | `optionCustomContent` |
| `referencesetvaluecontent` | `referenceSetValueContent` |

## Object Lists (repeating child entries)

### `searchattribute` → property `searchAttributes`

Item properties:

| Property | Operation |
|----------|-----------|
| `searchAttribute` | attribute |

---

Regenerated by `mxcli widget docs` and by `refresh catalog`. For the same data live from the `.mpk` — including anything added by a widget upgrade since this file was written — run `mxcli widget describe searchablereferenceselectormxnine -p <app.mpr>`.
