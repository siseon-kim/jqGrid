# free fork of jqGrid
======

jqGrid is a popular jQuery Plugin for displaying and editing data in tabular form. It has some other more sophisticated features, like subgrids, TreeGrids, grouping and so on.

jqGrid was developed mostly by [Tony Tomov](https://github.com/tonytomov) in the past and it was available under MIT/GPL-licences till the version 4.7.0 published Dec 8, 2014 (see [here](https://github.com/tonytomov/jqGrid/tree/v4.7.0)). Short time after that the license agreement was changed (see <a href="https://github.com/tonytomov/jqGrid/commit/1b2cb55c93ee8b279f15a3faf5a2f82a98da3b4c">here</a>) and new 4.7.1 version was <a href="https://github.com/tonytomov/jqGrid/tree/v4.7.1">published</a>.

The code from the GitHib repository is the fork of jqGrid 4.7.0 - the latest version available under MIT/GPL-licences. It will be provided under MIT/GPL-licences.

Below you can find short description of new features already implemented in the fork.

### Compatibility with jqGrid 4.7.0

* the default values of some option of jqGrid are changed (see detailed description below).
* some changes in "local files" from `i18n` folder are made. One should used the files included in the fork and not combine old "local files" of jqGrid 4.7.0 with new `jquery.jqGrid.min.js` or `jquery.jqGrid.src.js`.

### The following **new features** are implemented currently

* Auto-adjustment of the width on columns based on the content of data in the column and the column headers.

### The default values of the following old jqGrid options are changed (comparing with jqGrid 4.7)

* **gridview: true** are used now. It improves performance of rendering of the grid. In case of usage `afterInsertRow` instead of `cellatr` and `rowattr` (which is *very ineffective*) in old projects one will have to need to specify `gridview: true` explicitly.
* **autoencode: false** are used now instead of `autoencode: false` used before. It corresponds to rendering on the client side. The old default `autoencode: false` combined with JSON data loaded from the server or local data loaded from the object produces sometimes to strange side effects if the data contains symbols `&`, `;`, `>` and other used in HTML markup.
* **height: "auto"** are used now instead of `150` before. It improves the visibility of small grids or the grids having small number of rows. No `scrollOffset: 0` are required to remove unneeded free space which one sees on some grids which have no vertical scrollbar.
* *dynamic* default value is used now for `rowNum`. The default value `rowNum: 20` will be changed to 10000 (the value of the new `maxRowNum` property) if no pager exists in jqGrid (no `pager` and `toppager: true` option are used) or if one uses jqGrid option which switches off the pagination (like `treeGrid: true`). New jqGrid option `maxRowNum` can be used to change the maximal value of rows displayed in the grid from 10000 default to another value.
* **datatype: "local"** are use now instead of **datatype: "xml"** used before. If one uses `"xml"` input data then one should add **datatype: "xml"** option which explicitly specify the type of data.
* **editurl: "clientArray"** are use now instead of **editurl: null** used before. It allows to use *local* editing without minimal additional efforts and the requirement to have any server part implemented.
* **cellsubmit: "clientArray"** are use now instead of **cellsubmit: "remote"** used before.

The most the changes corresponds the tendency of web development last years. Local JavaScript data and JSON data loaded from the server (especially in combination with `loadonce: true`) are used now much more frequently. The data

### The following *new features* are implemented (comparing with jqGrid 4.7)

* support of **auto-adjustment of the column width based on the content of data in the column and the content of the column header**. To use the feature one should specify `autoResizable: true` property in the column (one can use `cmTemplate: {autoResizable: true}` to set the property for all columns). After that the usage of **double-click in the resizer** (in the column header close to vertical line between columns) the width of the column will be adjusted. One can use `autoresizeOnLoad: true` jqGrid option to autoresize *all columns* having `autoResizable: true` property directly after loading/sorting/paging of the grid.
* one can specify the alignment of the column headers. See below the description of `labelAlign` and `labelClasses` properties of `colModel`.
* CSS of jqGrid is changed to simplify integration of jqGrid in projects which uses frameworks other as jQuery UI, for example Bootstrap.
* `jsonmap` property of `colModel` can be used now with `datatype: "local"`. The only exception is the existence of non-empty `dataTypeOrg` jqGrid option. The option will be set *automatically* after loading the data from the server and changing `datatype: "json"` and `datatype: "xml"` to `datatype: "local"`. The option allows to use `jsonmap` property for the data loading from the server and skip the property in later processing of the local data.
* `.trigger("reloadGrid")` will reload the data from the server *automatically* in case of the usage remote `datatype` (`"json"` and `"xml"`) together with `loadonce: true`. No additional call of `setGridParam` to reset `datatype` to original value are more required (like described in [the old answer](http://stackoverflow.com/a/5398136/315935)).
* including of English localization file `grid.locale-en.js` is not more required for successful working of jqGrid. 

### The following *new jqGrid options* are implemented (comparing with jqGrid 4.7)

* new `lastSelectedData` option with sorted and filtered `data` can be used. See [the old answer](http://stackoverflow.com/a/9831125/315935) for details. No "subclassing" of internal `$.jgrid.from` method are required more. The name of the internal option `lastSelected` are changed to `lastSelectedData`.
* new `widthOrg` option saves the value of `width` during creating of the grid. It will be used internally mostly to detect the case when jqGrid was created without specifying of any `width` explicitly. It will be interpreted so, that the width of the grid could be adjusted on other changes of the width of the columns.
* new `dataTypeOrg` option will be used internally in case of the usage remote `datatype` (`"json"` and `"xml"`) together with `loadonce: true`. The option will be deleted by `.trigger("reloadGrid")`.
* new `doubleClickSensitivity` option with the default value `250` specify the time in ms. The resizer will stay visible at least the time after the first click. In the time the user can makes the second click and the double-click on the resizer could be detect.
* new `autoresizeOnLoad` option used in combination with `autoResizable: true` property of `colModel`. If `autoresizeOnLoad: true` option are used then jqGrid make auto-resizing of all columns having `autoResizable: true` property direct after `loadComplete`.
* new `autoResizing` option is map of properties like `groupingView` used in grouping. It allows to tune some behaviour of auto-resizing.
  * compact - default value `false`. Means the usage of compact calculation of the width of the column header without reservation of the place of sorting icons
  * widthOfVisiblePartOfSortIcon: default value 12. Should be used only if one replaces the default jQuery UI icons to another icons.
  * minColWidth: 33 - minimal width of column after resizing
  * maxColWidth: 300 - maximal width of column after resizing
  * wrapperClassName: "ui-jqgrid-cell-wrapper" - the name of the class assign to `<span>` included in every cell of the grid
  * adjustGridWidth: true - means that the width of the grid need be adjusted after resizing of the column
  * fixWidthOnShrink: false - fill be removed later. It will be not included in the release

### The following *new `colModel` properties* are implemented (comparing with jqGrid 4.7)

* one can use `template: "integer"`, `template: "number"`, `template: "actions"` in `colModel` to simplify the usage of `formatter: "integer"`, `formatter: "number"` and `formatter: "actions"` in `colModel`. The list of the standard templates will be extended in the next versions. One can use `$.extend(true, $.jgrid.cmTemplate, { myDataTemplate: {...}}` to define custom column templates which can be used like `template: "myDataTemplate"` in `colModel`. See [the post](http://www.trirand.com/blog/?page_id=393/bugs/bug-in-cmtemplate-new-feature) and [the old answer](http://stackoverflow.com/a/6047856/315935) for more information about column templates.
* `autoResizable` which will be used for auto-adjustment of the column width based on the content of data in the column and the content of the column header
* `autoResizingOption` property is an object like `editoptions`, `searchoptions` or `formatoptions`. It can be used to change some common `autoResizing` grid options to another value which is specific for the column only. The properties of `autoResizingOption`: `minColWidth`, `maxColWidth`, `compact`.
* `labelAlign` property with "left", "center" (default), "right" and "likeData" values, 
* `labelClasses` property allows to add CSS class to the column header.

### The following *new methods* are implemented (comparing with jqGrid 4.7)

* setColWidth - allows to change the width of the column after the grid is created.
* autoResizeColumn - has no parameters. It resize of all columns having `autoResizable: true` property
* getGridComponent - allows to get different components of jqGrid like . The method will be extended later.

### The following *new callbacks and jQuery events* are implemented (comparing with jqGrid 4.7)

* `fatalError` - new callback which can be used to change displaying of critical error with respect of another function as default JvaScript function alert. One can use the feature in unit tests for example.
* `resizeDblClick` callback and `jqGridResizeDblClick` event will be called on the double-click on the column resizer. It's important to stress that the callbacks will be called even if no `autoResizable: true` property are defined in `colModel`. It allows to implement some custom action on double-click on the column resizer. Returning `false` or `"stop"` value from `resizeDblClick` callback or `jqGridResizeDblClick` event in case of `autoResizable: true` property set in the column will prevent resizing on the column by calling of `autoResizeColumn`.

### The following **bugs** are fixed

* ... to be included soon

### Other changes in jqGrid and remarks

* files from `i18n` are changes to UTF-8 format. The texts should be always used as Unicode characters if the corresponding characters are visible. Compare for example [grid.locale-ja.js](https://github.com/OlegKi/jqGrid/blob/master/js/i18n/grid.locale-ja.js) with the corresponding file included in jqGrid 4.7.0 (see [here](https://github.com/tonytomov/jqGrid/blob/v4.7.0/js/i18n/grid.locale-ja.js)).
* jqGrid display the alert message now if some additional fatal error will be detected. For example if one don't included  
* some common properties which have no relation to the language are moved to
* we *recommend* to use `getGridParam` *without* additional parameter to get the reference to object with all jqGrid options: `var p = $("#grid").jqGrid("gerGridParam");` for example. One can use the `p.rowNum` to access `rowNum` option. The usage of `$("#grid").jqGrid("gerGridParam", "rowNum")` will be supported too, but the usage of `$("#grid")[0].p.rowNum` could be not work in some of the future version, because the place of the option could be changed in the future.

### Some demos which demonstrates new features

* [GetFilteredData](http://www.ok-soft-gmbh.com/jqGrid/OK/GetFilteredData.htm) - demonstrates how to use new `lastSelectedData` option which returns, in contrast to `data`, *filtered* and *sorted* data items from all pages of jqGrid. Try to set some filter in the demo, make sorting by some column and set the page size to 2 for example. Click the button above the grid and see the displayed results.
* [autoResizing](http://www.ok-soft-gmbh.com/jqGrid/OK/autoresizeOnDoubleClickOnColumnResizer.htm) - demonstrates the default behaviour of auto-resizing feature. Double-click on the column resizer (in the header close to the right border which divides the columns). You will see the default behaviour of column resizing.
* [autoResizingCompact](http://www.ok-soft-gmbh.com/jqGrid/OK/autoresizeOnDoubleClickOnColumnResizer1.htm) - demonstrates the default behaviour of auto-resizing feature. Double-click on the column resizer (in the header close to the right border which divides the columns). You will see the behaviour of column resizing in case of usage `autoResizing: { compact: true }`.
* [autoResizingWithShrinkCompact](http://www.ok-soft-gmbh.com/jqGrid/OK/autoresizeOnDoubleClickOnColumnResizerWithShrink.htm) - modification of the previous demo. It uses no `shrinkToFit: false` option and `width: 518` instead.
* [autoresizeOnLoad](http://www.ok-soft-gmbh.com/jqGrid/OK/autoresizeOnLoad1.htm) - demonstrates auto-resizing on loading. Try to use sorting, paging the grid, and see the results.
* [autoresizeOnLoadCompact](http://www.ok-soft-gmbh.com/jqGrid/OK/autoresizeOnLoad2.htm) - the same demo as before (auto-resizing on loading), but with the usage of `autoResizing: { compact: true }` additionally.
* [autoResizingPerformane1000](http://www.ok-soft-gmbh.com/jqGrid/OK/performane-1000.htm) - the demo create the grid with 1000 rows. By double-click on resizer you can see the performance of resizing for relatively large number of rows.
* [alignLabel](http://www.ok-soft-gmbh.com/jqGrid/OK/alignLabel.htm) - demonstrates the usage of new `labelAlign` and `labelClasses` properties of `colModel`.