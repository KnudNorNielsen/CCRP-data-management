# Data structure

### Content:
-   Storage
    1.  The structural backbone
    2.  Processing raw data
    3.  Readme files
    4.  Governance
    5.  Note
-   Computation
    1.  ERDA
    2.  Computerome

## Storage

Data should be managed between the platforms ERDA, Github and Biolomics. All generated raw data, processed data and results should be stored at ERDA. Scripts used for processing the raw data and generating results should be stored at Github. Metadata, and other data where the format allows it, should be stored in the Biolomics platform. All associated data should be tracable between repositories, meaning that metadata in Biolomics should point to the ERDA and Github and vice versa.

## The structural backbone
ERDA or Electronic Research Data Archive at University of Copenhagen (UCPH) is meant for storing, sharing, analyzing and archiving research data. ERDA is delivered by SCIENCE HPC Center and is formally for the faculty of Science, but others have access mainly for data archiving and publication with UCPH DOIs.

The central idea is to work with three data layers: Raw data, processed data, and analysed data. 
An example of how this partitioning of the data into the three levels, could be sequencing data from a nanopore machine:
	
Raw data
    As raw as it gets! Nanopore raw file as they come directly from the sequencer: FAST5 format
    Remember to accompany your data with metadata files, describing all the relevant info regarding the data, so that someone else will be able to go into the folder in ten years and make sense of it.
Processed data
    This becomes more complicated, since processing can be many things. Basecalling on the FAST5 files will output > fastq files
    These files may need to be trimmed > trimmed fastq files
    Sequence data will be submitted to ENA/NCBI where it will get accessions. trimmed fastq > accessioned trimmed fastq files
    These three steps in the data workflow output data that has been through a process that should be documented in a script or readme file which shall be stored in the same path as the file itself (if scripts are stored in the GitHub repository, then this should be noted in a readme file in the path of the data file). The associated metadata/readme files should make a clear link to the raw data. You could copy the readme files from the raw data, and add to this.
    This folder should contain the files used as input in our analyses. (There might be special cases where analyses will be run directly on raw data)
Analyses
    Result files from various analyses.
    The analyses level will be populated with a forest of different folders, which the users create for their different analyses. But remember, that this is a space intended for collaboration and sharing, so please be structured and name folders in a meaningful way! 
    Make a WP or subproject folder for your analyses.


 
As an example, the MATRIX folder structure is shown below. 
-   RawData
    -   Microbiomics
        -   Ref_Genomics
            -   WGS_Illumina
            -   WGS_Nanopore
            -   Amp_16S
            -   Amp_ITS
        -   Metagenomics
            -   WGS_Illumina
            -   Amp_16S
            -   Amp_ITS
        -   Metabolomics
        -   Phenomics
        -   Climate
-   ProcessedData
    -   Microbiomics
        -   Genomics
            -   WGS_Illumina
            -   WGS_Nanopore
            -   Amp_16S
            -   Amp_ITS
    -   Metagenomics
            -   WGS_Illumina
            -   Amp_16S
            -   Amp_ITS
    -   Metabolomics
    -   Phenomics
    -   Climate
-   Analysis
    -   Microbiomics
        -   Genomics
        -   Ref_genomes
    -   Metagenomics
        -   MAGs
        -   Network
    -   Metabolomics
    -   Phenomics
    -   Multi-omics
    -   Mechanistic
-   Manuscripts
-   Temp
	-   Phenomics

 
## Processing raw data
All data should be identifiable, and linked with the appropriate metadata.

KNN suggests that the raw data sample ID should be supplemented with a BioSample accession under common MATRIX, INTERACT, and INROOT BioProjects.

Data within the *ProcessedData* folder should be ready for analyses, i.e all parts of the data should have been assigned publication ready accessions. This will ensure that all downstream analyses on this data across CCRP will ‘talk’ together and it will minimise the room for data handling errors.

## NOTE:
-   Sample accessions: A sample identifier that maybe could be used for both metagenomics metabolomics and phenomics and thereby facilitate data integration
-   Read accessions and maybe other identifiers that could be added to the raw data to facilitate analysis
Integration between ENA and Metabolights (https://www.ebi.ac.uk/metabolights/index)


## Readme files
Each data set, i.e. 16S amplicon sequencing for 2021 Taastrup, should be accompanied with a readme file placed in the same folder as the data set. This readme file should specify all relevant data regarding the file including who it was that generated the data and who and when it was uploaded to ERDA. 
Readme file for the Processed data should be continuations of the earlier “raw data readme file”, and we suggest that you copy the “raw data readme file” to the ProcessedData folder and add have the the raw data has been process. If possible, include scripts that were used processing the data.

 
## Governance - Read-only access to data
Raw data and processed data, along with the readme files should be “read only” ensuring data integrity, when different people start using the data for analyses.
The read-only security is enforced by the ERDA workgroup owners who have access to the workgroup administration. Write access is either given to all members or to none.
Currently, it is not possible to have writing access only for owners. This implies that if a workgroup should be write-protected after the upload of data an owner of the group should go to the workgroup administration page and clik on “none” under write access.



## NOTE:
-   Each sub-workgroup can be assigned independent access rights if necessary as described here. This document also describes how you request access to the workgroups.
-   Each of the fourth-level workgroups (Microbiomics, Metabolomics, Phenomics, Climate) will be read-only, except for the designated owners who has data that should be uploaded to these specific folders
-   Each workgroup should follow the same basic structure as outlined below in blue. Raw data, processed data and analyses should be kept separately.

