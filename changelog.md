# Changelog

## 2021-09-15

**v2.0**

Addresses issues: [#172](https://github.com/Big-Life-Lab/ODM/issues/172), [#120](https://github.com/Big-Life-Lab/ODM/issues/120)

- **Schema changes**

  **Removed tables**
  - The `WWMeasure`, `SiteMeasure`and `CovidPublicHealthData`tables are . The values they contained are migrating to the new `Value`table (see below).

  **Added tables**
  - The `Value` table is added to store values stemming from a `measure`s.
  - The `Measure` table store metadata that fully classify and define a measurement type. This new table is supported by satellite tables that carry further information about the classification of measure and its properties:
    - The `Domain` table stores domain classifications for measures (Biological chemical, physical)
    - The `Specimen` table defines all possible `measure` to parent system relationships (measure in wastewater sample, measure from site, measure from polygon, etc.)
    - The `MeasureGroup` lists names given to groups of measure related to a common substance of interest (e.g., covid-19).
    - The `MeasureClass` list types of measures performed via similar protocols (e.g., allele counts, or nitrogen concentration).
    - The `Units` table stores all possible units used to report measurement values.
    - The `Scale` table stores the different cardinalities that can be assigned to units (quantitative, qualitative, etc.)
    - The `Aggregation` table stores the different types of aggregations that could be applied to a measurement before reporting. 
  - The `Assay` table describes unique experimental procedures that yield measure values.

  - The many-to-many relationship tables `AssayHasInstrument`, `MeasureClassHasUnits`, and `UnitHasAggregtation` have also been added.

  **Removed variables**
  
  The following variables were removed from the schema. Users are encourages to store default values in a configuration file
  - `Sample` Table
    - `sampleTypeOtherDefault`
    - `sampleCollectionDefault`
    - `sampleCollectOtherDefault`
    - `sampleStorageTempCDefault`
    - `measureFractionAnalyzedDefault`
  - `Reporter` Table
    - `siteIDDefault`
    - `labIDDefault
  - `Lab` Table
    - assayMethodIDDefault

  

## 2021-08-09 (update this date when merging to main)

\*\*v1....

**Variable changes (non-breaking changes)**

- `index`: Change type from integer to string. Allows greter flexiblity for naming indexes.


## 2021-02-18

**v1.1.0**

Addresses issues: #59, #84, #90, #92, #93, #96, #97, #99, #101, #102, #103, #104, #106, #112, #113, #114, #116.

- **Variable name changes (breaking changes :bangbang:)**

  - `extractionVolMl`: Change from `sampleSizeL`. Description of the variable was also changed.
  - `sampleTypeDefault`: Change from `SampleTypeDefault`. Now consistent use of lowercase first letter.
  - `sampleTypeOtherDefault`: Change from `SampleTypeOtherDefault`.
  - `sampleCollectionDefault`: Change from `SampleCollectionDefault`.
  - `sampleCollectOtherDefault`: Change from `SampleCollectOtherDefault`.
  - `sampleStorageTempCDefault`: Change from `SampleStorageTempCDefault`.
  - `measureFractionAnalyzedDefault`: Change from `MeasureFractionAnalyzedDefault`.

- **New variables**

- Reporter

  - `organization`: Organization of reporter. Issue [#97](https://github.com/Big-Life-Lab/covid-19-wastewater/issues/97)

- Sample

  - `reporterID`: Reporter ID. Currently, reporterID is `WWmeasure` table but reporter for samples can be different. Issue [#93](https://github.com/Big-Life-Lab/covid-19-wastewater/issues/93)
  - `index`: Index number in case the sample was taken multiple times. Issue [#103](https://github.com/Big-Life-Lab/covid-19-wastewater/issues/103)
  - `InstrumentID`: Links with the Instrument table to describe the instrument used for sampling. Issue [#104](https://github.com/Big-Life-Lab/covid-19-wastewater/issues/104)

- Site

  - `publicHealthDepartment`: Public health department or region. The public health department or region where the site or institute is located. See also `healthRegion` if there is a separate regional health care delivery authority. Issue [#116](https://github.com/Big-Life-Lab/covid-19-wastewater/issues/116)
  - `healthRegion`: Health planning region. The health planning authority where is site or insititute is located. See also `publicHealthDepartment`. Issue [#116](https://github.com/Big-Life-Lab/covid-19-wastewater/issues/116)

- SiteMeasure
  - `SampleID`: Makes sure that samples can easily be linked back to the site measurements, without the need for comparing dates. In case that multiple samples need to be linked to the same site measurement, create a comma separated list of sample IDs.
- **Deleted variable categories (breaking changes :bangbang:)**

  - `SiteMeasure` table, `aggregation` variable, all of the options that included normalization
    - `sdNr`,"Standard deviation, normalized",L'�cart type normalis�
    - `geoMnNr`,"Geometric mean, normalized",Moyenne g�om�trique normalis�e.
    - `meanNr`,"Arithmetic mean, normalized",Moyenne arithm�tique normalis�e.

- **New variable categories**

  - Categories added to allow variant reporting. See `WWMeasure` table, `type` variable:

    - `varB117`: Variant B.1.1.7
    - `varB1351`: Variant B.1.351
    - `varP1`: Variant P.1

  - Updated description of `WWMeasure` table, `unit` measure. These descriptions now reference gene or variant copies.
  - New `detected`: Gene copies or variant detected in the sampleGene or variant copies. Detected = 1. Gene or variant copiesNot detected = 0.
  - New `propVar`: Proportion of variant in sample.
  - `SiteMeasure` table, `type` variable
    - `wwBOD5t`, 5 day total biochemical oxygen demand
    - `wwBOD5c`, 5 day carbonaceous biochemical oxygen demand
    - `wwPtot`, Total phosphates
    - `wwPP`, Total phosphorous
    - ,
  - New `dailyAvg`: Average value taken over a 24h period, in the `SiteMeasure` table, `aggregation` variable
  - The `SiteMeasure` table now has a categorical `unit` variable with the following options

    - `°C`, Degrees Celcius
    - `mm`, Millimeters
    - `m3/h`, Cubic meters per hour
    - `m3/d`, Cubic meters per day
    - `mg/L`, Milligrams per liter
    - `pH`, pH units
    - `uS/cm`, Micro-siemens per centimeter

- **Migrate .md files for tables**

  - Variable and variable categories to CSV files. Please modify the appropriate CSV file for future updates. `metadata.md` is now automatically generated from the CSV files.

    - `Tables.csv`: list of tables.
    - `Variables.csv`: list of variables.
    - `VariableCategories`: list of categories for variables.

- **Other**
  - Missing values for the `value` field in the different tables should be reported using the following notation 'NA', and ideally follow with a note that explains why the value is missing. An example: Every day a daily average flow measurement is generated, yet because of fouling the instrument stopped functioning for one day which makes that this data is missing.
  - SQL template updated to reflect v1.1.0 (and also v1.0.0). These files are now automatically generated from the metadata tables (above). The SQL tables are in SQLite format.
  - Several small grammatical errors corrected in the English variable descriptions.

## 2021-02-15

Metadata migrated to:

- `Tables.csv`: List of all tables.
- `Variables.csv`: List of all variables.
- `VariableCategory.csv`: List of all cteogies within variables.

Modify these tables for future additions and changes. `metadata.md` is automatically generated from `metadata_template.md`.

## 2021-01-26

**v1.0.0 - Many additions and breaking changes. This version is recommended for widespread use.**

- Naming conventions were further developed. Category names shortened to 7 digits to allow wide variable names up to 31 characters.

- Three NEW tables

  - [SiteMeasure](metadata.md#sitemeasure): The site of wastewater sampling, including several defaults that can be used to populate new samples upon creation. SiteMeasure complements the [WWMeasure](metadata.md#wwmeasure) table. It includes measures that are commonly collected by staff at wastewater treatment facilities and field sample locations. Whereas WWMeasure includes measures that are commonly generated by wastewater testing laboratories.
  - [Inststrument](metadata.md#instrument): Instruments that are used for measurements in SiteMeasure and WWMeasure. Note that the assay method itself for viral measurement is described in [AssayMethod](metadata.md#assaymethod).
  - [Lookup](metadata.md#lookup): Reference for categorical variables.

- Names of variables were updated according to the extended naming conventions. Note that these changes are NOT listed here!

- Examples are provided on how to generate wide and long variables and category names.

- Information on how to collaborate is updated.

- Measurement metadata

  - `aggregation` - Following the existing options, one more option was added to the list `geoMeanNormal`.

- [SiteMeasure](metadata.md#sitemeasure) variables:

  - `ID`: (NEW) Unique identifier for each contextual measurement.
  - `SiteID`: (NEW) Links with the Site table to describe the location of measurement.
  - `dateTime`: (NEW) The date and time the measurement was performed.
  - `type`: (NEW) The type of measurement that was performed.
  - `typeOther`: (NEW) Description of the measurement in case it is not listed in type.
  - `typeDescription`: (NEW) Additional information on the performed measurement.
  - `name`: (NEW) Name of the instrument used to perform the measurement.
  - `type`: (NEW) Type of instrument used to perform the measurement.
  - `typeOther`: (NEW) Description of the instrument in case it is not listed in instrumentType.
  - `aggregation`: (NEW) When reporting an aggregate measurement, this field describes the method used.
  - `aggregationOther`: (NEW) Description for other type of aggregation not listed in aggregation.
  - `aggregationDescription`: (NEW) Information on OR reference to which measurements that were included to calculate the aggregated measurement that is being reported.
  - `value`: (NEW) The actual value that is being reported for this measurement.
  - `unit`: (NEW) The engineering unit of the measurement.
  - `qualityFlag`: (NEW) Does the reporter suspect quality issues with the value of this measurement? (Boolean)
  - `notes`: (NEW) Any additional notes.

- [Sample](metadata.md#sample) variables

  - `samplingTempC`: (NEW) Temperature that the sample is stored at while it is being sampled. This field is mainly relevant for composite samples which are either kept at ambient temperature or refrigerated while being sampled.
  - `mailedOnIce`: (NEW) Was the sample kept cool while being sent to the lab? (Boolean)
  - `category` - A distinction is now made between SARS-CoV-2 gene measurements `covid` and the measurement of water quality parameters on the sample `wq`.

- [Site](metadata.md#site) variables

  - `type` - Additional site types were added `airplane`, `correctionalFacility`, `elementarySchool`, `hospital`, `longTermCareFacility`, `sewageTruck`, `universityCampus`, `WWTP`
  - `accessType`: (NEW) Access point of where the sample was collected at the site.
  - `measurement.fractionAnalyzedDefault`: (NEW) Used as default when a new measurement is created for this site. See `fractionAnalyzed` in `Measurement` table.

- [AssayMethod](metadata.md#assaymethod): New variables were introduced to replace `assayDesc`, those are

  - `methodConcentration`: (NEW) Description of the method used to concentrate the sample
  - `methodExtraction`: (NEW) Description of the method used to extract the sample
  - `methodPcr`: (NEW) Description of the PCR method used
  - `qualityAssuranceQC`: (NEW) Description of the quality control steps taken
  - `inhibition`: (NEW) Description of the inhibition parameters.
  - `surrogateRecovery`: (NEW) Description of the surrogate recovery for this method.
  - `description`: (NEW) Description of the assay.
  - `referenceLink`: (NEW) Link to standard operating procedure (assay reference method)

- [CovidPublicHealthData](metadata.md#covidpublichealthdata)

  - `valueType`: (NEW) A categorical variable that replaces the individual variables, instead it provides listed options: `confirmed`, `active`, `tests`, `positiveTests`, `percentPositivityRate`, `hospitalCensus`, `hospitalAdmit`.

## 2021-01-08

- All variable names were updated according to the name convention.

## 2020-11-25

- Change date formatting on `wastewater_virus.csv` to YYYY-MM-DD.

## 2020-11-25

**v0.1.1 - Additions to metadata. No breaking changes.**

- Measurement metadata

  - Add categories `measurementType`:

    - `geoMean`: GeoMean of results
    - `rangeLowestValue`: Lowest value in a range of values
    - `rangeHighestValue`: Highest value in a range of values
    - `singleton`: This value is not an aggregate measurement in any way, and thus is not a `mean`, `median`, `geomean` or other

  - Add `measureValueDetected`: Boolean Value if True then covid-19 was detected.

  - Add `reportDate`: Note use of `reportDate` when historic results are updated for new reporting standards.

- AssayMethod metadata

  - Add `sampleSizeL`: Size of the sample that is analysed in liters
  - Add `loq`: Limit of Quantification for this method if one exists
  - Add `lod`: Limit of detection for this method if one exists
  - Add `inhibition`: Text decription of the inhibition
  - Add `surrogateRecovery`: Text description of the Surrogate Recovery for this method

- Other small corrections to metadata category labels.

- CovidPublicHealthData

  - `dateType`: Type of date used.

- Updated `wastewater_virus.csv` to reflect metadata v0.1.1.

## 2020-11-17

v0.1.0 - Breaking changes to metadata.

- Assay method database added.
- Change test results to be represented as key:values. Each test result has a measurement type (`measureType`) with a corresponding value (`measureValue`). For example a measureType is `mean` and the corresponding `measureValue` has the mean value.

## 2020-11-16

- `wastewater_virus.csv` dataset updated to remove adjustment for percent viral recovery from solids. The adjustment allign reporting with other laboratories. The adjustment reduces N1 and N2 values a maginitude of 10 (approximately).

## 2020-10-29

- Replace invalid values (such as \#DIV/0) with `NA`.

## 2020-10-27

V0.0.2 - Breaking changes to metadata.

- Change `locationID` to `siteID`.
- Change `locationName` to `siteName`.

## 2020-10-16

- All Ottawa data points prior to Oct 2nd have been slightly modified to normalize data for a new centrifuge that is being used to collect wastewater samples at the Ottawa site.

## 2020-10-09

V0.0.1 - Initial variable names and labels.
