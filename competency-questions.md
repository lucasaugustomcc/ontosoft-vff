# Competency questions

The following SPARQL queries can be run against the example RDF available at: <https://w3id.org/ontosoft-vff/example>. 

The ontology is available at <https://w3id.org/ontosoft-vff/ontology>.

We use the following namespaces:
```sparql
prefix sw: <http://ontosoft.org/software#>
prefix vff: <https://w3id.org/ontosoft-vff/ontology#>
prefix ex: <https://w3id.org/ontosoft-vff/example#>
prefix dc: <http://purl.org/dc/elements/1.1/> 
prefix prov: <http://www.w3.org/ns/prov#> 
prefix owl:   <http://www.w3.org/2002/07/owl#> 
prefix rdf:   <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
prefix xsd:   <http://www.w3.org/2001/XMLSchema#> 
prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> 
```

**Query 1:** Given a software function invoked in a workflow component implementation (e.g. *ex:weka.weka3.6.2-J48Modeler*), what is the software and software version of this function?

```sparql
select ?sw ?swVersion where {
	?swVersion rdf:type sw:SoftwareVersion ;
		vff:hasSoftwareFunction ex:weka.weka3.6.2-J48Modeler .
	?sw rdf:type sw:Software ;
		sw:hasSoftwareVersion ?swVersion .
}
```

### Results
<table>
<thead>
<tr>
<th>sw</th>
<th>swVersion</th>
</tr>
</thead>
<tbody>
<tr>
<td>ex:Weka</td>
<td>ex:weka.weka3.6.2</td>
</tr></tbody></table>

**Query 2:** Are there any newer versions for a given function (e.g. *ex:weka.weka3.6.2-J48Classifier*)?

```sparql
select ?swVersionNew ?swFunctionNew where {
	?swVersionNew rdf:type sw:SoftwareVersion ;
		vff:hasSoftwareFunction ?swFunctionNew .
	?swFunctionNew rdf:type vff:SoftwareFunction ;
		prov:wasRevisionOf ex:weka.weka3.6.2-J48Classifier .
}
```

### Results
<table>
<thead>
<tr>
<th>swVersionNew</th>
<th>swFunctionNew</th>
</tr>
</thead>
<tbody>
<tr>
<td>ex:weka.weka3.9.2</td>
<td>ex:weka.weka3.9.2-J48Classifier</td>
</tr></tbody></table>

**Query 3:** What are the differences between two versions of a given software function?
In this example, we compare the query results for *ex:weka.weka3.6.2-J48Classifier* and *ex:weka.weka3.9.2-J48Classifier*. We conclude that the function in both versions has the same inputs and outputs.

```sparql
select ?inputFileName ?inputFileDataFormatName ?inputFileDataTypeName ?inputParameterName ?inputParameterDataTypeName ?outputName
	?outputDataFormatName ?outputDataTypeName where {
	ex:weka.weka3.6.2-J48Classifier rdf:type vff:SoftwareFunction ;
		vff:hasInputFile ?inputFile ;
        vff:hasInputParameter ?inputParameter ;
		vff:hasOutput ?output .
	?input vff:hasInputFileDataFormat ?inputFileDataFormat ;
		vff:hasInputFileDataType ?inputFileDataType ;
		vff:hasInputFileName ?inputFileName .
    ?inputFileDataType rdfs:label ?inputFileDataTypeName .
    ?inputFileDataFormat rdfs:label ?inputFileDataFormatName .
    ?inputParameter 
        vff:hasInputParameterDataType ?inputParameterDataType ;
		vff:hasInputParameterName ?inputParameterName .
    ?inputParameterDataType rdfs:label ?inputParameterDataTypeName .
	?output vff:hasOutputDataFormat ?outputDataFormat ;
		vff:hasOutputDataType ?outputDataType ;
		vff:hasOutputName ?outputName .
    ?outputDataType rdfs:label ?outputDataTypeName .
    ?outputDataFormat rdfs:label ?outputDataFormatName .
}
```

### Results
<table>
<thead>
<tr>
<th>inputFileName</th>
<th>inputFileDataFormatName</th>
<th>inputFileDataTypeName</th>
<th>inputParameterName</th>
<th>inputParameterDataTypeName</th>
<th>outputName</th>
<th>outputDataFormatName</th>
<th>outputDataTypeName</th>
</tr>
</thead>
<tbody>
<tr>
<td>"testFile"</td>
<td>"ARFF"</td>
<td>"Tabular"</td>
<td>"classIndex"</td>
<td>"String"</td>
<td>"classification"</td>
<td>"Java Object"</td>
<td>"Text"</td>
</tr></tbody></table>

