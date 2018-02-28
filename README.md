# library_link_network
BU Libraries use these scripts to pre-process bibliographic records extracted from the Libraries' resource management system (Alma) before they are loaded to the Library.Link Network where they are converted to BibFrame format and hosted on our website: http://link.bu.edu

Two goals are achieved with these scripts. First, we remove unwanted fields and subfields from the marcxml records. Second, we are experimenting with enhancing the records with links to external data sources such as Wikipedia,VIAF, Wikidata, IMdb, the Getty vocabularies, etc. We build a lookup table based on the a dump of the Wikidata database. As each bibliographic record is processed, the lookup table is checked using Library of Congress authority headings to discover other possible sources of data.

## buildLookup.py
This script downloads the latest version of the Wikidata database and uses it to create the desired lookup table. Building the table can requires considerable time and is updated infrequently

## get_files_and_submit_jobs.py

This script looks for the presence of files published from Alma to the Libraries' ftp server. If they exist, it uses pysftp to copy them to the extract directory for processing. The processing is done on a high performance computing cluster that allows multi-threaded processing. The files downloaded are divided into separate directories and a job is added to the queue to process the files in each directory. This allows multiple jobs to be processed at the same time. When the files are separated into subdiretories, the 'enhance_bib_records_with_linked_data_sources.py' script is called to process the files in that directory. This script is scheduled by cron to run every month.

## enhance_bib_records_with_linked_data_sources.py

This script reads each record field at a time, performing the required cleanup and enhancing the field with links from the lookup table when they exist. The lookup utilizes Library of Congress authority subject and name authority numbers to perform the lookup.

