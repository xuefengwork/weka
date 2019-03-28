# weka

=====================================================================
                                                                       
                              ======                                   
                              README                                   
                              ======                                   
                                                                       
                            WEKA 3.8.3
                           4 Sept 2018
                                                                       
                 Java Programs for Machine Learning 

           Copyright (C) 1998-2018  University of Waikato

              web: http://www.cs.waikato.ac.nz/~ml/weka            
                                                                       
=====================================================================


Contents:
---------

1. Using one of the graphical user interfaces in Weka

2. Weka packages and the package manager

3. The Weka data format (ARFF)

4. Using Weka from the command line

   - Classifiers
   - Association rules
   - Filters

5. Database access

6. The Experiment package

7. Weka manual

8. Source code

9. Credits

10. Submission of code and bug reports

11. Copyright


----------------------------------------------------------------------

1. Using one of the graphical user interfaces in Weka:
------------------------------------------------------

This assumes that the Weka archive that you have downloaded has been
extracted into a directory containing this README and that you haven't
used an automatic installer (e.g. the one provided for Windows).

Weka 3.7 requires Java 1.6 or higher. Depending on your platform you
may be able to just double-click on the weka.jar icon to run the
graphical user interfaces for Weka. Otherwise, from a command-line
(assuming you are in the directory containing weka.jar), type

java -jar weka.jar

or if you are using Windows use

javaw -jar weka.jar

Note:
Using "-jar" overrides your CLASSPATH variable! If you need to
use classes specified in the CLASSPATH, use the following command
instead:

java -classpath $CLASSPATH:weka.jar weka.gui.GUIChooser

or if you are using Windows use

javaw -classpath "%CLASSPATH%;weka.jar" weka.gui.GUIChooser

This will start a graphical user interface (weka.gui.GUIChooser) from
which you can select various interfaces, like the SimpleCLI interface 
or the more sophisticated Explorer, Experimenter, and Knowledge Flow 
interfaces. SimpleCLI just acts like a simple command shell. The 
Explorer is currently the main interface for data analysis using 
Weka. The Experimenter can be used to compare the performance of 
different learning algorithms across various datasets. The Knowledge 
Flow provides a component-based alternative to the Explorer interface.

Example datasets that can be used with Weka are in the sub-directory
called "data", which should be located in the same directory as this
README file.

The Weka user interfaces provide extensive built-in help facilities
(tool tips, etc.). Documentation for the Explorer can be found in
ExplorerGuide.pdf (also in the same directory as this
README).

You can also start the GUI "Main" class from within weka.jar:

java -classpath weka.jar:$CLASSPATH weka.gui.Main
or if you are using Windows use
javaw -classpath weka.jar;$CLASSPATH weka.gui.Main

----------------------------------------------------------------------

2. Weka packages and the package manager:
-----------------------------------------

From Weka 3.7.2 many Weka algorithms and tools have been extracted from
the main Weka distribution and encapsulated in separate downloadable
packages. These can be obtained from the Weka project on Sourceforge
and installed manually, or Weka's new built-in package manager can
be used to take care of installing/removing packages. There is both
a command line and GUI package manager that can be used to browse
and install packages. The package manager takes care of resolving
dependencies and checking for conflicts.

The GUI package manager can be found in the "Tools" menu of the
GUIChooser. Detailed information on how to use the package management
system can be found in the Weka manual ($WEKAINSTALL/WekaManual.pdf).

----------------------------------------------------------------------

3. The Weka data format (ARFF):
-------------------------------

Datasets for WEKA should be formatted according to the ARFF
format. (However, there are several converters included in WEKA that
can convert other file formats to ARFF. The Weka Explorer will use
these automatically if it doesn't recognize a given file as an ARFF
file.) Examples of ARFF files can be found in the "data" subdirectory.
What follows is a short description of the file format. A more
complete description is available from the Weka web page.

A dataset has to start with a declaration of its name:

@relation name

followed by a list of all the attributes in the dataset (including 
the class attribute). These declarations have the form

@attribute attribute_name specification

If an attribute is nominal, specification contains a list of the 
possible attribute values in curly brackets:

@attribute nominal_attribute {first_value, second_value, third_value}

If an attribute is numeric, specification is replaced by the keyword 
numeric: (Integer values are treated as real numbers in WEKA.)

@attribute numeric_attribute numeric

In addition to these two types of attributes, there also exists a
string attribute type. This attribute provides the possibility to
store a comment or ID field for each of the instances in a dataset:

@attribute string_attribute string

After the attribute declarations, the actual data is introduced by a 

@data

