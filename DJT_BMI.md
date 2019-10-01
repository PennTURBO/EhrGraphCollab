There's a person named DJT who had their height and weight measured in an outpatient health care encounter. Those data were recorded in the `demographics` table in some database.

patient|height_cm|weight_kg
-|-|-
DJT|191|113

- How do we describe the provenance of that data, in terms of rows and columns. Assume that the `patient` column can be used a unique primary key.
- How do we describe the SAPRQL update process that calculated DJT's BMI?
- How do we say that the BMI data was transformed into RDF triples? How do we say what RDF triples are?
    - say some software application generates the update statement below, based on a configuration file. The configuration file could be the plan specification. How should the application (which reads the configuration and the input data and writes the BMI triples) be instantiated? Some thing the bears the plan? Something that participates in a process?


    # baseURI: https://www.whitehouse.gov#
    # prefix: whitehouse
    
    @prefix : <https://www.whitehouse.gov#> .
    @prefix about: <http://purl.obolibrary.org/obo/IAO_0000136> .
    @prefix cm: <http://purl.obolibrary.org/obo/UO_0000015> .
    @prefix concretizes: <http://purl.obolibrary.org/obo/RO_0000059> .
    @prefix efo: <http://www.ebi.ac.uk/efo/> .
    @prefix has_meas_unit_lab: <http://purl.obolibrary.org/obo/IAO_0000039> .
    @prefix has_spec_val: <http://purl.obolibrary.org/obo/OBI_0002135> .
    @prefix has_specified_input: <http://purl.obolibrary.org/obo/OBI_0000293> .
    @prefix has_specified_output: <http://purl.obolibrary.org/obo/OBI_0000299> .
    @prefix has_val_spec: <http://purl.obolibrary.org/obo/OBI_0001938> .
    @prefix inheres: <http://purl.obolibrary.org/obo/RO_0000052> .
    @prefix kg: <http://purl.obolibrary.org/obo/UO_0000009> .
    @prefix kg_per_m_sq: <http://purl.obolibrary.org/obo/UO_0000086> .
    @prefix obo: <http://purl.obolibrary.org/obo/> .
    @prefix owl: <http://www.w3.org/2002/07/owl#> .
    @prefix part: <http://purl.obolibrary.org/obo/BFO_0000050> .
    @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix realizes: <http://purl.obolibrary.org/obo/BFO_0000055> .
    @prefix turbo_merged.owl: <https://raw.githubusercontent.com/PennTURBO/Turbo-Ontology/master/ontologies/turbo_merged.owl/> .
    @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
    
    <https://www.whitehouse.gov#>
      a owl:Ontology ;
    .
    :DJT
      a obo:NCBITaxon_9606 ;
    .
    
    :DJT_length_meas_dat
      a obo:IAO_0000408 ;
      part: :source_data ;
      has_meas_unit_lab: obo:UO_0000015 ;
      about: :DJT ;
      has_val_spec: :DJT_length_val_spec ;
    .
    :DJT_length_val_spec
      a obo:OBI_0001933 ;
      has_spec_val: "191"^^xsd:float ;
    .
    :DJT_mass_meas_dat
      a obo:IAO_0000414 ;
      part: :source_data ;
      has_meas_unit_lab: obo:UO_0000009 ;
      about: :DJT ;
      has_val_spec: :DJT_mass_val_spec ;
    .
    :DJT_mass_val_spec
      a obo:OBI_0001933 ;
      has_spec_val: "113"^^xsd:float ;
    .
    :DJT_pat_role
      a obo:OBI_0000093 ;
      inheres: :DJT ;
    .
    :DJT_physical
      a obo:OGMS_0000099 ;
      realizes: :DJT_pat_role ;
      has_specified_output: :DJT_length_meas_dat ;
      has_specified_output: :DJT_mass_meas_dat ;
    .
    
    :source_data a obo:IAO_0000100 .

