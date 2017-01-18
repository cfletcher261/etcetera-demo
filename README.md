# etcetera-demo
A small example project as showcase and tutorial for etcetera

## Requirements
 - php7.0 (php7.0 php7.0-xml php7.0-mbstring  php7.0-zip)
 
## Using the demo
1. Clone the project
1. run ```composer update```
1. run ```php runXlsx.php``` and/or ```php runXml.php``` from project root
1. See what happens :-)

## What does the demo do?
If you look at ```/data/people.xlsx``` you'll find a sheet with dummy data 
generated by sheet-faker containing 1000 tuples of firstname, lastname and email.

We have configured several extractions in ```/config/people/extractor.yml``` and 
```/config/people/extractor.json``` to show most of the possibilities how etcetera may be used.

The demo shows how different kinds of entities and / or relations may be 
extracted within one single run of etcetera.

You should have a look at ```/runXlsx.php``` since this is the place where things are stitched together:
 
## Deep dive (see ```/runXlsx.php```)
Since instanciating an extractor manually may be a lot of code to write 
etcetera offers the possibility to use a factoring to create the extractor 
using a config. First the configuration is read from yml or json using one 
of the according reader provided in etcetera.
 
After that an instance of ```StandardExtractorFactory``` is created. 
If you wish to use filters, validators or converters for properties or 
filters and decorators for entities you need to add an according factory 
to the ```StandardExtractorFactory``` having configured your own aliases 
and mappings for them.

After having configured all you need you are able to create an extractor 
instance calling ```StandardExtractorFactory::create``` using your configuration.

Since readers and writers may be very complex and may strongly vary 
depending on your goals and complexity of your source we only offer simple 
readers for CSV, XLSX, XLS and XML. In this example the Excel2007Reader (XLSX) 
reader is used for our dummy data file.

For using a file reader, you only need to instanciate it and set the source 
file (so it may be re-used for multi-file processing).

Writers may also be very complex as you could write the extract to any target 
you want (Neo4J, MongoDB, MQ...) etcetera only offers the interfaces to 
implement your own by now.

In this example you may have a look at [```ConsoleWriter```](src/bitExpert/EtceteraDemo/Writer/ConsoleWriter.php)
and [```CsvWriter```](src/bitExpert/EtceteraDemo/Writer/CsvWriter.php) 
which are implementations of the interface for this demo.

At this point we have an instance for reader, extractor and writer and 
that's all we need for the processor.

Instanciate the Processor with reader, extractor and writer and call
```$processor->process()``` to let etcetera do its job.

After having completed it's work, you may either see a plenty of output
on the console ([```ConsoleWriter```](src/bitExpert/EtceteraDemo/Writer/ConsoleWriter.php)) or a bunch of CSV files in the 
```/out``` folder, representing a set of entities or relations (one file each).