tag, which is followed by a list of all the instances. The instances 
are listed in comma-separated format, with a question mark 
representing a missing value. 

Comments are lines starting with % and are ignored by Weka.

----------------------------------------------------------------------

4. Using Weka from the command line:
------------------------------------

If you want to use Weka from your standard command-line interface
(e.g. bash under Linux):

a) Set WEKAINSTALL to be the directory which contains this README.
b) Add $WEKAINSTALL/weka.jar to your CLASSPATH environment variable.
c) Bookmark $WEKAINSTALL/doc/packages.html in your web browser.

Alternatively you can try using the SimpleCLI user interface available
from the GUI chooser discussed above.

In the following, the names of files assume use of a unix command-line
with environment variables. For other command-lines (including
SimpleCLI) you should substitute the name of the directory where
weka.jar lives for $WEKAINSTALL. If your platform uses something other
character than / as the path separator, also make the appropriate
substitutions.

===========
Classifiers
===========

Try:

java weka.classifiers.trees.J48 -t $WEKAINSTALL/data/iris.arff

This prints out a decision tree classifier for the iris dataset 
and ten-fold cross-validation estimates of its performance. If you
don't pass any options to the classifier, WEKA will list all the 
available options. Try:

java weka.classifiers.trees.J48

The options are divided into "general" options that apply to most
classification schemes in WEKA, and scheme-specific options that only
apply to the current scheme---in this case J48. WEKA has a common
interface to all classification methods. Any class that implements a
classifier can be used in the same way as J48 is used above. WEKA
knows that a class implements a classifier if it extends the
Classifier class in weka.classifiers. Almost all classes in
weka.classifiers fall into this category. Try, for example:

java weka.classifiers.bayes.NaiveBayes -t $WEKAINSTALL/data/labor.arff

Here is a list of some of the classifiers currently implemented in
weka.classifiers:

a) Classifiers for categorical prediction:

weka.classifiers.lazy.IBk: k-nearest neighbour learner
weka.classifiers.trees.J48: C4.5 decision trees 
weka.classifiers.rules.PART: rule learner 
weka.classifiers.bayes.NaiveBayes: naive Bayes with/without kernels
weka.classifiers.rules.OneR: Holte's OneR
weka.classifiers.functions.SMO: support vector machines
weka.classifiers.functions.Logistic: logistic regression
weka.classifiers.meta.AdaBoostM1: AdaBoost
weka.classifiers.meta.LogitBoost: logit boost
weka.classifiers.trees.DecisionStump: decision stumps (for boosting)
etc.

b) Classifiers for numeric prediction:

weka.classifiers.functions.LinearRegression: linear regression
weka.classifiers.trees.M5P: model trees
weka.classifiers.rules.M5Rules: model rules
weka.classifiers.lazy.IBk: k-nearest neighbour learner
weka.classifiers.lazy.LWL: locally weighted learning

=================
Association rules
=================

Next to classification schemes, there is some other useful stuff in 
WEKA. Association rules, for example, can be extracted using the 
Apriori algorithm. Try

java weka.associations.Apriori -t $WEKAINSTALL/data/weather.nominal.arff

=======
Filters
=======

There are also a number of tools that allow you to manipulate a
dataset. These tools are called filters in WEKA and can be found
in weka.filters.

weka.filters.unsupervised.attribute.Discretize: discretizes numeric data
weka.filters.unsupervised.attribute.Remove: deletes/selects attributes
etc.

Try:

java weka.filters.supervised.attribute.Discretize -i
  $WEKAINSTALL/data/iris.arff -c last

----------------------------------------------------------------------

5. Database access:
-------------------

In terms of database connectivity, you should be able to use any
database with a Java JDBC driver. When using classes that access a
database (e.g. the Explorer), you will probably want to create a
properties file that specifies which JDBC drivers to use, where to
find the database, and specify a mapping for the data types. This file
should reside in your home directory or the current directory and be
called "DatabaseUtils.props". An example is provided in
weka/experiment (you need to expand weka.jar to be able to look a this
file). Note that the settings in this file are used unless they are
overidden by settings in the DatabaseUtils.props file in your home
directory or the current directory (in that order).

There are also example DatabaseUtils.props files for several common
databases available (also in weka/experiment):

* HSQLDB: DatabaseUtils.props.hsql
* MS SQL Server 2000: DatabaseUtils.props.mssqlserver
* MS SQL Server 2005 Express Edition: DatabaseUtils.props.mssqlserver2005
* MySQL: DatabaseUtils.props.mysql
* ODBC: DatabaseUtils.props.odbc
* Oracle: DatabaseUtils.props.oracle
* PostgreSQL: DatabaseUtils.props.postgresql

