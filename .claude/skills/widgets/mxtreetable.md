# TreeTable

- **Widget ID:** `com.mendix.widget.custom.mxtreetable.MxTreeTable`
- **Type:** CUSTOMWIDGET
- **Version:** 2.3.1

## MDL Example

```sql
CUSTOMWIDGET 'com.mendix.widget.custom.mxtreetable.MxTreeTable' widget1 {
  columnlist item1   -- one entry of `columnList`
  inlineactionbutton item2   -- one entry of `inlineActionButtons`
  selectactionbutton item3   -- one entry of `selectActionButtons`
}
```

## Properties

| Property | Type | Required | Default | Values / notes | Group | Description |
|----------|------|----------|---------|----------------|-------|-------------|
| `nodeEntity` | entity | Yes |  |  | Data::General | Entity for the Node that is displayed in the widget. |
| `loadScenario` | enumeration | Yes | partial | `partial` \| `all` | Data::Scenario | Decide what the widget needs to load when it has a context object. - In case of "Whole Tree", make sure you set the "Root" attribute (see bottom) and Child reference (see Data Children) |
| `dataSource` | enumeration | Yes | xpath | `xpath` \| `mf` \| `nf` | Data::Source | Choose how you retrieve the data. In case Scenario is set to "Partial" this only applies to top-level data. |
| `constraint` | entityConstraint |  |  |  | Data::Source | The xpath constraint on the entity. |
| `getDataMf` | microflow |  |  |  | Data::Source | Choose 'Microflow' in Data Source and choose a microflow here |
| `getDataNf` | nanoflow |  |  |  | Data::Source | Choose 'Nanoflow' in Data Source and choose a nanoflow here. NOTE: Non-persistent entities are discouraged as they have unpredictable behavior combined with Nanoflows. |
| `nodeIsRootAttr` | attribute |  |  |  | Data::Scenario: Whole Tree | When loading the whole tree, the widget needs to know which nodes are considered top-level nodes |
| `dataResetOnContextChange` | boolean | Yes | false |  | Data::Reset | This should be false by default, because you only want to reload the whole table when the context object is switched (new object guid). If for some reason you need to reset the table whenever the context object changes, set this to true. Use this combined with the 'Whole Tree' scenario and state management. |
| `childMethod` | enumeration | Yes | disabled | `disabled` \| `reference` \| `microflow` \| `nanoflow` | Data Children | Method how to get the child elements |
| `childReference` | entity |  |  |  | Data Children | Set method: 'Reference'. Scenario 1: Child reference entity. Choose this and it will get the data over reference, through a normal mx.data.get call. This needs a complete tree-structure. - Note: This reference is needed whatsoever when loading a whole tree (see Data). |
| `childBoolean` | attribute |  |  |  | Data Children | Set method: 'Microflow/Nanoflow'. Scenario 2/3: Has child attribute. Choose this if you do not have a complete tree-structure available. This will mean you will need to get the data through the getChildren microflow/nanoflow. Make sure the 'Child reference' attribute is NOT set. |
| `getChildMf` | microflow |  |  |  | Data Children | Set method: 'Microflow'. Scenario 2: Get children through a microflow. Make sure you set the 'Has child attr'. If it detects it has children, it will get the children using this microflow. |
| `getChildNf` | nanoflow |  |  |  | Data Children | Set method: 'Nanoflow'. Scenario 3: Get children through a nanoflow. Make sure you set the 'Has child attr'. If it detects it has children, it will get the children using this nanoflow. NOTE: Non-persistent entities are discouraged as they have unpredictable behavior combined with Nanoflows. |
| `helperEntity` | entity |  |  |  | Helper | This helper entity is necessary when doing selections/clicks. The reason for that is that when doing a onClick or Selection, you want to know both the Context (your view) as well as the Node that was clicked/selected. This can only be accomplished by introducing a Helper object. It should be a non-persistent entity (persistent is not permitted) with a reference to your context object, and a reference SET to your node objects. The widget will create the references for you when you click on a Node or on a button when doing selections. If you use this for an onClick/Single Selection, the reference set will contain only 1 Node object |
| `helperContextReference` | entity |  |  |  | Helper | Reference to context |
| `helperNodeReference` | entity |  |  |  | Helper | Reference to Node Entity |
| `columnMethod` | enumeration | Yes | static | `static` \| `microflow` | Columns::General | How you define the columns. - You will need to take care of your own sorting when getting columns dynamically. |
| `columnList` | object |  |  | list; 6 sub-properties below | Columns::List | Define the columns in the Tree Table |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `columnHeader` | string |  |  |  |  | Title that is above the column |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `columnTitleType` | enumeration | Yes | attr | `attr` \| `nanoflow` |  | Choose between attribute or nanoflow. - Note: When loading the whole tree (See Data), it is strongly discouraged to use a Nanoflow, because it slows down rendering when loading a lot of objects. Use the Nanoflow very sparsely. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `columnAttr` | attribute |  |  |  |  | Attribute that holds the value |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `transformNanoflow` | nanoflow |  |  |  |  | Use this nanoflow to retrieve a string as the value. See note above. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `columnWidth` | string |  |  |  |  | Set this to either a number (like '100' for 100px) or a string ('50%') |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `columnClassName` | string |  |  |  |  | Set a classname for this column. This is preferred over setting a certain width |
| `columnHeaderEntity` | entity |  |  |  | Columns::Dynamic | Used when you set the column definition (See Column) to Microflow/Nanoflow |
| `columnHeaderLabelAttribute` | attribute |  |  |  | Columns::Dynamic | Attribute that holds the label |
| `columnHeaderAttrAttribute` | attribute |  |  |  | Columns::Dynamic | Attribute that holds the attribute name |
| `columnHeaderClassAttribute` | attribute |  |  |  | Columns::Dynamic | Attribute that holds the class name |
| `columnHeaderMicroflow` | microflow |  |  |  | Columns::Dynamic | Choose 'Microflow' in 'Definition' and choose a microflow here |
| `inlineActionButtons` | object |  |  | list; 9 sub-properties below | Columns::Inline buttons | These are inline action buttons that are appended to every row. These will be added at the end of the row as separate columns. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonClass` | string |  | btn actionButton |  |  | Use this to style your buttons. This could be usefull when you have a button without a label |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButttonLabel` | string |  |  |  |  | Use this as a label for your individual button |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonColumnLabel` | string |  |  |  |  | When you add columns, you can optionally set a label for your column |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonColumnClass` | string |  |  |  |  | When you add columns, you can optionally set a class for your column |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonOnClickAction` | enumeration | Yes | mf | `mf` \| `nf` \| `open` |  | Note: For pages it uses the Node object, for microflow/nanoflow the Helper object! Make sure you have configured this. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonOnClickMf` | microflow |  |  |  |  | Button click Microflow |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonOnClickNf` | nanoflow |  |  |  |  | Button click Nanoflow |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonOnClickForm` | form |  |  |  |  | The page to open when a row is clicked |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `actionButtonOnClickOpenPageAs` | enumeration | Yes | content | `content` \| `popup` \| `modal` |  | Button click open page as |
| `onClickAction` | enumeration | Yes | nothing | `nothing` \| `mf` \| `nf` \| `open` | Events::Single click | Note: For pages it uses the Node object, for microflow/nanoflow the Helper object! Make sure you have configured this. |
| `onClickMf` | microflow |  |  |  | Events::Single click | Single click Microflow |
| `onClickNf` | nanoflow |  |  |  | Events::Single click | Single click Nanoflow |
| `onClickForm` | form |  |  |  | Events::Single click | The page to open when a row is clicked |
| `onClickOpenPageAs` | enumeration | Yes | content | `content` \| `popup` \| `modal` | Events::Single click | Single click open page as |
| `onDblClickAction` | enumeration | Yes | nothing | `nothing` \| `mf` \| `nf` \| `open` | Events::Double click | Note: For pages it uses the Node object, for microflow/nanoflow the Helper object! Make sure you have configured this. |
| `onDblClickMf` | microflow |  |  |  | Events::Double click | Double click Microflow |
| `onDblClickNf` | nanoflow |  |  |  | Events::Double click | Double click Nanoflow |
| `onDblClickForm` | form |  |  |  | Events::Double click | The page to open when a row is clicked |
| `onDblClickOpenPageAs` | enumeration | Yes | content | `content` \| `popup` \| `modal` | Events::Double click | Double click open page as |
| `uiShowHeader` | boolean | Yes | true |  | UI::General | Make header row visible/invisible |
| `uiRowClassAttr` | attribute |  |  |  | UI::General | Attribute that will set an extra class on a row. This attribute should contain a valid css className, like 'mx-treeview-row'. This className can also be used in styling, where you would define a couple of classNames to set certain statuses and give each a color. This can be a calculated attribute, but keep in mind that performance might go down if you use that too much. |
| `uiRowIconAttr` | attribute |  |  |  | UI::General | Attribute that will determine which icon to show in the first column of the row. Can be prefixed (see next) |
| `uiIconPrefix` | string |  | glyphicon glyphicon- |  | UI::General | When using icons in a row, it will use an <i> element with this className. When empty, it will use the standard 'glyphicon glyphicon-' prefix to use Bootstrap style elements. You can set your own icon prefixes here. |
| `uiExpandFirstLevelWholeTree` | boolean | Yes | false |  | UI::Scenario: Whole Tree | Only works while loading the whole tree: Automatically expand the first level of the tree. Currently it is only possible to automatically expand the first level. This will be ignored if the user has already used state management before. |
| `uiRenderExpandButton` | boolean | Yes | false |  | UI::Scenario: Whole Tree | Render a button to expand/collapse all |
| `selectMode` | enumeration | Yes | none | `none` \| `single` \| `multi` | Selection::Selection behavior | - Make sure you add the Selection Helper entity when doing selections! - Selection is only enabled when you have added something in the Button Bar or added an On Change Action. If neither is configured, selection will be disabled. |
| `selectClickSelect` | boolean | Yes | true |  | Selection::Selection behavior | If enabled, you can select a row by simply clicking on it. If disabled, you will need to click the checkbox. |
| `selectHideCheckboxes` | boolean | Yes | false |  | Selection::Selection behavior | Hides the left column containing the checkboxes. |
| `selectSelectFirstOnSingle` | boolean | Yes | false |  | Selection::Selection behavior | If enabled, it will automatically select the first row on load. Note: this might not always work when the columns change. |
| `selectMultiChildren` | boolean | Yes | false |  | Selection::Multi select | When selecting an element in Multi select mode, you might want to automatically select the children. This is disabled by default. Note: This only works properly when the whole tree is loaded (Data -> Scenario). If you apply this to a partial tree, it will obviously only send the child nodes that have been loaded. |
| `selectOnChangeAction` | enumeration | Yes | nothing | `nothing` \| `mf` \| `nf` | Selection::Selection onChange | Execute this action when the selection changes |
| `selectOnChangeMicroflow` | microflow |  |  |  | Selection::Selection onChange | Microflow |
| `selectOnChangeNanoflow` | nanoflow |  |  |  | Selection::Selection onChange | Nanoflow |
| `selectActionButtons` | object |  |  | list; 6 sub-properties below | Selection::Selection UI | Buttons that can be triggered when you have selected something. If no buttons are configured, Select Mode will be ignored. This is a button bar that will appear on top of the table. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `selectABLabel` | string | Yes |  |  |  | Caption |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `selectABClass` | string |  |  |  |  | Extra class names |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `selectABHideOnNotApplicable` | boolean | Yes | true |  |  | If this is set to 'true', it will hide the button when it is disabled |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `selectABAction` | enumeration | Yes | mf | `mf` \| `nf` |  | Note: For the microflow/nanoflow you will need to define the Helper entity! |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `selectABMicroflow` | microflow |  |  |  |  | Microflow |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `selectABNanoflow` | nanoflow |  |  |  |  | Nanoflow |
| `stateManagementType` | enumeration | Yes | disabled | `disabled` \| `localStorage` | Misc::State Management | Note: State management (currently) ONLY works when you load a whole tree (see Data section). In case of 'Top level', it will be disabled. |
| `stateLocalStorageType` | enumeration | Yes | session | `session` \| `local` | Misc::Browser | If you use session (default), it will only apply to current session. If you use local storage, it will stay in the browser as local storage, persisting over multiple sessions. This does mean the local storage can get polluted with a lot of states. |
| `stateLocalStorageTime` | integer | Yes | 60 |  | Misc::Browser | Storage Timeout for State Management is set in MINUTES. When the state is older, it will be ignored |
| `stateLocalStorageKey` | string |  |  |  | Misc::Browser | When storing the state in local storage, it will choose a specific key, based on the the GUID of your context. If you happen to use a non-persistent context, this will change every time. In that case you can set a key here (make it unique for your current view). Otherwise, leave it empty. |
| `stateLocalStorageKeyIncludeGUID` | boolean | Yes | false |  | Misc::Browser | When you set a key you can still add the GUID if needed. This is not useful when using non-persistent objects |
| `stateExecuteSelectActionOnRestore` | boolean | Yes | true |  | Misc::Browser | When you restore your state, and you have an On Change action defined for your selection (see Selection tab), you can execute this. This is enabled by default. |
| `experimentalExposeSetSelected` | boolean | Yes | false |  | Misc::Experimental | This is an EXPERIMENTAL feature. It will expose the function to select a certain object/list of objects in the Tree Table, so you can target it with a Nanoflow + Javascript Action. Notes: - This is EXPERIMENTAL. Please debug in the console when having problems. No guarantees that this feature will stay in the widget. - This only works properly when the whole tree is loaded, or at least all parents + selected node. It will try to expand the parents if possible. - The name of the function will depend on the context. This is done to make sure the function is unique. The name of the function is "window.__TreeTable_{guid}_select", where "{guid}" is the ID of the context object. When executing this in a Javascript action, please verify that the function exists. |

## Object Lists (repeating child entries)

### `columnlist` → property `columnList`

Item properties:

| Property | Operation |
|----------|-----------|
| `columnHeader` | primitive |
| `columnTitleType` | primitive |
| `columnAttr` | attribute |
| `columnWidth` | primitive |
| `columnClassName` | primitive |

### `inlineactionbutton` → property `inlineActionButtons`

Item properties:

| Property | Operation |
|----------|-----------|
| `actionButtonClass` | primitive |
| `actionButttonLabel` | primitive |
| `actionButtonColumnLabel` | primitive |
| `actionButtonColumnClass` | primitive |
| `actionButtonOnClickAction` | primitive |
| `actionButtonOnClickOpenPageAs` | primitive |

### `selectactionbutton` → property `selectActionButtons`

Item properties:

| Property | Operation |
|----------|-----------|
| `selectABLabel` | primitive |
| `selectABClass` | primitive |
| `selectABHideOnNotApplicable` | primitive |
| `selectABAction` | primitive |

---

Regenerated by `mxcli widget docs` and by `refresh catalog`. For the same data live from the `.mpk` — including anything added by a widget upgrade since this file was written — run `mxcli widget describe mxtreetable -p <app.mpr>`.