----

    prefix : <https://www.whitehouse.gov#> 
    prefix about: <http://purl.obolibrary.org/obo/IAO_0000136> 
    prefix cm: <http://purl.obolibrary.org/obo/UO_0000015> 
    prefix concretizes: <http://purl.obolibrary.org/obo/RO_0000059> 
    prefix efo: <http://www.ebi.ac.uk/efo/> 
    prefix has_meas_unit_lab: <http://purl.obolibrary.org/obo/IAO_0000039> 
    prefix has_spec_val: <http://purl.obolibrary.org/obo/OBI_0002135> 
    prefix has_specified_input: <http://purl.obolibrary.org/obo/OBI_0000293> 
    prefix has_specified_output: <http://purl.obolibrary.org/obo/OBI_0000299> 
    prefix has_val_spec: <http://purl.obolibrary.org/obo/OBI_0001938> 
    prefix inheres: <http://purl.obolibrary.org/obo/RO_0000052> 
    prefix kg: <http://purl.obolibrary.org/obo/UO_0000009> 
    prefix kg_per_m_sq: <http://purl.obolibrary.org/obo/UO_0000086> 
    prefix obo: <http://purl.obolibrary.org/obo/> 
    prefix owl: <http://www.w3.org/2002/07/owl#> 
    prefix part: <http://purl.obolibrary.org/obo/BFO_0000050> 
    prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
    prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> 
    prefix realizes: <http://purl.obolibrary.org/obo/BFO_0000055> 
    prefix turbo_merged.owl: <https://raw.githubusercontent.com/PennTURBO/Turbo-Ontology/master/ontologies/turbo_merged.owl/> 
    prefix xsd: <http://www.w3.org/2001/XMLSchema#> 
    construct {
        ?bmi_calc
            a obo:OBI_0200000 ;
            realizes: ?bmi_calc_plan ;
            has_specified_input: ?length_meas_dat ;
            has_specified_input: ?mass_meas_dat ;
            has_specified_output: ?bmi_dat ;
            .
        ?bmi_dat
            a efo:EFO_0004340 ;
            part: :derrived_data ;
            has_meas_unit_lab: kg_per_m_sq:  ;
            about: ?person ;
            has_val_spec: ?bmi_val_spec ;
            .
        ?bmi_val_spec
            a obo:OBI_0001933 ;
            has_spec_val: ?bmi_val ;
            .
        ?bmi_calc_plan
            a obo:OBI_0000260 ;
            concretizes: ?bmi_calc_plan_spec ;
            .
        ?bmi_calc_plan_spec
            a obo:IAO_0000104 ;
            .
        :derrived_data a obo:IAO_0000100 .
    }
    where
    {
        ?person a obo:NCBITaxon_9606 ;
                .
        ?length_meas_dat
            a obo:IAO_0000408 ;
            part: :source_data ;
            has_meas_unit_lab: obo:UO_0000015 ;
            about: ?person ;
            has_val_spec: ?length_val_spec ;
            .
        ?length_val_spec
            a obo:OBI_0001933 ;
            has_spec_val: ?length_val ;
            .
        ?mass_meas_dat
            a obo:IAO_0000414 ;
            part: :source_data ;
            has_meas_unit_lab: obo:UO_0000009 ;
            about: ?person ;
            has_val_spec: ?mass_val_spec ;
            .
        ?mass_val_spec
            a obo:OBI_0001933 ;
            has_spec_val: ?mass_val ;
            .
        ?pat_role
            a obo:OBI_0000093 ;
            inheres: ?person ;
            .
        ?physical
            a obo:OGMS_0000099 ;
            realizes: ?pat_role ;
            has_specified_output: ?length_meas_dat ;
            has_specified_output: ?mass_meas_dat ;
            .
        :source_data a obo:IAO_0000100 .
        bind(?mass_val/(?length_val * ?length_val / 10000) as ?bmi_val)
        bind(uuid() as ?bmi_dat)
        bind(uuid() as ?bmi_val_spec)
        bind(uuid() as ?bmi_calc)    
        bind(uuid() as ?bmi_calc_plan)
        bind(uuid() as ?bmi_calc_plan_spec)
    }

----


| subject                                       | predicate             | object                                         |
|-----------------------------------------------|-----------------------|------------------------------------------------|
| https://www.whitehouse.gov#derrived_data      | rdf:type              | obo:IAO_0000100                                |
| urn:uuid:4c26f974-ce4b-4975-9e85-e64e67bb51c0 | realizes:             | urn:uuid:a156e312-7b7b-4a00-87a0-cbf426ff2f74  |
| urn:uuid:4c26f974-ce4b-4975-9e85-e64e67bb51c0 | has_specified_input:  | https://www.whitehouse.gov#DJT_length_meas_dat |
| urn:uuid:4c26f974-ce4b-4975-9e85-e64e67bb51c0 | has_specified_input:  | https://www.whitehouse.gov#DJT_mass_meas_dat   |
| urn:uuid:4c26f974-ce4b-4975-9e85-e64e67bb51c0 | has_specified_output: | urn:uuid:4e59bcd7-0125-4f35-b6c8-066ba8479429  |
| urn:uuid:4c26f974-ce4b-4975-9e85-e64e67bb51c0 | rdf:type              | obo:OBI_0200000                                |
| urn:uuid:4e59bcd7-0125-4f35-b6c8-066ba8479429 | part:                 | https://www.whitehouse.gov#derrived_data       |
| urn:uuid:4e59bcd7-0125-4f35-b6c8-066ba8479429 | has_meas_unit_lab:    | kg_per_m_sq:                                   |
| urn:uuid:4e59bcd7-0125-4f35-b6c8-066ba8479429 | about:                | https://www.whitehouse.gov#DJT                 |
| urn:uuid:4e59bcd7-0125-4f35-b6c8-066ba8479429 | has_val_spec:         | urn:uuid:99679644-c976-4d48-9758-1693c4985903  |
| urn:uuid:4e59bcd7-0125-4f35-b6c8-066ba8479429 | rdf:type              | efo:EFO_0004340                                |
| urn:uuid:99679644-c976-4d48-9758-1693c4985903 | has_spec_val:         | 30.975029^^xsd:float                           |
| urn:uuid:99679644-c976-4d48-9758-1693c4985903 | rdf:type              | obo:OBI_0001933                                |
| urn:uuid:a156e312-7b7b-4a00-87a0-cbf426ff2f74 | concretizes:          | urn:uuid:b74fde9c-0a30-4260-b194-e663dbd6a5ac  |
| urn:uuid:a156e312-7b7b-4a00-87a0-cbf426ff2f74 | rdf:type              | obo:OBI_0000260                                |
| urn:uuid:b74fde9c-0a30-4260-b194-e663dbd6a5ac | rdf:type              | obo:IAO_0000104                                |
