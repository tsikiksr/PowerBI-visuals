# Developer guide. Building a professional custom visual for Power BI

## Requirements of professional custom visual

Power BI gallery users expect the visuals to contain a minimum set of features and work properly in their Power BI reports. A full-featured visual should include formatting options for the user, selection and filtering, exception handling, and responsive rendering.

Check Bar Chart [tutorial](https://github.com/Microsoft/PowerBI-visuals#building-bar-chart). This's a great example of professionally maden Power BI custom visual.

## Dataset support
Make the visual's barrier of entry lower. 

It's better to add dataset validation and to support large and various datasets.

1. Support large data sets.
2. Provide validation for all data types in the Field well (expect any type of data).
3. Provide error handling for unexpected data values.

[DataViewUtils](https://www.npmjs.com/package/powerbi-visuals-utils-dataviewutils) NPM package can help you with reaching those three.

## Formatting options

Support wide variety of formatting options.

Add this list of expected Formating Panel options, which let users to specify "look and feel" of the visual:

1. Data labels – support the following properties as applicable.
  - Color
  - Font size
  - Label style
  - Display unit
  - Decimal places
  - Density
  - Position

2. Legend – support the following properties as applicable.
  - Font size
  - Color
  - Title
  - Position
  
3. Axis (X-Axis, Y-Axis, or both) properties.
  - Title
  - Color
  - Display unit

4. Data points – support the following properties as applicable.
  - Font size
  - Fill
  - Tooltips
  - Sort

These options could be implemented with [Capabilities](https://github.com/Microsoft/PowerBI-visuals/blob/master/Capabilities/Capabilities.md) of the visual.

For convinient and reliable options parsing and validataion use [DataViewObjectsParser](https://github.com/Microsoft/powerbi-visuals-utils-dataviewutils/blob/master/docs/api/data-view-objects-parser.md) from [DataViewUtils](https://www.npmjs.com/package/powerbi-visuals-utils-dataviewutils) NPM package.

Use the Power BI [Color Palette](https://github.com/Microsoft/PowerBI-visuals/blob/master/Tutorial/ColorPalette.md) for all color choices .

## Support cross-filtering when available.

Provide cross-filtering or cross-highlighting, if appropriate, for your visual. 

This gives a better presentation integration with any report and helps users to use your visual as Selection control for their data the report.

Check the [document](https://github.com/Microsoft/PowerBI-visuals/blob/master/Visual/Selection.md) with Selection functionality description. Also check the [example](https://github.com/Microsoft/PowerBI-visuals/blob/master/Tutorial/Selection.md) of using Selection.

## Versioning

Set the consistent version of your visual. 

Current versioning has the following format: **xx.xx.xx**. The first number is major version, second is minor, third is patch/hotfix.

**MAJOR.MINOR.PATCH**

**MAJOR** version is incremented when you make incompatible API changes.

1. changes that are done after incompatible changes in infrastructure code or

2. changes that require user to update his report that contains this visual - because visual does not support previous version of settings.

**MINOR** version is changed when you add functionality/feature in a backwards-compatible manner.

**PATCH** (or HOTFIX) version is changed when you make backwards-compatible bug fixes. 

Every package submission needs to have different version.


## Testing.

Test your visual, automatically if possible. Unit tests and manual tests are куйгшкувю

To provide high-quality visuals to the Power BI community, it is important to fully test your visual before submission to the gallery. Upon receipt of your visual submission, the Power BI custom visual team will use these test cases and acceptance criteria to validate your submission.

Use package [validator tool](https://powerbi-validator-v-glpol.herokuapp.com/) (TODO: fix to production link) for an artifact review of the your package.

Also check great [TestUtils](https://www.npmjs.com/package/powerbi-visuals-utils-testutils) NPM package, it can help you with creation of unit tests.

### Best Practices

Use these items planing your visual:

1. The behavior of your visual should be the same in mobile, web and desktop versions of Power BI application. And mobile-friendly at the same time in terms of touch support and tooltip appearance, 

2. Use a different type of data and datasets. Do not rely only on your sample dataset that was used for development of the custom visual.

3. Test critical situations – multiple instances on the same page or the same report, big datasets, different types of data, user input and formatting settings, etc.

4. Ensure that elements of your visual will not overlap each other on different sizes of your visual, different type of datasets and different formatting settings.

5. Set min/max fields of your data roles to limit the number of fields that can be dropped into the specific field bucket.

6. Add unit tests that will validate your visual ensuring that new changes do not break your visual.

Check [Common problems of custom visuals](./SubmissionCommonProblems.md).

Also check our [test cases and acceptable web browsers](./SubmissionTesting.md).