----------------------------------------------------------------------

6. The Experiment package:
-----------------------------------------

There is support for running experiments that involve evaluating
classifiers on repeated randomizations of datasets, over multiple
datasets (you can do much more than this, besides). The classes for
this reside in the weka.experiment package. The basic architecture is
that a ResultProducer (which generates results on some randomization
of a dataset) sends results to a ResultListener (which is responsible
for stating whether it already has the result, and otherwise storing
results).

Example ResultListeners include:

weka.experiment.CSVResultListener: outputs results as
comma-separated-value files.
weka.experiment.InstancesResultListener: converts results into a set
of Instances.
weka.experiment.DatabaseResultListener: sends results to a database
via JDBC. 

Example ResultProducers include:

weka.experiment.RandomSplitResultProducer: train/test on a % split
weka.experiment.CrossValidationResultProducer: n-fold cross-validation
weka.experiment.AveragingResultProducer: averages results from another
ResultPoducer 
weka.experiment.DatabaseResultProducer: acts as a cache for results,
storing them in a database.

The RandomSplitResultProducer and CrossValidationResultProducer make
use of a SplitEvaluator to obtain actual results for a particular
split, provided are ClassifierSplitEvaluator (for nominal
classification) and RegressionSplitEvaluator (for numeric
classification). Each of these uses a Classifier for actual results
generation. 

So, you might have a DatabaseResultListener, that is sent results from
an AveragingResultProducer, which produces averages over the n results
produced for each run of an n-fold CrossValidationResultProducer,
which in turn is doing nominal classification through a
ClassifierSplitEvaluator, which uses OneR for prediction. Whew. But
you can combine these things together to do pretty much whatever you
want. You might want to write a LearningRateResultProducer that splits
a dataset into increasing numbers of training instances.

To run a simple experiment from the command line, try:

java weka.experiment.Experiment -r -T datasets/UCI/iris.arff  \
  -D weka.experiment.InstancesResultListener \
  -P weka.experiment.RandomSplitResultProducer -- \
  -W weka.experiment.ClassifierSplitEvaluator -- \
  -W weka.classifiers.rules.OneR

(Try "java weka.experiment.Experiment -h" to find out what these
options mean)

If you have your results as a set of instances, you can perform paired
t-tests using weka.experiment.PairedTTester (use the -h option to find
out what options it needs).

However, all this is much easier if you use the Experimenter GUI.

----------------------------------------------------------------------

7. Weka Manual:
------------------

A comprehensive manual covering Weka's graphical and command-line
user interfaces. $WEKAINSTALL/WekaManual.pdf

----------------------------------------------------------------------

8. Source code:
---------------

The source code for WEKA is in $WEKAINSTALL/weka-src.jar. To expand it, 
use the jar utility that's in every Java distribution (or any file 
archiver that can handle ZIP files).

----------------------------------------------------------------------

9. Credits:
------------

Refer to the web page for a list of contributors:

http://www.cs.waikato.ac.nz/~ml/weka/

----------------------------------------------------------------------

10. Call for code and bug reports:
---------------------------------

If you have implemented a learning scheme, filter, application,
visualization tool, etc., using the WEKA classes, and you would
like to make it available to the community, then create a Weka
"package" and submit your package's "Description.props" file
to us. We will check the package to make sure that it works
as advertised and doesn't contain any "nasties" and then
add your Description.props to the central package meta data
repository hosted on Sourceforge. Hosting downloadable
packages is the responsibility of the contributer.

The conditions for new classifiers (schemes in general) are that,
firstly, they have to be published in the proceedings of a renowned
conference (e.g., ICML) or as an article of respected journal (e.g.,
Machine Learning) and, secondly, that they outperform other standard
schemes (e.g., J48/C4.5).

More information on contributing to Weka and how to create a Weka
package can be found in the Weka manual ($WEKAINSTALL/WekaManual.pdf).

If you find any bugs, send a bug report to the wekalist mailing list.

1) For core Weka components, i.e. everything in the main weka.jar file
(not including packages), send a bug report to the wekalist mailing
list.

2) For packages, check the package description (either online at
Sourceforge or by using the package management system) and contact the
maintainer of the package directly.

-----------------------------------------------------------------------

11. Copyright:
--------------

The core WEKA system is distributed under the GNU public
license. Please read the file COPYING.

Packages may be distributed under various licenses - check the
description of the package in question for license details.

-----------------------------------------------------------------------


$Revision: 1.13 $

