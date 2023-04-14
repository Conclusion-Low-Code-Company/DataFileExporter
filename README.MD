# Data(warehouse) File Exporter

## Description
For exporting data of your Mendix application for e.g. your datawarehouse.

## Typical usage scenario
Use this module if you want to periodically want to export data of your application.

## Features and limitations
Features
- Create an application role with module roles an access rules to data object that you want to export. The access granted is used in the data export. 
- Export data periodically by defining your own logic for scheduling and transferring.

## Dependencies
(- v1.0.0 for MX 7.23.31 or higher)
(- v2.0.0 for MX 8.18.19 or higher )
- v1.0.0 for MX 9

## Installation
Download the module and add it to your project.

## Configuration

**1. Create application role for export**
This module leverages the entity access rules for determining the entities that need to be exported. Therefore create a new application role and assign it with the right module roles that specify the entity access and thus entities that you want to export.

**2. Constant values**
- Configure the constants to the values that you will use in your project. Set the value in your project configuration and cloud deployment environment.

1. `APIUser_Password` - The password for the API user.
2. `APIUser_Username` - The username for the API user.
3. `daysToKeepFiles` - The amount of days to keep the exported files on this server. Default value : `0`.
4. `IgnoreModules` - Modules to ignore in the export e.g. System and Administration would be options. Multiple values supported, seperated by a comma e.g.: `system,administration`
5. `MaxAmountOfRecordsPerFile` - Split exports into multiple files if the amount of records to export exceeds this threshold. We need to split to multiple files due to restrictions of the disk space within the Mendix container - that can not be changed - for temporary file size. Default value `1000000`
6. `memberNameChangedDate` - The name of the member used in the WHERE clause of the OQL the default member e.g: `changedDate`
7. `memberNameOrderBy` - The sorting for the export of the records on this member name e.g. `changedDate`
8. `memberNameRecordCount` - The name of the member - integer - for the record count e.g. `RecordCount`
9. `parameterNameLastSyncDate` - Used in JAVA to add the parameter name e.g. `$LastSyncDate`

**3. After Startup**
- Add the microflow `AfterStartUp_Datawarehouse_IS` of the module `Datawarehouse_IS` to your applications after startup microflow

**4. Create module**
- Create a new module `Datawarehouse_Custom`
- Create a microflow `TransferData` with no parameters. Implement the transferring of the data here.
- Create a microflow `UserRole_APIUser` with no parameters that will return the user role of the API user that is user to export the data. e.g. XPATH constraint line `[id = '[%UserRole_APIExportUserApplicationRole%]']`

**5. Exporting data**
- Create your own scheduler e.g. use scheduled events for instance. Call the microflow `Proces_ExportData`
