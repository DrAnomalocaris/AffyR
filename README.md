#title           :AffyR.py
#description     :Uses a pData, Cel files or normalized CSV and hgTable as input to give the distances
#                 of significant gene changed from telomeres and centromeres.
#author          :Albert Lahat
#date            :20130922
#version         :1.1.0
#python_version  :2.7.3
#==============================================================================

AffyR package

needed packages:
    pyper
    numpy
    xlsxwriter (not needed if saveSpreadSheet() not used)
    os
    inspect
    from R:
        simpleaffy
        affy
        quantsmooth
        nugohs1a520180.db 	(or respective Affymetrix annotation data, Package
          			needed is automatically choosen but it has to be
 				installed already)

How to use
Start by making an AffyR object. 

	import AffyR
	affy = AffyR()

To completly analyze the data use CEL files in directory use the analyzeCEL() function. It will read, normalize, show cluster plot, as to remove any samples,if sample is removed it will loop through normalization clusterploting and sample removal. It will then ask what samples to compare, perform ttest, ask what pCritical to use, filter, anotate, calculate telomeric and centromeric distances and output data into a .CSV, .XLSX, and an ideogram.
	
	affy.analyzeCEL()

	or simply:
	AffyR().analyzeCEL()

Most intermediate steps require previous steps to be done in order so calculatedistances() will automatically call all previous steps needed to ease scripting.

If using a normalized table instead of  raw CEL files, the script should start with readNormalized() or the entire analysis can be run using analyzeCSV().

	affy.readNormalized()

	or
	
	affy.analyseCSV()

To analyse in one batch all posible combinations of ttest comparisons testAllCombinationsCSV() can be time saving.

	affy.testAllCombinationsCSV()



class AffyR
 |  Methods defined here:
 |  
 |  AN(self)
 |      uses narrates function to state the function name in use,
 |      can be turned of by changing the value of self.autonarrate to
 |      False
 |  
 |  __init__(self)
 |  
 |  analyzeCEL(self)
 |      Will run from reading CEL files, looping through remove,
 |      and saving data in ideograms and tables.
 |  
 |  analyzeCSV(self, compare, pcrit)
 |      Will run from reading a normalized csv file
 |      and saving data in ideograms and tables. takes as input a list with the items to compare and the pcritical
 |  
 |  anotate(self)
 |      will use R anotation library to anotate the filtered samples
 |      will automatically call ttest(), getLibrary(), and dilter()
 |  
 |  askCompareType(self, types=None)
 |      takes types for the ttest comparison, if None is inputed then
 |      it asks
 |  
 |  askRemove(self, Name=None)
 |      takes input Name and returns an indexnumber tp use in R for accessing that sample
 |      if no Name is used then will ask
 |  
 |  askpCrit(self)
 |      asks what pCrit to use for filtering
 |  
 |  calculatedistances(self)
 |      will calculate the distance from telomere and centromeres,
 |      will automatically call pythonateR()
 |  
 |  cenTable(self)
 |      Analyzes a hgTables.CSV file in directory and outputs a centromere length dictionary
 |  
 |  closePlot(self)
 |      closes any R plot open
 |  
 |  filter(self, pCrit=None)
 |      filters the ttest results by pCrit, if no pCrit is inputed it ask.
 |      if ttest has not been perform, it will automatically call ttest()
 |  
 |  getAnnotationCode(self)
 |      returns the library name from an R expresionSet object.
 |      The data has to be read from CEL files before, Or it will call
 |      the readCEL() function automatically.
 |  
 |  getLibrary(self, library=None)
 |      Imports library, if None is inputed it will attempt to
 |      retrive the library name
 |  
 |  narrate(self, string)
 |      Prints a number folowed by the string inputed, number
 |      later increases by one
 |  
 |  normalize(self)
 |      Normalizes the expresionSet object in R, if CEL files
 |      have not been read it will automatically call readCEL().
 |  
 |  preparetabulateData(self)
 |      prepares data to tabulate. necessary for ttest and for tabulating data
 |  
 |  pvclust(self)
 |      make and shows a cluster plot of all samples, if data has not been normalized
 |      will automatically call normalize()
 |  
 |  pythonateR(self)
 |      will take data and anotations from R and trasnslate it into python lists
 |      will automatically call anotate() if data has not been anotated()
 |  
 |  readCEL(self, directory=None)
 |      Uses R to read pData.txt and CEL files in direcrory, directory can
 |      be changed.
 |  
 |  readNormalized(self, directory=None)
 |      Reads a normalized csv file, has to be run instead of readCEL() and normalize,  diractory can be changed
 |  
 |  remove(self, Name=None)
 |      Removes sample from R expresionSet, will automatically normalize data after being called
 |  
 |  saveCSV(self, table=None)
 |      saves data into a .CSV table into the results directory
 |  
 |  saveIdeogram(self)
 |      saves an ideogram pdf into the results folder
 |  
 |  savePlot(self)
 |      makes and saves a clusterplot in a .pdf format into the result directory
 |  
 |  saveSpreadSheet(self)
 |      saves data into an .XLSX SpreadSheet into the results folder.
 |  
 |  savenormalizeddata(self)
 |      saves normalized data into a .txt file into the results folder
 |  
 |  tabulateData(self)
 |      calls preparetabulateData() and presents elegantly all
 |      the samples in a nicely formated table
 |  
 |  telTable(self)
 |      Analyzes a hgTables.CSV file in directory and outputs a telomere length dictionary
 |  
 |  testAllCombinationsCSV(self, pcrit=0.05)
 |      will read a normalized csv file and make all posible iterations and ttest. takes as input the pcritical desired
 |      (default is 0.05).
 |  
 |  triSave(self)
 |      Will call saveSpreadSheet, saveCSV(), and saveIdeogram().
 |  
 |  ttest(self, inp=None)
 |      performs ttest, it compares the types of samples from the inputed
 |      list (a two string long list), if no list is inputed it will call
 |      askCompareType(), If data is not normalized it will call normalize()
