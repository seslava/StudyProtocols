OHDSI Large-Scale Population-Level Evidence Generation study
======================================================================

This study aims to generate population-level evidence on treatments used for major depressive disorder.

Detailed information and protocol is available on the [OHDSI Wiki](http://www.ohdsi.org/web/wiki/doku.php?id=research:largescalepopest).

How to run
==========
1. Make sure that you have Java installed. If you don't have Java already intalled on your computed (on most computers it already is installed), go to java.com to get the latest version. (If you have trouble building with rJava below, be sure on Windows that your Path variable includes the path to jvm.dll (Windows Button --> type "path" --> Edit Environmental Variables --> Edit PATH variable, add to end ;C:/Program Files/Java/jre/bin/server) or wherever it is on your system.)

2. In R, use the following code to install the study package and its dependencies:

	```r
	install.packages("devtools")
	library(devtools)
	install_github("ohdsi/OhdsiRTools") 
	install_github("ohdsi/DatabaseConnector")
	install_github("ohdsi/FeatureExtraction") 
	install_github("ohdsi/CohortMethod")
	install_github("ohdsi/OhdsiSharing")
	install_github("ohdsi/MethodEvaluation")
	install_github("ohdsi/StudyProtocolSandbox/LargeScalePopEst")
	```

3. Once installed, you can execute the study by modifying and using the following code:

	```r
	library(LargeScalePopEst)

	connectionDetails <- createConnectionDetails(dbms = "postgresql",
												 user = "joe",
												 password = "secret",
												 server = "myserver")
    workFolder <- "s:/temp/LargeScalePopEst"
    oracleTempSchema <- NULL

	execute(connectionDetails,
		    cdmDatabaseSchema = "cdm_data",
		    oracleTempSchema = oracleTempSchema,
		    workDatabaseSchema = "results",
	   	    studyCohortTable = "ohdsi_depression_cohorts",
	   	    exposureCohortSummaryTable = "ohdsi_depression_exposure_summary",
		    outputFolder = "c:/temp/study_results",
		    maxCores = 4)
	```

	* For details on how to configure```createConnectionDetails``` in your environment type this for help:
	```r
	?createConnectionDetails
	```

	* ```cdmDatabaseSchema``` should specify the schema name where your patient-level data in OMOP CDM format resides. Note that for SQL Server, this should include both the database and schema name, for example 'cdm_data.dbo'.

	* ```oracleTempSchema``` should be used in Oracle to specify a schema where the user has write priviliges for storing temporary tables.

	* ```cdmVersion``` is the version of the CDM. Can be "4" or "5".


