# Power BI report bookmarks (Preview)

Power BI report bookmarks allow capturing the currently configured view of a report page, selection state, filtering state of the visual e.t.c. But it requires additional action from custom visuals side to support the bookmark and react correctly to changes of report state. Read more about bookmarks in the [documentation](https://docs.microsoft.com/en-us/power-bi/desktop-bookmarks)

## Bookmarks support in your visuals

If your visuals interact with other visuals, selects datapoint or filters other visuals, you need to restore state from properties.

## How to add bookmarks support

Install the required utils or update to new one. The `powerbi-visuals-utils-interactivityutils`(https://github.com/Microsoft/PowerBI-visuals-utils-interactivityutils/) version 3.0.0 or higher contains additional classes to manipulate with state selection or filter. Install or update utils in your project before continue.

### How the bookmarks support visuals interact with Power BI

Let's consider the sample, where a user creates several bookmarks in the report page and use different selections in each bookmark.

The user selects a datapoint in your visual. The visual interacts with Power BI and other visuals by passings selections to the host. Then the user selects "Add" in `Bookmark panel` and Power BI saves the current selections for the new bookmark.

It happens each time when the user change selection and adds new bookmarks.
Once created, the user can switch between the bookmarks.

When the user clicks on a bookmark, PowerBI restores the saved selections and passes them to the visual.
The `update` method of the visuals will be called, and inside the update options there will be a special object at `options.dataViews[0].metadata.objects.general.filter`. It is expression three of the selection (filter), although it is not recommended to use this object directly.

You can use `FilterManager.restoreSelectionIds` method to convert this object to an array of `ISelectionId`.

### Visuals with selections


### Visuals with filter

Let's assume that the visual creates a filter of data by date range. So, we have `startDate` and `endDate` as start and end of the range.
The visual creates an advanced filter and calls host method `applyJsonFilter` to filter data by the relevant conditions.
The `target` is the table for filtering

```typescript
const filter: IAdvancedFilter = new window["powerbi-models"].AdvancedFilter(
    target,
    "And",
    {
        operator: "GreaterThanOrEqual",
        value: startDate
            ? startDate.toJSON()
            : null
    },
    {
        operator: "LessThanOrEqual",
        value: endDate
            ? endDate.toJSON()
            : null
    });

this.host.applyJsonFilter(
    filter,
    "general",
    "filter",
    (startDate && endDate)
        ? FilterAction.merge
        : FilterAction.remove
);
```

Each time a user clicks a bookmark, the custom visual gets an `update` call.

The custom visual should check the filter in the object:

```typescript
const filter: IAdvancedFilter = FilterManager.restoreFilter(
    dataView.metadata
    && dataView.metadata.objects
    && dataView.metadata.objects["general"]
    && dataView.metadata.objects["general"]["filter"] as any
) as IAdvancedFilter;
```

If the `filter` object is not null, the visual should restore filter conditions from the object:

```typescript
if (filter
    && filter.conditions
    && filter.conditions[0]
    && filter.conditions[1]
) {
    const startDate: Date = filter.conditions[0].value as any;
    const endDate: Date = filter.conditions[1].value as any;

    // apply restored conditions
} else {
    // apply default settings
}
```

After that, the visual should change internal state to the corresponding conditions.

**The visual shouldn't call `applyJsonFilter` to filter other visuals because they already filtered by Power BI.**

// TODO Explain what means `change internal state`

[Timeline Slicer](https://appsource.microsoft.com/en-us/product/power-bi-visuals/WA104380786) changes range selector to correspond data ranges.

![](Tutorial/images/TimelinesBookmarksSupport.png)