# Competency questions

The following SPARQL queries can be run against the example RDF available at: https://w3id.org/ontosoft-vff/example. 

The ontology is available at https://w3id.org/ontosoft-vff/ontology.

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

**Query 1:** What is the software and software version of the function invoked in the implementation of a given workflow component?

```sparql
select ?sw ?swVersion where {
	?swVersion rdfs:type sw:SoftwareVersion ;
		vff:hasSoftwareFunction ex:weka.weka3.6.2-J48Modeler .
	?sw rdfs:type sw:Software ;
		sw:hasSoftwareVersion ?swVersion .
}
```

### Results
| sw | swVersion |
|----|---|
| ex:Weka | ex:weka.weka3.6.2 |

**Query 2:** Are there any newer versions for a given function?

```sparql
select ?swVersionNew ?swFunctionNew where {
	?swVersionNew rdfs:type sw:SoftwareVersion ;
		sw:hasSoftwareFunction ?swFunctionNew .
	?swFunctionNew rdfs:type vff:SoftwareFunction ;
		prov:wasRevisionOf ex:weka.weka3.6.2-J48Classifier .
}
```

### Results
| swVersionNew | swFunctionNew |
|--------------|---------------|
|ex:weka.weka3.9.2|ex:weka.weka3.9.2-J48Classifier|

**Query 3:** What are the differences between two versions of
a given software function?

```sparql
select ?inputName ?inputDataFormat ?inputDataType ?outputName
	?outputDataFormat ?outputDataType where {
	ex:weka.weka3.6.2-J48Classifier rdfs:type vff:SoftwareFunction ;
		vff:hasInputFile ?input ;
		vff:hasOutput ?output .
	?input vff:hasInputFileDataFormat ?inputDataFormat ;
		vff:hasInputFileDataType ?inputDataType ;
		vff:hasInputFileName ?inputName .
	?output vff:hasOutputDataFormat ?outputDataFormat ;
		vff:hasOutputDataType ?outputDataType ;
		vff:hasOutputName ?outputName .
}
```

### Results
| inputName | inputDataFormatName | inputDataTypeName  | outputName | outputDataFormatName | outputDataTypeName |
|-----------|-----------------|---------------|------------|------------------|----------------|
| testFile | ARFF | Tabular | classification | Java Object | Text |

```sparql
select ?inputName ?inputDataFormatName ?inputDataTypeName ?inputParameterName ?inputParameterDataTypeName ?outputName
	?outputDataFormatName ?outputDataTypeName where {
	ex:weka.weka3.9.2-J48Classifier rdfs:type vff:SoftwareFunction ;
		vff:hasInputFile ?input ;
        vff:hasInputParameter ?inputParameter ;
		vff:hasOutput ?output .
	?input vff:hasInputFileDataFormat ?inputDataFormat ;
		vff:hasInputFileDataType ?inputDataType ;
		vff:hasInputFileName ?inputName .
    ?inputDataType rdfs:label ?inputDataTypeName .
    ?inputDataFormat rdfs:label ?inputDataFormatName .
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



**Query 4:** Are there any similar functions to a given function in newer software versions?

```sparql
select ?swFunction where {
	ex:weka.weka3.6.2-J48Classifier rdf:type sw:SoftwareFunction ;
		vff:implementsFunctionality ?functionality .
	?swFunction rdf:type vff:SoftwareFunction ;
		vff:implementsFunctionality ?functionality .
	ex:weka.weka3.6.2 vff:hasSoftwareFunction ?swFunction .
}
```

### Results


**Query 5:** How to invoke a given software function?

```sparql
select ?functionInvocation ?containerInvocation where {
	<ex:weka3.9.2-ID3Classifier> rdfs:type vff:SoftwareFunction ;
		vff:hasFunctionInvocation ?functionInvocation .
	?swVersion rdfs:type sw:SoftwareVersion ;
		hasContainerInvocation ?containerInvocation ;
		hasSoftwareFunction ex:weka.weka3.9.2-ID3Classifier .
}
```

### Results



**Query 6:** 
*a)* Are there any known issues that affects a given
software function?

```sparql
select ?bug ?bugDescription where {
	?bug rdfs:type sw:KnownIssue ;
		vff:hasKnownIssueDescription ?bugDescription ;
		vff:affectsSoftwareFunction ex:weka.weka3.6.2-J48Classifier .
}
```

### Results


*b)* Are there any important changes associated with new
versions of a given software function?

```sparql
select ?bugFix ?bugFixDescription where {
	?bugfix rdfs:type vff:BugFix ;
		vff:hasBugFixDescription ?description .
		vff:affectsSoftwareFunction ex:weka.weka3.6.2-J48Classifier .
}
```

### Results