```sparql
select ?inputFileName ?inputFileDataFormatName ?inputFileDataTypeName ?inputParameterName ?inputParameterDataTypeName ?outputName
	?outputDataFormatName ?outputDataTypeName where {
	ex:weka.weka3.9.2-J48Classifier rdf:type vff:SoftwareFunction ;
		vff:hasInputFile ?inputFile ;
        vff:hasInputParameter ?inputParameter ;
		vff:hasOutput ?output .
	?input vff:hasInputFileDataFormat ?inputFileDataFormat ;
		vff:hasInputFileDataType ?inputFileDataType ;
		vff:hasInputFileName ?inputFileName .
    ?inputFileDataType rdfs:label ?inputFileDataTypeName .
    ?inputFileDataFormat rdfs:label ?inputFileDataFormatName .
    ?inputParameter 
        vff:hasInputParameterDataType ?inputParameterDataType ;
		vff:hasInputParameterName ?inputParameterName .
    ?inputParameterDataType rdfs:label ?inputParameterDataTypeName .
	?output vff:hasOutputDataFormat ?outputDataFormat ;
		vff:hasOutputDataType ?outputDataType ;
		vff:hasOutputName ?outputName .
    ?outputDataType rdfs:label ?outputDataTypeName .
    ?outputDataFormat rdfs:label ?outputDataFormatName .
}
```

### Results
<table>
<thead>
<tr>
<th>inputFileName</th>
<th>inputFileDataFormatName</th>
<th>inputFileDataTypeName</th>
<th>inputParameterName</th>
<th>inputParameterDataTypeName</th>
<th>outputName</th>
<th>outputDataFormatName</th>
<th>outputDataTypeName</th>
</tr>
</thead>
<tbody>
<tr>
<td>"testFile"</td>
<td>"ARFF"</td>
<td>"Tabular"</td>
<td>"classIndex"</td>
<td>"String"</td>
<td>"classification"</td>
<td>"Java Object"</td>
<td>"Text"</td>
</tr></tbody></table>


**Query 4:** Are there any similar functions to a given function in newer software versions?
In this example, we are interested in a function with the same functionality of *ex:weka.weka3.6.2-J48Classifier* in the *ex:weka.weka3.9.2* Weka version.

```sparql
select ?swFunction where {
	ex:weka.weka3.6.2-J48Classifier rdf:type vff:SoftwareFunction ;
		vff:implementsFunctionality ?functionality .
	?swFunction rdf:type vff:SoftwareFunction ;
		vff:implementsFunctionality ?functionality .
	ex:weka.weka3.9.2 vff:hasSoftwareFunction ?swFunction .
}
```

### Results
<table>
<thead>
<tr>
<th>swFunction</th>
</tr>
</thead>
<tbody>
<tr>
<td>"ex:weka.weka3.9.2-ID3Classifier"</td>
</tr></tbody></table>

**Query 5:** How to invoke a given software function (e.g. *ex:weka.weka3.9.2-ID3Classifier*)?

```sparql
select ?functionInvocation ?containerInvocation where {
	ex:weka.weka3.9.2-ID3Classifier rdf:type vff:SoftwareFunction ;
		vff:hasSoftwareFunctionInvocation ?functionInvocation .
	?swVersion rdf:type sw:SoftwareVersion ;
        vff:hasContainerImage ?containerImage ;
		vff:hasSoftwareFunction ex:weka.weka3.9.2-ID3Classifier .
    ?containerImage vff:hasContainerImageInvocation ?containerInvocation .
}
```

### Results
<table>
<thead>
<tr>
<th>functionInvocation</th>
<th>containerInvocation</th>
</tr>
</thead>
<tbody>
<tr>
<td>"java -jar weka.classifiers.trees.Id3 -T testData -l inputFile -c classIndex > classification"</td>
<td>"docker run --rm -v "$(pwd)" -w "$(pwd)" ontosoft-vff/weka3.6.2"</td>
</tr></tbody></table>


**Query 6:** 
*a)* Are there any known issues that affects a given software function (e.g. *ex:weka.weka3.6.2-J48Classifier*)?

```sparql
select ?bug ?bugDescription where {
	?bug rdf:type vff:KnownIssue ;
		vff:hasKnownIssueDescription ?bugDescription ;
		vff:affectsSoftwareFunction ex:weka.weka3.6.2-J48Classifier .
}
```

### Results
<table>
<thead>
<tr>
<th>bug</th>
<th>bugDescription</th>
</tr>
</thead>
<tbody>
<tr>
<td>ex:weka.weka3.6.2-J48Classifier-Issue</td>
<td>"A bug description example."</td>
</tr></tbody></table>

*b)* Are there any important changes associated with new versions of a given software function (e.g. *ex:weka.weka3.6.2-J48Classifier*)?

```sparql
select ?bugFix ?bugFixDescription where {
	?bugFix rdf:type vff:BugFix ;
		vff:hasBugFixDescription ?bugFixDescription ;
		vff:fixesKnownIssue ?knownIssue .
	?knownIssue
		vff:affectsSoftwareFunction ex:weka.weka3.6.2-J48Classifier .
}
```

### Results
<table>
<thead>
<tr>
<th>bug</th>
<th>bugDescription</th>
</tr>
</thead>
<tbody>
<tr>
<td>ex:weka.weka3.9.2-BugFix</td>
<td>"A bugfix description example."</td>
</tr></tbody></table>